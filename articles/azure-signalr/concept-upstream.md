---
title: Configuración ascendente en Azure SignalR Service
description: Obtenga una introducción a la configuración ascendente y los protocolos de los mensajes ascendentes.
author: chenyl
ms.service: signalr
ms.topic: conceptual
ms.date: 06/11/2020
ms.author: chenyl
ms.openlocfilehash: 1d51f5e8d2fac1e2b180a608c840d0a322e76271
ms.sourcegitcommit: dbe434f45f9d0f9d298076bf8c08672ceca416c6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/17/2020
ms.locfileid: "92143235"
---
# <a name="upstream-settings"></a>Configuración ascendente

La característica ascendente es una característica en vista previa (GB) que permite a Azure SignalR Service enviar mensajes y eventos de conexión a un conjunto de puntos de conexión en el modo sin servidor. Puede usar la característica ascendente para invocar un método de centro de conectividad desde clientes en modo sin servidor y permitir que los puntos de conexión reciban una notificación cuando las conexiones de cliente se conecten o desconecten.

> [!NOTE]
> Solo el modo sin servidor puede establecer la configuración ascendente.

## <a name="details-of-upstream-settings"></a>Detalles de la configuración ascendente

La configuración ascendente consta de una lista de elementos que dependen del orden. Cada elemento consta de:

* Una plantilla de dirección URL, que especifica a dónde se envían los mensajes.
* Un conjunto de reglas.
* Configuraciones de autenticación. 

Cuando se produce el evento especificado, las reglas de un elemento se comprueban de una en una en orden. Los mensajes se enviarán a la dirección URL ascendente del primer elemento coincidente.

### <a name="url-template-settings"></a>Configuración de plantillas de dirección URL

Puede parametrizar la dirección URL para admitir varios patrones. Hay tres parámetros predefinidos:

|Parámetro predefinido|Descripción|
|---------|---------|
|{centro}| Un centro es un concepto de Azure SignalR Service. Un centro es una unidad de aislamiento. El ámbito de los usuarios y de la entrega de mensajes está restringido a un centro.|
|{categoría}| Una categoría puede tener uno de estos valores: <ul><li>**connections**: Eventos de duración de la conexión. Se activa cuando una conexión de cliente se conecta o desconecta. Incluye eventos conectados y desconectados.</li><li>**messages**: Se activa cuando los clientes invocan un método de centro. Incluye todos los demás eventos, excepto los de la categoría **connections**.</li></ul>|
|{evento}| En la categoría **messages**, un evento es el destino en el [mensaje de invocación](https://github.com/dotnet/aspnetcore/blob/master/src/SignalR/docs/specs/HubProtocol.md#invocation-message-encoding) que envían los clientes. En la categoría **connections**, solo se usan *connected* y *disconnected*.|

Estos parámetros predefinidos se pueden usar en el patrón de dirección URL. Los parámetros se reemplazarán por un valor especificado cuando se esté evaluando la dirección URL ascendente. Por ejemplo: 
```
http://host.com/{hub}/api/{category}/{event}
```
Cuando se conecte una conexión de cliente en el centro "chat", se enviará un mensaje a esta dirección URL:
```
http://host.com/chat/api/connections/connected
```
Cuando un cliente del centro "chat" invoque el método de centro de conectividad `broadcast`, se enviará un mensaje a esta dirección URL:
```
http://host.com/chat/api/messages/broadcast
```

### <a name="rule-settings"></a>Configuración de reglas

Puede establecer reglas para *reglas de centro*, *reglas de categoría* y *reglas de eventos* por separado. La regla coincidente admite tres formatos. Tome como ejemplo las reglas de eventos:
- Use un asterisco (*) para asociar con cualquier evento.
- Use una coma (,) para combinar varios eventos. Por ejemplo, `connected, disconnected` asocia los eventos conectados y desconectados.
- Use el nombre de evento completo para que se asocie con el evento. Por ejemplo, `connected` asocia el evento conectado.

> [!NOTE]
> Si usa Azure Functions y el [desencadenador de SignalR](../azure-functions/functions-bindings-signalr-service-trigger.md), dicho desencadenador expondrá un punto de conexión único con el siguiente formato: `https://<APP_NAME>.azurewebsites.net/runtime/webhooks/signalr?code=<API_KEY>`.
> Solo puede configurar la plantilla de dirección URL para esta URL.

### <a name="authentication-settings"></a>Configuración de autenticación

Puede configurar la autenticación para cada elemento de configuración ascendente por separado. Al configurar la autenticación, se establece un token en el encabezado `Authentication` del mensaje ascendente. Actualmente, Azure SignalR Service admite los siguientes tipos de autenticación:
- `None`
- `ManagedIdentity`

Al seleccionar `ManagedIdentity`, debe habilitar una identidad administrada en Azure SignalR Service de antemano y, de manera opcional, especificar un recurso. Para obtener más información, consulte [Identidades administradas para Azure SignalR Service](howto-use-managed-identity.md).

## <a name="create-upstream-settings-via-the-azure-portal"></a>Cree una configuración ascendente mediante Azure Portal

1. Vaya a Azure SignalR Service.
2. Seleccione **Configuración** y cambie **Modo de servicio** a **Sin servidor**. Aparecerá la configuración ascendente:

    :::image type="content" source="media/concept-upstream/upstream-portal.png" alt-text="Configuración ascendente":::

3. Agregue las direcciones URL en **Upstream URL Pattern** (Patrón de dirección URL ascendente). A continuación, las opciones, como **Hub Rules** (Reglas de centros), mostrarán el valor predeterminado.
4. Para establecer la configuración de **Hub Rules** (Reglas de centros), **Event Rules** (Reglas de eventos), **Category Rules** (Reglas de categorías) y **Upstream Authentication** (Autenticación ascendente), seleccione el valor de **Hub Rules**. Aparece una página que le permite editar la configuración:

    :::image type="content" source="media/concept-upstream/upstream-detail-portal.png" alt-text="Configuración ascendente":::

5. Para establecer **Upstream Authentication** (Autenticación ascendente), asegúrese de que ha habilitado primero una identidad administrada. A continuación, seleccione **Use Managed Identity** (Usar identidad administrada). Según sus necesidades, puede elegir cualquier opción en **Auth Resource ID** (Id. De recurso de autenticación). Para obtener más información, consulte [Identidades administradas para Azure SignalR Service](howto-use-managed-identity.md).

## <a name="create-upstream-settings-via-resource-manager-template"></a>Creación de una configuración ascendente a través de una plantilla de Resource Manager

Para crear una configuración ascendente mediante una [plantilla de Azure Resource Manager](../azure-resource-manager/templates/overview.md), establezca la propiedad `upstream` en la propiedad `properties`. El siguiente fragmento de código muestra cómo establecer la propiedad `upstream` para crear y actualizar la configuración ascendente.

```JSON
{
  "properties": {
    "upstream": {
      "templates": [
        {
          "UrlTemplate": "http://host.com/{hub}/api/{category}/{event}",
          "EventPattern": "*",
          "HubPattern": "*",
          "CategoryPattern": "*",
          "Auth": {
            "Type": "ManagedIdentity",
            "ManagedIdentity": {
              "Resource": "<resource>"
            }
          }
        }
      ]
    }
  }
}
```

## <a name="serverless-protocols"></a>Protocolos sin servidor

Azure SignalR Service envía mensajes a los puntos de conexión que cumplen con los protocolos siguientes.

### <a name="method"></a>Método

POST

### <a name="request-header"></a>Encabezado de solicitud

|Nombre |Descripción|
|---------|---------|
|X-ASRS-Connection-Id |Identificador de conexión para la conexión del cliente.|
|X-ASRS-Hub |El centro al que pertenece la conexión del cliente.|
|X-ASRS-Category |Categoría a la que pertenece el mensaje.|
|X-ASRS-Event |Evento al que pertenece el mensaje.|
|X-ASRS-Signature |Código de autenticación de mensajes basado en hash (HMAC) que se utiliza para la validación. Consulte [Firma](#signature) para obtener más información.|
|X-ASRS-User-Claims |Un grupo de notificaciones de la conexión del cliente.|
|X-ASRS-User-Id |Identidad de usuario del cliente que envía el mensaje.|
|X-ASRS-Client-Query |Consulta de la solicitud cuando los clientes se conectan al servicio.|
|Authentication |Un token opcional cuando se usa `ManagedIdentity`. |

### <a name="request-body"></a>Cuerpo de la solicitud

#### <a name="connected"></a>Conectado

Content-Type: application/json

#### <a name="disconnected"></a>Escenario desconectado

Content-Type: `application/json`

|Nombre  |Tipo  |Descripción  |
|---------|---------|---------|
|Error |string |Mensaje de error de una conexión cerrada. Está vacío cuando las conexiones se cierran sin errores.|

#### <a name="invocation-message"></a>Mensaje de invocación

Content-Type: `application/json` o `application/x-msgpack`

|Nombre  |Tipo  |Descripción  |
|---------|---------|---------|
|InvocationId |string | Una cadena opcional que representa un mensaje de invocación. Obtenga más detalles en [Invocaciones](https://github.com/dotnet/aspnetcore/blob/master/src/SignalR/docs/specs/HubProtocol.md#invocations).|
|Destino |string | El mismo que el evento y el mismo que el destino en un [mensaje de invocación](https://github.com/dotnet/aspnetcore/blob/master/src/SignalR/docs/specs/HubProtocol.md#invocation-message-encoding). |
|Argumentos |Matriz de objetos |Una matriz que contiene los argumentos que se van a aplicar al método al que se hace referencia en `Target`. |

### <a name="signature"></a>Signature

El servicio calculará el código SHA256 para el valor `X-ASRS-Connection-Id` mediante la clave de acceso principal y la clave de acceso secundaria como clave `HMAC`. El servicio lo establecerá en el encabezado `X-ASRS-Signature` al hacer las solicitudes HTTP al nivel ascendente:
```
Hex_encoded(HMAC_SHA256(accessKey, connection-id))
```

## <a name="next-steps"></a>Pasos siguientes

- [Identidades administradas para Azure SignalR Service](howto-use-managed-identity.md)
- [Desarrollo y configuración de Azure Functions con Azure SignalR Service](signalr-concept-serverless-development-config.md)
---
title: 'Desarrollo y configuración de una aplicación de Azure Functions: Azure Signalr'
description: Detalles sobre cómo desarrollar y configurar aplicaciones sin servidor en tiempo real con Azure Functions y Azure SignalR Service
author: anthonychu
ms.service: signalr
ms.topic: conceptual
ms.date: 03/01/2019
ms.author: antchu
ms.openlocfilehash: e1157a695d34c75b237391427b37365421366ef8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "77523177"
---
# <a name="azure-functions-development-and-configuration-with-azure-signalr-service"></a>Desarrollo y configuración de Azure Functions con Azure SignalR Service

Las aplicaciones de Azure Functions pueden aprovechar los [enlaces de Azure SignalR Service](../azure-functions/functions-bindings-signalr-service.md) para agregar funcionalidades en tiempo real. Las aplicaciones cliente usan los SDK de cliente disponibles en varios lenguajes para conectarse a Azure SignalR Service y recibir mensajes en tiempo real.

En este artículo se describen los conceptos para desarrollar y configurar una aplicación de función de Azure que se integra con SignalR Service.

## <a name="signalr-service-configuration"></a>Configuración de SignalR Service

Azure SignalR Service puede configurarse de distintos modos. Cuando se usa con Azure Functions, el servicio debe configurarse en modo *Sin servidor*.

En Azure Portal, busque la página *Configuración* del recurso de SignalR Service. Establezca *Modo de servicio* en *Sin servidor*.

![Modo SignalR Service](media/signalr-concept-azure-functions/signalr-service-mode.png)

## <a name="azure-functions-development"></a>Desarrollo de Azure Functions

Una aplicación en tiempo real sin servidor creada con Azure Functions y Azure SignalR Service normalmente requiere dos funciones de Azure:

* Una función "negotiate" que el cliente llama para obtener un token de acceso válido de SignalR Service y la dirección URL del punto de conexión de servicio.
* Una o varias funciones que envían mensajes o que administran la pertenencia al grupo.

### <a name="negotiate-function"></a>función de negociación

Una aplicación cliente requiere un token de acceso válido para conectarse a Azure SignalR Service. Puede ser un token de acceso anónimo o autenticado para un identificador de usuario determinado. Las aplicaciones de SignalR Service sin servidor requieren un punto de conexión HTTP denominado "negotiate" para obtener un token y otra información de conexión, como la dirección URL del punto de conexión de SignalR Service.

Use una función de Azure desencadenada por HTTP y el enlace de entrada *SignalRConnectionInfo* para generar el objeto de información de conexión. La función debe tener una ruta HTTP que termina en `/negotiate`.

Para más información sobre cómo crear la función de negociación, consulte la [*SignalRConnectionInfo*de referencia del enlace de entrada](../azure-functions/functions-bindings-signalr-service-input.md)

Para aprender a crear un token autenticado, consulte [Uso de la autenticación de App Service](#using-app-service-authentication).

### <a name="sending-messages-and-managing-group-membership"></a>Envío de mensajes y administración de la pertenencia a grupos

Use el enlace de salida *SignalR* para enviar mensajes a los clientes conectados a Azure SignalR Service. Puede difundir mensajes a todos los clientes, o enviarlos a un subconjunto de clientes que están autenticados con un identificador de usuario específico o que se han agregado a un grupo determinado.

Se pueden agregar usuarios a uno o más grupos. También puede usar el enlace de salida de *SignalR* para agregar o quitar usuarios en los grupos.

Para más información, consulte la [*SignalR* de referencia del enlace de salida](../azure-functions/functions-bindings-signalr-service-output.md).

### <a name="signalr-hubs"></a>Concentradores de SignalR

SignalR tiene un concepto de "concentradores". Cada conexión de cliente y cada mensaje enviado desde Azure Functions se limitan a un concentrador concreto. Puede usar concentradores como una manera de separar las conexiones y los mensajes en espacios de nombres lógicos.

## <a name="client-development"></a>Desarrollo de cliente

Las aplicaciones cliente de SignalR pueden aprovechar el SDK de cliente de SignalR en uno de varios lenguajes para conectarse fácilmente a Azure SignalR Service y recibir mensajes de este.

### <a name="configuring-a-client-connection"></a>Configuración de una conexión de cliente

Para conectarse a SignalR Service, un cliente debe realizar una negociación de conexión correcta que consta de estos pasos:

1. Realice una solicitud al punto de conexión HTTP *negotiate* mencionado anteriormente para obtener información de conexión válida
1. Conéctese a SignalR Service mediante la dirección URL del punto de conexión de servicio y el token de acceso obtenido del punto de conexión *negotiate*.

Los SDK de cliente de SignalR ya contienen la lógica necesaria para realizar el enlace de negociación. Pase la dirección URL del punto de conexión de negociación, menos el segmento `negotiate`, al elemento `HubConnectionBuilder` del SDK. Este es un ejemplo en JavaScript:

```javascript
const connection = new signalR.HubConnectionBuilder()
  .withUrl('https://my-signalr-function-app.azurewebsites.net/api')
  .build()
```

Por convención, el SDK anexa automáticamente `/negotiate` a la dirección URL y lo usa para comenzar la negociación.

> [!NOTE]
> Si va a usar el SDK de JavaScript/TypeScript en un explorador, deberá [habilitar el uso compartido de recursos entre orígenes (CORS)](#enabling-cors) en la aplicación de función.

Para más información sobre cómo usar el SDK de cliente de SignalR, consulte la documentación correspondiente a su lenguaje:

* [.NET Standard](https://docs.microsoft.com/aspnet/core/signalr/dotnet-client)
* [JavaScript](https://docs.microsoft.com/aspnet/core/signalr/javascript-client)
* [Java](https://docs.microsoft.com/aspnet/core/signalr/java-client)

### <a name="sending-messages-from-a-client-to-the-service"></a>Envío de mensajes desde un cliente hasta el servicio

Aunque el SDK de SignalR permite que las aplicaciones cliente invoquen la lógica de back-end en un concentrador de SignalR, esta funcionalidad no se admite aún cuando se usa SignalR Service con Azure Functions. Use solicitudes HTTP para invocar Azure Functions.

## <a name="azure-functions-configuration"></a>Configuración de Azure Functions

Las aplicaciones de Azure Functions que se integran con Azure SignalR Service se pueden implementar como cualquier aplicación de función de Azure normal, mediante técnicas como [implementación continua](../azure-functions/functions-continuous-deployment.md), [implementación de archivos ZIP](../azure-functions/deployment-zip-push.md) y [ejecutar desde el paquete](../azure-functions/run-functions-from-deployment-package.md).

Sin embargo, hay algunas consideraciones especiales para las aplicaciones que usan los enlaces de SignalR Service. Si el cliente se ejecuta en un explorador, CORS debe estar habilitado. Y si la aplicación requiere autenticación, puede integrar el punto de conexión de negociación con la autenticación de App Service.

### <a name="enabling-cors"></a>Habilitación de CORS

El cliente de JavaScript/TypeScript realiza solicitudes HTTP a la función de negociación para iniciar la negociación de la conexión. Cuando la aplicación cliente se hospeda en un dominio diferente al de la aplicación de función de Azure, se debe habilitar el uso compartido de recursos entre orígenes (CORS) en la aplicación de función o el explorador bloqueará las solicitudes.

#### <a name="localhost"></a>Localhost

Cuando se ejecuta la aplicación de función en el equipo local, puede agregar una sección `Host` a *local.settings.json* para habilitar CORS. En la sección `Host`, agregue dos propiedades:

* `CORS`: escriba la URL base que es el origen de la aplicación cliente.
* `CORSCredentials`: establézcala en `true` para permitir solicitudes "withCredentials".

Ejemplo:

```json
{
  "IsEncrypted": false,
  "Values": {
    // values
  },
  "Host": {
    "CORS": "http://localhost:8080",
    "CORSCredentials": true
  }
}
```

#### <a name="cloud---azure-functions-cors"></a>Nube - CORS en Azure Functions

Para habilitar CORS en una aplicación de función de Azure, vaya a la pantalla de configuración de CORS en la pestaña *Características de la plataforma* de la aplicación de función en Azure Portal.

> [!NOTE]
> La configuración de CORS todavía no está disponible en el plan de Consumo para Linux de Azure Functions. Use [Azure API Management](#cloud---azure-api-management) para habilitar CORS.

Debe estar habilitado CORS con Access-Control-Allow-Credentials para que el cliente de SignalR llame a la función de negociación. Active la casilla para habilitarlo.

En la sección *Orígenes permitidos*, agregue una entrada con la dirección URL base de origen de la aplicación web.

![Configuración de CORS](media/signalr-concept-serverless-development-config/cors-settings.png)

#### <a name="cloud---azure-api-management"></a>Nube: Azure API Management

Azure API Management proporciona una puerta de enlace de API que agrega funcionalidades a los servicios back-end existentes. Puede usarlo para agregar CORS a la aplicación de función. Ofrece un nivel de consumo con precios de pago por acción y una concesión gratuita mensual.

Consulte la documentación de API Management para obtener información sobre cómo [importar una aplicación de Azure Function](../api-management/import-function-app-as-api.md). Una vez importado, puede agregar una directiva de entrada para habilitar CORS con compatibilidad Access-Control-Allow-Credentials.

```xml
<cors allow-credentials="true">
  <allowed-origins>
    <origin>https://azure-samples.github.io</origin>
  </allowed-origins>
  <allowed-methods>
    <method>GET</method>
    <method>POST</method>
  </allowed-methods>
  <allowed-headers>
    <header>*</header>
  </allowed-headers>
  <expose-headers>
    <header>*</header>
  </expose-headers>
</cors>
```

Configure los clientes SignalR para usar la dirección URL de API Management.

### <a name="using-app-service-authentication"></a>Uso de la autenticación de App Service

Azure Functions cuenta con autenticación integrada y admite proveedores conocidos, como Facebook, Twitter, Google, Cuenta Microsoft y Azure Active Directory. Esta característica se puede integrar con el enlace *SignalRConnectionInfo* para crear conexiones a Azure SignalR Service que se han autenticado con un identificador de usuario. La aplicación puede enviar mensajes mediante el enlace de salida *SignalR* que se destinan a ese identificador de usuario.

En Azure Portal, en la pestaña *Características de la plataforma* de la aplicación de función, abra la ventana de configuración *Autenticación/autorización*. Siga la documentación de [Autenticación de App Service](../app-service/overview-authentication-authorization.md) para configurar la autenticación mediante un proveedor de identidades de su elección.

Una vez configurada, las solicitudes HTTP autenticadas incluirán los encabezados `x-ms-client-principal-name` y `x-ms-client-principal-id` que contienen, respectivamente, el nombre de usuario y el identificador de usuario de la identidad autenticada.

Puede usar estos encabezados en la configuración de enlace *SignalRConnectionInfo* para crear las conexiones autenticadas. Este es un ejemplo de la función de negociación de C# que usa el encabezado `x-ms-client-principal-id`.

```csharp
[FunctionName("negotiate")]
public static SignalRConnectionInfo Negotiate(
    [HttpTrigger(AuthorizationLevel.Anonymous)]HttpRequest req,
    [SignalRConnectionInfo
        (HubName = "chat", UserId = "{headers.x-ms-client-principal-id}")]
        SignalRConnectionInfo connectionInfo)
{
    // connectionInfo contains an access key token with a name identifier claim set to the authenticated user
    return connectionInfo;
}
```

A continuación, puede enviar mensajes a ese usuario si establece la propiedad `UserId` de un mensaje de SignalR.

```csharp
[FunctionName("SendMessage")]
public static Task SendMessage(
    [HttpTrigger(AuthorizationLevel.Anonymous, "post")]object message,
    [SignalR(HubName = "chat")]IAsyncCollector<SignalRMessage> signalRMessages)
{
    return signalRMessages.AddAsync(
        new SignalRMessage
        {
            // the message will only be sent to these user IDs
            UserId = "userId1",
            Target = "newMessage",
            Arguments = new [] { message }
        });
}
```

Para información sobre otros lenguajes, consulte los [enlaces de Azure SignalR Service](../azure-functions/functions-bindings-signalr-service.md) para hacer referencia a Azure Functions.

## <a name="next-steps"></a>Pasos siguientes

En este artículo, ha aprendido a desarrollar y configurar aplicaciones de SignalR Service sin servidor con Azure Functions. Intente crear una aplicación por su cuenta mediante uno de los inicios rápidos o tutoriales de la [página de información general de SignalR Service](index.yml).
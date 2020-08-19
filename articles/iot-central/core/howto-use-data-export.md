---
title: Exportación de datos desde IoT Central | Microsoft Docs
description: Cómo usar la nueva exportación de datos para exportar los datos de IoT a Azure y a destinos en la nube personalizados.
services: iot-central
author: viv-liu
ms.author: viviali
ms.date: 08/04/2020
ms.topic: how-to
ms.service: iot-central
manager: corywink
ms.openlocfilehash: f51630154b77233aeb2587ac3a2d603c1da6fa4f
ms.sourcegitcommit: bfeae16fa5db56c1ec1fe75e0597d8194522b396
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/10/2020
ms.locfileid: "88036562"
---
# <a name="export-iot-data-to-cloud-destinations-using-data-export-preview"></a>Exportación de datos de IoT a destinos en la nube mediante la exportación de datos (versión preliminar)

> [!Note]
> ¿Está buscando la exportación de datos heredada? Puede encontrar documentos de exportación de datos [aquí](./howto-export-data.md). Para información sobre las diferencias entre la nueva exportación de datos y la exportación de datos heredada, consulte la [tabla de comparación](#comparison-of-legacy-data-export-and-new-data-export).

En este artículo se describe cómo usar las características de la versión preliminar de la nueva exportación de datos en Azure IoT Central. Puede usar esta característica para exportar continuamente los datos de IoT filtrados y enriquecidos a los servicios en la nube. Puede usar la exportación de datos para enviar los cambios casi en tiempo real a otras partes de la solución en la nube, para obtener información sobre la ruta de acceso activa, el análisis y el almacenamiento. 

 Por ejemplo, se puede:
-   Exportar continuamente los datos de telemetría y los cambios de propiedad en formato JSON casi en tiempo real
-   Filtrar estos flujos de datos para exportar funcionalidades específicas que coincidan con las condiciones personalizadas
-   Enriquecer los flujos de datos con valores personalizados y valores de propiedad del dispositivo
-   Enviar estos datos a destinos como Azure Event Hubs, Azure Service Bus, Azure Blob Storage y puntos de conexión de webhook

> [!Note]
> Cuando se activa la exportación de datos, solo se obtienen los datos a partir de ese momento. Actualmente, no se pueden recuperar los datos del tiempo durante el que la exportación de datos estaba desactivada. Para conservar más datos históricos, active la exportación de datos al principio.

## <a name="prerequisites"></a>Prerrequisitos

Para usar la exportación de datos (versión preliminar), debe tener una aplicación V3 y permisos de exportación de datos.

## <a name="set-up-export-destination"></a>Configuración del destino de exportación

El destino de exportación debe existir antes de configurar la exportación de datos. Estos son los tipos de destinos disponibles:
  - Azure Event Hubs
  - Cola de Azure Service Bus
  - Tema de Azure Service Bus
  - Azure Blob Storage
  - webhook

### <a name="create-an-event-hubs-destination"></a>Creación de un destino de Event Hubs

Si no tiene un espacio de nombres existente de Event Hubs al que exportar, siga estos pasos:

1. Cree un [espacio de nombres de Event Hubs en Azure Portal](https://ms.portal.azure.com/#create/Microsoft.EventHub). Puede encontrar más información en los [documentos de Azure Event Hubs](../../event-hubs/event-hubs-create.md).

2. Cree un centro de eventos en el espacio de nombres de Event Hubs Vaya al espacio de nombres y seleccione **+ Centro de eventos** en la parte superior para crear una instancia de centro de eventos.

3. Genere una clave que usará en IoT Central para configurar la exportación de datos. 
    - Haga clic en la instancia del centro de eventos que creó. 
    - Haga clic en **Configuración/Directivas de acceso compartido**. 
    - Cree una nueva clave o elija una clave existente que tenga permisos de **envío**. 
    - Copie la cadena de conexión principal o la secundaria. La usará para configurar un nuevo destino en IoT Central.

### <a name="create-a-service-bus-queue-or-topic-destination"></a>Creación de una cola o un tema de Service Bus como destino

Si no tiene un espacio de nombres existente de Service Bus al que exportar, siga estos pasos:

1. Cree un [nuevo espacio de nombres de Service Bus en Azure Portal](https://ms.portal.azure.com/#create/Microsoft.ServiceBus.1.0.5). Puede encontrar más información en los [documentos de Azure Service Bus](../../service-bus-messaging/service-bus-create-namespace-portal.md).

2. Para crear una cola o tema como destino de exportación, vaya al espacio de nombres de Service Bus y seleccione **+ Cola** o **+ Tema**.

3. Genere una clave que usará en IoT Central para configurar la exportación de datos. 
    - Haga clic en la cola o el tema que creó. 
    - Haga clic en **Configuración/Directivas de acceso compartido**. 
    - Cree una nueva clave o elija una clave existente que tenga permisos de **envío**. 
    - Copie la cadena de conexión principal o la secundaria. La usará para configurar un nuevo destino en IoT Central.

### <a name="create-an-azure-blob-storage-destination"></a>Creación de una instancia de Azure Blob Storage como destino

Si no tiene una cuenta existente de Azure Storage a la que exportar, siga estos pasos:

1. Cree una [cuenta de almacenamiento en Azure Portal](https://ms.portal.azure.com/#create/Microsoft.StorageAccount-ARM). Puede obtener más información sobre cómo crear nuevas [cuentas de almacenamiento de Azure Blob ](https://aka.ms/blobdocscreatestorageaccount) o [cuentas de almacenamiento de Azure Data Lake Storage v2](../../storage/blobs/data-lake-storage-quickstart-create-account.md). La exportación de datos solo puede escribir datos en cuentas de almacenamiento que admiten blobs en bloques. En la lista siguiente se muestran los tipos de cuenta de almacenamiento compatibles conocidos:

    |Nivel de rendimiento|Tipo de cuenta|
    |-|-|
    |Estándar|Uso general V2|
    |Estándar|Uso general V1|
    |Estándar|Blob Storage|
    |Premium|Almacenamiento de blob en bloques|

2. Cree un contenedor en la cuenta de almacenamiento. Vaya a la cuenta de almacenamiento. En **Blob Service** seleccione **Examinar blobs**. Seleccione **+ Contenedor** en la parte superior para crear un contenedor.

3. Genere una cadena de conexión para la cuenta de almacenamiento en **Configuración/Claves de acceso**. Copie una de las dos cadenas de conexión.

### <a name="create-a-webhook-endpoint"></a>Creación de un punto de conexión de webhook
Puede proporcionar un punto de conexión HTTP disponible públicamente al que se exportarán los datos. Puede crear un punto de conexión de webhook de prueba mediante RequestBin. Tenga en cuenta que RequestBin tiene un límite de solicitud establecido antes de que las solicitudes se limiten.

1. Abra [RequestBin](https://requestbin.net/).
2. Cree una instancia de RequestBin y copie la dirección URL de la papelera.

## <a name="set-up-data-export"></a>Configuración de la exportación de datos

Ahora que tiene un destino al que exportar los datos, siga estos pasos para configurar la exportación de datos.

1. Inicie sesión en su aplicación de IoT Central.

2. En el panel izquierdo, seleccione **Exportación de datos**.

    > [!Tip]
    > Si no ve **Exportación de datos** en el panel izquierdo, no tiene permisos para configurar la exportación de datos en la aplicación. Hable con un administrador para configurar la exportación de datos.

3. Seleccione el botón **+ Nueva exportación**. 

4. Escriba un nombre para mostrar para la nueva exportación y asegúrese de que el botón de alternancia **Habilitado** está activado para que los datos fluyan.

## <a name="1-choose-the-type-of-data-to-export"></a>1. Elección del tipo de datos para exportar
Puede elegir entre exportar continuamente distintos tipos de datos. Estos son los tipos de datos compatibles:

| Tipo de datos | Descripción | Formato de datos |
| :------------- | :---------- | :----------- |
|  Telemetría | Exporte los mensajes de telemetría de los dispositivos casi en tiempo real. Cada mensaje exportado contendrá todo el contenido del mensaje del dispositivo original, que estará normalizado.   |  [Formato del mensaje de telemetría](#telemetry-format)   |
| Cambios de la propiedad | Exporte los cambios de las propiedades de dispositivos y la nube casi en tiempo real. En el caso de las propiedades de dispositivos de solo lectura, se exportarán los cambios realizados en los valores proporcionados. En el caso de las propiedades de lectura y escritura, se exportarán los valores declarados y deseados. | [Formato de los mensajes de cambio de propiedad](#property-changes-format) |

## <a name="2-optional-add-filters"></a>2. (Opcional) Adición de filtros
Agregue filtros para reducir la cantidad de datos exportados en función de las condiciones de filtro. Hay diferentes tipos de filtros disponibles para cada tipo de datos que se van a exportar.

### <a name="telemetry-filters"></a>Filtros de telemetría
  - **Filtro de funcionalidad**: si elige un elemento de telemetría en el menú desplegable, el flujo exportado solo contendrá la telemetría que cumpla la condición de filtro. Si elige un elemento de propiedad de la nube o un dispositivo en la lista desplegable, el flujo exportado solo contendrá la telemetría de los dispositivos que tengan propiedades que coincidan con la condición de filtro.
  - **Filtro de propiedad del mensaje**: los dispositivos que usan los SDK de dispositivo pueden enviar *propiedades de mensajes* o *propiedades de aplicaciones* en cada mensaje de telemetría, que es un contenedor de pares clave-valor que se suele usar para etiquetar el mensaje con identificadores personalizados. Puede crear un filtro de propiedad de mensajes escribiendo la clave de la propiedad del mensaje que está buscando y especificando una condición. Solo se exportarán los mensajes de telemetría que tengan propiedades de mensajes que coincidan con la condición de filtro especificada. Solo se admiten los operadores de comparación de cadenas (es igual a, no es igual a, contiene, no contiene, existe y no existe). [Obtenga más información sobre las propiedades de aplicaciones en la documentación de IoT Hub](../../iot-hub/iot-hub-devguide-messages-construct.md).

### <a name="property-changes-filters"></a>Filtros de cambios de propiedades
**Filtro de propiedades**: elija un elemento de propiedad en el menú desplegable, y el flujo exportado solo contendrá los cambios de la propiedad seleccionada que cumpla la condición de filtro.

## <a name="3-optional-add-enrichments"></a>3. (Opcional) Adición de características enriquecidas
Agregue características enriquecidas para mejorar los mensajes exportados con metadatos adicionales en pares clave-valor. Estas son las características enriquecidas disponibles para los tipos de datos de cambios de propiedad y telemetría:
  - **Cadena personalizada**: agrega una cadena estática personalizada a cada mensaje. Escriba cualquier clave y especifique cualquier valor de cadena.
  - **Propiedad**: agrega la propiedad notificada de dispositivo actual o el valor de propiedad de la nube a cada mensaje. Escriba cualquier clave y elija una propiedad de dispositivo o nube. Si el mensaje exportado procede de un dispositivo que no tiene la propiedad de dispositivo o de la nube especificada, el mensaje exportado no tendrá dicha característica enriquecida.

## <a name="4-add-destinations"></a>4. Adición de destinos
Cree un destino o agregue uno que ya haya creado. 
  
1. Haga clic en el vínculo **Crear un destino**. Rellene la información siguiente:
    - **Nombre del destino**: el nombre para mostrar del destino en IoT Central.
    - **Tipo de destino**: elija el tipo de destino. Si aún no lo ha hecho, [configure el destino de exportación](#set-up-export-destination).
    - En Azure Event Hubs y temas o colas de Azure Service Bus, pegue la cadena de conexión para el recurso. 
    - En Azure Blob Storage, pegue la cadena de conexión para el recurso y escriba el nombre del contenedor (distingue mayúsculas de minúsculas).
    - Si se trata de un webhook, pegue la dirección URL de devolución de llamada para el punto de conexión de webhook. 
    - Haga clic en **Crear**

2. Haga clic en **+ Destino** y elija un destino en el menú desplegable. Puede agregar hasta 5 destinos a una única exportación.

3. Una vez que haya terminado de configurar la exportación, haga clic en **Guardar**. Transcurridos unos minutos, los datos aparecerán en los destinos.

## <a name="export-contents-and-format"></a>Exportación del contenido y formato

### <a name="azure-blob-storage-destination"></a>Destino de Azure Blob Storage

Los datos se exportan una vez por minuto, y cada archivo contiene el lote de cambios desde el último archivo exportado. Los datos exportados se colocan en tres carpetas con formato JSON. Las rutas de acceso predeterminadas de la cuenta de almacenamiento son:

- Telemetría: _{container}/{app-id}/{partition_id}/{YYYY}/{MM}/{dd}/{hh}/{mm}/{filename}_
- Cambios de propiedad: _{container}/{app-id}/{partition_id}/{YYYY}/{MM}/{dd}/{hh}/{mm}/{filename}_

Para examinar los archivos exportados en Azure Portal, navegue hasta al archivo y seleccione la pestaña **Editar blob**.

### <a name="azure-event-hubs-and-azure-service-bus-destinations"></a>Destinos de Azure Event Hubs y Azure Service Bus

Los datos se exportan casi en tiempo real. Los datos están en el cuerpo del mensaje y están en formato JSON codificados como UTF-8. 

En las anotaciones o en el contenedor de propiedades del sistema del mensaje, puede encontrar `iotcentral-device-id`, `iotcentral-application-id`, `iotcentral-message-source` y `iotcentral-message-type`, que tienen los mismos valores que los campos correspondientes del cuerpo del mensaje.

### <a name="webhook-destination"></a>Destino de webhook
En el caso de los destinos de webhooks, los datos también se exportan casi en tiempo real. Los datos del cuerpo del mensaje están en el mismo formato que en Event Hubs y Service Bus.


## <a name="telemetry-format"></a>Formato de los datos de telemetría
Cada mensaje exportado contiene un formato normalizado del mensaje completo que el dispositivo envía en el cuerpo del mensaje en formato JSON codificado como UTF-8. La información adicional de cada mensaje incluye:

- `applicationId` de la aplicación de IoT Central.
- `messageSource` que es *telemetry* para exportar datos de telemetría.
- `deviceId` del dispositivo que envió el mensaje de telemetría.
- `schema` es el nombre y la versión del esquema de carga.
- `templateId` es el identificador de plantilla de dispositivo asociado al dispositivo.
- `enrichments` son todas las características enriquecidas configuradas en la exportación.
- `messageProperties` son fragmentos adicionales de datos que el dispositivo envío junto con el mensaje. Esto también se conoce como *propiedades de aplicaciones*. [Obtenga más información en la documentación de IoT Hub](../../iot-hub/iot-hub-devguide-messages-construct.md).

En el caso de Event Hubs y Service Bus, IoT Central exporta un mensaje nuevo rápidamente después de recibir el mensaje de un dispositivo.

Para Blob Storage, los mensajes se procesan por lotes y se exportan una vez por minuto.

En el ejemplo siguiente se muestra un mensaje de telemetría exportado:

```json

{
    "applicationId": "1dffa667-9bee-4f16-b243-25ad4151475e",
    "messageSource": "telemetry",
    "deviceId": "1vzb5ghlsg1",
    "schema": "default@preview",
    "templateId": "urn:qugj6vbw5:___qbj_27r",
    "enqueuedTime": "2020-08-05T22:26:55.455Z",
    "telemetry": {
        "Activity": "running",
        "BloodPressure": {
            "Diastolic": 7,
            "Systolic": 71
        },
        "BodyTemperature": 98.73447010562934,
        "HeartRate": 88,
        "HeartRateVariability": 17,
        "RespiratoryRate": 13
    },
    "enrichments": {
      "userSpecifiedKey": "sampleValue"
    },
    "messageProperties": {
      "messageProp": "value"
    }
}
```

## <a name="property-changes-format"></a>Formato de cambios de propiedades

Cada mensaje o registro representa un cambio en una propiedad de la nube o del dispositivo. En el caso de las propiedades del dispositivo, solo los cambios en el valor notificado se exportan como un mensaje independiente. La información adicional de cada mensaje exportado incluye:

- `applicationId` de la aplicación de IoT Central.
- `messageSource` que es *properties* para exportar datos de cambios de propiedades.
- `messageType` que es *cloudPropertyChange*, *devicePropertyDesiredChange* o *devicePropertyReportedChange*.
- `deviceId` del dispositivo cuyas propiedades han cambiado.
- `schema` es el nombre y la versión del esquema de carga.
- `templateId` es el identificador de plantilla de dispositivo asociado al dispositivo.
- `enrichments` son todas las características enriquecidas configuradas en la exportación.

En Event Hubs y Service Bus, IoT Central exporta los nuevos datos de los mensajes al centro de eventos o a la cola o el tema de Service Bus casi en tiempo real.

Para Blob Storage, los mensajes se procesan por lotes y se exportan una vez por minuto.

En el ejemplo siguiente, se muestra un mensaje de cambio de propiedad exportado recibido en Azure Blob Storage.

```json
{
    "applicationId": "1dffa667-9bee-4f16-b243-25ad4151475e",
    "messageSource": "properties",
    "messageType": "cloudPropertyChange",
    "deviceId": "18a985g1fta",
    "schema": "default@preview",
    "templateId": "urn:qugj6vbw5:___qbj_27r",
    "enqueuedTime": "2020-08-05T22:37:32.942Z",
    "properties": [{
        "fieldName": "MachineSerialNumber",
        "value": "abc"
    }],
    "enrichments": {
        "userSpecifiedKey" : "sampleValue"
    }
}
```

## <a name="comparison-of-legacy-data-export-and-new-data-export"></a>Comparación de exportación de datos heredada y exportación de datos nueva
Esta es una tabla que resalta las diferencias entre la exportación de datos heredada y la exportación de datos nueva. Puede obtener información sobre la exportación de datos heredada [aquí](howto-export-data.md).

| Capacidades  | Exportación de datos heredada | Exportación de datos nueva |
| :------------- | :---------- | :----------- |
| Tipos de datos disponibles | Telemetría, dispositivos y plantillas de dispositivo | Cambios de datos de telemetría y propiedades |
| Filtrado | None | Depende del tipo de datos exportado. Para datos de telemetría, filtrado por telemetría, propiedades de mensaje, valores de propiedad |
| Características enriquecidas | None | Enriquecer con una cadena personalizada o un valor de propiedad en el dispositivo |
| Destinations | Azure Event Hubs, colas y temas de Azure Service Bus, Azure Blob Storage | Igual que para los webhooks y la exportación de datos heredada| 
| Aplicaciones admitidas | V2, V3 | Solo v3 |
| Límites destacados | 5 exportaciones por aplicación, 1 destino por exportación | 10 conexiones de exportaciones y destinos por aplicación | 

## <a name="next-steps"></a>Pasos siguientes

Ahora que sabe cómo usar la exportación de datos nueva, continúe con el paso siguiente:

> [!div class="nextstepaction"]
> [Uso de análisis en IoT Central](./howto-create-analytics.md)

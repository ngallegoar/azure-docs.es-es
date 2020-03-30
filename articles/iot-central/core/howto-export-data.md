---
title: Exportación de los datos de Azure IoT Central | Microsoft Docs
description: Cómo exportar datos desde la aplicación de Azure IoT Central a Azure Event Hubs, Azure Service Bus y Azure Blob Storage
services: iot-central
author: viv-liu
ms.author: viviali
ms.date: 01/30/2019
ms.topic: how-to
ms.service: iot-central
manager: corywink
ms.openlocfilehash: 725c5acf961fffb1fd4cf9bc17e37a5940f871cc
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "80157915"
---
# <a name="export-iot-data-to-destinations-in-azure"></a>Exportación de datos de IoT a destinos en Azure

*Este tema se aplica a administradores*.

En este artículo se describe cómo usar la característica de exportación continua de datos de Azure IoT Central para exportar los datos a instancias de **Azure Event Hubs**, **Azure Service Bus** o **Azure Blob Storage** Los datos se exportan en formato JSON y pueden incluir telemetría, información del dispositivo e información de la plantilla de dispositivo. Use los datos exportados para:

- Información y análisis de la ruta de acceso activa. Esta opción incluye el desencadenamiento de reglas personalizadas en Azure Stream Analytics, el desencadenamiento de flujos de trabajo personalizados en Azure Logic Apps o su paso a través de Azure Functions para su transformación.
- Análisis en frío de rutas de acceso, como modelos de entrenamiento en Azure Machine Learning o análisis de tendencias a largo plazo en Microsoft Power BI.

> [!Note]
> Al activar la exportación de datos continua, solo obtendrá los datos a partir de ese momento. Actualmente, los datos no se pueden recuperar durante un tiempo cuando la exportación de datos continua estaba desactivada. Para conservar más datos históricos, activa la exportación continua de datos al principio.

## <a name="prerequisites"></a>Prerequisites

Ser administrador en la aplicación de IoT Central o contar con permisos de exportación de datos.

## <a name="set-up-export-destination"></a>Configuración del destino de exportación

El destino de exportación debe existir antes de configurar la exportación continua de datos.

### <a name="create-event-hubs-namespace"></a>Creación de un espacio de nombres de Event Hubs

Si no tiene un espacio de nombres existente de Event Hubs al que exportar, siga estos pasos:

1. Cree un [espacio de nombres de Event Hubs en Azure Portal](https://ms.portal.azure.com/#create/Microsoft.EventHub). Puede encontrar más información en los [documentos de Azure Event Hubs](../../event-hubs/event-hubs-create.md).

2. Elija una suscripción. Ahora puede exportar datos a otras suscripciones que no estén en la misma suscripción que la aplicación de IoT Central. En este caso se conecta mediante una cadena de conexión.

3. Cree un centro de eventos en el espacio de nombres de Event Hubs Vaya al espacio de nombres y seleccione **+ Centro de eventos** en la parte superior para crear una instancia de centro de eventos.

### <a name="create-service-bus-namespace"></a>Creación de un espacio de nombres de Service Bus

Si no tiene un espacio de nombres existente de Service Bus al que exportar, siga estos pasos:

1. Cree un [nuevo espacio de nombres de Service Bus en Azure Portal](https://ms.portal.azure.com/#create/Microsoft.ServiceBus.1.0.5). Puede encontrar más información en los [documentos de Azure Service Bus](../../service-bus-messaging/service-bus-create-namespace-portal.md).
2. Elija una suscripción. Ahora puede exportar datos a otras suscripciones que no estén en la misma suscripción que la aplicación de IoT Central. En este caso se conecta mediante una cadena de conexión.

3. Vaya al espacio de nombres de Service Bus y seleccione **+ Cola** o **+ Tema** en la parte superior para crear una cola o un tema como destino de exportación.

Al elegir Service Bus como destino de exportación, las colas y los temas no deben tener habilitada la opción de detección de sesiones o de duplicados. Si cualquiera de estas opciones está habilitada, algunos mensajes no llegarán a la cola o tema.

### <a name="create-storage-account"></a>Crear cuenta de almacenamiento

Si no tiene una cuenta existente de Azure Storage a la que exportar, siga estos pasos:

1. Cree una [cuenta de almacenamiento en Azure Portal](https://ms.portal.azure.com/#create/Microsoft.StorageAccount-ARM). Puede obtener más información acerca de cómo crear nuevas [cuentas de Azure Blob Storage](https://aka.ms/blobdocscreatestorageaccount) o [cuentas de almacenamiento de Azure Data Lake Storage v2](../../storage/blobs/data-lake-storage-quickstart-create-account.md). La exportación de datos solo puede escribir datos en cuentas de almacenamiento que admiten blobs en bloques. Esta es una lista de tipos compatibles conocidos de cuentas de almacenamiento: 

    |Nivel de rendimiento|Tipo de cuenta|
    |-|-|
    |Estándar|Uso general V2|
    |Estándar|Uso general V1|
    |Estándar|Blob Storage|
    |Premium|Almacenamiento de blobs en bloque|

2. Cree un contenedor en la cuenta de almacenamiento. Vaya a la cuenta de almacenamiento. En **Blob Service** seleccione **Examinar blobs**. Seleccione **+ Contenedor** en la parte superior para crear un contenedor.

## <a name="set-up-continuous-data-export"></a>Configuración de la exportación continua de datos

Ahora que tiene un destino al que exportar los datos, siga estos pasos para configurar la exportación de datos continua.

1. Inicie sesión en su aplicación de IoT Central.

2. En el panel izquierdo, seleccione **Exportación de datos**.

    > [!Note]
    > Si no ve Exportación de datos en el panel izquierdo, no tiene permisos para configurar la exportación de datos en la aplicación. Hable con un administrador para configurar la exportación de datos.

3. Seleccione el botón **+ Nuevo** en la esquina superior derecha. Elija **Azure Event Hubs**, **Azure Service Bus** o **Azure Blob Storage** como destino de la exportación. El número máximo de exportaciones por aplicación es cinco.

    ![Creación de una exportación de datos continua](media/howto-export-data/new-export-definition.png)

4. En el cuadro de lista desplegable, seleccione su **espacio de nombres de Event Hubs**, **espacio de nombres de Service Bus**, **espacio de nombres de la cuenta de almacenamiento** o **escriba una cadena de conexión**.

    - Solo verá las cuentas de almacenamiento, los espacios de nombres de Event Hubs y los espacios de nombres de Service Bus de la misma suscripción que la de la aplicación de IoT Central. Si quiere exportar a un destino fuera de esta suscripción, elija **Escriba una cadena de conexión** y vea el paso 5.
    - Para las aplicaciones creadas con el plan de tarifas gratis, la única manera de configurar la exportación de datos continua es mediante una cadena de conexión. Las aplicaciones en el plan de tarifas gratis no tienen una suscripción a Azure asociada.

    ![Creación de un centro de eventos](media/howto-export-data/export-event-hub.png)

5. (Opcional) Si eligió **Escriba una cadena de conexión**, aparece un nuevo cuadro para que pegue la cadena de conexión. Para obtener la cadena de conexión para su:
    - Event Hubs o Service Bus, vaya al espacio de nombres en Azure Portal.
        - En **Configuración**, seleccione **Directivas de acceso compartido**.
        - Elija el valor predeterminado **RootManageSharedAccessKey** o cree uno.
        - Copie la cadena de conexión principal o la secundaria.
    - Cuenta de almacenamiento, vaya a la cuenta de almacenamiento en Azure Portal:
        - En **Configuración**, seleccione **Claves de acceso**.
        - Copie la cadena de conexión key1 o la cadena de conexión key2.

6. Elija un centro de eventos, una cola, un tema o un contenedor del cuadro de lista desplegable.

7. En **Datos para exportar**, elija los tipos de datos para exportar y establezca el tipo en **Activado**.

8. Para activar la exportación de datos continua, asegúrese de que **Habilitado** esté **Activado**. Seleccione **Guardar**.

9. Transcurridos unos minutos, los datos aparecen en el destino elegido.

## <a name="export-contents-and-format"></a>Exportación del contenido y formato

Los datos de telemetría exportados contienen la totalidad del mensaje que los dispositivos enviaron a IoT Central, no solo los valores de los propios datos de telemetría. Los datos de dispositivo exportados contienen los cambios en las propiedades y los metadatos de todos los dispositivos; las plantillas de dispositivo exportadas contienen los cambios en todas las plantillas de dispositivo.

Para Event Hubs y Service Bus, los datos se exportan casi en tiempo real. Los datos se ubican en la propiedad Body y están en formato JSON (consulte a continuación para ver ejemplos).

Para Blob Storage, los datos se exportan una vez por minuto y cada archivo contiene el lote de cambios desde el último archivo exportado. Los datos exportados se colocan en tres carpetas con formato JSON. Las rutas de acceso predeterminadas de la cuenta de almacenamiento son:

- Telemetría: _{contenedor}/{id. de aplicación}/telemetry/{AAAA}/{MM}/{dd}/{hh}/{mm}/{nombre de archivo}_
- Dispositivos: _{contenedor}/{id. de aplicación}/devices/{AAAA}/{MM}/{dd}/{hh}/{mm}/{nombre de archivo}_
- Plantillas de dispositivo: _{contenedor}/{id. de aplicación}/deviceTemplates/{AAAA}/{MM}/{dd}/{hh}/{mm}/{nombre de archivo}_

Puede examinar los archivos exportados en Azure Portal si va al archivo y elige la pestaña para **editar el blob**.


## <a name="telemetry"></a>Telemetría

Para Event Hubs y Service Bus, se exporta un mensaje nuevo rápidamente después de que IoT Central reciba el mensaje de un dispositivo y cada mensaje exportado contiene el mensaje completo que el dispositivo envió en la propiedad body en formato JSON.

Para Blob Storage, los mensajes se procesan por lotes y se exportan una vez por minuto. Los archivos exportados usan el mismo formato que los archivos de mensajes exportados por el [enrutamiento de mensajes de IoT Hub](../../iot-hub/tutorial-routing.md) a Blob Storage. 

> [!NOTE]
> Para Blob Storage, asegúrese de que los dispositivos estén enviando mensajes que tengan `contentType: application/JSON` y `contentEncoding:utf-8` (o `utf-16`, `utf-32`). Consulte la [documentación de IoT Hub](../../iot-hub/iot-hub-devguide-routing-query-syntax.md#message-routing-query-based-on-message-body) para ver un ejemplo.

El dispositivo que envió la telemetría se representa mediante el identificador del dispositivo (consulte las secciones siguientes). Para obtener los nombres de los dispositivos, exporte los datos del dispositivo y ponga en correlación cada mensaje mediante el objeto **connectionDeviceId** que coincide con el objeto **deviceId** del mensaje del dispositivo.

Este es un mensaje de ejemplo recibido en una cola o tema de Event Hubs o Service Bus.

```json
{
  "body":{
    "temp":67.96099945281145,
    "humid":58.51139305465015,
    "pm25":36.91162432340187
  },
  "annotations":{
    "iothub-connection-device-id":"<deviceId>",
    "iothub-connection-auth-method":"{\"scope\":\"hub\",\"type\":\"sas\",\"issuer\":\"iothub\",\"acceptingIpFilterRule\":null}",
    "iothub-connection-auth-generation-id":"<generationId>",
    "iothub-enqueuedtime":1539381029965,
    "iothub-message-source":"Telemetry",
    "x-opt-sequence-number":25325,
    "x-opt-offset":"<offset>",
    "x-opt-enqueued-time":1539381030200
  },
  "sequenceNumber":25325,
  "enqueuedTimeUtc":"2018-10-12T21:50:30.200Z",
  "offset":"<offset>",
  "properties":{
    "content_type":"application/json",
    "content_encoding":"utf-8"
  }
}
```

Este es un registro de ejemplo exportado al almacenamiento de blobs:

```json
{
  "EnqueuedTimeUtc":"2019-09-26T17:46:09.8870000Z",
  "Properties":{

  },
  "SystemProperties":{
    "connectionDeviceId":"<deviceid>",
    "connectionAuthMethod":"{\"scope\":\"device\",\"type\":\"sas\",\"issuer\":\"iothub\",\"acceptingIpFilterRule\":null}",
    "connectionDeviceGenerationId":"637051167384630591",
    "contentType":"application/json",
    "contentEncoding":"utf-8",
    "enqueuedTime":"2019-09-26T17:46:09.8870000Z"
  },
  "Body":{
    "temp":49.91322758395974,
    "humid":49.61214852573155,
    "pm25":25.87332214661367
  }
}
```

## <a name="devices"></a>Dispositivos

Cada mensaje o registro de una instantánea representa uno o más cambios que se han producido en un dispositivo, y en las propiedades del dispositivo y la nube desde la exportación del último mensaje. Esto incluye:

- `id` del dispositivo en IoT Central
- `displayName` del dispositivo
- Id. de la plantilla de dispositivo en `instanceOf`
- Marca `simulated`, se cumple si se trata de un dispositivo simulado
- Marca `provisioned`, se cumple si el dispositivo se ha aprovisionado
- Marca `approved`, se cumple si el dispositivo tiene aprobado el envío de datos
- Valores de propiedad
- `properties`, incluidos los valores de propiedades de la nube y del dispositivo

Los dispositivos eliminados no se exportan. Actualmente, no hay ningún indicador en los mensajes exportados para los dispositivos eliminados.

En Event Hubs y Service Bus, los mensajes que contienen datos del dispositivo se envían a la cola o tema de Event Hubs o Service Bus casi en tiempo real, tal y como aparece en IoT Central. 

Para Blob Storage, una nueva instantánea que contiene todos los cambios desde la última vez escrita se exporta una vez por minuto.

Este es un mensaje de ejemplo sobre dispositivos y datos de propiedades en una cola o tema de Event Hub o Service Bus:

```json
{
  "body":{
    "id": "<device Id>",
    "etag": "<etag>",
    "displayName": "Sensor 1",
    "instanceOf": "<device template Id>",
    "simulated": false,
    "provisioned": true,
    "approved": true,
    "properties": {
        "sensorComponent": {
            "setTemp": "30",
            "fwVersion": "2.0.1",
            "status": { "first": "first", "second": "second" },
            "$metadata": {
                "setTemp": {
                    "desiredValue": "30",
                    "desiredVersion": 3,
                    "desiredTimestamp": "2020-02-01T17:15:08.9284049Z",
                    "ackVersion": 3
                },
                "fwVersion": { "ackVersion": 3 },
                "status": {
                    "desiredValue": {
                        "first": "first",
                        "second": "second"
                    },
                    "desiredVersion": 2,
                    "desiredTimestamp": "2020-02-01T17:15:08.9284049Z",
                    "ackVersion": 2
                }
            },
            
        }
    },
    "installDate": { "installDate": "2020-02-01" }
},
  "annotations":{
    "iotcentral-message-source":"devices",
    "x-opt-partition-key":"<partitionKey>",
    "x-opt-sequence-number":39740,
    "x-opt-offset":"<offset>",
    "x-opt-enqueued-time":1539274959654
  },
  "partitionKey":"<partitionKey>",
  "sequenceNumber":39740,
  "enqueuedTimeUtc":"2020-02-01T18:14:49.3820326Z",
  "offset":"<offset>"
}
```

Esta es una instantánea de ejemplo que contiene los datos de los dispositivos y las propiedades en Blob Storage. Los archivos exportados contienen una sola línea por registro.

```json
{
  "id": "<device Id>",
  "etag": "<etag>",
  "displayName": "Sensor 1",
  "instanceOf": "<device template Id>",
  "simulated": false,
  "provisioned": true,
  "approved": true,
  "properties": {
      "sensorComponent": {
          "setTemp": "30",
          "fwVersion": "2.0.1",
          "status": { "first": "first", "second": "second" },
          "$metadata": {
              "setTemp": {
                  "desiredValue": "30",
                  "desiredVersion": 3,
                  "desiredTimestamp": "2020-02-01T17:15:08.9284049Z",
                  "ackVersion": 3
              },
              "fwVersion": { "ackVersion": 3 },
              "status": {
                  "desiredValue": {
                      "first": "first",
                      "second": "second"
                  },
                  "desiredVersion": 2,
                  "desiredTimestamp": "2020-02-01T17:15:08.9284049Z",
                  "ackVersion": 2
              }
          },
          
      }
  },
  "installDate": { "installDate": "2020-02-01" }
}
```

## <a name="device-templates"></a>Plantillas de dispositivo

Cada mensaje o registro de instantáneas representa uno o más cambios que se han producido en una plantilla de dispositivo publicada desde la exportación del último mensaje. Entre la información enviada en cada mensaje o registro se incluye lo siguiente:

- `id` de la plantilla de dispositivo que coincide con el objeto `instanceOf` del flujo de dispositivos anterior
- `displayName` de la plantilla de dispositivo
- El dispositivo `capabilityModel`, incluido su `interfaces`, y los datos de telemetría, las propiedades y las definiciones de comandos
- Definiciones de `cloudProperties`
- Reemplazos y valores iniciales, insertados con el `capabilityModel`

Las plantillas de dispositivo eliminadas no se exportan. Actualmente, no hay ningún indicador en los mensajes exportados para las plantillas de dispositivo eliminadas.

En Event Hubs y Service Bus, los mensajes que contienen datos de la plantilla de dispositivo se envían a la cola o tema de Event Hubs o Service Bus casi en tiempo real, tal y como aparece en IoT Central. 

Para Blob Storage, una nueva instantánea que contiene todos los cambios desde la última vez escrita se exporta una vez por minuto.

Este es un mensaje de ejemplo sobre datos de plantilla de dispositivo en una cola o tema de Event Hub o Service Bus:

```json
{
  "body":{
      "id": "<device template id>",
      "etag": "<etag>",
      "types": ["DeviceModel"],
      "displayName": "Sensor template",
      "capabilityModel": {
          "@id": "<capability model id>",
          "@type": ["CapabilityModel"],
          "contents": [],
          "implements": [
              {
                  "@id": "<component Id>",
                  "@type": ["InterfaceInstance"],
                  "name": "sensorComponent",
                  "schema": {
                      "@id": "<interface Id>",
                      "@type": ["Interface"],
                      "displayName": "Sensor interface",
                      "contents": [
                          {
                              "@id": "<id>",
                              "@type": ["Telemetry"],
                              "displayName": "Humidity",
                              "name": "humidity",
                              "schema": "double"
                          },
                          {
                              "@id": "<id>",
                              "@type": ["Telemetry", "SemanticType/Event"],
                              "displayName": "Error event",
                              "name": "error",
                              "schema": "integer"
                          },
                          {
                              "@id": "<id>",
                              "@type": ["Property"],
                              "displayName": "Set temperature",
                              "name": "setTemp",
                              "writable": true,
                              "schema": "integer",
                              "unit": "Units/Temperature/fahrenheit",
                              "initialValue": "30"
                          },
                          {
                              "@id": "<id>",
                              "@type": ["Property"],
                              "displayName": "Firmware version read only",
                              "name": "fwversion",
                              "schema": "string"
                          },
                          {
                              "@id": "<id>",
                              "@type": ["Property"],
                              "displayName": "Display status",
                              "name": "status",
                              "writable": true,
                              "schema": {
                                  "@id": "urn:testInterface:status:obj:ka8iw8wka:1",
                                  "@type": ["Object"]
                              }
                          },
                          {
                              "@id": "<id>",
                              "@type": ["Command"],
                              "commandType": "synchronous",
                              "request": {
                                  "@id": "<id>",
                                  "@type": ["SchemaField"],
                                  "displayName": "Configuration",
                                  "name": "config",
                                  "schema": "string"
                              },
                              "response": {
                                  "@id": "<id>",
                                  "@type": ["SchemaField"],
                                  "displayName": "Response",
                                  "name": "response",
                                  "schema": "string"
                              },
                              "displayName": "Configure sensor",
                              "name": "sensorConfig"
                          }
                      ]
                  }
              }
          ],
          "displayName": "Sensor capability model"
      },
      "solutionModel": {
          "@id": "<id>",
          "@type": ["SolutionModel"],
          "cloudProperties": [
              {
                  "@id": "<id>",
                  "@type": ["CloudProperty"],
                  "displayName": "Install date",
                  "name": "installDate",
                  "schema": "dateTime",
                  "valueDetail": {
                      "@id": "<id>",
                      "@type": ["ValueDetail/DateTimeValueDetail"]
                  }
              }
          ]
      }
  },
    "annotations":{
      "iotcentral-message-source":"deviceTemplates",
      "x-opt-partition-key":"<partitionKey>",
      "x-opt-sequence-number":25315,
      "x-opt-offset":"<offset>",
      "x-opt-enqueued-time":1539274985085
    },
    "partitionKey":"<partitionKey>",
    "sequenceNumber":25315,
    "enqueuedTimeUtc":"2019-10-02T16:23:05.085Z",
    "offset":"<offset>"
  }
}
```

Esta es una instantánea de ejemplo que contiene los datos de los dispositivos y las propiedades en Blob Storage. Los archivos exportados contienen una sola línea por registro.

```json
{
      "id": "<device template id>",
      "etag": "<etag>",
      "types": ["DeviceModel"],
      "displayName": "Sensor template",
      "capabilityModel": {
          "@id": "<capability model id>",
          "@type": ["CapabilityModel"],
          "contents": [],
          "implements": [
              {
                  "@id": "<component Id>",
                  "@type": ["InterfaceInstance"],
                  "name": "Sensor component",
                  "schema": {
                      "@id": "<interface Id>",
                      "@type": ["Interface"],
                      "displayName": "Sensor interface",
                      "contents": [
                          {
                              "@id": "<id>",
                              "@type": ["Telemetry"],
                              "displayName": "Humidity",
                              "name": "humidity",
                              "schema": "double"
                          },
                          {
                              "@id": "<id>",
                              "@type": ["Telemetry", "SemanticType/Event"],
                              "displayName": "Error event",
                              "name": "error",
                              "schema": "integer"
                          },
                          {
                              "@id": "<id>",
                              "@type": ["Property"],
                              "displayName": "Set temperature",
                              "name": "setTemp",
                              "writable": true,
                              "schema": "integer",
                              "unit": "Units/Temperature/fahrenheit",
                              "initialValue": "30"
                          },
                          {
                              "@id": "<id>",
                              "@type": ["Property"],
                              "displayName": "Firmware version read only",
                              "name": "fwversion",
                              "schema": "string"
                          },
                          {
                              "@id": "<id>",
                              "@type": ["Property"],
                              "displayName": "Display status",
                              "name": "status",
                              "writable": true,
                              "schema": {
                                  "@id": "urn:testInterface:status:obj:ka8iw8wka:1",
                                  "@type": ["Object"]
                              }
                          },
                          {
                              "@id": "<id>",
                              "@type": ["Command"],
                              "commandType": "synchronous",
                              "request": {
                                  "@id": "<id>",
                                  "@type": ["SchemaField"],
                                  "displayName": "Configuration",
                                  "name": "config",
                                  "schema": "string"
                              },
                              "response": {
                                  "@id": "<id>",
                                  "@type": ["SchemaField"],
                                  "displayName": "Response",
                                  "name": "response",
                                  "schema": "string"
                              },
                              "displayName": "Configure sensor",
                              "name": "sensorconfig"
                          }
                      ]
                  }
              }
          ],
          "displayName": "Sensor capability model"
      },
      "solutionModel": {
          "@id": "<id>",
          "@type": ["SolutionModel"],
          "cloudProperties": [
              {
                  "@id": "<id>",
                  "@type": ["CloudProperty"],
                  "displayName": "Install date",
                  "name": "installDate",
                  "schema": "dateTime",
                  "valueDetail": {
                      "@id": "<id>",
                      "@type": ["ValueDetail/DateTimeValueDetail"]
                  }
              }
          ]
      }
  }
```
## <a name="data-format-change-notice"></a>Aviso de cambio de formato de datos

> [!Note]
> El formato de los datos del flujo de telemetría no se ve afectado por este cambio. Solo se ven afectados los flujos de datos de dispositivos y de plantillas de dispositivos.

Si tiene una exportación de datos existente en la aplicación de versión preliminar con los flujos *Dispositivos* y *Plantillas de dispositivo* activados, tendrá que actualizar la exportación antes del **30 de junio de 2020**. Esto se aplica a las exportaciones a Azure Blob Storage, Azure Event Hubs y Azure Service Bus.

A partir del 3 de febrero de 2020, todas las nuevas exportaciones en aplicaciones con Dispositivos y Plantillas de dispositivo habilitados tendrán el formato de datos descrito anteriormente. Todas las exportaciones creadas antes de esta fecha permanecerán en el formato de datos antiguo hasta el 30 de junio de 2020, tras lo cual, estas exportaciones se migrarán automáticamente al nuevo formato de datos. El nuevo formato de datos coincide con los objetos [dispositivo](https://docs.microsoft.com/rest/api/iotcentral/devices/get), [propiedad del dispositivo](https://docs.microsoft.com/rest/api/iotcentral/devices/getproperties), [propiedad de la nube del dispositivo](https://docs.microsoft.com/rest/api/iotcentral/devices/getcloudproperties) y [plantilla de dispositivo](https://docs.microsoft.com/rest/api/iotcentral/devicetemplates/get) en la API pública de IoT Central. 
 
En el caso de **Dispositivos**, las diferencias importantes entre el formato de datos anterior y el nuevo formato de datos incluyen:
- Se quitó `@id` para el dispositivo, se cambió el nombre de `deviceId` a `id` 
- Se agregó la marca `provisioned` para describir el estado de aprovisionamiento del dispositivo
- Se agregó la marca `approved` para describir el estado de aprobación del dispositivo
- `properties`, incluidas las propiedades de dispositivo y de nube, coincide con las entidades de la API pública

En el caso de **Plantillas de dispositivo**, las diferencias importantes entre el formato de datos anterior y el nuevo formato de datos incluyen:

- `@id` para la plantilla de dispositivo cambió de nombre a `id`
- `@type` para la plantilla de dispositivo cambió de nombre a `types`y ahora es una matriz

### <a name="devices-format-deprecated-as-of-3-february-2020"></a>Dispositivos (formato en desuso a partir del 3 de febrero de 2020)
```json
{
  "@id":"<id-value>",
  "@type":"Device",
  "displayName":"Airbox",
  "data":{
    "$cloudProperties":{
        "Color":"blue"
    },
    "EnvironmentalSensor":{
      "thsensormodel":{
        "reported":{
          "value":"Neque quia et voluptatem veritatis assumenda consequuntur quod.",
          "$lastUpdatedTimestamp":"2019-09-30T20:35:43.8478978Z"
        }
      },
      "pm25sensormodel":{
        "reported":{
          "value":"Aut alias odio.",
          "$lastUpdatedTimestamp":"2019-09-30T20:35:43.8478978Z"
        }
      }
    },
    "urn_azureiot_DeviceManagement_DeviceInformation":{
      "totalStorage":{
        "reported":{
          "value":27900.9730905171,
          "$lastUpdatedTimestamp":"2019-09-30T20:35:43.8478978Z"
        }
      },
      "totalMemory":{
        "reported":{
          "value":4667.82916715811,
          "$lastUpdatedTimestamp":"2019-09-30T20:35:43.8478978Z"
        }
      }
    }
  },
  "instanceOf":"<template-id>",
  "deviceId":"<device-id>",
  "simulated":true
}
```

### <a name="device-templates-format-deprecated-as-of-3-february-2020"></a>Plantillas de dispositivo (formato en desuso a partir del 3 de febrero de 2020)
```json
{
  "@id":"<template-id>",
  "@type":"DeviceModelDefinition",
  "displayName":"Airbox",
  "capabilityModel":{
    "@id":"<id>",
    "@type":"CapabilityModel",
    "implements":[
      {
        "@id":"<id>",
        "@type":"InterfaceInstance",
        "name":"EnvironmentalSensor",
        "schema":{
          "@id":"<id>",
          "@type":"Interface",
          "comment":"Requires temperature and humidity sensors.",
          "description":"Provides functionality to report temperature, humidity. Provides telemetry, commands and read-write properties",
          "displayName":"Environmental Sensor",
          "contents":[
            {
              "@id":"<id>",
              "@type":"Telemetry",
              "description":"Current temperature on the device",
              "displayName":"Temperature",
              "name":"temp",
              "schema":"double",
              "unit":"Units/Temperature/celsius",
              "valueDetail":{
                "@id":"<id>",
                "@type":"ValueDetail/NumberValueDetail",
                "minValue":{
                  "@value":"50"
                }
              },
              "visualizationDetail":{
                "@id":"<id>",
                "@type":"VisualizationDetail"
              }
            },
            {
              "@id":"<id>",
              "@type":"Telemetry",
              "description":"Current humidity on the device",
              "displayName":"Humidity",
              "name":"humid",
              "schema":"integer"
            },
            {
              "@id":"<id>",
              "@type":"Telemetry",
              "description":"Current PM2.5 on the device",
              "displayName":"PM2.5",
              "name":"pm25",
              "schema":"integer"
            },
            {
              "@id":"<id>",
              "@type":"Property",
              "description":"T&H Sensor Model Name",
              "displayName":"T&H Sensor Model",
              "name":"thsensormodel",
              "schema":"string"
            },
            {
              "@id":"<id>",
              "@type":"Property",
              "description":"PM2.5 Sensor Model Name",
              "displayName":"PM2.5 Sensor Model",
              "name":"pm25sensormodel",
              "schema":"string"
            }
          ]
        }
      },
      {
        "@id":"<id>",
        "@type":"InterfaceInstance",
        "name":"urn_azureiot_DeviceManagement_DeviceInformation",
        "schema":{
          "@id":"<id>",
          "@type":"Interface",
          "displayName":"Device information",
          "contents":[
            {
              "@id":"<id>",
              "@type":"Property",
              "comment":"Total available storage on the device in kilobytes. Ex. 20480000 kilobytes.",
              "displayName":"Total storage",
              "name":"totalStorage",
              "displayUnit":"kilobytes",
              "schema":"long"
            },
            {
              "@id":"<id>",
              "@type":"Property",
              "comment":"Total available memory on the device in kilobytes. Ex. 256000 kilobytes.",
              "displayName":"Total memory",
              "name":"totalMemory",
              "displayUnit":"kilobytes",
              "schema":"long"
            }
          ]
        }
      }
    ],
    "displayName":"AAEONAirbox52"
  },
  "solutionModel":{
    "@id":"<id>",
    "@type":"SolutionModel",
    "cloudProperties":[
      {
        "@id":"<id>",
        "@type":"CloudProperty",
        "displayName":"Color",
        "name":"Color",
        "schema":"string",
        "valueDetail":{
          "@id":"<id>",
          "@type":"ValueDetail/StringValueDetail"
        },
        "visualizationDetail":{
          "@id":"<id>",
          "@type":"VisualizationDetail"
        }
      }
    ]
  }
}
```
## <a name="next-steps"></a>Pasos siguientes

Ahora que sabe cómo exportar los datos a Azure Event Hubs, Azure Service Bus y Azure Blob Storage, vaya al paso siguiente:

> [!div class="nextstepaction"]
> [Procedimiento para crear webhooks](./howto-create-webhooks.md)

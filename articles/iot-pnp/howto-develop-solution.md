---
title: Interacción con un dispositivo de IoT Plug and Play (versión preliminar) desde una solución de Azure IoT | Microsoft Docs
description: Como desarrollador de soluciones, aprenda a usar el SDK del servicio para interactuar con dispositivos de IoT Plug and Play.
author: Philmea
ms.author: philmea
ms.date: 12/26/2019
ms.topic: how-to
ms.custom: mvc
ms.service: iot-pnp
services: iot-pnp
manager: philmea
ms.openlocfilehash: e349aadfd629202b1c8cdb5c53a88e0a6c2e06de
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "80159224"
---
# <a name="connect-to-and-interact-with-an-iot-plug-and-play-preview-device"></a>Conexión e interacción con un dispositivo de la versión preliminar de IoT Plug and Play

En esta guía paso a paso se indica cómo usar los ejemplos del SDK del servicio de Node que muestran cómo la solución de IoT puede interactuar con los dispositivos de la versión preliminar de IoT Plug and Play.

Si no ha completado el inicio rápido [Conexión de un dispositivo de IoT Plug and Play a la solución](quickstart-connect-pnp-device-solution-node.md), debe hacerlo ahora. En él se muestra cómo descargar e instalar el SDK y ejecutar algunos de los ejemplos.

Antes de ejecutar los ejemplos de servicio, abra un nuevo terminal, vaya a la carpeta raíz del repositorio clonado, vaya a la carpeta **digitaltwins/quickstarts/service** y, luego, ejecute el siguiente comando para instalar las dependencias:

```cmd/sh
npm install
```

## <a name="run-the-service-samples"></a>Ejecución de los ejemplos de servicio

Use los ejemplos siguientes para explorar las funcionalidades del SDK del servicio de Node.js. Asegúrese de que la variable de entorno `IOTHUB_CONNECTION_STRING` esté establecida en el shell que se usa:

### <a name="retrieve-a-digital-twin-and-list-the-interfaces"></a>Recuperación de un gemelo digital y enumeración de las interfaces

**get_digital_twin.js** obtiene el gemelo digital asociado al dispositivo e imprime su componente en la línea de comandos. No necesita un ejemplo de dispositivo en ejecución para funcionar correctamente.

**get_digital_twin_interface_instance.js** obtiene una única instancia de la interfaz del gemelo digital asociado al dispositivo y la imprime en la línea de comandos. No se necesita el ejemplo del dispositivo para funcionar.

### <a name="get-and-set-properties-using-the-node-service-sdk"></a>Obtención y establecimiento de propiedades mediante el SDK del servicio de Node

**update_digital_twin.js** actualiza una propiedad grabable en el gemelo digital del dispositivo mediante una revisión completa. Si quiere, puede actualizar varias propiedades en varias interfaces. Para que esta interfaz funcione correctamente, el ejemplo de dispositivo debe estar en ejecución al mismo tiempo. Que funcione correctamente puede suponer que el ejemplo de dispositivo imprima algo sobre la actualización de una propiedad y que el ejemplo de servicio imprima un gemelo digital actualizado en el terminal.

### <a name="send-a-command-and-retrieve-the-response-using-the-node-service-sdk"></a>Envío de un comando y recuperación de la respuesta mediante el SDK del servicio de Node

**invoke_command.js** invoca un comando sincrónico en el gemelo digital del dispositivo. Para que esta interfaz funcione correctamente, el ejemplo de dispositivo debe estar en ejecución al mismo tiempo. Que funcione correctamente puede suponer que el ejemplo del dispositivo imprima algo sobre la confirmación de un comando y que el cliente del servicio imprima el resultado del comando en el terminal.

### <a name="connect-to-the-public-repository-and-retrieve-a-model-definition-using-the-node-service-sdk"></a>Conexión al repositorio público y recuperación de una definición de modelo mediante el SDK del servicio de Node

Con las mismas instrucciones que para los ejemplos de servicio y dispositivo, debe establecer la siguiente variable de entorno:

* `AZURE_IOT_MODEL_REPOSITORY_CONNECTION_STRING`

Encontrará esta cadena de conexión en el [portal de Azure Certified for IoT](https://preview.catalog.azureiotsolutions.com), en la pestaña **Connection strings** (Cadenas de conexión) del **repositorio de la empresa**.

La cadena de conexión se parece al ejemplo siguiente:

```text
HostName={repo host name};RepositoryId={repo ID};SharedAccessKeyName={repo key ID};SharedAccessKey={repo key secret}
```

Cuando haya establecido estas cuatro variables de entorno, ejecute el ejemplo de la misma manera que ejecutó los otros ejemplos:

```cmd/sh
node model_repo.js
```

En este ejemplo se descarga la interfaz **ModelDiscovery** y se imprime este modelo en el terminal.

### <a name="run-queries-in-iot-hub-based-on-capability-models-and-interfaces"></a>Ejecución de consultas en IoT Hub según los modelos de funcionalidad y las interfaces

El lenguaje de consulta de IoT Hub admite `HAS_INTERFACE` y `HAS_CAPABILITYMODEL` como se muestra en los ejemplos siguientes:

```sql
select * from devices where HAS_INTERFACE('id without version', version)
```

```sql
select * from devices where HAS_CAPABILITYMODEL('id without version', version)
```

### <a name="creating-digital-twin-routes"></a>Creación de rutas de gemelos digitales

La solución puede recibir notificaciones de eventos de cambios de gemelos digitales. Para suscribirse a estas notificaciones, use la [característica de enrutamiento de IoT Hub](../iot-hub/iot-hub-devguide-endpoints.md) para enviar las notificaciones a un punto de conexión, como Blob Storage, Event Hubs o una cola de Service Bus.

Para crear una ruta de gemelo digital, siga estos pasos:

1. En Azure Portal, vaya hasta su recurso de IoT Hub.
1. Seleccione **Enrutamiento de mensajes**.
1. En la pestaña **Rutas**, seleccione **Agregar**.
1. Escriba un valor en el campo **Nombre** y elija un **punto de conexión**. Si no ha configurado un punto de conexión, seleccione **Agregar punto de conexión**.
1. En la lista desplegable **Origen de datos**, seleccione **Digital Twin Change Events** (Eventos de cambio de gemelo digital).
1. Seleccione **Guardar**.

El siguiente JSON muestra un ejemplo de un evento de cambio de gemelo digital:

```json
{
  "interfaces": {
    "urn_azureiot_ModelDiscovery_DigitalTwin": {
      "name": "urn_azureiot_ModelDiscovery_DigitalTwin",
      "properties": {
        "modelInformation": {
          "reported": {
            "value": {
              "modelId": "urn:domain:capabilitymodel:TestCapability:1",
              "interfaces": {
                "MyInterfaceFoo": "urn:domain:interfaces:FooInterface:1",
                "urn_azureiot_ModelDiscovery_DigitalTwin": "urn:azureiot:ModelDiscovery:DigitalTwin:1"
              }
            }
          }
        }
      }
    },
    "MyInterfaceFoo": {
      "name": "MyInterfaceFoo",
      "properties": {
        "property_1": { "desired": { "value": "value_1" } },
        "property_2": {
          "desired": { "value": 20 },
          "reported": {
            "value": 10,
            "desiredState": {
              "code": 200,
              "version": 22,
              "subCode": 400,
              "description": ""
            }
          }
        },
        "property_3": { "reported": { "value": "value_3" } }
      }
    }
  },
  "version": 4
}
```

## <a name="next-steps"></a>Pasos siguientes

Ahora que ha aprendido sobre las soluciones del servicio que interactúan con los dispositivos de IoT Plug and Play, un paso siguiente sugerido es obtener información sobre la [detección de modelos](concepts-model-discovery.md).

---
title: Implementación de la detección de modelo de IoT Plug and Play (versión preliminar) | Microsoft Docs
description: Los creadores de soluciones deben aprender a implementar la detección de modelo de IoT Plug and Play en las soluciones.
author: prashmo
ms.author: prashmo
ms.date: 07/23/2020
ms.topic: conceptual
ms.service: iot-pnp
services: iot-pnp
ms.openlocfilehash: 364b85a8ead09858b97d5d7e6ca8c130b9960b2c
ms.sourcegitcommit: 46f8457ccb224eb000799ec81ed5b3ea93a6f06f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/28/2020
ms.locfileid: "87337388"
---
# <a name="implement-iot-plug-and-play-preview-model-discovery-in-an-iot-solution"></a>Implementación de la detección de modelo de IoT Plug and Play (versión preliminar) en una solución de IoT

En este artículo se describe la forma en que los creadores de soluciones, pueden implementar la detección de modelo de IoT Plug and Play (versión preliminar) en una solución de IoT. En la detección de modelos describe cómo:

- Los dispositivos IoT Plug and Play registran su id. de modelo.
- Una solución de IoT recupera las interfaces implementadas por el dispositivo.

Hay dos grandes categorías de soluciones IoT:

- Las *soluciones de IoT creadas con un propósito* funcionan con un conjunto conocido de modelos de dispositivo IoT Plug and Play.

- Las *soluciones de IoT basadas en modelo* pueden funcionar con cualquier dispositivo IoT Plug and Play. La creación de una solución basada en modelos es más compleja, pero la ventaja es que la solución funcionará con cualquier dispositivo que agregue en el futuro.

    Para crear una solución de IoT basada en modelo es preciso crear una lógica con las primitivas de la interfaz de IoT Plug and Play: telemetría, propiedades y comandos. La lógica de la solución representa un dispositivo mediante la combinación de las funcionalidades de varios comandos, propiedades y datos de telemetría.

En este artículo se describe cómo implementar la detección de modelo en ambos tipos de soluciones.

## <a name="model-discovery"></a>Detección de modelo

Para detectar el modelo implementado por un dispositivo, una solución puede obtener el id. de modelo mediante la detección basada en eventos o la detección basada en gemelos:

### <a name="event-based-discovery"></a>Detección basada en eventos

Cuando un dispositivo IoT Plug and Play se conecta a IoT Hub, registra el modelo que implementa. Este registro da como resultado una notificación de [evento de cambio de gemelo digital](concepts-digital-twin.md#digital-twin-change-events). Para información sobre cómo habilitar el enrutamiento para eventos de gemelo digital, consulte [Uso del enrutamiento de mensajes de IoT Hub para enviar mensajes del dispositivo a la nube a distintos puntos de conexión](../iot-hub/iot-hub-devguide-messages-d2c.md#non-telemetry-events).

La solución puede usar el evento que se muestra en el siguiente fragmento de código para obtener información sobre el dispositivo IoT Plug and Play que se conecta y obtiene su id. de modelo:

```json
iothub-connection-device-id:sample-device
iothub-enqueuedtime:7/22/2020 8:02:27 PM
iothub-message-source:digitalTwinChangeEvents
correlation-id:100f322dc2c5
content-type:application/json-patch+json
content-encoding:utf-8
[
  {
    "op": "replace",
    "path": "/$metadata/$model",
    "value": "dtmi:com:example:TemperatureController;1"
  }
]
```

Este evento se desencadena cuando se agrega o actualiza el id. de modelo de dispositivo.

### <a name="twin-based-discovery"></a>Detección basada en gemelos

Si la solución quiere información sobre las funcionalidades de un dispositivo determinado, puede usar la API [Get Digital Twin](https://docs.microsoft.com/rest/api/iothub/service/digitaltwin/getdigitaltwin) (Obtener gemelo digital) para recuperarla.

En el siguiente fragmento de código de gemelo digital, `$metadata.$model` contiene el id. de modelo de un dispositivo IoT Plug and Play:

```json
{
    "$dtId": "sample-device",
    "$metadata": {
        "$model": "dtmi:com:example:TemperatureController;1",
        "serialNumber": {
            "lastUpdateTime": "2020-07-17T06:10:31.9609233Z"
        }
    }
}
```

La solución también puede usar **Get Twin** (Obtener gemelo) para recuperar el id. de modelo del dispositivo digital, tal como se muestra en el siguiente fragmento de código:

```json
{
    "deviceId": "sample-device",
    "etag": "AAAAAAAAAAc=",
    "deviceEtag": "NTk0ODUyODgx",
    "status": "enabled",
    "statusUpdateTime": "0001-01-01T00:00:00Z",
    "connectionState": "Disconnected",
    "lastActivityTime": "2020-07-17T06:12:26.8402249Z",
    "cloudToDeviceMessageCount": 0,
    "authenticationType": "sas",
    "x509Thumbprint": {
        "primaryThumbprint": null,
        "secondaryThumbprint": null
    },
    "modelId": "dtmi:com:example:TemperatureController;1",
    "version": 15,
    "properties": {...}
    }
}
```

## <a name="model-resolution"></a>Resolución de modelos

Una solución usa la resolución de modelos para obtener acceso a las interfaces que forman un modelo a partir del id. de modelo. 

- Las soluciones pueden optar por almacenar estas interfaces como archivos en una carpeta local. 
- Las soluciones pueden usar el [repositorio de modelos](concepts-model-repository.md).

## <a name="next-steps"></a>Pasos siguientes

Ahora que ha obtenido información sobre la detección de modelos en una solución de IoT, consulte [Plataforma Azure IoT](overview-iot-plug-and-play.md) para usar otras funcionalidades de la solución.

- [Interacción con un dispositivo desde su solución](quickstart-service-node.md)
- [API de REST de gemelo digital de IoT](https://docs.microsoft.com/rest/api/iothub/service/digitaltwin)
- [Azure IoT Explorer](howto-use-iot-explorer.md)

---
title: Interacción con un dispositivo IoT Plug and Play (versión preliminar) que esté conectado a la solución (Node.js) | Microsoft Docs
description: Use Node.js para conectarse e interactuar con un dispositivo IoT Plug and Play (versión preliminar) que esté conectado a su solución de Azure IoT.
author: miagdp
ms.author: miag
ms.date: 12/27/2019
ms.topic: quickstart
ms.service: iot-pnp
services: iot-pnp
ms.custom: mvc
ms.openlocfilehash: 7f86ae8ffefa88fe1cdfa429b7de4671422839ff
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/23/2020
ms.locfileid: "87044011"
---
# <a name="quickstart-interact-with-an-iot-plug-and-play-preview-device-thats-connected-to-your-solution-nodejs"></a>Inicio rápido: Interacción con un dispositivo IoT Plug and Play (versión preliminar) que esté conectado a una solución (Node.js)

[!INCLUDE [iot-pnp-quickstarts-3-selector.md](../../includes/iot-pnp-quickstarts-3-selector.md)]

IoT Plug and Play (versión preliminar) simplifica IoT, ya que permite interactuar con las funcionalidades de un dispositivo sin tener que conocer la implementación subyacente del dispositivo. En este inicio rápido se muestra cómo usar Node.js para conectarse y controlar un dispositivo IoT Plug and Play que esté conectado a una solución.

## <a name="prerequisites"></a>Prerrequisitos

Para completar este inicio rápido, necesita Node.js en la máquina de desarrollo. Puede descargar la última versión recomendada para varias plataformas desde [nodejs.org](https://nodejs.org).

Puede verificar la versión actual de Node.js en el equipo de desarrollo con el comando siguiente:

```cmd/sh
node --version
```

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

[!INCLUDE [iot-pnp-prepare-iot-hub.md](../../includes/iot-pnp-prepare-iot-hub.md)]

Ejecute el siguiente comando para obtener la _cadena de conexión de IoT Hub_ para el centro (anótela para usarla más adelante):

```azurecli-interactive
az iot hub show-connection-string --hub-name <YourIoTHubName> --output table
```

## <a name="run-the-sample-device"></a>Ejecución del dispositivo de ejemplo

En este inicio rápido, se usa un sensor ambiental de ejemplo, que se ha escrito en Node.js, como dispositivo IoT Plug and Play. Las siguientes instrucciones muestran cómo instalar y ejecutar el dispositivo:

1. Abra una ventana de terminal en el directorio que prefiera. Ejecute el siguiente comando para clonar el repositorio de GitHub de [ejemplos de Azure IoT para Node.js](https://github.com/azure-samples/azure-iot-samples-node) en esta ubicación:

    ```cmd/sh
    git clone https://github.com/azure-samples/azure-iot-samples-node
    ```

1. Esta ventana de terminal se usará ahora como terminal de _dispositivo_. Vaya a la carpeta del repositorio clonado y luego a la carpeta **/azure-iot-samples-node/digital-twins/Quickstarts/Device**. Instale todas las dependencias, para lo que debe ejecutar el siguiente comando:

    ```cmd/sh
    npm install
    ```

1. Configure la _cadena de conexión del dispositivos_:

    ```cmd/sh
    set DEVICE_CONNECTION_STRING=<YourDeviceConnectionString>
    ```

1. Ejecute el ejemplo con el siguiente comando:

    ```cmd/sh
    node sample_device.js
    ```

1. Verá mensajes que señalan que el dispositivo ha enviado cierta información y ha indicado que está en línea. Esto significa que el dispositivo ha comentado a enviar datos de telemetría al centro y que ya está listo para recibir comandos y actualizaciones de propiedades. No cierre este terminal, ya que lo necesitará más adelante para confirmar que los ejemplos del servicio también han funcionado.

## <a name="run-the-sample-solution"></a>Ejecución de la solución de ejemplo

En este inicio rápido, se usa una solución de IoT de ejemplo creada en Node.js para interactuar con el dispositivo de ejemplo.

1. Abra otra ventana de terminal (que será el terminal de _servicio_). Vaya a la carpeta del repositorio clonado y luego a la carpeta **/azure-iot-samples-node/digital-twins/Quickstarts/Service**. Instale todas las dependencias, para lo que debe ejecutar el siguiente comando:

    ```cmd/sh
    npm install
    ```

1. Configure la _cadena de conexión de IoT Hub_ para permitir que el servicio se conecte a ella:

    ```cmd/sh
    set IOTHUB_CONNECTION_STRING=<YourIoTHubConnectionString>
    ```

### <a name="read-a-property"></a>Lectura de una propiedad

1. Al conectar el _dispositivo_ en su terminal, vio el siguiente mensaje que indica su estado en línea:

    ```cmd/sh
    reported state property as online
    ```

1. En la carpeta **/azure-iot-samples-node/digital-twins/Quickstarts/Service**, abra el archivo **get_digital_twin.js**. Reemplace el marcador de posición `<DEVICE_ID_GOES_HERE>` por el identificador de dispositivo y guarde el archivo.

1. Vaya al terminal de _servicio_ y use el siguiente comando para ejecutar el ejemplo para leer información del dispositivo:

    ```cmd/sh
    node get_digital_twin.js
    ```

1. En la salida del terminal de _servicio_, desplácese hasta el componente `environmentalSensor`. Verá que la propiedad `state` figura como _en línea_:

    ```JSON
    "environmentalSensor": {
      "name": "environmentalSensor",
      "properties": {
        "state": {
          "reported": {
            "value": "online"
          }
        }
      }
    }
    ```

### <a name="update-a-writable-property"></a>Actualización de una propiedad editable

1. Abra el archivo **update_digital_twin_property.js**.

1. Al principio del archivo, hay un conjunto de constantes definidas con marcadores de posición en mayúsculas. Reemplace el marcador de posición `<DEVICE_ID_GOES_HERE>` por el identificador de dispositivo real, actualice las constantes restantes con los valores siguientes y guarde el archivo:

    ```javascript
    const interfaceInstanceName = 'environmentalSensor';
    const propertyName = 'brightness';
    const propertyValue = 42;
    ```

1. Vaya al terminal de _servicio_ y use el siguiente comando para ejecutar el ejemplo para actualizar la propiedad:

    ```cmd/sh
    node update_digital_twin_property.js
    ```

1. La salida del terminal de _servicio_ muestra la información actualizada del dispositivo. Desplácese hasta el componente `environmentalSensor` para ver el nuevo valor de brillo de 42.

    ```json
    "environmentalSensor": {
      "name": "environmentalSensor",
      "properties": {
        "brightness": {
          "desired": {
            "value": "42"
          }
        },
        "state": {
          "reported": {
            "value": "online"
          }
        }
      }
    }
    ```

1. Vaya al terminal del _dispositivo_ y verá que el dispositivo ha recibido la actualización:

    ```cmd/sh
    Received an update for brightness: 42
    updated the property
    ```
2. Vuelva al terminal de _servicio_ y ejecute de nuevo el comando siguiente para obtener la información del dispositivo y confirmar que la propiedad se ha actualizado.
    
    ```cmd/sh
    node get_digital_twin.js
    ```

3. En la salida del terminal de _servicio_, en el componente `environmentalSensor`, verá que se ha notificado el valor de brillo actualizado. Note: el dispositivo puede tardar un tiempo en finalizar la actualización. Este paso se puede repetir tantas veces como sea necesario hasta que el dispositivo haya procesado realmente la actualización de la propiedad.
    
    ```json
    "environmentalSensor": {
      "name": "environmentalSensor",
      "properties": {
        "brightness": {
          "reported": {
            "value": "42",
            "desiredState": {
              "code": 200,
              "version": 2,
              "description": "helpful descriptive text"
            }
          },
          "desired": {
            "value": "42"
          }
        },
        "state": {
          "reported": {
            "value": "online"
          }
        }
      }
    }
    ```

### <a name="invoke-a-command"></a>Invocación de un comando

1. Abra el archivo **invoke_command.js**.

1. Al principio del archivo, reemplace el marcador de posición `<DEVICE_ID_GOES_HERE>` por el identificador de dispositivo real. Actualice las constantes restantes con los valores siguientes y, después, guarde el archivo:

    ```javascript
    const interfaceInstanceName = 'environmentalSensor';
    const commandName = 'blink';
    const commandArgument = '<For the environmental sensor, this value does not matter. Any string will do.>'; 
    ```

1. Vaya al terminal de _servicio_. Use el siguiente comando para ejecutar el ejemplo para invocar el comando:

    ```cmd/sh
    node invoke_command.js
    ```

1. La salida del terminal de _servicio_ debe mostrar la siguiente confirmación:

    ```cmd/sh
    invoking command blink on interface instanceenvironmentalSensor for device <device ID>...
    {
      "result": "helpful response text",
      "statusCode": 200,
      "requestId": "<some ID value>",
      "_response": "helpful response text"
    }
    ```

1. Vaya al terminal del _dispositivo_ y verá que el comando se ha confirmado:

    ```cmd/sh
    received command: blink for interfaceInstance: environmentalSensor
    acknowledgement succeeded.
    ```

[!INCLUDE [iot-pnp-clean-resources.md](../../includes/iot-pnp-clean-resources.md)]

## <a name="next-steps"></a>Pasos siguientes

En este inicio rápido, ha aprendido a conectar un dispositivo IoT Plug and Play a una solución de IoT. Para más información sobre cómo crear una solución que interactúe con dispositivos IoT Plug and Play, consulte:

> [!div class="nextstepaction"]
> [Procedimiento: Conexión e interacción con un dispositivo](howto-develop-solution.md)

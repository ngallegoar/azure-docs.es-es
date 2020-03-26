---
title: Interacción con un dispositivo IoT Plug and Play (versión preliminar) conectado a una solución de Azure IoT | Microsoft Docs
description: Use C# (.NET) para conectarse e interactuar con un dispositivo IoT Plug and Play (versión preliminar) que esté conectado a su solución de IoT de Azure.
author: dominicbetts
ms.author: dobett
ms.date: 12/30/2019
ms.topic: quickstart
ms.service: iot-pnp
services: iot-pnp
ms.custom: mvc
ms.openlocfilehash: 0953f68839217c1c75eb86f8399ce023f3863ab4
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/26/2020
ms.locfileid: "76963978"
---
# <a name="quickstart-interact-with-an-iot-plug-and-play-preview-device-thats-connected-to-your-solution-c"></a>Inicio rápido: Interacción con un dispositivo IoT Plug and Play (versión preliminar) que esté conectado a la solución (C#)

[!INCLUDE [iot-pnp-quickstarts-3-selector.md](../../includes/iot-pnp-quickstarts-3-selector.md)]

IoT Plug and Play (versión preliminar) simplifica IoT, ya que permite interactuar con las funcionalidades de un dispositivo sin tener que conocer la implementación subyacente del dispositivo. En este inicio rápido se muestra cómo usar C# (con .NET) para conectarse y controlar un dispositivo IoT Plug and Play que esté conectado a la solución.

## <a name="prerequisites"></a>Prerrequisitos

Para completar este inicio rápido, necesita instalar .NET Core (2.x.x o 3.x.x) en la máquina de desarrollo. Puede descargar la versión preferida del SDK de .NET Core para varias plataformas desde [Descargar .NET Core](https://dotnet.microsoft.com/download/dotnet-core/).

Para comprobar la versión de .NET que se encuentra en la máquina de desarrollo, ejecute el siguiente comando en una ventana de terminal local: 

```cmd/sh
dotnet --version
```

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

[!INCLUDE [iot-pnp-prepare-iot-hub.md](../../includes/iot-pnp-prepare-iot-hub.md)]

Ejecute el siguiente comando para obtener la _cadena de conexión de IoT Hub_ para el centro (anótela para usarla más adelante):

```azurecli-interactive
az iot hub show-connection-string --hub-name <YourIoTHubName> --output table
```

## <a name="run-the-sample-device"></a>Ejecución del dispositivo de ejemplo

En este inicio rápido, se usa un sensor ambiental de ejemplo, que se ha escrito en C#, como dispositivo IoT Plug and Play. Las siguientes instrucciones muestran cómo instalar y ejecutar el dispositivo:

1. Abra una ventana de terminal en el directorio que prefiera. Ejecute el siguiente comando para clonar el repositorio de GitHub de [ejemplos de IoT de Azure para C# (.NET)](https://github.com/Azure-Samples/azure-iot-samples-csharp) en esta ubicación:

    ```cmd/sh
    git clone https://github.com/Azure-Samples/azure-iot-samples-csharp
    ```

1. Esta ventana de terminal se usará ahora como terminal de _dispositivo_. Vaya a la carpeta del repositorio clonado y luego a la carpeta **/azure-iot-samples-csharp/digitaltwin/Samples/device/EnvironmentalSensorSample**.

1. Configure la _cadena de conexión del dispositivos_:

    ```cmd/sh
    set DIGITAL_TWIN_DEVICE_CONNECTION_STRING=<YourDeviceConnectionString>
    ```

1. Cree los paquetes necesarios y ejecute el ejemplo con el comando siguiente:

    ```cmd\sh
        dotnet run
    ```

1. Verá mensajes que indican que el dispositivo se ha registrado correctamente y que espera actualizaciones de la nube. Esto significa que el dispositivo ya está listo para recibir comandos y actualizaciones de propiedades y que ha comenzado a enviar datos de telemetría al concentrador. No cierre este terminal, ya que lo necesitará más adelante para confirmar que los ejemplos del servicio también han funcionado.

## <a name="run-the-sample-solution"></a>Ejecución de la solución de ejemplo

En este inicio rápido, se usa una solución de IoT de ejemplo creada en C# para interactuar con el dispositivo de ejemplo.

1. Abra otra ventana de terminal (que será el terminal de _servicio_). Vaya a la carpeta del repositorio clonado y luego a la carpeta **/azure-iot-samples-csharp/digitaltwin/Samples/service**.

1. Configure la _cadena de conexión de IoT Hub_ y la _identidad del dispositivo_ para permitir que el servicio se conecte a ella:

    ```cmd/sh
    set IOTHUB_CONNECTION_STRING=<YourIoTHubConnectionString>
    set DEVICE_ID=<YourDeviceID>
    ```

### <a name="read-a-property"></a>Lectura de una propiedad

1. Al conectar el _dispositivo_ en su terminal, vio el siguiente mensaje que indica su estado en línea:

    ```cmd/sh
    Waiting to receive updates from cloud...
    ```

1. Vaya al terminal de _servicio_ y use los siguientes comandos para ejecutar el ejemplo para leer información del dispositivo:

    ```cmd/sh
    cd GetDigitalTwin
    dotnet run
    ```

1. En la salida del terminal de _servicio_, desplácese hasta el componente `environmentalSensor`. La propiedad `state`, que se usa para indicar si el dispositivo está o no en línea, se ha informado como _true_:

    ```JSON
    "environmentalSensor": {
      "name": "environmentalSensor",
      "properties": {
        "state": {
          "reported": {
            "value": true
          }
        }
      }
    }
    ```

### <a name="update-a-writable-property"></a>Actualización de una propiedad editable

1. Vaya al terminal de _servicio_ y establezca las siguientes variables para definir la propiedad que se va a actualizar:
    ```cmd/sh
    set INTERFACE_INSTANCE_NAME=environmentalSensor
    set PROPERTY_NAME=brightness
    set PROPERTY_VALUE=42
    ```

1. Utilice los siguientes comandos para ejecutar el ejemplo para actualizar la propiedad:

    ```cmd/sh
    cd ..\UpdateProperty
    dotnet run
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
            "value": true
          }
        }
      }
    }
    ```

1. Vaya al terminal del _dispositivo_ y verá que el dispositivo ha recibido la actualización:

    ```cmd/sh
    Received updates for property 'brightness'
    Desired brightness = '"42"'.
    Reported brightness = ''.
    Version is '2'.
    Sent pending status for brightness property.
    Sent completed status for brightness property.
    ```
2. Vuelva al terminal de _servicio_ y ejecute de nuevo el comando siguiente para obtener la información del dispositivo y confirmar que la propiedad se ha actualizado.
    
    ```cmd/sh
    cd ..\GetDigitalTwin
    dotnet run
    ```
3. En la salida del terminal de _servicio_, en el componente `environmentalSensor`, verá que se ha notificado el valor de brillo actualizado. Note: el dispositivo puede tardar un tiempo en finalizar la actualización. Este paso se puede repetir tantas veces como sea necesario hasta que el dispositivo haya procesado realmente la actualización de la propiedad.
    
    ```json
    "environmentalSensor": {
      "name": "environmentalSensor",
      "properties": {
        "brightness": {
          "desired": {
            "value": "42"
          },
          "reported": {
            "value": "42",
            "desiredState": {
              "code": 200,
              "description": "Request completed",
              "version": 2
            }
          }
        },
        "state": {
          "reported": {
            "value": true
          }
        }
      }
    },
    ```

### <a name="invoke-a-command"></a>Invocación de un comando

1. Vaya al terminal de _servicio_ y establezca las siguientes variables para definir el comando que se va a invocar:
    ```cmd/sh
    set INTERFACE_INSTANCE_NAME=environmentalSensor
    set COMMAND_NAME=blink
    ```

1. Use los comandos siguientes para ejecutar el ejemplo para invocar el comando:

    ```cmd/sh
    cd ..\InvokeCommand
    dotnet run
    ```

1. La salida del terminal de _servicio_ debe mostrar la siguiente confirmación:

    ```cmd/sh
    Invoking blink on device <YourDeviceID> with interface instance name environmentalSensor
    Command blink invoked on the device successfully, the returned status was 200 and the request id was <some ID value>
    The returned payload was
    {"description": "abc"}
    Enter any key to finish
    ```

1. Vaya al terminal del _dispositivo_ y verá que el comando se ha confirmado:

    ```cmd/sh
    Command - blink was invoked from the service
    Data - null
    Request Id - <some ID value>.
    ```

[!INCLUDE [iot-pnp-clean-resources.md](../../includes/iot-pnp-clean-resources.md)]

## <a name="next-steps"></a>Pasos siguientes

En este inicio rápido, ha aprendido a conectar un dispositivo IoT Plug and Play a una solución de IoT. Para más información sobre cómo crear una solución que interactúe con dispositivos IoT Plug and Play, consulte:

> [!div class="nextstepaction"]
> [Procedimiento: Conexión e interacción con un dispositivo](howto-develop-solution.md)

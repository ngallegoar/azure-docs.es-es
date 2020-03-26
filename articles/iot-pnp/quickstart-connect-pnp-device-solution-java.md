---
title: Interacción con un dispositivo IoT Plug and Play (versión preliminar) conectado a una solución de Azure IoT | Microsoft Docs
description: Use Java para conectarse e interactuar con un dispositivo IoT Plug and Play (versión preliminar) que esté conectado a su solución de IoT de Azure.
author: dominicbetts
ms.author: dobett
ms.date: 12/27/2019
ms.topic: quickstart
ms.service: iot-pnp
services: iot-pnp
ms.custom: mvc
ms.openlocfilehash: 53812e68fe397b81f29869565e0e4a0640a9ef23
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/26/2020
ms.locfileid: "76964641"
---
# <a name="quickstart-interact-with-an-iot-plug-and-play-preview-device-thats-connected-to-your-solution-java"></a>Inicio rápido: Interacción con un dispositivo IoT Plug and Play (versión preliminar) que esté conectado a la solución (Java)

[!INCLUDE [iot-pnp-quickstarts-3-selector.md](../../includes/iot-pnp-quickstarts-3-selector.md)]

IoT Plug and Play (versión preliminar) simplifica IoT, ya que permite interactuar con las funcionalidades de un dispositivo sin tener que conocer la implementación subyacente del dispositivo. En este inicio rápido se muestra cómo usar Java para conectarse y controlar un dispositivo IoT Plug and Play que esté conectado a la solución.

## <a name="prerequisites"></a>Prerrequisitos

Para completar este inicio rápido, necesita Java SE 8 en la máquina de desarrollo. También debe instalar Maven 3.

Para obtener más información sobre cómo configurar con ellos, consulte [Preparación del entorno de desarrollo](https://github.com/Azure/azure-iot-sdk-java/blob/preview/doc/java-devbox-setup.md) en el SDK de dispositivo IoT de Microsoft Azure para Java.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

[!INCLUDE [iot-pnp-prepare-iot-hub.md](../../includes/iot-pnp-prepare-iot-hub.md)]

Ejecute el siguiente comando para obtener la _cadena de conexión de IoT Hub_ para el centro (anótela para usarla más adelante):

```azurecli-interactive
az iot hub show-connection-string --hub-name <YourIoTHubName> --output table
```

## <a name="run-the-sample-device"></a>Ejecución del dispositivo de ejemplo

En este inicio rápido, se usa un sensor ambiental de ejemplo, que se ha escrito en Java, como dispositivo IoT Plug and Play. Las siguientes instrucciones muestran cómo instalar y ejecutar el dispositivo:

1. Abra una ventana de terminal en el directorio que prefiera. Ejecute el siguiente comando para clonar el repositorio de GitHub de [ejemplos de IoT de Azure para Java](https://github.com/Azure-Samples/azure-iot-samples-java) en esta ubicación:

    ```cmd/sh
    git clone https://github.com/Azure-Samples/azure-iot-samples-java
    ```

1. Esta ventana de terminal se usará ahora como terminal de _dispositivo_. Vaya a la carpeta del repositorio clonado y luego a la carpeta **/azure-iot-samples-java/digital-twin/Samples/device/JdkSample**. Instale las bibliotecas necesarias y compile la aplicación de dispositivo simulado mediante la ejecución del comando siguiente:

    ```cmd/sh
    mvn clean install -DskipTests
    ```

1. Configure la _cadena de conexión del dispositivos_:

    ```cmd/sh
    set DIGITAL_TWIN_DEVICE_CONNECTION_STRING=<YourDeviceConnectionString>
    ```

1. Ejecute el siguiente comando para ejecutar el ejemplo a la carpeta del dispositivo.

    ```cmd/sh
    java -jar environmental-sensor-sample\target\environmental-sensor-sample-with-deps.jar
    ```

1. Verá mensajes que indican que el dispositivo está conectado, realizando varios pasos de configuración y esperando actualizaciones de servicio, seguidos de los registros de telemetría. Esto significa que el dispositivo ya está listo para recibir comandos y actualizaciones de propiedades y que ha comenzado a enviar datos de telemetría al concentrador. Siga ejecutando el ejemplo a medida que complete los pasos siguientes. No cierre este terminal, ya que lo necesitará más adelante para confirmar que los ejemplos del servicio también han funcionado.

## <a name="run-the-sample-solution"></a>Ejecución de la solución de ejemplo

En este inicio rápido, se usa una solución de IoT de ejemplo creada en Java para interactuar con el dispositivo de ejemplo.

1. Abra otra ventana de terminal (que será el terminal de _servicio_). Vaya a la carpeta del repositorio clonado y luego a la carpeta **/azure-iot-samples-java\digital-twin\Samples\service\JdkSample**.

1. Instale las bibliotecas necesarias y compile el ejemplo de servicio mediante la ejecución del siguiente comando:

    ```cmd/sh
    mvn clean install -DskipTests
    ```

1. Configure la _cadena de conexión de IoT Hub_ y la _identidad del dispositivo_ para permitir que el servicio se conecte a ella:

    ```cmd/sh
    set IOTHUB_CONNECTION_STRING=<YourIoTHubConnectionString>
    set DEVICE_ID=<YourDeviceID>
    ```

### <a name="read-a-property"></a>Lectura de una propiedad

1. Al conectar el _dispositivo_ en su terminal, uno de los mensajes de salida fue el mensaje siguiente que indica su estado en línea: La propiedad `state`, que se usa para indicar si el dispositivo está o no en línea, es _true_:

    ```cmd/sh
    State of environmental sensor was set to true
    ```

1. Vaya al terminal de _servicio_ y use el siguiente comando para ejecutar el ejemplo de servicio para leer información del dispositivo:

    ```cmd/sh
    java -jar get-digital-twin/target/get-digital-twin-with-deps.jar
    ```

1. En la salida del terminal de _servicio_, desplácese hasta el componente `environmentalSensor`. Verá que la propiedad `state` figura como _true_:
    ```JSON
    "environmentalSensor" : {
      "name" : "environmentalSensor",
      "properties" : {
        "state" : {
          "reported" : {
            "value" : true,
            "desiredState" : null
          },
          "desired" : null
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

1. Use el siguiente comando para ejecutar el ejemplo de servicio para actualizar la propiedad:

    ```cmd/sh
    java -jar update-digital-twin/target/update-digital-twin-with-deps.jar
    ```

1. La salida del terminal de _servicio_ muestra la información actualizada del dispositivo. Desplácese hasta el componente `environmentalSensor` para ver el nuevo valor de brillo de 42.

    ```json
    "environmentalSensor": {
        "name": "environmentalSensor",
        "properties": {
            "brightness": {
                "reported": null,
                "desired": {
                    "value": "42"
                }
            },
    ```

1. Vaya al terminal del _dispositivo_ y verá que el dispositivo ha recibido la actualización:

    ```cmd/sh
    OnPropertyUpdate called: propertyName=brightness, reportedValue=null, desiredVersion=2, desiredValue={"value":"42"}
    Report property: propertyName=brightness, reportedValue={"value":"42"}, desiredVersion=2 was DIGITALTWIN_CLIENT_OK
    ```
2. Vuelva al terminal de _servicio_ y ejecute de nuevo el comando siguiente para obtener la información del dispositivo y confirmar que la propiedad se ha actualizado.
    
    ```cmd/sh
    java -jar get-digital-twin/target/get-digital-twin-with-deps.jar
    ```
3. En la salida del terminal de _servicio_, en el componente `environmentalSensor`, verá que se ha notificado el valor de brillo actualizado. Note: el dispositivo puede tardar un tiempo en finalizar la actualización. Este paso se puede repetir tantas veces como sea necesario hasta que el dispositivo haya procesado realmente la actualización de la propiedad.
    
    ```json
    "environmentalSensor" : {
      "name" : "environmentalSensor",
      "properties" : {
        "brightness" : {
          "reported" : {
            "value" : {
              "value" : "42"
            },
            "desiredState" : {
              "code" : 200,
              "version" : 2,
              "description" : "OK"
            }
          },
          "desired" : {
            "value" : "42"
          }
        },
        "state" : {
          "reported" : {
            "value" : true,
            "desiredState" : null
          },
          "desired" : null
        }
      }
    },       
    ```

### <a name="invoke-a-command"></a>Invocación de un comando

1. Vaya al terminal de _servicio_ y establezca las siguientes variables para definir el comando que se va a invocar:
    ```cmd/sh
    set INTERFACE_INSTANCE_NAME=environmentalSensor
    set COMMAND_NAME=blink
    set PAYLOAD=10
    ```

1. Use el siguiente comando para ejecutar el ejemplo de servicio para invocar el comando:

    ```cmd/sh
    java -jar invoke-digital-twin-command/target/invoke-digital-twin-command-with-deps.jar
    ```

1. La salida del terminal de _servicio_ debe mostrar la siguiente confirmación:

    ```cmd/sh
    Invoking blink on device <YourDeviceID> with interface instance name environmentalSensor
    Command invoked on the device successfully, the returned status was 200 and the request id was <some ID value>
    The returned PAYLOAD was
    ```

1. Vaya al terminal del _dispositivo_ y verá que el comando se ha confirmado:

    ```cmd/sh
    OnCommandReceived called: commandName=blink, requestId=<some ID value>, commandPayload="10"
    EnvironmentalSensor is blinking every 10 seconds.
    ```

[!INCLUDE [iot-pnp-clean-resources.md](../../includes/iot-pnp-clean-resources.md)]

## <a name="next-steps"></a>Pasos siguientes

En este inicio rápido, ha aprendido a conectar un dispositivo IoT Plug and Play a una solución de IoT. Para más información sobre cómo crear una solución que interactúe con dispositivos IoT Plug and Play, consulte:

> [!div class="nextstepaction"]
> [Procedimiento: Conexión e interacción con un dispositivo](howto-develop-solution.md)

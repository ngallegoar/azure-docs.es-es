---
title: 'Inicio rápido: Conexión del código del dispositivo Python de ejemplo de IoT Plug and Play a Azure IoT Hub | Microsoft Docs'
description: 'Inicio rápido: Uso Python para compilar y ejecutar el código de dispositivo de ejemplo de IoT Plug and Play conectado a un centro de IoT Use la herramienta Azure IoT Explorer para ver la información enviada por el dispositivo al centro.'
author: ericmitt
ms.author: ericmitt
ms.date: 7/14/2020
ms.topic: quickstart
ms.service: iot-pnp
services: iot-pnp
ms.custom: mvc, devx-track-azurecli
ms.openlocfilehash: 38c6b835f0a3ddaaf4c049ca6ea3aeb484ad3280
ms.sourcegitcommit: 7cc10b9c3c12c97a2903d01293e42e442f8ac751
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/06/2020
ms.locfileid: "93421573"
---
# <a name="quickstart-connect-a-sample-iot-plug-and-play-device-application-to-iot-hub-python"></a>Inicio rápido: Conexión de una aplicación de dispositivo IoT Plug and Play de ejemplo a IoT Hub (Python)

[!INCLUDE [iot-pnp-quickstarts-device-selector.md](../../includes/iot-pnp-quickstarts-device-selector.md)]

En este inicio rápido se muestra cómo compilar una aplicación de dispositivo IoT Plug and Play de ejemplo, conectarla al centro de IoT y usar la herramienta Azure IoT Explorer para ver los datos de telemetría que envía. La aplicación de ejemplo se escribe para Python y se incluye en el SDK de dispositivo de Azure IoT Hub para Python. Un generador de soluciones puede usar la herramienta Azure IoT Explorer para comprender las funcionalidades de cualquier dispositivo IoT Plug and Play sin necesidad de ver nada de código del dispositivo.

## <a name="prerequisites"></a>Requisitos previos

[!INCLUDE [iot-pnp-prerequisites](../../includes/iot-pnp-prerequisites.md)]

Para completar este inicio rápido, necesita Python 3.7 en la máquina de desarrollo. Puede descargar la última versión recomendada para varias plataformas desde [python.org](https://www.python.org/). Puede comprobar la versión de Python con el siguiente comando:  

```cmd/sh
python --version
```

En el entorno local de Python, instale el paquete como se indica a continuación:

```cmd/sh
pip install azure-iot-device
```

Clone el repositorio IoT del SDK de Python y consulte el **maestro**:

```cmd/sh
git clone https://github.com/Azure/azure-iot-sdk-python
```

## <a name="run-the-sample-device"></a>Ejecución del dispositivo de ejemplo

La carpeta *azure-iot-sdk-python\azure-iot-device\samples\pnp* contiene el código de ejemplo para el dispositivo IoT Plug and Play. En este inicio rápido se usa el archivo *simple_thermostat.py*. Este código de ejemplo implementa un dispositivo compatible con IoT Plug and Play y usa la biblioteca de cliente de dispositivo Python de Azure IoT.

Abra el archivo **simple_thermostat.py** en un editor de texto. Observe cómo:

1. Define un identificador de modelo de dispositivo gemelo (DTMI) único que representa de forma única el [termostato](https://github.com/Azure/opendigitaltwins-dtdl/blob/master/DTDL/v2/samples/Thermostat.json). El usuario debe conocer el DTMI, que varía en función del escenario de implementación del dispositivo. En el ejemplo actual, el modelo representa un termostato que tiene telemetría, propiedades y comandos asociados a la temperatura de supervisión.

1. Tiene funciones para definir implementaciones de controlador de comandos. Estos controladores se escriben para definir el modo en que el dispositivo responde a las solicitudes de comandos.

1. Tiene una función para definir una respuesta de comando. Debe crear funciones de respuesta de comandos para devolver una respuesta a su centro de IoT.

1. Define una función de escucha de teclado de entrada que le permite salir de la aplicación.

1. Tiene una función **main**. La función **main**:

    1. Usa el SDK de dispositivo para crear un cliente de dispositivo y conectarse a su centro de IoT.

    1. Actualiza las propiedades. El modelo que usamos, **Thermostat**, define `targetTemperature` y `maxTempSinceLastReboot` como las dos propiedades de nuestro termostato, de modo que es lo que vamos a usar. Las propiedades se actualizan mediante el método `patch_twin_reported_properties` definido en `device_client`.

    1. Inicia la escucha de las solicitudes de comando con la función **execute_command_listener**. La función configura un "agente de escucha" para escuchar los comandos que provienen del servicio. Al configurar el agente de escucha, debe proporcionar los objetos `method_name`, `user_command_handler` y `create_user_response_handler`.
        - La función `user_command_handler` define lo que debe hacer el dispositivo cuando recibe un comando. Por ejemplo, si suena la alarma, el efecto de recibir este comando es que despierta. Piense en este como el "efecto" del comando que invoca.
        - La función `create_user_response_handler` crea una respuesta para enviar al centro de IoT cuando un comando se ejecuta correctamente. Por ejemplo, si suena la alarma, su respuesta es posponerla, que es el comentario que se envía al servicio. Piense en esta como la respuesta que proporciona al servicio. Puede ver esta respuesta en el portal.

    1. Empieza a enviar telemetría. El objeto **pnp_send_telemetry** se define en el archivo pnp_methods.py. En el código de ejemplo se usa un bucle para llamar a esta función cada ocho segundos.

    1. Deshabilita todos los agentes de escucha y las tareas, y sale del bucle al presionar **Q** o **q**.

[!INCLUDE [iot-pnp-environment](../../includes/iot-pnp-environment.md)]

Para más información sobre la configuración de ejemplo, consulte el [archivo Léame de ejemplo](https://github.com/Azure/azure-iot-sdk-python/blob/master/azure-iot-device/samples/pnp/README.md).

Ahora que ha visto el código, use el siguiente comando para ejecutar el ejemplo:

```cmd/sh
python simple_thermostat.py
```

Verá la salida siguiente, lo que significa que el dispositivo está enviando datos de telemetría al centro y que ya está listo para recibir comandos y actualizaciones de propiedades:

```cmd/sh
Listening for command requests and property updates
Press Q to quit
Sending telemetry for temperature
Sent message
```

Siga ejecutando el ejemplo a medida que complete los pasos siguientes.

## <a name="use-azure-iot-explorer-to-validate-the-code"></a>Uso de Azure IoT Explorer para validar el código

Una vez iniciado el ejemplo de cliente de dispositivo, compruebe que funciona con la herramienta Azure IoT Explorer.

[!INCLUDE [iot-pnp-iot-explorer.md](../../includes/iot-pnp-iot-explorer.md)]

## <a name="next-steps"></a>Pasos siguientes

En este inicio rápido ha aprendido a conectar un dispositivo IoT Plug and Play a un centro de IoT. Para más información sobre cómo crear una solución que interactúe con dispositivos IoT Plug and Play, consulte:

> [!div class="nextstepaction"]
> [Interacción con un dispositivo IoT Plug and Play que esté conectado a una solución](quickstart-service-python.md)

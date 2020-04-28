---
title: Conexión del código del dispositivo de ejemplo de la versión preliminar de IoT Plug and Play a IoT Hub (Windows) | Microsoft Docs
description: Compile y ejecute el código de dispositivo de ejemplo de la versión preliminar de IoT Plug and Play en Windows que se conecta a un centro de IoT. Use la herramienta Azure IoT Explorer para ver la información enviada por el dispositivo al centro.
author: Philmea
ms.author: philmea
ms.date: 12/26/2019
ms.topic: quickstart
ms.service: iot-pnp
services: iot-pnp
ms.custom: mvc
ms.openlocfilehash: 080820024db1302ff5a841761428442396b90040
ms.sourcegitcommit: d57d2be09e67d7afed4b7565f9e3effdcc4a55bf
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/22/2020
ms.locfileid: "81769862"
---
# <a name="quickstart-connect-a-sample-iot-plug-and-play-preview-device-application-running-on-windows-to-iot-hub-c-windows"></a>Inicio rápido: Conexión de una aplicación de dispositivo de ejemplo de la versión preliminar de IoT Plug and Play en Windows a IoT Hub (C Windows)

[!INCLUDE [iot-pnp-quickstarts-2-selector.md](../../includes/iot-pnp-quickstarts-2-selector.md)]

En este inicio rápido se muestra cómo crear una aplicación de dispositivo IoT Plug and Play de ejemplo, conectarla a IoT Hub y usar la herramienta Azure IoT Explorer para ver la información que envía al centro. La aplicación de ejemplo se escribe en C y se incluye en el SDK de dispositivo de Azure IoT Hub para C. Los desarrolladores de soluciones puede usar la herramienta Azure IoT Explorer para conocer las funcionalidades de cualquier dispositivo IoT Plug and Play sin necesidad de ver nada de código de dispositivo.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="prerequisites"></a>Prerrequisitos

Para completar este inicio rápido, es preciso instalar el siguiente software en el equipo local:

* [Visual Studio (Community, Professional o Enterprise)](https://visualstudio.microsoft.com/downloads/): asegúrese de incluir el componente **Administrador de paquetes NuGet** y la carga de trabajo **Desarrollo para el escritorio con C++** al instalar Visual Studio.
* [Git](https://git-scm.com/download/).
* [CMake](https://cmake.org/download/).

### <a name="install-the-azure-iot-explorer"></a>Instalación de Azure IoT Explorer

Descargue e instale la versión más reciente de **Azure IoT Explorer** desde la página [Repositorio](https://github.com/Azure/azure-iot-explorer/releases) de la herramienta, seleccionando el archivo .msi en "Recursos" para la actualización más reciente.

[!INCLUDE [iot-pnp-prepare-iot-hub.md](../../includes/iot-pnp-prepare-iot-hub.md)]

Ejecute el siguiente comando para obtener la _cadena de conexión de IoT Hub_ para el centro (anótela para usarla más adelante):

```azurecli-interactive
az iot hub show-connection-string --hub-name <YourIoTHubName> --output table
```

## <a name="prepare-the-development-environment"></a>Preparación del entorno de desarrollo

En este inicio rápido, se prepara un entorno de desarrollo que se puede usar para clonar y compilar el SDK de C del dispositivo de Azure IoT Hub.

Abra un símbolo del sistema en el directorio que prefiera. Ejecute el siguiente comando para clonar el repositorio de GitHub de las [bibliotecas y los SDK de C de Azure IoT](https://github.com/Azure/azure-iot-sdk-c) en esta ubicación:

```cmd/sh
git clone https://github.com/Azure/azure-iot-sdk-c --recursive -b public-preview
```

Esta operación puede tardar varios minutos en completarse.

## <a name="build-the-code"></a>Compilación del código

Para compilar el código de ejemplo incluido, se usa el SDK del dispositivo. La aplicación que se compila simula un dispositivo que se conecta a un centro de IoT. La aplicación envía datos de telemetría y propiedades, y recibe comandos.

1. Cree un subdirectorio `cmake` en la carpeta raíz del SDK del dispositivo y vaya a esa carpeta:

    ```cmd\sh
    cd <root folder>\azure-iot-sdk-c
    mkdir cmake
    cd cmake
    ```

1. Ejecute los siguientes comandos para compilar el SDK del dispositivo y el código auxiliar generado:

    ```cmd\sh
    cmake ..
    cmake --build . -- /m /p:Configuration=Release
    ```

    > [!NOTE]
    > Si cmake no encuentra el compilador de C++, aparecerán errores de compilación al ejecutar el comando anterior. En ese caso, pruebe a ejecutar el comando en el [símbolo del sistema de Visual Studio](https://docs.microsoft.com/dotnet/framework/tools/developer-command-prompt-for-vs).

## <a name="run-the-device-sample"></a>Ejecución del ejemplo de dispositivo

Ejecute una aplicación de ejemplo en el SDK para simular un dispositivo IoT Plug and Play que envía telemetría a su instancia de IoT Hub. Para ejecutar la aplicación de ejemplo, use estos comandos y pase la _cadena de conexión del dispositivo_ como un parámetro.

```cmd\sh
cd digitaltwin_client\samples\digitaltwin_sample_device\Release
copy ..\EnvironmentalSensor.interface.json .
digitaltwin_sample_device.exe "<YourDeviceConnectionString>"
```

El dispositivo ya está listo para recibir comandos y actualizaciones de propiedades, y ha empezado a enviar datos de telemetría al centro. Siga ejecutando el ejemplo a medida que complete los pasos siguientes.

## <a name="use-the-azure-iot-explorer-to-validate-the-code"></a>Uso de Azure IoT Explorer para validar el código

[!INCLUDE [iot-pnp-iot-explorer-1.md](../../includes/iot-pnp-iot-explorer-1.md)]

4. Para asegurarse de que la herramienta puede leer las definiciones del modelo de interfaz del dispositivo, seleccione **Configuración**. En el menú Configuración, es posible que **On the connected device** (En el dispositivo conectado) ya aparezca en las configuraciones de Plug and Play. Si no es así, seleccione **+ Add module definition source** (Agregar origen de definición de módulo) y, después, **On the connected device** (En el dispositivo conectado) para agregarlo.

1. En la página de información general **Dispositivos**, busque la identidad del dispositivo que creó anteriormente y selecciónela para ver más detalles. Con la aplicación de dispositivo todavía en ejecución en el símbolo del sistema, compruebe que el **estado de conexión** del dispositivo en Azure IoT Explorer se muestra como _Conectado_ (de lo contrario, presione **Actualizar** hasta que lo sea). Seleccione el dispositivo para ver más detalles.

1. Expanda la interfaz con el identificador **urn:YOUR_COMPANY_NAME_HERE:EnvironmentalSensor:1** para ver la interfaz y las primitivas de IoT Plug and Play (propiedades, comandos y telemetría).

[!INCLUDE [iot-pnp-iot-explorer-2.md](../../includes/iot-pnp-iot-explorer-2.md)]

[!INCLUDE [iot-pnp-clean-resources.md](../../includes/iot-pnp-clean-resources.md)]

## <a name="next-steps"></a>Pasos siguientes

En este inicio rápido ha aprendido a conectar un dispositivo IoT Plug and Play a un centro de IoT. Para más información sobre cómo crear una solución que interactúe con dispositivos IoT Plug and Play, consulte:

> [!div class="nextstepaction"]
> [Procedimiento: Conexión e interacción con un dispositivo de la versión preliminar de IoT Plug and Play](howto-develop-solution.md)

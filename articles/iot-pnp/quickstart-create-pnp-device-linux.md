---
title: Creación de un dispositivo de Azure IoT Plug and Play en versión preliminar (Linux) | Microsoft Docs
description: Uso de un modelo de funcionalidad de dispositivo para generar código de dispositivo. Luego, ejecute el código de dispositivo y verá que el dispositivo se conecta a su centro de IoT.
author: dominicbetts
ms.author: dobett
ms.date: 12/27/2019
ms.topic: quickstart
ms.service: iot-pnp
services: iot-pnp
ms.custom: mvc
ms.openlocfilehash: d2cc440572d6f33480972c15f5c498cc384cb2e3
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/26/2020
ms.locfileid: "75550526"
---
# <a name="quickstart-use-a-device-capability-model-to-create-an-iot-plug-and-play-preview-device-linux"></a>Inicio rápido: Uso de un modelo de funcionalidad de dispositivo para crear un dispositivo de versión preliminar de IoT Plug and Play (Linux)

[!INCLUDE [iot-pnp-quickstarts-1-selector.md](../../includes/iot-pnp-quickstarts-1-selector.md)]

Un _modelo de funcionalidad de dispositivo_ (DCM) describe las funcionalidades de un dispositivo IoT Plug and Play. Un DCM suele estar asociado a una SKU del producto. Las funcionalidades definidas en el DCM se organizan en interfaces reutilizables. Desde un DCM se puede generar el código de un dispositivo básico. En este inicio rápido se muestra cómo usar VS Code en Ubuntu Linux para crear un dispositivo IoT Plug and Play mediante un DCM.

## <a name="prerequisites"></a>Prerrequisitos

En este inicio rápido se asume que usa Ubuntu Linux con un entorno de escritorio. Los pasos de este tutorial se han probado con Ubuntu 18.04.

Para completar este inicio rápido, es preciso instalar el siguiente software en la máquina Linux local:

* Instale **GCC**, **GIT**, **cmake** y todas las dependencias con el comando `apt-get`:

    ```sh
    sudo apt-get update
    sudo apt-get install -y git cmake build-essential curl libcurl4-openssl-dev libssl-dev uuid-dev
    ```

    Compruebe que la versión de `cmake` es posterior a **2.8.12** y que la versión de **GCC** es posterior a **4.4.7**.

    ```sh
    cmake --version
    gcc --version
    ```

* [Visual Studio Code](https://code.visualstudio.com/).

### <a name="install-azure-iot-tools"></a>Instalación de Azure IoT Tools

Siga estos pasos para instalar el paquete de extensiones [Azure IoT Tools para VS Code](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-tools):

1. En VS Code, seleccione la pestaña **Extensions** (Extensiones).
1. Busque **Azure IoT Tools**.
1. Seleccione **Instalar**.

### <a name="get-the-connection-string-for-your-company-model-repository"></a>Obtención de la cadena de conexión para el repositorio de modelos de su empresa

Puede encontrar la _cadena de conexión para el repositorio de modelos de su empresa_ en el [ portal de Azure Certified for IoT ](https://preview.catalog.azureiotsolutions.com) cuando inicia sesión con una cuenta profesional o educativa de Microsoft o con su identificador de asociado de Microsoft, en caso de que tenga. Después de iniciar sesión, seleccione **Company repository** (Repositorio de la empresa) y, después, **Connection strings** (Cadenas de conexión).

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

[!INCLUDE [iot-pnp-prepare-iot-hub.md](../../includes/iot-pnp-prepare-iot-hub.md)]

## <a name="prepare-the-development-environment"></a>Preparación del entorno de desarrollo

En este inicio rápido, utilizará el administrador de bibliotecas [Vcpkg](https://github.com/microsoft/vcpkg) para instalar el SDK para dispositivos C de Azure IoT en el entorno de desarrollo.

Abra un shell. Ejecute el comando siguiente para instalar Vcpkg:

```bash
cd ~
git clone https://github.com/microsoft/vcpkg
cd vcpkg
./bootstrap-vcpkg.sh
./vcpkg install azure-iot-sdk-c[public-preview,use_prov_client]
```

Esta operación puede tardar varios minutos en completarse.

## <a name="author-your-model"></a>Creación de un modelo

En este inicio rápido, se usa un modelo de funcionalidad del dispositivo de ejemplo existente y las interfaces asociadas.

1. Cree un directorio `pnp_app` en su unidad local. Use esta carpeta para los archivos del modelo de dispositivo y el código auxiliar del código de dispositivo.

    ```bash
    cd ~
    mkdir pnp_app
    ```

1. Descargue el modelo de funcionalidad del dispositivo y los archivos de ejemplo de interfaz en la carpeta `pnp_app`.

    ```bash
    cd pnp_app
    curl -O -L https://raw.githubusercontent.com/Azure/IoTPlugandPlay/master/samples/SampleDevice.capabilitymodel.json
    curl -O -L https://raw.githubusercontent.com/Azure/IoTPlugandPlay/master/samples/EnvironmentalSensor.interface.json
    ```

1. Abra la carpeta `pnp_app` con VS Code. Los archivos se pueden ver con IntelliSense:

    ![Modelo de funcionalidad del dispositivo](media/quickstart-create-pnp-device-linux/dcm.png)

1. En los archivos descargados, reemplace `<YOUR_COMPANY_NAME_HERE>` en los campos `@id` y `schema` por un valor único. Use solo los caracteres a-z, A-Z, 0-9 y el carácter de subrayado. Para más información, consulte [Formato de identificador de gemelo digital](https://github.com/Azure/IoTPlugandPlay/tree/master/DTDL#digital-twin-identifier-format).

## <a name="generate-the-c-code-stub"></a>Generación de código auxiliar de C

Ahora que ya tiene un DCM y sus interfaces asociadas, puede generar el código del dispositivo que implementa el modelo. Para generar el código auxiliar de C en VS Code:

1. Con la `pnp_app`carpeta abierta en VS Code, use **Ctrl+Shift+P** para abrir la paleta de comandos, escriba **IoT Plug and Play** y seleccione **Generar código auxiliar de dispositivo**.

    > [!NOTE]
    > La primera vez que use la utilidad de generación de código de IoT Plug and Play, tardará unos segundos en descargarse e instalarse automáticamente.

1. Elija el archivo de **SampleDevice.capabilitymodel.json** que se usará para generar el código auxiliar del dispositivo.

1. Escriba el nombre del proyecto **sample_device**. Este será el nombre de la aplicación de su dispositivo.

1. Elija **ANSI C** como lenguaje.

1. Elija **Via IoT Hub device connection string** (Mediante la cadena de conexión del dispositivo de IoT Hub) como método de conexión.

1. Elija **CMake Project on Linux** (Proyecto de CMake en Linux) como plantilla de proyecto.

1. Elija **Via Vcpkg** (Mediante Vcpkg) como forma de incluir el SDK del dispositivo.

1. Una nueva carpeta llamada **sample_device** se crea en la misma ubicación que el archivo DCM y en ella se encuentran los archivos de código auxiliar de código de dispositivo generados. VS Code abre una nueva ventana para mostrarlos.
    ![Código del dispositivo](media/quickstart-create-pnp-device-linux/device-code.png)

## <a name="build-and-run-the-code"></a>Compilación y ejecución del código

El código fuente del SDK del dispositivo se usa para compilar el código auxiliar del dispositivo generado. La aplicación que se compila simula un dispositivo que se conecta a un centro de IoT. La aplicación envía datos de telemetría y propiedades, y recibe comandos.

1. Cree una carpeta de compilación de **CMake** para la aplicación **sample_device**:

    ```bash
    cd ~/pnp_app/sample_device
    mkdir cmake
    cd cmake
    ```

1. Ejecute CMake para compilar la aplicación con el SDK. El siguiente comando supone que ha instalado **vcpkg** en la carpeta principal:

    ```bash
    cmake .. -DCMAKE_TOOLCHAIN_FILE=~/vcpkg/scripts/buildsystems/vcpkg.cmake -Duse_prov_client=ON -Dhsm_type_symm_key:BOOL=ON
    cmake --build .
    ```

1. Una vez que la compilación se complete correctamente, ejecute la aplicación y use la cadena de conexión del dispositivo al centro de IoT como parámetro.

    ```sh
    cd ~/pnp_app/sample_device/cmake
    ./sample_device "<YourDeviceConnectionString>"
    ```

1. La aplicación del dispositivo comienza a enviar datos a IoT Hub.

    ![Aplicación del dispositivo en ejecución](media/quickstart-create-pnp-device-linux/device-app-running.png)

## <a name="validate-the-code"></a>Validación del código

### <a name="publish-device-model-files-to-model-repository"></a>Publicación de los archivos del modelo del dispositivo en el repositorio de modelos

Para validar el código del dispositivo con la CLI de **az**, es preciso publicar los archivos en el repositorio de modelos.

1. Con la carpeta `pnp_app` abierta en VS Code, use **Ctrl+Mayús+P** para abrir la paleta de comandos y, a continuación, escriba y seleccione **IoT Plug & Play: Submit files to Model Repository** (IoT Plug and Play: Enviar archivos al repositorio de modelos).

1. Seleccione los archivos `SampleDevice.capabilitymodel.json` y `EnvironmentalSensor.interface.json`.

1. Escriba la cadena de conexión del repositorio de modelos de su empresa.

    > [!NOTE]
    > La cadena de conexión solo es necesaria la primera vez que se conecta al repositorio.

1. Tanto en la ventana de salida de VS Code como en la notificación, puede comprobar que los archivos se han publicado correctamente.

    > [!NOTE]
    > Si aparecen errores al publicar los archivos del modelo de dispositivo, puede probar a usar el comando **IoT Plug and Play: Sign out Model Repository** (IoT Plug and Play: Cerrar sesión del repositorio de modelos) para cerrar la sesión y repasar los pasos realizados.

### <a name="use-the-azure-iot-cli-to-validate-the-code"></a>Uso de la CLI de IoT de Azure para validar el código

Una vez que se inicia el ejemplo de cliente de dispositivo, puede comprobar que funciona con la CLI de Azure.

Use el siguiente comando para ver la telemetría que envía el dispositivo de ejemplo. Es posible que tenga que esperar un minuto o dos antes de ver la telemetría en la salida:

```azurecli-interactive
az iot dt monitor-events --hub-name <YourIoTHubNme> --device-id <YourDeviceID>
```

Use el siguiente comando para ver todas las propiedades que envía el dispositivo:

```azurecli-interactive
az iot dt list-properties --device-id <YourDeviceID> --hub-name <YourIoTHubNme> --source private --repo-login "<YourCompanyModelRepositoryConnectionString>"
```

[!INCLUDE [iot-pnp-clean-resources.md](../../includes/iot-pnp-clean-resources.md)]

## <a name="next-steps"></a>Pasos siguientes

En este inicio rápido ha aprendido a crear un dispositivo IoT Plug and Play mediante un DCM.

Para más información sobre los DCM y cómo crear sus propios modelos, continúe con el tutorial:

> [!div class="nextstepaction"]
> [Tutorial: Creación y prueba de un modelo de funcionalidad del dispositivo mediante Visual Studio Code](tutorial-pnp-visual-studio-code.md)

---
title: Creación de un dispositivo IoT Plug and Play (versión preliminar) que esté preparado para la certificación | Microsoft Docs
description: Los desarrolladores de dispositivos deben aprender a crear un dispositivo IoT Plug and Play (versión preliminar) que esté listo para su certificación.
author: tbhagwat3
ms.author: tanmayb
ms.date: 12/28/2019
ms.topic: tutorial
ms.custom: mvc
ms.service: iot-pnp
services: iot-pnp
manager: philmea
ms.openlocfilehash: e97aa07d2a43a03805fd881c674157ee676c37b4
ms.sourcegitcommit: 253d4c7ab41e4eb11cd9995190cd5536fcec5a3c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/25/2020
ms.locfileid: "80239902"
---
# <a name="build-an-iot-plug-and-play-preview-device-thats-ready-for-certification"></a>Creación de un dispositivo IoT Plug and Play (versión preliminar) que esté listo para la certificación

En este tutorial se describe la forma en que los desarrolladores de dispositivos pueden compilar un dispositivo IoT Plug and Play (versión preliminar) que esté listo para su certificación.

Las pruebas de certificación comprueban que:

- El código del dispositivo IoT Plug and Play está instalado en el dispositivo.
- El código del dispositivo IoT Plug and Play se ha creado con el SDK de Azure IoT.
- El código del dispositivo admite [Azure IoT Hub Device Provisioning Service](../iot-dps/about-iot-dps.md).
- El código del dispositivo implementa la interfaz de información del dispositivo.
- El modelo de funcionalidad y el código de dispositivo funcionan con IoT Central.

## <a name="prerequisites"></a>Prerrequisitos

Para completar este tutorial, necesita:

- [Visual Studio Code](https://code.visualstudio.com/download)
- Paquete de extensiones [Azure IoT Tools para VS Code](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-tools)

También debe completar el artículo [Inicio rápido: Uso de un modelo de funcionalidad de dispositivo para crear un dispositivo de versión preliminar de IoT Plug and Play (Windows)](quickstart-create-pnp-device-windows.md). En el inicio rápido se muestra cómo configurar el entorno de desarrollo mediante Vcpkg y cómo crear un proyecto de ejemplo.

## <a name="store-a-capability-model-and-interfaces"></a>Almacenamiento de un modelo de funcionalidad y las interfaces

En el caso de los dispositivos de IoT Plug and Play, debe crear un modelo de funcionalidad e interfaces que definan las funcionalidades del dispositivo en forma de archivos JSON.

Estos archivos se pueden almacenar en tres ubicaciones diferentes:

- En el repositorio de modelos público.
- En el repositorio de modelos de la empresa.
- En su dispositivo.

Actualmente, para certificar el dispositivo, los archivos deben almacenarse en el repositorio de modelos de la empresa o en el repositorio de modelos público.

## <a name="include-the-required-interfaces"></a>Inclusión de las interfaces necesarias

Para pasar el proceso de certificación, es preciso incluir e implementar la interfaz de **información del dispositivo** en el modelo de funcionalidad. Esta interfaz tiene la siguiente identificación:

```json
"@id": "urn:azureiot:DeviceManagement:DeviceInformation:1"
```

> [!NOTE]
> Si ha completado el [Inicio rápido: Uso de un modelo de funcionalidad para crear un dispositivo](quickstart-create-pnp-device-windows.md), ya ha incluido la interfaz de **información del dispositivo** en el modelo.

Para incluir la interfaz de **información del dispositivo** en el modelo de dispositivo, agregue el identificador de la interfaz a la propiedad `implements` del modelo de capacidad:

```json
{
  "@id": "urn:yourcompanyname:sample:ThermostatDevice:1",
  "@type": "CapabilityModel",
  "displayName": "Thermostat T-1000",
  "implements": [
    "urn:yourcompanyname:sample:Thermostat:1",
    "urn:azureiot:DeviceManagement:DeviceInformation:1"
  ],
  "@context": "http://azureiot.com/v1/contexts/IoTModel.json"
}
```

Para ver la interfaz de **información del dispositivo** en VS Code:

1. Use **Ctrl+Mayús+P** para abrir la paleta de comandos.

1. Escriba **Plug and Play** y seleccione el comando **IoT Plug and Play Open Model Repository**. Elija **Open Public Model Repository** (Abrir repositorio de modelos público). El repositorio de modelos público se abre en VS Code.

1. En el repositorio de modelos público, seleccione la pestaña **Interfaces**, seleccione el icono de filtro y escriba **información del dispositivo** en el campo de filtro.

1. Para crear una copia local de la interfaz de **información del dispositivo**, selecciónela la lista filtrada y, después, seleccione **Download** (Descargar). VS Code muestra el archivo de la interfaz.

Para ver la interfaz de **información del dispositivo** en la CLI de Azure:

1. [Instale la extensión de la CLI de Azure IoT](howto-install-pnp-cli.md).

1. Use el siguiente comando de la CLI de Azure para mostrar una interfaz con el identificador de la interfaz de información del dispositivo:

    ```azurecli
    az iot pnp interface show --interface urn:azureiot:DeviceManagement:DeviceInformation:1
    ```

Para más información, consulte [Instalación y uso de la extensión de Azure IoT para la CLI de Azure](howto-install-pnp-cli.md).

## <a name="update-device-code"></a>Actualización del código de dispositivo

### <a name="enable-device-provisioning-through-the-azure-iot-device-provisioning-service-dps"></a>Habilitación del aprovisionamiento de dispositivos mediante Azure IoT Device Provisioning Service (DPS)

Para certificar el dispositivo, debe habilitar el aprovisionamiento mediante [Azure IoT Device Provisioning Service (DPS)](https://docs.microsoft.com/azure/iot-dps/about-iot-dps). Para agregar la capacidad de usar DPS, puede generar el código auxiliar de C en VS Code. Siga estos pasos:

1. Abra la carpeta con los archivos de DCM en VS Code, use **Ctrl+Mayús+P** para abrir la paleta de comandos, escriba **, IoT Plug and Play** y seleccione **Generate Device Code Stub** (Generar código auxiliar de dispositivo).

1. Elija el archivo de DCM que quiere usar para generar el código auxiliar del dispositivo.

1. Escriba el nombre del proyecto, por ejemplo, **dispositivo_de_ejemplo**. Este es el nombre de la aplicación del dispositivo.

1. Elija **ANSI C** como lenguaje.

1. Elija **Via DPS (Device Provisioning Service) symmetric key** (Mediante clave simétrica de DPS [Device Provisioning Service]) como método de conexión.

1. Elija **proyecto CMake en Windows** como plantilla de proyecto.

1. Elija **Via Vcpkg** (Mediante Vcpkg) como forma de incluir el SDK del dispositivo.

1. VS Code abre una nueva ventana con los archivos del código auxiliar del dispositivo generados.

## <a name="build-and-run-the-code"></a>Compilación y ejecución del código

El paquete Vcpkg se usa para compilar el código auxiliar del dispositivo generado. La aplicación que se compila simula un dispositivo que se conecta a un centro de IoT. La aplicación envía datos de telemetría y propiedades, y recibe comandos.

1. Cree un subdirectorio `cmake` en la carpeta `sample_device` y vaya a esa carpeta:

    ```cmd
    mkdir cmake
    cd cmake
    ```

1. Ejecute los siguientes comandos para crear el código auxiliar de código generado (reemplazando el marcador de posición por el directorio de su repositorio Vcpkg):

    ```cmd
    cmake .. -G "Visual Studio 16 2019" -A Win32 -Duse_prov_client=ON -Dhsm_type_symm_key:BOOL=ON -DCMAKE_TOOLCHAIN_FILE="<directory of your Vcpkg repo>\scripts\buildsystems\vcpkg.cmake"

    cmake --build .
    ```
    
    > [!NOTE]
    > Si usa Visual Studio 2017 o 2015, deberá especificar el generador de CMake en función de las herramientas de compilación que esté utilizando:
    >```cmd
    ># Either
    >cmake .. -G "Visual Studio 15 2017" -Duse_prov_client=ON -Dhsm_type_symm_key:BOOL=ON -DCMAKE_TOOLCHAIN_FILE="{directory of your Vcpkg repo}\scripts\buildsystems\vcpkg.cmake"
    ># or
    >cmake .. -G "Visual Studio 14 2015" -Duse_prov_client=ON -Dhsm_type_symm_key:BOOL=ON -DCMAKE_TOOLCHAIN_FILE="{directory of your Vcpkg repo}\scripts\buildsystems\vcpkg.cmake"
    >```

    > [!NOTE]
    > Si cmake no encuentra el compilador de C++, aparecerán errores de compilación al ejecutar el comando anterior. En ese caso, pruebe a ejecutar el comando en el [símbolo del sistema de Visual Studio](https://docs.microsoft.com/dotnet/framework/tools/developer-command-prompt-for-vs).

1. Una vez finalizada la compilación correctamente, escriba las credenciales de DPS (**DPS ID Scope** [Ámbito de id. de DPS], **DPS Symmetric Key** (Clave simétrica de DPS), **Device Id** [id. de dispositivo]) como parámetros para la aplicación. Para obtener las credenciales desde el portal de certificación, consulte [Conexión y prueba del dispositivo de IoT Plug and Play](tutorial-certification-test.md#connect-and-discover-interfaces).

    ```cmd\sh
    .\Debug\sample_device.exe [Device ID] [DPS ID Scope] [DPS symmetric key]
    ```

### <a name="implement-standard-interfaces"></a>Implementación de interfaces estándar

#### <a name="implement-the-model-information-and-sdk-information-interfaces"></a>Implementación de las interfaces de información del modelo e información del SDK

El SDK de dispositivo IoT de Azure implementa las interfaces Model Information (Información del modelo) y SDK Information (Información del SDK). Si usa la función de generación de código en VS Code, el código de dispositivo utiliza el SDK de dispositivo de IoT Plug and Play.

Si decide no usar el SDK de dispositivo IoT de Azure, puede usar el código fuente del SDK como referencia para su propia implementación.

#### <a name="implement-the-device-information-interface"></a>Implementación de la interfaz de información del dispositivo

Implemente **la interfaz de** información del dispositivo en el propio dispositivo y proporcione información específica del dispositivo en el tiempo de ejecución.

Puede usar una implementación de ejemplo de la interfaz de **información del dispositivo** para [Linux](https://github.com/Azure/azure-iot-sdk-c/tree/public-preview) como referencia.

### <a name="implement-all-the-capabilities-defined-in-your-model"></a>Implementación de todas las funcionalidades definidas en el modelo

Durante la certificación, el dispositivo se prueba mediante programación para asegurarse de que implementa las funcionalidades definidas en sus interfaces. Use el código de estado HTTP 501 para responder a las solicitudes de comandos y propiedades de lectura y escritura si el dispositivo no las implementa.

## <a name="next-steps"></a>Pasos siguientes

Una vez que ha creado un dispositivo IoT Plug and Play listo para su certificación, el siguiente paso es:

> [!div class="nextstepaction"]
> [Aprender a certificar dispositivos](tutorial-certification-test.md)

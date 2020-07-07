---
title: Use Visual Studio y Visual Studio Code para compilar dispositivos de la versión preliminar de IoT Plug and Play | Microsoft Docs
description: Use Visual Studio y Visual Studio Code para acelerar la creación de modelos de dispositivos IoT Plug and Play y la implementación del código de dispositivo.
author: liydu
ms.author: liydu
ms.date: 12/26/2019
ms.topic: how-to
ms.service: iot-pnp
services: iot-pnp
ms.custom: mvc
ms.openlocfilehash: 038d9ff39f388d1ef7b09b951c09dbe3420858b7
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "84298234"
---
# <a name="use-visual-studio-and-visual-studio-code-to-build-iot-plug-and-play-devices"></a>Uso de Visual Studio y Visual Studio Code para compilar dispositivos de IoT Plug and Play

Azure IoT Tools par Visual Studio Code proporciona un entorno integrado para crear interfaces y modelos de funcionalidad del dispositivo (DCM), publicar en repositorios de modelos y generar código C de esqueleto para implementar la aplicación de dispositivo.

En este artículo aprenderá a:

- Generar código de dispositivo y el proyecto de aplicación.
- Usar el código generado en el proyecto de dispositivo.
- Iterar volviendo a generar el código de esqueleto.

Para más información sobre cómo usar VS Code para desarrollar dispositivos IoT, vea [https://github.com/microsoft/vscode-iot-workbench](https://github.com/microsoft/vscode-iot-workbench).

## <a name="prerequisites"></a>Prerrequisitos

Instale [Visual Studio Code](https://code.visualstudio.com/).

Siga estos pasos para instalar el paquete de extensiones en VS Code.

1. En VS Code, seleccione la pestaña **Extensiones**.
1. Busque **Azure IoT Tools** desde Marketplace e instálelo.

## <a name="generate-device-code-and-project"></a>Generación del código de dispositivo y del proyecto

En VS Code, use **Ctrl+Mayús+P** para abrir la paleta de comandos, escriba **IoT Plug and Play** y seleccione **Generate Device Code Stub** (Generar código stub de dispositivo) para configurar el código de esqueleto y el tipo de proyecto. Aquí se describe cada paso de forma pormenorizada:

- **Archivo de DCM que se va a usar para generar el código**. Para generar el código del dispositivo de esqueleto, abra la carpeta que contenga los archivos de DCM y de interfaz que implementa. Si el generador de código no encuentra una interfaz que un DCM necesite, la descarga del repositorio de modelos según proceda.

- **Lenguaje del código del dispositivo**. Actualmente, el generador de código solo admite ANSI C.

- **Nombre del proyecto**. El nombre del proyecto se usa como el nombre de la carpeta en el código generado y en otros archivos del proyecto. La carpeta se coloca de forma predeterminada junto al archivo de DCM. Para no tener que copiar manualmente la carpeta de código generado cada vez que el DCM se actualice y se vuelva a generar el código del dispositivo, conserve el archivo de DCM en la misma carpeta que la carpeta del proyecto.

- **Método para conectarse a Azure IoT**. Los archivos generados también contienen código para configurar el dispositivo para que se conecte a Azure IoT Hub. Puede elegir entre conectarse directamente a [Azure IoT Hub](https://docs.microsoft.com/azure/iot-hub) o usar [Device Provisioning Service](https://docs.microsoft.com/azure/iot-dps).

    - **A través de una cadena de conexión de dispositivo de IoT Hub** : especifique la cadena de conexión de dispositivo para que la aplicación de dispositivo se conecte a IoT Hub directamente.
    - **A través de la clave simétrica de DPS**: especifique el **identificador de ámbito**, la **clave simétrica** y el **identificador de dispositivo** de la aplicación de dispositivo, que son necesarios para conectarse a IoT Hub o IoT Central con DPS.

- **Tipo de proyecto**. El generador de código también genera un proyecto CMake o Arduino. Actualmente, los tipos de proyecto admitidos son estos:

    - **Proyecto de CMake en Windows**: pensado para un proyecto de dispositivo en el que se usa [CMake](https://cmake.org/) como sistema de compilación en Windows. Esta opción genera `CMakeLists.txt` con las configuraciones del SDK de dispositivo en la misma carpeta que el código de C.
    - **Proyecto de CMake en Linux**: pensado para un proyecto de dispositivo en el que se usa [CMake](https://cmake.org/) como sistema de compilación en Linux. Esta opción genera `CMakeLists.txt` con las configuraciones del SDK de dispositivo en la misma carpeta que el código de C.
    - **Proyecto de MXChip IoT DevKit**: pensado para un proyecto de dispositivo que se ejecuta en un dispositivo [MXChip IoT DevKit](https://aka.ms/iot-devkit). Esta opción genera un proyecto de Arduino que se puede [usar en VS Code](https://docs.microsoft.com/azure/iot-hub/iot-hub-arduino-iot-devkit-az3166-get-started) o en el IDE de Arduino para compilarse y ejecutarse en un dispositivo DevKit de IoT.

- **Tipo de SDK de dispositivos**. Si selecciona CMake como tipo de proyecto, este es el paso para configurar el modo en que el código generado incluirá el SDK de dispositivo IoT de Azure para C en `CMakeLists.txt`:

    - **A través del código fuente**: el código generado se basa en el [código fuente del SDK de dispositivo ](https://github.com/Azure/azure-iot-sdk-c) para incluirlo y compilarlo junto a él. Esto se recomienda cuando se ha personalizado el código fuente del SDK de dispositivo.
    - **A través de Vcpkg**: el código generado se basa en el [Vcpkg del SDK de dispositivo](https://github.com/microsoft/vcpkg/tree/master/ports/azure-iot-sdk-c) para incluirlo y compilarlo junto a él. Esta es la manera recomendada para los dispositivos que ejecutan Windows, Linux o macOS.

    > [!NOTE]
    > Estamos trabajando en la compatibilidad de macOS con el Vcpkg del SDK de dispositivo IoT de Azure para C.

El generador de código intenta utilizar los archivos de DCM y de interfaz que se encuentran en la carpeta local. Si los archivos de interfaz no están en la carpeta local, el generador de código los busca en el repositorio de modelos público o en el repositorio de modelos de la empresa. Los [archivos de interfaz comunes](./concepts-common-interfaces.md) se almacenan en el repositorio de modelos público.

Una vez finalizada la generación de código, la extensión abre una nueva ventana de VS Code con el código. Si abre un archivo generado, como **main.c**, es posible que IntelliSense le notifique que no puede abrir los archivos de origen del SDK de C. Para permitir una navegación de código e IntelliSense correcta, siga estos pasos para incluir el origen del SDK de C:

1. En VS Code, use **Ctrl+Mayús+P** para abrir la paleta de comandos, escriba y seleccione **C/C++: Edit Configurations (JSON)** [C/C++: Editar configuraciones (JSON)] para abrir el archivo **c_cpp_properties.json**.

1. Agregue la ruta de acceso del SDK del dispositivo a la sección `includePath`:

    ```json
    "includePath": [
        "${workspaceFolder}/**",
        "{path_of_device_c_sdk}/**"
    ]
    ```

1. Guarde el archivo.

## <a name="use-the-generated-code"></a>Uso del código generado

Las siguientes instrucciones describen cómo usar el código generado en su propio proyecto de dispositivo en distintas plataformas de equipo de desarrollo. En estas instrucciones se da por hecho que está usando una cadena de conexión de dispositivo IoT Hub con el código generado:

### <a name="linux"></a>Linux

Para compilar el código de dispositivo junto con el SDK de dispositivo para C mediante CMake en un entorno de Linux, como Ubuntu o Debian:

1. Abra una aplicación de terminal.

1. Instale **GCC**, **Git**, `cmake` y todas las dependencias con el comando `apt-get`:

    ```bash
    sudo apt-get update
    sudo apt-get install -y git cmake build-essential curl libcurl4-openssl-dev libssl-dev uuid-dev
    ```

    Compruebe que la versión de `cmake` es posterior a **2.8.12** y que la versión de **GCC** es posterior a **4.4.7**.

    ```bash
    cmake --version
    gcc --version
    ```

1. Instalación de Vcpkg:

    ```bash
    git clone https://github.com/Microsoft/vcpkg.git
    cd vcpkg

    ./bootstrap-vcpkg.sh
    ```

    A continuación, para conectar la [integración](https://github.com/microsoft/vcpkg/blob/master/docs/users/integration.md) para los usuarios, ejecute lo siguiente:

    ```bash
    ./vcpkg integrate install
    ```

1. Instale el Vcpkg del SDK de dispositivo IoT de Azure para C:

    ```bash
    ./vcpkg install azure-iot-sdk-c[public-preview,use_prov_client]
    ```

1. Cree un subdirectorio de `cmake` en la carpeta que contenga el código auxiliar generado y navegue hasta esa carpeta:

    ```bash
    mkdir cmake
    cd cmake
    ```

1. Ejecute los siguientes comandos para usar CMake para compilar el SDK del dispositivo y el código stub generado:

    ```bash
    cmake .. -Duse_prov_client=ON -Dhsm_type_symm_key:BOOL=ON -DCMAKE_TOOLCHAIN_FILE="{directory of your Vcpkg repo}/scripts/buildsystems/vcpkg.cmake"

    cmake --build .
    ```

1. Después de que la compilación finalice correctamente, ejecute la aplicación, especificando la cadena de conexión del dispositivo de IoT Hub como un parámetro.

    ```bash
    ./{generated_code_project_name} "[IoT Hub device connection string]"
    ```

### <a name="windows"></a>Windows

Para compilar el código de dispositivo junto con el SDK de C del dispositivo en Windows usando CMake y los compiladores de Visual Studio C/C++ en la línea de comandos, vea el [inicio rápido de IoT Plug and Play](./quickstart-create-pnp-device-windows.md). En los siguientes pasos se muestra cómo compilar el código de dispositivo junto con el Vcpkg del SDK de dispositivo para C como un proyecto de CMake en Visual Studio.

1. Siga los pasos descritos del [inicio rápido](https://docs.microsoft.com/azure/iot-pnp/quickstart-create-pnp-device-windows#prepare-the-development-environment) para instalar el SDK de dispositivo IoT de Azure para C a través de Vcpkg.

1. Instale [Visual Studio 2019 (Community, Professional o Enterprise)](https://visualstudio.microsoft.com/downloads/), asegurándose de incluir el componente **Administrador de paquetes NuGet** y la carga de trabajo **Desarrollo para el escritorio con C++** .

1. Abra Visual Studio, elija **Archivo > Abrir > CMake...** para abrir el archivo `CMakeLists.txt` en la carpeta que contiene el código generado.

1. En la barra de herramientas **General**, busque el menú desplegable **Configuraciones**. Seleccione **Administrar configuración** para agregar la configuración de CMake del proyecto.

    ![Administrar configuración](media/howto-develop-with-vs-vscode/vs-manage-config.png)

1. En la **configuración de CMake**, agregue una nueva configuración y seleccione **x86-Debug** como destino.

1. En **Argumentos de comandos de CMake**, agregue la siguiente línea:

    ```txt
    -Duse_prov_client=ON -Dhsm_type_symm_key:BOOL=ON
    ```

1. Guarde el archivo.

1. Cambie a **x86-Debug** en la lista desplegable **Configuraciones**. Necesita esperar unos segundos para que CMake genere la memoria caché. Vea la ventana de salida para comprobar el progreso.

    ![Salida de CMake](media/howto-develop-with-vs-vscode/vs-cmake-output.png)

1. En el **Explorador de soluciones**, haga clic con el botón derecho en el archivo `CMakeLists.txt` en la carpeta raíz y seleccione **Compilar** en el menú contextual para compilar el SDK de dispositivo y el código stub generado.

1. Una vez que la compilación finaliza correctamente, ejecute la aplicación en el símbolo del sistema, especificando la cadena de conexión del dispositivo IoT Hub como un parámetro.

    ```cmd
    .\out\build\x86-Debug\{generated_code_project_name}.exe "[IoT Hub device connection string]"
    ```

> [!TIP]
> Para más información sobre cómo usar CMake en Visual Studio, vea [Compilar proyectos de CMake](https://docs.microsoft.com/cpp/build/cmake-projects-in-visual-studio?view=vs-2019#building-cmake-projects).

### <a name="macos"></a>macOS

En los siguientes pasos se muestra cómo compilar el código de dispositivo junto con el código fuente del SDK de dispositivo para C en macOS mediante CMake:

1. Abra una aplicación de terminal.

1. Use [Homebrew](https://brew.sh) para instalar todas las dependencias:

    ```bash
    brew update
    brew install git cmake pkgconfig openssl ossp-uuid
    ```

1. Compruebe que la versión de [CMake](https://cmake.org/) es como mínimo **2.8.12**:

    ```bash
    cmake --version
    ```

1. [Actualice la CURL](https://github.com/Azure/azure-iot-sdk-c/blob/master/doc/devbox_setup.md#upgrade-curl-on-mac-os) a la versión más reciente disponible.

1. En la carpeta que contiene el código generado, clone el repositorio del [SDK de dispositivo IoT de Azure para C](https://github.com/Azure/azure-iot-sdk-c):

    ```bash
    git clone https://github.com/Azure/azure-iot-sdk-c --recursive -b public-preview
    ```

    Esta operación puede tardar varios minutos en completarse.

1. Cree una carpeta llamada `cmake` en la carpeta que contiene el código generado y navegue a esa carpeta.

    ```bash
    mkdir cmake
    cd cmake
    ```

1. Ejecute los siguientes comandos para usar CMake para compilar el SDK del dispositivo y el código stub generado:

    ```bash
    cmake -Duse_prov_client=ON -Dhsm_type_symm_key:BOOL=ON -DOPENSSL_ROOT_DIR:PATH=/usr/local/opt/openssl ..
    cmake --build .
    ```

1. Después de que la compilación finalice correctamente, ejecute la aplicación, especificando la cadena de conexión del dispositivo de IoT Hub como un parámetro.

    ```bash
    cd {generated_code_folder_name}/cmake/
    ./{generated_code_project_name} "[IoT Hub device connection string]"
    ```

## <a name="iterate-by-regenerating-the-skeleton-code"></a>Iteración volviendo a generar el código de esqueleto

El generador de código puede volver a generar el código si actualiza los archivos de DCM y de interfaz. Suponiendo que ya ha generado el código de dispositivo a partir de un archivo de DCM, haga lo siguiente para volver a generarlo:

1. Con la carpeta con los archivos de DCM abierta, use **Ctrl+Mayús+P** para abrir la paleta de comandos, escriba **, IoT Plug and Play** y seleccione **Generate Device Code Stub** (Generar código auxiliar de dispositivo).

1. Elija el archivo de DCM que ha actualizado.

1. Seleccione **Re-generate code for {project name}** (Volver a generar código para {nombre del proyecto}).

1. El generador de código utiliza la configuración que se estableció anteriormente y vuelve a generar el código, pero no sobrescribe los archivos que pueden contener código de usuario, como `main.c` y `{project_name}_impl.c`.

> [!NOTE]
> Si actualiza el identificador de URN en el archivo de interfaz, se tratará como una interfaz nueva. Al volver a generar el código, el generador de código genera código para la interfaz, pero no sobrescribe el original en el archivo `{project_name}_impl.c`.

## <a name="problems-and-feedback"></a>Problemas y comentarios

Azure IoT Tools es un proyecto de código abierto en GitHub. En caso de que se produzca alguna incidencia o quiera solicitar una característica, puede [crear una incidencia en GitHub](https://github.com/microsoft/vscode-azure-iot-tools/issues/new).

## <a name="next-steps"></a>Pasos siguientes

En este artículo de procedimientos, ha aprendido a usar Visual Studio y Visual Studio Code para generar código de esqueleto C e implementar la aplicación de dispositivo. El siguiente paso sugerido es obtener información sobre cómo [instalar y usar la herramienta de explorador de Azure IoT](./howto-install-iot-explorer.md).

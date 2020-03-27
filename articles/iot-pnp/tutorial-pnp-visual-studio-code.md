---
title: Creación y prueba de un dispositivo IoT Plug and Play (versión preliminar) | Microsoft Docs
description: Como desarrollador de dispositivos, aprenda a usar VS Code para crear y probar un nuevo modelo de funcionalidad para un dispositivo IoT Plug and Play (versión preliminar).
author: dominicbetts
ms.author: dobett
ms.date: 12/30/2019
ms.topic: tutorial
ms.custom: mvc
ms.service: iot-pnp
services: iot-pnp
manager: philmea
ms.openlocfilehash: 720b3e56e1dd45bd2940b337adefa6ebdaa2e5a1
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/24/2020
ms.locfileid: "76719728"
---
# <a name="tutorial-create-and-test-a-device-capability-model-using-visual-studio-code"></a>Tutorial: Creación y prueba de un modelo de funcionalidad del dispositivo mediante Visual Studio Code

En este tutorial se muestra cómo usar Visual Studio Code, en cuanto desarrollador de dispositivos, para crear un _modelo de funcionalidad del dispositivo_. Se puede usar el modelo para generar el código básico y que se ejecute en un dispositivo que se conecta a una instancia de Azure IoT Hub en la nube.

En la sección de este tutorial en la que se describe cómo crear el código básico generado se supone que usa Windows.

En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Crear un modelo de funcionalidad del dispositivo
> * Generar el código básico del dispositivo a partir del modelo
> * Implementar los códigos auxiliares en el código generado
> * Ejecutar el código para probar las interacciones con un centro de IoT

## <a name="prerequisites"></a>Prerrequisitos

Para trabajar con el modelo de funcionalidad del dispositivo en este tutorial, necesita lo siguiente:

* [Visual Studio Code](https://code.visualstudio.com/download): VS Code está disponible para varias plataformas
* Paquete de extensiones [Azure IoT Tools para VS Code](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-tools). Siga estos pasos para instalar el paquete de extensiones en VS Code:

    1. En VS Code, seleccione la pestaña **Extensions** (Extensiones).
    1. Busque **Azure IoT Tools**.
    1. Seleccione **Instalar**.

Para compilar el código C generado en Windows en este tutorial, necesitará lo siguiente:

* [Herramientas de compilación para Visual Studio](https://visualstudio.microsoft.com/thank-you-downloading-visual-studio/?sku=BuildTools&rel=16) con las cargas de trabajo de **herramientas de compilación de C++** y el **componente Administrador de paquetes NuGet**. O si ya tiene [Visual Studio (Community, Professional o Enterprise)](https://visualstudio.microsoft.com/downloads/) 2019, 2017 o 2015 con las mismas cargas de trabajo instaladas.
* [Git](https://git-scm.com/download)
* [CMake](https://cmake.org/download/)

Para probar el código del dispositivo en este tutorial, necesitará lo siguiente:

* [Azure IoT Explorer](https://github.com/Azure/azure-iot-explorer/releases).
* Suscripción a Azure. Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="model-your-device"></a>Modelado del dispositivo

Para crear un modelo de funcionalidad del dispositivo, se usará el _lenguaje de definición de gemelos digitales_. Un modelo normalmente se compone de varios archivos de definición de _interfaz_ y un único archivo de modelo. **Azure IoT Tools para VS Code** incluye herramientas que le ayudan a crear y editar estos archivos JSON.

### <a name="create-the-interface-file"></a>Creación del archivo de interfaz

Para crear un archivo de interfaz que defina las funcionalidades del dispositivo IoT en VS Code, siga estos pasos:

1. Cree una carpeta llamada **devicemodel**.

1. Inicie VS Code y use **Ctrl+Mayús+P** para abrir la paleta de comandos.

1. Escriba **Plug and Play** y, luego, seleccione el comando **IoT Plug & Play: Create Interface** (Crear interfaz).

1. Busque y seleccione la carpeta **devicemodel** que ha creado.

1. Luego, escriba **EnvironmentalSensor** como el nombre de la interfaz y presione **Entrar**. VS Code crea un archivo de interfaz de ejemplo denominado **EnvironmentalSensor.interface.json**.

1. Reemplace el contenido de este archivo por el siguiente JSON y reemplace `{your name}` en el campo `@id` por un valor único. Use solo los caracteres a-z, A-Z, 0-9 y el carácter de subrayado. Para más información, consulte [Formato de identificador de gemelo digital](https://github.com/Azure/IoTPlugandPlay/tree/master/DTDL#digital-twin-identifier-format). El identificador de interfaz debe ser único para guardar la interfaz en el repositorio:

    ```json
    {
      "@id": "urn:{your name}:EnvironmentalSensor:1",
      "@type": "Interface",
      "displayName": "Environmental Sensor",
      "description": "Provides functionality to report temperature, humidity. Provides telemetry, commands and read-write properties",
      "comment": "Requires temperature and humidity sensors.",
      "contents": [
        {
          "@type": "Property",
          "displayName": "Device State",
          "description": "The state of the device. Two states online/offline are available.",
          "name": "state",
          "schema": "boolean"
        },
        {
          "@type": "Property",
          "displayName": "Customer Name",
          "description": "The name of the customer currently operating the device.",
          "name": "name",
          "schema": "string",
          "writable": true
        },
        {
          "@type": "Property",
          "displayName": "Brightness Level",
          "description": "The brightness level for the light on the device. Can be specified as 1 (high), 2 (medium), 3 (low)",
          "name": "brightness",
          "writable": true,
          "schema": "long"
        },
        {
          "@type": [
            "Telemetry",
            "SemanticType/Temperature"
          ],
          "description": "Current temperature on the device",
          "displayName": "Temperature",
          "name": "temp",
          "schema": "double",
          "unit": "Units/Temperature/fahrenheit"
        },
        {
          "@type": [
            "Telemetry",
            "SemanticType/Humidity"
          ],
          "description": "Current humidity on the device",
          "displayName": "Humidity",
          "name": "humid",
          "schema": "double",
          "unit": "Units/Humidity/percent"
        },
        {
          "@type": "Telemetry",
          "name": "magnetometer",
          "displayName": "Magnetometer",
          "comment": "This shows a complex telemetry that contains a magnetometer reading.",
          "schema": {
            "@type": "Object",
            "fields": [
              {
                "name": "x",
                "schema": "integer"
              },
              {
                "name": "y",
                "schema": "integer"
              },
              {
                "name": "z",
                "schema": "integer"
              }
            ]
          }
        },
        {
          "@type": "Command",
          "name": "turnon",
          "response": {
            "name": "turnon",
            "schema": "string"
          },
          "comment": "This Commands will turn-on the LED light on the device.",
          "commandType": "synchronous"
        },
        {
          "@type": "Command",
          "name": "turnoff",
          "comment": "This Commands will turn-off the LED light on the device.",
          "response": {
            "name": "turnoff",
            "schema": "string"
          },
          "commandType": "synchronous"
        }
      ],
      "@context": "http://azureiot.com/v1/contexts/IoTModel.json"
    }
    ```

    Esta interfaz define las propiedades del dispositivo, como el **nombre del cliente**, los tipos de telemetría, como la **temperatura**, y comandos como **turnon**.

1. Agregue una funcionalidad de comando llamada **blink** al final de este archivo de interfaz. Asegúrese de agregar una coma antes de agregar el comando. Pruebe a escribir la definición para ver cómo IntelliSense, la función autocompletar y la validación pueden ayudarle a editar una definición de interfaz:

    ```json
    {
      "@type": "Command",
      "description": "This command will begin blinking the LED for given time interval.",
      "name": "blink",
      "request": {
        "name": "blinkRequest",
        "schema": {
          "@type": "Object",
          "fields": [
            {
              "name": "interval",
              "schema": "long"
            }
          ]
        }
      },
      "response": {
        "name": "blinkResponse",
        "schema": "string"
      },
      "commandType": "synchronous"
    }
    ```

1. Guarde el archivo.

### <a name="create-the-model-file"></a>Creación del archivo de modelo

El archivo de modelo especifica las interfaces que implementa el dispositivo IoT Plug and Play. Normalmente hay al menos dos interfaces en un modelo: una o más que definen las funcionalidades específicas del dispositivo, y una interfaz estándar que deben implementar todos los dispositivos IoT Plug and Play.

Para crear un archivo de modelo que especifique las interfaces que implementa el dispositivo IoT Plug and Play en VS Code, siga estos pasos:

1. Use **Ctrl+Mayús+P** para abrir la paleta de comandos.

1. Escriba **Plug and Play** y, luego, seleccione el comando **IoT Plug & Play: Create Capability Model** (Crear modelo de funcionalidad). Luego, escriba **SensorboxModel** como nombre del modelo. VS Code crea un archivo de interfaz de ejemplo llamado **SensorboxModel.capabilitymodel**.

1. Reemplace el contenido de este archivo por el siguiente JSON y reemplace `{your name}` en el campo `@id` y en la interfaz `EnvironmentalSensor` por el mismo valor que usó en el archivo **EnvironmentalSensor.interface.json**. El identificador de interfaz debe ser único para guardar la interfaz en el repositorio:

    ```json
    {
      "@id": "urn:{your name}:SensorboxModel:1",
      "@type": "CapabilityModel",
      "displayName": "Environmental Sensorbox Model",
      "implements": [
        {
          "schema": "urn:{your name}:EnvironmentalSensor:1",
          "name": "environmentalSensor"
        },
        {
          "schema": "urn:azureiot:DeviceManagement:DeviceInformation:1",
          "name": "deviceinfo"
        }
      ],
      "@context": "http://azureiot.com/v1/contexts/IoTModel.json"
    }
    ```

    El modelo define un dispositivo que implementa la interfaz **EnvironmentalSensor** y la interfaz **DeviceInformation** estándar.

1. Guarde el archivo.

### <a name="download-the-deviceinformation-interface"></a>Descarga de la interfaz DeviceInformation

Antes de poder generar código básico a partir del modelo, debe crear una copia local de **DeviceInformation** desde el *repositorio de modelos públicos*. El repositorio de modelos públicos ya contiene la interfaz **DeviceInformation**.

Para descargarla mediante VS Code, siga estos pasos:

1. Use **Ctrl+Mayús+P** para abrir la paleta de comandos.

1. Escriba **Plug and Play**, seleccione el comando **Open Model Repository** (Abrir el repositorio de modelos) y, luego, **Open Public Model Repository** (Abrir el repositorio de modelos públicos).

1. Seleccione sucesivamente **Interfaces** (Interfaces), la interfaz de información de dispositivo con el identificador `urn:azureiot:DeviceManagement:DeviceInformation:1` y, luego, **Download** (Descargar).

Ahora tiene los tres archivos que componen el modelo de funcionalidad del dispositivo:

* urn_azureiot_DeviceManagement_DeviceInformation_1.interface.json
* EnvironmentalSensor.interface.json
* SensorboxModel.capabilitymodel.json

## <a name="publish-the-model"></a>Publicar el modelo

Para que la herramienta Azure IoT Explorer lea el modelo de funcionalidad del dispositivo, debe publicarlo en el repositorio de la empresa. Para publicarlo desde VS Code, necesita la cadena de conexión del repositorio de la empresa:

1. Vaya al [portal de Azure Certified for IoT](https://aka.ms/ACFI).

1. Use su _cuenta profesional_ de Microsoft para iniciar sesión en el portal.

1. Seleccione **Company repository** (Repositorio de la empresa) y, luego, **Connection strings** (Cadenas de conexión).

1. Copie la cadena de conexión.

Para abrir el repositorio de la empresa en VS Code, siga estos pasos:

1. Use **Ctrl+Mayús+P** para abrir la paleta de comandos.

1. Escriba **Plug and Play** y, luego, seleccione el comando **IoT Plug & Play: Open Model Repository** (Abrir el repositorio de modelos).

1. Seleccione **Open Organizational Model Repository** (Abrir el repositorio de modelos organizativos) y pegue la cadena de conexión.

1. Presione **Entrar** para abrir el repositorio de la empresa.

Para publicar el modelo de funcionalidad del dispositivo y las interfaces en el repositorio de la empresa, siga estos pasos:

1. Use **Ctrl+Mayús+P** para abrir la paleta de comandos.

1. Escriba **Plug and Play** y, luego, seleccione el comando **IoT Plug & Play: Submit files to Model Repository** (Enviar archivos al repositorio de modelos).

1. Seleccione los archivos **EnvironmentalSensor.interface.json** y **SensorboxModel.capabilitymodel.json** y seleccione **OK** (Aceptar).

Los archivos se almacenan ahora en el repositorio de la empresa.

## <a name="generate-code"></a>Generación de código

**Azure IoT Tools para VS Code** se puede usar para generar código de C básico a partir de un modelo. Para generar el código básico en VS Code, siga estos pasos:

1. Use **Ctrl+Mayús+P** para abrir la paleta de comandos.

1. Escriba **Plug and Play** y, luego, seleccione el comando **IoT Plug & Play: Generate Device Code Stub** (Generar código auxiliar del dispositivo).

1. Seleccione el archivo del modelo de funcionalidad **SensorboxModel.capabilitymodel.json**.

1. Escriba **sensorbox_app** como nombre del proyecto.

1. Elija **ANSI C** como lenguaje.

1. Elija **Via IoT Hub device connection string** (A través de la cadena de conexión del dispositivo de IoT Hub).

1. Elija **CMake Project on Windows** (Proyecto de CMake en Windows) como plantilla de proyecto.

1. Elija **Via Vcpkg** (Mediante Vcpkg) como forma de incluir el SDK del dispositivo.

VS Code genera el código de C básico y guarda los archivos en la carpeta **sensorbox_app** de la carpeta **modelcode**. VS Code abre una nueva ventana que contiene los archivos de código generados.

## <a name="update-the-generated-code"></a>Actualización del código generado

Antes de poder compilar y ejecutar el código, debe implementar las propiedades, la telemetría y los comandos con stub.

Para proporcionar implementaciones para el código con stub en VS Code, siga estos pasos:

1. Abra el archivo **SensorboxModel_impl.c**.

1. Agregue código para implementar las funciones con stub.

1. Guarde los cambios.

## <a name="build-the-code"></a>Compilación del código

Antes de ejecutar el código para probar el dispositivo IoT Plug and Play con un centro de IoT de Azure, debe compilar el código.

Siga las instrucciones del archivo **Readme.md** de la carpeta **sensorbox_app** para compilar y ejecutar el código en Windows. En la siguiente sección se incluyen instrucciones para recuperar una cadena de conexión de dispositivo que se usará al ejecutar el código del dispositivo.

## <a name="test-the-code"></a>Prueba del código

Al ejecutar el código, se conecta a IoT Hub y comienza a enviar valores de propiedad y telemetría de ejemplo. El dispositivo también responde a los comandos enviados desde IoT Hub. Para comprobar este comportamiento:

1. Para crear un centro de IoT:

    ```azurecli-interactive
    az group create --name environmentalsensorresources --location centralus
    az iot hub create --name {your iot hub name} \
      --resource-group environmentalsensorresources --sku F1
    ```

1. Agregue un dispositivo a su centro de IoT y recupere su cadena de conexión:

    ```azurecli-interactive
    az iot hub device-identity create --hub-name {your iot hub name} --device-id MyPnPDevice
    az iot hub device-identity show-connection-string --hub-name {your iot hub name} --device-id MyPnPDevice --output table
    ```

    Anote el nombre de la cadena de conexión.

1. En un símbolo del sistema, vaya a la carpeta **azure-iot-sdk-c** donde compiló el SDK y los ejemplos. A continuación, vaya a la carpeta **cmake\\sensorbox_app\\Release**.

1. Ejecute el siguiente comando:

    ```cmd
    sensorbox_app.exe {your device connection string}
    ```

1. Use la herramienta Azure IoT Explorer para interactuar con el dispositivo IoT Plug and Play conectado a su centro de IoT. Para más información, consulte [Instalación y uso de Azure IoT Explorer](./howto-install-iot-explorer.md).

## <a name="next-steps"></a>Pasos siguientes

Ahora que ha creado un dispositivo IoT Plug and Play listo para la certificación, aprenda a:

> [!div class="nextstepaction"]
> [Compilar un dispositivo listo para la certificación](tutorial-build-device-certification.md)

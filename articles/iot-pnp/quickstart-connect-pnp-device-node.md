---
title: Conexión del código del dispositivo de ejemplo de la versión preliminar de IoT Plug and Play a IoT Hub | Microsoft Docs
description: Con Node.js, compile y ejecute el código de dispositivo de ejemplo de la versión preliminar de IoT Plug and Play que se conecta a una instancia de IoT Hub. Use la herramienta Azure IoT Explorer para ver la información enviada por el dispositivo al centro.
author: dominicbetts
ms.author: dobett
ms.date: 12/26/2019
ms.topic: quickstart
ms.service: iot-pnp
services: iot-pnp
ms.custom: mvc
ms.openlocfilehash: c5d69ae21cd240b0c68b9694a55de2cf879a1966
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/26/2020
ms.locfileid: "76964794"
---
# <a name="quickstart-connect-a-sample-iot-plug-and-play-preview-device-application-to-iot-hub-nodejs"></a>Inicio rápido: Conexión de una aplicación de dispositivo de ejemplo de la versión preliminar de IoT Plug and Play a IoT Hub (Node.js)

[!INCLUDE [iot-pnp-quickstarts-2-selector.md](../../includes/iot-pnp-quickstarts-2-selector.md)]

En este inicio rápido se muestra cómo crear una aplicación de dispositivo IoT Plug and Play de ejemplo, conectarla a IoT Hub y usar la herramienta Azure IoT Explorer para ver la información que envía al centro. La aplicación de ejemplo se escribe para Node.js y se incluye en el SDK de dispositivo de Azure IoT Hub para Node.js. Los desarrolladores de soluciones puede usar la herramienta Azure IoT Explorer para conocer las funcionalidades de cualquier dispositivo IoT Plug and Play sin necesidad de ver nada de código de dispositivo.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="prerequisites"></a>Prerrequisitos

Para completar este inicio rápido, necesita Node.js en la máquina de desarrollo. Puede descargar la última versión recomendada para varias plataformas desde [nodejs.org](https://nodejs.org).

Puede verificar la versión actual de Node.js en el equipo de desarrollo con el comando siguiente:

```cmd/sh
node --version
```

### <a name="install-the-azure-iot-explorer"></a>Instalación de Azure IoT Explorer

Descargue e instale la versión más reciente de **Azure IoT Explorer** desde la página [Repositorio](https://github.com/Azure/azure-iot-explorer/releases) de la herramienta, seleccionando el archivo .msi en "Recursos" para la actualización más reciente.

[!INCLUDE [iot-pnp-prepare-iot-hub.md](../../includes/iot-pnp-prepare-iot-hub.md)]

Ejecute el siguiente comando para obtener la _cadena de conexión de IoT Hub_ para el centro (anótela para usarla más adelante):

```azurecli-interactive
az iot hub show-connection-string --hub-name <YourIoTHubName> --output table
```

## <a name="prepare-the-development-environment"></a>Preparación del entorno de desarrollo

En este inicio rápido, se prepara un entorno de desarrollo que se puede usar para clonar y compilar el SDK del dispositivo de Azure IoT Hub para Node.js.

Abra un símbolo del sistema en el directorio que prefiera. Ejecute el siguiente comando para clonar el repositorio de GitHub del [SDK de IoT de Microsoft Azure para Node.js](https://github.com/Azure/azure-iot-sdk-node) en esta ubicación:

```cmd/sh
git clone https://github.com/Azure/azure-iot-sdk-node --recursive -b digitaltwins-preview
```

Esta operación puede tardar varios minutos en completarse.

## <a name="install-required-libraries"></a>Instalación de bibliotecas necesarias

Para compilar el código de ejemplo incluido, se usa el SDK del dispositivo. La aplicación que se compila simula un dispositivo que se conecta a un centro de IoT. La aplicación envía datos de telemetría y propiedades, y recibe comandos.

1. En una ventana del terminal local, vaya a la carpeta del repositorio clonado y luego a la carpeta **/azure-iot-sdk-node/digitaltwins/samples/device/javascript**. Después, ejecute el siguiente comando para instalar las bibliotecas necesarias:

    ```cmd/sh
    npm install
    ```
1. Configure la _cadena de conexión del dispositivos_:

    ```cmd/sh
    set DEVICE_CONNECTION_STRING=<YourDeviceConnectionString>
    ```

## <a name="run-the-sample-device"></a>Ejecución del dispositivo de ejemplo

Ejecute una aplicación de ejemplo en el SDK para simular un dispositivo IoT Plug and Play que envía telemetría a su instancia de IoT Hub. Para ejecutar la aplicación de ejemplo, utilice el comando siguiente:

```cmd\sh
    node sample_device.js
```

Verá la salida siguiente, lo que significa que el dispositivo ha comenzado a enviar datos de telemetría al concentrador y que ya está listo para recibir comandos y actualizaciones de propiedades.

   ![Mensajes de confirmación de dispositivo](media/quickstart-connect-pnp-device/device-confirmation-node.png)

 Siga ejecutando el ejemplo a medida que complete los pasos siguientes.

## <a name="use-the-azure-iot-explorer-to-validate-the-code"></a>Uso de Azure IoT Explorer para validar el código

[!INCLUDE [iot-pnp-iot-explorer-1.md](../../includes/iot-pnp-iot-explorer-1.md)]

4. Para asegurarse de que la herramienta puede leer las definiciones del modelo de interfaz del dispositivo, seleccione **Configuración**. En el menú Configuración, es posible que **On the connected device** (En el dispositivo conectado) ya aparezca en las configuraciones de Plug and Play. Si no es así, seleccione **+ Add module definition source** (Agregar origen de definición de módulo) y, después, **On the connected device** (En el dispositivo conectado) para agregarlo.

1. En la página de información general **Dispositivos**, busque la identidad del dispositivo que creó anteriormente y selecciónela para ver más detalles. Con la aplicación de dispositivo todavía en ejecución en el símbolo del sistema, compruebe que el **estado de conexión** del dispositivo en Azure IoT Explorer se muestra como _Conectado_ (de lo contrario, presione **Actualizar** hasta que lo sea). Seleccione el dispositivo para ver más detalles.

1. Expanda la interfaz con el identificador **urn:contoso:com:EnvironmentalSensor:1** para ver la interfaz y las primitivas de IoT Plug and Play (propiedades, comandos y telemetría).

[!INCLUDE [iot-pnp-iot-explorer-2.md](../../includes/iot-pnp-iot-explorer-2.md)]

[!INCLUDE [iot-pnp-clean-resources.md](../../includes/iot-pnp-clean-resources.md)]

## <a name="next-steps"></a>Pasos siguientes

En este inicio rápido ha aprendido a conectar un dispositivo IoT Plug and Play a un centro de IoT. Para más información sobre cómo crear una solución que interactúe con dispositivos IoT Plug and Play, consulte:

> [!div class="nextstepaction"]
> [Procedimiento: Conexión e interacción con un dispositivo de la versión preliminar de IoT Plug and Play](howto-develop-solution.md)

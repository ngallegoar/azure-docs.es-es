---
title: Procedimiento para conectar una muestra de puente IoT Plug and Play que se ejecuta en Linux o Windows a una instancia de IoT Hub | Microsoft Docs
description: Compile y ejecute un puente IoT Plug and Play en Linux o Windows que se conecta a un centro de IoT. Use la herramienta Azure IoT Explorer para ver la información enviada por el dispositivo al centro.
author: usivagna
ms.author: ugans
ms.date: 09/22/2020
ms.topic: how-to
ms.service: iot-pnp
services: iot-pnp
ms.openlocfilehash: 6670f654685f8d5cdcaf55d2b1679738a57ecab4
ms.sourcegitcommit: 2e72661f4853cd42bb4f0b2ded4271b22dc10a52
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/14/2020
ms.locfileid: "92042803"
---
# <a name="how-to-connect-an--iot-plug-and-play-bridge-sample-running-on-linux-or-windows-to-iot-hub"></a>Procedimiento para conectar una muestra de puente de IoT Plug and Play que se ejecuta en Linux o Windows a IoT Hub

En este procedimiento se muestra cómo compilar el adaptador de muestra de puente IoT Plug and Play, conectarlo a IoT Hub y usar la herramienta Azure IoT Explorer para ver los datos de telemetría que envía. El puente IoT Plug and Play se escribe en C e incluye el SDK de dispositivo IoT de Azure para C. Al final de este tutorial, debería poder ejecutar el puente IoT Plug and Play y ver los informes de telemetría en Azure IoT Explorer: :::image type="content" source="media/concepts-iot-pnp-bridge/iot-pnp-bridge-explorer-telemetry.png" alt-text="Captura de pantalla que muestra Azure IoT Explorer con una tabla de telemetría (humedad, temperatura) notificada del puente IoT Plug and Play.":::

## <a name="prerequisites"></a>Requisitos previos

Puede ejecutar este inicio rápido en Linux o Windows. Los comandos de shell de este procedimiento siguen la convención de Windows para los separadores de ruta de acceso "`\`". Si utiliza Linux, asegúrese de cambiar estos separadores por "`/`".

Los requisitos previos difieren en función del sistema operativo:

### <a name="linux"></a>Linux

En este inicio rápido se asume que utiliza Ubuntu Linux. Los pasos de este inicio rápido se han probado con Ubuntu 18.04.

Para completar este inicio rápido en Linux, es preciso instalar el siguiente software en el entorno Linux local:

Instale **GCC**, **Git**, **cmake** y todas las dependencias necesarias con el comando `apt-get`:

```sh
sudo apt-get update
sudo apt-get install -y git cmake build-essential curl libcurl4-openssl-dev libssl-dev uuid-dev
```

Compruebe que la versión de `cmake` es posterior a **2.8.12** y que la versión de **GCC** es posterior a **4.4.7**.

```sh
cmake --version
gcc --version
```

### <a name="windows"></a>Windows

Para completar este inicio rápido en Windows, instale el siguiente software en el entorno Windows local:

* [Visual Studio (Community, Professional o Enterprise)](https://visualstudio.microsoft.com/downloads/): asegúrese de incluir la carga de trabajo **Desarrollo para el escritorio con C++** cuando [instale](/cpp/build/vscpp-step-0-installation?preserve-view=true&view=vs-2019) Visual Studio.
* [Git](https://git-scm.com/download/).
* [CMake](https://cmake.org/download/).

### <a name="azure-iot-explorer"></a>Azure IoT Explorer

Para interactuar con el dispositivo de ejemplo en la segunda parte de esta guía de inicio rápido, use la herramienta **Azure IoT Explorer**. [Descargue e instale la versión de Azure IoT Explorer más reciente](./howto-use-iot-explorer.md) para su sistema operativo.

[!INCLUDE [iot-pnp-prepare-iot-hub.md](../../includes/iot-pnp-prepare-iot-hub.md)]

Ejecute el siguiente comando para obtener la _cadena de conexión de IoT Hub_ para su centro. Tome nota de esta cadena de conexión, ya que la usará más adelante en este inicio rápido:

```azurecli-interactive
az iot hub show-connection-string --hub-name <YourIoTHubName> --output table
```

> [!TIP]
> También puede usar la herramienta Azure IoT Explorer para buscar la cadena de conexión del centro de IoT.

Ejecute el siguiente comando para obtener la _cadena de conexión del dispositivo_ que ha agregado al centro. Tome nota de esta cadena de conexión, ya que la usará más adelante en este inicio rápido:

```azurecli-interactive
az iot hub device-identity show-connection-string --hub-name <YourIoTHubName> --device-id <YourDeviceID> --output table
```

## <a name="view-the-model"></a>Vista del modelo

Más adelante, usará Azure IoT Explorer para ver el dispositivo cuando se conecte a IoT Hub. Azure IoT Explorer necesitará una copia local del archivo de modelo que coincida con el **identificador de modelo** que envía el dispositivo. El archivo de modelo permite que Azure IoT Explorer muestre los datos de telemetría, las propiedades y los comandos que implementa el dispositivo.

Al descargar el código en el paso siguiente, se incluyen los archivos de modelo de ejemplo en la carpeta `pnpbridge/docs/schema`. Para preparar Azure IoT Explorer:

1. Cree una carpeta denominada *models* en la máquina local.
1. Visualice [EnvironmentalSensor.json](https://aka.ms/iot-pnp-bridge-env-model) y guarde el archivo JSON en la carpeta *models*.
1. Visualice [RootBridgeSampleDevice.json](https://aka.ms/iot-pnp-bridge-root-model) y guarde el archivo JSON en la carpeta *models*.

## <a name="download-the-code"></a>Descargar el código

Abra un símbolo del sistema en el directorio que prefiera. Ejecute el siguiente comando para clonar el repositorio de GitHub del [puente IoT Plug and Play](https://aka.ms/iotplugandplaybridge) en esta ubicación:

```cmd
git clone https://github.com/Azure/iot-plug-and-play-bridge.git
```

Después de clonar el repositorio de puente de IoT Plug and Play Bridge en la máquina, abra un símbolo del sistema de comandos administrativo y navegue hasta el directorio del repositorio clonado:

```cmd
cd pnpbridge
git submodule update --init --recursive
```

Esta operación puede tardar varios minutos en completarse.

>[!NOTE]
> Si surgen problemas con la actualización del submódulo del clon de GIT, se trata de un problema conocido con las rutas de acceso de archivos de Windows y GIT. Consulte https://github.com/msysgit/git/pull/110. Para resolver el problema, puede probar el comando siguiente: `git config --system core.longpaths true`

## <a name="setting-up-the-configuration-json"></a>Ajustes del código JSON de configuración

Después de clonar el repositorio de puente IoT Plug and Play en el equipo, vaya al directorio `pnpbridge/docs/schema` del repositorio clonado, donde podrá encontrar el [código JSON de configuración](https://aka.ms/iot-pnp-bridge-env-config) o `config.json` para la muestra del sensor del entorno del puente. Puede obtener más información acerca de los archivos de configuración en el [documento sobre conceptos de puente IoT Plug and Play](concepts-iot-pnp-bridge.md).

En el campo `root-_interface_model_id`, deberá copiar el identificador del modelo de IoT Plug and Play que identifique el modelo del dispositivo. En este ejemplo, es `dtmi:com:example:SampleDevice;1`. Modifique los parámetros siguientes en **pnp_bridge_parameters** en el archivo `config.json` en:

  Uso de la cadena de conexión (el valor de symmetric_key debe coincidir con la clave SAS en la cadena de conexión):

  ```JSON
    {
      "connection_parameters": {
        "connection_type" : "connection_string",
        "connection_string" : "[CONNECTION STRING]",
        "root_interface_model_id": "[To fill in]",
        "auth_parameters" : {
          "auth_type" : "symmetric_key",
          "symmetric_key" : "[SYMMETRIC KEY]"
        }
      }
    }
  }
  ```

 Una vez completado, el archivo `config.json` debe tener este aspecto:

   ```JSON
    {
      "connection_parameters": {
        "connection_type" : "connection_string",
        "connection_string" : "[CONNECTION STRING]",
        "root_interface_model_id": "dtmi:com:example:SampleDevice;1",
        "auth_parameters" : {
          "auth_type" : "symmetric_key",
          "symmetric_key" : "[SYMMETRIC KEY]"
        }
      }
    }
  }
```

 Una vez compilado el puente, tendrá que colocar `config.json` en el mismo directorio que el puente o especificar su ruta de acceso cuando se ejecute.

## <a name="build-the-iot-plug-and-play-bridge"></a>Compilación del puente IoT Plug and Play

Navegue hasta la carpeta *pnpbridge* en el directorio del repositorio.

En Windows, ejecute lo siguiente en un [Símbolo del sistema para desarrolladores para Visual Studio](/dotnet/framework/tools/developer-command-prompt-for-vs):

```cmd
cd scripts\windows
build.cmd
```

Del mismo modo, para Linux, ejecute el siguiente comando:

```bash
cd scripts/linux
./setup.sh
./build.sh
```

>[!TIP]
>En Windows, puede abrir la solución generada por el comando cmake en Visual Studio 2019. Abra el archivo de proyecto *azure_iot_pnp_bridge.sln* en el directorio cmake y establezca el proyecto *pnpbridge_bin* como proyecto de inicio en la solución. Ahora puede compilar el ejemplo en Visual Studio y ejecutarlo en modo de depuración.

## <a name="start-the-iot-plug-and-play-bridge"></a>Inicio del puente IoT Plug and Play

 Inicie la muestra del puente IoT Plug and Play para los sensores de entorno. Para ello, vaya a la carpeta *pnpbridge* y ejecute lo siguiente en un símbolo del sistema:

```bash
 cd cmake/pnpbridge_x86/src/adaptors/samples/environmental_sensor/
./pnpbridge_environmentalsensor

```

```cmd
REM Windows
cd cmake\pnpbridge_x86\src\adaptors\samples\environmental_sensor
Debug\pnpbridge_environmentalsensor.exe
```

## <a name="use-azure-iot-explorer-to-validate-the-code"></a>Uso de Azure IoT Explorer para validar el código

Una vez iniciado el ejemplo de cliente de dispositivo, compruebe que funciona con la herramienta Azure IoT Explorer.

[!INCLUDE [iot-pnp-iot-explorer.md](../../includes/iot-pnp-iot-explorer.md)]

[!INCLUDE [iot-pnp-clean-resources.md](../../includes/iot-pnp-clean-resources.md)]

## <a name="next-steps"></a>Pasos siguientes

En este inicio rápido ha aprendido a conectar un dispositivo IoT Plug and Play a un centro de IoT. Para más información sobre cómo crear una solución que interactúe con dispositivos IoT Plug and Play, consulte:

* [¿Qué es un puente IoT Plug and Play?](./concepts-iot-pnp-bridge.md)
* [Consulte la referencia para desarrolladores de GitHub para el puente IoT Plug and Play](https://aka.ms/iot-pnp-bridge-dev-doc)
* [Puente IoT Plug and Play en GitHub](https://aka.ms/iotplugandplaybridge)
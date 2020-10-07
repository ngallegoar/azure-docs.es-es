---
title: Conexión del código de dispositivo de C# de ejemplo de IoT Plug and Play a IoT Hub | Microsoft Docs
description: Compile y ejecute el código de dispositivo de ejemplo de IoT Plug and Play en Windows que se conecta a un centro de IoT. Use la herramienta Azure IoT Explorer para ver la información enviada por el dispositivo al centro.
author: ericmitt
ms.author: ericmitt
ms.date: 07/14/2020
ms.topic: quickstart
ms.service: iot-pnp
services: iot-pnp
ms.openlocfilehash: d1deac1c7932a8f3cec06d9c264ba401f7f1341d
ms.sourcegitcommit: a422b86148cba668c7332e15480c5995ad72fa76
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/30/2020
ms.locfileid: "91577040"
---
# <a name="quickstart-connect-a-sample-iot-plug-and-play-device-application-running-on-windows-to-iot-hub-c"></a>Inicio rápido: Conexión de una aplicación de dispositivo de ejemplo de IoT Plug and Play en Windows a IoT Hub (C#)

[!INCLUDE [iot-pnp-quickstarts-device-selector.md](../../includes/iot-pnp-quickstarts-device-selector.md)]

En este inicio rápido se muestra cómo compilar una aplicación de dispositivo IoT Plug and Play de ejemplo, conectarla al centro de IoT y usar la herramienta Azure IoT Explorer para ver los datos de telemetría que envía. La aplicación de ejemplo se escribe en CSharp y se incluye en el SDK de dispositivo de Azure IoT para C#. Un generador de soluciones puede usar la herramienta Azure IoT Explorer para comprender las funcionalidades de cualquier dispositivo IoT Plug and Play sin necesidad de ver nada de código del dispositivo.

## <a name="prerequisites"></a>Requisitos previos

[!INCLUDE [iot-pnp-prerequisites](../../includes/iot-pnp-prerequisites.md)]

Para completar este inicio rápido en Windows, es preciso que el siguiente software esté instalado en la máquina de desarrollo:

* [Visual Studio 2019 (Community, Professional o Enterprise)](https://visualstudio.microsoft.com/downloads/).
* [Git](https://git-scm.com/download/).

## <a name="download-the-code"></a>Descargar el código

En este inicio rápido, se prepara un entorno de desarrollo que se puede usar para clonar y compilar el SDK de C# del dispositivo de Azure IoT Hub.

Abra un símbolo del sistema en la carpeta de su elección. Ejecute el siguiente comando para clonar el repositorio de GitHub de los [ejemplos de Microsoft Azure IoT para .NET](https://github.com/Azure-Samples/azure-iot-samples-csharp) en esta ubicación:

```cmd
git clone  https://github.com/Azure-Samples/azure-iot-samples-csharp.git
```

## <a name="build-the-code"></a>Compilación del código

Ahora puede compilar el ejemplo en Visual Studio y ejecutarlo en modo de depuración.

1. Abra el archivo de proyecto *azure-iot-samples-csharp\iot-hub\Samples\device\PnpDeviceSamples\Thermostat\Thermostat.csproj* en Visual Studio 2019.

1. En Visual Studio, vaya a **Project > Thermostat Properties > Debug** (Proyecto > Propiedades del termostato > Depurar). Después, agregue las siguientes variables de entorno al proyecto:

    | Nombre | Value |
    | ---- | ----- |
    | IOTHUB_DEVICE_SECURITY_TYPE | DPS |
    | IOTHUB_DEVICE_DPS_ENDPOINT | global.azure-devices-provisioning.net |
    | IOTHUB_DEVICE_DPS_ID_SCOPE | El valor que anotó al completar la [configuración del entorno](set-up-environment.md). |
    | IOTHUB_DEVICE_DPS_DEVICE_ID | my-pnp-device |
    | IOTHUB_DEVICE_DPS_DEVICE_KEY | El valor que anotó al completar la [configuración del entorno](set-up-environment.md). |

Ahora puede compilar el ejemplo en Visual Studio y ejecutarlo en modo de depuración.

## <a name="run-the-device-sample"></a>Ejecución del ejemplo de dispositivo

Para realizar el seguimiento de la ejecución del código en Visual Studio en Windows, agregue un punto de interrupción a la función `main` en el archivo program.cs.

El dispositivo ya está listo para recibir comandos y actualizaciones de propiedades, y ha empezado a enviar datos de telemetría al centro. Siga ejecutando el ejemplo a medida que complete los pasos siguientes.

## <a name="use-azure-iot-explorer-to-validate-the-code"></a>Uso de Azure IoT Explorer para validar el código

Una vez iniciado el ejemplo de cliente de dispositivo, compruebe que funciona con la herramienta Azure IoT Explorer.

[!INCLUDE [iot-pnp-iot-explorer.md](../../includes/iot-pnp-iot-explorer.md)]

## <a name="review-the-code"></a>Revisión del código

En este ejemplo se implementa un dispositivo termostato de IoT Plug and Play sencillo. El modelo que se implementa en este ejemplo no usa los [componentes](concepts-components.md) de IoT Plug and Play. El [archivo de modelo del lenguaje de definición de Digital Twins (DTDL) del dispositivo termostato](https://github.com/Azure/opendigitaltwins-dtdl/blob/master/DTDL/v2/samples/Thermostat.json) define la telemetría, las propiedades y los comandos que implementa el dispositivo.

El código del dispositivo se conecta al centro de IoT mediante el método `CreateFromConnectionString` estándar. El dispositivo envía el identificador de modelo del modelo DTDL que implementa en la solicitud de conexión. Un dispositivo que envía un identificador de modelo es un dispositivo IoT Plug and Play:

```csharp
private static void InitializeDeviceClientAsync()
{
  var options = new ClientOptions
  {
    ModelId = ModelId,
  };
  s_deviceClient = DeviceClient.CreateFromConnectionString(s_deviceConnectionString, TransportType.Mqtt, options);
  s_deviceClient.SetConnectionStatusChangesHandler((status, reason) =>
  {
     s_logger.LogDebug($"Connection status change registered - status={status}, reason={reason}.");
  });
}
```

El identificador del modelo se almacena en el código tal y como se muestra en el siguiente fragmento de código:

```csharp
private const string ModelId = "dtmi:com:example:Thermostat;1";
```

El código que actualiza las propiedades, controla los comandos y envía la telemetría es idéntico al código de un dispositivo que no usa las convenciones de IoT Plug and Play.

En el ejemplo se usa una biblioteca JSON para analizar objetos JSON en las cargas enviadas desde el centro de IoT:

```csharp
using Newtonsoft.Json;

...

DateTime since = JsonConvert.DeserializeObject<DateTime>(request.DataAsJson);
```

## <a name="next-steps"></a>Pasos siguientes

En este inicio rápido ha aprendido a conectar un dispositivo IoT Plug and Play a un centro de IoT. Para más información sobre cómo crear una solución que interactúe con dispositivos IoT Plug and Play, consulte:

> [!div class="nextstepaction"]
> [Procedimiento: Conexión e interacción con un dispositivo](howto-develop-solution.md)

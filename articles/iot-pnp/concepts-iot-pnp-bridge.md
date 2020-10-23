---
title: Puente IoT Plug and Play | Microsoft Docs
description: Comprenda el puente Iot Plug and Play y cómo usarlo para conectar dispositivos existentes conectados a una puerta de enlace Windows o Linux como dispositivos IoT Plug and Play.
author: usivagna
ms.author: ugans
ms.date: 09/22/2020
ms.topic: conceptual
ms.service: iot-pnp
services: iot-pnp
ms.custom: mvc
ms.openlocfilehash: 0435fe3946118d59d786dd3e6cec350a5ab4eee4
ms.sourcegitcommit: 2e72661f4853cd42bb4f0b2ded4271b22dc10a52
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/14/2020
ms.locfileid: "92046458"
---
# <a name="iot-plug-and-play-bridge"></a>Puente de IoT Plug and Play

El puente Iot Plug and Play es una aplicación de código abierto para conectar dispositivos existentes conectados a una puerta de enlace Windows o Linux como dispositivos IoT Plug and Play. Después de instalar y configurar la aplicación en la máquina Windows o Linux, puede usarla para conectar los dispositivos conectados a IoT Hub. Puede usar el puente para asignar interfaces de IoT Plug and Play a la telemetría que los dispositivos conectados envían, trabajar con propiedades de dispositivo e invocar comandos.

:::image type="content" source="media/concepts-iot-pnp-bridge/iot-pnp-bridge-high-level.png" alt-text="En el lado izquierdo, hay un par de sensores existentes conectados (tanto cableados como inalámbricos) a un equipo Windows o Linux que contiene un puente IoT Plug and Play. A continuación, el puente IoT Plug and Play se conecta a IoT Hub en el lado derecho.":::

El puente IoT Plug and Play se puede implementar como un archivo ejecutable independiente en cualquier dispositivo IoT, equipo industrial, servidor o puerta de enlace que ejecute Windows 10 o Linux. También se puede compilar en el código de la aplicación. Un archivo JSON de configuración sencillo indica al puente IoT Plug and Play qué dispositivos o periféricos conectados deben exponerse a Azure.

## <a name="supported-protocols-and-sensors"></a>Protocolos y sensores admitidos

El puente IoT Plug and Play admite los siguientes tipos de periféricos de forma predeterminada, con vínculos a la documentación del adaptador:

|Periférico|Windows|Linux|
|---------|---------|---------|
|[Bluetooth LE](https://aka.ms/iot-pnp-bridge-bluetooth)       |Sí|No|
|[Cámaras](https://aka.ms/iot-pnp-bridge-camera)               |Sí|No|
|[Modbus](https://aka.ms/iot-pnp-bridge-modbus)                |Sí|Sí|
|[MQTT](https://aka.ms/iot-pnp-bridge-mqtt)                    |Sí|Sí|
|[Serie](https://aka.ms/iot-pnp-bridge-serial)                |Sí|Sí|
|[Periféricos USB de Windows](https://aka.ms/iot-pnp-bridge-usb)  |Sí|No aplicable|

>[!Important]
>Los desarrolladores pueden extender el puente IoT Plug and Play para admitir protocolos de dispositivo adicionales siguiendo las instrucciones de la **[documentación para desarrolladores de puentes IoT Plug and Play](https://aka.ms/iot-pnp-bridge-dev-doc)** .

## <a name="prerequisites"></a>Requisitos previos

### <a name="os-platform"></a>Plataforma de sistema operativo

Se admiten las siguientes plataformas y versiones de SO:

|Plataforma  |Versiones admitidas  |
|---------|---------|
|Windows 10 |     Se admiten todas las SKU de Windows. Por ejemplo: IoT Enterprise, Server, Desktop y IoT Core. *Para la funcionalidad de seguimiento de estado de la cámara, se recomienda 20H1 o una versión posterior. El resto de funcionalidades están disponibles en todas las compilaciones de Windows 10.*  |
|Linux     |Esta funcionalidad, probada y admitida en Ubuntu 18.04, no se ha probado en otras distribuciones.         |
||

### <a name="hardware"></a>Hardware

- Cualquier plataforma de hardware capaz de admitir las SKU y versiones anteriores del sistema operativo.
- Los sensores y periféricos de serie, USB, Bluetooth y cámara se admiten de forma nativa. El puente IoT Plug and Play se puede extender para admitir cualquier periférico o sensor personalizado ([consulte la sección sobre periféricos anterior](#iot-plug-and-play-bridge)).

### <a name="development-environment"></a>Entorno de desarrollo

Para compilar, extender y desarrollar el puente IoT Plug and Play, necesitará:  

- Un entorno de desarrollo que admite la compilación en C++, como: [Visual Studio (Community, Professional o Enterprise)](https://visualstudio.microsoft.com/downloads/): asegúrese de incluir la carga de trabajo Desarrollo para el escritorio con C++ al instalar Visual Studio.
- [CMake](https://cmake.org/download/): al instalar CMake, seleccione la opción `Add CMake to the system PATH`.
- Si está compilando en Windows, también tendrá que descargar el SDK de Windows 17763: [https://developer.microsoft.com/windows/downloads/windows-10-sdk](https://developer.microsoft.com/windows/downloads/windows-10-sdk)
- [SDK en C del dispositivo Azure IoT Hub](https://github.com/Azure/azure-iot-sdk-c). Los scripts de compilación incluidos en este repositorio clonarán automáticamente el SDK en de Azure IoT requerido.

### <a name="azure-iot-products-and-tools"></a>Productos y herramientas de Azure IoT

- **Azure IoT Hub**: necesitará una instancia de [Azure IoT Hub](../iot-hub/index.yml) en su suscripción de Azure para conectar el dispositivo. Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/) antes de empezar. Si no tiene una instancia de IoT Hub, siga [estas instrucciones para crear una](../iot-hub/iot-hub-create-using-cli.md).

> [!Note]
> IoT Plug and Play se encuentra actualmente disponible en los centros de IoT creados en las regiones Centro de EE. UU., Norte de Europa y Japón Oriental. La compatibilidad con IoT Plug and Play no se incluye en los centros de IoT de nivel básico. Para interactuar con el dispositivo IoT Plug and Play, puede usar la herramienta Azure IoT Explorer. [Descargue e instale la versión de Azure IoT Explorer más reciente](./howto-use-iot-explorer.md) para su sistema operativo.

## <a name="iot-plug-and-play-bridge-architecture"></a>Arquitectura de puente IoT Plug and Play

:::image type="content" source="media/concepts-iot-pnp-bridge/iot-pnp-bridge-components.png" alt-text="En el lado izquierdo, hay un par de sensores existentes conectados (tanto cableados como inalámbricos) a un equipo Windows o Linux que contiene un puente IoT Plug and Play. A continuación, el puente IoT Plug and Play se conecta a IoT Hub en el lado derecho.":::

## <a name="download-iot-plug-and-play-bridge"></a>Descarga de un puente IoT Plug and Play

Puede descargar una versión pregenerada del puente con adaptadores admitidos en las [versiones de puente de IoT Plug and Play](https://aka.ms/iot-pnp-bridge-releases) y expandir la lista de recursos para la versión más reciente. Descargue la versión más reciente de la aplicación para el sistema operativo.

También puede descargar y ver el código fuente del puente [IoT Plug and Play en GitHub](https://aka.ms/bridge).

## <a name="next-steps"></a>Pasos siguientes

Ahora que tiene una visión general de la arquitectura de un puente IoT Plug and Play, los pasos siguientes sirven para obtener más información acerca de:

- [Procedimiento para usar el puente IoT Plug and Play](./howto-use-iot-pnp-bridge.md)
- [Consulte la referencia para desarrolladores de GitHub para el puente IoT Plug and Play](https://aka.ms/iot-pnp-bridge-dev-doc)
- [Puente IoT Plug and Play en GitHub](https://aka.ms/iotplugandplaybridge)
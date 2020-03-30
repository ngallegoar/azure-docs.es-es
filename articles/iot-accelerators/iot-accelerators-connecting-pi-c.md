---
title: Aprovisionamiento de Raspberry Pi para la supervisión remota con C en Azure | Microsoft Docs
description: Se describe cómo conectar un dispositivo Raspberry Pi al acelerador de la solución de supervisión remota mediante una aplicación creada en C.
author: dominicbetts
manager: timlt
ms.service: iot-accelerators
services: iot-accelerators
ms.topic: conceptual
ms.date: 03/08/2019
ms.author: dobett
ms.openlocfilehash: 3331db51f4d141cf142d1bd0578043ca6681f3cd
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "61454513"
---
# <a name="connect-your-raspberry-pi-device-to-the-remote-monitoring-solution-accelerator-c"></a>Conexión del dispositivo Raspberry Pi al acelerador de la solución de supervisión remota (C)

[!INCLUDE [iot-suite-selector-connecting](../../includes/iot-suite-selector-connecting.md)]

En este tutorial se muestra cómo conectar un dispositivo real al acelerador de soluciones de supervisión remota. Como sucede con la mayoría de aplicaciones insertadas que se ejecutan en dispositivos restringidos, el código cliente para la aplicación del dispositivo Raspberry Pi se escribe en C. En este tutorial, compilará la aplicación en un dispositivo Raspberry Pi que ejecuta el sistema operativo Raspbian.

Si prefiere simular un dispositivo, consulte [Crear y probar un nuevo dispositivo simulado](iot-accelerators-remote-monitoring-create-simulated-device.md).

### <a name="required-hardware"></a>Requisitos de hardware

Un equipo de escritorio que permita conectarse remotamente a la línea de comandos en Raspberry Pi.

[Kit de inicio de Microsoft IoT para Raspberry Pi 3](https://azure.microsoft.com/develop/iot/starter-kits/) o componentes equivalentes. En este tutorial se usan los siguientes elementos del kit:

- Raspberry Pi 3
- Tarjeta MicroSD (con NOOBS)
- Un cable USB mini
- Un cable Ethernet

### <a name="required-desktop-software"></a>Requiere el software de escritorio

Necesita el cliente SSH en su máquina de escritorio para poder acceder de forma remota a la línea de comandos en su Raspberry Pi.

- Windows no incluye ningún cliente SSH. Se recomienda usar [PuTTY](https://www.putty.org/).
- La mayoría de las distribuciones de Linux y Mac OS incluyen la utilidad de línea de comandos de SSH. Para más información, consulte [SSH Using Linux or Mac OS](https://www.raspberrypi.org/documentation/remote-access/ssh/unix.md) (SSH cuando se usa Linux o Mac OS).

### <a name="required-raspberry-pi-software"></a>Software necesario de Raspberry Pi

En este artículo se supone que instaló la versión más reciente del [sistema operativo Raspbian OS en un dispositivo Raspberry Pi](https://www.raspberrypi.org/learning/software-guide/quickstart/).

Los pasos siguientes muestran cómo preparar el dispositivo Raspberry Pi para compilar una aplicación C que se conecta al acelerador de la solución:

1. Conecte con su dispositivo Raspberry Pi mediante **ssh**. Para obtener más información, consulte [SSH (Secure Shell)](https://www.raspberrypi.org/documentation/remote-access/ssh/README.md) en el [sitio web de Raspberry Pi](https://www.raspberrypi.org/).

1. Use el siguiente comando para actualizar Raspberry Pi:

    ```sh
    sudo apt-get update
    ```

1. Para completar los pasos descritos en esta guía de procedimientos, siga los pasos indicados en [Set up a Linux development environment](https://github.com/Azure/azure-iot-sdk-c/blob/master/doc/devbox_setup.md#linux) (Configurar un entorno de desarrollo de Linux) para agregar las bibliotecas y las herramientas de desarrollo necesarias a su Raspberry Pi.

## <a name="view-the-code"></a>Ver el código

El [código de ejemplo](https://github.com/Azure/azure-iot-sdk-c/tree/master/samples/solutions/remote_monitoring_client) que se usa en esta guía está disponible en el repositorio de GitHub de los SDK de C para Azure IoT.

### <a name="download-the-source-code-and-prepare-the-project"></a>Descarga del código fuente y preparación del proyecto

Para preparar el proyecto, clone o descargue el [repositorio de SDK de C para Azure IoT](https://github.com/Azure/azure-iot-sdk-c) en GitHub.

El ejemplo se encuentra en la carpeta **samples/solutions/remote_monitoring_client**.

Abra el archivo **remote_monitoring.c** de la carpeta **samples/solutions/remote_monitoring_client** en un editor de texto.

[!INCLUDE [iot-accelerators-connecting-code](../../includes/iot-accelerators-connecting-code.md)]

## <a name="build-and-run-the-application"></a>Compilación y ejecución de la aplicación

En los pasos siguientes se describe cómo se puede usar *CMake* para compilar la aplicación cliente. La aplicación cliente de supervisión remota se compila como parte del proceso de compilación del SDK.

1. Edite el archivo **remote_monitoring.c** para reemplazar `<connectionstring>` por la cadena de conexión de dispositivo que ha anotado al principio de esta guía paso a paso al agregar un dispositivo al acelerador de soluciones.

1. Vaya a la raíz de la copia clonada del [repositorio de SDK de C para Azure IoT](https://github.com/Azure/azure-iot-sdk-c) y ejecute los comandos siguientes para compilar la aplicación cliente:

    ```sh
    mkdir cmake
    cd cmake
    cmake ../
    make
    ```

1. Ejecute la aplicación cliente y envíe los datos telemetría a IoT Hub:

    ```sh
    ./samples/solutions/remote_monitoring_client/remote_monitoring_client
    ```

    La consola muestra mensajes como:

    - La aplicación envía datos de telemetría de ejemplo al acelerador de la solución.
    - Responde a los métodos invocados desde el panel de la solución.

[!INCLUDE [iot-suite-visualize-connecting](../../includes/iot-suite-visualize-connecting.md)]

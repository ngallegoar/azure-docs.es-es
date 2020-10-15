---
title: 'Aprovisionamiento de dispositivos Linux para la supervisión remota en C: Azure | Microsoft Docs'
description: Se describe cómo conectar un dispositivo al acelerador de la solución de supervisión remota mediante una aplicación creada en C y que se ejecuta en Linux.
author: dominicbetts
manager: timlt
ms.service: iot-accelerators
services: iot-accelerators
ms.topic: conceptual
ms.date: 08/31/2018
ms.author: dobett
ms.openlocfilehash: 91d4eda566c8b534daa10c62637db28ccb01bbb6
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "61454508"
---
# <a name="connect-your-device-to-the-remote-monitoring-solution-accelerator-linux"></a>Conexión del dispositivo al acelerador de la solución de supervisión remota (Linux)

[!INCLUDE [iot-suite-selector-connecting](../../includes/iot-suite-selector-connecting.md)]

En este tutorial se muestra cómo conectar un dispositivo real al acelerador de soluciones de supervisión remota.

Como sucede con la mayoría de aplicaciones insertadas que se ejecutan en dispositivos restringidos, el código cliente para la aplicación del dispositivo está escrito en C. En este tutorial, se crea la aplicación en un equipo que ejecuta Ubuntu (Linux).

Si prefiere simular un dispositivo, consulte [Crear y probar un nuevo dispositivo simulado](iot-accelerators-remote-monitoring-create-simulated-device.md).

## <a name="prerequisites"></a>Prerequisites

Para completar los pasos de esta guía paso a paso, necesita un dispositivo que ejecute la versión 15.04 o posterior de Ubuntu. Antes de continuar, [configure su entorno de desarrollo de Linux](https://github.com/Azure/azure-iot-sdk-c/blob/master/doc/devbox_setup.md#linux).

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

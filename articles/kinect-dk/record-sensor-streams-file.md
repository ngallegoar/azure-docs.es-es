---
title: 'Inicio rápido: Grabación de secuencias del sensor de Azure Kinect en un archivo'
description: En este inicio rápido, aprenderá a grabar flujos de datos del SDK de Sensor en un archivo.
author: tesych
ms.author: tesych
ms.prod: kinect-dk
ms.date: 06/26/2019
ms.topic: quickstart
keywords: azure, kinect, record, play back, reader, matroska, mkv, streams, depth, rgb, camera, color, imu, audio, sensor
ms.openlocfilehash: 3dab147b593bf012bd6cd9c95d0195e84a2cbcf1
ms.sourcegitcommit: fe6c9a35e75da8a0ec8cea979f9dec81ce308c0e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/26/2020
ms.locfileid: "85277836"
---
# <a name="quickstart-record-azure-kinect-sensor-streams-to-a-file"></a>Inicio rápido: Grabación de secuencias del sensor de Azure Kinect en un archivo

En este inicio rápido se proporciona información sobre la forma de usar la [grabadora de Azure Kinect](azure-kinect-recorder.md) para grabar en un archivo flujos de datos del SDK del sensor.

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

## <a name="prerequisites"></a>Requisitos previos

En este inicio rápido se da por hecho que:

- Azure Kinect DK está conectado a su equipo host y recibe alimentación eléctrica correctamente.
- Ha terminado de [configurar](set-up-azure-kinect-dk.md) el hardware.

## <a name="create-recording"></a>Creación de una grabación

1. Abra un símbolo del sistema y especifique la ruta de acceso a la [grabadora de Azure Kinect](azure-kinect-recorder.md), que se encuentra en el directorio de herramientas instaladas como `k4arecorder.exe`. Por ejemplo: `C:\Program Files\Azure Kinect SDK\tools\k4arecorder.exe`.
2. Grabe 5 segundos.

    `k4arecorder.exe -l 5 %TEMP%\output.mkv`

3. De forma predeterminada, la grabadora usa el modo de profundidad NFOV sin binning y genera salidas de RGB de 1080p a 30 fps, incluidos los datos de IMU. Para obtener una información general completa de las opciones de grabación y algunas sugerencias, consulte el artículo sobre la [grabadora de Azure Kinect](azure-kinect-recorder.md).

## <a name="play-back-recording"></a>Reproducción de grabación

Puede usar el [visor de Azure Kinect](azure-kinect-viewer.md) para reproducir una grabación.

1. Inicio [`k4aviewer.exe`](azure-kinect-viewer.md)
2. Seleccione la pestaña **Open Recording** (Abrir grabación) y abra la grabación.

## <a name="next-steps"></a>Pasos siguientes

Ahora que ha aprendido a grabar flujos de sensor en un archivo, es el momento de

> [!div class="nextstepaction"]
> [Compilar la primera aplicación de Azure Kinect](build-first-app.md)

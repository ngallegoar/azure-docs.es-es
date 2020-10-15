---
title: Grabadora de Azure Kinect DK
description: Descripción de cómo grabar flujos de datos del SDK del sensor en un archivo mediante la grabadora de Azure Kinect.
author: tesych
ms.author: tesych
ms.prod: kinect-dk
ms.date: 06/26/2019
ms.topic: conceptual
keywords: Kinect, grabar, reproducir, lector, matroska, mkv, flujos, profundidad, RGB, cámara, color, IMU, sonido
ms.openlocfilehash: 5fb6895d4102956a991c67ffab836a26b7a3abb0
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "85276961"
---
# <a name="azure-kinect-dk-recorder"></a>Grabadora de Azure Kinect DK

En este artículo se explica cómo puede usar la utilidad de línea de comandos `k4arecorder` para registrar flujos de datos del SDK del sensor en un archivo.

>[!NOTE]
>La grabadora de Azure Kinect no graba sonido.

## <a name="recorder-options"></a>Opciones de grabadora

`k4arecorder` tiene varios argumentos de línea de comandos para especificar el archivo de salida y los modos de grabación.

Las grabaciones se almacenan en el [formato Matroska .mkv](record-file-format.md). La grabación usa varias pistas de vídeo para el color y la profundidad, además de información adicional, como la calibración y los metadatos de la cámara.

```console
k4arecorder [options] output.mkv

 Options:
  -h, --help              Prints this help
  --list                  List the currently connected K4A devices
  --device                Specify the device index to use (default: 0)
  -l, --record-length     Limit the recording to N seconds (default: infinite)
  -c, --color-mode        Set the color sensor mode (default: 1080p), Available options:
                            3072p, 2160p, 1536p, 1440p, 1080p, 720p, 720p_NV12, 720p_YUY2, OFF
  -d, --depth-mode        Set the depth sensor mode (default: NFOV_UNBINNED), Available options:
                            NFOV_2X2BINNED, NFOV_UNBINNED, WFOV_2X2BINNED, WFOV_UNBINNED, PASSIVE_IR, OFF
  --depth-delay           Set the time offset between color and depth frames in microseconds (default: 0)
                            A negative value means depth frames will arrive before color frames.
                            The delay must be less than 1 frame period.
  -r, --rate              Set the camera frame rate in Frames per Second
                            Default is the maximum rate supported by the camera modes.
                            Available options: 30, 15, 5
  --imu                   Set the IMU recording mode (ON, OFF, default: ON)
  --external-sync         Set the external sync mode (Master, Subordinate, Standalone default: Standalone)
  --sync-delay            Set the external sync delay off the master camera in microseconds (default: 0)
                            This setting is only valid if the camera is in Subordinate mode.
  -e, --exposure-control  Set manual exposure value (-11 to 1) for the RGB camera (default: auto exposure)
```

## <a name="record-files"></a>Grabación de archivos

Ejemplo 1. Grabar con el modo de profundidad NFOV sin binning (640 x 576), RGB de 1080p a 30 fps con IMU.
Presione las teclas **CTRL+C** para detener la grabación.

```
k4arecorder.exe output.mkv
```

Ejemplo 2. Grabar con WFOV sin binning (1 MP), RGB de 3072p a 15 fps sin IMU, durante 10 segundos.

```
k4arecorder.exe -d WFOV_UNBINNED -c 3072p -r 15 -l 10 --imu OFF output.mkv
```

Ejemplo 3. Grabar WFOV de 2x2 con binning a 30 fps durante 5 segundos y guardarlo en output.mkv.

```
k4arecorder.exe -d WFOV_2X2BINNED -c OFF --imu OFF -l 5 output.mkv
```

>[!TIP]
>Puede usar el [visor de Azure Kinect](azure-kinect-viewer.md) para configurar los controles de la cámara RGB antes de la grabación (por ejemplo, para configurar un equilibrio de blancos manual).

## <a name="verify-recording"></a>Comprobación de la grabación

Puede abrir el archivo output.mkv con [el visor de Azure Kinect](azure-kinect-viewer.md).

Para extraer las pistas o ver la información del archivo, están disponibles herramientas como `mkvinfo` como parte del kit de herramientas [MKVToolNix](https://mkvtoolnix.download/).

## <a name="next-steps"></a>Pasos siguientes

[Uso de la grabadora con unidades sincronizadas externas](record-external-synchronized-units.md)
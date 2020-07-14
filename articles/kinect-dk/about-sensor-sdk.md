---
title: Acerca del SDK del sensor de Azure Kinect
description: Información general sobre el kit de desarrollo de software (SDK) del sensor de Azure Kinect, sus características y herramientas.
author: tesych
ms.author: tesych
ms.prod: kinect-dk
ms.date: 06/26/2019
ms.topic: article
keywords: Azure, Kinect, RGB, IR, grabación, sensor, SDK, acceso, profundidad, vídeo, cámara, IMU, movimiento, sensor, sonido, micrófono, Matroska, SDK de sensor, descargar
ms.openlocfilehash: 17c1b33120eacb5d0c6d3c02e692d1488ef474e6
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85276985"
---
# <a name="about-azure-kinect-sensor-sdk"></a>Acerca del SDK del sensor de Azure Kinect

En este artículo se proporciona información general sobre el kit de desarrollo de software (SDK) del sensor de Azure Kinect, sus características y herramientas.

## <a name="features"></a>Características

El SDK del sensor de Azure Kinect proporciona acceso multiplataforma de bajo nivel a la configuración de dispositivo y los flujos de sensor de hardware de Azure Kinect, incluidos:

- Control del modo y del acceso a la cámara de profundidad (un modo de IR pasivo y modos de profundidad de campo de vista ancho y estrecho) 
- Control y acceso a la cámara RGB (por ejemplo, exposición y balance de blancos) 
- Acceso al sensor de movimiento (giroscopio y acelerómetro) 
- Streaming de la cámara RGB de profundidad sincronizada con retraso configurable entre cámaras 
- Control de sincronización de dispositivos externos con compensación de retraso entre dispositivos configurable 
- Acceso a los metadatos de los fotogramas de cámara para la resolución de imagen, marca de tiempo, etc. 
- Acceso a datos de calibración del dispositivo 

## <a name="tools"></a>Herramientas

- Un [visor de Azure Kinect](azure-kinect-viewer.md) para supervisar los flujos de datos del dispositivo y configurar los diferentes modos.
- Una API de lector de reproducción y [grabadora de Azure Kinect](azure-kinect-recorder.md) que usa el [formato de contenedor Matroska](record-file-format.md).
- Una [herramienta de actualización del firmware](azure-kinect-firmware-tool.md) de Azure Kinect DK.

## <a name="sensor-sdk"></a>SDK del sensor

- [Descargue el SDK del sensor](sensor-sdk-download.md).
- El SDK del sensor está disponible en el [código abierto en GitHub](https://github.com/microsoft/Azure-Kinect-Sensor-SDK).
- Para obtener más información sobre el uso, consulte [Documentación de la API del SDK del sensor](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/index.html).

## <a name="next-steps"></a>Pasos siguientes

Ahora que ha aprendido acerca del SDK del sensor de Azure Kinect, también puede:
>[!div class="nextstepaction"]
>[Descargar el código del SDK del sensor](sensor-sdk-download.md)

>[!div class="nextstepaction"]
>[Buscar y abrir un dispositivo](find-then-open-device.md)

---
title: Sistemas de coordenadas de Azure Kinect DK
description: Descripción de los sistemas de coordenadas de Azure Kinect DK asociados a los sensores de Azure DK
author: tesych
ms.author: tesych
ms.prod: kinect-dk
ms.date: 06/26/2019
ms.topic: conceptual
keywords: kinect, azure, sensor, sdk, cámara de profundidad, tof, principios, rendimiento, invalidación
ms.openlocfilehash: 4bb1c3b79862b918870cff786042d9b4c66270d1
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85276894"
---
# <a name="azure-kinect-dk-coordinate-systems"></a>Sistemas de coordenadas de Azure Kinect DK

En este artículo se describen las convenciones que se usan en los sistemas de coordenadas 2D y 3D.  Hay sistemas de coordenadas independientes asociados al dispositivo de cada sensor y las [funciones de calibración](use-calibration-functions.md) permitidas para transformar puntos entre ellos. Las [funciones de transformación](use-image-transformation.md) transforman imágenes completas entre sistemas de coordenadas.  

## <a name="2d-coordinate-systems"></a>Sistemas de coordenadas 2D

 Las cámaras de profundidad y de color están asociadas a un sistema de coordenadas 2D independiente. Una coordenada [x,y] se representa en unidades de píxeles donde *x* oscila entre 0 y ancho-1 e *y* oscila entre 0 y alto-1. El ancho y el alto dependen del modo elegido en el que funcionan las cámaras de profundidad y de color. La coordenada de píxeles `[0,0]` corresponde al píxel superior izquierdo de la imagen. Las coordenadas de píxeles pueden ser fracciones que representan coordenadas de subpíxeles.

El sistema de coordenadas 2D se basa en 0, es decir, la coordenada de subpíxeles `[0.0, 0.0]` representa el centro y `[0.5,0.5]` la esquina inferior derecha del píxel, como se muestra a continuación.

   ![Sistema de coordenadas 2D](./media/concepts/concepts-coordinate-systems/coordinate-systems-sdk-2d-system.png)

## <a name="3d-coordinate-systems"></a>Sistemas de coordenadas 3D

Cada cámara, el acelerómetro y el giroscopio están asociados a un sistema espacial de coordenadas 3D independiente.

Los puntos de los sistemas de coordenadas 3D se representan como tripletes de coordenadas [X,Y,Z] métricos con unidades en milímetros.

### <a name="depth-and-color-camera"></a>Cámara de profundidad y de color

El origen `[0,0,0]` se encuentra en el punto focal de la cámara. El sistema de coordenadas se orienta de forma que el eje X positivo apunte a la derecha, el eje Y positivo apunte hacia abajo y el eje Z positivo apunte hacia delante.

La cámara de profundidad está inclinada 6 grados hacia abajo con respecto a la cámara de color, como se muestra a continuación. 

La cámara de profundidad usa dos iluminadores. El iluminador que se usa en los modos de campo de visión estrecho (NFOV) está alineado con la cámara de profundidad, así que no está inclinado. El iluminador que se usa en los modos de campo de visión amplio (WFOV) está inclinado 1,3 grados más hacia abajo con respecto a la cámara de profundidad.

![Convenciones de coordenadas 3D](./media/concepts/concepts-coordinate-systems/coordinate-systems-camera-features.png)

### <a name="gyroscope-and-accelerometer"></a>Giroscopio y acelerómetro

El origen `[0,0,0]` del giroscopio es idéntico al origen de la cámara de profundidad. El origen del acelerómetro coincide con su ubicación física. Los sistemas de coordenadas del acelerómetro y el giroscopio son diestros. El eje X positivo del sistema de coordenadas apunta hacia atrás, el eje Y positivo apunta a la izquierda y el eje Z positivo apunta hacia abajo, como se muestra a continuación.

![Sistema de coordenadas del IMU](./media/concepts/concepts-coordinate-systems/coordinate-systems-gyroscope.png)

## <a name="next-steps"></a>Pasos siguientes

[Más información sobre el SDK del sensor Azure Kinect](about-sensor-sdk.md)
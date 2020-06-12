---
title: 'Notas de la versión de Live Video Analytics on IoT Edge: Azure'
description: En este tema se proporcionan las notas de la versión sobre las mejoras, las versiones, las correcciones de errores y los problemas conocidos de Live Video Analytics on IoT Edge.
ms.topic: conceptual
ms.date: 04/27/2020
ms.openlocfilehash: fd607ee027b46cf386180338440e6530da911302
ms.sourcegitcommit: 223cea58a527270fe60f5e2235f4146aea27af32
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/01/2020
ms.locfileid: "84260328"
---
# <a name="live-video-analytics-on-iot-edge-release-notes"></a>Notas de la versión de Live Video Analytics on IoT Edge

>Reciba notificaciones para volver a visitar esta página y obtener actualizaciones; para ello, copie y pegue esta URL (`https://docs.microsoft.com/api/search/rss?search=%22Live+Video+Analytics+on+IoT+Edge+release+notes%22&locale=en-us`) en el lector de fuentes RSS.

En este artículo se proporciona información acerca de lo siguiente:

* Versiones más recientes
* Problemas conocidos
* Corrección de errores
* Funciones obsoletas

## <a name="june-1-2020"></a>1 de junio de 2020

Esta versión es la primera versión preliminar pública de Live Video Analytics on IoT Edge. La etiqueta de versión es la siguiente:

```
     mcr.microsoft.com/media/live-video-analytics:1.0.0
```

### <a name="supported-functionalities"></a>Funcionalidades admitidas
* Analice secuencias de vídeo en directo con los módulos de inteligencia artificial que prefiera y, de manera opcional, grabe vídeos en su dispositivo perimetral o en la nube.
* Use este módulo en sistemas operativos Linux AMD64 [compatibles](https://docs.microsoft.com/azure/iot-edge/support) con IoT Edge.
* Implemente y configure el módulo a través de IoT Hub mediante Azure Portal o Visual Studio Code.
* Administre [topologías e instancias de grafos](media-graph-concept.md#media-graph-topologies-and-instances) de forma remota o local mediante la llamada a los siguientes métodos directos:

    *   GraphTopologyGet
    *   GraphTopologySet
    *   GraphTopologyDelete
    *   GraphTopologyList
    *   GraphInstanceGet
    *   GraphInstanceSet
    *   GraphInstanceDelete
    *   GraphInstanceList


## <a name="next-steps"></a>Pasos siguientes

[Información general](overview.md)

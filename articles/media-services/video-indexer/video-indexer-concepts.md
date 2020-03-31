---
title: Conceptos de Video Indexer
titleSuffix: Azure Media Services
description: En este artículo se describen algunos conceptos del servicio Video Indexer de Azure Media Services.
services: media-services
author: Juliako
manager: femila
ms.service: media-services
ms.subservice: video-indexer
ms.topic: article
ms.date: 05/15/2019
ms.author: juliako
ms.openlocfilehash: 73dad1db4f44134f871c9f3d6e7edcdd3bd1e2ea
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "74900675"
---
# <a name="video-indexer-concepts"></a>Conceptos de Video Indexer
 
En este artículo se describen algunos conceptos del servicio de Video Indexer.
    
## <a name="summarized-insights"></a>Información resumida

La información resumida contiene una vista agregada de los datos: caras, temas y emociones. Por ejemplo, en lugar de pasar por cada uno de los miles de intervalos de tiempo y comprobar qué caras hay en ellos, la información resumida contiene todas las caras y, para cada una de ellas, los intervalos de tiempo en los que aparece y el porcentaje del tiempo en el que se muestra.

## <a name="time-range-vs-adjusted-time-range"></a>intervalo de tiempo frente a intervalo de tiempo ajustado

TimeRange es el intervalo de tiempo del vídeo original. Intervalo de tiempo relativo a la lista de reproducción actual. Como puede crear una lista de reproducción a partir de diferentes líneas de vídeos distintas, puede grabar un vídeo de una hora y utilizar solo una línea de él, por ejemplo, de 10:00 a 10:15. En ese caso, tendrá una lista de reproducción con 1 línea, donde el intervalo de tiempo es 10:00-10:15, pero adjustedTimeRange es 00:00-00:15.
 
## <a name="blocks"></a>Blocks

Los bloques están pensados para que resulte más fácil el recorrido por los datos. Por ejemplo, el bloque puede dividirse en función de cuándo cambian los oradores o de si hay una pausa larga.

## <a name="next-steps"></a>Pasos siguientes

Para obtener información sobre cómo empezar, consulte [Suscripción y carga del primer vídeo](video-indexer-get-started.md).

## <a name="see-also"></a>Consulte también

[Introducción a Video Indexer](video-indexer-overview.md)

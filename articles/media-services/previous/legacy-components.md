---
title: Componentes heredados de Azure Media Services | Microsoft Docs
description: En este tema se describen los componentes heredados de Azure Media Services.
services: media-services
documentationcenter: ''
author: myoungerman
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/27/2020
ms.author: v-myoung
ms.openlocfilehash: d5a10ccc887124b2bc5595d66baad833a32275cc
ms.sourcegitcommit: 80b9c8ef63cc75b226db5513ad81368b8ab28a28
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/16/2020
ms.locfileid: "90600898"
---
# <a name="azure-media-services-legacy-components"></a>Componentes heredados de Azure Media Services

[!INCLUDE [media services api v2 logo](./includes/v2-hr.md)]

Con el tiempo, se mejoran los componentes de servicio multimedia y se retiran los componentes heredados. Este artículo le ayuda a migrar la aplicación de un componente heredado a un componente actual.
 
## <a name="retirement-plans-of-legacy-components-and-migration-guidance"></a>Planes de retirada de componentes heredados y guía de migración

Los procesadores de multimedia *Windows Azure Media Encoder* (WAME) y *Azure Media Encoder* (AME) han quedado en desuso.

* [Migración de Windows Azure Media Encoder a Media Encoder Standard](migrate-windows-azure-media-encoder.md)
* [Migración de Azure Media Encoder a Media Encoder Standard](migrate-azure-media-encoder.md)

Los siguientes procesadores de multimedia de Media Analytics están en desuso o a punto de estarlo:  
 
|Nombre de procesador de multimedia|Fecha de retirada|Notas adicionales|
|---|---|
|Azure Media Indexer 2|1 de enero de 2020|[Video Indexer de Azure Media Services](../video-indexer/index.yml) ha reemplazado a este procesador de multimedia. Para más información, consulte [Migración de Azure Media Indexer 2 a Video Indexer de Azure Media Services](migrate-indexer-v1-v2.md).|
|[Azure Media Indexer](media-services-index-content.md)|1 de marzo de 2023|[Video Indexer de Azure Media Services](../video-indexer/index.yml) reemplazará a este procesador de multimedia. Para más información, consulte [Migración de Azure Media Indexer a Video Indexer de Azure Media Services](migrate-indexer-v1-v2.md).|
|Detección de movimiento|1 de junio de 2020|No hay planes de sustitución en este momento.|
|Resumen de vídeo](media-services-video-summarization.md|1 de junio de 2020|No hay planes de sustitución en este momento.|
|Reconocimiento óptico de caracteres de vídeo|1 de junio de 2020|[Video Indexer de Azure Media Services](../video-indexer/index.yml) ha reemplazado a este procesador de multimedia. Además, considere la posibilidad de usar la [API de Azure Media Services v3](../latest/analyzing-video-audio-files-concept.md). <br/>Consulte [Comparación de los valores predeterminados de Video Indexer y Azure Media Services v3](../video-indexer/compare-video-indexer-with-media-services-presets.md).|
|Detector de caras|1 de junio de 2020|[Video Indexer de Azure Media Services](../video-indexer/index.yml) ha reemplazado a este procesador de multimedia. Además, considere la posibilidad de usar la [API de Azure Media Services v3](../latest/analyzing-video-audio-files-concept.md). <br/>Consulte [Comparación de los valores predeterminados de Video Indexer y Azure Media Services v3](../video-indexer/compare-video-indexer-with-media-services-presets.md).|
|Content Moderator|1 de junio de 2020|[Video Indexer de Azure Media Services](../video-indexer/index.yml) ha reemplazado a este procesador de multimedia. Además, considere la posibilidad de usar la [API de Azure Media Services v3](../latest/analyzing-video-audio-files-concept.md). <br/>Consulte [Comparación de los valores predeterminados de Video Indexer y Azure Media Services v3](../video-indexer/compare-video-indexer-with-media-services-presets.md).|

## <a name="next-steps"></a>Pasos siguientes

[Guía de migración para mover de Media Services v2 a v3](../latest/migrate-from-v2-to-v3.md)

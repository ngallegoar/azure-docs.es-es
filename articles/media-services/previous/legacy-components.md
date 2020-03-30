---
title: Componentes heredados de Azure Media Services | Microsoft Docs
description: En este tema se describen los componentes heredados de Azure Media Services.
services: media-services
documentationcenter: ''
author: juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/27/2020
ms.author: juliako
ms.openlocfilehash: 94a70a1234d902787f248890f0cb538a4ba9c2f9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "77921086"
---
# <a name="azure-media-services-legacy-components"></a>Componentes heredados de Azure Media Services

Con el tiempo, se han realizado mejoras constantes en los componentes de Media Services. Dichas mejoras han dado lugar a la retirada de algunos componentes heredados. Puede encontrar instrucciones sobre cómo migrar la aplicación del componente heredado a un componente actual en los artículos siguientes.
 
## <a name="retirement-plans-of-legacy-components-and-migration-guidance"></a>Planes de retirada de componentes heredados y guía de migración

Anunciamos el desuso de los procesadores de multimedia *Windows Azure Media Encoder* (WAME) y *Azure Media Encoder* (AME). Estos procesadores se retirarán el 31 de marzo de 2020.

* [Migración de Windows Azure Media Encoder a Media Encoder Standard](migrate-windows-azure-media-encoder.md)
* [Migración de Azure Media Encoder a Media Encoder Standard](migrate-azure-media-encoder.md)

También anunciamos la retirada de los siguientes procesadores de multimedia de Media Analytics: 
 
|Nombre de procesador de multimedia|Fecha de retirada|Notas adicionales|
|---|---|
|[Azure Media Indexer 2](media-services-process-content-with-indexer2.md)|1 de enero de 2020|Este procesador de multimedia se reemplazará por [Video Indexer de Azure Media Services](https://docs.microsoft.com/azure/media-services/video-indexer/). Para más información, consulte [Migración de Azure Media Indexer 2 a Video Indexer de Azure Media Services](migrate-indexer-v1-v2.md).|
|[Azure Media Indexer](media-services-index-content.md)|1 de marzo de 2023|Este procesador de multimedia se reemplazará por [Video Indexer de Azure Media Services](https://docs.microsoft.com/azure/media-services/video-indexer/). Para más información, consulte [Migración de Azure Media Indexer a Video Indexer de Azure Media Services](migrate-indexer-v1-v2.md)|
|[Detección de movimiento](media-services-motion-detection.md)|1 de junio de 2020|No hay planes de sustitución en este momento.|
|[Resumen de vídeo](media-services-video-summarization.md)|1 de junio de 2020|No hay planes de sustitución en este momento.|
|[Reconocimiento óptico de caracteres de vídeo](media-services-video-optical-character-recognition.md)|1 de junio de 2020|Este procesador de multimedia se reemplazará por [Video Indexer de Azure Media Services](https://docs.microsoft.com/azure/media-services/video-indexer/). Además, considere la posibilidad de usar la [API de Azure Media Services v3](https://docs.microsoft.com/azure/media-services/latest/analyzing-video-audio-files-concept). <br/>Consulte [Comparación de los valores predeterminados de Video Indexer y Azure Media Services v3](https://docs.microsoft.com/azure/media-services/video-indexer/compare-video-indexer-with-media-services-presets).|
|[Detector de caras](media-services-face-and-emotion-detection.md)|1 de junio de 2020|Este procesador de multimedia se reemplazará por [Video Indexer de Azure Media Services](https://docs.microsoft.com/azure/media-services/video-indexer/). Además, considere la posibilidad de usar la [API de Azure Media Services v3](https://docs.microsoft.com/azure/media-services/latest/analyzing-video-audio-files-concept). <br/>Consulte [Comparación de los valores predeterminados de Video Indexer y Azure Media Services v3](https://docs.microsoft.com/azure/media-services/video-indexer/compare-video-indexer-with-media-services-presets).|
|[Content Moderator](media-services-content-moderation.md)|1 de junio de 2020|Este procesador de multimedia se reemplazará por [Video Indexer de Azure Media Services](https://docs.microsoft.com/azure/media-services/video-indexer/). Además, considere la posibilidad de usar la [API de Azure Media Services v3](https://docs.microsoft.com/azure/media-services/latest/analyzing-video-audio-files-concept). <br/>Consulte [Comparación de los valores predeterminados de Video Indexer y Azure Media Services v3](https://docs.microsoft.com/azure/media-services/video-indexer/compare-video-indexer-with-media-services-presets).|

## <a name="next-steps"></a>Pasos siguientes

[Guía de migración para mover de Media Services v2 a v3](../latest/migrate-from-v2-to-v3.md)

---
title: 'Obtención de vídeos desde la vista personalizada: Bing Custom Search'
titleSuffix: Azure Cognitive Services
description: Información general de alto nivel acerca del uso de Bing Custom Search para obtener vídeos desde la vista personalizada de la web.
services: cognitive-services
author: swhite-msft
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-custom-search
ms.topic: conceptual
ms.date: 09/10/2018
ms.author: scottwhi
ms.openlocfilehash: 4e2bb14519f8ed4e3e4a0e9ac8800957b30229a9
ms.sourcegitcommit: 9eda79ea41c60d58a4ceab63d424d6866b38b82d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/30/2020
ms.locfileid: "96338425"
---
# <a name="get-videos-from-your-custom-view"></a>Obtención de vídeos desde la vista personalizada

> [!WARNING]
> Bing Search APIs se mueve de Cognitive Services a Bing Search Services. A partir del **30 de octubre de 2020**, las nuevas instancias de Bing Search deben aprovisionarse siguiendo el proceso documentado [aquí](/bing/search-apis/bing-web-search/create-bing-search-service-resource).
> El aprovisionamiento de Bing Search APIs con Cognitive Services será posible durante los próximos tres años o hasta que finalice el Contrato Enterprise, lo que suceda primero.
> Puede encontrar instrucciones sobre la migración en [Bing Search Services](/bing/search-apis/bing-web-search/create-bing-search-service-resource).

Bing Custom Videos Search le permite enriquecer su experiencia de búsqueda personalizada con vídeos. De forma similar a los resultados web, la búsqueda personalizada permite buscar vídeos en la lista de sitios web de su instancia. Puede obtener los vídeos mediante la Bing Custom Videos Search API o mediante la característica de interfaz de usuario hospedada. La característica de interfaz de usuario hospedada es fácil usar y se recomienda para que su experiencia de búsqueda esté lista para usarse sin contratiempos. Para información acerca de cómo configurar la interfaz de usuario hospedada para que incluya vídeos, consulte [Configure su experiencia de interfaz de usuario hospedada](hosted-ui.md).

Si desea más control sobre la visualización de los resultados de búsqueda, puede usar la Bing Custom Videos Search API. Dado que una llamada a la API es similar a una llamada a Bing Video Search API, eche un vistazo a [Bing Video Search](../bing-video-search/overview.md) para algunos ejemplos de llamadas a la API. Pero antes de hacerlo, familiarícese con el contenido de [referencia de Bing Custom Videos Search API](/rest/api/cognitiveservices-bingsearch/bing-custom-videos-api-v7-reference). Las principales diferencias son los parámetros de consulta admitidos (debe incluir el parámetro de consulta customConfig) y los puntos de conexión a los que envía las solicitudes.

<!--
## Next steps

[Call your custom view](search-your-custom-view.md)
-->
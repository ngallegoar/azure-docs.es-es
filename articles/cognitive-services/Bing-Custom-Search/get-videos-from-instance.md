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
ms.openlocfilehash: 7dbd9f609944fc63c186ca150d5b9921f3e86622
ms.sourcegitcommit: 3bdeb546890a740384a8ef383cf915e84bd7e91e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2020
ms.locfileid: "93090586"
---
# <a name="get-videos-from-your-custom-view"></a>Obtención de vídeos desde la vista personalizada

> [!WARNING]
> Bing Search APIs se mueve de Cognitive Services a Bing Search Services. A partir del **30 de octubre de 2020** , las nuevas instancias de Bing Search deben aprovisionarse siguiendo el proceso documentado [aquí](https://aka.ms/cogsvcs/bingmove).
> El aprovisionamiento de Bing Search APIs con Cognitive Services será posible durante los próximos tres años o hasta que finalice el Contrato Enterprise, lo que suceda primero.
> Puede encontrar instrucciones sobre la migración en [Bing Search Services](https://aka.ms/cogsvcs/bingmigration).

Bing Custom Videos Search le permite enriquecer su experiencia de búsqueda personalizada con vídeos. De forma similar a los resultados web, la búsqueda personalizada permite buscar vídeos en la lista de sitios web de su instancia. Puede obtener los vídeos mediante la Bing Custom Videos Search API o mediante la característica de interfaz de usuario hospedada. La característica de interfaz de usuario hospedada es fácil usar y se recomienda para que su experiencia de búsqueda esté lista para usarse sin contratiempos. Para información acerca de cómo configurar la interfaz de usuario hospedada para que incluya vídeos, consulte [Configure su experiencia de interfaz de usuario hospedada](hosted-ui.md).

Si desea más control sobre la visualización de los resultados de búsqueda, puede usar la Bing Custom Videos Search API. Dado que una llamada a la API es similar a una llamada a Bing Video Search API, eche un vistazo a [Bing Video Search](../Bing-Video-Search/search-the-web.md) para algunos ejemplos de llamadas a la API. Pero antes de hacerlo, familiarícese con el contenido de [referencia de Bing Custom Videos Search API](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-custom-videos-api-v7-reference). Las principales diferencias son los parámetros de consulta admitidos (debe incluir el parámetro de consulta customConfig) y los puntos de conexión a los que envía las solicitudes.

<!--
## Next steps

[Call your custom view](search-your-custom-view.md)
-->

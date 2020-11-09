---
title: ¿Qué son las versiones de Bing Search API?
titleSuffix: Azure Cognitive Services
description: Use este artículo para conocer las versiones de Bing Search API y cómo habilitar las búsquedas cognitivas en Internet en sus aplicaciones y servicios.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-web-search
ms.topic: conceptual
ms.date: 03/12/2019
ms.author: aahi
ms.openlocfilehash: a982c150349ca31fdf93a38b38910c3572b14d2f
ms.sourcegitcommit: 3bdeb546890a740384a8ef383cf915e84bd7e91e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2020
ms.locfileid: "93078771"
---
# <a name="what-are-the-bing-search-apis"></a>¿Qué son las versiones de Bing Search API?

> [!WARNING]
> Bing Search APIs se mueve de Cognitive Services a Bing Search Services. A partir del **30 de octubre de 2020** , las nuevas instancias de Bing Search deben aprovisionarse siguiendo el proceso documentado [aquí](https://aka.ms/cogsvcs/bingmove).
> El aprovisionamiento de Bing Search APIs con Cognitive Services será posible durante los próximos tres años o hasta que finalice el Contrato Enterprise, lo que suceda primero.
> Puede encontrar instrucciones sobre la migración en [Bing Search Services](https://aka.ms/cogsvcs/bingmigration).

Las versiones de Bing Search API permiten crear aplicaciones y servicios conectados a la web que buscan páginas web, imágenes, noticias, lugares y mucho más sin anuncios. Mediante el envío de solicitudes de búsqueda con las API REST o los SDK de Bing Search, puede obtener información y contenido apropiados para búsquedas web. Use este artículo para conocer las diversas API de búsqueda de Bing y cómo puede integrar las búsquedas cognitivas en sus aplicaciones y servicios. Los precios y límites de velocidad pueden variar según la API.

## <a name="the-bing-web-search-api"></a>Bing Web Search API

[Bing Web Search API](../Bing-Web-Search/overview.md) devuelve páginas web, imágenes, vídeo, noticias y mucho más. Las consultas de búsqueda que se envían a esta API se pueden filtrar para incluir o excluir determinados tipos de contenido.

Considere el uso de Bing Web Search API en aplicaciones que necesitan buscar todos los tipos de contenido web apropiado. Si la aplicación busca un tipo específico de contenido en línea, considere una de las siguientes API de búsqueda:

## <a name="content-specific-bing-search-apis"></a>API de búsqueda de Bing específicas de contenido

Las siguientes API de búsqueda de Bing devuelven contenido específico de la web, como imágenes, noticias, negocios locales y vídeos.

| API de Bing | Description |
| -- | -- |
| [Entity Search](../Bing-Entities-Search/overview.md) | Bing Entity Search API devuelve resultados de búsqueda que contienen entidades, que pueden ser personas, lugares o cosas. En función de la consulta, la API devolverá una o más entidades que satisfagan la consulta de búsqueda. La consulta de búsqueda puede incluir personas importantes, negocios locales, puntos de referencia, destinos y mucho más. |
| [Image Search](../Bing-Image-Search/overview.md) | Bing Image Search API le permite buscar y encontrar imágenes estáticas y animadas de alta calidad, de forma parecida a [Bing.com/images](https://www.Bing.com/images). Puede refinar las búsquedas para incluir o excluir imágenes por atributo, como tamaño, color, licencia y novedad. También puede buscar imágenes populares, cargar imágenes para obtener conclusiones sobre ellas y mostrar vistas previas en miniatura. |
| [News Search](../Bing-News-Search/search-the-web.md) | Bing News Search API le permite buscar noticias, de forma parecida a [Bing.com/news](https://www.Bing.com/news). La API devuelve artículos de noticias de varios orígenes o dominios concretos. Puede buscar entre las categorías para obtener artículos populares, artículos destacados y titulares. |
| [Video Search](../Bing-Video-Search/overview.md) | Bing Video Search API le permite buscar vídeos en la web. Obtenga vídeos populares, contenido relacionado y vistas previas en miniatura. |
| [Visual Search](../Bing-visual-search/overview.md) | Cargue una imagen o use una dirección URL para obtener información detallada sobre ella, como productos visualmente parecidos, imágenes y búsquedas relacionadas. |
 [Local Business Search](../bing-local-business-search/overview.md) | Bing Local Business Search API permite a las aplicaciones buscar información acerca de la ubicación y forma de contacto de empresas locales en función de las consultas de búsqueda. |

## <a name="the-bing-custom-search-api"></a>Bing Custom Search API

La creación de una instancia de búsqueda personalizada con [Bing Custom Search API](../Bing-Custom-Search/overview.md) le permite crear una experiencia de búsqueda centrada únicamente en el contenido y los temas que le interesan. Por ejemplo, después de especificar los dominios, los sitios web y las páginas web específicas que se buscarán en Bing, Bing Custom Search adapta los resultados a ese contenido específico. Puede incorporar las API Bing Custom Autosuggest, Image, y Video Search para personalizar aún más la experiencia de búsqueda.

## <a name="additional-bing-search-apis"></a>Otras versiones de Bing Search API

Las siguientes versiones de Bing Search API le permiten mejorar la experiencia de búsqueda ya que se pueden combinar con otras API de búsqueda de Bing.

| API | Descripción |
| -- | -- |
| [Bing Autosuggest](../Bing-Autosuggest/get-suggested-search-terms.md) | Mejore la experiencia de búsqueda de la aplicación con Bing Autosuggest API gracias a que las búsquedas sugeridas se devuelven en tiempo real.  |
| [Bing Statistics](bing-web-stats.md) | Bing Statistics proporciona análisis de las versiones de Bing Search API que usa la aplicación. Algunos de los análisis disponibles incluyen el volumen de llamadas, las cadenas de consulta principales y la distribución geográfica. |

## <a name="next-steps"></a>Pasos siguientes

* [Detalles de precios](https://azure.microsoft.com/pricing/details/cognitive-services/search-api/) de Bing Search API
* En los [requisitos de uso y visualización de Bing](./use-display-requirements.md) se especifican usos aceptables del contenido y la información adquirida mediante las API de búsqueda de Bing.

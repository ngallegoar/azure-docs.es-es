---
title: 'Navegación por los resultados de una búsqueda: Bing Search API'
titleSuffix: Azure Cognitive Services
description: Aprenda a navegar por los resultados de una búsqueda desde Bing Search API.
services: cognitive-services
author: aahill
manager: nitinme
ms.assetid: 26CA595B-0866-43E8-93A2-F2B5E09D1F3B
ms.service: cognitive-services
ms.subservice: bing-web-search
ms.topic: conceptual
ms.date: 10/31/2019
ms.author: aahi
ms.openlocfilehash: ea883bb294a8769b3c9be1e0eafc2e3e7c811b48
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "73481727"
---
# <a name="how-to-page-through-results-from-the-bing-search-apis"></a>Cómo navegar por los resultados desde Bing Search API

Cuando se envía una llamada a las API Bing Web Search, Bing Custom Search, Bing Image Search, Bing News Search o Bing Video Search, Bing devuelve un subconjunto del número total de resultados que pueden ser pertinentes para la consulta. Para obtener el número total estimado de resultados disponibles, acceda al campo `totalEstimatedMatches` del objeto de respuesta. 

Por ejemplo: 

```json
{
    "_type" : "SearchResponse",
    "webPages" : {
        "webSearchUrl" : "https:\/\/www.bing.com\/cr?IG=3A43CA...",
        "totalEstimatedMatches" : 262000,
        "value" : [...]
    }
}  
```

## <a name="paging-through-search-results"></a>Navegación por los resultados de la búsqueda

Para navegar por los resultados disponibles, use los parámetros de consulta `count` y `offset` al enviar la solicitud.  

> [!NOTE]
>
> * La paginación con las API Bing Video Search, Bing Image Search y Bing News Search solo se aplica a las búsquedas generales en vídeos (`/video/search`), noticias (`/news/search`) e imágenes (`/image/search`). No se admite la navegación por temas populares y categorías.  
> * El campo `TotalEstimatedMatches` es una estimación del número total de resultados de búsqueda de la consulta actual. Al establecer los parámetros `count` y `offset`, esta estimación puede cambiar.

| Parámetro | Descripción                                                                                                                                                                |
|-----------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `count`   | Especifica el número de resultados que se devolverán en la respuesta. Tenga en cuenta que el valor predeterminado de `count` y el número máximo de resultados que puede solicitar varía en función de la API. Estos valores se pueden encontrar en la documentación de referencia en [Pasos siguientes](#next-steps). |
| `offset`  | Especifica el número de resultados que se van a omitir. `offset` está basado en cero y debe ser menor que (`totalEstimatedMatches` - `count`).                                           |

Por ejemplo, si quiere mostrar 15 resultados por página, debe establecer `count` en 15 y `offset` en 0 para obtener la primera página de resultados. Con cada llamada API posterior, `offset` se incrementaría en 15. El ejemplo siguiente solicita 15 páginas web a partir del desplazamiento 45.

```  
GET https://api.cognitive.microsoft.com/bing/v7.0/search?q=sailing+dinghies&count=15&offset=45&mkt=en-us HTTP/1.1  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
Host: api.cognitive.microsoft.com  
```

Si usa el valor `count` predeterminado, solo debe especificar el parámetro de consulta `offset` en las llamadas API.  

```  
GET https://api.cognitive.microsoft.com/bing/v7.0/search?q=sailing+dinghies&offset=45&mkt=en-us HTTP/1.1  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
Host: api.cognitive.microsoft.com  
```

Al usar las API Bing Image y Video, puede usar el valor `nextOffset` para evitar resultados de búsqueda duplicados. Obtenga el valor de los objetos de respuesta `Images` o `Videos` y úselo en las solicitudes con el parámetro `offset`.  

> [!NOTE]
> La API Bing Web Search devuelve resultados de búsqueda que pueden incluir páginas web y pueden incluir imágenes, vídeos y noticias. Cuando navegue por los resultados de la búsqueda desde la API Bing Web Search, solo se paginan [WebPages](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-web-api-v7-reference#webpage) y no otros tipos de respuesta, como imágenes o noticias. Los resultados de la búsqueda de los objetos `WebPage` pueden incluir también los resultados que aparecen en otros tipos de respuesta.
>
> Si usa el parámetro de consulta `responseFilter` sin especificar ningún valor de filtro, no utilice los parámetros `count` y `offset`. 

## <a name="next-steps"></a>Pasos siguientes

* [¿Qué son las API Bing Web Search?](bing-api-comparison.md)
* [Referencia de Bing Web Search API v7](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-web-api-v7-reference)
* [Referencia de la API Bing Custom Search, versión 7](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-custom-search-api-v7-reference)
* [Referencia de la API Bing News Search, versión 7](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-news-api-v7-reference)
* [Referencia de la API Bing Video Search, versión 7](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-video-api-v7-reference)
* [Referencia de la API Bing Image Search, versión 7](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference)

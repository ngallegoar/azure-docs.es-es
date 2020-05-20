---
title: Obtención de imágenes populares con Bing Image Search API
titleSuffix: Azure Cognitive Services
description: Busque las imágenes populares actualmente desde la web con Bing Image Search API.
services: cognitive-services
author: swhite-msft
manager: nitinme
ms.assetid: EAB92D35-5C0B-4A0A-8F49-02DF7FAD44B4
ms.service: cognitive-services
ms.subservice: bing-image-search
ms.topic: conceptual
ms.date: 03/04/2019
ms.author: scottwhi
ms.custom: seodec2018
ms.openlocfilehash: 2936b94d7ba791b1a4e5a9b95aca3ca3ecdb5904
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "66383430"
---
# <a name="get-trending-images-from-the-web"></a>Obtención de imágenes populares de la web

Para obtener las imágenes populares de hoy, envíe la solicitud GET siguiente:  

```
GET https://api.cognitive.microsoft.com/bing/v7.0/images/trending?mkt=en-us HTTP/1.1  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
X-MSEdge-ClientIP: 999.999.999.999  
X-Search-Location: lat:47.60357;long:-122.3295;re:100  
X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>  
Host: api.cognitive.microsoft.com  
```  

Trending Images API solo se admite actualmente en los mercados siguientes:  

- en-US (inglés, Estados Unidos)  
- en-CA (inglés, Canadá)  
- en-AU (inglés, Australia)  
- zh-CN (chino, China)

La respuesta contiene un objeto[TrendingImages](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#trendingimages) que muestra las imágenes por categoría. Use el elemento `title` de la categoría para agrupar las imágenes en la experiencia del usuario. Las categorías pueden cambiar todos los días.  

```json
{
    "_type" : "TrendingImages",  
    "categories" : [{  
        "title" : "Popular people searches",  
        "tiles" : [{  
            "query" : {  
                "text" : "Smith",  
                "displayText" : "Mr. Smith",  
                "webSearchUrl" : "https:\/\/www.bing.com\/images\/search?q=smith&FORM=..."
            },  
            "image" : {  
                "id" : "C3C60AE779A054D5CD80D3CACF0F3",  
                "thumbnailUrl" : "https:\/\/tse3.mm.bing.net\/th?id=OIP.M2532...",  
                "contentUrl" : "http:\/\/www.contoso.com.au\/assets\/Uploads\/smith-SH01.jpg",  
                "thumbnail" : {  
                    "width" : 288,  
                    "height" : 300  
                }  
            }  
        },  
        . . .  
        ]  
    },  
    . . .  
    {  
        "title" : "Popular Halloween searches",  
        "tiles" : [{  
            "query" : {  
                "text" : "Halloween costumes for adults",  
                "displayText" : "Halloween costumes for adults",  
                "webSearchUrl" : "https:\/\/www.bing.com\/images\/search?q=Halloween+costumes..."
            },  
            "image" : {  
                "id" : "0F3395F2983003F89DCEE711B55D7FA53E4",  
                "thumbnailUrl" : "https:\/\/tse4.mm.bing.net\/th?id=OIP.Me429c...",  
                "contentUrl" : "http:\/\/images.domain.com\/products\/8179\/1-1\/adult-squirrel...",  
                "thumbnail" : {  
                    "width" : 336,  
                    "height" : 480  
                }  
            }  
        }]  
    }]  
}  
```  

Cada icono contiene una imagen y opciones para obtener imágenes relacionadas. Para obtener las imágenes relacionadas, puede usar la consulta `text` para llamar a [Image Search API](./search-the-web.md) y mostrar las imágenes relacionadas por su cuenta. O bien, puede usar la dirección URL en `webSearchUrl` para llevar al usuario a la página de resultados de búsqueda de imágenes de Bing, que contiene las imágenes relacionadas.

Si llama a Image Search API para obtener las imágenes relacionadas, establezca el parámetro de consulta [id](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#id) en el identificador del campo `id`. Al especificar el identificador, se garantiza que la respuesta contiene la imagen (es la primera imagen de la respuesta) y sus imágenes relacionadas. Además, establezca el parámetro de consulta [q](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference) en el texto del campo `text` del objeto `query`.

En el ejemplo siguiente se muestra cómo usar el identificador de la imagen para obtener las imágenes relacionadas de Mr. Smith en la respuesta anterior de Trending Images API.

```  
GET https://api.cognitive.microsoft.com/bing/v7.0/images/search?q=Smith&id=77FDE4A1C6529A23C7CF0EC073FAA64843E828F2&mkt=en-us HTTP/1.1  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
X-MSEdge-ClientIP: 999.999.999.999  
X-Search-Location: lat:47.60357;long:-122.3295;re:100  
X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>  
Host: api.cognitive.microsoft.com  
```  

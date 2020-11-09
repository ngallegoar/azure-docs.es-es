---
title: Búsqueda de vídeos populares en la Web con Bing Video Search API
titleSuffix: Azure Cognitive Services
description: Se muestra cómo usar Bing Video Search API para buscar vídeos populares en la Web.
services: cognitive-services
author: swhite-msft
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-video-search
ms.topic: conceptual
ms.date: 01/31/2019
ms.author: scottwhi
ms.openlocfilehash: 8232af6cb409628a1066a044a196777ddc46348f
ms.sourcegitcommit: 3bdeb546890a740384a8ef383cf915e84bd7e91e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2020
ms.locfileid: "93098974"
---
# <a name="get-trending-videos-with-the-bing-video-search-api"></a>Obtención de vídeos populares con Bing Video Search API 

> [!WARNING]
> Bing Search APIs se mueve de Cognitive Services a Bing Search Services. A partir del **30 de octubre de 2020** , las nuevas instancias de Bing Search deben aprovisionarse siguiendo el proceso documentado [aquí](https://aka.ms/cogsvcs/bingmove).
> El aprovisionamiento de Bing Search APIs con Cognitive Services será posible durante los próximos tres años o hasta que finalice el Contrato Enterprise, lo que suceda primero.
> Puede encontrar instrucciones sobre la migración en [Bing Search Services](https://aka.ms/cogsvcs/bingmigration).

Bing Video Search API permite buscar vídeos populares de hoy en la Web y en distintas categorías. 

## <a name="get-request"></a>Solicitud GET

Para obtener vídeos populares de hoy en día desde Bing Video Search API, envíe la solicitud GET siguiente:  
  
```cURL
GET https://api.cognitive.microsoft.com/bing/v7.0/videos/trending?mkt=en-us HTTP/1.1
Ocp-Apim-Subscription-Key: 123456789ABCDE  
User-Agent: Mozilla/5.0 (compatible; MSIE 10.0; Windows Phone 8.0; Trident/6.0; IEMobile/10.0; ARM; Touch; NOKIA; Lumia 822)  
X-MSEdge-ClientIP: 999.999.999.999  
X-Search-Location: lat:47.60357;long:-122.3295;re:100  
X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>  
Host: api.cognitive.microsoft.com  
```

## <a name="market-support"></a>Compatibilidad en los mercados

Los mercados siguientes admiten vídeos populares.  
 
-   en-AU (inglés, Australia)  
-   en-CA (inglés, Canadá)  
-   en-GB (inglés, Gran Bretaña)  
-   en-ID (inglés, Indonesia)  
-   en-IE (inglés, Irlanda)  
-   en-IN (inglés, India)  
-   en-NZ (inglés, Nueva Zelanda)  
-   en-PH (inglés, Filipinas)  
-   en-SG (inglés, Singapur)  
-   en-US (inglés, Estados Unidos)  
-   en-WW (inglés, código agregado internacional)  
-   en-ZA (inglés, Sudáfrica)  
-   zh-CN (chino, China)

## <a name="example-json-response"></a>Ejemplo de respuesta JSON  

El ejemplo siguiente muestra una respuesta de la API que contiene vídeos populares, que se enumeran por categoría y subcategoría. La respuesta también contiene vídeos de banner, que son los más populares, y puede proceder de una o de varias categorías.  

```json
{  
    "_type" : "TrendingVideos",  
    "bannerTiles" : [
        {  
            "query" : {  
                "text" : "Hello - Smith",  
                "displayText" : "Hello - Smith",  
                "webSearchUrl" : "https:\/\/www.bing.com\/cr?IG=3E8F5..."
            },  
            "image" : {  
                "description" : "Image from: contosowallpapers.com",  
                "thumbnailUrl" : "https:\/\/tse4.mm.bing.net\/th?id=RsA%2fdPlTmx4zS...",  
                "headLine" : "\"Hello\" is a song by...",  
                "contentUrl" : "http:\/\/www.contosowallpapers.com\/wp-content..."  
            }  
        },  
        . . .  
    ],  
    "categories" : [
        {  
            "title" : "Music videos",  
            "subcategories" : [
                {  
                    "tiles" : [
                        {  
                            "query" : {  
                                "text" : "Song Title - Artist Name",  
                                "displayText" : "Song Title - Artist Name",  
                                "webSearchUrl" : "https:\/\/www.bing.com\/cr?IG=3E8F5..."
                            },  
                            "image" : {  
                                "description" : "Image from: contoso.com",  
                                "thumbnailUrl" : "https:\/\/tse2.mm.bing.net\/th?id=...",  
                                "contentUrl" : "http:\/\/images6.contoso.com\/image..."  
                            }  
                        },  
                        . . .  
                    ],
                    "title" : "Top "  
                },
                {
                    "tiles" : [...],
                    "title" : "Trending"
                },
                . . .
            ],  
        },
        {
            "title" : "Viral videos",
            "subcategories" : [
                {
                    "tiles" : [...],
                    "title" : "Trending"
                },
                . . .
            ],  
        },
        . . .  
    ]  
}  
  
```

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Obtención de información de vídeos](video-insights.md)
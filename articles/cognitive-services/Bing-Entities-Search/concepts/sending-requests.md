---
title: Envío de solicitudes de búsqueda a Bing Entity Search API
titleSuffix: Azure cognitive Services
description: Bing Entity Search API envía una consulta de búsqueda a Bing y obtiene resultados que incluyen entidades y lugares.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-entity-search
ms.topic: conceptual
ms.date: 06/27/2019
ms.author: aahi
ms.openlocfilehash: b290539bf6424895a9685c1e4cdb53d8fb8cfb41
ms.sourcegitcommit: 22da82c32accf97a82919bf50b9901668dc55c97
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/08/2020
ms.locfileid: "94365914"
---
# <a name="sending-search-requests-to-the-bing-entity-search-api"></a>Envío de solicitudes de búsqueda a Bing Entity Search API

> [!WARNING]
> Bing Search APIs se mueve de Cognitive Services a Bing Search Services. A partir del **30 de octubre de 2020** , las nuevas instancias de Bing Search deben aprovisionarse siguiendo el proceso documentado [aquí](https://aka.ms/cogsvcs/bingmove).
> El aprovisionamiento de Bing Search APIs con Cognitive Services será posible durante los próximos tres años o hasta que finalice el Contrato Enterprise, lo que suceda primero.
> Puede encontrar instrucciones sobre la migración en [Bing Search Services](https://aka.ms/cogsvcs/bingmigration).

Bing Entity Search API envía una consulta de búsqueda a Bing y obtiene resultados que incluyen entidades y lugares. Los resultados de lugares incluyen restaurantes, hoteles u otros negocios locales. En el caso de los lugares, la consulta puede especificar el nombre del negocio local o puede solicitar una lista (por ejemplo, restaurantes próximos a mi ubicación). Los resultados de entidad incluyen personas, lugares o cosas. A este contexto pertenecen atracciones turísticas, provincias, regiones o países, etc.

## <a name="the-endpoint"></a>El punto de conexión

Para obtener resultados de búsqueda de entidades y lugares, envíe una solicitud GET al siguiente punto de conexión:  

```
https://api.cognitive.microsoft.com/bing/v7.0/entities
```

Las solicitudes deben usar el protocolo HTTPS.

Se recomienda que todas las solicitudes procedan de un servidor. Al distribuir la clave como parte de una aplicación cliente, existe una mayor probabilidad de que un tercero malintencionado acceda a ella. Además, la realización de llamadas desde un servidor proporciona un único punto de actualización para futuras versiones de la API.

## <a name="specifying-query-parameters-and-headers"></a>Especificación de parámetros y encabezados de consulta

La solicitud debe especificar el parámetro de consulta [q](/rest/api/cognitiveservices-bingsearch/bing-entities-api-v7-reference#query), que contiene el término de búsqueda del usuario. La solicitud debe especificar también el parámetro de consulta [mkt](/rest/api/cognitiveservices-bingsearch/bing-entities-api-v7-reference#mkt), que identifica el mercado de donde desea que procedan los resultados. Para ver una lista de parámetros de consulta opcionales, consulte [Query Parameters](/rest/api/cognitiveservices-bingsearch/bing-entities-api-v7-reference#query-parameters) (Parámetros de consulta). Codifique en URL de todos los parámetros de consulta.  
  
La solicitud debe especificar el encabezado [Ocp-Apim-Subscription-Key](/rest/api/cognitiveservices-bingsearch/bing-entities-api-v7-reference#subscriptionkey). Aunque es opcional, se recomienda especificar los encabezados siguientes:  
  
-   [User-Agent](/rest/api/cognitiveservices-bingsearch/bing-entities-api-v7-reference#useragent)  
-   [X-MSEdge-ClientID](/rest/api/cognitiveservices-bingsearch/bing-entities-api-v7-reference#clientid)  
-   [X-MSEdge-ClientIP](/rest/api/cognitiveservices-bingsearch/bing-entities-api-v7-reference#clientip)  
-   [X-Search-Location](/rest/api/cognitiveservices-bingsearch/bing-entities-api-v7-reference#location)  

Los encabezados de IP y ubicación del cliente son importantes para devolver contenido relativo a la ubicación.  

Para ver una lista de todos los encabezados de solicitud y respuesta, consulte [Encabezados](/rest/api/cognitiveservices-bingsearch/bing-entities-api-v7-reference#headers).

## <a name="the-request"></a>Solicitud

A continuación se muestra una solicitud de entidades que incluye todos los encabezados y parámetros de consulta sugeridos. 

```  
GET https://api.cognitive.microsoft.com/bing/v7.0/entities?q=mount+rainier&mkt=en-us HTTP/1.1  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
User-Agent: Mozilla/5.0 (compatible; MSIE 10.0; Windows Phone 8.0; Trident/6.0; IEMobile/10.0; ARM; Touch; NOKIA; Lumia 822)  
X-Search-ClientIP: 999.999.999.999  
X-Search-Location: lat:47.60357;long:-122.3295;re:100  
X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>  
Host: api.cognitive.microsoft.com  
```  

Si es la primera vez que llama a cualquiera de las API de Bing, no incluya el encabezado de identificador de cliente. Solo debe incluir el identificador de cliente si se ha llamado previamente a una API de Bing y Bing ha devuelto un identificador de cliente para esa combinación de usuario y dispositivo.

## <a name="the-response"></a>La respuesta

A continuación se muestra la respuesta a la solicitud anterior. En el ejemplo también se muestran los encabezados de respuesta específicos de Bing. Para información sobre el objeto de respuesta, consulte [SearchResponse](/rest/api/cognitiveservices-bingsearch/bing-entities-api-v7-reference#searchresponse).

[!INCLUDE [cognitive-services-bing-url-note](../../../../includes/cognitive-services-bing-url-note.md)]

```json
BingAPIs-TraceId: 76DD2C2549B94F9FB55B4BD6FEB6AC
X-MSEdge-ClientID: 1C3352B306E669780D58D607B96869
BingAPIs-Market: en-US

{
    "_type" : "SearchResponse",
    "queryContext" : {
        "originalQuery" : "mount rainier"
    },
    "entities" : {
        "queryScenario" : "DominantEntity",
        "value" : [{
            "contractualRules" : [{
                "_type" : "ContractualRules\/LicenseAttribution",
                "targetPropertyName" : "description",
                "mustBeCloseToContent" : true,
                "license" : {
                    "name" : "CC-BY-SA",
                    "url" : "http:\/\/creativecommons.org\/licenses\/by-sa\/3.0\/"
                },
                "licenseNotice" : "Text under CC-BY-SA license"
            },
            {
                "_type" : "ContractualRules\/LinkAttribution",
                "targetPropertyName" : "description",
                "mustBeCloseToContent" : true,
                "text" : "en.wikipedia.org",
                "url" : "http:\/\/en.wikipedia.org\/wiki\/Mount_Rainier"
            },
            {
                "_type" : "ContractualRules\/MediaAttribution",
                "targetPropertyName" : "image",
                "mustBeCloseToContent" : true,
                "url" : "http:\/\/en.wikipedia.org\/wiki\/Mount_Rainier"
            }],
            "webSearchUrl" : "https:\/\/www.bing.com\/search?q=Mount%20Rainier...",
            "name" : "Mount Rainier",
            "image" : {
                "name" : "Mount Rainier",
                "thumbnailUrl" : "https:\/\/www.bing.com\/th?id=A21890c0e1f...",
                "provider" : [{
                    "_type" : "Organization",
                    "url" : "http:\/\/en.wikipedia.org\/wiki\/Mount_Rainier"
                }],
                "hostPageUrl" : "http:\/\/upload.wikimedia.org\/wikipedia...",
                "width" : 110,
                "height" : 110
            },
            "description" : "Mount Rainier, Mount Tacoma, or Mount Tahoma is the highest...",
            "entityPresentationInfo" : {
                "entityScenario" : "DominantEntity",
                "entityTypeHints" : ["Attraction"],
                "entityTypeDisplayHint" : "Mountain"
            },
            "bingId" : "9ae3e6ca-81ea-6fa1-ffa0-42e1d78906"
        }]
    }
}
```


## <a name="next-steps"></a>Pasos siguientes

* [Búsqueda de entidades con Bing Entity API](search-for-entities.md)
* [Requisitos de uso y visualización de Bing API](../../bing-web-search/use-display-requirements.md)
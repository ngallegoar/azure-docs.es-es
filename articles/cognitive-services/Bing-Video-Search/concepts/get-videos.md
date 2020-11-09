---
title: Búsqueda de vídeos mediante Bing Video Search API
titleSuffix: Azure Cognitive Services
description: Aunque la API Bing Video Search busca y devuelve vídeos pertinentes de Internet, también proporciona varias características para la recuperación de vídeos inteligente y centrada en la web.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-video-search
ms.topic: conceptual
ms.date: 06/24/2019
ms.author: aahi
ms.openlocfilehash: 8074b2411a053c8b55457f9ace716481f6b107a5
ms.sourcegitcommit: 3bdeb546890a740384a8ef383cf915e84bd7e91e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2020
ms.locfileid: "93102231"
---
# <a name="search-for-videos-with-the-bing-video-search-api"></a>Búsqueda de vídeos con Bing Video Search API

> [!WARNING]
> Bing Search APIs se mueve de Cognitive Services a Bing Search Services. A partir del **30 de octubre de 2020** , las nuevas instancias de Bing Search deben aprovisionarse siguiendo el proceso documentado [aquí](https://aka.ms/cogsvcs/bingmove).
> El aprovisionamiento de Bing Search APIs con Cognitive Services será posible durante los próximos tres años o hasta que finalice el Contrato Enterprise, lo que suceda primero.
> Puede encontrar instrucciones sobre la migración en [Bing Search Services](https://aka.ms/cogsvcs/bingmigration).

Bing Video Search API facilita la integración de las funcionalidades cognitivas de búsqueda de noticias de Bing en sus aplicaciones. Aunque la API principalmente busca y devuelve vídeos pertinentes de Internet, también proporciona varias características para la recuperación de vídeos inteligente y centrada en la web.

## <a name="getting-videos"></a>Obtención de vídeos

Para obtener vídeos relacionados con el término de búsqueda del usuario desde la web, envíe la solicitud GET siguiente:

```http
GET https://api.cognitive.microsoft.com/bing/v7.0/videos/search?q=sailing+dinghies&mkt=en-us HTTP/1.1
Ocp-Apim-Subscription-Key: 123456789ABCDE
User-Agent: Mozilla/5.0 (compatible; MSIE 10.0; Windows Phone 8.0; Trident/6.0; IEMobile/10.0; ARM; Touch; NOKIA; Lumia 822)
X-Search-ClientIP: 999.999.999.999
X-Search-Location: lat:47.60357;long:-122.3295;re:100
X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>
Host: api.cognitive.microsoft.com
```

Todas las solicitudes deben realizarse desde un servidor.

Si es la primera vez que llama a cualquiera de las API de Bing, no incluya el encabezado de identificador de cliente. Solo debe incluir el identificador de cliente si se ha llamado previamente a una API de Bing y Bing ha devuelto un identificador de cliente para esa combinación de usuario y dispositivo.

Para obtener vídeos de un dominio específico, utilice el operador de consulta [site:](https://msdn.microsoft.com/library/ff795613.aspx).

```http
GET https://api.cognitive.microsoft.com/bing/v7.0/videos/search?q=sailing+dinghies+site:contososailing.com&mkt=en-us HTTP/1.1
```

La respuesta contiene el objeto [Videos](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-video-api-v7-reference#videos) que contiene una lista de vídeos que para Bing eran importantes para la consulta. Cada objeto [Video](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-video-api-v7-reference#video) de la lista incluye la dirección URL del vídeo, su duración, sus dimensiones y su formato de codificación, entre otros atributos. El objeto de vídeo también incluye la dirección URL de una miniatura del vídeo y sus dimensiones.

```json
{
    "_type" : "Videos",
    "webSearchUrl" : "https:\/\/www.bing.com\/cr?IG=81EF7545...",
    "totalEstimatedMatches" : 1000,
    "value" : [
        {
            "name" : "How to sail - What to Wear for Dinghy Sailing",
            "description" : "An informative video on what to wear when...",
            "webSearchUrl" : "https:\/\/www.bing.com\/cr?IG=81EF7...",
            "thumbnailUrl" : "https:\/\/tse4.mm.bing.net\/th?id=OVP.DYW...",
            "datePublished" : "2014-03-04T11:51:53",
            "publisher" : [
                {
                    "name" : "Fabrikam"
                }
            ],
            "creator" : 
            {
                "name" : "Marcus Appel"
            },
            "contentUrl" : "https:\/\/www.fabrikam.com\/watch?v=vzmPjZ--g",
            "hostPageUrl" : "https:\/\/www.bing.com\/cr?IG=81EF7545D569...",
            "encodingFormat" : "h264",
            "hostPageDisplayUrl" : "https:\/\/www.fabrikam.com\/watch?v=vzmPjZ--g",
            "width" : 1280,
            "height" : 720,
            "duration" : "PT2M47S",
            "motionThumbnailUrl" : "https:\/\/tse3.mm.bing.net\/th?id=OM.Y62...",
            "embedHtml" : "<iframe width=\"1280\" height=\"720\" src=\"https:...><\/iframe>",
            "allowHttpsEmbed" : true,
            "viewCount" : 8743,
            "thumbnail" : 
            {
                "width" : 300,
                "height" : 168
            },
            "videoId" : "6DB795E11A6E3CBAAD636DB795E113CBAAD63",
            "allowMobileEmbed" : true,
            "isSuperfresh" : false
        },
        ...
    ],
    "queryExpansions" : [...],
    "nextOffsetAddCount" : 0,
    "pivotSuggestions" : [...]
}
```

## <a name="video-thumbnails"></a>Miniaturas de vídeo

Puede mostrar todas las miniaturas de vídeo o un subconjunto de ellas que devuelva Bing Video Search API. Si muestra un subconjunto, proporcione al usuario la opción para ver los vídeos restantes. Como parte de los [requisitos de uso y visualización](../UseAndDisplayRequirements.md) de las API de Bing, debe mostrar los vídeos en el orden indicado en la respuesta. Para obtener información acerca de cómo cambiar el tamaño de la miniatura, consulte [Cambio de tamaño y recorte de miniaturas](../../bing-web-search/resize-and-crop-thumbnails.md). 

A medida que el usuario vaya desplazando el mouse sobre la miniatura, puede usar [motionThumbnailUrl](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-video-api-v7-reference#video-motionthumbnailurl) para reproducir una versión en miniatura del vídeo. No olvide atribuir la miniatura del movimiento al mostrarlo.

<!-- Removing until the images can be sanitized.
![Motion thumbnail of a video](../bing-web-search/media/cognitive-services-bing-web-api/bing-web-video-motion-thumbnail.PNG)
-->

Al hacer clic en una miniatura, hay tres opciones para visualizar el vídeo:

- Use [hostPageUrl](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-video-api-v7-reference#video-hostpageurl) para ver el vídeo en el sitio web del host (por ejemplo, YouTube).
- Utilice [webSearchUrl](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-video-api-v7-reference#video-websearchurl) para ver el vídeo en el explorador de vídeos de Bing.
- Use [embdedHtml](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-video-api-v7-reference#video-embedhtml) para insertar el vídeo en su propia experiencia. 

Asegúrese de utilizar el publicador y el creador para atribuir el vídeo al reproducirlo.

Para obtener más información sobre el uso de [videoId](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-video-api-v7-reference#video-videoid) para obtener información detallada del vídeo, consulte [Información de vídeos](../video-insights.md).

## <a name="filtering-videos"></a>Filtrado de vídeos

De forma predeterminada, Video Search API devuelve todos los vídeos que están relacionados con la consulta. Si solo quiere vídeos gratuitos o de menos de cinco minutos de duración, debe usar los siguientes parámetros de consulta de filtro:

- [pricing](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-video-api-v7-reference#pricing)&mdash;filtra los vídeos por el precio (por ejemplo, vídeos gratuitos o por los que tiene que pagar).
- [resolution](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-video-api-v7-reference#resolution)&mdash;filtra los vídeos por la resolución (por ejemplo, vídeos con un resolución de 720p o mayor).
- [videoLength](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-video-api-v7-reference#videolength)&mdash;filtra los vídeos por la duración del vídeo (por ejemplo, vídeos de menos de cinco minutos de duración).
- [freshness](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-video-api-v7-reference#freshness)&mdash;filtra los vídeos por antigüedad (por ejemplo, vídeos detectados por Bing en la última semana).

Para obtener vídeos de un dominio específico, incluya el operador de consulta [site:](https://msdn.microsoft.com/library/ff795613.aspx) en la cadena de consulta.

> [!NOTE]
> En función de la consulta, si usa el operador de consulta `site:`, existe la posibilidad de que la respuesta incluya contenido para adultos sin tener en cuenta la configuración [Búsqueda segura](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-video-api-v7-reference#safesearch). Debería usar `site:` solo si es consciente del contenido del sitio y el escenario admite la posibilidad de contenido para adultos.

En el ejemplo siguiente se muestra cómo obtener vídeos gratuitos de ContosoSailing.com con una resolución de 720p o mejor y que Bing ha detectado en el mes pasado.

```http
GET https://api.cognitive.microsoft.com/bing/v7.0/videos/search?q=sailing+dinghies+site:contososailing.com&pricing=free&freshness=month&resolution=720p&mkt=en-us HTTP/1.1
Ocp-Apim-Subscription-Key: 123456789ABCDE
User-Agent: Mozilla/5.0 (compatible; MSIE 10.0; Windows Phone 8.0; Trident/6.0; IEMobile/10.0; ARM; Touch; NOKIA; Lumia 822)
X-MSEdge-ClientIP: 999.999.999.999
X-Search-Location: lat:47.60357;long:-122.3295;re:100
X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>
Host: api.cognitive.microsoft.com
```

## <a name="expanding-the-query"></a>Expansión de la consulta

Si Bing puede expandir la consulta para restringir la búsqueda original, el objeto [Videos](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-video-api-v7-reference#videos) contiene el campo `queryExpansions`. Por ejemplo, si la consulta fuese *Limpieza de canalones* , las consultas expandidas podrían ser: **herramientas** de limpieza de canalones, limpieza de canalones **desde cero** , **máquina** de limpieza de canalones y limpieza de canalones **fácil**.

En el ejemplo siguiente se muestran las consultas expandidas para *Limpieza de canalones*.

```json
{
    "_type" : "Videos",
    "webSearchUrl" : "https:\/\/www.bing.com\/cr?IG=B52FBC5...",
    "totalEstimatedMatches" : 1000,
    "value" : [...],
    "nextOffsetAddCount" : 4,
    "queryExpansions" : [
        {
            "text" : "Gutter Cleaning Tools",
            "displayText" : "Tools",
            "webSearchUrl" : "https:\/\/www.bing.com\/cr?IG=B52FB....",
            "searchLink" : "https:\/\/api.cognitive.microsoft.com\/api\/v5...",
            "thumbnail" : {
                "thumbnailUrl" : "https:\/\/tse4.mm.bing.net\/th?q=Gutter..."
            }
        },
        ...
    ]
    "pivotSuggestions" : [...],
}
```

El campo `queryExpansions` contiene una lista de objetos [Query](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-video-api-v7-reference#query_obj). El campo `text` contiene la consulta expandida y el campo `displayText` contiene el término de expansión. Puede usar los campos e texto y miniatura para mostrar al usuario las cadenas de consulta expandidas en caso de que la cadena de consulta expandida sea realmente lo que está buscando. Ofrezca al usuario la posibilidad de hacer clic en la miniatura y el texto mediante la dirección URL `webSearchUrl` o la dirección URL `searchLink`. Use `webSearchUrl` para enviar al usuario a los resultados de búsqueda de Bing o `searchLink` si proporciona su propia página de resultados.

## <a name="pivoting-the-query"></a>Dinamizar la consulta

Si Bing puede segmentar la consulta de búsqueda original, el objeto [Videos](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-video-api-v7-reference#videos) contiene el campo `pivotSuggestions`. Por ejemplo, si la consulta original fuese *Limpieza de canalones* , Bing podría segmentar la consulta en *limpieza* y *canalones*.

En el ejemplo siguiente se muestran las sugerencias dinámicas para *Limpieza de canalones*.

```json
{
    "_type" : "Videos",
    "webSearchUrl" : "https:\/\/www.bing.com\/cr?IG=B52FBC...",
    "totalEstimatedMatches" : 1000,
    "value" : [...],
    "nextOffsetAddCount" : 0,
    "queryExpansions" : [...],
    "pivotSuggestions" : [
        {
            "pivot" : "cleaning",
            "suggestions" : [
                {
                    "text" : "Gutter Repair",
                    "displayText" : "Repair",
                    "webSearchUrl" : "https:\/\/www.bing.com\/cr?IG=B52...",
                    "searchLink" : "https:\/\/api.cognitive.microsoft.com\/api\/v5\/videos...",
                    "thumbnail" : {
                        "thumbnailUrl" : "https:\/\/tse3.mm.bing.net\/th?q=Gutter..."
                    }
                },
                ...
            ]
        },
        {
            "pivot" : "gutters",
            "suggestions" : [
                {
                    "text" : "Window Cleaning",
                    "displayText" : "Window",
                    "webSearchUrl" : "https:\/\/www.bing.com\/cr?IG=B52FBC59...",
                    "searchLink" : "https:\/\/api.cognitive.microsoft.com\/api\/v5...",
                    "thumbnail" : {
                        "thumbnailUrl" : "https:\/\/tse2.mm.bing.net\/th?q=Window..."
                    }
                },
                ...
            ]
        }
    ]
}
```

Para cada área dinámica, la respuesta contiene una lista de objetos [Query](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-video-api-v7-reference#query_obj) que contienen las consultas sugeridas. El campo `text` contiene la consulta sugerida y el campo `displayText` contiene el término que reemplaza al área dinámica en la consulta original. Por ejemplo, Limpieza de ventanas.

Puede usar los campos `text` y `thumbnail` para mostrar al usuario las cadenas de consulta expandidas en caso de que la cadena de consulta expandida sea realmente lo que están buscando. Ofrezca al usuario la posibilidad de hacer clic en la miniatura y el texto mediante la dirección URL `webSearchUrl` o la dirección URL `searchLink`. Use `webSearchUrl` para enviar al usuario a los resultados de búsqueda de Bing o `searchLink` si proporciona su propia página de resultados.

## <a name="throttling-requests"></a>Solicitudes de limitación

[!INCLUDE [cognitive-services-bing-throttling-requests](../../../../includes/cognitive-services-bing-throttling-requests.md)]

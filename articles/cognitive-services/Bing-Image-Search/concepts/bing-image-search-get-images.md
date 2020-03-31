---
title: 'Obtención de imágenes desde la web: Bing Image Search API'
titleSuffix: Azure Cognitive Services
description: Use Bing Image Search API para buscar y obtener imágenes pertinentes desde la web.
services: cognitive-services
author: aahill
manager: nitinme
ms.assetid: AB1B9898-C94A-4B59-91A1-8623C94BA3D4
ms.service: cognitive-services
ms.subservice: bing-image-search
ms.topic: conceptual
ms.date: 03/04/2019
ms.author: aahi
ms.openlocfilehash: 309bbca762149f8804742d9ef02d4c3e8dfcdc6b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "67542757"
---
# <a name="get-images-from-the-web-with-the-bing-image-search-api"></a>Obtención de imágenes desde la web con Bing Image Search API

Cuando use la API REST de Bing Image Search, puede enviar la solicitud GET siguiente para obtener de la web imágenes relacionadas con el término de búsqueda:

```http
GET https://api.cognitive.microsoft.com/bing/v7.0/images/search?q=sailing+dinghies&mkt=en-us HTTP/1.1
Ocp-Apim-Subscription-Key: 123456789ABCDE
X-MSEdge-ClientIP: 999.999.999.999
X-Search-Location: lat:47.60357;long:-122.3295;re:100
X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>
Host: api.cognitive.microsoft.com
```

Use el parámetro de consulta [q](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#query) para el término de búsqueda con codificación URL. Por ejemplo, si escribe *sailing dinghies*, establezca `q` en `sailing+dinghies` o `sailing%20dinghies`.

> [!IMPORTANT]
> * Todas las solicitudes se deben realizar desde un servidor y no desde un cliente.
> * Si es la primera vez que llama a cualquiera de las API de búsqueda de Bing, no incluya el encabezado de identificador de cliente. Solo debe incluir el identificador de cliente si se ha llamado previamente a una API de Bing que ha devuelto un identificador de cliente para esa combinación de usuario y dispositivo.

## <a name="get-images-from-a-specific-web-domain"></a>Obtención de imágenes de un dominio web específico

Para obtener imágenes de un dominio específico, utilice el operador de consulta [site:](https://msdn.microsoft.com/library/ff795613.aspx).

```http
GET https://api.cognitive.microsoft.com/bing/v7.0/images/search?q=sailing+dinghies+site:contososailing.com&mkt=en-us HTTP/1.1
```

> [!NOTE]
> Las respuestas a las consultas que utilizan el operador `site:` pueden incluir contenido para adultos, independientemente de la configuración de [safeSearch](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#safesearch). Use `site:` solo si es consciente del contenido del dominio.

## <a name="filter-images"></a>Filtrado de imágenes

 De forma predeterminada, Image Search API devuelve todas las imágenes que están relacionadas con la consulta. Si quiere filtrar las imágenes que devuelve Bing (por ejemplo, para devolver solo imágenes con un fondo transparente o imágenes de un tamaño específico), use los siguientes parámetros de consulta:

* [aspect](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#aspect): filtra las imágenes según la relación de aspecto (por ejemplo, imágenes de pantalla estándar o extendida).
* [color](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#color): filtra las imágenes por color dominante o blanco y negro.
* [freshness](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#freshness): filtra las imágenes por edad (por ejemplo, imágenes detectadas por Bing en la última semana).
* [height](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#height), [width](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#width): filtra las imágenes por ancho y alto.
* [imageContent](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#imagecontent): filtra las imágenes por contenido (por ejemplo, las imágenes que muestran solo la cara de una persona).
* [imageType](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#imagetype): filtra las imágenes por tipo (por ejemplo, imágenes prediseñadas, GIF animados o fondos transparentes).
* [license](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#license): filtra las imágenes por el tipo de licencia asociado al sitio.
* [size](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#size): filtra las imágenes por tamaño, como las pequeñas imágenes de hasta 200 x 200 píxeles.

Para obtener imágenes de un dominio específico, utilice el operador de consulta [site:](https://msdn.microsoft.com/library/ff795613.aspx).

En el ejemplo siguiente se muestra cómo obtener imágenes pequeñas de ContosoSailing.com que Bing ha detectado en la última semana.  

```http
GET https://api.cognitive.microsoft.com/bing/v7.0/images/search?q=sailing+dinghies+site:contososailing.com&size=small&freshness=week&mkt=en-us HTTP/1.1  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
X-MSEdge-ClientIP: 999.999.999.999  
X-Search-Location: lat:47.60357;long:-122.3295;re:100  
X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>  
Host: api.cognitive.microsoft.com  
```

## <a name="bing-image-search-response-format"></a>Formato de respuesta de Bing Image Search

El mensaje de respuesta de Bing contiene la respuesta [Images](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#images) que contiene una lista de imágenes que Cognitive Services ha determinado como pertinentes para la consulta. Cada objeto [Image](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#image) de la lista incluye la siguiente información sobre la imagen: la dirección URL, su tamaño, sus dimensiones, su formato de codificación, la dirección URL de una vista en miniatura de la imagen y las dimensiones de la miniatura.

> [!NOTE]
> * Debe mostrar las imágenes en el orden proporcionado en la respuesta.
> * Como los formatos y los parámetros de dirección URL están sujetos a cambios sin previo aviso, use todas las direcciones URL tal y como están. No debe tomar dependencias en el formato o los parámetros de dirección URL, excepto donde se indique.

```json
{
    "name": "Rich Passage Sailing Dinghy",
    "webSearchUrl": "https:\/\/www.bing.com\/cr?IG=73118C8B4E3...",
    "thumbnailUrl": "https:\/\/tse1.mm.bing.net\/th?id=OIP.GNarK7m...",
    "datePublished": "2011-10-29T11:26:00",
    "contentUrl": "http:\/\/www.bing.com\/cr?IG=73118C8B4E3D4C3...",
    "hostPageUrl": "http:\/\/www.bing.com\/cr?IG=73118C8B4E3D4C3687...",
    "contentSize": "79239 B",
    "encodingFormat": "jpeg",
    "hostPageDisplayUrl": "en.contoso.org\/wiki\/File:Rich_Passage...",
    "width": 526,
    "height": 688,
    "thumbnail": {
        "width": 229,
        "height": 300
    },
    "imageInsightsToken": "ccid_GNarK7ma*mid_CCF85447ADA6...",
    "insightsSourcesSummary": {
        "shoppingSourcesCount": 0,
        "recipeSourcesCount": 0
    },
    "imageId": "CCF85447ADA6FFF9E96E7DF0B796F7A86E34593",
    "accentColor": "376094"
},
```

Cuando se llama a Bing Image Search API, Bing devuelve una lista de resultados. La lista es un subconjunto del número total de resultados que están relacionados con la consulta. El campo `totalEstimatedMatches` de la respuesta contiene una estimación del número de imágenes que están disponibles para ver. Para obtener más información acerca de cómo desplazarse por las páginas del resto de las imágenes, consulte [Paginación de imágenes](../paging-images.md).

## <a name="next-steps"></a>Pasos siguientes

Si no ha probado Bing Image Search API antes, consulte una [guía de inicio rápido](../quickstarts/csharp.md). Si busca algo más complejo, consulte el tutorial para crear una [aplicación web de una sola página](../tutorial-bing-image-search-single-page-app.md).

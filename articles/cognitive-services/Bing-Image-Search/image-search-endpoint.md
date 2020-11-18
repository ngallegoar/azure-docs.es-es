---
title: Puntos de conexión para Bing Image Search API
titleSuffix: Azure Cognitive Services
description: Image Search API incluye tres puntos de conexión. El punto de conexión 1 devuelve imágenes de la Web. El punto de conexión 2 devuelve ImageInsights. El punto de conexión 3 devuelve imágenes populares.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-image-search
ms.topic: conceptual
ms.date: 03/04/2019
ms.author: aahi
ms.openlocfilehash: 67fec77136f5d593279be2846e63c51b60e16bb4
ms.sourcegitcommit: 1cf157f9a57850739adef72219e79d76ed89e264
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/13/2020
ms.locfileid: "94593525"
---
# <a name="endpoints-for-the-bing-image-search-api"></a>Puntos de conexión para Bing Image Search API

> [!WARNING]
> Bing Search APIs se mueve de Cognitive Services a Bing Search Services. A partir del **30 de octubre de 2020**, las nuevas instancias de Bing Search deben aprovisionarse siguiendo el proceso documentado [aquí](/bing/search-apis/bing-web-search/create-bing-search-service-resource).
> El aprovisionamiento de Bing Search APIs con Cognitive Services será posible durante los próximos tres años o hasta que finalice el Contrato Enterprise, lo que suceda primero.
> Puede encontrar instrucciones sobre la migración en [Bing Search Services](/bing/search-apis/bing-web-search/create-bing-search-service-resource).

**Image Search API** incluye tres puntos de conexión.  El punto de conexión 1 devuelve imágenes de la web en función de una consulta. El punto de conexión 2 devuelve [ImageInsights](/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#imageinsightsresponse).  El punto de conexión 3 devuelve imágenes populares.

## <a name="endpoints"></a>Puntos de conexión

Para obtener resultados de imágenes con Bing API, envíe una solicitud a uno de los siguientes puntos de conexión. Use los encabezados y parámetros de dirección URL para definir especificaciones adicionales.

**Punto de conexión 1:** devuelve imágenes relacionadas con la consulta de búsqueda del usuario definida por `?q=""`.
```
GET https://api.cognitive.microsoft.com/bing/v7.0/images/search
```

**Punto de conexión 2:** devuelve información detallada sobre una imagen, ya sea mediante `GET` o `POST`.
```
 GET or POST https://api.cognitive.microsoft.com/bing/v7.0/images/details
```
Una solicitud GET devuelve información detallada sobre una imagen, como páginas web que incluyen la imagen. Incluya el parámetro [insightsToken](/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#insightstoken) con una solicitud `GET`.

O bien, puede incluir una imagen binaria en el cuerpo de una solicitud `POST` y establecer el parámetro [modules](/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#modulesrequested) en `RecognizedEntities`. Esto devolverá [insightsToken](/rest/api/cognitiveservices-bingsearch/bing-images-api-v5-reference#insightstoken) que se utilizará como parámetro en una solicitud `GET` subsiguiente, que devuelve información sobre las personas de la imagen.  Establezca `modules` en `All` para obtener todos los detalles, excepto `RecognizedEntities` en los resultados de `POST` sin realizar otra llamada mediante `insightsToken`.


**Punto de conexión 3:** devuelve imágenes que son populares en función de las solicitudes de búsqueda que realizan otros usuarios. Las imágenes se dividen en categorías diferentes, por ejemplo, en función de personas o eventos destacables.
```
GET https://api.cognitive.microsoft.com/bing/v7.0/images/trending
```

Para una lista de los mercados que admiten imágenes populares, consulte [Imágenes populares](./trending-images.md).

Para más información sobre encabezados, parámetros, códigos de mercado, objetos de respuesta, errores etc., consulte la referencia [Bing Image Search API v7](/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference).
## <a name="response-json"></a>Respuesta JSON
La respuesta a una solicitud de búsqueda de imágenes incluye los resultados como objetos JSON. Para obtener ejemplos de análisis de los resultados, consulte el [tutorial](./tutorial-bing-image-search-single-page-app.md) y el [código fuente](./tutorial-bing-image-search-single-page-app.md).

## <a name="next-steps"></a>Pasos siguientes
Las API de **Bing** admiten acciones de búsqueda que devuelven resultados según su tipo.  Todos los puntos de conexión de búsqueda devuelven resultados como objetos de respuesta JSON.  Todos los puntos de conexión admiten consultas que devuelven un idioma o una ubicación en concreto por longitud, latitud y radio de búsqueda.

Para una información completa acerca de los parámetros admitidos por cada punto de conexión, consulte las páginas de referencia de cada tipo.
Para obtener ejemplos de solicitudes básicas mediante Image Search API, consulte los [inicios rápidos de Image Search](./overview.md).
---
title: Puntos de conexión de Bing News Search
titleSuffix: Azure Cognitive Services
description: En este artículo se proporciona un resumen de los puntos de conexión de la API de búsqueda de noticias; noticias, noticias más importantes y noticias sobre tendencias.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-news-search
ms.topic: conceptual
ms.date: 1/10/2019
ms.author: aahi
ms.openlocfilehash: 8c724925a12535c561b035296e374691f3fb2689
ms.sourcegitcommit: 3bdeb546890a740384a8ef383cf915e84bd7e91e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2020
ms.locfileid: "93098355"
---
# <a name="bing-news-search-api-endpoints"></a>Puntos de conexión de Bing News Search API

> [!WARNING]
> Bing Search APIs se mueve de Cognitive Services a Bing Search Services. A partir del **30 de octubre de 2020** , las nuevas instancias de Bing Search deben aprovisionarse siguiendo el proceso documentado [aquí](https://aka.ms/cogsvcs/bingmove).
> El aprovisionamiento de Bing Search APIs con Cognitive Services será posible durante los próximos tres años o hasta que finalice el Contrato Enterprise, lo que suceda primero.
> Puede encontrar instrucciones sobre la migración en [Bing Search Services](https://aka.ms/cogsvcs/bingmigration).

**News Search API** devuelve artículos de noticias, páginas web, imágenes, vídeos y [entidades](https://docs.microsoft.com/azure/cognitive-services/bing-entities-search/search-the-web). Las entidades contienen información de resumen sobre una persona, lugar o tema.

## <a name="endpoints"></a>Puntos de conexión

Para obtener nuevos resultados de búsqueda con Bing News Search API, envíe una solicitud `GET` a uno de los siguientes puntos de conexión. Los encabezados y parámetros de dirección URL definen especificaciones adicionales.

### <a name="news-items-by-search-query"></a>Elementos nuevos por consulta de búsqueda

```
GET https://api.cognitive.microsoft.com/bing/v7.0/news/search
```

Devuelve nuevos elementos según una consulta de búsqueda. Si la consulta de búsqueda está vacía, la API devolverá los principales elementos de noticias de categorías diferentes. Para enviar una consulta por URL, codifique el término de búsqueda y anéxelo al parámetro `q=""`. Para información sobre disponibilidad, consulte los [países o regiones y mercados admitidos](language-support.md#supported-markets-for-news-search-endpoint).

### <a name="top-news-items-by-category"></a>Principales elementos de noticias por categoría

```
GET https://api.cognitive.microsoft.com/bing/v7.0/news  
```

Devuelve los principales elementos de noticias por categoría. En concreto, puede solicitar los artículos principales sobre negocios, deportes o entretenimiento mediante `category=business`, `category=sports` o `category=entertainment`.  El parámetro `category` solo se puede usar con la dirección URL `/news`. Existen algunos requisitos formales para especificar las categorías; consulte `category` en la documentación de [parámetros de consulta](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-news-api-v7-reference#query-parameters). Para enviar una consulta por URL, codifique el término de búsqueda y anéxelo al parámetro `q=""`. Para información sobre disponibilidad, consulte los [países o regiones y mercados admitidos](language-support.md#supported-markets-for-news-endpoint).

### <a name="trending-news-topics"></a>Temas de noticias de tendencia 

```
GET https://api.cognitive.microsoft.com/bing/v7.0/news/trendingtopics
```

Devuelve los temas de noticias que actualmente son tendencia en las redes sociales. Cuando se incluye la opción `/trendingtopics`, la búsqueda de Bing omite otros parámetros, como `freshness` y `?q=""`. Para información sobre disponibilidad, consulte los [países o regiones y mercados admitidos](language-support.md#supported-markets-for-news-trending-endpoint).

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre encabezados, parámetros, códigos de mercado, objetos de respuesta, errores etc., consulte la referencia [Bing News Search API v7](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-news-api-v7-reference).

Para una información completa acerca de los parámetros admitidos por cada punto de conexión, consulte las páginas de referencia de cada tipo.
Para obtener ejemplos de solicitudes básicas con News Search API, consulte los [inicios rápidos de Bing News Search](https://docs.microsoft.com/azure/cognitive-services/bing-news-search).

---
title: Punto de conexión de Web Search
titleSuffix: Azure Cognitive Services
description: Para obtener los resultados de la búsqueda web, envíe una solicitud `GET` al punto de conexión siguiente. Los encabezados y parámetros de dirección URL definen especificaciones adicionales.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-web-search
ms.topic: conceptual
ms.date: 11/14/2018
ms.author: aahi
ms.openlocfilehash: c882e3e4d0cd6ba594a700f4fd53c14103a8d1d1
ms.sourcegitcommit: 8a1ba1ebc76635b643b6634cc64e137f74a1e4da
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/09/2020
ms.locfileid: "94381259"
---
# <a name="web-search-endpoint"></a>Punto de conexión de Web Search

> [!WARNING]
> Bing Search APIs se mueve de Cognitive Services a Bing Search Services. A partir del **30 de octubre de 2020** , las nuevas instancias de Bing Search deben aprovisionarse siguiendo el proceso documentado [aquí](/bing/search-apis/bing-web-search/create-bing-search-service-resource).
> El aprovisionamiento de Bing Search APIs con Cognitive Services será posible durante los próximos tres años o hasta que finalice el Contrato Enterprise, lo que suceda primero.
> Puede encontrar instrucciones sobre la migración en [Bing Search Services](/bing/search-apis/bing-web-search/create-bing-search-service-resource).

**Web Search API** devuelve páginas web, noticias, imágenes, vídeos y [entidades](../bing-entities-search/overview.md). Las entidades contienen información de resumen sobre una persona, lugar o tema.

## <a name="endpoint"></a>Punto de conexión

Para obtener resultados de búsqueda web mediante la API de Bing, envíe una solicitud `GET` al punto de conexión siguiente. Los encabezados y parámetros de dirección URL definen especificaciones adicionales.

**Punto de conexión** : devuelve resultados web relacionados con la consulta de búsqueda del usuario definida por `?q=""`.

```http
GET https://api.cognitive.microsoft.com/bing/v7.0/search
```

Punto de conexión: para más información sobre encabezados, parámetros, códigos de mercado, objetos de respuesta, errores, etc., consulte la referencia [Bing Web API v7](/rest/api/cognitiveservices-bingsearch/bing-web-api-v7-reference).

## <a name="response-json"></a>Respuesta JSON

La respuesta a una solicitud de búsqueda web incluye todos los resultados como objetos JSON. Para analizar el resultado se necesitan procedimientos que controlen los elementos de cada tipo. Consulte el [tutorial](./tutorial-bing-web-search-single-page-app.md) y el [código fuente](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/tree/master/Tutorials/Bing-Web-Search) para obtener ejemplos.

## <a name="next-steps"></a>Pasos siguientes

Las API de **Bing** admiten acciones de búsqueda que devuelven resultados según su tipo.  Todos los puntos de conexión de búsqueda devuelven resultados como objetos de respuesta JSON.  Todos los puntos de conexión admiten consultas que devuelven un idioma y una ubicación en concreto por longitud, latitud y radio de búsqueda.

Para una información completa acerca de los parámetros admitidos por cada punto de conexión, consulte las páginas de referencia de cada tipo.
Para obtener ejemplos de solicitudes básicas mediante Web Search API, consulte los [inicios rápidos de búsqueda en la Web](./overview.md).
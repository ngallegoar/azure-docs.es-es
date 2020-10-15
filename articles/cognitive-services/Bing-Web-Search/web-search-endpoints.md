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
ms.openlocfilehash: 2edc6a4cf4d4fb174c1b2067d8b3fb3b6f31859f
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "74111343"
---
# <a name="web-search-endpoint"></a>Punto de conexión de Web Search

**Web Search API** devuelve páginas web, noticias, imágenes, vídeos y [entidades](https://docs.microsoft.com/azure/cognitive-services/bing-entities-search/search-the-web). Las entidades contienen información de resumen sobre una persona, lugar o tema.

## <a name="endpoint"></a>Punto de conexión

Para obtener resultados de búsqueda web mediante la API de Bing, envíe una solicitud `GET` al punto de conexión siguiente. Los encabezados y parámetros de dirección URL definen especificaciones adicionales.

**Punto de conexión**: devuelve resultados web pertinentes para la consulta de búsqueda del usuario definida por `?q=""`.

```http
GET https://api.cognitive.microsoft.com/bing/v7.0/search
```

Punto de conexión: para más información acerca de encabezados, parámetros, códigos de mercado, objetos de respuesta, errores, etc., consulte la referencia [Bing Web API v7](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-web-api-v7-reference).

## <a name="response-json"></a>Respuesta JSON

La respuesta a una solicitud de búsqueda web incluye todos los resultados como objetos JSON. Para analizar el resultado se necesitan procedimientos que controlen los elementos de cada tipo. Consulte el [tutorial](https://docs.microsoft.com/azure/cognitive-services/bing-web-search/tutorial-bing-web-search-single-page-app) y el [código fuente](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/tree/master/Tutorials/Bing-Web-Search) para obtener ejemplos.

## <a name="next-steps"></a>Pasos siguientes

Las API de **Bing** admiten acciones de búsqueda que devuelven resultados según su tipo. Todos los puntos de conexión de búsqueda devuelven resultados como objetos de respuesta JSON.  Todos los puntos de conexión admiten consultas que devuelven un idioma y una ubicación en concreto por longitud, latitud y radio de búsqueda.

Para una información completa acerca de los parámetros admitidos por cada punto de conexión, consulte las páginas de referencia de cada tipo.
Para obtener ejemplos de solicitudes básicas mediante Web Search API, consulte los [inicios rápidos de búsqueda en la Web](https://docs.microsoft.com/azure/cognitive-services/bing-web-search/search-the-web).

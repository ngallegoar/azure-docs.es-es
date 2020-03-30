---
title: Punto de conexión de Bing Entity Search API
titleSuffix: Azure Cognitive Services
description: Bing Entity Search API incluye un punto de conexión que devuelve las entidades desde la Web basándose en una consulta. Estos resultados de búsqueda se devuelven en formato JSON.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-entity-search
ms.topic: conceptual
ms.date: 02/01/2019
ms.author: aahi
ms.openlocfilehash: e7f5af42d0bb0079746cc9e64b621adfebd565d1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "74072664"
---
# <a name="bing-entity-search-api-endpoint"></a>Punto de conexión de Bing Entity Search API


Bing Entity Search API incluye un punto de conexión que devuelve las entidades desde la Web basándose en una consulta. Estos resultados de búsqueda se devuelven en formato JSON.

## <a name="get-entity-results-from-the-endpoint"></a>Obtención de resultados de entidad a partir del punto de conexión

Para obtener resultados de entidades mediante **Bing API**, envíe una solicitud `GET` al punto de conexión siguiente. Use [encabezados](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-entities-api-v7-reference#headers) y [parámetros de consulta](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-entities-api-v7-reference#query-parameters) para personalizar la solicitud de búsqueda. Se pueden enviar solicitudes de búsqueda mediante el parámetro `?q=`.

```cURL
 GET https://api.cognitive.microsoft.com/bing/v7.0/entities
```

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [¿Qué es Bing Entity Search API?](overview.md)

## <a name="see-also"></a>Consulte también 

Para más información sobre encabezados, parámetros, códigos de mercado, objetos de respuesta, errores etc, consulte la referencia sobre [Bing Entity Search API v7](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-entities-api-v7-reference).

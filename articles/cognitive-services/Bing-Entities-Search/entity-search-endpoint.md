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
ms.openlocfilehash: 1d9b7c79919569830834915fc609b849e717dce8
ms.sourcegitcommit: 22da82c32accf97a82919bf50b9901668dc55c97
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/08/2020
ms.locfileid: "94365846"
---
# <a name="bing-entity-search-api-endpoint"></a>Punto de conexión de Bing Entity Search API

> [!WARNING]
> Bing Search APIs se mueve de Cognitive Services a Bing Search Services. A partir del **30 de octubre de 2020** , las nuevas instancias de Bing Search deben aprovisionarse siguiendo el proceso documentado [aquí](https://aka.ms/cogsvcs/bingmove).
> El aprovisionamiento de Bing Search APIs con Cognitive Services será posible durante los próximos tres años o hasta que finalice el Contrato Enterprise, lo que suceda primero.
> Puede encontrar instrucciones sobre la migración en [Bing Search Services](https://aka.ms/cogsvcs/bingmigration).


Bing Entity Search API incluye un punto de conexión que devuelve las entidades desde la Web basándose en una consulta. Estos resultados de búsqueda se devuelven en formato JSON.

## <a name="get-entity-results-from-the-endpoint"></a>Obtención de resultados de entidad a partir del punto de conexión

Para obtener resultados de entidades mediante **Bing API** , envíe una solicitud `GET` al punto de conexión siguiente. Use [encabezados](/rest/api/cognitiveservices-bingsearch/bing-entities-api-v7-reference#headers) y [parámetros de consulta](/rest/api/cognitiveservices-bingsearch/bing-entities-api-v7-reference#query-parameters) para personalizar la solicitud de búsqueda. Se pueden enviar solicitudes de búsqueda mediante el parámetro `?q=`.

```cURL
 GET https://api.cognitive.microsoft.com/bing/v7.0/entities
```

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [¿Qué es Bing Entity Search API?](overview.md)

## <a name="see-also"></a>Consulte también 

Para más información sobre encabezados, parámetros, códigos de mercado, objetos de respuesta, errores etc, consulte la referencia sobre [Bing Entity Search API v7](/rest/api/cognitiveservices-bingsearch/bing-entities-api-v7-reference).
---
title: 'Uso de la clasificación para mostrar respuestas: Bing Entity Search'
titleSuffix: Azure Cognitive Services
description: Aprenda a usar la clasificación para mostrar las respuestas que Bing Entity Search API devuelve.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-entity-search
ms.topic: conceptual
ms.date: 02/01/2019
ms.author: aahi
ms.openlocfilehash: 110cef117683b20170649a231226c8193496edf3
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/29/2020
ms.locfileid: "68423921"
---
# <a name="using-ranking-to-display-entity-search-results"></a>Uso de la clasificación para mostrar resultados de Entity Search  

Cada respuesta de Entity Search incluye una respuesta [RankingResponse](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference#rankingresponse) que especifica cómo debe mostrar los resultados de la búsqueda que devuelve Bing Entity Search API. La respuesta de clasificación agrupa los resultados en contenido de polo, línea principal y como barra lateral. El resultado de polo es el resultado más importante o destacado y debe mostrarse primero. Si no muestra los resultados restantes en un formato tradicional de línea principal y barra lateral, debe dar mayor visibilidad al contenido de la línea principal que al contenido de la barra lateral. 
  
Dentro de cada grupo, la matriz [Elementos](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference#rankinggroup-items) identifica el orden en que debe aparecer el contenido. Cada elemento ofrece dos formas de identificar el resultado dentro de una respuesta.  
 

|Campo | Descripción  |
|---------|---------|
|`answerType` y `resultIndex` | `answerType` identifica la respuesta (como Entidad o Lugar) y `resultIndex` identifica un resultado dentro de una respuesta (por ejemplo, una entidad). El índice comienza en 0.|
|`value`    | `value` contiene un identificador que coincide con el identificador de una respuesta o de un resultado dentro de una respuesta. La respuesta o los resultados contienen el identificador, pero no ambos. |
  
El uso de `answerType` y `resultIndex` es un proceso de dos pasos. Primero, use `answerType` para identificar la respuesta que contiene el resultado que se va a mostrar. Después, use `resultIndex` para indexar en los resultados de esa respuesta para obtener el resultado que se va a mostrar. (El valor `answerType` es el nombre del campo en el objeto [SearchResponse](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference#searchresponse)). Si se supone que se van a mostrar todos los resultados de la respuesta juntos, el elemento de la respuesta de clasificación no incluye el campo `resultIndex`.

El uso del identificador requiere que se asocie el identificador de la clasificación con el identificador de una respuesta o de uno de sus resultados. Si el objeto de una respuesta incluye un campo `id`, muestre todos los resultados de la respuesta juntos. Por ejemplo, si el objeto `Entities` incluye el campo `id`, muestre todos los artículos de entidades juntos. Si el objeto `Entities` no incluye el campo `id`, cada entidad contiene un campo `id` y la respuesta de clasificación combina las entidades con los resultados de lugares.  
  
## <a name="ranking-response-example"></a>Ejemplo de respuesta de clasificación

A continuación se muestra un ejemplo de [RankingResponse](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference#rankingresponse).
  
```json
{
  "_type": "SearchResponse",
  "queryContext": {
    "originalQuery": "Jimi Hendrix"
  },
  "entities": { ... },
  "rankingResponse": {
    "sidebar": {
      "items": [
        {
          "answerType": "Entities",
          "resultIndex": 0,
          "value": {
            "id": "https://www.bingapis.com/api/v7/#Entities.0"
          }
        },
        {
          "answerType": "Entities",
          "resultIndex": 1,
          "value": {
            "id": "https://www.bingapis.com/api/v7/#Entities.1"
          }
        }
      ]
    }
  }
}
```

En función de esta respuesta de clasificación, la barra lateral mostraría dos los resultados de entidades relacionados con Jimi Hendrix.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Creación de una aplicación web de una sola página](tutorial-bing-entities-search-single-page-app.md)

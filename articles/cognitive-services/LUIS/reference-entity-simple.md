---
title: 'Tipo de entidad simple: LUIS'
titleSuffix: Azure Cognitive Services
description: Una entidad simple describe un concepto único desde el contexto del aprendizaje automático. Agregue una lista de frases al usar una entidad simple para mejorar los resultados.
services: cognitive-services
manager: nitinme
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: reference
ms.date: 09/29/2019
ms.openlocfilehash: 384d3df2de551e7c79f13a0fe47ffb26c7825f1b
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "91539293"
---
# <a name="simple-entity"></a>Entidad simple

Una entidad simple es una entidad genérica que describe un concepto único que se ha aprendido en el contexto de aprendizaje automático. Dado que las entidades simples suelen ser nombres, como por ejemplo, nombres de compañías, de productos u otras categorías de nombres, agregue un [lista de frases](luis-concept-feature.md) cuando se use una entidad simple para aumentar la señal de los nombres usados.

**La entidad es la opción ideal cuando:**

* Los datos no tienen un formato coherente, pero indican lo mismo.

![entidad simple](./media/luis-concept-entities/simple-entity.png)

## <a name="example-json"></a>Ejemplo de JSON

`Bob Jones wants 3 meatball pho`

En la expresión anterior, `Bob Jones` se etiqueta como una entidad `Customer` simple.

Los datos devueltos desde el punto de conexión incluyen el nombre de la entidad, el texto de la expresión que se ha detectado, la ubicación del texto detectado y la puntuación:

#### <a name="v2-prediction-endpoint-response"></a>[Respuesta de punto de conexión de predicción de V2](#tab/V2)

```JSON
"entities": [
  {
  "entity": "bob jones",
  "type": "Customer",
  "startIndex": 0,
  "endIndex": 8,
  "score": 0.473899543
  }
]
```

#### <a name="v3-prediction-endpoint-response"></a>[Respuesta de punto de conexión de predicción de V3](#tab/V3)

Este es el valor JSON si `verbose=false` se establece en la cadena de consulta:

```json
"entities": {
    "Customer": [
        "Bob Jones"
    ]
}```

This is the JSON if `verbose=true` is set in the query string:

```json
"entities": {
    "Customer": [
        "Bob Jones"
    ],
    "$instance": {
        "Customer": [
            {
                "type": "Customer",
                "text": "Bob Jones",
                "startIndex": 0,
                "length": 9,
                "score": 0.9339134,
                "modelTypeId": 1,
                "modelType": "Entity Extractor",
                "recognitionSources": [
                    "model"
                ]
            }
        ]
    }
}
```

* * *

|Objeto de datos|Nombre de entidad|Value|
|--|--|--|
|Entidad simple|`Customer`|`bob jones`|

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Sintaxis del patrón de aprendizaje](reference-pattern-syntax.md)
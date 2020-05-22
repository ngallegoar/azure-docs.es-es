---
title: 'Tipo de entidad compuesta: LUIS'
titleSuffix: Azure Cognitive Services
description: Una entidad compuesta consta de otras entidades, como las entidades pregeneradas, expresiones simples y regulares, y entidades de lista. Las entidades independientes forman una entidad completa.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: reference
ms.date: 04/14/2020
ms.author: diberry
ms.openlocfilehash: bac37e91933d16f36f2d8917760968122a4f5619
ms.sourcegitcommit: bb0afd0df5563cc53f76a642fd8fc709e366568b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/19/2020
ms.locfileid: "83588911"
---
# <a name="composite-entity"></a>Entidad compuesta

Una entidad compuesta consta de otras entidades, como las entidades pregeneradas, expresiones simples y regulares, y entidades de lista. Las entidades independientes forman una entidad completa.

> [!CAUTION]
> Esta entidad está **en desuso**. Migre a una [entidad con aprendizaje automático](reference-entity-machine-learned-entity.md).

**Esta entidad es la opción ideal cuando los datos:**

* Están relacionados entre sí.
* Se relacionan entre sí en el contexto de la expresión.
* Usan una variedad de tipos de entidad.
* Deben agruparse y la aplicación cliente debe procesarlos como una unidad de información.
* Tienen una variedad de expresiones de usuario que requieren aprendizaje automático.

![entidad compuesta](./media/luis-concept-entities/composite-entity.png)

## <a name="example-json"></a>Ejemplo de JSON

Considere una entidad compuesta de las entidades `number` y `Location::ToLocation` pregeneradas con la expresión siguiente:

`book 2 tickets to cairo`

Fíjese en que `2`, el número, y `cairo`, el valor de ToLocation, contienen palabras entre ellos que no forman parte de ninguna de las entidades. El subrayado verde, que se usa en una expresión con etiqueta en el sitio web de [LUIS](luis-reference-regions.md), indica una entidad compuesta.

![Entidad compuesta](./media/luis-concept-data-extraction/composite-entity.png)

#### <a name="v2-prediction-endpoint-response"></a>[Respuesta de punto de conexión de predicción de V2](#tab/V2)

Las entidades compuestas se devuelven en una matriz `compositeEntities` y todas las entidades dentro de la composición también se devuelven en la matriz `entities`:

```JSON
  "entities": [
    {
      "entity": "2 tickets to cairo",
      "type": "ticketinfo",
      "startIndex": 5,
      "endIndex": 22,
      "score": 0.9214487
    },
    {
      "entity": "cairo",
      "type": "builtin.geographyV2.city",
      "startIndex": 18,
      "endIndex": 22
    },
    {
      "entity": "2",
      "type": "builtin.number",
      "startIndex": 5,
      "endIndex": 5,
      "resolution": {
        "subtype": "integer",
        "value": "2"
      }
    }
  ],
  "compositeEntities": [
    {
      "parentType": "ticketinfo",
      "value": "2 tickets to cairo",
      "children": [
        {
          "type": "builtin.number",
          "value": "2"
        },
        {
          "type": "builtin.geographyV2.city",
          "value": "cairo"
        }
      ]
    }
  ]
```

#### <a name="v3-prediction-endpoint-response"></a>[Respuesta de punto de conexión de predicción de V3](#tab/V3)

Este es el valor JSON si `verbose=false` se establece en la cadena de consulta:

```json
"entities": {
    "ticketinfo": [
        {
            "number": [
                2
            ],
            "geographyV2": [
                "cairo"
            ]
        }
    ]
}
```

Este es el valor JSON si `verbose=true` se establece en la cadena de consulta:

```json
"entities": {
    "ticketinfo": [
        {
            "number": [
                2
            ],
            "geographyV2": [
                "cairo"
            ],
            "$instance": {
                "number": [
                    {
                        "type": "builtin.number",
                        "text": "2",
                        "startIndex": 5,
                        "length": 1,
                        "modelTypeId": 2,
                        "modelType": "Prebuilt Entity Extractor",
                        "recognitionSources": [
                            "model"
                        ]
                    }
                ],
                "geographyV2": [
                    {
                        "type": "builtin.geographyV2.city",
                        "text": "cairo",
                        "startIndex": 18,
                        "length": 5,
                        "modelTypeId": 2,
                        "modelType": "Prebuilt Entity Extractor",
                        "recognitionSources": [
                            "model"
                        ]
                    }
                ]
            }
        }
    ],
    "$instance": {
        "ticketinfo": [
            {
                "type": "ticketinfo",
                "text": "2 tickets to cairo",
                "startIndex": 5,
                "length": 18,
                "score": 0.9214487,
                "modelTypeId": 4,
                "modelType": "Composite Entity Extractor",
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
|Entidad creada previamente de número|"builtin.number"|"2"|
|Entidad precompilada: GeographyV2|"Location::ToLocation"|"cairo"|

## <a name="next-steps"></a>Pasos siguientes

Más información sobre las entidades:

* [Conceptos](luis-concept-entity-types.md)
* [Creación](luis-how-to-add-entities.md)

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
ms.date: 09/29/2019
ms.author: diberry
ms.openlocfilehash: a5a1ad467074ee0aa55d14d50ae153ac68304e6f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "71695167"
---
# <a name="composite-entity"></a>Entidad compuesta 

Una entidad compuesta consta de otras entidades, como las entidades pregeneradas, expresiones simples y regulares, y entidades de lista. Las entidades independientes forman una entidad completa. 

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

En este [tutorial](luis-tutorial-composite-entity.md), agregue una **entidad compuesta** para agrupar los datos extraídos de varios tipos en una única entidad contenedora. Mediante la agrupación de los datos, la aplicación cliente puede extraer fácilmente los datos relacionados en diferentes tipos de datos.

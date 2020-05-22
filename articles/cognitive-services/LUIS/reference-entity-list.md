---
title: 'Tipo de entidad de lista: LUIS'
description: Las entidades de lista representan un conjunto fijo y cerrado de palabras relacionadas y sus sinónimos. LUIS no detecta valores adicionales para las entidades de lista. Use la característica Recommend (Recomendar) para ver sugerencias de palabras nuevas en función de la lista actual.
ms.topic: reference
ms.date: 04/14/2020
ms.openlocfilehash: 339fb832ef4af069b6f040c5264426002189f93f
ms.sourcegitcommit: bb0afd0df5563cc53f76a642fd8fc709e366568b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/19/2020
ms.locfileid: "83588894"
---
# <a name="list-entity"></a>Entidad de lista

Las entidades de lista representan un conjunto fijo y cerrado de palabras relacionadas y sus sinónimos. LUIS no detecta valores adicionales para las entidades de lista. Use la característica **Recommend** (Recomendar) para ver sugerencias de palabras nuevas en función de la lista actual. Si hay más de una entidad de lista con el mismo valor, se devolverá cada entidad en la consulta de punto de conexión.

Una entidad de lista no está relacionada con el aprendizaje automático. Es una coincidencia de texto exacta. LUIS marca todas las coincidencias de un elemento de cualquier lista como una entidad en la respuesta.

**La entidad es la opción ideal cuando los datos de texto:**

* Son un conjunto conocido.
* No cambian a menudo. Si necesita cambiar la lista a menudo o desea que la lista se expanda automáticamente, es mejor una entidad sencilla potenciada con una lista de frases.
* El conjunto no excede los [límites](luis-limits.md) máximos de LUIS para este tipo de entidad.
* El texto de la expresión es una coincidencia, sin distinción entre mayúsculas y minúsculas, con un sinónimo o el nombre canónico. LUIS no usa la lista más allá de la coincidencia. La coincidencia aproximada, la lematización, los plurales y otras variaciones no se resuelven con una entidad de lista. Para administrar las variaciones, considere el uso de un [patrón](reference-pattern-syntax.md#syntax-to-mark-optional-text-in-a-template-utterance) con la sintaxis de texto opcional.

![entidad de lista](./media/luis-concept-entities/list-entity.png)

## <a name="example-json-to-import-into-list-entity"></a>Ejemplo. JSON para importarse en la entidad de lista

  Puede importar los valores en una entidad de lista existente con el formato .json siguiente:

  ```JSON
  [
      {
          "canonicalForm": "Blue",
          "list": [
              "navy",
              "royal",
              "baby"
          ]
      },
      {
          "canonicalForm": "Green",
          "list": [
              "kelly",
              "forest",
              "avacado"
          ]
      }
  ]
  ```

## <a name="example-json-response"></a>Ejemplo de respuesta JSON

Imagine que la aplicación tiene una lista, denominada `Cities`, que permite variaciones de nombres de ciudades, incluidos la ciudad del aeropuerto (Sea-tac), el código del aeropuerto (SEA), el código postal (98101) y el código de área telefónica (206).

|Elemento de lista|Sinónimos del elemento|
|---|---|
|`Seattle`|`sea-tac`, `sea`, `98101`, `206`, `+1` |
|`Paris`|`cdg`, `roissy`, `ory`, `75001`, `1`, `+33`|

`book 2 tickets to paris`

En la expresión anterior, la palabra `paris` se asigna al elemento paris como parte de la entidad de lista `Cities`. La entidad de lista coincide con el nombre normalizado del elemento y con los sinónimos del elemento.

#### <a name="v2-prediction-endpoint-response"></a>[Respuesta de punto de conexión de predicción de V2](#tab/V2)

```JSON
  "entities": [
    {
      "entity": "paris",
      "type": "Cities",
      "startIndex": 18,
      "endIndex": 22,
      "resolution": {
        "values": [
          "Paris"
        ]
      }
    }
  ]
```

#### <a name="v3-prediction-endpoint-response"></a>[Respuesta de punto de conexión de predicción de V3](#tab/V3)


Este es el valor JSON si `verbose=false` se establece en la cadena de consulta:

```json
"entities": {
    "Cities": [
        [
            "Paris"
        ]
    ]
}
```

Este es el valor JSON si `verbose=true` se establece en la cadena de consulta:

```json
"entities": {
    "Cities": [
        [
            "Paris"
        ]
    ],
    "$instance": {
        "Cities": [
            {
                "type": "Cities",
                "text": "paris",
                "startIndex": 18,
                "length": 5,
                "modelTypeId": 5,
                "modelType": "List Entity Extractor",
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
|Entidad de lista|`Cities`|`paris`|


## <a name="next-steps"></a>Pasos siguientes

Más información sobre las entidades:

* [Conceptos](luis-concept-entity-types.md)
* [Creación](luis-how-to-add-entities.md)

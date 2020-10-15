---
title: 'Entidades precompiladas Dimension: LUIS'
titleSuffix: Azure Cognitive Services
description: Este artículo contiene información sobre la entidad precompilada de dimensión en Language Understanding (LUIS).
services: cognitive-services
ms.custom: seodec18
manager: nitinme
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: reference
ms.date: 10/14/2019
ms.openlocfilehash: 0b5f081dc8c0bc203ccafde4a513371d11e36879
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "91538354"
---
# <a name="dimension-prebuilt-entity-for-a-luis-app"></a>Entidad precompilada Dimension para una aplicación de LUIS
La entidad de dimensión precompilada detecta varios tipos de dimensiones, con independencia de la referencia cultural de la aplicación LUIS. Dado que esta entidad ya está entrenada, no se necesita agregar expresiones de ejemplo que contengan dimensiones en las intenciones de la aplicación. La entidad de dimensión se admite en [muchas referencias culturales](luis-reference-prebuilt-entities.md).

## <a name="types-of-dimension"></a>Tipos de dimensión

La dimensión se administra desde el repositorio [Recognizers-Text](https://github.com/Microsoft/Recognizers-Text/blob/master/Patterns/English/English-NumbersWithUnit.yaml) de GitHub.

## <a name="resolution-for-dimension-entity"></a>Resolución de la entidad de dimensión

La consulta devuelve los siguientes objetos de entidad:

`10 1/2 miles of cable`

#### <a name="v3-response"></a>[Respuesta de V3](#tab/V3)

El siguiente JSON es con el parámetro `verbose` establecido en `false`:

```json
"entities": {
    "dimension": [
        {
            "number": 10.5,
            "units": "Mile"
        }
    ]
}
```
#### <a name="v3-verbose-response"></a>[Respuesta detallada de V3](#tab/V3-verbose)
El siguiente JSON es con el parámetro `verbose` establecido en `true`:

```json
"entities": {
    "dimension": [
        {
            "number": 10.5,
            "units": "Mile"
        }
    ],
    "$instance": {
        "dimension": [
            {
                "type": "builtin.dimension",
                "text": "10 1/2 miles",
                "startIndex": 0,
                "length": 12,
                "modelTypeId": 2,
                "modelType": "Prebuilt Entity Extractor",
                "recognitionSources": [
                    "model"
                ]
            }
        ]
    }
}
```

#### <a name="v2-response"></a>[Respuesta de V2](#tab/V2)

En el siguiente ejemplo, se muestra la resolución de la entidad **builtin.ordinal**.

```json
{
    "entity": "10 1/2 miles",
    "type": "builtin.dimension",
    "startIndex": 0,
    "endIndex": 11,
    "resolution": {
    "unit": "Mile",
    "value": "10.5"
    }
}
```
* * *

## <a name="next-steps"></a>Pasos siguientes

Más información acerca del [punto de conexión de predicción de V3](luis-migration-api-v3.md).

Obtenga información acerca de las entidades de [correo electrónico](luis-reference-prebuilt-email.md), [número](luis-reference-prebuilt-number.md) y [ordinal](luis-reference-prebuilt-ordinal.md).

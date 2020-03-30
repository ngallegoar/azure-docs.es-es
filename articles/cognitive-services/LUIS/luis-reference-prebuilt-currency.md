---
title: 'Entidad precompilada Currency: LUIS'
titleSuffix: Azure Cognitive Services
description: Este artículo contiene información de la entidad precompilada de la moneda en Language Understanding (LUIS).
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: reference
ms.date: 10/14/2019
ms.author: diberry
ms.openlocfilehash: 56d264fa976967ae4772f50b4aa6f58b30355d6f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "78270779"
---
# <a name="currency-prebuilt-entity-for-a-luis-app"></a>Entidad precompilada Currency para una aplicación de LUIS
La entidad precompilada de moneda detecta la moneda en muchas denominaciones, países y regiones, independientemente de la referencia cultural de la aplicación de LUIS. Dado que esta entidad ya está entrenada, no es necesario agregar expresiones de ejemplo que contengan la moneda para las intenciones de la aplicación. La entidad de la moneda se admite en [muchas referencias culturales](luis-reference-prebuilt-entities.md).

## <a name="types-of-currency"></a>Tipos de moneda
La moneda se administra desde el repositorio de GitHub [Recognizers-text](https://github.com/Microsoft/Recognizers-Text/blob/master/Patterns/English/English-NumbersWithUnit.yaml#L26)

## <a name="resolution-for-currency-entity"></a>Resolución de la entidad de moneda

#### <a name="v3-response"></a>[Respuesta de V3](#tab/V3)

El siguiente JSON es con el parámetro `verbose` establecido en `false`:

```json
"entities": {
    "money": [
        {
            "number": 10.99,
            "units": "Dollar"
        }
    ]
}
```
#### <a name="v3-verbose-response"></a>[Respuesta detallada de V3](#tab/V3-verbose)
El siguiente JSON es con el parámetro `verbose` establecido en `true`:

```json
"entities": {
    "money": [
        {
            "number": 10.99,
            "unit": "Dollar"
        }
    ],
    "$instance": {
        "money": [
            {
                "type": "builtin.currency",
                "text": "$10.99",
                "startIndex": 23,
                "length": 6,
                "modelTypeId": 2,
                "modelType": "Prebuilt Entity Extractor"
            }
        ]
    }
}
```

#### <a name="v2-response"></a>[Respuesta de V2](#tab/V2)

En el siguiente ejemplo se muestra la resolución de la entidad **builtin.currency**.

```json
"entities": [
    {
        "entity": "$10.99",
        "type": "builtin.currency",
        "startIndex": 23,
        "endIndex": 28,
        "resolution": {
        "unit": "Dollar",
        "value": "10.99"
        }
    }
]
```
* * *

## <a name="next-steps"></a>Pasos siguientes

Más información acerca del [punto de conexión de predicción de V3](luis-migration-api-v3.md).

Obtenga información acerca de las entidades [datetimeV2](luis-reference-prebuilt-datetimev2.md), [dimension](luis-reference-prebuilt-dimension.md) y [email](luis-reference-prebuilt-email.md).

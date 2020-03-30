---
title: 'Entidad precompilada Ordinal V2: LUIS'
titleSuffix: Azure Cognitive Services
description: Este artículo contiene información acerca de la entidad precompilada ordinal V2 en Language Understanding (LUIS).
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: reference
ms.date: 09/27/2019
ms.author: diberry
ms.openlocfilehash: 5e852313db75e598da647ea0f985e2ee18af16de
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "78270496"
---
# <a name="ordinal-v2-prebuilt-entity-for-a-luis-app"></a>Entidad precompilada Ordinal V2 para una aplicación de LUIS
Ordinal V2 expande la entidad [Ordinal](luis-reference-prebuilt-ordinal.md) para proporcionar referencias relativas como `next`, `last` y `previous`. Estas no se extraen mediante la entidad precompilada ordinal.

## <a name="resolution-for-prebuilt-ordinal-v2-entity"></a>Resolución de la entidad precompilada ordinal V2

La consulta devuelve los siguientes objetos de entidad:

`what is the second to last choice in the list`

#### <a name="v3-response"></a>[Respuesta de V3](#tab/V3)

El siguiente JSON es con el parámetro `verbose` establecido en `false`:

```json
"entities": {
    "ordinalV2": [
        {
            "offset": -1,
            "relativeTo": "end"
        }
    ]
}
```

#### <a name="v3-verbose-response"></a>[Respuesta detallada de V3](#tab/V3-verbose)

El siguiente JSON es con el parámetro `verbose` establecido en `true`:

```json
"entities": {
    "ordinalV2": [
        {
            "offset": -1,
            "relativeTo": "end"
        }
    ],
    "$instance": {
        "ordinalV2": [
            {
                "type": "builtin.ordinalV2.relative",
                "text": "the second to last",
                "startIndex": 8,
                "length": 18,
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

En el siguiente ejemplo, se muestra la resolución de la entidad **builtin.ordinalV2**.

```json
"entities": [
    {
        "entity": "the second to last",
        "type": "builtin.ordinalV2.relative",
        "startIndex": 8,
        "endIndex": 25,
        "resolution": {
            "offset": "-1",
            "relativeTo": "end"
        }
    }
]
```
* * *

## <a name="next-steps"></a>Pasos siguientes

Más información acerca del [punto de conexión de predicción de V3](luis-migration-api-v3.md).

Más información sobre las entidades [percentage](luis-reference-prebuilt-percentage.md), [phonenumber](luis-reference-prebuilt-phonenumber.md) y [temperature](luis-reference-prebuilt-temperature.md).

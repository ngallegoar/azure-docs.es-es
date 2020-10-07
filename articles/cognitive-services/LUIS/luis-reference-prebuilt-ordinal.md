---
title: 'Entidad precompilada Ordinal: LUIS'
titleSuffix: Azure Cognitive Services
description: Este artículo contiene información acerca de la entidad ordinal precompilada en Language Understanding (LUIS).
services: cognitive-services
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: reference
ms.date: 10/14/2019
ms.openlocfilehash: 1fa86c8960ea9f32163ebd7991260a19ef7a5d79
ms.sourcegitcommit: f5580dd1d1799de15646e195f0120b9f9255617b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/29/2020
ms.locfileid: "91535464"
---
# <a name="ordinal-prebuilt-entity-for-a-luis-app"></a>entidad precompilada Ordinal para una aplicación de LUIS
Un número ordinal es una representación numérica de un objeto dentro de un conjunto: `first`, `second`, `third`. Dado que esta entidad ya está entrenada, no es necesario agregar expresiones de ejemplo que contengan ordinal a las intenciones de la aplicación. La entidad ordinal se admite en [muchas referencias culturales](luis-reference-prebuilt-entities.md).

## <a name="types-of-ordinal"></a>Tipos de ordinales
La entidad ordinal se administra desde el repositorio de GitHub [Recognizers-Text](https://github.com/Microsoft/Recognizers-Text/blob/master/Patterns/English/English-Numbers.yaml#L45).

## <a name="resolution-for-prebuilt-ordinal-entity"></a>Resolución de la entidad ordinal precompilada

La consulta devuelve los siguientes objetos de entidad:

`Order the second option`

#### <a name="v3-response"></a>[Respuesta de V3](#tab/V3)

El siguiente JSON es con el parámetro `verbose` establecido en `false`:

```json
"entities": {
    "ordinal": [
        2
    ]
}
```
#### <a name="v3-verbose-response"></a>[Respuesta detallada de V3](#tab/V3-verbose)
El siguiente JSON es con el parámetro `verbose` establecido en `true`:

```json
"entities": {
    "ordinal": [
        2
    ],
    "$instance": {
        "ordinal": [
            {
                "type": "builtin.ordinal",
                "text": "second",
                "startIndex": 10,
                "length": 6,
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
"entities": [
  {
    "entity": "second",
    "type": "builtin.ordinal",
    "startIndex": 10,
    "endIndex": 15,
    "resolution": {
      "value": "2"
    }
  }
]
```
* * *

## <a name="next-steps"></a>Pasos siguientes

Más información acerca del [punto de conexión de predicción de V3](luis-migration-api-v3.md).

Información sobre las entidades [OrdinalV2](luis-reference-prebuilt-ordinal-v2.md), [phone number](luis-reference-prebuilt-phonenumber.md) y [temperature](luis-reference-prebuilt-temperature.md).

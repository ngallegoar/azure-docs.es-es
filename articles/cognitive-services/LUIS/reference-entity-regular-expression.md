---
title: 'Tipo de entidad de expresión regular: LUIS'
titleSuffix: Azure Cognitive Services
description: Una expresión regular es mejor para el texto de enunciado sin formato. No distingue entre mayúsculas y minúsculas e ignora la variante cultural.  La coincidencia de expresiones regulares se aplica después de las modificaciones de la ortografía en los caracteres, no en el nivel de token.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: reference
ms.date: 09/29/2019
ms.author: diberry
ms.openlocfilehash: b9da76a80183f353a74d43e667bf6c9219eb6c05
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "74841224"
---
# <a name="regular-expression-entity"></a>Entidad de expresión regular

Una entidad de expresión regular extrae una entidad basada en un patrón de expresión regular que se proporciona.

Una expresión regular es mejor para el texto de enunciado sin formato. No distingue entre mayúsculas y minúsculas e ignora la variante cultural.  La coincidencia de expresiones regulares se aplica después de las modificaciones de la ortografía en los caracteres, no en el nivel de token. Si la expresión regular es demasiado compleja (por ejemplo, uso excesivo de corchetes), no podrá agregar la expresión al modelo. Usa una parte de la biblioteca de [expresiones regulares de .NET](https://docs.microsoft.com/dotnet/standard/base-types/regular-expressions), pero no toda ella.

**La entidad es la opción ideal cuando:**

* Los datos tienen un formato coherente con cualquier variación que también sea coherente.
* La expresión regular no necesita más de 2 niveles de anidamiento.

![Entidad de expresión regular](./media/luis-concept-entities/regex-entity.png)

## <a name="usage-considerations"></a>Consideraciones de uso

Las expresiones regulares pueden generar más coincidencias de las esperables. Un ejemplo de esto es la coincidencia de números en palabras como `one` y `two`. Un ejemplo es la siguiente expresión regular, que coincide con el número `one` junto con otros números:

```javascript
(plus )?(zero|one|two|three|four|five|six|seven|eight|nine)(\s+(zero|one|two|three|four|five|six|seven|eight|nine))*
```

Esta expresión regular también coincide con cualquier palabra que termina con estos números, como `phone`. Para solucionar problemas como este, asegúrese de que las coincidencias de expresiones regulares tienen en cuenta los límites de palabras. La expresión regular para usar los límites de palabras en este ejemplo se utiliza en la siguiente expresión regular:

```javascript
\b(plus )?(zero|one|two|three|four|five|six|seven|eight|nine)(\s+(zero|one|two|three|four|five|six|seven|eight|nine))*\b
```

### <a name="example-json"></a>Ejemplo de JSON

Al usar `kb[0-9]{6}` como definición de la entidad de expresión regular, la siguiente respuesta JSON es una expresión de ejemplo con las entidades de expresión regular devueltas para la consulta:

`When was kb123456 published?`:

#### <a name="v2-prediction-endpoint-response"></a>[Respuesta de punto de conexión de predicción de V2](#tab/V2)

```JSON
"entities": [
  {
    "entity": "kb123456",
    "type": "KB number",
    "startIndex": 9,
    "endIndex": 16
  }
]
```


#### <a name="v3-prediction-endpoint-response"></a>[Respuesta de punto de conexión de predicción de V3](#tab/V3)


Este es el valor JSON si `verbose=false` se establece en la cadena de consulta:

```json
"entities": {
    "KB number": [
        "kb123456"
    ]
}
```

Este es el valor JSON si `verbose=true` se establece en la cadena de consulta:

```json
"entities": {
    "KB number": [
        "kb123456"
    ],
    "$instance": {
        "KB number": [
            {
                "type": "KB number",
                "text": "kb123456",
                "startIndex": 9,
                "length": 8,
                "modelTypeId": 8,
                "modelType": "Regex Entity Extractor",
                "recognitionSources": [
                    "model"
                ]
            }
        ]
    }
}
```

* * *

## <a name="next-steps"></a>Pasos siguientes

En este [tutorial](tutorial-regex-entity.md) va a crear una aplicación para extraer datos con formato de forma coherente a partir de una expresión con la entidad de **expresión regular**.
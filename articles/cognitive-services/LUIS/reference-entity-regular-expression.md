---
title: 'Tipo de entidad de expresión regular: LUIS'
description: Una expresión regular es mejor para el texto de enunciado sin formato. No distingue entre mayúsculas y minúsculas e ignora la variante cultural.  La coincidencia de expresiones regulares se aplica después de las modificaciones de la ortografía en los caracteres, no en el nivel de token.
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: reference
ms.date: 04/14/2020
ms.openlocfilehash: 0c8d4a5013f23a805c22dc5c44444de6b10ab369
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "91316246"
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

Más información sobre las entidades:

* [Conceptos](luis-concept-entity-types.md)
* [Creación](luis-how-to-add-entities.md)
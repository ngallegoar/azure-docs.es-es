---
title: 'Tipo de entidad Pattern.any: LUIS'
titleSuffix: Azure Cognitive Services
description: Pattern.any es un marcador de posición de longitud variable que solo se usa en la expresión de plantilla de un patrón para marcar dónde empieza y acaba la entidad.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: reference
ms.date: 09/29/2019
ms.author: diberry
ms.openlocfilehash: 5164bf55ef8233cf34a470524da3bc852678d79a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "75979177"
---
# <a name="patternany-entity"></a>Entidad Pattern.any

Pattern.any es un marcador de posición de longitud variable que solo se usa en la expresión de plantilla de un patrón para marcar dónde empieza y acaba la entidad.  

Las entidades Pattern.Any deben marcarse en los ejemplos de la plantilla de [patrón](luis-how-to-model-intent-pattern.md), y no en los ejemplos de la intención del usuario.

**La entidad es la opción ideal cuando:**

* El final de la entidad se puede confundir con el resto del texto de la expresión.

## <a name="usage"></a>Uso

Dada una aplicación cliente que busca libros en función del título, pattern.any extrae el título completo. `Was {BookTitle} written by an American this year[?]` es una expresión de plantilla que usa pattern.any para este libro.

En la tabla siguiente, cada fila tiene dos versiones de la declaración. La expresión superior es la forma en que LUIS ve inicialmente la expresión. No está claro dónde comienza y termina el título del libro. La expresión inferior usa la entidad Pattern.any para marcar el principio y el final de la entidad.

|Expresión con la entidad en negrita|
|--|
|`Was The Man Who Mistook His Wife for a Hat and Other Clinical Tales written by an American this year?`<br><br>¿El libro **El hombre que confundió a su mujer con un sombrero** lo escribió un autor americano este año?|
|`Was Half Asleep in Frog Pajamas written by an American this year?`<br><br>¿El libro **Was Half Asleep in Frog Pajamas** (Medio dormido en pijama con estampado de ranas) lo escribió un autor americano este año?|
|`Was The Particular Sadness of Lemon Cake: A Novel written by an American this year?`<br><br>¿El libro **La insólita amargura del pastel de limón** lo escribió un autor americano este año?|
|`Was There's A Wocket In My Pocket! written by an American this year?`<br><br>¿El libro **¡Hay un molillo en mi bolsillo!** lo escribió un autor americano este año?|
||



## <a name="example-json"></a>Ejemplo de JSON

Considere la siguiente consulta:

`where is the form Understand your responsibilities as a member of the community and who needs to sign it after I read it?`

Con el nombre del formulario incrustado que se va a extraer como Pattern.any:

`Understand your responsibilities as a member of the community`

#### <a name="v2-prediction-endpoint-response"></a>[Respuesta de punto de conexión de predicción de V2](#tab/V2)

```JSON
"entities": [
  {
    "entity": "understand your responsibilities as a member of the community",
    "type": "FormName",
    "startIndex": 18,
    "endIndex": 78,
    "role": ""
  }
```


#### <a name="v3-prediction-endpoint-response"></a>[Respuesta de punto de conexión de predicción de V3](#tab/V3)

Este es el valor JSON si `verbose=false` se establece en la cadena de consulta:

```json
"entities": {
    "FormName": [
        "Understand your responsibilities as a member of the community"
    ]
}
```

Este es el valor JSON si `verbose=true` se establece en la cadena de consulta:

```json
"entities": {
    "FormName": [
        "Understand your responsibilities as a member of the community"
    ],
    "$instance": {
        "FormName": [
            {
                "type": "FormName",
                "text": "Understand your responsibilities as a member of the community",
                "startIndex": 18,
                "length": 61,
                "modelTypeId": 7,
                "modelType": "Pattern.Any Entity Extractor",
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

En este [tutorial](luis-tutorial-pattern.md), use la entidad **Pattern.any** para extraer datos de expresiones con el formato correcto y donde el final de los datos se puede confundir fácilmente con el resto de las palabras de la expresión.

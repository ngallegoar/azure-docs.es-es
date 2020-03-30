---
title: Método de búsqueda de diccionario de Translator Text API
titleSuffix: Azure Cognitive Services
description: El método Dictionary Lookup proporciona traducciones alternativas para una palabra y un pequeño número de frases hechas.
services: cognitive-services
author: swmachan
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: reference
ms.date: 01/21/2020
ms.author: swmachan
ms.openlocfilehash: bd27827441082698bb4e0b43e7dd22d5b7e66539
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "76548958"
---
# <a name="translator-text-api-30-dictionary-lookup"></a>Translator Text API 3.0: Búsqueda de diccionario

Proporciona traducciones alternativas para una palabra y un pequeño número de frases hechas. Cada traducción tiene funciones de sintaxis y una lista de traducciones inversas. Las traducciones inversas permiten al usuario comprender la traducción en contexto. La operación [Ejemplo de diccionario](./v3-0-dictionary-examples.md) le permite profundizar más para ver ejemplos de usos de cada par de traducción.

## <a name="request-url"></a>URL de la solicitud

Envíe una solicitud `POST` a:

```HTTP
https://api.cognitive.microsofttranslator.com/dictionary/lookup?api-version=3.0
```

## <a name="request-parameters"></a>Parámetros de solicitud

Los parámetros de solicitud que se pasaron en la cadena de consulta son:

| Parámetro de consulta  | Descripción |
| ------ | ----------- |
| api-version <img width=200/>   | **Parámetro obligatorio**.<br/>Versión de la API que el cliente solicitó. El valor debe ser `3.0`. |
| desde | **Parámetro obligatorio**.<br/>Especifica el idioma del texto de entrada. El idioma de origen debe ser uno de los [idiomas admitidos](./v3-0-languages.md) que están incluidos en el ámbito `dictionary`. |
| to   | **Parámetro obligatorio**.<br/>Especifica el idioma del texto de salida. El idioma de destino debe ser uno de los [idiomas admitidos](v3-0-languages.md) que están incluidos en el ámbito `dictionary`. |


Los encabezados de solicitud incluyen lo siguiente:

| encabezados  | Descripción |
| ------ | ----------- |
| Encabezados de autenticación <img width=200/>  | **Encabezado de solicitud obligatorio**.<br/>Consulte las <a href="https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-reference#authentication">opciones disponibles para la autenticación</a>. |
| Content-Type | **Encabezado de solicitud obligatorio**.<br/>Especifica el tipo de contenido de la carga. Los valores posibles son: `application/json`. |
| Content-Length   | **Encabezado de solicitud obligatorio**.<br/>Longitud del cuerpo de la solicitud. |
| X-ClientTraceId   | **Opcional**.<br/>GUID generado por el cliente para identificar de forma única la solicitud. Puede omitir este encabezado si incluye el id. de seguimiento en la cadena de la consulta mediante un parámetro de consulta denominado `ClientTraceId`. |

## <a name="request-body"></a>Cuerpo de la solicitud

El cuerpo de la solicitud es una matriz JSON. Cada elemento de la matriz es un objeto JSON con una propiedad de cadena denominada `Text`, que representa el término que se va a buscar.

```json
[
    {"Text":"fly"}
]
```

Se aplican las siguientes limitaciones:

* La matriz puede tener como máximo 10 elementos.
* El valor de texto de un elemento de la matriz no puede superar los 100 caracteres, incluyendo los espacios.

## <a name="response-body"></a>Response body

Una respuesta correcta es una matriz JSON con un resultado para cada cadena en la matriz de entrada. Un objeto del resultado incluye las siguientes propiedades:

  * `normalizedSource`: es una cadena que proporciona el formato normalizado del término de origen. Por ejemplo, si la solicitud es "JOHN", la forma normalizada será "john". El contenido de este campo se convierte en la entrada de los [ejemplos de búsqueda](./v3-0-dictionary-examples.md).
    
  * `displaySource`: es una cadena que proporciona el término de origen de la forma más adecuada para mostrársela al usuario final. Por ejemplo, si la entrada es "JOHN", el formulario de visualización reflejará la ortografía habitual del nombre: "John". 

  * `translations`: es una lista de traducciones para el término de origen. Cada elemento de la lista es un objeto que consta de las siguientes propiedades:

    * `normalizedTarget`: es una cadena que da la forma normalizada del término en el idioma de destino. Este valor debe usarse como entrada para [buscar ejemplos](./v3-0-dictionary-examples.md).

    * `displayTarget`: es una cadena que proporciona el término en el idioma de destino y de una forma más adecuada para mostrársela al usuario final. Generalmente, esto solo se diferencia de `normalizedTarget` en lo que respecta al uso de las mayúsculas. Por ejemplo, un nombre propio como "Juan" tendrá `normalizedTarget = "juan"` y `displayTarget = "Juan"`.

    * `posTag`: es una cadena que asocia este término con una etiqueta de funciones de sintaxis.

        | Nombre de etiqueta | Descripción  |
        |----------|--------------|
        | ADJ      | Adjetivos   |
        | ADV      | Adverbios      |
        | CONJ     | Conjunciones |
        | DET      | Determinantes  |
        | MODAL    | Verbos        |
        | SUSTANTIVO     | Sustantivos        |
        | PREP     | Preposiciones |
        | PRON     | Pronombres     |
        | VERBO     | Verbos        |
        | OTHER    | Otros        |

        Como nota de implementación, estas etiquetas se determinaron como funciones de sintaxis mediante el etiquetado de la parte en inglés, y usando la etiqueta más frecuente para cada par de origen o destino. Por lo tanto, si las personas traducen con frecuencia una palabra en español a una etiqueta de función de sintaxis que está en inglés, las etiquetas pueden terminar siendo incorrectas (con respecto a la palabra en español).

    * `confidence`: es un valor entre 0,0 y 1,0 que representa la "confianza" o, más exactamente, la "probabilidad en los datos de aprendizaje" del par de traducción. La suma de las puntuaciones de confianza para una palabra de origen puede, o no, sumar 1,0. 

    * `prefixWord`: es una cadena que indica que la palabra para mostrar es un prefijo en la traducción. En la actualidad, este es el determinante de género de los sustantivos, que se usa en idiomas que tienen determinantes de género. Por ejemplo, el prefijo de la palabra en español "mosca" es "la", ya que "mosca" es un nombre femenino en español. Esto solo depende de la traducción, y no del origen. Si no hay prefijo, la cadena quedará vacía.
    
    * `backTranslations`: es una lista de "traducciones inversas" del destino. Por ejemplo, las palabras de origen que se pueden traducir al idioma de destino. La lista garantiza que se encontrará la palabra de origen que se solicitó (por ejemplo, si la palabra de origen que se busca es "fly", entonces se garantiza que "fly" estará en la lista `backTranslations`). Sin embargo, no se garantiza que esté en la primera posición, y a menudo no lo estará. Cada elemento de la lista `backTranslations` es un objeto que consta de las siguientes propiedades:

        * `normalizedText`: una cadena que proporciona la forma normalizada del término de origen, que es a su vez una traducción inversa del destino. Este valor debe usarse como entrada para [buscar ejemplos](./v3-0-dictionary-examples.md).        

        * `displayText`: una cadena que proporciona el término de origen que es una traducción inversa del destino, y que se muestra de la forma más adecuada para el usuario final.

        * `numExamples`: es un entero que representa el número de ejemplos que están disponibles para este par de traducción. Los ejemplos reales deben obtenerse con una llamada independiente a la opción [buscar ejemplos](./v3-0-dictionary-examples.md). El número está diseñado principalmente para facilitar la presentación en un diseño UX. Por ejemplo, una interfaz de usuario puede agregar un hipervínculo a la traducción inversa si el número de ejemplos es mayor que cero, y si se muestra la traducción inversa como texto sin formato si es que no hay ejemplos. Tenga en cuenta que el número real de ejemplos que devuelve una llamada a la opción [buscar ejemplos](./v3-0-dictionary-examples.md) puede ser menor que `numExamples`, ya que se puede aplicar un filtro adicional sobre la marcha para quitar ejemplos que sean "incorrectos".
        
        * `frequencyCount`: un entero que representa la frecuencia de este par de traducción en los datos. El propósito principal de este campo es proporcionar una interfaz de usuario con un medio para ordenar traducciones inversas, y así conseguir que los términos más frecuentes aparezcan en primer lugar.

    > [!NOTE]
    > Si el término que está buscando no está en el diccionario, la respuesta es 200 (OK), pero la lista `translations` será una lista vacía.

## <a name="examples"></a>Ejemplos

En este ejemplo se muestra cómo buscar traducciones alternativas en español del término inglés `fly` .

```curl
curl -X POST "https://api.cognitive.microsofttranslator.com/dictionary/lookup?api-version=3.0&from=en&to=es" -H "Ocp-Apim-Subscription-Key: <client-secret>" -H "Content-Type: application/json" -d "[{'Text':'fly'}]"
```

El cuerpo de la respuesta (abreviado para mayor claridad) es:

```
[
    {
        "normalizedSource":"fly",
        "displaySource":"fly",
        "translations":[
            {
                "normalizedTarget":"volar",
                "displayTarget":"volar",
                "posTag":"VERB",
                "confidence":0.4081,
                "prefixWord":"",
                "backTranslations":[
                    {"normalizedText":"fly","displayText":"fly","numExamples":15,"frequencyCount":4637},
                    {"normalizedText":"flying","displayText":"flying","numExamples":15,"frequencyCount":1365},
                    {"normalizedText":"blow","displayText":"blow","numExamples":15,"frequencyCount":503},
                    {"normalizedText":"flight","displayText":"flight","numExamples":15,"frequencyCount":135}
                ]
            },
            {
                "normalizedTarget":"mosca",
                "displayTarget":"mosca",
                "posTag":"NOUN",
                "confidence":0.2668,
                "prefixWord":"",
                "backTranslations":[
                    {"normalizedText":"fly","displayText":"fly","numExamples":15,"frequencyCount":1697},
                    {"normalizedText":"flyweight","displayText":"flyweight","numExamples":0,"frequencyCount":48},
                    {"normalizedText":"flies","displayText":"flies","numExamples":9,"frequencyCount":34}
                ]
            },
            //
            // ...list abbreviated for documentation clarity
            //
        ]
    }
]
```

En este ejemplo se muestra lo que sucede cuando el término que se va a buscar no existe para el par de diccionario válido.

```curl
curl -X POST "https://api.cognitive.microsofttranslator.com/dictionary/lookup?api-version=3.0&from=en&to=es" -H "X-ClientTraceId: 875030C7-5380-40B8-8A03-63DACCF69C11" -H "Ocp-Apim-Subscription-Key: <client-secret>" -H "Content-Type: application/json" -d "[{'Text':'fly123456'}]"
```

Dado que el término no se encuentra en el diccionario, el cuerpo de la respuesta incluye la lista vacía `translations`.

```
[
    {
        "normalizedSource":"fly123456",
        "displaySource":"fly123456",
        "translations":[]
    }
]
```

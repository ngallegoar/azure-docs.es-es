---
title: Método de ejemplos de diccionario de Translator Text API
titleSuffix: Azure Cognitive Services
description: El método Dictionary Examples de Translator Text API ofrece ejemplos que muestran cómo se utilizan los términos del diccionario en contexto.
services: cognitive-services
author: swmachan
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: reference
ms.date: 01/21/2020
ms.author: swmachan
ms.openlocfilehash: b3551a8df19e47178c7bacd9218cfa60b66d81f9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "76548074"
---
# <a name="translator-text-api-30-dictionary-examples"></a>Translator Text API 3.0: Ejemplos de diccionario

Proporciona ejemplos que muestran cómo se usan los términos del diccionario en el contexto. Esta operación se usa junto con la opción [Búsqueda de diccionario](./v3-0-dictionary-lookup.md).

## <a name="request-url"></a>URL de la solicitud

Envíe una solicitud `POST` a:

```HTTP
https://api.cognitive.microsofttranslator.com/dictionary/examples?api-version=3.0
```

## <a name="request-parameters"></a>Parámetros de solicitud

Los parámetros de solicitud que se pasaron en la cadena de consulta son:

| Parámetro de consulta | Descripción |
| --------- | ----------- |
| api-version <img width=200/> | **Parámetro obligatorio**.<br/>Versión de la API que el cliente solicitó. El valor debe ser `3.0`. |
| desde | **Parámetro obligatorio**.<br/>Especifica el idioma del texto de entrada. El idioma de origen debe ser uno de los [idiomas admitidos](./v3-0-languages.md) que están incluidos en el ámbito `dictionary`. |
| to | **Parámetro obligatorio**.<br/>Especifica el idioma del texto de salida. El idioma de destino debe ser uno de los [idiomas admitidos](./v3-0-languages.md) que están incluidos en el ámbito `dictionary`.  | 

Los encabezados de solicitud incluyen lo siguiente:

| encabezados  | Descripción |
| ------ | ----------- |
| Encabezados de autenticación <img width=200/>  | **Encabezado de solicitud obligatorio**.<br/>Consulte las <a href="https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-reference#authentication">opciones disponibles para la autenticación</a>. |
| Content-Type | **Encabezado de solicitud obligatorio**.<br/>Especifica el tipo de contenido de la carga. Los valores posibles son: `application/json`. |
| Content-Length   | **Encabezado de solicitud obligatorio**.<br/>Longitud del cuerpo de la solicitud. |
| X-ClientTraceId   | **Opcional**.<br/>GUID generado por el cliente para identificar de forma única la solicitud. Puede omitir este encabezado si incluye el id. de seguimiento en la cadena de la consulta mediante un parámetro de consulta denominado `ClientTraceId`. |

## <a name="request-body"></a>Cuerpo de la solicitud

El cuerpo de la solicitud es una matriz JSON. Cada elemento de la matriz es un objeto JSON que consta de las siguientes propiedades:

  * `Text`: una cadena que especifica el término que se va a buscar. Este debería ser el valor de un campo `normalizedText` que proceda de las traducciones inversas encontradas cuando se realizó una solicitud de una [búsqueda de diccionario](./v3-0-dictionary-lookup.md) anterior. También puede ser el valor del campo `normalizedSource`.

  * `Translation`: una cadena que especifica el texto traducido, y que previamente devolvió la operación [Búsqueda de diccionario](./v3-0-dictionary-lookup.md). Este debería ser el valor del campo `normalizedTarget` en la lista `translations` de la respuesta [Búsqueda de diccionario ](./v3-0-dictionary-lookup.md). El servicio devolverá ejemplos para el par de palabras específico "fuente-objetivo".

Ejemplo:

```json
[
    {"Text":"fly", "Translation":"volar"}
]
```

Se aplican las siguientes limitaciones:

* La matriz puede tener como máximo 10 elementos.
* El valor de texto de un elemento de la matriz no puede superar los 100 caracteres, incluyendo los espacios.

## <a name="response-body"></a>Response body

Una respuesta correcta es una matriz JSON con un resultado para cada cadena en la matriz de entrada. Un objeto del resultado incluye las siguientes propiedades:

  * `normalizedSource`: es una cadena que proporciona el formato normalizado del término de origen. En general, esto debería ser idéntico al valor del campo `Text` en el índice de la lista de coincidencias del cuerpo de la solicitud.
    
  * `normalizedTarget`:es una cadena que da la forma normalizada del término de destino. En general, esto debería ser idéntico al valor del campo `Translation` en el índice de la lista de coincidencias del cuerpo de la solicitud.
  
  * `examples`: es una lista de ejemplos para el par (término de origen y de destino). Cada elemento de la lista es un objeto que consta de las siguientes propiedades:

    * `sourcePrefix`: es la cadena que hay que concatenar _antes_ del valor de `sourceTerm` para formar un ejemplo completo. No agregue un carácter de espacio, ya que ya estará allí en el momento apropiado. Este valor puede ser una cadena vacía.

    * `sourceTerm`: es una cadena equivalente al término real que se esta buscando. La cadena se agrega con `sourcePrefix` y `sourceSuffix` para formar el ejemplo completo. Su valor está separado, por lo que se puede marcar en una interfaz de usuario poniéndolo en negrita, por ejemplo.

    * `sourceSuffix`: es la cadena que hay que concatenar _después_ del valor de `sourceTerm` para formar un ejemplo completo. No agregue un carácter de espacio, ya que ya estará allí en el momento apropiado. Este valor puede ser una cadena vacía.

    * `targetPrefix`: es una cadena similar a `sourcePrefix`, pero se usa en el destino.

    * `targetTerm`: es una cadena similar a `sourceTerm`, pero se usa en el destino.

    * `targetSuffix`: es una cadena similar a `sourceSuffix`, pero se usa en el destino.

    > [!NOTE]
    > Si no hay ejemplos en el diccionario, la respuesta es 200 (OK), pero la lista `examples` será una lista vacía.

## <a name="examples"></a>Ejemplos

En este ejemplo se muestra cómo buscar ejemplos de la pareja formada por el término inglés `fly` y su traducción al español `volar`.

```curl
curl -X POST "https://api.cognitive.microsofttranslator.com/dictionary/examples?api-version=3.0&from=en&to=es" -H "Ocp-Apim-Subscription-Key: <client-secret>" -H "Content-Type: application/json" -d "[{'Text':'fly', 'Translation':'volar'}]"
```

El cuerpo de la respuesta (abreviado para mayor claridad) es:

```
[
    {
        "normalizedSource":"fly",
        "normalizedTarget":"volar",
        "examples":[
            {
                "sourcePrefix":"They need machines to ",
                "sourceTerm":"fly",
                "sourceSuffix":".",
                "targetPrefix":"Necesitan máquinas para ",
                "targetTerm":"volar",
                "targetSuffix":"."
            },      
            {
                "sourcePrefix":"That should really ",
                "sourceTerm":"fly",
                "sourceSuffix":".",
                "targetPrefix":"Eso realmente debe ",
                "targetTerm":"volar",
                "targetSuffix":"."
            },
            //
            // ...list abbreviated for documentation clarity
            //
        ]
    }
]
```

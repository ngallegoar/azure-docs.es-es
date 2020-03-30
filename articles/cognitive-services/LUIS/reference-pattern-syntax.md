---
title: 'Referencia de la sintaxis del patrón: LUIS'
titleSuffix: Azure Cognitive Services
description: Cree entidades para extraer datos clave de las expresiones de usuario de las aplicaciones de Language Understanding (LUIS). La aplicación cliente utiliza los datos extraídos.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: reference
ms.date: 12/09/2019
ms.author: diberry
ms.openlocfilehash: 696f4bdc22bed01a4b5be8bff63ade482a8dbe0a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "79218710"
---
# <a name="pattern-syntax"></a>Sintaxis del patrón

La sintaxis del patrón es una plantilla para una expresión. La plantilla debe contener las palabras y entidades que quiere que coincidan, así como las palabras y los signos de puntuación que quiere omitir. **No** es una expresión regular.

> [!CAUTION]
> Los patrones solo incluyen elementos primarios de la entidad de aprendizaje automático, no los subcomponentes.

Las entidades en los patrones aparecen entre llaves, `{}`. Los patrones pueden incluir entidades y entidades con roles. [Pattern.any](luis-concept-entity-types.md#patternany-entity) es una entidad que solo se usa en los patrones.

La sintaxis de patrón admite la sintaxis siguiente:

|Función|Sintaxis|Nivel de anidamiento|Ejemplo|
|--|--|--|--|
|Entidad| {}: llaves|2|Where is form {entity-name}?|
|opcional|[]: corchetes<BR><BR>Hay un límite de 3 niveles de anidamiento de cualquier combinación de opcional y agrupación. |2|El signo de interrogación es opcional [?]|
|agrupación|(): paréntesis|2|es (a \| b)|
|or| \|: barra vertical<br><br>Hay un límite de 2 en las barras verticales (o) en un grupo. |-|Where is form ({form-name-short} &#x7c; {form-name-long} &#x7c; {form-number})|
|principio o final de la expresión|^: símbolo de intercalación|-|^comienzo de la expresión<br>la expresión está terminada^<br>^coincidencia estricta de literal de la expresión entera con la entidad {number}^|

## <a name="nesting-syntax-in-patterns"></a>Sintaxis de anidamiento en los patrones

La sintaxis **opcional**, con corchetes, se puede anidar dos niveles. Por ejemplo: `[[this]is] a new form`. Este ejemplo permite las expresiones siguientes:

|Ejemplo de expresión opcional anidada|Explicación|
|--|--|
|this is a new form|coincide con todas las palabras del patrón|
|is a new form|coincide con la palabra opcional externa y las palabras no opcionales del patrón|
|a new form|solo coincide con las palabras necesarias|

La sintaxis de **agrupación**, con paréntesis, se puede anidar dos niveles. Por ejemplo: `(({Entity1.RoleName1} | {Entity1.RoleName2} ) | {Entity2} )`. Esta característica permite buscar la coincidencia con cualquiera de las tres entidades.

Si Entity1 es una ubicación con roles como origen (Seattle) y destino (El Cairo) y Entity2 es un nombre de edificio conocido de una entidad de la lista (RedWest-C), las expresiones siguientes se asignarían a este patrón:

|Ejemplo de expresión de agrupación anidada|Explicación|
|--|--|
|RedWest-C|coincide con la entidad de agrupación externa|
|Seattle|coincide con una de las entidades de agrupación interna|
|El Cairo|coincide con una de las entidades de agrupación interna|

## <a name="nesting-limits-for-groups-with-optional-syntax"></a>Límites de anidamiento de grupos con sintaxis opcional

Una combinación de sintaxis de **agrupación** con **opcional** tiene un límite de 3 niveles de anidamiento.

|Permitida|Ejemplo|
|--|--|
|Sí|( [ ( test1 &#x7c; test2 ) ] &#x7c; test3 )|
|No|( [ ( [ test1 ] &#x7c; test2 ) ] &#x7c; test3 )|

## <a name="nesting-limits-for-groups-with-or-ing-syntax"></a>Límites de anidamiento para grupos con sintaxis or-ing

Una combinación de sintaxis de **agrupación** con sintaxis **or-ing** tiene un límite de 2 barras verticales.

|Permitida|Ejemplo|
|--|--|
|Sí|( test1 &#x7c; test2 &#x7c; ( test3 &#x7c; test4 ) )|
|No|( test1 &#x7c; test2 &#x7c; test3 &#x7c; ( test4 &#x7c; test5 ) ) |

## <a name="syntax-to-add-an-entity-to-a-pattern-template"></a>Sintaxis para agregar una entidad a una plantilla de patrón
Para agregar una entidad a la plantilla de patrón, incluya el nombre de la entidad entre llaves, por ejemplo, `Who does {Employee} manage?`.

|Patrón con entidad|
|--|
|`Who does {Employee} manage?`|

## <a name="syntax-to-add-an-entity-and-role-to-a-pattern-template"></a>Sintaxis para agregar una entidad y un rol a una plantilla de patrón
Un rol de entidad se representa como `{entity:role}`, donde el nombre de la entidad va seguido por un signo de dos puntos y, a continuación, el nombre del rol. Para agregar una entidad con un rol a la plantilla de patrón, incluya el nombre de la entidad y el nombre del rol entre llaves, por ejemplo, `Book a ticket from {Location:Origin} to {Location:Destination}`.

|Patrón con roles de entidad|
|--|
|`Book a ticket from {Location:Origin} to {Location:Destination}`|

## <a name="syntax-to-add-a-patternany-to-pattern-template"></a>Sintaxis para agregar pattern.any a una plantilla de patrón
La entidad pattern.any permite agregar una entidad de longitud variable al patrón. Mientras se siga la plantilla de patrón, pattern.any puede tener cualquier longitud.

Para agregar una entidad **pattern.any** a la plantilla de patrón, incluya dicha entidad entre llaves, por ejemplo, `How much does {Booktitle} cost and what format is it available in?`.

|Patrón con entidad Pattern.any|
|--|
|`How much does {Booktitle} cost and what format is it available in?`|

|Títulos de libros en el patrón|
|--|
|¿Cuánto cuesta **robar este libro** y en qué formato está disponible?|
|¿Cuánto cuesta **preguntar** y en qué formato está disponible?|
|¿Cuánto cuesta **El curioso incidente del perro en la noche** y en qué formato está disponible?|

Las palabras del título de un libro no resultan confusas para LUIS porque sabe dónde finaliza el título del libro, en función de la entidad Pattern.any.

## <a name="explicit-lists"></a>Listas explícitas

Cree una [lista explícita](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5ade550bd5b81c209ce2e5a8) mediante la API de creación para permitir la excepción cuando:

* El patrón contenga una entidad [Pattern.any](luis-concept-entity-types.md#patternany-entity).
* Y la sintaxis de ese patrón permita la posibilidad de una extracción de entidades incorrecta según la expresión.

Por ejemplo, suponga que tiene un patrón que contiene tanto la sintaxis opcional, `[]`, como la sintaxis de la entidad, `{}`, combinadas para extraer datos de forma incorrecta.

Considere el patrón `[find] email about {subject} [from {person}]'.

En las siguientes expresiones, las entidades **subject** y **person** se extraen de forma correcta e incorrecta:

|Expresión|Entidad|Extracción correcta|
|--|--|:--:|
|email about dogs from Chris|subject=dogs<br>person=Chris|✔|
|email about the man from La Mancha|subject=the man<br>person=La Mancha|X|

En la tabla anterior, el asunto debería ser `the man from La Mancha` (el título de un libro), pero como el asunto incluye la palabra opcional `from`, el título se predice de forma incorrecta.

Para corregir esta excepción al patrón, agregue `the man from la mancha` como una coincidencia de lista explícita de la entidad {subject} mediante la [API de creación de lista explícita](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5ade550bd5b81c209ce2e5a8).

## <a name="syntax-to-mark-optional-text-in-a-template-utterance"></a>Sintaxis para marcar texto opcional en una expresión de plantilla
Marque texto opcional en la expresión mediante la sintaxis de corchetes de expresión regular, `[]`. El texto opcional solo puede anidar dos corchetes.

|Patrón con texto opcional|Significado|
|--|--|
|`[find] email about {subject} [from {person}]`|`find` y `from {person}` son opcionales.|
|"¿Puede ayudarme[?]|El signo de puntuación es opcional.|

Los signos de puntuación (`?`, `!`, `.`) se deben omitir y deberá omitirlos usando la sintaxis de corchetes en los patrones.

## <a name="next-steps"></a>Pasos siguientes

Más información sobre los patrones:

* [Incorporación de patrones](luis-how-to-model-intent-pattern.md)
* [Incorporación de la entidad pattern.any](luis-how-to-add-entities.md#add-a-patternany-entity)
* [Conceptos de patrones](luis-concept-patterns.md)

Comprenda cómo se devuelve [opinión](luis-reference-prebuilt-sentiment.md) en la respuesta .json.
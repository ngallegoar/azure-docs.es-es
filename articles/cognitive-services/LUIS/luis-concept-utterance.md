---
title: 'Expresiones de ejemplo correctas: LUIS'
titleSuffix: Azure Cognitive Services
description: Las expresiones son datos proporcionados por el usuario que la aplicación necesita interpretar. Recopile frases que crea que los usuarios pueden escribir. Incluya expresiones que signifiquen lo mismo, pero que se construyan de forma diferente tanto en longitud de palabras como en el orden de las palabras.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 10/15/2019
ms.author: diberry
ms.openlocfilehash: 7412677773b60a1894a6ece7251e797bfddee091
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "79218778"
---
# <a name="understand-what-good-utterances-are-for-your-luis-app"></a>Comprender cuáles son las expresiones correctas para la aplicación de LUIS

Las **expresiones** son datos proporcionados por el usuario que la aplicación necesita interpretar. Para entrenar a LUIS para que extraiga intenciones y entidades de ellas, es importante capturar varias expresiones de ejemplo diferentes por cada intención. El aprendizaje activo o el proceso de entrenar continuamente en nuevas expresiones es esencial para la inteligencia de aprendizaje automático que LUIS proporciona.

Recopile expresiones que crea que los usuarios pueden escribir. Incluya expresiones que significan lo mismo, pero que se construyen de maneras diferentes:

* Longitud de expresión: corta, media o larga para la aplicación cliente
* Longitud de la palabra y frase 
* Ubicación de las palabras: entidad al principio, en el medio o al final de una expresión
* Gramática 
* Pluralización
* Raíz
* Elección del nombre y del verbo
* Signos de puntuación: una buena variedad con un uso correcto, incorrecto y sin gramática

## <a name="how-to-choose-varied-utterances"></a>Cómo elegir expresiones variadas

Cuando empiece por primera vez a [agregar expresiones de ejemplo](luis-how-to-add-example-utterances.md) al modelo de LUIS, debe tener en cuenta algunos de los principios siguientes.

### <a name="utterances-arent-always-well-formed"></a>Las expresiones no siempre tienen el formato correcto

Puede ser una frase, como "Reservar un billete a París para mí" o un fragmento de una frase, como "Reservar" o "Vuelo a París".  Los usuarios a menudo cometen errores ortográficos. Al planear la aplicación, tenga en cuenta si usar [Bing Spell Check](luis-tutorial-bing-spellcheck.md) para revisar la ortografía de la entrada del usuario o no, antes de pasarla a LUIS. 

Si no revisa la ortografía de las expresiones del usuario, debe entrenar a LUIS en expresiones que incluyan errores tipográficos y faltas de ortografía.

### <a name="use-the-representative-language-of-the-user"></a>Utilice el lenguaje representativo del usuario

Al elegir expresiones, tenga en cuenta que lo que cree que es un término o frase común tal vez no sea correcto para el típico usuario de la aplicación cliente. Es posible que no tenga experiencia de dominio. Preste atención al utilizar términos o frases que solo las diría un experto.

### <a name="choose-varied-terminology-as-well-as-phrasing"></a>Elija terminología así como frases variadas

Observará que aunque realice esfuerzos para crear patrones de frases variadas,se seguirá repitiendo algo de vocabulario.

Considere las siguientes expresiones de ejemplo:

|Expresiones de ejemplo|
|--|
|¿Cómo puedo obtener un equipo?|
|¿Dónde puedo obtener un equipo?|
|Quiero obtener un equipo, ¿cómo lo hago?|
|¿Cuándo puedo tener un equipo?| 

En este caso, el término principal, "equipo", no varía. Use alternativas como "equipo de escritorio", "portátil", "estación de trabajo" o incluso simplemente "máquina". LUIS puede deducir de forma inteligente los sinónimos a partir del contexto, pero cuando se crean expresiones para el entrenamiento, siempre es mejor variarlas.

## <a name="example-utterances-in-each-intent"></a>Expresiones de ejemplo en cada intención

Cada intención debe tener expresiones de ejemplo, al menos 15. Si tiene una intención que no tiene ninguna expresión de ejemplo, no podrá entrenar a LUIS. Si dispone de una intención con una o muy pocas expresiones de ejemplo, es posible que LUIS no pueda predecirla con precisión. 

## <a name="add-small-groups-of-15-utterances-for-each-authoring-iteration"></a>Adición de grupos pequeños de 15 expresiones para cada iteración de creación

En cada iteración del modelo, no agregue una gran cantidad de expresiones. Agregue expresiones en grupos de 15. [Entrene](luis-how-to-train.md), [publique](luis-how-to-publish-app.md) y vuelva a [realizar pruebas](luis-interactive-test.md).  

LUIS compila modelos efectivos con expresiones seleccionadas cuidadosamente por su autor de modelos. Agregar demasiadas expresiones no resulta útil porque genera confusión.

Es mejor empezar con pocas expresiones y, luego, [revisar las expresiones del punto de conexión](luis-how-to-review-endpoint-utterances.md) para que la extracción de la entidad y la predicción de intención se realicen correctamente.

## <a name="utterance-normalization"></a>Normalización de expresiones

La normalización de expresiones es el proceso de omitir los efectos de la puntuación y los signos diacríticos durante el entrenamiento y la predicción.

## <a name="utterance-normalization-for-diacritics-and-punctuation"></a>Normalización de expresiones los signos diacríticos y la puntuación

La normalización de expresiones se define al crear o importar la aplicación porque es una configuración del archivo JSON de la aplicación. La configuración de la normalización de expresiones está desactivada de forma predeterminada. 

Los signos diacríticos son marcas o signos dentro del texto, como: 

```
İ ı Ş Ğ ş ğ ö ü
```

Si la aplicación activa la normalización, las puntuaciones del panel **Prueba**, las pruebas por lotes y las consultas de puntos de conexión cambiarán en todas las expresiones que usan signos diacríticos o puntuación.

Active la normalización de expresiones para los signos diacríticos o la puntuación en el archivo de aplicación JSON de LUIS en el parámetro `settings`.

```JSON
"settings": [
    {"name": "NormalizePunctuation", "value": "true"},
    {"name": "NormalizeDiacritics", "value": "true"}
] 
```

Normalizar la **puntuación** significa que antes de que los modelos se entrenen y antes de que las consultas de punto de conexión se predigan, la puntuación se quitará de las expresiones. 

Al normalizar los **signos diacríticos** se reemplazan los caracteres con signos diacríticos en las expresiones por caracteres normales. Por ejemplo, `Je parle français` pasa a ser `Je parle francais`. 

La normalización no significa que deje de ver la puntuación y los signos diacríticos en sus expresiones o respuestas de predicción de ejemplo, sino que simplemente se omiten durante el entrenamiento y la predicción.


### <a name="punctuation-marks"></a>Signos de puntuación

Los signos de puntuación son un token independiente de LUIS. Una expresión que contiene un punto final frente a una expresión que no lo tiene son dos expresiones distintas y pueden tener predicciones diferentes. 

Si la puntuación no se normaliza, LUIS tiene en cuenta los signos de puntuación de forma predeterminada, ya que es posible que estos signos tengan una mayor importancia en algunas aplicaciones cliente. Asegúrese de que haya expresiones de ejemplo con y sin signos de puntuación ellos para que los dos estilos devuelvan los mismos resultados relativos. 

Asegúrese de que el modelo controla los signos de puntuación, ya sea en las expresiones de ejemplo (que los tengan o no) o en los [patrones](luis-concept-patterns.md), donde es más sencillo pasar por alto los signos de puntuación con la sintaxis especial: `I am applying for the {Job} position[.]`.

Si la puntuación no tiene un significado concreto en la aplicación cliente, podría [omitir la puntuación](#utterance-normalization) mediante la normalización de la puntuación. 

### <a name="ignoring-words-and-punctuation"></a>Omisión de palabras y puntuación

Si quiere omitir palabras o signos de puntuación específicos en los patrones, use un [patrón](luis-concept-patterns.md#pattern-syntax) con la sintaxis _ignore_ de corchetes, `[]`. 

## <a name="training-utterances"></a>Expresiones de entrenamiento

El entrenamiento no es generalmente determinista: la predicción de expresiones podría variar ligeramente entre versiones o aplicaciones. Puede quitar el aprendizaje no determinista actualizando la [API de configuración de versión](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/versions-update-application-version-settings) con el par nombre-valor `UseAllTrainingData` para que así pueda utilizar todos los datos de aprendizaje.

## <a name="testing-utterances"></a>Prueba de expresiones 

Los desarrolladores deben empezar a probar su aplicación de LUIS con tráfico real mediante el envío de expresiones a la URL de [punto de conexión de predicción](luis-how-to-azure-subscription.md). Estas expresiones se utilizan para mejorar el rendimiento de las intenciones y las entidades con la [revisión de expresiones](luis-how-to-review-endpoint-utterances.md). Las pruebas enviadas mediante el panel de pruebas del sitio web de LUIS no se envían a través del punto de conexión y, por lo tanto, no contribuyen al aprendizaje activo. 

## <a name="review-utterances"></a>Revisión de las expresiones

Una vez que el modelo esté entrenado, publicado y reciba consultas del [punto de conexión](luis-glossary.md#endpoint), [revise las expresiones](luis-how-to-review-endpoint-utterances.md) sugeridas por LUIS. LUIS selecciona expresiones del punto de conexión que tienen puntuaciones bajas, ya sea para la intención o para la entidad. 

## <a name="best-practices"></a>Procedimientos recomendados

Revise los [procedimientos recomendados](luis-concept-best-practices.md) y aplíquelos como parte del ciclo de creación regular.

## <a name="label-for-word-meaning"></a>Etiqueta para el significado de las palabras

Si la elección o la organización de las palabras es la misma pero no significa lo mismo, no la etiquete con la entidad. 

En las siguientes expresiones inglesas, la palabra `fair` es un homógrafo. Es decir, se escribe igual pero tiene un significado diferente:

|Expresión|
|--|
|¿Qué tipo de ferias locales se producen en el área de Seattle este verano?|
|¿La clasificación actual para la revisión de Seattle es razonable?|

Si quiere que una entidad de evento busque todos los datos de eventos, etiquete la palabra `fair` en la primera expresión, pero no en la segunda.


## <a name="next-steps"></a>Pasos siguientes
Consulte [Agregar expresiones de ejemplo](luis-how-to-add-example-utterances.md) para información sobre cómo entrenar una aplicación de LUIS para comprender las expresiones del usuario.


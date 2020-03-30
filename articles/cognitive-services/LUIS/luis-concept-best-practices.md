---
title: Prácticas recomendadas para la creación de una aplicación de LUIS
titleSuffix: Azure Cognitive Services
description: Obtenga información sobre los procedimientos recomendados para conseguir los mejores resultados del modelo de la aplicación de LUIS.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 10/25/2019
ms.author: diberry
ms.openlocfilehash: b4be79338db71ad83204fae971da0b77885a8070
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "74280923"
---
# <a name="best-practices-for-building-a-language-understanding-luis-app"></a>Procedimientos recomendados para crear aplicaciones de Language Understanding (LUIS)
Use el proceso de creación de aplicaciones para compilar la aplicación de LUIS: 

* Creación de modelos de idioma (intenciones y entidades)
* Adición de algunas expresiones de ejemplo de entrenamiento (15-30 por intención)
* Publicación en el punto de conexión
* Probar desde el punto de conexión 

Cuando la aplicación está [publicada](luis-how-to-publish-app.md), use el ciclo de vida de creación para agregar características, publicar y probar desde el punto de conexión. No empiece el siguiente ciclo de creación agregando más expresiones de ejemplo, ya que no permite que LUIS aprenda del modelo con expresiones de usuario del mundo real. 

No expanda las expresiones hasta que el conjunto actual de expresiones de ejemplo y punto de conexión devuelva resultados seguros y con predicciones altas. Mejore las puntuaciones mediante el [aprendizaje activo](luis-concept-review-endpoint-utterances.md). 




## <a name="do-and-dont"></a>Qué se debe hacer y qué no
En la lista siguiente, se incluyen los procedimientos recomendados para las aplicaciones de LUIS:

|Lo que es necesario hacer:|Lo que debe evitar:|
|--|--|
|[Definir diferentes intenciones](#do-define-distinct-intents)<br>[Agregar descriptores a intenciones](#do-add-descriptors-to-intents) |[Agregar varias expresiones de ejemplo a las intenciones](#dont-add-many-example-utterances-to-intents)<br>[Usar unas pocas entidades sencillas](#dont-use-few-or-simple-entities) |
|[Buscar un punto óptimo entre ser demasiado genérico y demasiado específico para cada intención](#do-find-sweet-spot-for-intents)|[Usar LUIS como una plataforma de aprendizaje](#dont-use-luis-as-a-training-platform)|
|[Compilar la aplicación de forma iterativa con versiones](#do-build-your-app-iteratively-with-versions)<br>[Crear entidades para la descomposición del modelo](#do-build-for-model-decomposition)|[Agregar varias expresiones de ejemplo del mismo formato, omitiendo otros formatos](#dont-add-many-example-utterances-of-the-same-format-ignoring-other-formats)|
|[Agregar patrones en iteraciones posteriores](#do-add-patterns-in-later-iterations)|[Combinar la definición de intenciones y entidades](#dont-mix-the-definition-of-intents-and-entities)|
|[Equilibrar las expresiones en todas las intenciones](#balance-your-utterances-across-all-intents), excepto en la intención None.<br>[Agregar expresiones de ejemplo a la intención None](#do-add-example-utterances-to-none-intent)|[Crear descriptores con todos los valores posibles](#dont-create-descriptors-with-all-the-possible-values)|
|[Aprovechar la característica de sugerencia para un aprendizaje activo](#do-leverage-the-suggest-feature-for-active-learning)|[Agregar demasiados patrones](#dont-add-many-patterns)|
|[Supervisar el rendimiento de la aplicación con la prueba por lotes](#do-monitor-the-performance-of-your-app)|[Entrenar y publicar al agregar cada expresión de ejemplo](#dont-train-and-publish-with-every-single-example-utterance)|

## <a name="do-define-distinct-intents"></a>Definir diferentes intenciones
Asegúrese de que el vocabulario de cada intención sea solo para esa intención y no se superponga con otra intención. Por ejemplo, si quiere que una aplicación controle preparativos de viaje como vuelos y hoteles, puede elegir que estas áreas de asuntos sean intenciones independientes o que tengan una misma intención con entidades de datos específicos dentro de la expresión.

Si el vocabulario entre dos intenciones es el mismo, combine la intención y use entidades. 

Considere las siguientes expresiones de ejemplo:

|Expresiones de ejemplo|
|--|
|Reservar un vuelo|
|Reservar un hotel|

`Book a flight` y `Book a hotel` utilizan el mismo vocabulario de `book a `. Este formato es el mismo, por lo que debería tratarse de la misma intención con las diferentes palabras de `flight` y `hotel` que las entidades extraídas. 

## <a name="do-add-descriptors-to-intents"></a>Agregar descriptores a intenciones

Los descriptores ayudan a describir características para una intención. Un descriptor puede ser una lista de frases de palabras que son significativas para esa intención o una entidad que es significativa para dicha intención. 

## <a name="do-find-sweet-spot-for-intents"></a>Buscar un punto óptimo para las intenciones
Use datos de predicción de LUIS para determinar si las intenciones se superponen. Las intenciones que se superponen confunden a LUIS. El resultado es que la intención con mayor puntuación es demasiado cercana a otra intención. Dado que LUIS no usa exactamente la misma ruta a través de los datos para el aprendizaje todas las veces, una intención que se superpone tiene una posibilidad de ser la primera o segunda en el aprendizaje. Quiere que la puntuación de la expresión de cada intención esté más alejada para que esto no suceda. Una buena distinción de las intenciones debería dar como resultado cada vez la intención superior que se esperaba. 
 
<a name="#do-build-the-app-iteratively"></a>

## <a name="do-build-your-app-iteratively-with-versions"></a>Compilar la aplicación de forma iterativa con versiones

Cada ciclo de creación debe estar en una nueva [versión](luis-concept-version.md), clonada de una versión existente. 

## <a name="do-build-for-model-decomposition"></a>Compilar para la descomposición del modelo

La descomposición del modelo tiene el siguiente proceso típico:

* crear una **intención** basada en las intenciones del usuario de la aplicación cliente.
* agregar entre 15 a 30 expresiones de ejemplo basadas en la entrada de usuario del mundo real
* etiquetar el concepto de datos de nivel superior en la expresión de ejemplo
* dividir el concepto de datos en subcomponentes
* agregar descriptores (características) a subcomponentes
* agregar descriptores (características) a la intención 

Cuando haya creado la intención y agregado las expresiones de ejemplo, en el siguiente ejemplo se describe la descomposición de entidades. 

Comience por identificar los conceptos de datos completos que quiere extraer en una expresión. Esta es la entidad con aprendizaje automático. A continuación, descomponga la frase en partes. Esto incluye la identificación de subcomponentes (como entidades), junto con descriptores y restricciones. 

Por ejemplo, si desea extraer una dirección, la entidad superior con aprendizaje automático podría llamarse `Address`. Al crear la dirección, identifique algunos de sus subcomponentes, como la dirección postal, la ciudad, el estado y el código postal. 

Siga descomponiendo esos elementos mediante la **restricción** del código postal a una expresión regular. Descomponga la dirección postal en partes del número de calle (con un número pregenerado), un nombre de calle y un tipo de calle. El tipo de calle se puede describir con una lista de **descriptores** como avenida, plaza, calle y vía.

La API de creación V3 permite la descomposición del modelo. 

## <a name="do-add-patterns-in-later-iterations"></a>Agregar patrones en iteraciones posteriores

Es necesario entender cómo se comporta la aplicación antes de agregar [patrones](luis-concept-patterns.md), ya que los patrones tienen mayor peso que las expresiones de ejemplo y sesgarán la confianza. 

Una vez que comprenda cómo se comporta la aplicación, agregue los patrones que se aplican a la aplicación. No es necesario agregarlos con cada [iteración](luis-concept-app-iteration.md). 

No existe ningún inconveniente en agregarlas al principio del diseño del modelo, pero es más fácil ver cómo cada patrón cambia el modelo después de que el modelo se haya probado con expresiones. 
 
<!--

### Phrase lists

[Phrase lists](luis-concept-feature.md) allow you to define dictionaries of words related to your app domain. Seed your phrase list with a few words then use the suggest feature so LUIS knows about more words in the vocabulary specific to your app. A Phrase List improves intent detection and entity classification by boosting the signal associated with words or phrases that are significant to your app. 

Don't add every word to the vocabulary since the phrase list isn't an exact match. 

For more information:
* Concept: [Phrase list features in your LUIS app](luis-concept-feature.md)
* How-to: [Use phrase lists to boost signal of word list](luis-how-to-add-features.md)



### Patterns

Real user utterances from the endpoint, very similar to each other, may reveal patterns of word choice and placement. The [pattern](luis-concept-patterns.md) feature takes this word choice and placement along with regular expressions to improve your prediction accuracy. A regular expression in the pattern allows for words and punctuation you intend to ignore while still matching the pattern. 

Use pattern's [optional syntax](luis-concept-patterns.md) for punctuation so punctuation can be ignored. Use the [explicit list](luis-concept-patterns.md#explicit-lists) to compensate for pattern.any syntax issues. 

For more information:
* Concept: [Patterns improve prediction accuracy](luis-concept-patterns.md)
* How-to: [How to add Patterns to improve prediction accuracy](luis-how-to-model-intent-pattern.md)
-->

<a name="balance-your-utterances-across-all-intents"></a>

## <a name="do-balance-your-utterances-across-all-intents"></a>Equilibrar las expresiones en todas las intenciones

Para que las predicciones de LUIS sean precisas, la cantidad de expresiones de ejemplo en cada intención (excepto para la intención None), debe ser relativamente igual. 

Si tiene una intención con 100 expresiones de ejemplo y una intención con 20 expresiones de ejemplo, la intención de las 100 expresiones tendrá una mayor tasa de predicción.  

## <a name="do-add-example-utterances-to-none-intent"></a>Agregar expresiones de ejemplo a la intención None

Esta es la intención de reserva, que indica todo lo que está fuera de la aplicación. Agregue una expresión de ejemplo en la intención None por cada 10 expresiones de ejemplo del resto de la aplicación de LUIS.

## <a name="do-leverage-the-suggest-feature-for-active-learning"></a>Aprovechar la característica de sugerencia para un aprendizaje activo

Use la [revisión de las expresiones del punto de conexión](luis-how-to-review-endpoint-utterances.md) del **aprendizaje activo** de forma habitual, en lugar de agregar más expresiones de ejemplo a las intenciones. Dado que la aplicación recibe de forma constante expresiones de punto de conexión, esta lista crece y cambia.

## <a name="do-monitor-the-performance-of-your-app"></a>Supervisar el rendimiento de la aplicación

Supervise la precisión de las predicciones mediante un conjunto de [pruebas de lote](luis-concept-batch-test.md). 

Mantenga un conjunto de expresiones que no se use como [expresiones de ejemplo](luis-concept-utterance.md) o de punto de conexión. Siga mejorando la aplicación del conjunto de pruebas. Adapte el conjunto de pruebas para que refleje expresiones de usuarios reales. Use este conjunto de pruebas para evaluar cada iteración o versión de la aplicación. 

## <a name="dont-add-many-example-utterances-to-intents"></a>Agregar varias expresiones de ejemplo a las intenciones

Una vez publicada la aplicación, agregue solo expresiones de aprendizaje activo en el proceso de ciclo de vida del desarrollo. Si las expresiones son demasiado similares, agregue un patrón. 

## <a name="dont-use-few-or-simple-entities"></a>No usar pocas entidades o entidades simples

Las entidades se crean para la extracción y predicción de datos. Es importante que cada intención tenga entidades con aprendizaje automático que describan los datos en la intención. Esto ayuda a LUIS a predecir la intención, incluso si la aplicación cliente no necesita usar la entidad extraída. 

## <a name="dont-use-luis-as-a-training-platform"></a>No usar LUIS como una plataforma de aprendizaje

LUIS es específico de un dominio de modelo de lenguaje. Esto no está pensado para que funcione como una plataforma general de aprendizaje de lenguaje natural. 

## <a name="dont-add-many-example-utterances-of-the-same-format-ignoring-other-formats"></a>No agregar varias expresiones de ejemplo del mismo formato, omitiendo otros formatos

LUIS espera variaciones en las expresiones de una intención. Las expresiones pueden variar y conservar el mismo significado general. Las variaciones pueden incluir la longitud de la expresión, así como la elección y ubicación de las palabras. 

|No usar el mismo formato|Usar un formato diferente|
|--|--|
|Comprar un billete a Seattle<br>Comprar un billete a París<br>Comprar un billete a Orlando|Comprar 1 billete a Seattle<br>Reservar dos asientos en el vuelo nocturno a París el próximo lunes<br>Quiero adquirir 3 billetes a Orlando para las vacaciones de primavera|

La segunda columna usa diferentes verbos (comprar, reservar y adquirir), distintas cantidades (1, dos y 3) y diferentes colocaciones de las palabras, pero todas tienen la misma intención de comprar billetes de avión para viajar. 

## <a name="dont-mix-the-definition-of-intents-and-entities"></a>No combinar la definición de intenciones y entidades

Cree una intención para cualquier acción que llevará a cabo el bot. Use entidades como parámetros que hacen posible esa acción. 

Para un bot que reservará vuelos, cree la intención **BookFlight**. No cree una intención para cada compañía aérea o destino. Use esos datos como [entidades](luis-concept-entity-types.md) y márquelos en las expresiones de ejemplo. 

## <a name="dont-create-descriptors-with-all-the-possible-values"></a>No crear descriptores con todos los valores posibles

Proporcione algunos ejemplos en las [listas de frases](luis-concept-feature.md), pero no todas las palabras. LUIS generaliza y tiene en cuenta el contexto. 

## <a name="dont-add-many-patterns"></a>No agregar muchos patrones

No agregue demasiados [patrones](luis-concept-patterns.md). LUIS está pensado para aprender rápidamente con pocos ejemplos. No sobrecargue el sistema innecesariamente.

## <a name="dont-train-and-publish-with-every-single-example-utterance"></a>No entrenar y publicar con cada expresión de ejemplo

Agregue 10 o 15 expresiones antes de entrenar y publicar. De esta forma, puede ver el impacto en la precisión de la predicción. Si agrega una única expresión, puede que no vea ningún impacto en la puntuación. 

## <a name="next-steps"></a>Pasos siguientes

* Obtenga información sobre cómo [planear la aplicación](luis-how-plan-your-app.md) en la aplicación de LUIS.

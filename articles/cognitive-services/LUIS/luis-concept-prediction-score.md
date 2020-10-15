---
title: 'Puntuaciones de predicción: LUIS'
description: Una puntuación de predicción indica el grado de confianza que tiene el servicio API de LUIS en los resultados de predicción, según una expresión del usuario.
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 04/14/2020
ms.openlocfilehash: d836273e61752ff208133466016ce7c6ff9c28fa
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "91316467"
---
# <a name="prediction-scores-indicate-prediction-accuracy-for-intent-and-entities"></a>Las puntuaciones de predicción indican la precisión de la predicción de las intenciones y las entidades.

Una puntuación de predicción indica el grado de confianza que tiene LUIS en los resultados de la predicción de una expresión del usuario.

Una puntuación de predicción se encuentra entre cero (0) y uno (1). Un ejemplo de una puntuación de LUIS de gran confianza es 0,99. Un ejemplo de una puntuación de confianza baja es 0,01.

|Valor de la puntuación|Confianza|
|--|--|
|1|coincidencia clara|
|0,99|confianza alta|
|0,01|confianza baja|
|0|falta de coincidencia clara|

## <a name="top-scoring-intent"></a>Intención de puntuación superior

Todas las predicciones de expresión devuelven una intención de puntuación superior. Esta predicción es una comparación numérica de las puntuaciones de predicción.

## <a name="proximity-of-scores-to-each-other"></a>Proximidad de puntuaciones entre sí

Puede haber una diferencia muy pequeña entre las dos puntuaciones mayores. LUIS no indica esta proximidad de otra forma que no sea devolver la mayor puntuación.

## <a name="return-prediction-score-for-all-intents"></a>Devolver la puntuación de predicción de todas las intenciones

Un resultado de prueba o punto de conexión puede incluir todas las intenciones. Esta configuración se establece en el punto de conexión con el par valor/nombre de la cadena de consulta correcta.

|Prediction API|Nombre de la cadena de consulta|
|--|--|
|V3|`show-all-intents=true`|
|V2|`verbose=true`|

## <a name="review-intents-with-similar-scores"></a>Revisar las intenciones con puntuaciones similares

Revisar la puntuación de todas las intenciones es una buena manera de comprobar que no solo se identifica la intención correcta, sino que la puntuación de la siguiente intención identificada es notable y sistemáticamente inferior en las expresiones.

Si varias intenciones tienen puntuaciones de predicción cercanas, en función del contexto de una expresión, LUIS puede alternar entre las intenciones. Para corregir esta situación, siga agregando expresiones a cada intención con una variedad más amplia de diferencias contextuales, o puede hacer que la aplicación cliente, como un bot de chat, realice elecciones de programación sobre cómo controlar las dos intenciones principales.

Las dos intenciones con puntuaciones muy cercanas se pueden invertir debido al **entrenamiento no determinista**. La puntuación superior podría convertirse en la segunda superior y la puntuación segunda podría convertirse en la puntuación superior. Con el fin de evitar esta situación, agregue expresiones de ejemplo a cada una de las dos intenciones principales para esa expresión con la elección de palabras y el contexto que las diferencia. Las dos intenciones deben tener aproximadamente el mismo número de expresiones de ejemplo. Una regla general de separación para evitar la inversión debida al aprendizaje sería una diferencia del 15 % en las puntuaciones.

Puede desactivar el **entrenamiento no determinista** mediante el [entrenamiento de todos los datos](luis-how-to-train.md#train-with-all-data).

## <a name="differences-with-predictions-between-different-training-sessions"></a>Diferencias con las predicciones entre sesiones de entrenamiento distintas

Si entrena el mismo modelo en otra aplicación y las puntuaciones no son las mismas, esta diferencia se debe a que hay **aprendizaje no determinista** (un elemento de aleatoriedad). En segundo lugar, cualquier superposición de una expresión a más de una intención significa que la intención superior de la misma expresión puede cambiar en función del aprendizaje.

Si el bot de chat requiere una puntuación de LUIS específica para indicar confianza en una intención, debe usar la diferencia de puntuación entre las dos intenciones principales. Esta situación proporciona flexibilidad para las variaciones en el entrenamiento.

Puede desactivar el **entrenamiento no determinista** mediante el [entrenamiento de todos los datos](luis-how-to-train.md#train-with-all-data).

## <a name="e-exponent-notation"></a>Notación E (exponencial)

Las puntuaciones de predicción pueden usar la notación exponencial, de forma que _aparecen_ por encima del intervalo 0-1, como `9.910309E-07`. Esta puntuación es una indicación de un número muy **pequeño**.

|Puntuación de notación E |Puntuación real|
|--|--|
|9,910309E-07|,0000009910309|

<a name="punctuation"></a>

## <a name="application-settings"></a>Configuración de la aplicación

Utilice la [configuración de la aplicación](luis-reference-application-settings.md) para controlar cómo los diacríticos y los signos de puntuación afectan a las puntuaciones de predicción.

## <a name="next-steps"></a>Pasos siguientes

Vea [Add entities](luis-how-to-add-entities.md) (Agregar entidades) para obtener más información sobre cómo agregar entidades a la aplicación de LUIS.

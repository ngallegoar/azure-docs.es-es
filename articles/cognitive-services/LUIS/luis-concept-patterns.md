---
title: 'Ayuda a la predicción de los patrones: LUIS'
titleSuffix: Azure Cognitive Services
description: Un patrón permite lograr más precisión en una intención sin proporcionar muchas más expresiones.
services: cognitive-services
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 11/11/2019
ms.openlocfilehash: 44a7a89740be3d6d502c5d22c79b46582034d88a
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "91535972"
---
# <a name="patterns-improve-prediction-accuracy"></a>Los patrones mejoran la precisión de las predicciones
Los patrones están diseñados para mejorar la precisión cuando varias expresiones son muy parecidas.  Un patrón permite lograr más precisión en una intención sin proporcionar muchas más expresiones.

## <a name="patterns-solve-low-intent-confidence"></a>Los patrones resuelven la confianza con intención baja
Piense en una aplicación de recursos humanos que informa sobre el organigrama en relación con un empleado. Dados el nombre y la relación del empleado, LUIS devuelve los empleados implicados. Considere un empleado, Tom, con una jefa llamada Alice, y un equipo de subalternos llamados: Michael, Rebecca y Carl.

![Imagen de organigrama](./media/luis-concept-patterns/org-chart.png)

|Grabaciones de voz|Intención pronosticada|Puntuación de intención|
|--|--|--|
|¿Quién es el subalterno de Tom?|GetOrgChart|.30|
|¿Quién es el subalterno de Tom?|GetOrgChart|.30|

Si una aplicación tiene entre 10 y 20 expresiones con distintas longitudes de oraciones, diferente orden de palabras, e incluso diferentes palabras (sinónimos de "subalterno", "administrar", "informe"), LUIS puede devolver una puntuación de confianza baja. Para ayudar a LUIS a comprender la importancia del orden de las palabras, cree un patrón.

Los patrones solucionan las situaciones siguientes:

* La puntuación de la intención es baja.
* La intención correcta no es la puntuación más alta, pero se acerca demasiado a ella.

## <a name="patterns-are-not-a-guarantee-of-intent"></a>Los patrones no son una garantía de intención
Los patrones emplean una combinación de tecnologías de predicción. Establecer una intención para una expresión de plantilla en un patrón no es una garantía de la predicción de la intención, pero es un buen indicio.

<a name="patterns-do-not-improve-entity-detection"></a>

## <a name="patterns-do-not-improve-machine-learning-entity-detection"></a>Los patrones no mejoran la detección de entidades de aprendizaje automático.

La única finalidad de un patrón es ayudar a la predicción de intenciones y roles. La entidad _pattern.any_ se usa para extraer entidades de forma libre. Si bien los patrones usan entidades, un patrón no ayuda a detectar una entidad de aprendizaje automático.

No espere ver una predicción de entidad mejorada si contrae varias expresiones en un patrón único. Para que las entidades simples se activen, tendrá que agregar expresiones o usar entidades de la lista; de lo contrario, no se activará el patrón.

## <a name="patterns-use-entity-roles"></a>Los patrones usan roles de entidad
Si dos o más entidades de un patrón están relacionadas por el contexto, los patrones usan los [roles](luis-concept-roles.md) de entidad para extraer información contextual sobre las entidades.

## <a name="prediction-scores-with-and-without-patterns"></a>Puntuaciones de predicción con y sin patrones
Dadas suficientes expresiones de ejemplo, LUIS podría aumentar la confianza de la predicción sin patrones. Los patrones aumentan la puntuación de confianza sin tener que proporcionar tantas expresiones.

## <a name="pattern-matching"></a>Coincidencia de patrones
La coincidencia con un patrón se basa en primer lugar en la detección de entidades dentro del patrón, para posteriormente validar el resto de las palabras y el orden de las palabras del patrón. Para que un patrón coincida, se necesitan las entidades en el patrón. El patrón se aplica a nivel de token, no a nivel de carácter.

## <a name="pattern-only-apps"></a>Aplicaciones de un único patrón
Puede crear una aplicación con intenciones que no tengan ninguna expresión de ejemplo, siempre y cuando haya un patrón para cada intención. En aplicaciones de un único patrón, el patrón no debe contener entidades de aprendizaje automático porque estas no requieren expresiones de ejemplo.

## <a name="patternany-entity"></a>Entidad Pattern.any

[!INCLUDE [Pattern.any entity - concepts](./includes/pattern-any-entity.md)]

## <a name="best-practices"></a>Procedimientos recomendados
Obtenga información sobre los [procedimientos recomendados](luis-concept-best-practices.md).

## <a name="pattern-syntax"></a>Sintaxis del patrón

Aprenda la sintaxis del patrón desde la [referencia de sintaxis del patrón](reference-pattern-syntax.md).

## <a name="next-steps"></a>Pasos siguientes

Más información sobre los patrones:

* [Incorporación de patrones](luis-how-to-model-intent-pattern.md)
* [Incorporación de la entidad pattern.any](luis-how-to-add-entities.md#add-a-patternany-entity)
* [Sintaxis de patrones](reference-pattern-syntax.md)

> [!div class="nextstepaction"]
> [Aprenda cómo implementar patrones en este tutorial](luis-tutorial-pattern.md)

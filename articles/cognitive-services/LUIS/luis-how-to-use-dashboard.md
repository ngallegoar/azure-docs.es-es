---
title: 'Panel: Language Understanding (LUIS)'
titleSuffix: Azure Cognitive Services
description: Corrija intenciones y entidades con el panel de la aplicación que se entrena. El panel muestra información general de la aplicación, con los aspectos destacados de las intenciones que se deben corregir.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 11/08/2019
ms.author: diberry
ms.openlocfilehash: d9ae126753f55349f9bf3eefd20bc4d222866af1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "73888213"
---
# <a name="how-to-use-the-dashboard-to-improve-your-app"></a>Cómo usar el Panel para mejorar la aplicación

Busque y corrija problemas con las intenciones de la aplicación entrenada cuando usa expresiones de ejemplo. El panel muestra información general de la aplicación, con los aspectos destacados de las intenciones que se deben corregir. 

La revisión del análisis del panel es un proceso iterativo, que se repite a medida que cambia y mejora el modelo.

Esta página no tendrá análisis pertinentes para las aplicaciones que no tengan expresiones de ejemplo en las intenciones, conocidas como aplicaciones de _solo patrón_. 

## <a name="what-issues-can-be-fixed-from-dashboard"></a>¿Qué incidencias se pueden corregir desde el panel?

Los tres problemas que trata el panel son:

|Problema|Color del gráfico|Explicación|
|--|--|--|
|Desequilibrio de datos|-|Esto ocurre cuando la cantidad de expresiones de ejemplo varía significativamente. Cada intención, excepto la intención None (Ninguno), debe tener _aproximadamente_ el mismo número de expresiones de ejemplo. Solo debe tener del 10 al 15 % de la cantidad total de expresiones de la aplicación.<br><br> Si los datos están desequilibrados, pero la precisión de la intención está por encima de un umbral determinado, este desequilibrio no se notifica como una incidencia.<br><br>**Comience con esta incidencia: puede ser la causa principal de las otras.**|
|Predicciones poco claras|Naranja|Esto sucede cuando las puntuaciones de la intención principal y la siguiente intención están tan próximas que podrían alternarse en el siguiente entrenamiento, debido a un [muestreo negativo](luis-how-to-train.md#train-with-all-data) o a que se han agregado más expresiones de ejemplo a la intención. |
|Predicciones incorrectas|Rojo|Esto se produce cuando no se predice una expresión de ejemplo para la intención etiquetada (la intención en la que está).|

Las predicciones correctas se representan con el color azul.

El panel muestra estas incidencias, le indica qué intenciones se ven afectadas y sugiere lo que debe hacer para mejorar la aplicación. 

## <a name="before-app-is-trained"></a>Antes de entrenar la aplicación 

Antes de entrenar la aplicación, el panel no contiene ninguna sugerencia para las correcciones. Entrene la aplicación para ver estas sugerencias.  

## <a name="check-your-publishing-status"></a>Comprobación del estado de publicación

La tarjeta **Estado de publicación** contiene información sobre la última publicación de la versión activa. 

Compruebe que la versión activa es la versión que desea corregir. 

![El panel muestra los servicios externos de la aplicación, las regiones en las que se ha publicado y las visitas agregadas al punto de conexión.](./media/luis-how-to-use-dashboard/analytics-card-1-shows-app-summary-and-endpoint-hits.png)

También muestra los servicios externos, las regiones en las que se ha publicado y las visitas agregadas al punto de conexión. 

## <a name="review-training-evaluation"></a>Revisión de la evaluación del entrenamiento

La tarjeta **Evaluación del entrenamiento** contiene el resumen agregado de la precisión general de la aplicación por área. La puntuación indica la calidad de la intención. 

![La tarjeta Evaluación del entrenamiento contiene la primera área de información sobre la precisión general de la aplicación.](./media/luis-how-to-use-dashboard/analytics-card-2-shows-app-overall-accuracy.png)

El gráfico indica las intenciones predichas correctamente y las áreas problemáticas con diferentes colores. A medida que mejora la aplicación con las sugerencias, esta puntuación aumenta. 

Las correcciones sugeridas se dividen por tipo de problema y son las más importantes para la aplicación. Si prefiere revisar y corregir las incidencias por intención, use la tarjeta **[Intenciones con errores](#intents-with-errors)** en la parte inferior de la página. 

Cada área problemática tiene intenciones que deben corregirse. Cuando selecciona el nombre de la intención, se abre la página **Intención** con un filtro aplicado a las expresiones. Este filtro le permite centrarse en las expresiones que causan el problema.

### <a name="compare-changes-across-versions"></a>Comparación de cambios entre versiones

Antes de realizar cambios en la aplicación, cree una nueva versión. En la nueva versión, realice los cambios sugeridos en las expresiones de ejemplo de la intención y luego vuelva a entrenar. En la tarjeta **Evaluación del entrenamiento** de la página del panel, use la opción **Mostrar cambios desde la versión entrenada** para comparar los cambios. 

![Comparación de cambios entre versiones](./media/luis-how-to-use-dashboard/compare-improvement-across-versions.png)

### <a name="fix-version-by-adding-or-editing-example-utterances-and-retraining"></a>Corrección de la versión mediante la adición o edición de las expresiones de ejemplo y un nuevo entrenamiento

El método principal para corregir la aplicación es agregar o editar las expresiones de ejemplo y volver a entrenar. Las expresiones nuevas o modificadas deben seguir las directrices de las [expresiones modificadas](luis-concept-utterance.md).

La adición de expresiones de ejemplo la debe realizar alguien que:

* Tenga un alto grado de conocimiento de las expresiones que hay en las distintas intenciones.
* Sepa cómo se podrían confundir las expresiones de una intención con otra intención.
* Pueda decidir si dos intenciones que se confunden con frecuencia se deberían contraer en una única intención. Si este es el caso, los distintos datos deben extraerse con entidades.

### <a name="patterns-and-phrase-lists"></a>Patrones y listas de frases

La página de análisis no indica cuándo usar [patrones](luis-concept-patterns.md) o [listas de frases](luis-concept-feature.md). Si se agregan, puede ayudar con las predicciones incorrectas o poco claras, pero no ayuda con el desequilibrio de datos. 

### <a name="review-data-imbalance"></a>Revisión del desequilibrio de datos

Comience con esta incidencia: puede ser la causa principal de las otras.

La lista de intenciones con **desequilibrio de datos** muestra las intenciones que necesitan más expresiones para corregir el desequilibrio de datos. 

**Para corregir esta incidencia**:

* Agregue más expresiones a la intención y vuelva a entrenar. 

No agregue expresiones a la intención None a menos que lo sugiera el panel.

> [!Tip]
> Utilice la tercera sección de la página, **Expresiones por intención** con el valor **Expresiones (número)** como una guía visual rápida de las intenciones que necesitan más expresiones.  
    ![Uso de "Expresiones (número)" para buscar las intenciones con desequilibrio de datos.](./media/luis-how-to-use-dashboard/predictions-per-intent-number-of-utterances.png)

### <a name="review-incorrect-predictions"></a>Revisión de predicciones incorrectas

La lista de intenciones con **predicción incorrecta** muestra las intenciones que tienen expresiones, que se utilizan como ejemplos para una intención específica, pero que se predicen para diferentes intenciones. 

**Para corregir esta incidencia**:

* Edite las expresiones para que sean más específicas para la intención y vuelva a entrenar.
* Combine las intenciones si las expresiones están muy alineadas y vuelva a entrenar.

### <a name="review-unclear-predictions"></a>Revisión de predicciones poco claras

La lista de intenciones con **predicción poco clara** muestra las intenciones con expresiones con puntuaciones de predicción que no se alejan lo suficiente de su rival más cercano o que puede cambiar la intención principal de la expresión en el siguiente entrenamiento, debido al [muestreo negativo](luis-how-to-train.md#train-with-all-data).

**Para corregir esta incidencia**:

* Edite las expresiones para que sean más específicas para la intención y vuelva a entrenar.
* Combine las intenciones si las expresiones están muy alineadas y vuelva a entrenar.

## <a name="utterances-per-intent"></a>Expresiones por intención

Esta tarjeta muestra el estado general de la aplicación en las distintas intenciones. A medida que corrige las intenciones y vuelve a entrenar, eche un vistazo a esta tarjeta en busca de incidencias.

El gráfico siguiente muestra una aplicación bien equilibrada con casi ninguna incidencia que corregir.

![El gráfico siguiente muestra una aplicación bien equilibrada con casi ninguna incidencia que corregir.](./media/luis-how-to-use-dashboard/utterance-per-intent-shows-data-balance.png)

El gráfico siguiente muestra una aplicación mal equilibrada con muchas incidencias que corregir.

![El gráfico siguiente muestra una aplicación bien equilibrada con casi ninguna incidencia que corregir.](./media/luis-how-to-use-dashboard/utterance-per-intent-shows-data-imbalance.png)

Mantenga el ratón sobre la barra de cada intención para obtener información sobre ella. 

![El gráfico siguiente muestra una aplicación bien equilibrada con casi ninguna incidencia que corregir.](./media/luis-how-to-use-dashboard/utterances-per-intent-with-details-of-errors.png)

Use la característica **Ordenar por** para organizar las intenciones por tipo de incidencia para que pueda centrarse en las intenciones más problemáticas con esa incidencia. 

## <a name="intents-with-errors"></a>Intenciones con errores

Esta tarjeta permite revisar las incidencias de una intención específica. La vista predeterminada de esta tarjeta son las intenciones más problemáticas, para que sepa dónde centrar sus esfuerzos.

![La tarjeta Intenciones con errores permite revisar las incidencias de una intención específica. De forma predeterminada, esta tarjeta se filtra por las intenciones más problemáticas, para que sepa dónde centrar sus esfuerzos.](./media/luis-how-to-use-dashboard/most-problematic-intents-with-errors.png)

El gráfico de anillos superior muestra las incidencias con la intención en los tres tipos de problemas. Si hay incidencias en los tres tipos de problemas, cada tipo tiene su propio gráfico a continuación, junto con las intenciones rivales. 

### <a name="filter-intents-by-issue-and-percentage"></a>Filtrado de intenciones por incidencia y porcentaje

Esta sección de la tarjeta permite buscar las expresiones de ejemplo que superan su umbral de error. Lo ideal es que quiera corregir las predicciones para que sean significativas. Ese porcentaje está orientado al negocio y el cliente. 

Determine los porcentajes del umbral que son adecuados para su negocio. 

El filtro le permite buscar las intenciones con una incidencia específica:

|Filter|Porcentaje sugerido|Propósito|
|--|--|--|
|Intenciones más problemáticas|-|**Comience aquí**: la corrección de las expresiones de esta intención mejorará la aplicación más que otras correcciones.|
|Predicciones correctas por debajo del|60%|Este es el porcentaje de expresiones de la intención seleccionada que son correctas pero tienen una puntuación de confianza por debajo del umbral. |
|Predicciones poco claras por encima del|15 %|Este es el porcentaje de expresiones de la intención seleccionada que se confunden con la intención rival más cercana.|
|Predicciones incorrectas por encima del|15 %|Este es el porcentaje de expresiones de la intención seleccionada que se predijeron incorrectamente. |

### <a name="correct-prediction-threshold"></a>Umbral de predicción correcta

¿Cuál es para usted es la puntuación de confianza de una predicción de confianza? Al principio del desarrollo de la aplicación, su objetivo puede ser el 60 %. Use la opción **Predicciones correctas por debajo del** con el porcentaje del 60 % para buscar las expresiones de la intención seleccionada que se deben corregir.

### <a name="unclear-or-incorrect-prediction-threshold"></a>Umbral de predicciones incorrectas o poco claras

Estos dos filtros permiten buscar expresiones de la intención seleccionada que superan el umbral. Puede considerar estos dos porcentajes como porcentajes de error. Si considera adecuada una tasa de error para las predicciones del 10 al 15 %, establezca el umbral del filtro en el 15 %, para buscar todas las expresiones por encima de este valor. 

## <a name="next-steps"></a>Pasos siguientes

* [Administración de los recursos de Azure](luis-how-to-azure-subscription.md)

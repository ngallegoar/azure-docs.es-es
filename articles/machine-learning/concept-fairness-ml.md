---
title: Evaluación y mitigación de la imparcialidad en los modelos de Machine Learning
titleSuffix: Azure Machine Learning
description: Obtenga información sobre la imparcialidad en los modelos de Machine Learning y cómo el paquete de Python de Fairlearn puede ayudarle a crear modelos más imparciales.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: luquinta
author: luisquintanilla
ms.date: 05/02/2020
ms.openlocfilehash: c21ec0329a7b5716a00262b7422296df3afe208b
ms.sourcegitcommit: bb0afd0df5563cc53f76a642fd8fc709e366568b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/19/2020
ms.locfileid: "83596364"
---
# <a name="fairness-in-machine-learning-models"></a>Imparcialidad en los modelos de Machine Learning

Obtenga información sobre la imparcialidad del aprendizaje automático y cómo el paquete de Python de código abierto de Fairlearn puede ayudarle a crear modelos que sean más imparciales.

## <a name="what-is-fairness-in-machine-learning-systems"></a>¿Qué es la imparcialidad en los sistemas de aprendizaje automático?

La inteligencia artificial y los sistemas de aprendizaje automático pueden mostrar un comportamiento parcial. Una manera de definir el comportamiento parcial es fijarse en el daño que hace o impacto que tiene en las personas. Existen muchos tipos de daños que los sistemas de IA pueden ocasionar. Estos son dos tipos comunes de daños que puede causar la IA:

- Daño en la asignación: un sistema de IA extiende o retiene oportunidades, recursos o información. Entre los ejemplos se incluyen la contratación, las admisiones escolares y los préstamos, donde un modelo podría ser mucho mejor a la hora de seleccionar buenos candidatos entre un grupo específico de personas que entre otros grupos.

- Daño en la calidad del servicio: un sistema de AI puede no funcionar tan bien para un grupo de personas como lo haría en otro. Por ejemplo, un sistema de reconocimiento de voz puede no funcionar correctamente para las mujeres, pero sí para los hombres.

Para reducir el comportamiento parcial de los sistemas de AI, tiene que evaluar y mitigar estos daños.

>[!NOTE]
> La imparcialidad es un desafío técnico y social. Muchos aspectos de la imparcialidad, tales como la justicia y el proceso de vencimiento, no se capturan en métricas cuantitativas de imparcialidad. Asimismo, muchas métricas cuantitativas de imparcialidad no se pueden satisfacer simultáneamente. El objetivo es permitir que los seres humanos evalúen distintas estrategias de mitigación y, a continuación, realicen intercambios que sean ventajosos para su escenario.

## <a name="fairness-assessment-and-mitigation-with-fairlearn"></a>Evaluación y mitigación de imparcialidad con Fairlearn

Fairlearn es un paquete de Python de código abierto que permite a los desarrolladores de sistemas de aprendizaje automático evaluar la imparcialidad de sus sistemas y mitigar los problemas de imparcialidad observados.

Fairlearn tiene dos componentes:

- Panel de evaluación: es un widget de Jupyter Notebook para evaluar cómo afectan las predicciones de un modelo a grupos diferentes. También permite comparar varios modelos mediante métricas de imparcialidad y rendimiento.
- Algoritmos de mitigación: es un conjunto de algoritmos para mitigar la imparcialidad en la clasificación y la regresión binarias.

Juntos, estos componentes permiten a los científicos de datos y a los líderes empresariales administrar cualquier intercambio entre imparcialidad y rendimiento, y seleccionar la estrategia de mitigación que mejor se adapte a sus necesidades.

## <a name="fairness-assessment"></a>Evaluación de imparcialidad

En Fairlearn, la imparcialidad se conceptualiza a través de un enfoque conocido como **imparcialidad de grupo**, en el cual debemos hacernos la siguiente pregunta: ¿Qué grupos de usuarios están en riesgo de experimentar daños?

Los grupos pertinentes, también conocidos como subpoblaciónes, se definen a través de **características confidenciales** o atributos confidenciales. Las características confidenciales se pasan a un estimador de Fairlearn como un vector o una matriz denominada `sensitive_features`. El término sugiere que el diseñador del sistema debe mantener la confidencialidad de estas características al evaluar la imparcialidad del grupo. Algo que se debe tener en cuenta, es si estas características contienen implicaciones de privacidad debido a la información de identificación personal. De todos modos, la palabra "confidencial" no implica que estas características no se utilicen para realizar predicciones.

Durante la fase de valoración, la imparcialidad se cuantifica a través de las métricas de disparidad. **Las métricas de disparidad** pueden evaluar y comparar el comportamiento del modelo en grupos diferentes, ya sea como proporciones o diferencias. Fairlearn admite dos clases de métricas de disparidad:


- Disparidad en el rendimiento del modelo: estos conjuntos de métricas calculan la disparidad (diferencia) en los valores de la métrica de rendimiento seleccionada en los distintos subgrupos. Estos son algunos ejemplos:

  - disparidad en la tasa de precisión
  - disparidad en la tasa de error
  - disparidad en la precisión
  - disparidad en la coincidencia
  - disparidad en la métrica MAE
  - muchos otros

- Disparidad en la tasa de selección: esta métrica contiene la diferencia de la tasa de selección entre distintos subgrupos. Un ejemplo de esto es la disparidad en la tasa de aprobación de préstamos. La tasa de selección indica la fracción de los puntos de referencia de cada clase clasificada como 1 (en la clasificación binaria) o la distribución de los valores de predicción (en la regresión).

## <a name="unfairness-mitigation"></a>Mitigación de la parcialidad

### <a name="parity-constraints"></a>Restricciones de paridad

Fairlearn incluye diversos algoritmos de mitigación de parcialidad. Estos algoritmos admiten un conjunto de restricciones en el comportamiento del predictor denominadas **restricciones de paridad** o criterios. Las restricciones de paridad requieren que algunos aspectos del comportamiento de predicción sean comparables en los grupos que definen las características confidenciales (por ejemplo, razas diferentes). Los algoritmos de mitigación de Fairlearn usan estas restricciones de paridad para mitigar los problemas de imparcialidad observados.

Fairlearn admite los siguientes tipos de restricciones de paridad:

|Restricción de paridad  | Propósito  |Tarea de Machine Learning  |
|---------|---------|---------|
|Paridad demográfica     |  Mitigar los daños en la asignación | Clasificación binaria, regresión |
|Probabilidades igualadas  | Diagnosticar los daños en la asignación y la calidad del servicio | Clasificación binaria        |
|Pérdida de grupos limitada     |  Mitigar los daños en la calidad del servicio | Regresión |

### <a name="mitigation-algorithms"></a>Algoritmos de mitigación

Fairlearn proporciona algoritmos de mitigación de parcialidad de posprocesamiento y reducción:

- Reducción: estos algoritmos toman un estimador ML estándar de caja negra (por ejemplo, un modelo LightGBM) y generan un conjunto de modelos que se vuelven a entrenar mediante una secuencia de conjuntos de datos de entrenamiento que se han vuelto a ponderar. Por ejemplo, los solicitantes de un sexo determinado podrían estar ponderados en mayor o menor medida para volver a entrenar los modelos y reducir así las disparidades entre los diferentes sexos. A continuación, los usuarios pueden elegir un modelo que les ofrezca el mejor equilibrio entre la precisión (u otra métrica de rendimiento) y la disparidad, lo que generalmente debería basarse en reglas de negocios y cálculos de costos.  
- Posprocesamiento: estos algoritmos usan un clasificador existente y la característica confidencial como entrada. A continuación, derivan una transformación de la predicción del clasificador para aplicar las restricciones de imparcialidad especificadas. La mayor ventaja de la optimización del umbral es su simplicidad y flexibilidad, ya que no es necesario volver a entrenar el modelo. 

| Algoritmo | Descripción | Tarea de Machine Learning | Características confidenciales | Restricciones de paridad admitidas | Tipo de algoritmo |
| --- | --- | --- | --- | --- | --- |
| `ExponentiatedGradient` | Enfoque de la caja negra para la clasificación imparcial descrita en [Enfoque de reducción para la clasificación imparcial](https://arxiv.org/abs/1803.02453) | Clasificación binaria | Categorías | [Paridad demográfica](#parity-constraints), [Probabilidades igualadas](#parity-constraints) | Reducción |
| `GridSearch` | Enfoque de la caja descrita en [Enfoque de reducción para la clasificación imparcial](https://arxiv.org/abs/1803.02453)| Clasificación binaria | Binary | [Paridad demográfica](#parity-constraints), [Probabilidades igualadas](#parity-constraints) | Reducción |
| `GridSearch` | Enfoque de la caja negra que implementa una variante de la búsqueda de cuadrícula de la regresión imparcial con el algoritmo, para la pérdida de grupos limitada descrita en [Regresión imparcial: Definiciones cuantitativas y algoritmos basados en la reducción](https://arxiv.org/abs/1905.12843) | Regresión | Binary | [Pérdida de grupos limitada](#parity-constraints) | Reducción |
| `ThresholdOptimizer` | Algoritmo de posprocesamiento basado en el artículo [La igualdad de oportunidades en el aprendizaje supervisado](https://arxiv.org/abs/1610.02413). Esta técnica toma como entrada un clasificador existente y la característica confidencial, y deriva una transformación monótona de la predicción del clasificador para aplicar las restricciones de paridad especificadas. | Clasificación binaria | Categorías | [Paridad demográfica](#parity-constraints), [Probabilidades igualadas](#parity-constraints) | Posprocesamiento |

## <a name="next-steps"></a>Pasos siguientes

- Para obtener información sobre cómo usar los distintos componentes, consulte el [repositorio GitHub de Fairlearn](https://github.com/fairlearn/fairlearn/) y los [cuadernos de ejemplo](https://github.com/fairlearn/fairlearn/tree/master/notebooks).
- Obtenga información acerca de la conservación de la privacidad de datos mediante la [privacidad diferencial y el paquete WhisperNoise](concept-differential-privacy.md).
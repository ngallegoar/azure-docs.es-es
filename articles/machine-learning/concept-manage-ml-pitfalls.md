---
title: Evitar el sobreajuste y los datos desequilibrados con AutoML
titleSuffix: Azure Machine Learning
description: Identifique y administre los problemas comunes de los modelos de ML con las soluciones de aprendizaje automático automatizado de Azure Machine Learning.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: nibaccam
author: nibaccam
ms.author: nibaccam
ms.date: 04/09/2020
ms.openlocfilehash: 76f920ad6aae68defb567a7a6623d1ffd488af5f
ms.sourcegitcommit: 2d7910337e66bbf4bd8ad47390c625f13551510b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80874864"
---
# <a name="prevent-overfitting-and-imbalanced-data-with-automated-machine-learning"></a>Evitar el sobreajuste y los datos desequilibrados con el aprendizaje automático automatizado

El sobreajuste y los datos desequilibrados son un problema común al crear modelos de Machine Learning. De forma predeterminada, el aprendizaje automático automatizado de Azure Machine Learning proporciona gráficos y métricas para ayudarle a identificar estos riesgos, e implementa prácticas recomendadas para ayudarle a mitigarlos. 

## <a name="identify-over-fitting"></a>Identificación del sobreajuste

El sobreajuste en el aprendizaje automático se produce cuando un modelo se ajusta demasiado bien a los datos de entrenamiento y, como resultado, no puede predecir con precisión sobre datos de prueba no vistos. En otras palabras, el modelo ha memorizado simplemente patrones y ruido específicos de los datos de entrenamiento, pero no es lo suficientemente flexible como para realizar predicciones sobre datos reales.

Examine los siguientes modelos entrenados y sus correspondientes precisiones de entrenamiento y prueba.

| Modelo | Precisión del entrenamiento | Precisión de la prueba |
|-------|----------------|---------------|
| Un | 99,9 % | 95 % |
| B | 87 % | 87 % |
| C | 99,9 % | 45 % |

Si observa el modelo **A**, hay una idea equivocada habitual de que si la precisión de la prueba en los datos no vistos es inferior a la precisión del entrenamiento, el modelo está sobreajustado. Sin embargo, la precisión de la prueba siempre debe ser menor que la precisión del entrenamiento y la distinción entre el sobreajuste y el ajuste adecuado se convierte en *cuánto* menos preciso. 

Al comparar los modelos **A** y **B**, el modelo **A** es un modelo mejor porque tiene mayor precisión en la prueba y, aunque la precisión de la prueba es ligeramente inferior al 95 %, no es una diferencia significativa que sugiera que existe sobreajuste. No elegiría el modelo **B** simplemente porque las precisiones del entrenamiento y de la prueba están más próximas.

El modelo **C** representa un caso claro de sobreajuste; la precisión del entrenamiento es muy alta, pero la precisión de la prueba no es tan alta. Esta distinción es subjetiva, pero proviene del conocimiento del problema y los datos y de las magnitudes de error que son aceptables.

## <a name="prevent-over-fitting"></a>Prevención del sobreajuste

En los casos más notorios, un modelo sobreajustado supondrá que las combinaciones de valores de características que se han visto durante el entrenamiento siempre darán exactamente la misma salida para el destino.

La mejor manera de evitar el sobreajuste es seguir los procedimientos recomendados de ML, entre los que se incluyen:

* Usar más datos de entrenamiento y eliminar el sesgo estadístico
* Evitar pérdidas de destino
* Usar menos características
* **Regularización y optimización de hiperparámetros**
* **Limitaciones de la complejidad del modelo**
* **Validación cruzada**

En el contexto del aprendizaje automático automatizado, los tres primeros elementos anteriores son **procedimientos recomendados que usted implementa**. Los tres últimos elementos en negrita son **procedimientos recomendados que el aprendizaje automático automatizado implementa** de forma predeterminada para protegerse frente al sobreajuste. En configuraciones distintas al aprendizaje automático automatizado, es conveniente seguir los seis procedimientos recomendados para evitar modelos con sobreajuste.

### <a name="best-practices-you-implement"></a>Procedimientos recomendados que usted implementa

El uso de **más datos** es la forma más sencilla y óptima de evitar el sobreajuste y, como ventaja adicional, suele aumentar la precisión. Cuando se usan más datos, resulta más difícil para el modelo memorizar patrones exactos y se ve obligado a obtener soluciones que son más flexibles para acomodar más condiciones. También es importante reconocer el **sesgo estadístico**, para asegurarse de que los datos de entrenamiento no incluyen patrones aislados que no existen en los datos de predicción en directo. Este escenario puede ser difícil de resolver, ya que es posible que no haya sobreajuste entre los conjuntos de entrenamiento y pruebas, pero puede haber sobreajuste cuando se comparan con los datos de pruebas en directo.

La pérdida de destino es un problema similar, en el que es posible que no vea sobreajuste entre los conjuntos de entrenamiento y pruebas, sino que aparece en tiempo de predicción. La pérdida de destino se produce cuando el modelo "hace trampa" durante el entrenamiento al tener acceso a datos que normalmente no debería tener en el momento de la predicción. Por ejemplo, si el problema es predecir el lunes cuál será el precio el viernes, pero una de sus características incluye accidentalmente datos del jueves, que serían datos que el modelo no tendrá en el tiempo de predicción, ya que no puede ver el futuro. La pérdida de destino es un error sencillo de evitar, pero a menudo se caracteriza por una precisión anormalmente alta para el problema. Si intenta predecir el precio de las acciones y ha entrenado un modelo con una precisión del 95 %, es probable que se produzcan pérdidas de destino en alguna parte de las características.

La eliminación de características también puede ayudar con el sobreajuste, ya que impide que el modelo tenga demasiados campos para memorizar patrones específicos, lo que hace que sea más flexible. Puede ser difícil de medir cuantitativamente, pero si puede eliminar características y conservar la misma precisión, probablemente haya hecho que el modelo sea más flexible y haya reducido el riesgo de sobreajuste.

### <a name="best-practices-automated-ml-implements"></a>Procedimientos recomendados que implementa el aprendizaje automático automatizado

La regularización es el proceso de minimizar una función de costo para penalizar modelos complejos y sobreajustados. Hay diferentes tipos de funciones de regularización, pero en general todas penalizan el tamaño del coeficiente del modelo, la varianza y la complejidad. El aprendizaje automático automatizado usa L1 (Lasso), L2 (Ridge) y ElasticNet (L1 y L2 simultáneamente) en combinaciones diferentes con diferentes configuraciones de hiperparámetros del modelo que controlan el sobreajuste. En términos simples, el aprendizaje automático automatizado variará cuánto se regula un modelo y elegirá el mejor resultado.

El aprendizaje automático automatizado también implementa limitaciones de complejidad del modelo explícitas para evitar el sobreajuste. En la mayoría de los casos, esta implementación se aplica específicamente a árboles de decisión o algoritmos de bosque, donde la profundidad máxima del árbol individual está limitada y el número total de árboles usados en el bosque o las técnicas conjunto están limitados.

La validación cruzada (CV) es el proceso de tomar muchos subconjuntos de los datos de entrenamiento completos y entrenar un modelo en cada subconjunto. La idea es que un modelo podría tener "suerte" y tener una gran precisión con un subconjunto, pero al usar muchos subconjuntos el modelo no alcanzará esta alta precisión cada vez. Al realizar la CV, se proporciona un conjunto de datos de exclusión de la validación, se especifican los pliegues de la CV (número de subconjuntos) y el aprendizaje automático automatizado entrena el modelo y ajusta los hiperparámetros para minimizar el error en el conjunto de validación. Un plegamiento de la CV podría estar sobreajustado, pero si se usan muchos de ellos, se reduce la probabilidad de que el modelo final esté sobreajustado. La contrapartida es que la CV genera tiempos de entrenamiento más largos y, por lo tanto, un costo mayor, porque en lugar de entrenar un modelo una vez, se entrena una vez por cada *n* subconjuntos de CV. 

> [!NOTE]
> La validación cruzada no está habilitada de forma predeterminada; se debe configurar en la configuración de aprendizaje automático automatizado. Sin embargo, después de configurar la validación cruzada y proporcionar un conjunto de datos de validación, el proceso es automático. Vea 

<a name="imbalance"></a>

## <a name="identify-models-with-imbalanced-data"></a>Identificación de modelos con datos desequilibrados

Los datos desequilibrados suelen encontrarse en los datos de los escenarios de clasificación de aprendizaje automático y hacen referencia a los datos que contienen una relación desproporcionada de observaciones en cada clase. Este desequilibrio puede dar lugar a un efecto positivo percibido de manera falsa de la precisión de un modelo, ya que los datos de entrada tienen un sesgo hacia una clase, lo que da lugar a que el modelo entrenado imite ese sesgo. 

Como la precisión de los algoritmos de clasificación se evalúan habitualmente, comprobar la puntuación de precisión de un modelo es una buena manera de identificar si los datos desequilibrados lo afectaron. ¿Tiene una precisión realmente alta o realmente baja para ciertas clases?

Además, las ejecuciones de ML automatizado generan automáticamente los gráficos siguientes, que pueden ayudarlo a entender la corrección de las clasificaciones del modelo e identificar los modelos potencialmente afectados por los datos desequilibrados.

Gráfico| Descripción
---|---
[Matriz de confusión](how-to-understand-automated-ml.md#confusion-matrix)| Evalúa las etiquetas clasificadas correctamente con respecto a las etiquetas reales de los datos. 
[Precisión-recuperación](how-to-understand-automated-ml.md#precision-recall-chart)| Evalúa la relación de las etiquetas correctas con respecto a la relación de las instancias de etiquetas encontradas de los datos. 
[Curvas ROC](how-to-understand-automated-ml.md#roc)| Evalúa la relación de etiquetas correctas con respecto a la relación de etiquetas de falsos positivos.

## <a name="handle-imbalanced-data"></a>Manejo de datos desequilibrados 

Como parte de su objetivo de simplificar el flujo de trabajo del aprendizaje automático, el ML automatizado tiene funcionalidades integradas que ayudan a tratar con datos desequilibrados como, por ejemplo: 

- Una **columna de peso**: el ML automatizado admite una columna de peso como entrada, lo que provoca que las filas de los datos se puedan subir o bajar, lo que hace que una clase sea más o menos "importante".

- Los algoritmos que usa el ML automatizado pueden controlar correctamente el desequilibrio de hasta 20:1, lo que significa que la clase más común puede tener 20 veces más filas en los datos que la clase menos común.

Las técnicas siguientes son otras opciones para controlar los datos desequilibrados fuera del ML automatizado. 

- Vuelva a muestrear para equilibrar el desequilibrio de clases, ya sea mediante el muestreo ascendente de las clases más pequeñas o el muestreo descendente de las clases más grandes. Estos métodos requieren conocimientos para procesar y analizar.

- Use una métrica de rendimiento que se ocupe mejor de los datos desequilibrados. Por ejemplo, la puntuación F1 es un promedio ponderado de precisión y recuperación. La precisión mide la exactitud de un clasificador (la precisión baja indica un gran número de falsos positivos), mientras que la recuperación mide la integridad de un clasificador (la recuperación baja indica un número elevado de falsos negativos). 

## <a name="next-steps"></a>Pasos siguientes

Vea ejemplos y aprenda cómo generar modelos mediante aprendizaje automático automatizado:

+ Lea el [Tutorial: entrenamiento automático de un modelo de regresión con Azure Machine Learning](tutorial-auto-train-models.md).

+ Configure el experimento de entrenamiento automático:
  + En Azure Machine Learning Studio, [use estos pasos](how-to-use-automated-ml-for-ml-models.md).
  + Con el SDK de Python, [siga estos pasos](how-to-configure-auto-train.md).



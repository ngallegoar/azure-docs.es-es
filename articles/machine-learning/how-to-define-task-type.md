---
title: Definición de una tarea de aprendizaje automático para una ejecución de aprendizaje automático automatizado
titleSuffix: Azure Machine Learning
description: Obtenga información sobre cómo definir una tarea de aprendizaje automático para una ejecución de aprendizaje automático automatizado.
services: machine-learning
author: RachelKellam
ms.author: rakellam
ms.reviewer: nibaccam
ms.service: machine-learning
ms.subservice: core
ms.topic: how-to
ms.date: 11/04/2019
ms.openlocfilehash: fef3f0347d963b8a7d5fe2f1ca771293e9c3d2a9
ms.sourcegitcommit: efefce53f1b75e5d90e27d3fd3719e146983a780
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/01/2020
ms.locfileid: "80475504"
---
# <a name="how-to-define-a-machine-learning-task"></a>Definición de una tarea de aprendizaje automático 
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

En este artículo aprenderá las tareas de aprendizaje automático compatibles y cómo definirlas para la ejecución de un experimento de aprendizaje automático automatizado (ML automatizado).


## <a name="what-is-a-machine-learning-task"></a><a name="What is a machine learning task?"></a> ¿Qué es una tarea de aprendizaje automático?

Una tarea de aprendizaje automático representa el tipo de problema que se resuelve con la creación de un modelo predictivo. El aprendizaje automático automatizado admite tres tipos diferentes de tareas, entre las que se incluyen la clasificación, la regresión y la previsión de serie temporal.

Tipo de tarea| Descripción| Ejemplo
----|----|----
clasificación | Tarea para predecir la categoría de una fila determinada en un conjunto de datos. | Detección de fraudes en una tarjeta de crédito. La columna de destino sería **Fraude detectado** con categorías de *True* o *False*. En este caso, estamos clasificando cada fila de los datos como True o False.
Regresión | Tarea para predecir una salida de cantidad continua. | Costo del automóvil en función de sus características, la columna de destino sería **Precio**.
Previsión |Tarea para crear estimaciones informadas para determinar la dirección de las tendencias futuras.| Previsión de la demanda energética de las próximas 48 horas. La columna de destino sería **Demanda** y los valores de predicción se utilizarían para mostrar patrones en la demanda energética.

El ML automatizado admite los siguientes algoritmos durante el proceso de optimización y automatización. Como usuario, no hay ninguna necesidad de especificar el algoritmo.

clasificación | Regresión | Previsión de series temporales
-- |-- |--
[Regresión logística](https://scikit-learn.org/stable/modules/linear_model.html#logistic-regression)| [Red elástica](https://scikit-learn.org/stable/modules/linear_model.html#elastic-net)| [Red elástica](https://scikit-learn.org/stable/modules/linear_model.html#elastic-net)
[Light GBM](https://lightgbm.readthedocs.io/en/latest/index.html)|[Light GBM](https://lightgbm.readthedocs.io/en/latest/index.html)|[Light GBM](https://lightgbm.readthedocs.io/en/latest/index.html)
[Potenciación del gradiente](https://scikit-learn.org/stable/modules/ensemble.html#classification)|[Potenciación del gradiente](https://scikit-learn.org/stable/modules/ensemble.html#regression)|[Potenciación del gradiente](https://scikit-learn.org/stable/modules/ensemble.html#regression)
[Árbol de decisión](https://scikit-learn.org/stable/modules/tree.html#decision-trees)|[Árbol de decisión](https://scikit-learn.org/stable/modules/tree.html#regression)|[Árbol de decisión](https://scikit-learn.org/stable/modules/tree.html#regression)
[K Vecinos más próximos](https://scikit-learn.org/stable/modules/neighbors.html#nearest-neighbors-regression)|[K Vecinos más próximos](https://scikit-learn.org/stable/modules/neighbors.html#nearest-neighbors-regression)|[K Vecinos más próximos](https://scikit-learn.org/stable/modules/neighbors.html#nearest-neighbors-regression)
[SVC lineal](https://scikit-learn.org/stable/modules/svm.html#classification)|[Lazo LARS](https://scikit-learn.org/stable/modules/linear_model.html#lars-lasso)|[Lazo LARS](https://scikit-learn.org/stable/modules/linear_model.html#lars-lasso)
[Clasificación de vectores de soporte de C (SVC)](https://scikit-learn.org/stable/modules/svm.html#classification)|[Descenso de gradiente estocástico (SGD)](https://scikit-learn.org/stable/modules/sgd.html#regression)|[Descenso de gradiente estocástico (SGD)](https://scikit-learn.org/stable/modules/sgd.html#regression)
[Bosque aleatorio](https://scikit-learn.org/stable/modules/ensemble.html#random-forests)|[Bosque aleatorio](https://scikit-learn.org/stable/modules/ensemble.html#random-forests)|[Bosque aleatorio](https://scikit-learn.org/stable/modules/ensemble.html#random-forests)
[Árboles extremadamente aleatorios](https://scikit-learn.org/stable/modules/ensemble.html#extremely-randomized-trees)|[Árboles extremadamente aleatorios](https://scikit-learn.org/stable/modules/ensemble.html#extremely-randomized-trees)|[Árboles extremadamente aleatorios](https://scikit-learn.org/stable/modules/ensemble.html#extremely-randomized-trees)
[Xgboost](https://xgboost.readthedocs.io/en/latest/parameter.html)|[Xgboost](https://xgboost.readthedocs.io/en/latest/parameter.html)| [Xgboost](https://xgboost.readthedocs.io/en/latest/parameter.html)
[Clasificador DNN](https://www.tensorflow.org/api_docs/python/tf/estimator/DNNClassifier)|[Regresor DNN](https://www.tensorflow.org/api_docs/python/tf/estimator/DNNRegressor) | [Regresor DNN](https://www.tensorflow.org/api_docs/python/tf/estimator/DNNRegressor)|
[Clasificador lineal DNN](https://www.tensorflow.org/api_docs/python/tf/estimator/LinearClassifier)|[Regresión lineal](https://www.tensorflow.org/api_docs/python/tf/estimator/LinearRegressor)|[Regresión lineal](https://www.tensorflow.org/api_docs/python/tf/estimator/LinearRegressor)
[Bayes naive](https://scikit-learn.org/stable/modules/naive_bayes.html#bernoulli-naive-bayes)||
[Descenso de gradiente estocástico (SGD)](https://scikit-learn.org/stable/modules/sgd.html#sgd)||


### <a name="set-the-task-type"></a>Establecimiento del tipo de tarea
Puede establecer el tipo de tarea para los experimentos de aprendizaje automático automatizado con el SDK o Azure Machine Learning Studio.

Use el parámetro `task` en el constructor `AutoMLConfig` para especificar el tipo de experimento.

```python
from azureml.train.automl import AutoMLConfig

# task can be one of classification, regression, forecasting
automl_config = AutoMLConfig(task="classification")
```

Puede establecer la tarea como parte de la creación de la ejecución del experimento de ML automatizado en Azure Machine Learning Studio. 

[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-enterprise-sku-inline.md)]

![Selección del tipo de tarea](./media/how-to-define-task-type/task-type.png)


## <a name="next-steps"></a>Pasos siguientes

+ Obtenga más información sobre el [aprendizaje automático automatizado](concept-automated-ml.md) en Azure Machine Learning.
+ Más información sobre el [entrenamiento automático de un modelo de previsión de serie temporal](how-to-auto-train-forecast.md) en Azure Machine Learning.
+ Pruebe el tutorial para la [clasificación del aprendizaje automático automatizado](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/classification-credit-card-fraud/auto-ml-classification-credit-card-fraud.ipynb).
+ Pruebe el cuaderno de ejemplo sobre la [regresión del aprendizaje automático automatizado](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/regression-hardware-performance-explanation-and-featurization/auto-ml-regression-hardware-performance-explanation-and-featurization.ipynb).


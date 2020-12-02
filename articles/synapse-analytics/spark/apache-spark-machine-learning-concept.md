---
title: Machine Learning con Apache Spark
description: En este artículo se proporciona información general conceptual sobre las funcionalidades de la ciencia de datos y el aprendizaje automático disponibles a través de Apache Spark en Azure Synapse Analytics.
services: synapse-analytics
author: midesa
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: machine-learning
ms.date: 11/13/2020
ms.author: midesa
ms.reviewer: jrasnick
ms.openlocfilehash: ced78955c71f37a5a6f5231f61e5327428834f00
ms.sourcegitcommit: a43a59e44c14d349d597c3d2fd2bc779989c71d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/25/2020
ms.locfileid: "95919101"
---
# <a name="machine-learning-with-apache-spark"></a>Machine Learning con Apache Spark

Apache Spark en Azure Synapse Analytics permite el aprendizaje automático con macrodatos, lo que proporciona la capacidad de obtener información valiosa de grandes cantidades de datos estructurados y no estructurados, y en rápido movimiento. 

En esta sección se incluye información general y tutoriales para flujos de trabajo de aprendizaje automático, incluido el análisis de datos exploratorio, la ingeniería de características, el entrenamiento de modelos, la puntuación de modelos y la implementación.  

## <a name="synapse-runtime"></a>Tiempo de ejecución de Synapse 
El tiempo de ejecución de Synapse es un entorno mantenido y optimizado para la ciencia de datos y el aprendizaje automático. El tiempo de ejecución de Synapse ofrece una variedad de bibliotecas de código abierto conocidas y compilaciones en el SDK de Azure Machine Learning de forma predeterminada. Asimismo, el tiempo de ejecución de Synapse también incluye gran variedad bibliotecas externas, como PyTorch, Scikit-Learn, XGBoost, etc.

Para obtener más información sobre las bibliotecas disponibles y sus versiones relacionadas, consulte el artículo publicado [Tiempo de ejecución de Azure Synapse Analytics](../spark/apache-spark-version-support.md).

## <a name="exploratory-data-analysis"></a>Análisis de datos exploratorios
Al usar Apache Spark en Azure Synapse Analytics, existen varias opciones integradas que le permitirán visualizar los datos, incluidas las opciones de gráfico del cuaderno de Synapse, el acceso a bibliotecas populares de código abierto como Seaborn y Matplotlib, y la integración con Synapse SQL y Power BI.

Para obtener más información sobre las opciones de visualización de datos y análisis de datos, consulte el artículo sobre cómo [visualizar datos mediante Azure Synapse notebooks](../spark/apache-spark-data-visualization.md).

## <a name="feature-engineering"></a>Ingeniería de características
De forma predeterminada, el tiempo de ejecución de Synapse incluye un conjunto de bibliotecas que se suelen usar para el diseño de características. Si trabaja con conjuntos de datos grandes, puede usar Spark SQL, MLlib y Koalas para la ingeniería de características. En el caso de conjuntos de datos más pequeños, las bibliotecas de terceros como NumPy, Pandas y Scikit-learn también proporcionan métodos útiles en estos escenarios.

## <a name="train-models"></a>Entrenamiento de modelos
Existen varias opciones al entrenar modelos de aprendizaje automático con Azure Spark en Azure Synapse Analytics: Apache Spark MLlib, Azure Machine Learning y otras bibliotecas de código abierto. 

Para obtener más información sobre las funcionalidades de aprendizaje automático, consulte el artículo sobre cómo [entrenar modelos en Azure Synapse Analytics](../spark/apache-spark-machine-learning-training.md).

### <a name="sparkml-and-mllib"></a>SparkML y MLlib
Las capacidades de cálculo distribuido en memoria de Spark lo convierten en una buena opción para algoritmos iterativos en los cálculos de gráficos y aprendizaje automático. ```spark.ml``` proporciona un conjunto uniforme de API de alto nivel que ayudan a los usuarios a crear y optimizar canalizaciones de aprendizaje automático. Para obtener más información acerca de ```spark.ml```, puede consultar la [guía de programación de Apache Spark ML](https://spark.apache.org/docs/1.2.2/ml-guide.html).

### <a name="azure-machine-learning-automl"></a>AutoML de Azure Machine Learning
[AutoML de Azure Machine Learning](https://docs.microsoft.com/azure/machine-learning/concept-automated-ml) (aprendizaje automático automatizado) le permite automatizar el proceso de desarrollo de modelos de Machine Learning. Permite que los desarrolladores, analistas y científicos de datos creen modelos de aprendizaje automático con un escalado, eficiencia y productividad altos, al mismo tiempo que mantiene la calidad del modelo. Los componentes para ejecutar el SDK de AutoML de Azure Machine Learning se compilan directamente en el tiempo de ejecución de Synapse.

### <a name="open-source-libraries"></a>Bibliotecas de código abierto
Cada grupo de Apache Spark de Azure Synapse Analytics incluye un conjunto de bibliotecas de aprendizaje automático conocidas y precargadas.  Algunas de las bibliotecas de aprendizaje automático relevantes que se incluyen de forma predeterminada son las siguientes:

- [Scikit-learn](https://scikit-learn.org/stable/index.html) es una de las bibliotecas de aprendizaje automático de nodo único más popular para algoritmos de ML clásicos. Scikit-learn es compatible con la mayoría de los algoritmos de aprendizaje supervisados y no supervisados; asimismo, también se puede usar para la minería y el análisis de datos.
  
- [XGBoost](https://xgboost.readthedocs.io/en/latest/) es una conocida biblioteca de aprendizaje automático que contiene algoritmos optimizados para el entrenamiento de árboles de decisión y bosques aleatorios. 
  
- [PyTorch](https://pytorch.org/) & [Tensorflow](https://www.tensorflow.org/) son eficaces bibliotecas de aprendizaje profundo de Python. Puede usar estas bibliotecas en un grupo de Apache Spark de Azure Synapse Analytics, para compilar modelos de máquina única estableciendo el número de ejecutores del grupo en cero. Aunque Apache Spark no funciona con esta configuración, es una manera sencilla y rentable de crear modelos de máquina única.

## <a name="track-model-development"></a>Seguimiento del desarrollo de modelos
[MLflow](https://www.mlflow.org/) es una biblioteca de código abierto para administrar el ciclo de vida de los experimentos de aprendizaje automático. MLFlow Tracking es un componente de MLflow que registra las métricas de ejecución de entrenamiento y los artefactos de modelo y realiza un seguimiento de ellos. Para obtener más información sobre cómo puede usar el seguimiento de MLFlow a través de Azure Synapse Analytics y Azure Machine Learning, consulte este tutorial sobre [cómo usar MLFlow](https://docs.microsoft.com/azure/machine-learning/how-to-use-mlflow).

## <a name="model-scoring"></a>Puntuación del modelo
La puntuación del modelo o la inferencia, es la fase en la que un modelo se usa para hacer predicciones. Para usar la puntuación del modelo con SparkML o MLLib, puede aprovechar los métodos de Spark nativos para realizar la inferencia directamente en una instancia de DataFrame de Spark. En cuanto a otras bibliotecas y tipos de modelo, puede crear una función definida por el usuario de Spark para escalar horizontalmente la inferencia en grandes conjuntos de datos. En el caso de conjuntos de datos más pequeños, también puede usar los métodos de inferencia de modelos nativos que proporciona la biblioteca.

## <a name="register-and-serve-models"></a>Registrar y servir modelos
Al registrar un modelo, podrá almacenar, editar y realizar un seguimiento de los metadatos correspondientes a los modelos de su área de trabajo. Una vez que haya terminado de entrenar el modelo, puede registrarlo en el [registro del modelo de Azure Machine Learning](https://docs.microsoft.com/azure/machine-learning/concept-model-management-and-deployment#register-package-and-deploy-models-from-anywhere). Una vez registrados, los modelos de ONNX también se pueden usar para [enriquecer los datos](../machine-learning/tutorial-sql-pool-model-scoring-wizard.md) almacenados en grupos de SQL dedicados.

## <a name="next-steps"></a>Pasos siguientes
Para empezar a trabajar con el aprendizaje automático en Azure Synapse Analytics, asegúrese de consultar los siguientes tutoriales:
- [Análisis de datos con cuadernos de Azure Synapse](../spark/apache-spark-data-visualization-tutorial.md)

- [Entrenar un modelo de Machine Learning con AutoML](../spark/apache-spark-azure-machine-learning-tutorial.md)

- [Entrenar un modelo de Machine Learning con Apache Spark MLlib](../spark/apache-spark-machine-learning-mllib-notebook.md)
  
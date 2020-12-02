---
title: Entrenamiento de modelos de Machine Learning con Apache Spark
description: Uso de Apache Spark en Azure Synapse Analytics para entrenar modelos de Machine Learning
author: midesa
ms.author: midesa
ms.reviewer: euang
services: synapse-analytics
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: machine-learning
ms.date: 09/13/2020
ms.openlocfilehash: 8250f7154cfb2739d065980284da67a200680d89
ms.sourcegitcommit: a43a59e44c14d349d597c3d2fd2bc779989c71d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/25/2020
ms.locfileid: "95919243"
---
# <a name="train-machine-learning-models"></a>Entrenamiento de modelos de Machine Learning
Apache Spark en Azure Synapse Analytics permite el aprendizaje automático con macrodatos, lo que proporciona la capacidad de obtener información valiosa de grandes cantidades de datos estructurados, no estructurados y en rápido movimiento. Existen varias opciones cuando se entrenan modelos de Machine Learning mediante Azure Spark en Azure Synapse Analytics: Apache Spark MLlib, Azure Machine Learning y otras bibliotecas de código abierto. 

## <a name="apache-sparkml-and-mllib"></a>Apache SparkML y MLlib
Apache Spark en Azure Synapse Analytics es una de las implementaciones de Microsoft de Apache Spark en la nube. Proporciona un marco de procesamiento datos paralelos de código abierto y unificado que admite el procesamiento en memoria para agilizar el análisis de macrodatos. El motor de procesamiento Spark se ha creado para ofrecer velocidad, facilidad de uso y análisis sofisticados. Las capacidades de cálculo distribuido en memoria de Spark lo convierten en una buena opción para algoritmos iterativos en los cálculos de gráficos y aprendizaje automático. 

Hay dos bibliotecas escalables de aprendizaje automático que ofrecen las funcionalidades del modelado algorítmico a este entorno distribuido: MLlib y SparkML. MLlib contiene la API original que se basa en RDD. SparkML es un paquete más reciente que proporciona una API de nivel más alto que se basa en DataFrames para construir canalizaciones ML. SparkML aún no admite todas las características de MLlib, pero está reemplazándola como biblioteca de aprendizaje automático estándar de Spark.

> [!NOTE]
> 
> Para más información sobre la creación de modelos de SparkML, consulte este [tutorial](../spark/apache-spark-azure-machine-learning-tutorial.md).

## <a name="popular-libraries"></a>Bibliotecas populares
Cada grupo de Apache Spark de Azure Synapse Analytics incluye un conjunto de bibliotecas de aprendizaje automático conocidas y precargadas. Estas bibliotecas proporcionan código reutilizable que se puede incluir en los programas o proyectos. Estas son algunas de las bibliotecas de aprendizaje automático pertinentes que se incluyen de forma predeterminada:
- [Scikit-learn](https://scikit-learn.org/stable/index.html) es una de las bibliotecas de aprendizaje automático de nodo único más populares para algoritmos de Machine Learning clásicos. Scikit-learn admite la mayoría de los algoritmos de aprendizaje supervisados y no supervisados y también se puede usar para la minería y el análisis de datos.
  
- [XGBoost](https://xgboost.readthedocs.io/en/latest/) es una conocida biblioteca de aprendizaje automático que contiene algoritmos optimizados para el entrenamiento de árboles de decisión y bosques aleatorios. 
  
- [PyTorch](https://pytorch.org/) & [Tensorflow](https://www.tensorflow.org/) son eficaces bibliotecas de aprendizaje profundo de Python. En los grupos de Apache Spark de Azure Synapse Analytics, estas bibliotecas se pueden usar para compilar modelos de máquina única estableciendo el número de ejecutores del grupo en cero. Aunque Apache Spark no funciona con esta configuración, es una manera sencilla y barata de crear modelos de máquina única.

Para más información sobre las bibliotecas disponibles y sus versiones relacionadas, consulte el artículo publicado acerca del [runtime de Azure Synapse Analytics](../spark/apache-spark-version-support.md).

## <a name="mmlspark"></a>MMLSpark
La biblioteca Microsoft Machine Learning para Apache Spark es [MMLSpark](https://github.com/Azure/mmlspark). Esta biblioteca se ha diseñado para que los científicos de datos sean más productivos en Spark, aumenten la velocidad de experimentación y aprovechen las técnicas de aprendizaje automático de vanguardia,como el aprendizaje profundo, en conjuntos de datos grandes. 

MMLSpark proporciona una capa sobre las API de bajo nivel de SparkML cuando se crean modelos de aprendizaje automático escalables, como las cadenas de indexación, la conversión de datos en un diseño esperado por los algoritmos de aprendizaje automático y el ensamblado de vectores de características. La biblioteca MMLSpark simplifica estas y otras tareas comunes para la creación de modelos en PySpark.

## <a name="automated-ml-in-azure-machine-learning"></a>Aprendizaje automático automatizado en Azure Machine Learning 
Azure Machine Learning es un entorno en la nube que permite entrenar, implementar, automatizar, administrar y realizar un seguimiento de los modelos de aprendizaje automático. ML automatizado (AutoML) en Azure Machine Learning acepta tanto valores de configuración como datos de entrenamiento y, después, recorre automáticamente combinaciones de diferentes métodos de estandarización o normalización de características, modelos y valores de hiperparámetros hasta obtener el mejor modelo. 

Si se usa ML automatizado en Azure Synapse Analytics, se puede aprovechar la integración profunda entre los distintos servicios para simplificar la autenticación y el entrenamiento de modelos. 

> [!NOTE]
> 
> Para más información sobre cómo crear experimentos de ML automatizado en Azure Machine Learning, consulte este [tutorial](./spark/../apache-spark-azure-machine-learning-tutorial.md).

## <a name="azure-cognitive-services"></a>Azure Cognitive Services
[Azure Cognitive Services](https://docs.microsoft.com/azure/cognitive-services/what-are-cognitive-services) proporciona funciones de aprendizaje automático para solucionar problemas generales, como el análisis de texto para la opinión emocional o el análisis de imágenes para reconocer objetos o caras. No es necesario tener conocimientos de aprendizaje automático ni ciencia de datos para usar estos servicios. Un servicio de Cognitive Service proporciona una parte o todos los componentes de una solución de aprendizaje automático: datos, algoritmos y modelos entrenados. Estos servicios están diseñados para requerir conocimientos generales sobre los datos sin necesidad de tener experiencia con el aprendizaje automático o la ciencia de datos. Estos servicios de Cognitive Services se pueden entrenar previamente en Azure Synapse Analytics.

## <a name="next-steps"></a>Pasos siguientes
En este artículo se proporciona información general sobre las distintas opciones para entrenar modelos de Machine Learning en los grupos de Apache Spark de Azure Synapse Analytics. Para más información sobre el entrenamiento de modelos, siga este tutorial:

- Ejecute experimentos de ML automatizado mediante Azure Machine Learning y Azure Synapse Analytics: [Tutorial de ML automatizado](./spark/../apache-spark-autoscale.md) 
- Ejecute experimentos de SparkML: [Tutorial de Apache SparkML](../spark/apache-spark-azure-machine-learning-tutorial.md)
- Visualización de las bibliotecas predeterminadas: [Runtime de Azure Synapse Analytics](../spark/apache-spark-version-support.md)
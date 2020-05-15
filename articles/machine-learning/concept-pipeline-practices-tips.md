---
title: Iteración y evolución de canalizaciones de aprendizaje automático
titleSuffix: Azure Machine Learning
description: Patrones, prácticas y sugerencias para el desarrollo rápido
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: laobri
author: lobrien
ms.date: 05/01/2020
ms.openlocfilehash: 2ea353469ed111eebb591aa6ba86c652683cc2f0
ms.sourcegitcommit: c535228f0b77eb7592697556b23c4e436ec29f96
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/06/2020
ms.locfileid: "82858184"
---
# <a name="iterating-and-evolving-machine-learning-pipelines"></a>Iteración y evolución de canalizaciones de aprendizaje automático

Las canalizaciones de Azure Machine Learning proporcionan una manera eficaz de modularizar el código, reutilizar los resultados y optimizar los recursos de proceso. A continuación se muestran algunas sugerencias prácticas y procedimientos para trabajar con canalizaciones.

## <a name="how-do-you-get-started-with-pipelines"></a>¿Qué debe hacer para empezar a usar las canalizaciones?

Hay varias opciones para empezar a usar las canalizaciones si no está familiarizado con estas:

* Si su mejor manera de aprender es leer y recrear el proceso de construcción, el artículo [Creación y ejecución de canalizaciones de Machine Learning con el SDK de Azure Machine Learning](how-to-create-your-first-pipeline.md) es una buena opción. 
* Si prefiere aprender de forma interactiva en un cuaderno de Jupyter Notebook, la canalización desarrollada en el cuaderno [Azure Machine Learning Pipelines: Getting Started](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/machine-learning-pipelines/intro-to-pipelines/aml-pipelines-getting-started.ipynb) (Canalizaciones de Azure Machine Learning: Introducción) puede ser adecuada en su caso.
* Si prefiere una situación de código primero, la clonación del repositorio [Azure Machine Learning Pipelines Demo](https://github.com/microsoft/aml-pipelines-demo) (Demostración de canalizaciones de Azure Machine Learning) proporciona un buen punto de partida.

## <a name="how-do-you-modularize-pipeline-code"></a>¿Cómo se modulariza el código de la canalización? 

Los módulos y la clase `ModuleStep` ofrecen una gran oportunidad para modularizar el código de ML. Sin embargo, se debe tener en cuenta que el movimiento entre los pasos de la canalización es mucho más caro que una llamada de función. La pregunta que debe plantearse no es tanto si estas funciones y datos son conceptualmente diferentes de los de esta otra sección, si no más bien, si desea que estas funciones y estos datos evolucionen por separado, o si este cálculo es costoso i si se puede reutilizar su salida. Para obtener más información, consulte el cuaderno [How to create Module, ModuleVersion, and use them in a pipeline with ModuleStep](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/machine-learning-pipelines/intro-to-pipelines/aml-pipelines-how-to-use-modulestep.ipynb) (Cómo crear los objetos Module y ModuleVersion, y usarlos en una canalización con ModuleStep).

Como se explicó anteriormente, separar la preparación de datos del entrenamiento de datos suele ser una buena opción. A veces, la preparación de datos es compleja y tarda tanto tiempo que se puede dividir el proceso en pasos de canalización independientes. Otras opciones incluyen pruebas y análisis posteriores al entrenamiento. 

## <a name="how-do-you-speed-pipeline-iteration"></a>¿Cómo se acelera la iteración de canalizaciones? 

Algunas técnicas comunes para iterar rápidamente las canalizaciones son: 

- Clonar la canalización (realizar una copia) y volver a ejecutar rápidamente la canalización.
- Mantener la instancia de proceso en ejecución, para evitar el tiempo de inicio.
- Configurar datos y pasos para posibilitar la reutilización permitirá que la canalización no vuelva a calcular los datos no modificados.

Si desea realizar la iteración rápidamente, puede clonar la canalización para crear una canalización y volver a ejecutarla. Otra técnica útil consiste en mantener el proceso activo para no incurrir en el costo de poner en marcha el nuevo proceso. Si configura el paso para permitir la reutilización del resultado de una ejecución, la ejecución repetida volverá a usar los resultados siempre que sea posible (cuando no haya ningún cambio en los pasos).

## <a name="how-do-you-collaborate-using-ml-pipelines"></a>¿Cómo puede colaborar en el uso de canalizaciones de ML? 

Las canalizaciones independientes son líneas naturales en las que dividir el trabajo. Varios desarrolladores o incluso varios equipos pueden trabajar en distintos pasos, siempre y cuando se acuerden los datos y los argumentos que fluyen entre los pasos. 

Durante el desarrollo activo, puede recuperar los resultados de las ejecuciones de `PipelineRun` y `StepRun` del área de trabajo, usar estos objetos para descargar las salidas final e intermedia, y usar esos artefactos para su propio trabajo modularizado.

## <a name="use-pipelines-to-test-techniques-in-isolation"></a>Uso de canalizaciones para probar técnicas en aislamiento

Por lo general, las soluciones de ML reales implican una personalización considerable de cada paso. Los datos sin procesar a menudo deben prepararse de alguna manera: filtrar, transformar y aumentar. Los procesos de entrenamiento pueden tener varias arquitecturas potenciales y, para el aprendizaje profundo, muchas variaciones posibles para los tamaños de capa y las funciones de activación. Incluso con una arquitectura coherente, la búsqueda de hiperparámetros puede generar ventajas significativas.

Además de las herramientas como [AutoML](concept-automated-ml.md) y [búsqueda automatizada de hiperparámetros](how-to-tune-hyperparameters.md), las canalizaciones pueden ser una herramienta importante para las soluciones de pruebas A/B. Si tiene varias variantes de los pasos de la canalización, es fácil generar ejecuciones independientes que intenten sus variantes: 

```python
data_preparation_variants = [data1, data2, data3]
data_augmentation_variants = [aug1, aug2]
architecture_variants = [train1, train2, train3]

# Cartesian product
all_variants = np.array(np.meshgrid(data_preparation_variants,data_augmentation_variants,architecture_variants)).T.reshape(-1,3)

pipelines = [Pipeline(workspace=ws, steps=variant.tolist(), description=str(variant)) for variant in all_variants]

```


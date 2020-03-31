---
title: 'Regresión de bosque de decisión: referencia para los módulos'
titleSuffix: Azure Machine Learning
description: Obtenga información sobre cómo usar el módulo Perceptrón promedio de dos clases en Azure Machine Learning para crear un modelo de Machine Learning basado en el algoritmo de perceptrón promedio.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 02/22/2020
ms.openlocfilehash: 73e23dd7d350ea63e9fd8b933a525a9d8aad9e3e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "77920780"
---
# <a name="two-class-averaged-perceptron-module"></a>Módulo Perceptrón promedio de dos clases

En este artículo se describe un módulo del diseñador de Azure Machine Learning (versión preliminar).

Utilice este módulo para crear un modelo de Machine Learning basado en el algoritmo de perceptrón promedio.  
  
Este algoritmo de clasificación es un método de aprendizaje supervisado y requiere un *conjunto de datos etiquetado*, que incluya una columna de etiqueta. Puede entrenar el modelo proporcionando el modelo y el conjunto de datos etiquetado como entrada para [Entrenar modelo](./train-model.md). Después, el modelo entrenado puede utilizarse para predecir valores para los nuevos ejemplos de entrada.  

### <a name="about-averaged-perceptron-models"></a>Acerca de los modelos de perceptrón promedio de dos clases

El *método de perceptrón promedio* es una versión anterior y sencilla de una red neuronal. En este enfoque, las entradas se clasifican en varias posibles salidas según una función lineal y, a continuación, se combinan con un conjunto de pesos que se derivan del vector de característica, de ahí, el nombre "perceptrón".

Los modelos de perceptrón más sencillos son adecuados para aprender patrones separables linealmente, mientras que las redes neuronales (especialmente las redes neuronales profundas) pueden modelar límites de clase más complejos. Sin embargo, los perceptrones son más rápidos y, puesto que procesan los casos en serie, pueden usarse en el entrenamiento continuo.

## <a name="how-to-configure-two-class-averaged-perceptron"></a>Procedimiento para configurar el perceptrón promedio de dos clases

1.  Agregue el módulo **Perceptrón promedio de dos clases** a la canalización.  

2.  Especifique cómo quiere que se entrene el modelo, estableciendo la opción **Create trainer mode** (Crear modo entrenador).  
  
    -   **Single Parameter** (Parámetro único): Si sabe cómo quiere configurar el modelo, proporcione un conjunto específico de valores como argumentos.

    -   **Parameter Range** (Intervalo de parámetros): seleccione esta opción si no está seguro de los mejores parámetros y quiere ejecutar un barrido de parámetros. Seleccione un rango de valores que iterar y el módulo [Optimización de hiperparámetros de un modelo](tune-model-hyperparameters.md) itera en todas las combinaciones posibles de los valores proporcionados para determinar los hiperparámetros que generan los resultados óptimos.  
  
3.  En **Velocidad de aprendizaje**, especifique un valor para la *velocidad de aprendizaje*. Los valores de velocidad de aprendizaje controlan el tamaño del paso que se utiliza en el descenso de gradiente estocástico cada vez que se prueba y se corrige el modelo.
  
     Al disminuir la velocidad, se prueba el modelo más a menudo, con el riesgo de que pueda quedarse atascado en un nivel local. Al aumentar el paso, puede convergir con mayor rapidez, con el riesgo de superar los mínimos verdaderos.
  
4.  En **Maximum number of iterations** (Número máximo de iteraciones), escriba el número de veces que desea que el algoritmo examine los datos de entrenamiento.  
  
     Si se detiene pronto proporciona, a menudo, una mejor generalización. Aumentar el número de iteraciones mejora el ajuste, con riesgo de sobreajuste.
  
5.  En **Random number seed** (Inicialización de número aleatorio), escriba opcionalmente un valor de entero que se usará como inicialización. Se recomienda usar una inicialización si desea garantizar la reproducibilidad de la canalización a través de las ejecuciones.  
  
1.  Conecte un conjunto de datos de entrenamiento y uno de los módulos de entrenamiento:
  
    -   Si establece **Create trainer mode** (Crear modo entrenador) en **Single Parameter** (Parámetro único), use el módulo [Entrenar modelo](train-model.md).




## <a name="next-steps"></a>Pasos siguientes

Consulte el [conjunto de módulos disponibles](module-reference.md) para Azure Machine Learning. 
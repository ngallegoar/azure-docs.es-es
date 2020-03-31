---
title: 'Árbol de decisión ampliado multiclase: referencia para los módulos'
titleSuffix: Azure Machine Learning
description: Aprenda a usar el módulo Multiclass Boosted Decision Tree (Árbol de decisión ampliado multiclase) en Azure Machine Learning para crear un clasificador mediante datos etiquetados.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 02/19/2020
ms.openlocfilehash: 7d51e3007b7773e28d846f8d30178426f5668cfb
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "77920066"
---
# <a name="multiclass-boosted-decision-tree"></a>Árbol de decisión ampliado multiclase

En este artículo se describe un módulo del diseñador de Azure Machine Learning (versión preliminar).

Utilice este módulo para crear un modelo de Machine Learning basado en el algoritmo de árboles de decisión ampliados.

Un árbol de decisión ampliado es un método de aprendizaje de conjuntos en que el segundo árbol corrige los errores del primer árbol, el tercer árbol corrige los errores del primer y del segundo árbol y así sucesivamente. Las predicciones se basan en el conjunto de árboles juntos.

## <a name="how-to-configure"></a>Cómo se configura 

En este módulo se crea un modelo de clasificación no entrenado. Dado que la clasificación es un método de aprendizaje supervisado, necesita un *conjunto de datos etiquetado* que incluya una columna de etiqueta con un valor para todas las filas.

Puede entrenar este tipo de modelo mediante [Entrenar modelo](././train-model.md). 

1.  Agregue el módulo **Árbol de decisión ampliado multiclase** a la canalización.

1.  Especifique cómo quiere que se entrene el modelo. Para ello, establezca la opción **Create trainer mode** (Crear modo entrenador).

    + **Single Parameter** (Parámetro único): Si sabe cómo quiere configurar el modelo, puede proporcionar un conjunto específico de valores como argumentos.
    
    + **Parameter Range** (Intervalo de parámetros): seleccione esta opción si no está seguro de los mejores parámetros y quiere ejecutar un barrido de parámetros. Seleccione un rango de valores que iterar y el módulo [Optimización de hiperparámetros de un modelo](tune-model-hyperparameters.md) itera en todas las combinaciones posibles de los valores proporcionados para determinar los hiperparámetros que generan los resultados óptimos.  

1. **Maximum number of leaves per tree** (Número máximo de hojas por árbol) limita el número máximo de nodos terminales (hojas) que se pueden crear en un árbol.
    
        By increasing this value, you potentially increase the size of the tree and achieve higher precision, at the risk of overfitting and longer training time.
  
1. **Minimum number of samples per leaf node** (Número mínimo de muestras por nodo hoja) indica el número de casos necesarios para crear cualquier nodo terminal (hoja) en un árbol.  

         By increasing this value, you increase the threshold for creating new rules. For example, with the default value of 1, even a single case can cause a new rule to be created. If you increase the value to 5, the training data would have to contain at least five cases that meet the same conditions.

1. La **velocidad de aprendizaje** define el tamaño del paso durante el aprendizaje. Especifique un número comprendido entre 0 y 1.

         The learning rate determines how fast or slow the learner converges on an optimal solution. If the step size is too large, you might overshoot the optimal solution. If the step size is too small, training takes longer to converge on the best solution.

1. **Number of trees constructed** (Número de árboles construidos) indica el número total de árboles de decisión que se van a crear en el conjunto. Si crea más árboles de decisión, puede obtener una cobertura potencialmente mejor, pero aumentará el tiempo de entrenamiento.

1. **Random number seed** (Inicialización con número aleatorio) establece opcionalmente un número entero no negativo para que se use como valor de inicialización aleatorio. Al especificar un valor, se garantiza la reproducibilidad durante las ejecuciones que tienen los mismos datos y parámetros.  

         The random seed is set by default to 42. Successive runs using different random seeds can have different results.

> [!Note]
> Si establece **Create trainer mode** (Crear modo entrenador) en **Single Parameter** (Parámetro único), conecte un conjunto de datos etiquetado y el módulo [Entrenar modelo](./train-model.md).

## <a name="next-steps"></a>Pasos siguientes

Consulte el [conjunto de módulos disponibles](module-reference.md) para Azure Machine Learning. 

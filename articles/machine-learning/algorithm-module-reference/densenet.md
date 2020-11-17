---
title: DenseNet
titleSuffix: Azure Machine Learning
description: Aprenda a usar el módulo DenseNet en el diseñador de Azure Machine Learning para crear un modelo de clasificación de imágenes con el algoritmo DenseNet.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 09/26/2020
ms.openlocfilehash: d21c7443f6b30d0b7d6e8295c0c9b060a80d9a62
ms.sourcegitcommit: 7cc10b9c3c12c97a2903d01293e42e442f8ac751
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/06/2020
ms.locfileid: "93421335"
---
# <a name="densenet"></a>DenseNet

En este artículo se describe cómo usar el módulo **DenseNet** en el diseñador de Azure Machine Learning para crear un modelo de clasificación de imágenes con el algoritmo DenseNet.  

Este algoritmo de clasificación es un método de aprendizaje supervisado y requiere un directorio de imágenes con etiqueta. 

> [!NOTE]
> Este módulo no admite el conjunto de datos con etiqueta generado a partir del *etiquetado de datos* en el estudio, sino que solo admite el directorio de imágenes con etiqueta generado a partir del módulo [Conversión al directorio de imagen](convert-to-image-directory.md). 

Puede entrenar el modelo proporcionando el modelo y el directorio de imagen con etiquetas como entradas para [Entrenamiento del modelo de PyTorch](train-pytorch-model.md). Después, el modelo entrenado puede utilizarse para predecir valores para los nuevos ejemplos de entrada mediante [Puntuación del modelo de imagen](score-image-model.md).

### <a name="more-about-densenet"></a>Más información sobre DenseNet

Para más información sobre DenseNet, consulte el documento de investigación [Redes circumvolucionales de conexión densa](https://arxiv.org/abs/1608.06993).

## <a name="how-to-configure-densenet"></a>Configuración de DenseNet

1.  Agregue el módulo **DenseNet** a la canalización en el diseñador.  

2.  Como **Nombre del modelo**, especifique el nombre de una determinada estructura DenseNet; puede seleccionar entre las instancias de DenseNet admitidas: "densenet121", "densenet161", "densenet169", "densenet201".

3.  En **Pretrained** (Entrenado previamente), especifique si desea usar un modelo entrenado previamente en ImageNet. Si se selecciona esta opción, puede ajustar el modelo en función de un modelo previamente entrenado seleccionado. Si se anula la selección, puede entrenarlo desde cero.

4.  Por **Memory efficient** (Eficiencia de memoria), especifique si quiere usar puntos de comprobación, que es mucho mejor para la eficiencia de la memoria, pero más lento. Para más información, consulte el documento de investigación [Implementación de DenseNets para un uso eficiente de la memoria](https://arxiv.org/pdf/1707.06990.pdf).

5.  Conecte la salida del módulo **DenseNet** y del módulo de conjunto de datos de imágenes de entrenamiento y validación a [Train Pytorch Model](train-pytorch-model.md) (Entrenamiento del modelo de PyTorch). 

6. Envíe la canalización.


## <a name="results"></a>Results

Una vez completada la ejecución de la canalización, para usar el modelo para la puntuación, conecte [Entrenamiento del modelo de PyTorch](train-pytorch-model.md) a [Puntuación del modelo de imagen](score-image-model.md), para predecir valores para los nuevos ejemplos de entrada.

## <a name="technical-notes"></a>Notas técnicas  

###  <a name="module-parameters"></a>Parámetros del módulo  

| Nombre             | Intervalo | Tipo    | Valor predeterminado     | Descripción                              |
| ---------------- | ----- | ------- | ----------- | ---------------------------------------- |
| Nombre del modelo       | Any   | Mode    | densenet201 | Nombre de una determinada estructura DenseNet     |
| Entrenado previamente       | Any   | Boolean | True        | Si desea usar un modelo entrenado previamente en ImageNet |
| Eficiencia de memoria | Any   | Boolean | False       | Si quiere usar puntos de comprobación, mucho mejor para la eficiencia de la memoria, pero más lento |

###  <a name="output"></a>Output  

| Nombre            | Tipo                    | Descripción                              |
| --------------- | ----------------------- | ---------------------------------------- |
| Modelo no entrenado | UntrainedModelDirectory | Un modelo DenseNet no entrenado que se puede conectar a Train Pytorch Model (Entrenamiento del modelo de PyTorch). |

## <a name="next-steps"></a>Pasos siguientes

Consulte el [conjunto de módulos disponibles](module-reference.md) para Azure Machine Learning. 

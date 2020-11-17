---
title: División de directorio de imagen
titleSuffix: Azure Machine Learning
description: Aprenda a usar el módulo de división de directorios de imágenes en el diseñador para dividir las imágenes de un directorio de imágenes en dos conjuntos distintos.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 09/26/2020
ms.openlocfilehash: 3ee4dd9b2e344ecb3e1a6424ce7310270e7cd076
ms.sourcegitcommit: 7cc10b9c3c12c97a2903d01293e42e442f8ac751
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/06/2020
ms.locfileid: "93421199"
---
# <a name="split-image-directory"></a>División de directorio de imagen

En este tema se describe cómo usar el módulo División del directorio de imagen en el diseñador de Azure Machine Learning para dividir las imágenes de un directorio de imagen en dos conjuntos distintos.

Este módulo es especialmente útil cuando tiene que separar los datos de imagen en conjuntos de entrenamiento y de prueba. 

## <a name="how-to-configure-split-image-directory"></a>Procedimientos para configurar Split Image Directory (División del directorio de imagen)

1. Agregue el módulo **Split Image Directory** (División del directorio de imagen) a la canalización. Puede encontrar este módulo en la categoría "Computer Vision/Image Data Transformation".

2. Conéctelo al módulo del que la salida es el directorio de la imagen.

3. Especifique **Fraction of images in the first output** (Fracción de imágenes de la primera salida) para determinar el porcentaje de datos que se van a colocar en la división izquierda, de forma predeterminada el valor es 0,9. Si el resultado de la fracción no es un entero, el módulo usa el entero más cercano de menor tamaño.


## <a name="technical-notes"></a>Notas técnicas

### <a name="expected-inputs"></a>Entradas esperadas

| Nombre                  | Tipo           | Descripción              |
| --------------------- | -------------- | ------------------------ |
| Directorio de imagen de entrada | ImageDirectory | Directorio de la imagen que se va a dividir |

### <a name="module-parameters"></a>Parámetros del módulo

| Nombre                                   | Tipo  | Intervalo | Opcional | Descripción                            | Valor predeterminado |
| -------------------------------------- | ----- | ----- | -------- | -------------------------------------- | ------- |
| Fracción de imágenes de la primera salida | Float | 0-1   | Obligatorio | Fracción de imágenes de la primera salida | 0.9     |

### <a name="outputs"></a>Salidas

| Nombre                    | Tipo           | Descripción                              |
| ----------------------- | -------------- | ---------------------------------------- |
| Directorio1 de imagen de salida | ImageDirectory | Directorio de imagen que contiene las imágenes seleccionadas |
| Directorio2 de imagen de salida | ImageDirectory | Directorio de imagen que contiene el resto de las imágenes |

## <a name="next-steps"></a>Pasos siguientes

Consulte el [conjunto de módulos disponibles](module-reference.md) para Azure Machine Learning. 


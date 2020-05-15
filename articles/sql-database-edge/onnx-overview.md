---
title: Aprendizaje automático e inteligencia artificial con ONNX en la versión preliminar de Azure SQL Database Edge | Microsoft Docs
description: El aprendizaje automático en la versión preliminar de Azure SQL Database Edge admite modelos del formato Open Neural Network Exchange (ONNX). ONNX es un formato abierto que puede usar para intercambiar modelos entre varios marcos y herramientas de aprendizaje automático.
keywords: Implementación de SQL Database Edge
services: sql-database-edge
ms.service: sql-database-edge
ms.subservice: ''
ms.topic: conceptual
author: dphansen
ms.author: davidph
ms.date: 03/26/2020
ms.openlocfilehash: 53e884c6ef0b03b3bbc1a0c9729f497e40290416
ms.sourcegitcommit: 309a9d26f94ab775673fd4c9a0ffc6caa571f598
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/09/2020
ms.locfileid: "82994298"
---
# <a name="machine-learning-and-ai-with-onnx-in-sql-database-edge-preview"></a>Aprendizaje automático e inteligencia artificial con ONNX en la versión preliminar de SQL Database Edge

El aprendizaje automático en la versión preliminar de Azure SQL Database Edge admite modelos del formato [Open Neural Network Exchange (ONNX)](https://onnx.ai/). ONNX es un formato abierto que puede usar para intercambiar modelos entre varios [marcos y herramientas de aprendizaje automático](https://onnx.ai/supported-tools).

## <a name="overview"></a>Información general

Para inferir modelos de Machine Learning en Azure SQL Database Edge, primero es preciso obtener un modelo. Puede tratarse de un modelo previamente entrenado, o bien de un modelo personalizado entrenado con la plataforma que prefiera. Azure SQL Database Edge admite el formato ONNX, por lo que tendrá que convertir el modelo a este formato. Esto no debería afectar a la precisión del modelo y, una vez que tenga el modelo ONNX, puede implementar el modelo en Azure SQL Database perimetral y usar la [puntuación nativa con la función PREDICT de T-SQL de predicción](/sql/advanced-analytics/sql-native-scoring/).

## <a name="get-onnx-models"></a>Obtención de modelos de ONNX

Para obtener un modelo en el formato ONNX:

- **Servicios de construcción de modelos**: Servicios como la [función de aprendizaje automático automatizado en Azure Machine Learning](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/classification-bank-marketing-all-features/auto-ml-classification-bank-marketing-all-features.ipynb) y [Azure Custom Vision Service](https://docs.microsoft.com/azure/cognitive-services/custom-vision-service/getting-started-build-a-classifier) admiten la exportación directa del modelo entrenado en el formato ONNX.

- [**Convertir y/o exportar modelos existentes**](https://github.com/onnx/tutorials#converting-to-onnx-format): Varios marcos de entrenamiento (p.ej. [PyTorch](https://pytorch.org/docs/stable/onnx.html), Chainer, y Caffe2) admiten la funcionalidad de exportación nativa a ONNX, lo que permite guardar el modelo entrenado en una versión específica del formato ONNX. Para los marcos que no admiten la exportación nativa, existen paquetes instalables de convertidor ONNX independientes que le permiten convertir modelos entrenados desde diferentes marcos de aprendizaje automático al formato ONNX.

     **Marcos admitidos**
   * [PyTorch](http://pytorch.org/docs/master/onnx.html)
   * [Tensorflow](https://github.com/onnx/tensorflow-onnx)
   * [Keras](https://github.com/onnx/keras-onnx)
   * [Scikit-learn](https://github.com/onnx/sklearn-onnx)
   * [CoreML](https://github.com/onnx/onnxmltools)
    
    Para obtener la lista completa de los marcos y ejemplos admitidos, consulte [Conversión al formato ONNX](https://github.com/onnx/tutorials#converting-to-onnx-format).

## <a name="limitations"></a>Limitaciones

Actualmente, no todas las versiones de ONNX son compatibles con Azure SQL Database Edge. La compatibilidad se limita a los modelos con **tipos de datos numéricos**:

- [int y bigint](https://docs.microsoft.com/sql/t-sql/data-types/int-bigint-smallint-and-tinyint-transact-sql)
- [real y float](https://docs.microsoft.com/sql/t-sql/data-types/float-and-real-transact-sql).
  
Otros tipos numéricos se pueden convertir en tipos compatibles mediante [CAST y CONVERT](https://docs.microsoft.com/sql/t-sql/functions/cast-and-convert-transact-sql).

Las entradas del modelo deben estructurarse de manera que cada entrada al modelo corresponda a una sola columna en una tabla. Por ejemplo, si usa una trama de datos de Pandas para entrenar un modelo, cada entrada debe ser una columna independiente del modelo.

## <a name="next-steps"></a>Pasos siguientes

- [Implementación de SQL Database Edge mediante Azure Portal](deploy-portal.md)
- [Implementación de un modelo de ONNX en la versión preliminar de Azure SQL Database Edge](deploy-onnx.md)

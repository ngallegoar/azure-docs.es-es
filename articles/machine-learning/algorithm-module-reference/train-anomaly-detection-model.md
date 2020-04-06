---
title: 'Entrenamiento de un modelo de detección de anomalías: referencia para los módulos'
titleSuffix: Azure Machine Learning
description: Aprenda a usar el módulo entrenar el modelo de detección de anomalías para crear un modelo de detección de anomalías entrenado.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 02/22/2020
ms.openlocfilehash: 78ed2d85729cce94e8dfa579545f558d2cfe4651
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "79504076"
---
# <a name="train-anomaly-detection-model"></a>Entrenamiento de un modelo de detección de anomalías

En este artículo se describe cómo usar el módulo **Entrenamiento de un modelo de detección de anomalías** en Azure Machine Learning Designer (versión preliminar) para crear un modelo de detección de anomalías entrenado.

El módulo toma como entrada un conjunto de parámetros de modelo para el modelo de detección de anomalías y un conjunto de datos sin etiquetar. Devuelve un modelo de detección de anomalías entrenado junto con un conjunto de etiquetas para los datos de entrenamiento.  

Para más información sobre los algoritmos de detección de anomalías que se proporcionan en el diseñador, consulte estos temas: 

+ [Detección de anomalías basada en PCA](pca-based-anomaly-detection.md)  

## <a name="how-to-configure-train-anomaly-detection-model"></a>Configuración del entrenamiento de un modelo de detección de anomalías 

1.  Agregue el módulo **Entrenamiento de un modelo de detección de anomalías** a la canalización en el diseñador. Puede encontrar este módulo en la categoría **Detección de anomalías**.

2. Conecte uno de los módulos diseñados para la detección de anomalías, como [Detección de anomalías basada en PCA](pca-based-anomaly-detection.md).

    No se admiten otros tipos de modelos o se obtendrá el error siguiente al ejecutar la canalización: Todos los modelos deben tener el mismo tipo de aprendiz.  

3.  Para configurar el módulo de detección de anomalías, elija la columna de etiqueta y establezca otros parámetros específicos para el algoritmo.  

4.  Adjunte un conjunto de datos de entrenamiento a la entrada de la derecha de **Entrenamiento de un modelo de detección de anomalías**.  

5.  Envíe la canalización.  

## <a name="results"></a>Results

Una vez completado el entrenamiento:

+ Para ver los parámetros del modelo, haga clic con el botón derecho en el módulo y seleccione **Visualizar**. 

+ Para crear predicciones, use [Puntuación de modelo](score-model.md) con datos de entrada nuevos.

+ Para guardar una instantánea del modelo entrenado, seleccione el módulo y haga clic en el icono **Registrar conjunto de datos** en la pestaña **Outputs+logs** (Salidas y registros) en el panel derecho.   

 
## <a name="next-steps"></a>Pasos siguientes

Consulte el [conjunto de módulos disponibles](module-reference.md) para Azure Machine Learning. 

Consulte [Excepciones y códigos de error para el diseñador (versión preliminar)](designer-error-codes.md) para ver una lista de errores específica de los módulos del diseñador.
'
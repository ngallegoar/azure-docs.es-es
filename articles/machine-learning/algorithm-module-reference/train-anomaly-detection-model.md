---
title: 'Entrenamiento de un modelo de detección de anomalías: Referencia del módulo'
titleSuffix: Azure Machine Learning
description: Aprenda a usar el módulo entrenar el modelo de detección de anomalías para crear un modelo de detección de anomalías entrenado.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 02/22/2020
ms.openlocfilehash: 73e8f0de9f879498acb71e97fa47c81b3447252d
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "84749607"
---
# <a name="train-anomaly-detection-model-module"></a>Módulo Train Anomaly Detection Model

En este artículo se describe cómo usar el módulo Train Anomaly Detection Model (Entrenar un modelo de detección de anomalías) en Azure Machine Learning Designer (versión preliminar) para crear un modelo de detección de anomalías entrenado.

El módulo toma como entrada un conjunto de parámetros para un modelo de detección de anomalías y un conjunto de datos sin etiquetar. Devuelve un modelo de detección de anomalías entrenado junto con un conjunto de etiquetas para los datos de entrenamiento.  

Para más información sobre los algoritmos de detección de anomalías que se proporcionan en el diseñador, consulte [Detección de anomalías basada en PCA](pca-based-anomaly-detection.md).  

## <a name="how-to-configure-train-anomaly-detection-model"></a>Configuración del entrenamiento de un modelo de detección de anomalías 

1.  Agregue el módulo **Entrenamiento de un modelo de detección de anomalías** a la canalización en el diseñador. Puede encontrar este módulo en la categoría **Anomaly Detection** (Detección de anomalías).

2. Conecte uno de los módulos diseñados para la detección de anomalías, como [Detección de anomalías basada en PCA](pca-based-anomaly-detection.md).

    No se admiten otros tipos de modelos. Al ejecutar la canalización, obtendrá el error "All models must have the same learner type" (Todos los modelos tienen que tener el mismo tipo de aprendiz).  

3.  Para configurar el módulo de detección de anomalías, elija la columna de etiqueta y establezca otros parámetros específicos para el algoritmo.  

4.  Asocie un conjunto de datos de entrenamiento a la entrada de la derecha de **Train Anomaly Detection Mode** (Entrenamiento de un modelo de detección de anomalías).  

5.  Envíe la canalización.  

## <a name="results"></a>Results

Una vez completado el entrenamiento:

+ Para ver los parámetros del modelo, haga clic con el botón derecho en el módulo y seleccione **Visualizar**. 

+ Para crear predicciones, use el módulo [Score Model](score-model.md) (Puntuar modelo) con datos de entrada nuevos.

+ Para guardar una instantánea del modelo entrenado, seleccione el módulo. A continuación, seleccione el icono de **Registro de un conjunto de datos** en la pestaña **Outputs+logs** (Salidas+registros) del panel derecho.   

 
## <a name="next-steps"></a>Pasos siguientes

Consulte el [conjunto de módulos disponibles](module-reference.md) para Azure Machine Learning. 

Consulte [Excepciones y códigos de error para el diseñador (versión preliminar)](designer-error-codes.md) para ver una lista de errores específica de los módulos del diseñador.
'
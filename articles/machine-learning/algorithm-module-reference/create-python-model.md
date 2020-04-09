---
title: 'Create Python Model: Referencia del módulo'
titleSuffix: Azure Machine Learning
description: Aprenda a usar el módulo Create Python Model (Crear modelo de Python) de Azure Machine Learning para crear un módulo de modelado o procesamiento de datos personalizado.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 11/19/2019
ms.openlocfilehash: 929938bba9c9512ecfd663a540cf4a7ebbf68e2b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "79371824"
---
# <a name="create-python-model-module"></a>Módulo Create Python Model (Crear modelo de Python)

En este artículo se describe un módulo del diseñador de Azure Machine Learning (versión preliminar).

Aprenda a usar el módulo Create Python Model (Crear modelo de Python) para crear un modelo no entrenado a partir de un script de Python. Puede basar el modelo en cualquier aprendiz que esté incluido en un paquete de Python en el entorno del diseñador de Azure Machine Learning. 

Después de crear el modelo, puede usar [Train model](train-model.md) (Entrenar modelo) para entrenar el modelo en un conjunto de datos, al igual que cualquier otro aprendiz en Azure Machine Learning. El modelo entrenado puede pasarse a [Score Model](score-model.md) (Puntuar modelo) para hacer predicciones. Después, puede guardar el modelo entrenado y publicar el flujo de trabajo de puntuación como un servicio web.

> [!WARNING]
> Actualmente no es posible pasar los resultados puntuados de un modelo de Python a [Evaluate Model](evaluate-model.md) (Evaluar modelo). Si tiene que evaluar un modelo, puede escribir un script de Python personalizado y ejecutarlo mediante el módulo [Execute Python Script](execute-python-script.md) (Ejecutar script de Python).  


## <a name="configure-the-module"></a>Configuración del módulo

El uso de este módulo requiere conocimientos intermedios o expertos de Python. El módulo admite el uso de cualquier aprendiz que se incluya en los paquetes de Python ya instalados en Azure Machine Learning. Consulte la lista de paquetes de Python instalados previamente en [Execute Python Script](execute-python-script.md) (Ejecutar script de Python).
  

En este artículo se muestra cómo usar **Create Python Model** (Crear modelo de Python) con una canalización sencilla. Este es el diagrama de la canalización:

![Diagrama de Create Python Model (Crear modelo de Python)](./media/module/create-python-model.png)

1. Seleccione **Create Python Model** (Crear modelo de Python) y edite el script para implementar el proceso de modelado o administración de datos. Puede basar el modelo en cualquier aprendiz que se incluya en un paquete de Python en el entorno de Azure Machine Learning.

   El código de ejemplo siguiente del clasificador Naive Bayes de dos clases usa el conocido paquete *sklearn*:

   ```Python

   # The script MUST define a class named AzureMLModel.
   # This class MUST at least define the following three methods:
       # __init__: in which self.model must be assigned,
       # train: which trains self.model, the two input arguments must be pandas DataFrame,
       # predict: which generates prediction result, the input argument and the prediction result MUST be pandas DataFrame.
   # The signatures (method names and argument names) of all these methods MUST be exactly the same as the following example.


   import pandas as pd
   from sklearn.naive_bayes import GaussianNB


   class AzureMLModel:
       def __init__(self):
           self.model = GaussianNB()
           self.feature_column_names = list()

       def train(self, df_train, df_label):
           self.feature_column_names = df_train.columns.tolist()
           self.model.fit(df_train, df_label)

       def predict(self, df):
           return pd.DataFrame(
               {'Scored Labels': self.model.predict(df[self.feature_column_names]), 
                'probabilities': self.model.predict_proba(df[self.feature_column_names])[:, 1]}
           )


   ```

1. Conecte el módulo **Create Python Model** (Crear modelo de Python) que acaba de crear a **Train Model** (Entrenar modelo) y **Score Model** (Puntuar modelo).

1. Si tiene que evaluar el modelo, agregue un módulo [Execute Python Script](execute-python-script.md) (Ejecutar script de Python) y edite el script de Python.

   El siguiente script es código de evaluación de ejemplo:

   ```Python


   # The script MUST contain a function named azureml_main
   # which is the entry point for this module.

   # imports up here can be used to 
   import pandas as pd

   # The entry point function can contain up to two input arguments:
   #   Param<dataframe1>: a pandas.DataFrame
   #   Param<dataframe2>: a pandas.DataFrame
   def azureml_main(dataframe1 = None, dataframe2 = None):
    
       from sklearn.metrics import accuracy_score, precision_score, recall_score, roc_auc_score, roc_curve
       import pandas as pd
       import numpy as np
    
       scores = dataframe1.ix[:, ("income", "Scored Labels", "probabilities")]
       ytrue = np.array([0 if val == '<=50K' else 1 for val in scores["income"]])
       ypred = np.array([0 if val == '<=50K' else 1 for val in scores["Scored Labels"]])    
       probabilities = scores["probabilities"]
    
       accuracy, precision, recall, auc = \
       accuracy_score(ytrue, ypred),\
       precision_score(ytrue, ypred),\
       recall_score(ytrue, ypred),\
       roc_auc_score(ytrue, probabilities)
    
       metrics = pd.DataFrame();
       metrics["Metric"] = ["Accuracy", "Precision", "Recall", "AUC"];
       metrics["Value"] = [accuracy, precision, recall, auc]
    
       return metrics,

   ```

## <a name="next-steps"></a>Pasos siguientes

Consulte el [conjunto de módulos disponibles](module-reference.md) para Azure Machine Learning. 
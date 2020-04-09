---
title: 'Referencia para el módulo: referencia para los módulos'
titleSuffix: Azure Machine Learning
description: Obtenga información sobre cómo usar el módulo Evaluar modelo en Azure Machine Learning para medir la precisión de un modelo entrenado.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 02/24/2020
ms.openlocfilehash: c1bcbb6a368c9c80f968c48c1a6e0bc6c95133d6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "79456411"
---
# <a name="evaluate-model-module"></a>Módulo Evaluate Model

En este artículo se describe un módulo del diseñador de Azure Machine Learning (versión preliminar).

Use este módulo para medir la precisión de un modelo entrenado. Hay que proporcionar un conjunto de datos que contenga las puntuaciones generadas a partir de un modelo y el módulo **Evaluate Model** calculará un conjunto de métricas de evaluación estándar del sector.
  
 Las métricas devueltas por **Evaluate Model** dependen del tipo de modelo que va a evaluar:  
  
-   **Modelos de clasificación**    
-   **Modelos de regresión**  
-   **Modelos de agrupación en clústeres**  


> [!TIP]
> Si no está familiarizado con la evaluación de modelos, le recomendamos la serie de vídeos del Dr. Stephen Elston, como parte del [curso de aprendizaje automático](https://blogs.technet.microsoft.com/machinelearning/2015/09/08/new-edx-course-data-science-machine-learning-essentials/) de EdX. 


Hay tres formas de usar el módulo **Evaluate Model**:

+ Generar puntuaciones sobre los datos de entrenamiento y evaluar el modelo según estas puntuaciones.
+ Generar puntuaciones en el modelo, pero compararlas con las de un conjunto de pruebas reservado.
+ Comparar las puntuaciones de dos modelos diferentes pero relacionados, usando el mismo conjunto de datos.

## <a name="use-the-training-data"></a>Usar datos de aprendizaje

Para evaluar un modelo, debe conectar un conjunto de datos que contenga un conjunto de puntuaciones y columnas de entrada.  Si no hay otros datos disponibles, puede usar el conjunto de datos original.

1. Conecte la salida de **Scored dataset** (Conjunto de datos puntuados) de [Score Model](./score-model.md) (Puntuar modelo) a la entrada de **Evaluate Model**. 
2. Haga clic en el módulo **Evaluate Model** (Evaluar modelo) y ejecute el experimento para generar las puntuaciones de la evaluación.

## <a name="use-testing-data"></a>Usar datos de prueba

Un escenario común en el aprendizaje automático consiste en separar el conjunto de datos original en conjuntos de datos de entrenamiento y prueba, mediante el módulo [Split](./split-data.md) (Dividir) o el módulo [Partition and Sample](./partition-and-sample.md) (Partición y ejemplo). 

1. Conecte la salida de **Scored dataset** (Conjunto de datos puntuados) de [Score Model](score-model.md) (Puntuar modelo) a la entrada de **Evaluate Model**. 
2. Conecte la salida del módulo Split Data (Dividir datos) que contenga los datos de prueba con la entrada de la derecha de **Evaluate Model**.
2. Haga clic en el módulo **Evaluate Model** y luego en **Ejecutar seleccionado** para generar las puntuaciones de la evaluación.

## <a name="compare-scores-from-two-models"></a>Comparar las puntuaciones de dos modelos

También puede conectar un segundo conjunto de puntuaciones a **Evaluate Model**.  Las puntuaciones podrían ser un conjunto de evaluación compartido con resultados conocidos o un conjunto de resultados de un modelo diferente para los mismos datos.

Esta característica es útil porque permite comparar fácilmente los resultados de dos modelos distintos con los mismos datos. O bien, puede comparar las puntuaciones de las dos ejecuciones diferentes con los mismos datos con parámetros diferentes.

1. Conecte la salida de **Scored dataset** (Conjunto de datos puntuados) de [Score Model](score-model.md) (Puntuar modelo) a la entrada de **Evaluate Model**. 
2. Conecte la salida del módulo Score Model (Puntuar modelo) para el segundo modelo a la entrada derecha de **Evaluate Model**.
3. Envíe la canalización.

## <a name="results"></a>Results

Después de ejecutar **Evaluate Model** (Evaluar modelo), haga clic con el botón derecho en el módulo y seleccione **Visualize Evaluation results** (Visualizar resultados de evaluación) para ver los resultados.

Si conecta los conjuntos de datos a las dos entradas de **Evaluate Model**, los resultados contendrán las métricas para ambos conjuntos de datos, o ambos modelos.
El modelo o los datos conectados al puerto izquierdo aparecen en primer lugar en el informe, seguido de las métricas del conjunto de datos, o el modelo conectado al puerto derecho.  

Por ejemplo, la imagen siguiente representa una comparación de los resultados de dos modelos de agrupación en clústeres que se crearon con los mismos datos, pero con distintos parámetros.  

![Comparing2Models](media/module/evaluate-2-models.png)  

Debido a que se trata de un modelo de agrupación en clústeres, los resultados de evaluación son diferentes de si compara las puntuaciones de dos modelos de regresión o si compara dos modelos de clasificación. Pero la presentación general es la misma. 

## <a name="metrics"></a>Métricas

Esta sección describe las métricas devueltas para los tipos específicos de los modelos admitidos para su uso con **Evaluate Model**:

+ [Modelos de clasificación](#metrics-for-classification-models)
+ [Modelos de regresión](#metrics-for-regression-models)
+ [Modelos de agrupación en clústeres](#metrics-for-clustering-models)

### <a name="metrics-for-classification-models"></a>Métricas para modelos de clasificación

Las siguientes métricas se notifican al evaluar los modelos de clasificación.
  
-   **Precisión** (Accuracy) mide la calidad de un modelo de clasificación como la proporción de resultados verdaderos en el total de casos.  
  
-   **Precisión** es la proporción de resultados verdaderos de todos los resultados positivos.  
  
-   **Recuperación** es una fracción de todos los resultados correctos devueltos por el modelo.  
  
-   **Puntuación F** se calcula como el promedio ponderado de precisión y recuperación entre 0 y 1, donde el valor ideal de puntuación F es 1.  
  
-   **AUC** mide el área bajo la curva trazada con los verdaderos positivos en el eje y los falsos positivos en el eje x. Esta métrica es útil porque proporciona un número único que le permite comparar los modelos de diferentes tipos.  
  
- **Promedio de pérdida logarítmica** es una puntuación única que se usa para expresar la penalización de los resultados incorrectos. Se calcula como la diferencia entre dos distribuciones de probabilidad: la verdadera y la que aparece en el modelo.  
  
- **Pérdida logarítmica de entrenamiento** es una puntuación única que representa la ventaja del clasificador sobre una predicción aleatoria. La pérdida de registro mide la incertidumbre del modelo comparando las probabilidades que envía a los valores conocidos (datos verdaderos) en las etiquetas. Quiere minimizar la pérdida de registro para el modelo en conjunto.

### <a name="metrics-for-regression-models"></a>Métricas para los modelos de regresión
 
Las métricas devueltas para los modelos de regresión están diseñadas para estimar la cantidad de errores.  Se considera que un modelo se ajusta a los datos correctamente si la diferencia entre los valores observados y los previstos es pequeña. Pero el patrón de los valores residuales (la diferencia entre un punto previsto y su valor real correspondiente) puede indicarle mucho sobre el sesgo potencial en el modelo.  
  
 Las siguientes métricas se notifican para evaluar los modelos de regresión.
  
- **Error medio absoluto (MAE)** : mide la proximidad de las predicciones con respecto a los resultados reales; por lo tanto, cuanto menor es la puntuación, mejor.  
  
- **Error cuadrático medio (RMSE)** : crea un valor único que resume el error en el modelo. Al elevar al cuadrado la diferencia, la métrica no tiene en cuenta la diferencia entre un exceso o un defecto de predicción.  
  
- **Error absoluto relativo (RAE)** : es la diferencia absoluta relativa entre los valores esperados y los reales; es relativa porque la diferencia media se divide por la media aritmética.  
  
- **Error cuadrático relativo (RSE)** : del mismo modo, normaliza el error cuadrático medio total de los valores previstos dividiendo entre el total de errores al cuadrado de los valores reales.  
  

  
- **Coeficiente de determinación**: a menudo conocido como R<sup>2</sup>, representa la eficacia predictiva del modelo como un valor entre 0 y 1. Cero significa que el modelo es aleatorio (no explica nada); 1 significa que hay un ajuste perfecto. Pero hay que tener precaución al interpretar los valores de R<sup>2</sup>, ya que los valores bajos pueden ser completamente normales y los valores altos pueden ser sospechosos.

###  <a name="metrics-for-clustering-models"></a>Métricas para modelos de agrupación en clústeres

Dado que los modelos de agrupación en clústeres se diferencian considerablemente de los modelos de clasificación y regresión en muchos aspectos, [Evaluar modelo](evaluate-model.md) también devuelve un conjunto distinto de estadísticas para los modelos de agrupación en clústeres.  
  
 Las estadísticas devueltas para un modelo de agrupación en clústeres describen el número de puntos de datos que se han asignado a cada clúster, la cantidad de separación entre clústeres y cómo de estrechamente están agrupados los puntos de datos dentro de cada clúster.  
  
 El promedio de las estadísticas del modelo de agrupación en clústeres se realiza en todo el conjunto de datos, con filas adicionales que contienen las estadísticas por clúster.  
  
Se notifican las siguientes métricas para evaluar los modelos de agrupación en clústeres.
    
-   Las puntuaciones de la columna **Average Distance to Other Center** (Distancia media a otro centro) representan la cercanía, como media, de cada punto del clúster a los centroides de todos los demás clústeres.   

-   Las puntuaciones de la columna **Average Distance to Cluster Center** (Distancia media al centro del clúster) representan la cercanía de todos los puntos del clúster al centroide de ese clúster.  
  
-   La columna **Number of Points** (Número de puntos) muestra cuántos puntos de datos se han asignado a cada clúster, junto con el número total de puntos de datos en cualquier clúster.  
  
     Si el número de puntos de datos asignado a los clústeres es menor que el número total de puntos de datos disponibles, significa que no se han podido asignar los puntos de datos a un clúster.  
  
-   Las puntuaciones de la columna **Maximal Distance to Cluster Center** (Distancia máxima al centro del clúster) representan la suma de las distancias entre cada punto y el centroide del clúster de ese punto.  
  
     Si este número es alto, puede significar que el clúster está muy disperso. Debe revisar esta estadística junto con **Average Distance to Cluster Center** (Distancia media al centro del clúster) para determinar la dispersión del clúster.   

-   La puntuación de **Combined Evaluation** (Evaluación combinada) en la parte inferior de cada sección de resultados indica las puntuaciones medias de los clústeres creados en ese modelo determinado.  
  

## <a name="next-steps"></a>Pasos siguientes

Consulte el [conjunto de módulos disponibles](module-reference.md) para Azure Machine Learning. 
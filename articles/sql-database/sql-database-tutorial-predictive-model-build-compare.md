---
title: 'Tutorial: Entrenamiento y comparación de modelos predictivos en R'
titleSuffix: Azure SQL Database Machine Learning Services (preview)
description: En la segunda parte de esta serie de tutoriales de tres partes, deberá crear dos modelos predictivos en R con Azure SQL Database Machine Learning Services (versión preliminar) y, luego, seleccionar el modelo más preciso.
services: sql-database
ms.service: sql-database
ms.subservice: machine-learning
ms.custom: ''
ms.devlang: r
ms.topic: tutorial
author: garyericson
ms.author: garye
ms.reviewer: davidph
manager: cgronlun
ms.date: 07/26/2019
ms.openlocfilehash: 52f4347fa545df88f11878709b4061662d5faffe
ms.sourcegitcommit: 8a9c54c82ab8f922be54fb2fcfd880815f25de77
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "80345750"
---
# <a name="tutorial-create-a-predictive-model-in-r-with-azure-sql-database-machine-learning-services-preview"></a>Tutorial: Creación de un modelo predictivo en R con Azure SQL Database Machine Learning Services (versión preliminar)

En la segunda parte de esta serie de tutoriales de tres partes, creará dos modelos predictivos en R y seleccionará el modelo más preciso. En la siguiente parte de esta serie, implementará este modelo en una base de datos SQL con Azure SQL Database Machine Learning Services (versión preliminar).

[!INCLUDE[ml-preview-note](../../includes/sql-database-ml-preview-note.md)]

En este artículo, aprenderá a:

> [!div class="checklist"]
> * Entrenar dos modelos de aprendizaje automático
> * Hacer predicciones a partir de ambos modelos
> * Comparar los resultados para elegir el modelo más preciso

En la [primera parte](sql-database-tutorial-predictive-model-prepare-data.md), aprendió a importar una base de datos de ejemplo y, luego, a preparar los datos para su uso en el entrenamiento de un modelo predictivo en R.

En la [tercera parte](sql-database-tutorial-predictive-model-deploy.md), aprenderá a almacenar el modelo en una base de datos y, luego, a crear procedimientos almacenados a partir de los scripts de R desarrollados en las partes uno y dos. Los procedimientos almacenados se ejecutarán en una base de datos SQL para hacer predicciones basadas en nuevos datos.

[!INCLUDE[ml-preview-note](../../includes/sql-database-ml-preview-note.md)]

## <a name="prerequisites"></a>Prerrequisitos

* En la segunda parte de este tutorial se da por hecho que ha realizado la [**primera parte**](sql-database-tutorial-predictive-model-prepare-data.md) y que satisface sus requisitos previos.

## <a name="train-two-models"></a>Entrenamiento de dos modelos

Para encontrar el mejor modelo para los datos de alquiler de esquís, cree dos modelos distintos (regresión lineal y árbol de decisión) y vea cuál es más preciso en sus predicciones. Deberá usar la trama de datos `rentaldata` que creó en la primera parte de esta serie.

```r
#First, split the dataset into two different sets:
# one for training the model and the other for validating it
train_data = rentaldata[rentaldata$Year < 2015,];
test_data  = rentaldata[rentaldata$Year == 2015,];

#Use the RentalCount column to check the quality of the prediction against actual values
actual_counts <- test_data$RentalCount;

#Model 1: Use rxLinMod to create a linear regression model, trained with the training data set
model_linmod <- rxLinMod(RentalCount ~  Month + Day + WeekDay + Snow + Holiday, data = train_data);

#Model 2: Use rxDTree to create a decision tree model, trained with the training data set
model_dtree  <- rxDTree(RentalCount ~ Month + Day + WeekDay + Snow + Holiday, data = train_data);
```

## <a name="make-predictions-from-both-models"></a>Hacer predicciones a partir de ambos modelos

Use una función de predicción para predecir el número de alquileres con cada modelo entrenado.

```r
#Use both models to make predictions using the test data set.
predict_linmod <- rxPredict(model_linmod, test_data, writeModelVars = TRUE, extraVarsToWrite = c("Year"));

predict_dtree  <- rxPredict(model_dtree,  test_data, writeModelVars = TRUE, extraVarsToWrite = c("Year"));

#To verify it worked, look at the top rows of the two prediction data sets.
head(predict_linmod);
head(predict_dtree);
```

```results
    RentalCount_Pred  RentalCount  Month  Day  WeekDay  Snow  Holiday
1         27.45858          42       2     11     4      0       0
2        387.29344         360       3     29     1      0       0
3         16.37349          20       4     22     4      0       0
4         31.07058          42       3      6     6      0       0
5        463.97263         405       2     28     7      1       0
6        102.21695          38       1     12     2      1       0
    RentalCount_Pred  RentalCount  Month  Day  WeekDay  Snow  Holiday
1          40.0000          42       2     11     4      0       0
2         332.5714         360       3     29     1      0       0
3          27.7500          20       4     22     4      0       0
4          34.2500          42       3      6     6      0       0
5         645.7059         405       2     28     7      1       0
6          40.0000          38       1     12     2      1       0
```

## <a name="compare-the-results"></a>Comparación de los resultados

Ahora quiere ver cuál de los modelos ofrece las mejores predicciones. Una manera rápida y sencilla de hacerlo es usar una función de trazado básica para ver la diferencia entre los valores reales de los datos de entrenamiento y los valores previstos.

```r
#Use the plotting functionality in R to visualize the results from the predictions
par(mfrow = c(2, 1));
plot(predict_linmod$RentalCount_Pred - predict_linmod$RentalCount, main = "Difference between actual and predicted. rxLinmod");
plot(predict_dtree$RentalCount_Pred  - predict_dtree$RentalCount,  main = "Difference between actual and predicted. rxDTree");
```

![Comparación de los dos modelos](./media/sql-database-tutorial-predictive-model-build-compare/compare-models.png)

Parece que el modelo de árbol de decisión es el más preciso de los dos modelos.

## <a name="clean-up-resources"></a>Limpieza de recursos

Si no va a continuar con este tutorial, elimine la base de datos TutorialDB del servidor de Azure SQL Database.

En Azure Portal, haga lo siguiente:

1. En el menú izquierdo de Azure Portal, seleccione **Todos los recursos** o **Bases de datos SQL**.
1. En el campo **Filtrar por nombre...** , escriba **TutorialDB** y seleccione su suscripción.
1. Seleccione la base de datos TutorialDB.
1. En la página **Información general**, seleccione **Eliminar**.

## <a name="next-steps"></a>Pasos siguientes

En la parte dos de esta serie de tutoriales, ha completado estos pasos:

* Entrenar dos modelos de aprendizaje automático
* Hacer predicciones a partir de ambos modelos
* Comparar los resultados para elegir el modelo más preciso

Para implementar el modelo de aprendizaje automático que ha creado, siga la tercera parte de esta serie de tutoriales:

> [!div class="nextstepaction"]
> [Tutorial: Implementación de un modelo predictivo en R con Azure SQL Database Machine Learning Services (versión preliminar)](sql-database-tutorial-predictive-model-deploy.md)

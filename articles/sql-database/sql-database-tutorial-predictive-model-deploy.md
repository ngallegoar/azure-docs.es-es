---
title: 'Tutorial: Implementación de un modelo predictivo en R'
titleSuffix: Azure SQL Database Machine Learning Services (preview)
description: En la tercera parte de este tutorial de tres partes, implementará un modelo predictivo en R con Azure SQL Database Machine Learning Services (versión preliminar).
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
ms.openlocfilehash: 7779db053344f99238d38d5d49762730efbc5fc4
ms.sourcegitcommit: 8a9c54c82ab8f922be54fb2fcfd880815f25de77
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "80346319"
---
# <a name="tutorial-deploy-a-predictive-model-in-r-with-azure-sql-database-machine-learning-services-preview"></a>Tutorial: Implementación de un modelo predictivo en R con Azure SQL Database Machine Learning Services (versión preliminar)

En la tercera parte de este tutorial de tres partes, implementará un modelo predictivo, desarrollado en R, en una base de datos SQL con Azure SQL Database Machine Learning Services (versión preliminar).

[!INCLUDE[ml-preview-note](../../includes/sql-database-ml-preview-note.md)]

Creará un procedimiento almacenado con un script de R insertado que realiza predicciones mediante el modelo. Como el modelo se ejecuta en la base de datos de Azure SQL, se puede entrenar fácilmente con los datos almacenados en la base de datos.

En este artículo, con los scripts de R que desarrolló en las partes uno y dos, aprenderá a:

> [!div class="checklist"]
> * Crear un procedimiento almacenado que genere el modelo de aprendizaje automático
> * Almacenar el modelo en una tabla de base de datos
> * Crear un procedimiento almacenado que realice predicciones mediante el modelo
> * Ejecutar el modelo con datos nuevos

En la [primera parte](sql-database-tutorial-predictive-model-prepare-data.md), aprendió a importar una base de datos de ejemplo y, luego, a preparar los datos para su uso en el entrenamiento de un modelo predictivo en R.

En la [segunda parte](sql-database-tutorial-predictive-model-build-compare.md), aprendió a crear y entrenar varios modelos de Machine Learning en R y, luego, elegir el más preciso.

[!INCLUDE[ml-preview-note](../../includes/sql-database-ml-preview-note.md)]

## <a name="prerequisites"></a>Prerrequisitos

* En la tercera parte de esta serie de tutoriales se da por hecho que ha completado [**la primera parte**](sql-database-tutorial-predictive-model-prepare-data.md) y la [**segunda parte**](sql-database-tutorial-predictive-model-build-compare.md).

## <a name="create-a-stored-procedure-that-generates-the-model"></a>Crear un procedimiento almacenado que genere el modelo

En la segunda parte de esta serie de tutoriales, decidió que un modelo de árbol de decisión (dtree) era el más preciso. Ahora, con los scripts de R desarrollados, cree un procedimiento almacenado (`generate_rental_rx_model`) que entrene y genere el modelo dtree mediante rxDTree desde el paquete RevoScaleR.

Ejecute los comandos siguientes en Azure Data Studio o SSMS.

```sql
-- Stored procedure that trains and generates an R model using the rental_data and a decision tree algorithm
DROP PROCEDURE IF EXISTS generate_rental_rx_model;
GO
CREATE PROCEDURE generate_rental_rx_model (@trained_model VARBINARY(max) OUTPUT)
AS
BEGIN
    EXECUTE sp_execute_external_script @language = N'R'
        , @script = N'
require("RevoScaleR");

rental_train_data$Holiday <- factor(rental_train_data$Holiday);
rental_train_data$Snow    <- factor(rental_train_data$Snow);
rental_train_data$WeekDay <- factor(rental_train_data$WeekDay);

#Create a dtree model and train it using the training data set
model_dtree <- rxDTree(RentalCount ~ Month + Day + WeekDay + Snow + Holiday, data = rental_train_data);
#Serialize the model before saving it to the database table
trained_model <- as.raw(serialize(model_dtree, connection=NULL));
'
        , @input_data_1 = N'
            SELECT RentalCount
                 , Year
                 , Month
                 , Day
                 , WeekDay
                 , Snow
                 , Holiday
            FROM dbo.rental_data
            WHERE Year < 2015
            '
        , @input_data_1_name = N'rental_train_data'
        , @params = N'@trained_model varbinary(max) OUTPUT'
        , @trained_model = @trained_model OUTPUT;
END;
GO
```

## <a name="store-the-model-in-a-database-table"></a>Almacenar el modelo en una tabla de base de datos

Cree una tabla en la base de datos TutorialDB y, después, guarde el modelo en la tabla.

1. Cree una tabla (`rental_rx_models`) para almacenar el modelo.

    ```sql
    USE TutorialDB;
    DROP TABLE IF EXISTS rental_rx_models;
    GO
    CREATE TABLE rental_rx_models (
          model_name VARCHAR(30) NOT NULL DEFAULT('default model') PRIMARY KEY
        , model VARBINARY(MAX) NOT NULL
        );
    GO
    ```

1. Guarde el modelo en la tabla como un objeto binario, con el nombre "rxDTree".

    ```sql
    -- Save model to table
    TRUNCATE TABLE rental_rx_models;
    
    DECLARE @model VARBINARY(MAX);
    
    EXECUTE generate_rental_rx_model @model OUTPUT;
    
    INSERT INTO rental_rx_models (
          model_name
        , model
        )
    VALUES (
         'rxDTree'
        , @model
        );
    
    SELECT *
    FROM rental_rx_models;
    ```

## <a name="create-a-stored-procedure-that-makes-predictions"></a>Creación de un procedimiento almacenado que realiza predicciones

Cree un procedimiento almacenado (`predict_rentalcount_new`) que hace predicciones mediante el modelo entrenado y un conjunto de nuevos datos.

```sql
-- Stored procedure that takes model name and new data as input parameters and predicts the rental count for the new data
DROP PROCEDURE IF EXISTS predict_rentalcount_new;
GO
CREATE PROCEDURE predict_rentalcount_new (
      @model_name VARCHAR(100)
    , @input_query NVARCHAR(MAX)
    )
AS
BEGIN
    DECLARE @rx_model VARBINARY(MAX) = (
            SELECT model
            FROM rental_rx_models
            WHERE model_name = @model_name
            );

    EXECUTE sp_execute_external_script @language = N'R'
        , @script = N'
require("RevoScaleR");

#Convert types to factors
rentals$Holiday <- factor(rentals$Holiday);
rentals$Snow    <- factor(rentals$Snow);
rentals$WeekDay <- factor(rentals$WeekDay);

#Before using the model to predict, we need to unserialize it
rental_model <- unserialize(rx_model);

#Call prediction function
rental_predictions <- rxPredict(rental_model, rentals);
'
        , @input_data_1 = @input_query
        , @input_data_1_name = N'rentals'
        , @output_data_1_name = N'rental_predictions'
        , @params = N'@rx_model varbinary(max)'
        , @rx_model = @rx_model
    WITH RESULT SETS(("RentalCount_Predicted" FLOAT));
END;
GO
```

## <a name="execute-the-model-with-new-data"></a>Ejecutar el modelo con datos nuevos

Ahora puede usar el procedimiento almacenado `predict_rentalcount_new` para predecir el número de alquileres a partir de los nuevos datos.

```sql
-- Use the predict_rentalcount_new stored procedure with the model name and a set of features to predict the rental count
EXECUTE dbo.predict_rentalcount_new @model_name = 'rxDTree'
    , @input_query = '
        SELECT CONVERT(INT,  3) AS Month
             , CONVERT(INT, 24) AS Day
             , CONVERT(INT,  4) AS WeekDay
             , CONVERT(INT,  1) AS Snow
             , CONVERT(INT,  1) AS Holiday
        ';
GO
```

Verá un resultado similar al siguiente:

```results
RentalCount_Predicted
332.571428571429
```

Ha creado, entrenado e implementado correctamente un modelo en una base de datos de Azure SQL. Después, ha usado el modelo en un procedimiento almacenado para predecir valores basándose en datos nuevos.

## <a name="clean-up-resources"></a>Limpieza de recursos

Cuando termine de usar la base de datos TutorialDB, elimínela del servidor de Azure SQL Database.

En Azure Portal, haga lo siguiente:

1. En el menú izquierdo de Azure Portal, seleccione **Todos los recursos** o **Bases de datos SQL**.
1. En el campo **Filtrar por nombre...** , escriba **TutorialDB** y seleccione su suscripción.
1. Seleccione la base de datos TutorialDB.
1. En la página **Información general**, seleccione **Eliminar**.

## <a name="next-steps"></a>Pasos siguientes

En la parte tres de esta serie de tutoriales, ha completado estos pasos:

* Crear un procedimiento almacenado que genere el modelo de aprendizaje automático
* Almacenar el modelo en una tabla de base de datos
* Crear un procedimiento almacenado que realice predicciones mediante el modelo
* Ejecutar el modelo con datos nuevos

Para más información sobre el uso de R en Azure SQL Database Machine Learning Services (versión preliminar), consulte:

* [Escribir funciones de R avanzadas en Azure SQL Database Machine Learning Services (versión preliminar)](sql-database-machine-learning-services-functions.md)
* [Trabajar con datos SQL y R en Azure SQL Database Machine Learning Services (versión preliminar)](sql-database-machine-learning-services-data-issues.md)
* [Incorporación de un paquete de R a Azure SQL Database Machine Learning Services (versión preliminar)](sql-database-machine-learning-services-add-r-packages.md)

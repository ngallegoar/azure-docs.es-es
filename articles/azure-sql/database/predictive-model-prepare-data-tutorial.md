---
title: 'Tutorial: Preparación de los datos para entrenar un modelo predictivo en R'
titleSuffix: Azure SQL Database Machine Learning Services (preview)
description: En la primera parte de esta serie de tutoriales de tres partes, preparará los datos de una base de datos de Azure SQL para entrenar un modelo predictivo en R con Azure SQL Database Machine Learning Services (versión preliminar).
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
ROBOTS: NOINDEX
ms.openlocfilehash: a82467a097c50314e8f26f4a5cc4507f867ad504
ms.sourcegitcommit: 053e5e7103ab666454faf26ed51b0dfcd7661996
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/27/2020
ms.locfileid: "84024795"
---
# <a name="tutorial-prepare-data-to-train-a-predictive-model-in-r-with-azure-sql-database-machine-learning-services-preview"></a>Tutorial: Preparación de los datos para entrenar un modelo predictivo en R con Azure SQL Database Machine Learning Services (versión preliminar)
[!INCLUDE[appliesto-sqldb](../includes/appliesto-sqldb.md)]

En la primera parte de esta serie de tutoriales de tres partes, importará y preparará los datos de una base de datos de Azure SQL mediante R. Más adelante en la serie, usará estos datos para entrenar e implementar un modelo de Machine Learning predictivo en R con Azure SQL Database Machine Learning Services (versión preliminar).

[!INCLUDE[ml-preview-note](../../../includes/sql-database-ml-preview-note.md)]

En esta serie de tutoriales, imagine que posee una empresa de alquiler de esquís y quiere predecir el número de alquileres que tendrá en una fecha futura. Esta información le ayudará a preparar las existencias, el personal y las instalaciones.

En las partes uno y dos de esta serie, va a desarrollar algunos scripts de R en RStudio para preparar los datos y entrenar un modelo de Machine Learning. Luego, en la tercera parte, ejecutará esos scripts de R en una base de datos SQL mediante procedimientos almacenados.

En este artículo, aprenderá a:

> [!div class="checklist"]
>
> * Importar una base de datos de ejemplo en una base de datos de Azure SQL mediante R
> * Cargar los datos de la base de datos de Azure SQL en una trama de datos de R
> * Preparar los datos en R mediante la identificación de algunas columnas como de categoría

En la [segunda parte](predictive-model-build-compare-tutorial.md), aprenderá a crear y entrenar varios modelos de Machine Learning en R y, luego, elegir el más preciso.

En la [tercera parte](predictive-model-deploy-tutorial.md), aprenderá a almacenar el modelo en una base de datos y, luego, a crear procedimientos almacenados a partir de los scripts de R desarrollados en las partes uno y dos. Los procedimientos almacenados se ejecutarán en una base de datos SQL para hacer predicciones basadas en nuevos datos.

## <a name="prerequisites"></a>Requisitos previos

* Suscripción a Azure: si no tiene una suscripción a Azure, [cree una cuenta](https://azure.microsoft.com/free/) antes de empezar.

* [Azure SQL Database con Machine Learning Services (con R)](machine-learning-services-overview.md) habilitado.

* Paquete de RevoScaleR: consulte [RevoScaleR](https://docs.microsoft.com/sql/advanced-analytics/r/ref-r-revoscaler?view=sql-server-2017#versions-and-platforms) para ver las opciones de instalación de este paquete localmente.

* IDE de R: en este tutorial se usa [RStudio Desktop](https://www.rstudio.com/products/rstudio/download/).

* Herramienta de consulta SQL: en este tutorial se da por hecho que usa [Azure Data Studio](https://docs.microsoft.com/sql/azure-data-studio/what-is) o [SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/sql-server-management-studio-ssms) (SSMS).

## <a name="sign-in-to-the-azure-portal"></a>Inicio de sesión en Azure Portal

Inicie sesión en [Azure Portal](https://portal.azure.com/).

## <a name="import-the-sample-database"></a>Importación de la base de datos de ejemplo

El conjunto de datos de ejemplo usado en este tutorial se ha guardado en un archivo de copia de seguridad de base de datos **.bacpac** para que pueda descargarlo y usarlo.

1. Descargue el archivo [TutorialDB.bacpac](https://sqlchoice.blob.core.windows.net/sqlchoice/static/TutorialDB.bacpac).

1. Siga las instrucciones que se indican en [Importación de un archivo BACPAC para crear una base de datos de Azure SQL](https://docs.microsoft.com/azure/sql-database/sql-database-import), y use estos datos:

   * Importe la base de datos del archivo **TutorialDB.bacpac** que ha descargado.
   * Durante la versión preliminar pública, elija la configuración **Gen5/vCore** (Gen5/Núcleo virtual) para la nueva base de datos.
   * Llame a la nueva base de datos "TutorialDB".

## <a name="load-the-data-into-a-data-frame"></a>Carga de los datos en una trama de datos

Para usar los datos en R, deberá cargar los datos de la base de datos de Azure SQL en una trama de datos (`rentaldata`).

Cree un archivo de RScript en RStudio y ejecute el siguiente script. Reemplace **Server** (Servidor), **UID** y **PWD** por su propia información de conexión.

```r
#Define the connection string to connect to the TutorialDB database
connStr <- paste("Driver=SQL Server",
               "; Server=", "<Logical SQL server>",
               "; Database=TutorialDB",
               "; UID=", "<user>",
               "; PWD=", "<password>",
                  sep = "");

#Get the data from the table
SQL_rentaldata <- RxSqlServerData(table = "dbo.rental_data", connectionString = connStr, returnDataFrame = TRUE);

#Import the data into a data frame
rentaldata <- rxImport(SQL_rentaldata);

#Take a look at the structure of the data and the top rows
head(rentaldata);
str(rentaldata);
```

Se mostrarán resultados similares a los siguientes.

```results
   Year  Month  Day  RentalCount  WeekDay  Holiday  Snow
1  2014    1     20      445         2        1      0
2  2014    2     13       40         5        0      0
3  2013    3     10      456         1        0      0
4  2014    3     31       38         2        0      0
5  2014    4     24       23         5        0      0
6  2015    2     11       42         4        0      0
'data.frame':       453 obs. of  7 variables:
$ Year       : int  2014 2014 2013 2014 2014 2015 2013 2014 2013 2015 ...
$ Month      : num  1 2 3 3 4 2 4 3 4 3 ...
$ Day        : num  20 13 10 31 24 11 28 8 5 29 ...
$ RentalCount: num  445 40 456 38 23 42 310 240 22 360 ...
$ WeekDay    : num  2 5 1 2 5 4 1 7 6 1 ...
$ Holiday    : int  1 0 0 0 0 0 0 0 0 0 ...
$ Snow       : num  0 0 0 0 0 0 0 0 0 0 ...
```

## <a name="prepare-the-data"></a>Preparación de los datos

En esta base de datos de ejemplo, ya se ha realizado la mayor parte de la preparación, pero aún tiene que hacer una cosa más.
Use el siguiente script de R para identificar tres columnas como *categorías*, para lo cual debe cambiar los tipos de datos a *factor*.

```r
#Changing the three factor columns to factor types
rentaldata$Holiday <- factor(rentaldata$Holiday);
rentaldata$Snow    <- factor(rentaldata$Snow);
rentaldata$WeekDay <- factor(rentaldata$WeekDay);

#Visualize the dataset after the change
str(rentaldata);
```

Se mostrarán resultados similares a los siguientes.

```results
data.frame':      453 obs. of  7 variables:
$ Year       : int  2014 2014 2013 2014 2014 2015 2013 2014 2013 2015 ...
$ Month      : num  1 2 3 3 4 2 4 3 4 3 ...
$ Day        : num  20 13 10 31 24 11 28 8 5 29 ...
$ RentalCount: num  445 40 456 38 23 42 310 240 22 360 ...
$ WeekDay    : Factor w/ 7 levels "1","2","3","4",..: 2 5 1 2 5 4 1 7 6 1 ...
$ Holiday    : Factor w/ 2 levels "0","1": 2 1 1 1 1 1 1 1 1 1 ...
$ Snow       : Factor w/ 2 levels "0","1": 1 1 1 1 1 1 1 1 1 1 ...
```

Los datos están ya preparados para el entrenamiento.

## <a name="clean-up-resources"></a>Limpieza de recursos

Si no quiere continuar con este tutorial, elimine la base de datos TutorialDB del servidor.

En Azure Portal, haga lo siguiente:

1. En el menú izquierdo de Azure Portal, seleccione **Todos los recursos** o **Bases de datos SQL**.
1. En el campo **Filtrar por nombre...** , escriba **TutorialDB** y seleccione su suscripción.
1. Seleccione la base de datos TutorialDB.
1. En la página **Información general**, seleccione **Eliminar**.

## <a name="next-steps"></a>Pasos siguientes

En la parte uno de esta serie de tutoriales, ha completado estos pasos:

* Importar una base de datos de ejemplo en una base de datos de Azure SQL mediante R
* Cargar los datos de la base de datos de Azure SQL en una trama de datos de R
* Preparar los datos en R mediante la identificación de algunas columnas como de categoría

Para crear un modelo de aprendizaje automático que use datos de la base de datos TutorialDB, siga la segunda parte de esta serie de tutoriales:

> [!div class="nextstepaction"]
> [Tutorial: Creación de un modelo predictivo en R con Azure SQL Database Machine Learning Services (versión preliminar)](predictive-model-build-compare-tutorial.md)

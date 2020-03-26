---
title: 'Tutorial: Preparación de datos para realizar la agrupación en clústeres en R'
titleSuffix: Azure SQL Database Machine Learning Services (preview)
description: En la primera parte de esta serie de tutoriales de tres partes, preparará los datos de una base de datos de Azure SQL para realizar la agrupación en clústeres en R con Azure SQL Database Machine Learning Services (versión preliminar).
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
ms.date: 07/29/2019
ms.openlocfilehash: 800dbfc05c47a949bf024e9a5c671979b49ad201
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/24/2020
ms.locfileid: "68639976"
---
# <a name="tutorial-prepare-data-to-perform-clustering-in-r-with-azure-sql-database-machine-learning-services-preview"></a>Tutorial: Preparación de los datos para realizar una agrupación en clústeres en R con Azure SQL Database Machine Learning Services (versión preliminar)

En la primera parte de esta serie de tres tutoriales importará y preparará los datos de una base de datos de Azure SQL mediante R. Más adelante en la serie usará estos datos para entrenar e implementar un modelo de Machine Learning de agrupación en clústeres en R con Azure SQL Database Machine Learning Services (versión preliminar).

*Agrupar en clústeres* es organizar datos en grupos, donde los miembros de un grupo son de alguna forma similares.
Usará el algoritmo **k-means** para realizar la agrupación de clientes en clústeres en un conjunto de datos de compras y devoluciones de productos. Al agrupar los clientes en clústeres, puede centrar sus actividades de marketing de forma más eficaz al dirigirse a grupos específicos.
La agrupación en clústeres k-means es un algoritmo de *aprendizaje no supervisado* que analiza patrones en datos basándose en similitudes.

En las partes uno y dos de esta serie, va a desarrollar algunos scripts de R en RStudio para preparar los datos y entrenar un modelo de Machine Learning. Luego, en la tercera parte, ejecutará esos scripts de R en una base de datos SQL mediante procedimientos almacenados.

En este artículo, aprenderá a:

> [!div class="checklist"]
> * Importar una base de datos de ejemplo en una base de datos de Azure SQL
> * Separar clientes en diferentes dimensiones mediante R
> * Cargar los datos de la base de datos de Azure SQL en una trama de datos de R

En la [segunda parte](sql-database-tutorial-clustering-model-build.md) aprenderá a crear y entrenar un modelo de agrupación en clústeres K-Means en R.

En la [tercera parte](sql-database-tutorial-clustering-model-deploy.md) aprenderá a crear un procedimiento almacenado en una base de datos de Azure SQL que puede realizar una agrupación en clústeres en R basada en datos nuevos.

[!INCLUDE[ml-preview-note](../../includes/sql-database-ml-preview-note.md)]

## <a name="prerequisites"></a>Prerrequisitos

* Suscripción a Azure: si no tiene una suscripción a Azure, [cree una cuenta](https://azure.microsoft.com/free/) antes de empezar.

* Servidor de Azure SQL Database con Machine Learning Services habilitado: durante la versión preliminar pública, Microsoft incorporará a los usuarios y habilitará el aprendizaje automático para las bases de datos nuevas o existentes. Siga los pasos que se indican en [Suscríbase a la versión preliminar](sql-database-machine-learning-services-overview.md#signup).

* Paquete de RevoScaleR: consulte [RevoScaleR](https://docs.microsoft.com/sql/advanced-analytics/r/ref-r-revoscaler?view=sql-server-2017#versions-and-platforms) para ver las opciones de instalación de este paquete localmente.

* IDE de R: en este tutorial se usa [RStudio Desktop](https://www.rstudio.com/products/rstudio/download/).

* Herramienta de consulta SQL: en este tutorial se da por hecho que usa [Azure Data Studio](https://docs.microsoft.com/sql/azure-data-studio/what-is) o [SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/sql-server-management-studio-ssms) (SSMS).

## <a name="sign-in-to-the-azure-portal"></a>Inicio de sesión en Azure Portal

Inicie sesión en [Azure Portal](https://portal.azure.com/).

## <a name="import-the-sample-database"></a>Importación de la base de datos de ejemplo

El conjunto de datos de ejemplo usado en este tutorial se ha guardado en un archivo de copia de seguridad de base de datos **.bacpac** para que pueda descargarlo y usarlo. Este conjunto de datos se basa en el conjunto de datos [tpcx-bb](http://www.tpc.org/tpcx-bb/default.asp) proporcionado por [Transaction Processing Performance Council (TPC)](http://www.tpc.org/default.asp).

1. Descargue el archivo [tpcxbb_1gb.bacpac](https://sqlchoice.blob.core.windows.net/sqlchoice/static/tpcxbb_1gb.bacpac).

1. Siga las instrucciones que se indican en [Importación de un archivo BACPAC para crear una base de datos de Azure SQL](https://docs.microsoft.com/azure/sql-database/sql-database-import), y use estos datos:

   * Importe desde el archivo **tpcxbb_1gb.bacpac** que ha descargado.
   * Durante la versión preliminar pública, elija la configuración **Gen5/vCore** (Gen5/Núcleo virtual) para la nueva base de datos.
   * Llame a la nueva base de datos "tpcxbb_1gb".

## <a name="separate-customers"></a>Separación de clientes

Cree un archivo de RScript en RStudio y ejecute el siguiente script.
En la consulta SQL, está dividiendo a los clientes entre las dimensiones siguientes:

* **orderRatio** = índice de devolución de pedidos (número total de pedidos con una devolución total o parcial comparado con el número total de pedidos)
* **itemsRatio** = índice de artículos devueltos (número total de artículos devueltos comparado con el número de artículos comprados)
* **monetaryRatio** = índice de importes de devoluciones (total de importes monetarios de los artículos devueltos comparado con el importe de las compras)
* **frequency** = frecuencia de devolución

En la función **paste**, reemplace **Server**, **UID** y **PWD** por su propia información de conexión.

```r
# Define the connection string to connect to the tpcxbb_1gb database
connStr <- paste("Driver=SQL Server",
               "; Server=", "<Azure SQL Database Server>",
               "; Database=tpcxbb_1gb",
               "; UID=", "<user>",
               "; PWD=", "<password>",
                  sep = "");


#Define the query to select data from SQL Server
input_query <- "
SELECT ss_customer_sk AS customer
    ,round(CASE 
            WHEN (
                       (orders_count = 0)
                    OR (returns_count IS NULL)
                    OR (orders_count IS NULL)
                    OR ((returns_count / orders_count) IS NULL)
                    )
                THEN 0.0
            ELSE (cast(returns_count AS NCHAR(10)) / orders_count)
            END, 7) AS orderRatio
    ,round(CASE 
            WHEN (
                     (orders_items = 0)
                  OR (returns_items IS NULL)
                  OR (orders_items IS NULL)
                  OR ((returns_items / orders_items) IS NULL)
                 )
            THEN 0.0
            ELSE (cast(returns_items AS NCHAR(10)) / orders_items)
            END, 7) AS itemsRatio
    ,round(CASE 
            WHEN (
                     (orders_money = 0)
                  OR (returns_money IS NULL)
                  OR (orders_money IS NULL)
                  OR ((returns_money / orders_money) IS NULL)
                 )
            THEN 0.0
            ELSE (cast(returns_money AS NCHAR(10)) / orders_money)
            END, 7) AS monetaryRatio
    ,round(CASE 
            WHEN (returns_count IS NULL)
            THEN 0.0
            ELSE returns_count
            END, 0) AS frequency
FROM (
    SELECT ss_customer_sk,
        -- return order ratio
        COUNT(DISTINCT (ss_ticket_number)) AS orders_count,
        -- return ss_item_sk ratio
        COUNT(ss_item_sk) AS orders_items,
        -- return monetary amount ratio
        SUM(ss_net_paid) AS orders_money
    FROM store_sales s
    GROUP BY ss_customer_sk
    ) orders
LEFT OUTER JOIN (
    SELECT sr_customer_sk,
        -- return order ratio
        count(DISTINCT (sr_ticket_number)) AS returns_count,
        -- return ss_item_sk ratio
        COUNT(sr_item_sk) AS returns_items,
        -- return monetary amount ratio
        SUM(sr_return_amt) AS returns_money
    FROM store_returns
    GROUP BY sr_customer_sk
    ) returned ON ss_customer_sk = sr_customer_sk
"
```

## <a name="load-the-data-into-a-data-frame"></a>Carga de los datos en una trama de datos

Ahora, use el siguiente script para que se devuelvan los resultados de la consulta a una trama de datos de R mediante la función **rxSqlServerData**.
Como parte del proceso, definirá el tipo de las columnas seleccionadas (mediante colClasses) para garantizar que los tipos se transfieren correctamente a R.

```r
# Query SQL Server using input_query and get the results back
# to data frame customer_returns
# Define the types for selected columns (using colClasses),
# to make sure that the types are correctly transferred to R
customer_returns <- rxSqlServerData(
                     sqlQuery=input_query,
                     colClasses=c(customer ="numeric",
                                  orderRatio="numeric",
                                  itemsRatio="numeric",
                                  monetaryRatio="numeric",
                                  frequency="numeric" ),
                     connectionString=connStr);

# Transform the data from an input dataset to an output dataset
customer_data <- rxDataStep(customer_returns);

# Take a look at the data just loaded from SQL Server
head(customer_data, n = 5);
```

Se mostrarán resultados similares a los siguientes.

```results
  customer orderRatio itemsRatio monetaryRatio frequency
1    29727          0          0      0.000000         0
2    26429          0          0      0.041979         1
3    60053          0          0      0.065762         3
4    97643          0          0      0.037034         3
5    32549          0          0      0.031281         4
```

## <a name="clean-up-resources"></a>Limpieza de recursos

***Si no va a continuar con este tutorial***, elimine la base de datos tpcxbb_1gb del servidor de Azure SQL Database.

En Azure Portal, haga lo siguiente:

1. En el menú izquierdo de Azure Portal, seleccione **Todos los recursos** o **Bases de datos SQL**.
1. En el campo **Filtrar por nombre...** , escriba **tpcxbb_1gb** y seleccione su suscripción.
1. Seleccione la base de datos **tpcxbb_1gb**.
1. En la página **Información general**, seleccione **Eliminar**.

## <a name="next-steps"></a>Pasos siguientes

En la parte uno de esta serie de tutoriales, ha completado estos pasos:

* Importar una base de datos de ejemplo en una base de datos de Azure SQL
* Separar clientes en diferentes dimensiones mediante R
* Cargar los datos de la base de datos de Azure SQL en una trama de datos de R

Para crear un modelo de aprendizaje automático que use datos de este cliente, siga la segunda parte de esta serie de tutoriales:

> [!div class="nextstepaction"]
> [Tutorial: Creación de un modelo predictivo en R con Azure SQL Database Machine Learning Services (versión preliminar)](sql-database-tutorial-clustering-model-build.md)

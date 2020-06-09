---
title: 'Tutorial: Implementación de un modelo de agrupación en clústeres en R'
titleSuffix: Azure SQL Database Machine Learning Services (preview)
description: En la tercera parte de este tutorial de tres partes, implementará un modelo de agrupación en clústeres en R con Azure SQL Database Machine Learning Services (versión preliminar).
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
ROBOTS: NOINDEX
ms.openlocfilehash: d5227fc89d99257f3390820d4930c3d57b63f03d
ms.sourcegitcommit: 053e5e7103ab666454faf26ed51b0dfcd7661996
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/27/2020
ms.locfileid: "84024113"
---
# <a name="tutorial-deploy-a-clustering-model-in-r-with-azure-sql-database-machine-learning-services-preview"></a>Tutorial: Implementación de un modelo de agrupación en clústeres en R con Azure SQL Database Machine Learning Services (versión preliminar)
[!INCLUDE[appliesto-sqldb](../includes/appliesto-sqldb.md)]

En la tercera parte de esta serie de tres tutoriales implementará un modelo de agrupación en clústeres desarrollado en R en una base de datos SQL con Azure SQL Database Machine Learning Services (versión preliminar).

[!INCLUDE[ml-preview-note](../../../includes/sql-database-ml-preview-note.md)]

Creará un procedimiento almacenado con un script de R insertado que realiza la agrupación en clústeres. Como el modelo se ejecuta en la base de datos de Azure SQL, se puede entrenar fácilmente con los datos almacenados en la base de datos.

En este artículo, aprenderá a:

> [!div class="checklist"]
>
> * Crear un procedimiento almacenado que genere el modelo
> * Agrupación en clústeres en una base de datos SQL
> * Uso de la información de agrupación en clústeres

En la [primera parte](clustering-model-prepare-data-tutorial.md) ha aprendido a preparar los datos de una base de datos de Azure SQL para realizar la agrupación en clústeres.

En la [segunda parte](clustering-model-build-tutorial.md) aprendió a crear y entrenar un modelo de agrupación en clústeres K-Means en R.

## <a name="prerequisites"></a>Requisitos previos

* En la tercera parte de esta serie de tutoriales se da por hecho que ha completado [**la primera parte**](clustering-model-prepare-data-tutorial.md) y la [**segunda parte**](clustering-model-build-tutorial.md).

## <a name="create-a-stored-procedure-that-generates-the-model"></a>Crear un procedimiento almacenado que genere el modelo

Ejecute el siguiente script de T-SQL para crear el procedimiento almacenado. El procedimiento recrea los pasos que ha desarrollado en las partes uno y dos de esta serie de tutoriales:

* clasificación de los clientes basándose en su historial de compras devoluciones
* generación de cuatro clústeres de clientes mediante un algoritmo k-means

El procedimiento almacena las asignaciones de clústeres de clientes resultantes en la tabla de la base de datos **customer_return_clusters**.

```sql
USE [tpcxbb_1gb]
DROP PROC IF EXISTS generate_customer_return_clusters;
GO
CREATE procedure [dbo].[generate_customer_return_clusters]
AS
/*
  This procedure uses R to classify customers into different groups
  based on their purchase & return history.
*/
BEGIN
    DECLARE @duration FLOAT
    , @instance_name NVARCHAR(100) = @@SERVERNAME
    , @database_name NVARCHAR(128) = db_name()
-- Input query to generate the purchase history & return metrics
    , @input_query NVARCHAR(MAX) = N'
SELECT ss_customer_sk AS customer,
    round(CASE
            WHEN (
                    (orders_count = 0)
                    OR (returns_count IS NULL)
                    OR (orders_count IS NULL)
                    OR ((returns_count / orders_count) IS NULL)
                    )
                THEN 0.0
            ELSE (cast(returns_count AS NCHAR(10)) / orders_count)
            END, 7) AS orderRatio,
    round(CASE
            WHEN (
                    (orders_items = 0)
                    OR (returns_items IS NULL)
                    OR (orders_items IS NULL)
                    OR ((returns_items / orders_items) IS NULL)
                    )
                THEN 0.0
            ELSE (cast(returns_items AS NCHAR(10)) / orders_items)
            END, 7) AS itemsRatio,
    round(CASE
            WHEN (
                    (orders_money = 0)
                    OR (returns_money IS NULL)
                    OR (orders_money IS NULL)
                    OR ((returns_money / orders_money) IS NULL)
                    )
                THEN 0.0
            ELSE (cast(returns_money AS NCHAR(10)) / orders_money)
            END, 7) AS monetaryRatio,
    round(CASE
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
 '
EXECUTE sp_execute_external_script
      @language = N'R'
    , @script = N'
# Define the connection string
connStr <- paste("Driver=SQL Server; Server=", instance_name,
               "; Database=", database_name,
               "; Trusted_Connection=true; ",
                  sep="" );

# Input customer data that needs to be classified.
# This is the result we get from the query.
customer_returns <- RxSqlServerData(
                     sqlQuery=input_query,
                     colClasses=c(customer ="numeric",
                                  orderRatio="numeric",
                                  itemsRatio="numeric",
                                  monetaryRatio="numeric",
                                  frequency="numeric" ),
                     connectionString=connStr);
# Output table to hold the customer cluster mappings
return_cluster = RxSqlServerData(table = "customer_return_clusters",
                                 connectionString = connStr);

# Set seed for random number generator for predictability
set.seed(10);

# Generate clusters using rxKmeans and output clusters to a table
# called "customer_return_clusters"
clust <- rxKmeans( ~ orderRatio + itemsRatio + monetaryRatio + frequency,
                   customer_returns,
                   numClusters = 4,
                   outFile = return_cluster,
                   outColName = "cluster",
                   writeModelVars = TRUE ,
                   extraVarsToWrite = c("customer"),
                   overwrite = TRUE);
'
    , @input_data_1 = N''
    , @params = N'@instance_name nvarchar(100), @database_name nvarchar(128), @input_query nvarchar(max), @duration float OUTPUT'
    , @instance_name = @instance_name
    , @database_name = @database_name
    , @input_query = @input_query
    , @duration = @duration OUTPUT;
END;

GO
```

## <a name="perform-clustering-in-sql-database"></a>Agrupación en clústeres en una base de datos SQL

Ahora que ha creado el procedimiento almacenado, ejecute el siguiente script para realizar la agrupación en clústeres.

```sql
--Empty table of the results before running the stored procedure
TRUNCATE TABLE customer_return_clusters;

--Execute the clustering
--This will load the table customer_return_clusters with cluster mappings
EXECUTE [dbo].[generate_customer_return_clusters];
```

Compruebe que funciona y que realmente tenemos la lista de los clientes y sus asignaciones de clúster.

```sql
--Select data from table customer_return_clusters
--to verify that the clustering data was loaded
SELECT TOP (5) *
FROM customer_return_clusters;
```

```result
cluster  customer  orderRatio  itemsRatio  monetaryRatio  frequency
1        29727     0           0           0              0
4        26429     0           0           0.041979       1
2        60053     0           0           0.065762       3
2        97643     0           0           0.037034       3
2        32549     0           0           0.031281       4
```

## <a name="use-the-clustering-information"></a>Uso de la información de agrupación en clústeres

Como ha almacenado el procedimiento de agrupación en clústeres en la base de datos, puede realizar la agrupación en clústeres de forma eficiente con los datos de cliente almacenados en la misma base de datos. Puede ejecutar el procedimiento cada vez que se actualicen los datos de clientes y usar la información de agrupación en clústeres actualizada.

Suponga que desea enviar un correo electrónico promocional a los clientes del clúster 3, el grupo que tiene el comportamiento de devoluciones más activo (puede ver la descripción de los cuatro clústeres en la [segunda parte](clustering-model-build-tutorial.md#analyze-the-results) del tutorial). El siguiente código selecciona las direcciones de correo electrónico de los clientes del clúster 3.

```sql
USE [tpcxbb_1gb]

SELECT customer.[c_email_address],
    customer.c_customer_sk
FROM dbo.customer
JOIN [dbo].[customer_return_clusters] AS r ON r.customer = customer.c_customer_sk
WHERE r.cluster = 3
```

Puede cambiar el valor **r.cluster** para que devuelva las direcciones de correo electrónico de los clientes de otros clústeres.

## <a name="clean-up-resources"></a>Limpieza de recursos

Cuando termine este tutorial, puede eliminar la base de datos tpcxbb_1gb del servidor.

En Azure Portal, haga lo siguiente:

1. En el menú izquierdo de Azure Portal, seleccione **Todos los recursos** o **Bases de datos SQL**.
1. En el campo **Filtrar por nombre...** , escriba **tpcxbb_1gb** y seleccione su suscripción.
1. Seleccione la base de datos **tpcxbb_1gb**.
1. En la página **Información general**, seleccione **Eliminar**.

## <a name="next-steps"></a>Pasos siguientes

En la parte tres de esta serie de tutoriales, ha completado estos pasos:

* Crear un procedimiento almacenado que genere el modelo
* Agrupación en clústeres en una base de datos SQL
* Uso de la información de agrupación en clústeres

Para más información sobre el uso de R en Azure SQL Database Machine Learning Services (versión preliminar), consulte:

* [Tutorial: Preparación de los datos para entrenar un modelo predictivo en R con Azure SQL Database Machine Learning Services (versión preliminar)](predictive-model-prepare-data-tutorial.md)
* [Escribir funciones de R avanzadas en Azure SQL Database Machine Learning Services (versión preliminar)](machine-learning-services-functions.md)
* [Trabajar con datos SQL y R en Azure SQL Database Machine Learning Services (versión preliminar)](machine-learning-services-data-issues.md)
* [Incorporación de un paquete de R a Azure SQL Database Machine Learning Services (versión preliminar)](machine-learning-services-add-r-packages.md)

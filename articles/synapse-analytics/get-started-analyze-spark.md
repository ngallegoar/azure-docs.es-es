---
title: 'Inicio rápido: Introducción al análisis con Spark'
description: En este tutorial aprenderá a analizar datos con Apache Spark
services: synapse-analytics
author: saveenr
ms.author: saveenr
manager: julieMSFT
ms.reviewer: jrasnick
ms.service: synapse-analytics
ms.subservice: spark
ms.topic: tutorial
ms.date: 07/20/2020
ms.openlocfilehash: 07537e26b169414e3f8ec35cc32945c20f7eb7ce
ms.sourcegitcommit: 0a9df8ec14ab332d939b49f7b72dea217c8b3e1e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/18/2020
ms.locfileid: "94843288"
---
# <a name="analyze-with-apache-spark"></a>Análisis con Apache Spark

## <a name="analyze-nyc-taxi-data-in-blob-storage-using-spark"></a>Análisis de los datos de taxis de Nueva York en Blob Storage con Spark

En este tutorial, aprenderá los pasos básicos para cargar y analizar datos con Apache Spark para Azure Synapse.

1. En el centro de conectividad **Data** (Datos), haga clic en **Add a new resource** (Agregar un recurso) (botón con el signo más encima de **Linked** [Vinculados]) >> **Browse Samples** (Examinar ejemplos). 
1. Busque el conjunto de datos **NYC Taxi & Limousine Commission - yellow taxi trip records** y haga clic en él. 
1. En la parte inferior de la página, presione **Continue** (Continuar) y **Add dataset** (Agregar conjunto de datos). 
1. Ahora, en el centro de conectividad **Data** (Datos) de **Linked** (Vinculados), haga clic con el botón derecho en **Azure Blob Storage >> Sample Datasets >> nyc_tlc_yellow** (Azure Blob Storage >> Conjuntos de datos de ejemplo >> nyc_tlc_yellow) y seleccione **New notebook** (Nuevo cuaderno).
1. Así se creará un cuaderno con el código siguiente:
    ```
    from azureml.opendatasets import NycTlcYellow

    data = NycTlcYellow()
    data_df = data.to_spark_dataframe()
    display(data_df.limit(10))
    ```
1. En el cuaderno, elija un grupo de Spark sin servidor del menú **Adjuntar a**.
1. Seleccione **Ejecutar** en la celda.
1. Si solo desea ver el esquema de la trama de datos, ejecute una celda con el siguiente código:
    ```
    data_df.printSchema()
    ```

## <a name="load-the-nyc-taxi-data-into-the-spark-nyctaxi-database"></a>Carga de los datos de los taxis de Nueva York en la base de datos nyctaxi de Spark

Los datos están disponibles en una tabla en **SQLPOOL1**. Cárguelos en una base de datos de Spark denominada **nyctaxi**.

1. En Synapse Studio, vaya al centro **Develop** (Desarrollo).
1. Seleccione **+**  > **Cuaderno**.
1. En la parte superior del cuaderno, establezca el valor de **Adjuntar a** en **Spark1**.
1. Seleccione **Agregar código** para agregar una celda de código del cuaderno y, a continuación, pegue el siguiente texto:

    ```scala
    %%spark
    spark.sql("CREATE DATABASE IF NOT EXISTS nyctaxi")
    val df = spark.read.sqlanalytics("SQLPOOL1.dbo.Trip") 
    df.write.mode("overwrite").saveAsTable("nyctaxi.trip")
    ```

1. Vaya al centro **Data** (Datos), haga clic con el botón derecho en **Databases** (Bases de datos) y seleccione **Refresh** (Actualizar). Debería ver estas bases de datos:

    - **SQLPOOL1 (SQL)**
    - **nyctaxi (Spark)**

## <a name="analyze-the-nyc-taxi-data-using-spark-and-notebooks"></a>Análisis de los datos de los taxis de Nueva York mediante Spark y cuadernos

1. Vuelva al cuaderno.
1. Cree una nueva celda de código y escriba el texto siguiente. A continuación, ejecute la celda para que se muestren los datos de taxis de Nueva York que cargamos en la base de datos **nyctaxi** de Spark.

   ```py
   %%pyspark
   df = spark.sql("SELECT * FROM nyctaxi.trip") 
   display(df)
   ```

1. Ejecute el código siguiente para realizar el mismo análisis que hicimos anteriormente con el grupo de SQL dedicado **SQLPOOL1**. Este código guarda los resultados del análisis en una tabla denominada **nyctaxi.passengercountstats** y visualiza los resultados.

   ```py
   %%pyspark
   df = spark.sql("""
      SELECT PassengerCount,
          SUM(TripDistanceMiles) as SumTripDistance,
          AVG(TripDistanceMiles) as AvgTripDistance
      FROM nyctaxi.trip
      WHERE TripDistanceMiles > 0 AND PassengerCount > 0
      GROUP BY PassengerCount
      ORDER BY PassengerCount
   """) 
   display(df)
   df.write.saveAsTable("nyctaxi.passengercountstats")
   ```

1. En los resultados de la celda, seleccione **Chart** (Gráfico) para ver los datos visualizados.

## <a name="customize-data-visualization-with-spark-and-notebooks"></a>Personalización de la visualización de datos con Spark y cuadernos

Puede controlar cómo se representan los gráficos mediante los cuadernos. El siguiente código muestra un ejemplo sencillo. Usa las conocidas bibliotecas **matplotlib** y **seaborn**. El código representa el mismo tipo de gráfico de líneas que las consultas SQL que se ejecutaron anteriormente.

```py
%%pyspark
import matplotlib.pyplot
import seaborn

seaborn.set(style = "whitegrid")
df = spark.sql("SELECT * FROM nyctaxi.passengercountstats")
df = df.toPandas()
seaborn.lineplot(x="PassengerCount", y="SumTripDistance" , data = df)
seaborn.lineplot(x="PassengerCount", y="AvgTripDistance" , data = df)
matplotlib.pyplot.show()
```



## <a name="load-data-from-a-spark-table-into-a-dedicated-sql-pool-table"></a>Carga de datos de una tabla de Spark en una tabla de grupos de SQL dedicado

Anteriormente copiamos datos de la tabla de grupos de SQL dedicados **SQLPOOL1.dbo.Trip** en la tabla de Spark **nyctaxi.trip**. Luego, mediante Spark, agregamos los datos a la tabla de Spark **nyctaxi.passengercountstats**. Ahora se copiarán los datos de **nyctaxi.passengercountstats** en una tabla de grupos de SQL dedicados denominada **SQLPOOL1.dbo.PassengerCountStats**.

Ejecute la siguiente celda en el cuaderno. Esta copia la tabla de Spark agregada en la tabla de grupos de SQL dedicados.

```scala
%%spark
val df = spark.sql("SELECT * FROM nyctaxi.passengercountstats")
df.write.sqlanalytics("SQLPOOL1.dbo.PassengerCountStats", Constants.INTERNAL )
```

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Análisis de datos con un grupo de SQL sin servidor](get-started-analyze-sql-on-demand.md)



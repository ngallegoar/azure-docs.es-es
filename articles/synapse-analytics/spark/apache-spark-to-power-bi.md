---
title: Cuadernos de Azure Synapse Studio
description: En este tutorial, encontrará información general acerca de cómo crear un panel de Power BI utilizando Apache Spark y un grupo de SQL sin servidor.
services: synapse-analytics
author: midesa
ms.author: midesa
ms.reviewer: jrasnick
ms.service: synapse-analytics
ms.subservice: spark
ms.topic: tutorial
ms.date: 11/16/2020
ms.openlocfilehash: ea8fcb602f49dec61187260e08d3ccd1b148cee8
ms.sourcegitcommit: a43a59e44c14d349d597c3d2fd2bc779989c71d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/25/2020
ms.locfileid: "95918958"
---
# <a name="tutorial-create-a-power-bi-report-using-apache-spark-and-azure-synapse-analytics"></a>Tutorial: Creación de un informe de Power BI con Apache Spark y Azure Synapse Analytics

Es habitual que las organizaciones necesiten procesar grandes volúmenes de datos para poder atender a las partes interesadas de la empresa. En este tutorial, aprenderá a utilizar las experiencias integradas en Azure Synapse Analytics para procesar datos mediante Apache Spark y proporcionar después los datos a los usuarios finales mediante Power BI y SQL sin servidor.

## <a name="before-you-begin"></a>Antes de empezar
- Necesitará un [área de trabajo de Synapse Analytics](../quickstart-create-workspace.md) con una cuenta de almacenamiento de ADLS Gen2 configurada como almacenamiento predeterminado. 
- Necesitará un área de trabajo de Power BI y Power BI Desktop para visualizar los datos. Para más información, consulte estos artículos sobre la [creación de un área de trabajo de Power BI](https://docs.microsoft.com/power-bi/service-create-the-new-workspaces) y la [instalación de Power BI Desktop](https://powerbi.microsoft.com/downloads/)
- Necesitará un servicio vinculado para conectar las áreas de trabajo de Power BI y Azure Synapse Analytics. Para más información, consulte este artículo sobre la [vinculación a un área de trabajo de Power BI](../quickstart-power-bi.md).
- Necesitará un grupo de Apache Spark sin servidor en el área de trabajo de Synapse Analytics. Para más información, consulte este artículo sobre la [creación de un grupo de Apache Spark sin servidor.](../quickstart-create-apache-spark-pool-studio.md)
  
## <a name="download-and-prepare-the-data"></a>Descarga y preparación de los datos
En este ejemplo, va a utilizar Spark para realizar algunos análisis de los datos sobre las propinas de los trayectos en taxi de Nueva York. Los datos están disponibles a través de [Azure Open Datasets](https://azure.microsoft.com/services/open-datasets/catalog/nyc-taxi-limousine-commission-yellow-taxi-trip-records/). Este subconjunto del conjunto de datos contiene información sobre las carreras de Yellow Taxi, incluida información sobre cada carrera, la hora y la ubicación de inicio y fin, el costo y otros atributos interesantes.

1. Ejecute las líneas siguientes para crear un dataframe de Spark pegando el código en una nueva celda. Se recuperan los datos a través de Open Datasets API. La extracción de todos estos datos genera aproximadamente 1500 millones de filas. En el ejemplo de código siguiente se usa start_date y end_date para aplicar un filtro que devuelve un mes único de datos.
   
   ```python
    from azureml.opendatasets import NycTlcYellow
    from dateutil import parser
    from datetime import datetime

    end_date = parser.parse('2018-06-06')
    start_date = parser.parse('2018-05-01')
    nyc_tlc = NycTlcYellow(start_date=start_date, end_date=end_date)
    filtered_df = nyc_tlc.to_spark_dataframe()
   ```
2. Vamos a utilizar Apache Spark SQL para crear una base de datos llamada NycTlcTutorial. Emplearemos esta base de datos para almacenar los resultados del procesamiento de datos.
   ```python
   %%pyspark
    spark.sql("CREATE DATABASE IF NOT EXISTS NycTlcTutorial")
   ```
3. A continuación, usaremos las operaciones de trama de datos de Spark para procesar los datos. En el código siguiente, vamos a realizar estas transformaciones:
   1. Eliminación de las columnas que no son necesarias.
   2. La eliminación de valores atípicos/valores incorrectos a través del filtrado.
   3. Creación de nuevas características, como ```tripTimeSecs``` y ```tipped```, para realizar otros análisis.
    ```python
    from pyspark.sql.functions import unix_timestamp, date_format, col, when

    taxi_df = filtered_df.select('totalAmount', 'fareAmount', 'tipAmount', 'paymentType', 'rateCodeId', 'passengerCount'\
                                    , 'tripDistance', 'tpepPickupDateTime', 'tpepDropoffDateTime'\
                                    , date_format('tpepPickupDateTime', 'hh').alias('pickupHour')\
                                    , date_format('tpepPickupDateTime', 'EEEE').alias('weekdayString')\
                                    , (unix_timestamp(col('tpepDropoffDateTime')) - unix_timestamp(col('tpepPickupDateTime'))).alias('tripTimeSecs')\
                                    , (when(col('tipAmount') > 0, 1).otherwise(0)).alias('tipped')
                                    )\
                            .filter((filtered_df.passengerCount > 0) & (filtered_df.passengerCount < 8)\
                                    & (filtered_df.tipAmount >= 0) & (filtered_df.tipAmount <= 25)\
                                    & (filtered_df.fareAmount >= 1) & (filtered_df.fareAmount <= 250)\
                                    & (filtered_df.tipAmount < filtered_df.fareAmount)\
                                    & (filtered_df.tripDistance > 0) & (filtered_df.tripDistance <= 100)\
                                    & (filtered_df.rateCodeId <= 5)
                                    & (filtered_df.paymentType.isin({"1", "2"})))
    ```
4. Por último, guardaremos la trama de datos utilizando el método ```saveAsTable``` de Apache Spark. Esto le permitirá realizar consultas posteriormente y conectarse a la misma tabla utilizando grupos de SQL sin servidor.
   
## <a name="query-data-using-serverless-sql-pools"></a>Consulta de datos mediante grupos de SQL sin servidor
Azure Synapse Analytics permite que los diferentes motores de las áreas de trabajo de cálculo compartan bases de datos y tablas entre sus grupos de Apache Spark sin servidor (versión preliminar) y el grupo de SQL sin servidor (versión preliminar). Esto se realiza mediante la funcionalidad de [administración de metadatos compartidos](../metadata/overview.md) de Synapse. Como resultado, las bases de datos creadas por Spark y las tablas con formato Parquet se vuelven visibles en el grupo de SQL sin servidor del área de trabajo.

Para consultar la tabla de Apache Spark con el grupo de SQL sin servidor:
   1. Una vez que haya guardado la tabla de Apache Spark, cambie a la pestaña **data** (datos).
   
   2. En **Workspaces** (Áreas de trabajo), busque la tabla de Apache Spark que acaba de crear y seleccione **New SQL script** (Nuevo script SQL) y **Select TOP 100 rows** (Seleccionar 100 primeras filas). 
      
      :::image type="content" source="../spark/media/apache-spark-power-bi/query-spark-table-with-sql.png" alt-text="Consulta SQL." border="true":::

   3. Puede restringir más la consulta o incluso visualizar los resultados utilizando las opciones de gráficos de SQL.

## <a name="connect-to-power-bi"></a>Conexión a Power BI
A continuación, vamos a conectar el grupo de SQL sin servidor al área de trabajo de Power BI. Una vez conectada el área de trabajo, podrá crear informes de Power BI directamente desde Azure Synapse Analytics o desde Power BI Desktop.

>[!Note]
> Para poder empezar, necesitará configurar un servicio vinculado al [área de trabajo de Power BI](../quickstart-power-bi.md) y descargar [Power BI Desktop](https://docs.microsoft.com/power-bi/service-create-the-new-workspaces).  

Para conectar el grupo de SQL sin servidor al área de trabajo de Power BI:

1.  Vaya a la pestaña **Conjuntos de datos de Power BI** y seleccione la opción **+ Nuevo conjunto de datos**. En el símbolo del sistema, descargue el archivo .pbids de la base de datos de SQL Analytics que desee usar como origen de datos. 
   :::image type="content" source="../spark/media/apache-spark-power-bi/power-bi-datasets.png" alt-text="Conjuntos de datos de Power BI." border="true":::

2.  Abra el archivo con Power BI Desktop para crear un conjunto de archivos. Una vez abierto, conéctese a la base de datos de SQL Server utilizando la **cuenta de Microsoft** y la opción **Importar**. 
   

## <a name="create-a-power-bi-report"></a>Crear un informe de Power BI
1. En Power BI Desktop, ahora puede agregar un gráfico de **elementos influyentes clave**.
   
   1. Arrastre la columna *tipAmount* con el valor promedio al eje **Analizar**.
   
   2. Arrastre las columnas **weekdayString**, **tripDistance** y **tripTimeSecs** con los valores promedios al eje **Explicar por**. 
   
   :::image type="content" source="../spark/media/apache-spark-power-bi/power-bi-desktop.png" alt-text="Power BI Desktop." border="true":::

2. En la pestaña Inicio de Power BI Desktop, seleccione **Publicar** y **Guardar**. Escriba un nombre de archivo y guarde este informe en el *área de trabajo NycTaxiTutorial*.
   
3. También puede crear visualizaciones de Power BI en el área de trabajo de Azure Synapse Analytics. Para ello, vaya a la pestaña **Develop** (Desarrollo) del área de trabajo de Azure Synapse y abra la pestaña de Power BI. Desde aquí, podrá seleccionar el informe y generar otras visualizaciones. 
   
   :::image type="content" source="../spark/media/apache-spark-power-bi/power-bi-synapse.png" alt-text="Área de trabajo de Azure Synapse Analytics." border="true":::

Para más información acerca de cómo crear un conjunto de datos con SQL sin servidor y conectarse a Power BI, puede visitar este tutorial acerca de [cómo conectarse a Power BI Desktop](../../synapse-analytics/sql/tutorial-connect-power-bi-desktop.md).

## <a name="next-steps"></a>Pasos siguientes
Para más información sobre las funcionalidades de visualización de datos de Azure Synapse Analytics, visite los siguientes documentos y tutoriales:
   - [Visualización de datos con grupos de Apache Spark sin servidor](../spark/apache-spark-data-visualization-tutorial.md)
   - [Información general sobre la visualización de datos con grupos de Apache Spark](../spark/apache-spark-data-visualization.md)
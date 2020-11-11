---
title: 'Exploración de datos en un clúster de Hadoop: proceso de ciencia de datos en equipos'
description: Uso del proceso de ciencia de datos en equipos para un escenario completo que emplea un clúster de Hadoop de HDInsight con el objetivo de compilar e implementar un modelo.
services: machine-learning
author: marktab
manager: marktab
editor: marktab
ms.service: machine-learning
ms.subservice: team-data-science-process
ms.topic: article
ms.date: 01/10/2020
ms.author: tdsp
ms.custom: seodec18, previous-author=deguhath, previous-ms.author=deguhath
ms.openlocfilehash: 53f50e98bcec4b8ace342808f0bcfd96770834b0
ms.sourcegitcommit: 96918333d87f4029d4d6af7ac44635c833abb3da
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/04/2020
ms.locfileid: "93312338"
---
# <a name="the-team-data-science-process-in-action-use-azure-hdinsight-hadoop-clusters"></a>Proceso de ciencia de datos en equipos en acción: Uso de clústeres de Azure HDInsight Hadoop
En este tutorial, empleamos el [proceso de ciencia de datos en equipo](overview.md) en un escenario completo. Utilizamos un [clúster de Hadoop para Azure HDInsight](https://azure.microsoft.com/services/hdinsight/) para almacenar, explorar y diseñar características de los datos del conjunto de datos de [NYC Taxi Trips](https://www.andresmh.com/nyctaxitrips/) disponible públicamente, así como para reducir el muestreo de los datos. Para controlar las tareas predictivas de clasificación binaria y de clases múltiples, así como las de regresión, se generan modelos de datos con Azure Machine Learning. 

Para acceder a un tutorial donde se muestra cómo administrar un conjunto de datos mayor, consulte [Proceso de ciencia de datos en equipos en acción: Uso de un clúster de Hadoop de Azure HDInsight en un conjunto de datos de 1 TB](hive-criteo-walkthrough.md).

También puede usar un cuaderno de IPython para realizar las tareas que se presentan en el tutorial que emplea el conjunto de datos de 1 TB. Para obtener más información, consulte [Criteo walkthrough using a Hive ODBC connection](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/iPythonNotebooks/machine-Learning-data-science-process-hive-walkthrough-criteo.ipynb) (Tutorial de Criteo con una conexión de ODBC de Hive).

## <a name="nyc-taxi-trips-dataset-description"></a><a name="dataset"></a>Descripción del conjunto de datos NYC Taxi Trips
Los datos de NYC Taxi Trip son aproximadamente 20 GB de archivos de valores separados por comas (CSV) comprimidos (unos 48 GB descomprimidos). Están compuestos por más de 173 millones de carreras e incluyen las tarifas que se pagaron para cada una de ellas. Cada registro de carrera incluye la hora y los puntos de recogida y destino, el número de licencia anonimizado del conductor y el número de identificación del taxi. Los datos cubren todos los viajes del año 2013 y, para cada mes, se proporcionan en los dos conjuntos de datos siguientes:

- Los archivos CSV trip_data contienen información detallada de las carreras: el número de pasajeros, los puntos de recogida y destino, la duración de las carreras y la longitud del recorrido. Estos son algunos registros de ejemplo:

  `medallion,hack_license,vendor_id,rate_code,store_and_fwd_flag,pickup_datetime,dropoff_datetime,passenger_count,trip_time_in_secs,trip_distance,pickup_longitude,pickup_latitude,dropoff_longitude,dropoff_latitude`

  `89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,1,N,2013-01-01 15:11:48,2013-01-01 15:18:10,4,382,1.00,-73.978165,40.757977,-73.989838,40.751171`

  `0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-06 00:18:35,2013-01-06 00:22:54,1,259,1.50,-74.006683,40.731781,-73.994499,40.75066`

  `0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-05 18:49:41,2013-01-05 18:54:23,1,282,1.10,-74.004707,40.73777,-74.009834,40.726002`

  `DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:54:15,2013-01-07 23:58:20,2,244,.70,-73.974602,40.759945,-73.984734,40.759388`

  `DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:25:03,2013-01-07 23:34:24,1,560,2.10,-73.97625,40.748528,-74.002586,40.747868`

- Los archivos CSV trip_fare contienen información detallada de la tarifa pagada en cada carrera: tipo de pago, importe de la tarifa, suplementos e impuestos, propinas y peajes, y el importe total pagado. Estos son algunos registros de ejemplo:

  `medallion, hack_license, vendor_id, pickup_datetime, payment_type, fare_amount, surcharge, mta_tax, tip_amount, tolls_amount, total_amount`

  `89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,2013-01-01 15:11:48,CSH,6.5,0,0.5,0,0,7`

  `0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-06 00:18:35,CSH,6,0.5,0.5,0,0,7`

  `0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-05 18:49:41,CSH,5.5,1,0.5,0,0,7`

  `DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:54:15,CSH,5,0.5,0.5,0,0,6`

  `DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:25:03,CSH,9.5,0.5,0.5,0,0,10.5`

La clave única para unir trip\_data and trip\_fare se compone de los campos: medallion, hack\_license and pickup\_datetime. Para obtener todos los detalles correspondientes a una carrera concreta, es suficiente con combinar estas tres claves.

## <a name="examples-of-prediction-tasks"></a><a name="mltasks"></a>Ejemplos de tareas de predicción
Determine el tipo de predicciones que quiere realizar según el análisis de datos para ayudar a clarificar las tareas de proceso necesarias. Estos son tres ejemplos de problemas de predicción que se abordan en este tutorial, todos basados en el importe de la propina, *tip\_amount* :

- **Clasificación binaria** : permite predecir si se dio propina en una carrera o no. Es decir, un *importe\_ de propina* que sea mayor que 0 $ es un ejemplo positivo, mientras que un *importe\_ de propina* de 0 $ es un ejemplo negativo.

  - Clase 0: tip_amount = 0 $
  - Clase 1: tip_amount > $0

- **Clasificación multiclase** : permite predecir el rango de importes de propina pagados por la carrera. Dividimos *tip\_amount* en cinco clases:

  - Clase 0: tip_amount = 0 $
  - Clase 1: tip_amount > 0 $ y tip_amount < = 5 $
  - Clase 2: tip_amount > 5 $ y tip_amount < = 10 $
  - Clase 3: tip_amount > 10 $ y tip_amount < = 20 $
  - Clase 4: tip_amount > 20 $

- **Tarea de regresión** : permite predecir el importe de la propina pagada por una carrera.  

## <a name="set-up-an-hdinsight-hadoop-cluster-for-advanced-analytics"></a><a name="setup"></a>Configuración de un clúster de Hadoop de HDInsight para el análisis avanzado
> [!NOTE]
> Esta tarea la suelen hacer los administradores.
> 
> 

Puede configurar un entorno de Azure para análisis avanzado que emplee un clúster de HDInsight en tres pasos:

1. [Creación de una cuenta de almacenamiento](../../storage/common/storage-account-create.md): esta cuenta de almacenamiento se utiliza para almacenar datos en Azure Blob Storage. Los datos utilizados en los clústeres de HDInsight también se encuentran aquí.
2. [Personalice los clústeres de Hadoop de HDInsight de Azure para la tecnología y procesos de análisis avanzado](../../hdinsight/spark/apache-spark-jupyter-spark-sql.md). Este paso crea un clúster de Hadoop de HDInsight con Anaconda Python 2.7 de 64 bits instalado en todos los nodos. Hay dos pasos importantes que debe recordar al personalizar el clúster de HDInsight.
   
   * Recuerde vincular la cuenta de almacenamiento que creó en el paso 1 con el clúster de HDInsight en el momento de crearlo. Esta cuenta de almacenamiento tiene acceso a los datos que se procesan en el clúster.
   * Después de crear el clúster, debe habilitar el acceso remoto a su nodo principal. Navegue hasta la pestaña **Configuración** y haga clic en **Habilitar de forma remota**. Este paso especifica las credenciales de usuario usadas para el inicio de sesión remoto.
3. [Creación de un área de trabajo de Azure Machine Learning](../classic/create-workspace.md): use esta área de trabajo para compilar modelos de Machine Learning. Esta tarea se lleva a cabo después de completar una exploración inicial de los datos y de reducir su tamaño con el clúster de HDInsight.

## <a name="get-the-data-from-a-public-source"></a><a name="getdata"></a>Obtención de los datos desde un origen público
> [!NOTE]
> Esta tarea la suelen hacer los administradores.
> 
> 

Para copiar el conjunto de datos [NYC Taxi Trips](https://www.andresmh.com/nyctaxitrips/) en su equipo desde una ubicación pública, use cualquiera de los métodos descritos en [Mover datos hacia y desde Azure Blob Storage](move-azure-blob.md).

Aquí se describe cómo utilizar AzCopy para transferir los archivos que contienen datos. Para descargar e instalar AzCopy, siga las indicaciones de [Introducción a la utilidad de línea de comandos AzCopy](../../storage/common/storage-use-azcopy-v10.md).

1. Desde una ventana de símbolo del sistema, ejecute los siguientes comandos de AzCopy, donde debe reemplazar *\<path_to_data_folder>* por el destino deseado:

    ```console
    "C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy\azcopy" /Source:https://nyctaxitrips.blob.core.windows.net/data /Dest:<path_to_data_folder> /S
    ```

1. Cuando se haya completado la copia, verá un total de 24 archivos comprimidos en la carpeta de datos elegida. Descomprima los archivos descargados en el mismo directorio del equipo local. Tome nota de la carpeta donde se encuentran los archivos sin comprimir. En lo sucesivo, esta carpeta se conoce como *\<path\_to\_unzipped_data\_files\>* .

## <a name="upload-the-data-to-the-default-container-of-the-hdinsight-hadoop-cluster"></a><a name="upload"></a>Carga de los datos en el contenedor predeterminado del clúster de Hadoop de HDInsight.
> [!NOTE]
> Esta tarea la suelen hacer los administradores.
> 
> 

En los siguientes comandos de AzCopy, reemplace los siguientes parámetros con los valores reales que se especificó al crear el clúster de Hadoop y descomprimir los archivos de datos.

* * **\<path_to_data_folder>** _ el directorio (junto con la ruta de acceso) de la máquina que contiene los archivos de datos sin comprimir.  
_* **\<storage account name of Hadoop cluster>** : cuenta de almacenamiento asociada al clúster de HDInsight.
_* **\<default container of Hadoop cluster>** _: contenedor predeterminado utilizado por el clúster. El nombre del contenedor predeterminado suele ser el mismo que el del propio clúster. Por ejemplo, si el clúster se llama "abc123.azurehdinsight.net", el contenedor predeterminado es abc123.
_* **\<storage account key>** _: clave para la cuenta de almacenamiento usada por el clúster.

Desde un símbolo del sistema o una ventana de Windows PowerShell, ejecute los dos comandos siguientes de AzCopy.

Este comando permite cargar los datos de carrera en el directorio _*_nyctaxitripraw_*_ del contenedor predeterminado del clúster de Hadoop.

```console
"C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy\azcopy" /Source:<path_to_unzipped_data_files> /Dest:https://<storage account name of Hadoop cluster>.blob.core.windows.net/<default container of Hadoop cluster>/nyctaxitripraw /DestKey:<storage account key> /S /Pattern:trip_data__.csv
```

Este comando permite cargar los datos de tarifas en el directorio * **nyctaxifareraw** _ del contenedor predeterminado del clúster de Hadoop.

```console
"C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy\azcopy" /Source:<path_to_unzipped_data_files> /Dest:https://<storage account name of Hadoop cluster>.blob.core.windows.net/<default container of Hadoop cluster>/nyctaxifareraw /DestKey:<storage account key> /S /Pattern:trip_fare__.csv
```

Los datos deben estar ahora en Azure Blob Storage, listos para usarse dentro del clúster de HDInsight.

## <a name="sign-in-to-the-head-node-of-hadoop-cluster-and-prepare-for-exploratory-data-analysis"></a><a name="#download-hql-files"></a>Inicio de sesión en el nodo principal del clúster de Hadoop y preparación para el análisis de exploración de datos
> [!NOTE]
> Esta tarea la suelen hacer los administradores.
> 
> 

Para tener acceso al nodo principal del clúster para el análisis de exploración de datos y la reducción de estos, siga el procedimiento descrito en [Acceso al nodo principal del clúster de Hadoop](../../hdinsight/spark/apache-spark-jupyter-spark-sql.md).

En este tutorial se usan principalmente las consultas escritas en [Hive](https://hive.apache.org/), un lenguaje de consultas de tipo SQL, para realizar exploraciones preliminares de los datos. Las consultas de Hive se almacenan en archivos ".hql". A continuación, se reducen estos datos para su uso en Machine Learning con el fin de generar modelos.

Para preparar el clúster para el análisis de exploración de datos, descargue los archivos ".hql" que contienen los scripts de Hive pertinentes desde [GitHub](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/DataScienceScripts) en un directorio local (C:\temp) del nodo principal. Abra el símbolo del sistema desde el nodo principal del clúster y ejecute los dos comandos siguientes:

```console
set script='https://raw.githubusercontent.com/Azure/Azure-MachineLearning-DataScience/master/Misc/DataScienceProcess/DataScienceScripts/Download_DataScience_Scripts.ps1'

@powershell -NoProfile -ExecutionPolicy unrestricted -Command "iex ((new-object net.webclient).DownloadString(%script%))"
```

Estos dos comandos descargan todos los archivos ".hql" necesarios en este tutorial en el directorio local * **C:\temp&#92;** _ del nodo principal.

## <a name="create-hive-database-and-tables-partitioned-by-month"></a><a name="#hive-db-tables"></a>Creación de base de datos y tablas de Hive con particiones por mes
> [!NOTE]
> Esta tarea suelen realizarla los administradores.
> 
> 

Ya está listo para crear tablas de Hive para el conjunto de datos de taxis de Nueva York.
En el nodo principal del clúster de Hadoop, abra la línea de comandos de Hadoop en el escritorio del nodo principal. Ejecute el comando siguiente para entrar en el directorio de Hive:

```console
cd %hive_home%\bin
```

> [!NOTE]
> Ejecute todos los comandos de Hive que aparecen en este tutorial desde el símbolo del sistema del directorio bin/ de Hive. De esta manera, cualquier problema con la ruta de acceso se soluciona automáticamente. En este tutorial se utilizan indistintamente los términos "símbolo del sistema del directorio de Hive", "símbolo del sistema del directorio bin/ de Hive" y "línea de comandos de Hadoop".
> 
> 

Desde el símbolo del sistema del directorio de Hive, ejecute el siguiente comando en la línea de comandos de Hadoop del nodo principal que crea la base de datos y las tablas de Hive:

```console
hive -f "C:\temp\sample_hive_create_db_and_tables.hql"
```

Este es el contenido del archivo _ *C:\temp\sample\_hive\_create\_db\_and\_tables.hql* * que crea la base de datos de Hive **nyctaxidb** y las tablas **trip** y **fare**.

```hiveql
create database if not exists nyctaxidb;

create external table if not exists nyctaxidb.trip
(
    medallion string,
    hack_license string,
    vendor_id string,
    rate_code string,
    store_and_fwd_flag string,
    pickup_datetime string,
    dropoff_datetime string,
    passenger_count int,
    trip_time_in_secs double,
    trip_distance double,
    pickup_longitude double,
    pickup_latitude double,
    dropoff_longitude double,
    dropoff_latitude double)  
PARTITIONED BY (month int)
ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' lines terminated by '\n'
STORED AS TEXTFILE LOCATION 'wasb:///nyctaxidbdata/trip' TBLPROPERTIES('skip.header.line.count'='1');

create external table if not exists nyctaxidb.fare
(
    medallion string,
    hack_license string,
    vendor_id string,
    pickup_datetime string,
    payment_type string,
    fare_amount double,
    surcharge double,
    mta_tax double,
    tip_amount double,
    tolls_amount double,
    total_amount double)
PARTITIONED BY (month int)
ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' lines terminated by '\n'
STORED AS TEXTFILE LOCATION 'wasb:///nyctaxidbdata/fare' TBLPROPERTIES('skip.header.line.count'='1');
```

Este script de Hive crea dos tablas:

* La tabla **trip** contiene detalles del recorrido de cada carrera (detalles del conductor, hora de recogida, distancia de viaje y horas).
* La tabla **fare** contiene los detalles de tarifa (importe de tarifa, propina, peajes y suplementos).

Si necesita ayuda adicional con estos procedimientos o bien si desea investigar otras alternativas, consulte la sección [Enviar consultas de Hive directamente desde la línea de comandos de Hadoop ](move-hive-tables.md#submit).

## <a name="load-data-to-hive-tables-by-partitions"></a><a name="#load-data"></a>Carga de datos en tablas de Hive por particiones
> [!NOTE]
> Esta tarea suelen realizarla los administradores.
> 
> 

El conjunto de datos de taxis de Nueva York tiene una partición natural por mes, que usamos para conseguir tiempos de procesamiento y consulta más rápidos. Los siguientes comandos de PowerShell (emitidos desde el directorio de Hive mediante la línea de comandos de Hadoop) cargan datos en las tablas "trip" y "fare" de Hive desglosadas por meses.

```powershell
for /L %i IN (1,1,12) DO (hive -hiveconf MONTH=%i -f "C:\temp\sample_hive_load_data_by_partitions.hql")
```

El archivo **sample\_hive\_load\_data\_by\_partitions.hql** contiene los siguientes comandos **LOAD** :

```hiveql
LOAD DATA INPATH 'wasb:///nyctaxitripraw/trip_data_${hiveconf:MONTH}.csv' INTO TABLE nyctaxidb.trip PARTITION (month=${hiveconf:MONTH});
LOAD DATA INPATH 'wasb:///nyctaxifareraw/trip_fare_${hiveconf:MONTH}.csv' INTO TABLE nyctaxidb.fare PARTITION (month=${hiveconf:MONTH});
```

Algunas de las consultas de Hive usadas aquí en el proceso de exploración implican examinar solo una o dos particiones. Sin embargo, estas consultas se pueden ejecutar en todo el conjunto de datos.

### <a name="show-databases-in-the-hdinsight-hadoop-cluster"></a><a name="#show-db"></a>Visualización de bases de datos en el clúster de Hadoop de HDInsight
Para mostrar las bases de datos creadas en el clúster de Hadoop de HDInsight dentro de la ventana de la línea de comandos de Hadoop, ejecute el siguiente comando en la línea de comandos de Hadoop:

```console
hive -e "show databases;"
```

### <a name="show-the-hive-tables-in-the-nyctaxidb-database"></a><a name="#show-tables"></a>Visualización de las tablas de Hive en la base de datos **nyctaxidb**
Para mostrar las tablas de la base de datos **nyctaxidb** , ejecute el siguiente comando en la línea de comandos de Hadoop:

```console
hive -e "show tables in nyctaxidb;"
```

Para confirmar que las tablas están desglosadas, se puede ejecutar el comando siguiente:

```console
hive -e "show partitions nyctaxidb.trip;"
```

Este es el resultado que se espera:

```output
month=1
month=10
month=11
month=12
month=2
month=3
month=4
month=5
month=6
month=7
month=8
month=9
Time taken: 2.075 seconds, Fetched: 12 row(s)
```

Del mismo modo, para confirmar que la tabla fare está desglosada, se puede ejecutar el comando siguiente:

```console
hive -e "show partitions nyctaxidb.fare;"
```

Este es el resultado que se espera:

```output
month=1
month=10
month=11
month=12
month=2
month=3
month=4
month=5
month=6
month=7
month=8
month=9
Time taken: 1.887 seconds, Fetched: 12 row(s)
```

## <a name="data-exploration-and-feature-engineering-in-hive"></a><a name="#explore-hive"></a>Exploración de datos e ingeniería de características en Hive
> [!NOTE]
> Esta tarea la suelen hacer los científicos de datos.
> 
> 

Puede usar las consultas de Hive para realizar tareas de exploración de datos e ingeniería de características para los datos cargados en las tablas de Hive. Estos son ejemplos de tareas:

* Ver los diez registros principales en ambas tablas.
* Explorar distribuciones de datos de algunos campos en diferentes ventanas de tiempo.
* Investigar la calidad de los datos de los campos de longitud y latitud.
* Generar etiquetas de clasificación binaria y multiclase según el importe de las propinas.
* Generar características calculando las distancias de las carreras directas.

### <a name="exploration-view-the-top-10-records-in-table-trip"></a>Exploración: Consulta de los 10 principales registros de la tabla "trip"
> [!NOTE]
> Esta tarea la suelen hacer los científicos de datos.
> 
> 

Para ver el aspecto de los datos, examinamos 10 registros de cada tabla. Para inspeccionar los registros, ejecute las dos consultas siguientes por separado desde el símbolo de sistema del directorio de Hadoop en la consola de línea de comandos de Hadoop.

Para obtener los 10 principales registros de la tabla "trip" correspondientes al primer mes:

```console
hive -e "select * from nyctaxidb.trip where month=1 limit 10;"
```

Para obtener los 10 principales registros de la tabla "fare" correspondientes al primer mes:

```console
hive -e "select * from nyctaxidb.fare where month=1 limit 10;"
```

Puede guardar los registros en un archivo para verlos con facilidad con solo un pequeño cambio en la consulta anterior:

```console
hive -e "select * from nyctaxidb.fare where month=1 limit 10;" > C:\temp\testoutput
```

### <a name="exploration-view-the-number-of-records-in-each-of-the-12-partitions"></a>Exploración: Consulta del número de registros en cada una de las 12 particiones
> [!NOTE]
> Esta tarea la suelen hacer los científicos de datos.
> 
> 

Resulta interesante comprobar cómo varía el número de carreras durante el año natural. La agrupación por mes muestra la distribución de los viajes.

```console
hive -e "select month, count(*) from nyctaxidb.trip group by month;"
```

Este comando genera el siguiente resultado:

```output
1       14776615
2       13990176
3       15749228
4       15100468
5       15285049
6       14385456
7       13823840
8       12597109
9       14107693
10      15004556
11      14388451
12      13971118
Time taken: 283.406 seconds, Fetched: 12 row(s)
```

En este caso, la primera columna es el mes y la segunda, el número de carreras de ese mes.

También se puede contar el número total de registros del conjunto de datos de carreras ejecutando el comando siguiente en el símbolo del sistema del directorio de Hive:

```console
hive -e "select count(*) from nyctaxidb.trip;"
```

Este comando genera:

```output
173179759
Time taken: 284.017 seconds, Fetched: 1 row(s)
```

Mediante el uso de comandos similares a los que se mostraron para el conjunto de datos de carreras, se pueden emitir consultas de Hive desde el símbolo del sistema del directorio de Hive para el conjunto de datos de tarifas, con el fin de validar el número de registros.

```console
hive -e "select month, count(*) from nyctaxidb.fare group by month;"
```

Este comando genera este resultado:

```output
1       14776615
2       13990176
3       15749228
4       15100468
5       15285049
6       14385456
7       13823840
8       12597109
9       14107693
10      15004556
11      14388451
12      13971118
Time taken: 253.955 seconds, Fetched: 12 row(s)
```

Devuelve exactamente el mismo número de carreras por mes para ambos conjuntos de datos, lo que proporciona la primera validación de que los datos se han cargado correctamente.

Puede contar el número total de registros en el conjunto de datos de tarifas con el comando siguiente desde el símbolo del sistema del directorio de Hive:

```console
hive -e "select count(*) from nyctaxidb.fare;"
```

Este comando genera:

```output
173179759
Time taken: 186.683 seconds, Fetched: 1 row(s)
```

El número total de registros en ambas tablas también es igual, lo que proporciona una segunda validación de que los datos se han cargado correctamente.

### <a name="exploration-trip-distribution-by-medallion"></a>Exploración: Distribución de carreras por medallion
> [!NOTE]
> Este análisis suele ser una tarea que realizan los científicos de datos.
> 
> 

Este ejemplo identifica los taxis (por su número de identificación) con más de 100 carreras dentro de un período de tiempo. La consulta se beneficia del acceso a la tabla con particiones puesto que está condicionada por la variable de partición **month**. Los resultados de la consulta se escriben en un archivo local **queryoutput.tsv** en `C:\temp` en el nodo principal.

```console
hive -f "C:\temp\sample_hive_trip_count_by_medallion.hql" > C:\temp\queryoutput.tsv
```

Aquí está el contenido del archivo **sample\_hive\_trip\_count\_by\_medallion.hql** para su inspección.

```hiveql
SELECT medallion, COUNT(*) as med_count
FROM nyctaxidb.fare
WHERE month<=3
GROUP BY medallion
HAVING med_count > 100
ORDER BY med_count desc;
```

El número de taxi del conjunto de datos de taxis de NYC identifica a cada taxi. Se puede identificar qué taxis trabajan más preguntando cuáles realizaron más de un determinado número de carreras en un período de tiempo determinado. El ejemplo siguiente identifica los taxis que realizaron más de cien carreras en los tres primeros meses y guarda los resultados de la consulta en un archivo local, **C:\temp\queryoutput.tsv**.

Aquí está el contenido del archivo **sample\_hive\_trip\_count\_by\_medallion.hql** para su inspección.

```hiveql
SELECT medallion, COUNT(*) as med_count
FROM nyctaxidb.fare
WHERE month<=3
GROUP BY medallion
HAVING med_count > 100
ORDER BY med_count desc;
```

Desde el símbolo del sistema del directorio de Hive, ejecute el siguiente comando:

```console
hive -f "C:\temp\sample_hive_trip_count_by_medallion.hql" > C:\temp\queryoutput.tsv
```

### <a name="exploration-trip-distribution-by-medallion-and-hack-license"></a>Exploración: Distribución de carreras por medallion y hack_license
> [!NOTE]
> Esta tarea suelen realizarla los científicos de datos.
> 
> 

Al explorar un conjunto de datos, con frecuencia se quieren examinar las distribuciones de grupos de valores. En esta sección se ofrece un ejemplo de cómo llevar a cabo este análisis para los taxis y los conductores.

El archivo **sample\_hive\_trip\_count\_by\_medallion\_license.hql** agrupa el conjunto de datos de tarifas en función de los valores de **medallion** y **hack_license** , y devuelve los recuentos de cada combinación. A continuación se muestra su contenido:

```hiveql
SELECT medallion, hack_license, COUNT(*) as trip_count
FROM nyctaxidb.fare
WHERE month=1
GROUP BY medallion, hack_license
HAVING trip_count > 100
ORDER BY trip_count desc;
```

Esta consulta devuelve combinaciones de taxi y conductor ordenadas por número descendente de carreras.

Desde el símbolo del sistema del directorio de Hive, ejecute:

```console
hive -f "C:\temp\sample_hive_trip_count_by_medallion_license.hql" > C:\temp\queryoutput.tsv
```

Los resultados de la consulta se escriben en un archivo local **C:\temp\queryoutput.tsv**.

### <a name="exploration-assessing-data-quality-by-checking-for-invalid-longitude-or-latitude-records"></a>Exploración: Evaluación de la calidad de los datos mediante la comprobación de registros con latitud o longitud no válida
> [!NOTE]
> Esta tarea la suelen hacer los científicos de datos.
> 
> 

Un objetivo común del análisis de exploración de datos consiste en descartar registros no válidos o incorrectos. En el ejemplo de esta sección se determina si los campos de longitud o latitud contienen un valor fuera del área de la ciudad de Nueva York. Es probable que estos registros tengan valores de longitud o latitud incorrectos, por lo que queremos eliminarlos de todos los datos que se van a usar para el modelado.

Aquí se muestra el contenido del archivo **sample\_hive\_quality\_assessment.hql** para su inspección.

```hiveql
    SELECT COUNT(*) FROM nyctaxidb.trip
    WHERE month=1
    AND  (CAST(pickup_longitude AS float) NOT BETWEEN -90 AND -30
    OR    CAST(pickup_latitude AS float) NOT BETWEEN 30 AND 90
    OR    CAST(dropoff_longitude AS float) NOT BETWEEN -90 AND -30
    OR    CAST(dropoff_latitude AS float) NOT BETWEEN 30 AND 90);
```

Desde el símbolo del sistema del directorio de Hive, ejecute:

```console
hive -S -f "C:\temp\sample_hive_quality_assessment.hql"
```

El argumento *-S* incluido en este comando suprime la impresión de la pantalla de estado de los trabajos de asignación/reducción de Hive. Este comando es útil porque hace que la impresión en pantalla de los resultados de la consulta de Hive sea más legible.

### <a name="exploration-binary-class-distributions-of-trip-tips"></a>Exploración: Distribuciones de clase binaria de las propinas de las carreras
> [!NOTE]
> Esta tarea la suelen hacer los científicos de datos.
> 
> 

Para el problema de clasificación binaria descrito en la sección [Ejemplos de tareas de predicción](hive-walkthrough.md#mltasks) , es útil saber si se recibió una propina o no. Esta distribución de las propinas es binaria:

* Propina entregada (clase 1, tip\_amount > 0 $).  
* Sin propina (clase 0, tip\_amount = 0 $).

El siguiente archivo **sample\_hive\_tipped\_frequencies.hql** muestra el comando que se va a ejecutar:

```hiveql
SELECT tipped, COUNT(*) AS tip_freq
FROM
(
    SELECT if(tip_amount > 0, 1, 0) as tipped, tip_amount
    FROM nyctaxidb.fare
)tc
GROUP BY tipped;
```

Desde el símbolo del sistema del directorio de Hive, ejecute:

```console
hive -f "C:\temp\sample_hive_tipped_frequencies.hql"
```


### <a name="exploration-class-distributions-in-the-multiclass-setting"></a>Exploración: Distribuciones de clase en la configuración de clases múltiples
> [!NOTE]
> Esta tarea la suelen hacer los científicos de datos.
> 
> 

Para el problema de clasificación de clases múltiples descrito en la sección [Ejemplos de tareas de predicción](hive-walkthrough.md#mltasks), este conjunto de datos también se presta a una clasificación natural para predecir el importe de las propinas entregadas. Podemos usar ubicaciones para definir intervalos de propinas en la consulta. Para obtener las distribuciones de clase para los distintos intervalos de propina, usamos el archivo **sample\_hive\_tip\_range\_frequencies.hql**. A continuación se muestra su contenido.

```hiveql
SELECT tip_class, COUNT(*) AS tip_freq
FROM
(
    SELECT if(tip_amount=0, 0,
        if(tip_amount>0 and tip_amount<=5, 1,
        if(tip_amount>5 and tip_amount<=10, 2,
        if(tip_amount>10 and tip_amount<=20, 3, 4)))) as tip_class, tip_amount
    FROM nyctaxidb.fare
)tc
GROUP BY tip_class;
```

Ejecute el siguiente comando desde la consola de la línea de comandos de Hadoop:

```console
hive -f "C:\temp\sample_hive_tip_range_frequencies.hql"
```

### <a name="exploration-compute-the-direct-distance-between-two-longitude-latitude-locations"></a>Exploración: Cálculo de la distancia directa entre dos ubicaciones de longitud-latitud
> [!NOTE]
> Esta tarea la suelen hacer los científicos de datos.
> 
> 

Es posible que quiera saber si hay una diferencia entre la distancia directa entre dos lugares y la distancia real del viaje del taxi. Puede que un pasajero sea más remiso a dar propina si sospecha que el conductor le lleva intencionadamente por un recorrido más largo.

Para ver la comparación entre la distancia real de la carrera y la [distancia Haversine](https://en.wikipedia.org/wiki/Haversine_formula) entre dos puntos de longitud-latitud (la distancia del "círculo máximo"), puede usar las funciones trigonométricas disponibles en Hive:

```hiveql
set R=3959;
set pi=radians(180);

insert overwrite directory 'wasb:///queryoutputdir'

select pickup_longitude, pickup_latitude, dropoff_longitude, dropoff_latitude, trip_distance, trip_time_in_secs,
${hiveconf:R}*2*2*atan((1-sqrt(1-pow(sin((dropoff_latitude-pickup_latitude)
 *${hiveconf:pi}/180/2),2)-cos(pickup_latitude*${hiveconf:pi}/180)
 *cos(dropoff_latitude*${hiveconf:pi}/180)*pow(sin((dropoff_longitude-pickup_longitude)*${hiveconf:pi}/180/2),2)))
 /sqrt(pow(sin((dropoff_latitude-pickup_latitude)*${hiveconf:pi}/180/2),2)
 +cos(pickup_latitude*${hiveconf:pi}/180)*cos(dropoff_latitude*${hiveconf:pi}/180)*
 pow(sin((dropoff_longitude-pickup_longitude)*${hiveconf:pi}/180/2),2))) as direct_distance
from nyctaxidb.trip
where month=1
and pickup_longitude between -90 and -30
and pickup_latitude between 30 and 90
and dropoff_longitude between -90 and -30
and dropoff_latitude between 30 and 90;
```

En la consulta anterior, R es el radio de la Tierra en millas y pi se ha convertido a radianes. Los puntos de longitud-latitud se han filtrado para quitar los valores que están lejos del área metropolitana de Nueva York.

En este caso, escribimos los resultados en un directorio llamado **queryoutputdir**. La secuencia de los comandos siguientes crea primero este directorio de salida y después ejecuta el comando de Hive.

Desde el símbolo del sistema del directorio de Hive, ejecute:

```hiveql
hdfs dfs -mkdir wasb:///queryoutputdir

hive -f "C:\temp\sample_hive_trip_direct_distance.hql"
```

Los resultados de la consulta se escriben en nueve blobs de Azure de **queryoutputdir/000000\_0** a **queryoutputdir/000008\_0** bajo el contenedor predeterminado del clúster de Hadoop.

Para ver el tamaño de los blobs individuales, se ejecuta el siguiente comando desde el símbolo del sistema del directorio de Hive:

```hiveql
hdfs dfs -ls wasb:///queryoutputdir
```

Para ver el contenido de un archivo determinado, por ejemplo, **000000\_0** , se usa el comando `copyToLocal` de Hadoop.

```hiveql
hdfs dfs -copyToLocal wasb:///queryoutputdir/000000_0 C:\temp\tempfile
```

> [!WARNING]
> `copyToLocal` puede ser muy lento para archivos de gran tamaño y no se recomienda para su uso con ellos.  
> 
> 

Una ventaja clave de tener estos datos en un blob de Azure es que se pueden explorar dentro de Machine Learning mediante el módulo [Importación de datos][import-data].

## <a name="down-sample-data-and-build-models-in-machine-learning"></a><a name="#downsample"></a>Reducción de datos y creación de modelos en Machine Learning
> [!NOTE]
> Esta tarea la suelen hacer los científicos de datos.
> 
> 

Después de la fase de análisis de exploración de datos, estamos preparados para reducir los datos y crear modelos en Machine Learning. En esta sección, se muestra cómo usar una consulta de Hive para reducir la muestra de datos. Después, Machine Learning accede a ellos desde el módulo [Importar datos][import-data].

### <a name="down-sampling-the-data"></a>Reducción de tamaño de los datos
Este procedimiento incluye dos pasos. En primer lugar, se unen las tablas **nyctaxidb.trip** y **nyctaxidb.fare** en función de tres claves incluidas en todos los registros: **medallion** , **hack\_license** y **pickup\_datetime**. Después se genera una etiqueta de clasificación binaria **tipped** y una etiqueta de clasificación de múltiples clases **tip\_class**.

Para usar los datos muestreados directamente desde el módulo [Importar datos][import-data] de Machine Learning, debe almacenar los resultados de la consulta anterior en una tabla interna de Hive. En el paso siguiente, se crea una tabla interna de Hive y se rellena su contenido con los datos con muestreo reducido y combinados.

La consulta aplica funciones estándar de Hive directamente para generar los siguientes parámetros de hora a partir del campo **pickup\_datetime** :
- Hora del día
- Semana del año
- weekday ("1" equivale a lunes y "7" a domingo)

La consulta también genera la distancia directa entre las ubicaciones de recogida y destino. Para obtener una lista completa de estas funciones, consulte [LanguageManual UDF](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF) (Manual de lenguaje: campos definidos por el usuario).

Después esta consulta reduce los datos para que los resultados quepan en Azure Machine Learning Studio. Aproximadamente, solo el 1 % del conjunto de datos original se importa en el estudio.

A continuación se muestra el contenido del archivo **sample\_hive\_prepare\_for\_aml\_full.hql** que prepara los datos para la creación de modelos en Machine Learning:

```hiveql
set R = 3959;
set pi=radians(180);

create table if not exists nyctaxidb.nyctaxi_downsampled_dataset (

medallion string,
hack_license string,
vendor_id string,
rate_code string,
store_and_fwd_flag string,
pickup_datetime string,
dropoff_datetime string,
pickup_hour string,
pickup_week string,
weekday string,
passenger_count int,
trip_time_in_secs double,
trip_distance double,
pickup_longitude double,
pickup_latitude double,
dropoff_longitude double,
dropoff_latitude double,
direct_distance double,
payment_type string,
fare_amount double,
surcharge double,
mta_tax double,
tip_amount double,
tolls_amount double,
total_amount double,
tipped string,
tip_class string
)
row format delimited fields terminated by ','
lines terminated by '\n'
stored as textfile;

--- now insert contents of the join into the above internal table

insert overwrite table nyctaxidb.nyctaxi_downsampled_dataset
select
t.medallion,
t.hack_license,
t.vendor_id,
t.rate_code,
t.store_and_fwd_flag,
t.pickup_datetime,
t.dropoff_datetime,
hour(t.pickup_datetime) as pickup_hour,
weekofyear(t.pickup_datetime) as pickup_week,
from_unixtime(unix_timestamp(t.pickup_datetime, 'yyyy-MM-dd HH:mm:ss'),'u') as weekday,
t.passenger_count,
t.trip_time_in_secs,
t.trip_distance,
t.pickup_longitude,
t.pickup_latitude,
t.dropoff_longitude,
t.dropoff_latitude,
t.direct_distance,
f.payment_type,
f.fare_amount,
f.surcharge,
f.mta_tax,
f.tip_amount,
f.tolls_amount,
f.total_amount,
if(tip_amount>0,1,0) as tipped,
if(tip_amount=0,0,
if(tip_amount>0 and tip_amount<=5,1,
if(tip_amount>5 and tip_amount<=10,2,
if(tip_amount>10 and tip_amount<=20,3,4)))) as tip_class

from
(
select
medallion,
hack_license,
vendor_id,
rate_code,
store_and_fwd_flag,
pickup_datetime,
dropoff_datetime,
passenger_count,
trip_time_in_secs,
trip_distance,
pickup_longitude,
pickup_latitude,
dropoff_longitude,
dropoff_latitude,
${hiveconf:R}*2*2*atan((1-sqrt(1-pow(sin((dropoff_latitude-pickup_latitude)
*${hiveconf:pi}/180/2),2)-cos(pickup_latitude*${hiveconf:pi}/180)
*cos(dropoff_latitude*${hiveconf:pi}/180)*pow(sin((dropoff_longitude-pickup_longitude)*${hiveconf:pi}/180/2),2)))
/sqrt(pow(sin((dropoff_latitude-pickup_latitude)*${hiveconf:pi}/180/2),2)
+cos(pickup_latitude*${hiveconf:pi}/180)*cos(dropoff_latitude*${hiveconf:pi}/180)*pow(sin((dropoff_longitude-pickup_longitude)*${hiveconf:pi}/180/2),2))) as direct_distance,
rand() as sample_key

from nyctaxidb.trip
where pickup_latitude between 30 and 90
    and pickup_longitude between -90 and -30
    and dropoff_latitude between 30 and 90
    and dropoff_longitude between -90 and -30
)t
join
(
select
medallion,
hack_license,
vendor_id,
pickup_datetime,
payment_type,
fare_amount,
surcharge,
mta_tax,
tip_amount,
tolls_amount,
total_amount
from nyctaxidb.fare
)f
on t.medallion=f.medallion and t.hack_license=f.hack_license and t.pickup_datetime=f.pickup_datetime
where t.sample_key<=0.01
```

Para ejecutar esta consulta, escriba en el símbolo del sistema del directorio de Hive:

```console
hive -f "C:\temp\sample_hive_prepare_for_aml_full.hql"
```

Ahora hay una tabla interna, **nyctaxidb.nyctaxi_downsampled_dataset** , a la que se puede acceder mediante el módulo [Importar datos][import-data] de Machine Learning. También se puede utilizar este conjunto de datos para generar modelos de Machine Learning.  

### <a name="use-the-import-data-module-in-machine-learning-to-access-the-down-sampled-data"></a>Uso del módulo Importar datos de Machine Learning para acceder a los datos muestreados
Para la emisión de consultas de Hive en el módulo [Importar datos][import-data] de Machine Learning, se necesita acceso a un área de trabajo de Machine Learning. También se necesita acceso a las credenciales del clúster y su cuenta de almacenamiento asociada.

Aquí se indican algunos detalles sobre el módulo [Importar datos][import-data] y los parámetros de entrada:

**URI del servidor de HCatalog** : Si el nombre del clúster es **abc123** , sería: https:\//abc123.azurehdinsight.net.

**Nombre de la cuenta de usuario de Hadoop** : nombre de usuario elegido para el clúster (no es el nombre de usuario de acceso remoto).

**Contraseña de cuenta de usuario de Hadoop** : contraseña elegida para el clúster (no es la contraseña de acceso remoto).

**Ubicación de los datos de salida** : se elige para que sea Azure.

**Nombre de la cuenta de Azure Storage** : nombre de la cuenta de almacenamiento predeterminada asociada al clúster.

**Nombre del contenedor de Azure** : nombre del contenedor predeterminado del clúster, que suele ser el mismo que el del clúster. En un clúster denominado **abc123** , el nombre es abc123.

> [!IMPORTANT]
> Cualquier tabla que se quiera consultar mediante el módulo [Importar datos][import-data] de Machine Learning debe ser una tabla interna.
> 
> 

Aquí se muestra cómo determinar si una tabla **T** en una base de datos **D.db** es una tabla interna. Desde el símbolo del sistema del directorio de Hive, ejecute el siguiente comando:

```hiveql
hdfs dfs -ls wasb:///D.db/T
```

Si la tabla es una tabla interna y está rellena, su contenido se debe mostrar aquí.

Otro modo de determinar si una tabla es una tabla interna es utilizar Explorador de Azure Storage. Úselo para navegar al nombre del contenedor predeterminado del clúster y, después, filtre por el nombre de tabla. Si se muestran la tabla y su contenido, se confirma que es una tabla interna.

Aquí se muestra una captura de pantalla de la consulta de Hive y el módulo [Importar datos][import-data]:

![Captura de pantalla de la consulta de Hive para el módulo de importación de datos](./media/hive-walkthrough/1eTYf52.png)

Debido a que la muestra de datos reducida se encuentra en el contenedor predeterminado, la consulta de Hive resultante de Machine Learning es sencilla. Es simplemente **SELECT * FROM nyctaxidb.nyctaxi\_downsampled\_data**.

Ahora el conjunto de datos se puede usar como punto de partida para la creación de modelos de Machine Learning.

### <a name="build-models-in-machine-learning"></a><a name="mlmodel"></a>Creación de modelos en Machine Learning
Ya puede pasar a la creación del modelo y la implementación del mismo en [Machine Learning](https://studio.azureml.net). Los datos ya están listos para usarlos con el fin de abordar los problemas de predicción identificados anteriormente:

- **Clasificación binaria** : permite predecir si se dio propina en una carrera o no.

  **Mecanismo de aprendizaje utilizado:** regresión logística de dos clases

  a. En este problema la etiqueta de destino (o clase) es **tipped**. El conjunto de datos con muestreo reducido original incluye algunas columnas que no contienen datos para el experimento de clasificación. Se trata, en concreto, de **tip\_class** , **tip\_amount** y **total\_amount** , que dan información sobre la etiqueta de destino que no está disponible en el momento de la prueba. Estas columnas se quitan mediante el módulo [Seleccionar columnas de conjunto de datos][select-columns].

  El siguiente diagrama muestra nuestro experimento para predecir si se pagó o no una propina por una carrera determinada:

  ![Diagrama del experimento para predecir si se pagó propina](./media/hive-walkthrough/QGxRz5A.png)

  b. En este experimento las distribuciones de la etiqueta de destino eran aproximadamente 1:1.

   La siguiente gráfica muestra la distribución de las etiquetas de clase de propina para el problema de clasificación binaria:

  ![Gráfica de distribución de las etiquetas de clase de sugerencia](./media/hive-walkthrough/9mM4jlD.png)

    Como resultado, obtenemos un área bajo la curva (AUC) de 0,987, tal como se muestra en la figura siguiente:

  ![Gráfica del valor de AUC](./media/hive-walkthrough/8JDT0F8.png)

- **Clasificación multiclase** : Permite predecir el intervalo de importes de propina pagadas por la carrera mediante las clases definidas anteriormente.

  **Mecanismo de aprendizaje utilizado:** regresión logística multiclase

  a. En este problema, la etiqueta de destino (o clase) es **tip\_class** , que puede adoptar uno de cinco valores (0,1,2,3,4). Como en el caso de clasificación binaria, tenemos algunas columnas que son pérdidas de destino para este experimento. Se trata, en concreto, de: **tipped** , **tip\_amount** y **total\_amount** , que dan información sobre la etiqueta de destino que no está disponible en el momento de la prueba. Estas columnas se quitan mediante el módulo [Seleccionar columnas de conjunto de datos][select-columns].

  En el siguiente diagrama se muestra el experimento para predecir en qué intervalo es probable que se sitúe una propina. Los intervalos son: Clase 0: propina = 0 USD, Clase 1: propina > 0 USD y propina <= 5 USD, Clase 2: propina > 5 USD y propina <= 10 USD, Clase 3: propina > 10 USD y propina <= 20 USD y Clase 4: propina > 20 USD.

  ![Diagrama del experimento para predecir el intervalo de propina](./media/hive-walkthrough/5ztv0n0.png)

  Ahora vemos qué aspecto tiene la distribución de clases de prueba real. Las clases 0 y 1 son frecuentes, las demás no lo son.

  ![Gráfico de distribución de la clase de prueba](./media/hive-walkthrough/Vy1FUKa.png)

  b. En este experimento se usa una matriz de confusión para analizar la precisión de las predicciones, como se muestra aquí:

  ![Matriz de confusión](./media/hive-walkthrough/cxFmErM.png)

  Aunque la precisión en las clases frecuentes es buena, el modelo no hace un buen trabajo de "aprendizaje" en las clases menos frecuentes.

- **Tarea de regresión** : Permite predecir el importe de la propina pagada por una carrera.

  **Mecanismo de aprendizaje utilizado:** árbol de decisión ampliado

  a. En este problema, la etiqueta de destino (o clase) es **tip\_amount**. En este caso, las pérdidas de destino son: **tipped** , **tip\_class** y **total\_amount**. Todas estas variables ofrecen información sobre el importe de la propina, que no suele estar disponible en el momento de la prueba. Estas columnas se quitan mediante el módulo [Seleccionar columnas de conjunto de datos][select-columns].

  El diagrama siguiente muestra nuestro experimento para predecir el importe de una propina determinada:

  ![Diagrama del experimento para predecir la cantidad de propina](./media/hive-walkthrough/11TZWgV.png)

  b. En los problemas de regresión se mide la precisión de nuestra predicción mediante la observación del error cuadrático en las predicciones y el coeficiente de determinación:

  ![Captura de pantalla de las estadísticas de predicción](./media/hive-walkthrough/Jat9mrz.png)

  Aquí, el coeficiente de determinación es 0,709, lo que implica que aproximadamente el 71 % de la varianza se explica por nuestros coeficientes de modelo.

> [!IMPORTANT]
> Para obtener más información sobre Machine Learning y cómo obtener acceso a él y usarlo, vea [¿Qué es Machine Learning en Azure?](../classic/index.yml) Además, [Azure AI Gallery](https://gallery.cortanaintelligence.com/) cubre una gama de experimentos y ofrece una introducción exhaustiva sobre la variedad de capacidades de Machine Learning.
> 
> 

## <a name="license-information"></a>Información de licencia
Microsoft comparte este tutorial de ejemplo y sus scripts adjuntos bajo la licencia MIT. Para más información, consulte el archivo **LICENSE.txt** del directorio del código de ejemplo en GitHub.

## <a name="references"></a>Referencias
•   [Página de descarga de NYC Taxi Trips de Andrés Monroy](https://www.andresmh.com/nyctaxitrips/)  
•   [FOILing NYC’s Taxi Trip Data de Chris Whong](https://chriswhong.com/open-data/foil_nyc_taxi/)   
•   [Estadísticas e investigación de la Comisión de taxis y limusinas de la Ciudad de Nueva York](https://www1.nyc.gov/site/tlc/about/tlc-trip-record-data.page)

[2]: ./media/hive-walkthrough/output-hive-results-3.png
[11]: ./media/hive-walkthrough/hive-reader-properties.png
[12]: ./media/hive-walkthrough/binary-classification-training.png
[13]: ./media/hive-walkthrough/create-scoring-experiment.png
[14]: ./media/hive-walkthrough/binary-classification-scoring.png
[15]: ./media/hive-walkthrough/amlreader.png

<!-- Module References -->
[select-columns]: /azure/machine-learning/studio-module-reference/select-columns-in-dataset
[import-data]: /azure/machine-learning/studio-module-reference/import-data
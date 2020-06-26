---
title: 'Tutorial: Introducción a Azure Synapse Analytics'
description: En este tutorial, aprenderá los pasos básicos para configurar y usar Azure Synapse Analytics.
services: synapse-analytics
author: saveenr
ms.author: saveenr
manager: julieMSFT
ms.reviewer: jrasnick
ms.service: synapse-analytics
ms.topic: tutorial
ms.date: 05/19/2020
ms.openlocfilehash: daa8b594b06203c7de9a9b462be469dd71ed2e49
ms.sourcegitcommit: 6571e34e609785e82751f0b34f6237686470c1f3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/15/2020
ms.locfileid: "84791867"
---
# <a name="get-started-with-azure-synapse-analytics"></a>Introducción a Azure Synapse Analytics

Este tutorial le guía por los pasos básicos para configurar y usar Azure Synapse Analytics.

## <a name="prepare-a-storage-account"></a>Preparación de una cuenta de almacenamiento

1. Abra [Azure Portal](https://portal.azure.com).
1. Cree una cuenta de almacenamiento con la siguiente configuración:

    |Pestaña|Configuración | Valor sugerido | Descripción |
    |---|---|---|---|
    |Aspectos básicos|**Nombre de cuenta de almacenamiento**| Puede asignarle cualquier nombre.|En este documento, usaremos el nombre **contosolake**.|
    |Aspectos básicos|**Tipo de cuenta**|Debe establecerse en **StorageV2**.||
    |Aspectos básicos|**Ubicación**|Elija cualquier ubicación.| Se recomienda que el área de trabajo de Azure Synapse Analytics y la cuenta de Azure Data Lake Storage Gen2 se encuentren en la misma región.|
    |Avanzado|**Data Lake Storage Gen2**|**Enabled**| Azure Synapse solo funciona con cuentas de almacenamiento en las que esté habilitado este valor.|
    |||||

1. Después de crear la cuenta de almacenamiento, seleccione **Control de acceso (IAM)** en el panel izquierdo. Luego, asigne los siguientes roles o asegúrese de que ya estén asignados:
    1. Asígnese el rol **Propietario**.
    1. Asígnese el rol **Propietario de datos de Storage Blob**.
1. En el panel izquierdo, seleccione **Contenedores** y cree un contenedor.
1. Puede asignar cualquier nombre al contenedor. En este documento, llamaremos **users** al contenedor.
1. Acepte el valor predeterminado **Nivel de acceso público** y, después, seleccione **Crear**.

En el paso siguiente, configurará el área de trabajo de Azure Synapse para usar esta cuenta de almacenamiento como su cuenta de almacenamiento "principal" y el contenedor para almacenar los datos del área de trabajo. El área de trabajo almacena datos en tablas de Apache Spark. Almacena los registros de aplicaciones de Spark en una carpeta denominada **/synapse/workspacename**.

## <a name="create-a-synapse-workspace"></a>Creación de un área de trabajo de Synapse

1. Abra [Azure Portal](https://portal.azure.com) y, en la parte superior, busque **Synapse**.
1. En los resultados de la búsqueda, en **Servicios**, seleccione **Azure Synapse Analytics (versión preliminar de las áreas de trabajo)** .
1. Seleccione **Agregar** para crear un área de trabajo con esta configuración:

    |Pestaña|Configuración | Valor sugerido | Descripción |
    |---|---|---|---|
    |Aspectos básicos|**Workspace name** (Nombre del área de trabajo)|Puede darle cualquier nombre.| En este documento, usaremos **myworkspace**.|
    |Aspectos básicos|**Región**|Coincidencia de la región de la cuenta de almacenamiento.|

1. En **Select Data Lake Storage Gen 2** (Seleccionar Data Lake Storage Gen 2), seleccione la cuenta y el contenedor que creó anteriormente.
1. Seleccione **Revisar y crear** > **Crear**. El área de trabajo estará lista en unos minutos.

## <a name="verify-access-to-the-storage-account"></a>Comprobación del acceso a la cuenta de almacenamiento

Es posible que las identidades administradas del área de trabajo de Azure Synapse ya tengan acceso a la cuenta de almacenamiento. Siga estos pasos para asegurarse:

1. Abra [Azure Portal](https://portal.azure.com) y la cuenta de almacenamiento principal elegida para el área de trabajo.
1. Seleccione **Control de acceso (IAM)** en el panel izquierdo.
1. Asigne los siguientes roles o asegúrese de que ya estén asignados. Se usa el mismo nombre para la identidad y para el nombre del área de trabajo.
    1. Para el rol **Colaborador de datos de Storage Blob** de la cuenta de almacenamiento, asigne **myworkspace** como identidad del área de trabajo.
    1. Asigne **myworkspace** como el nombre del área de trabajo.

1. Seleccione **Guardar**.

## <a name="open-synapse-studio"></a>Abrir Synapse Studio

Una vez creada el área de trabajo de Azure Synapse, hay dos maneras de abrir Synapse Studio:

* Abrir el área de trabajo de Synapse en [Azure Portal](https://portal.azure.com). En la parte superior de la sección **Información general**, seleccione **Iniciar Synapse Studio**.
* Ir a https://web.azuresynapse.net e iniciar sesión en el área de trabajo.

## <a name="create-a-sql-pool"></a>Creación de un grupo de SQL

1. En Synapse Studio, en el panel izquierdo, seleccione **Administrar** > **Grupos de SQL**.
1. Seleccione **Nuevo** y especifique estos valores:

    |Configuración | Valor sugerido | 
    |---|---|---|
    |**Nombre del grupo de SQL**| **SQLDB1**|
    |**Nivel de rendimiento**|**DW100C**|
    |||

1. Seleccione **Revisar y crear** > **Crear**. El grupo de SQL estará listo en unos minutos. El grupo de SQL se asocia con una base de datos del grupo de SQL también denominada **SQLDB1**.

Los grupos de SQL consumen recursos facturables mientras están activos. Puede pausar los grupos más adelante para reducir los costos.

## <a name="create-an-apache-spark-pool"></a>Creación de un grupo de Apache Spark

1. En Synapse Studio, en el lado izquierdo, seleccione **Administrar** > **Grupos de Apache Spark**.
1. Seleccione **Nuevo** y especifique estos valores:

    |Configuración | Valor sugerido | 
    |---|---|---|
    |**Nombre del grupo de Apache Spark**|**Spark1**
    |**Tamaño del nodo**| **Pequeño**|
    |**Número de nodos**| Establezca el valor mínimo en 3 y el máximo en 3|

1. Seleccione **Revisar y crear** > **Crear**. El grupo de Apache Spark estará listo en unos segundos.

> [!NOTE]
> A pesar del nombre, un grupo de Apache Spark no es como un grupo de SQL. Es solo un conjunto de metadatos básicos que se usan para indicar al área de trabajo de Azure Synapse cómo interactuar con Spark.

Al ser metadatos, los grupos de Spark no se pueden iniciar ni detener.

Cuando se realiza una actividad de Spark en Azure Synapse, se especifica el grupo de Spark que se va a usar. El grupo indica a Azure Synapse el número de recursos de Spark que se van a usar. Solo paga por los recursos que utiliza. Al dejar de usar el grupo de forma activa, los recursos agotarán el tiempo de espera y se reciclarán automáticamente.

> [!NOTE]
> Las bases de datos de Spark se crean de forma independiente de los grupos de Spark. Un área de trabajo siempre tiene una base de datos de Spark denominada **default**. Puede crear bases de datos de Spark adicionales.

## <a name="the-sql-on-demand-pool"></a>El grupo de SQL a petición

Todas las áreas de trabajo incluyen un grupo precompilado llamado **SQL on-demand**. Este grupo no se puede eliminar. Este grupo permite trabajar con SQL sin tener que crear ni pensar en la administración de un grupo de SQL en Azure Synapse.

A diferencia de los otros tipos de grupos, la facturación de SQL on-demand se basa en la cantidad de datos que se examinan para ejecutar la consulta (no en el número de recursos usados para ello).

* SQL on-demand tiene sus propias bases de datos de SQL On-Demand, que existen de forma independiente de cualquier grupo de SQL On-Demand.
* Un área de trabajo siempre tiene exactamente un grupo de SQL On-Demand denominado **SQL on-demand**.

## <a name="load-the-nyc-taxi-sample-data-into-the-sqldb1-database"></a>Carga de los datos del ejemplo de taxis de Nueva York en la base de datos SQLDB1

1. En Synapse Studio, en el menú azul de la parte superior, seleccione **?** .
1. Seleccione **Getting started** > **Getting started hub** (Introducción > Introducción al centro).
1. En la tarjeta con la etiqueta **Query sample data** (Consultar datos de ejemplo), seleccione el grupo de SQL llamado **SQLDB1**.
1. Seleccione **Query data** (Consultar datos). Aparece brevemente la notificación "Cargando datos de ejemplo". Una barra de estado de color azul claro cerca de la parte superior de Synapse Studio indica que se están cargando los datos en SQLDB1.
1. Cuando la barra de estado se vuelva verde, descártela.

## <a name="explore-the-nyc-taxi-data-in-the-sql-pool"></a>Exploración de los datos de taxis de Nueva York en el grupo de SQL

1. En Synapse Studio, vaya al centro **Data** (Datos).
1. Vaya a **SQLDB1** > **Tablas**. Verá que se han cargado varias tablas.
1. Haga clic con el botón derecho en la tabla **dbo.Trip** y seleccione **New SQL Script** > **Select TOP 100 Rows** (Nuevo script SQL > Seleccionar 100 primeras filas).
1. Espere mientras se crea un nuevo script de SQL y se ejecuta.
1. Observe que en la parte superior del script de SQL, en **Connect to** (Conectar a) está seleccionado automáticamente el grupo de SQL llamado **SQLDB1**.
1. Reemplace el texto del script de SQL por este código y ejecútelo.

    ```sql
    SELECT PassengerCount,
          SUM(TripDistanceMiles) as SumTripDistance,
          AVG(TripDistanceMiles) as AvgTripDistance
    FROM  dbo.Trip
    WHERE TripDistanceMiles > 0 AND PassengerCount > 0
    GROUP BY PassengerCount
    ORDER BY PassengerCount
    ```

    En esta consulta se muestra la relación entre las distancias recorridas totales y la distancia media recorrida, y el número de pasajeros.
1. En la ventana de resultados del script de SQL, cambie el valor de **View** (Vista) a **Chart** (Gráfico) para ver los resultados en un gráfico de líneas.

## <a name="load-the-nyc-taxi-data-into-the-spark-nyctaxi-database"></a>Carga de los datos de los taxis de Nueva York en la base de datos nyctaxi de Spark

Los datos están disponibles en una tabla en **SQLDB1**. Cárguelos en una base de datos de Spark denominada **nyctaxi**.

1. En Synapse Studio, vaya al centro **Develop** (Desarrollo).
1. Seleccione **+**  > **Cuaderno**.
1. En la parte superior del cuaderno, establezca el valor de **Adjuntar a** en **Spark1**.
1. Seleccione **Agregar código** para agregar una celda de código del cuaderno y, a continuación, pegue el siguiente texto:

    ```scala
    %%spark
    spark.sql("CREATE DATABASE IF NOT EXISTS nyctaxi")
    val df = spark.read.sqlanalytics("SQLDB1.dbo.Trip") 
    df.write.mode("overwrite").saveAsTable("nyctaxi.trip")
    ```

1. Vaya al centro **Data** (Datos), haga clic con el botón derecho en **Databases** (Bases de datos) y seleccione **Refresh** (Actualizar). Debería ver estas bases de datos:

    - **SQLDB1** (grupo de SQL)
    - **nyctaxi** (Spark)

## <a name="analyze-the-nyc-taxi-data-using-spark-and-notebooks"></a>Análisis de los datos de los taxis de Nueva York mediante Spark y cuadernos

1. Vuelva al cuaderno.
1. Cree una nueva celda de código y escriba el texto siguiente. A continuación, ejecute la celda para que se muestren los datos de taxis de Nueva York que cargamos en la base de datos **nyctaxi** de Spark.

   ```py
   %%pyspark
   df = spark.sql("SELECT * FROM nyctaxi.trip") 
   display(df)
   ```

1. Ejecute el código siguiente para realizar el mismo análisis que hicimos anteriormente con el grupo de SQL **SQLDB1**. Este código guarda los resultados del análisis en una tabla denominada **nyctaxi.passengercountstats** y visualiza los resultados.

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

## <a name="load-data-from-a-spark-table-into-a-sql-pool-table"></a>Carga de datos de una tabla de Spark en una tabla de grupos de SQL

Anteriormente copiamos datos de la tabla del grupo de SQL **SQLDB1.dbo.Trip** en la tabla de Spark **nyctaxi.trip**. Luego, mediante Spark, agregamos los datos a la tabla de Spark **nyctaxi.passengercountstats**. Ahora se copiarán los datos de **nyctaxi.passengercountstats** en una tabla de grupo de SQL denominada **SQLDB1.dbo.PassengerCountStats**.

Ejecute la siguiente celda en el cuaderno. Esta copia la tabla de Spark agregada en la tabla de grupos de SQL.

```scala
%%spark
val df = spark.sql("SELECT * FROM nyctaxi.passengercountstats")
df.write.sqlanalytics("SQLDB1.dbo.PassengerCountStats", Constants.INTERNAL )
```

## <a name="analyze-nyc-taxi-data-in-spark-databases-using-sql-on-demand"></a>Análisis de los datos de taxis de Nueva York en las bases de datos de Spark mediante SQL On-Demand

SQL On-Demand ve y puede consultar automáticamente las tablas de las bases de datos de Spark.

1. En Synapse Studio, vaya al centro **Develop** (Desarrollo) y cree un script de SQL.
1. En **Conectarse a** seleccione **SQL On-Demand**.
1. Pegue el siguiente texto en el script y ejecute el script.

    ```sql
    SELECT *
    FROM nyctaxi.dbo.passengercountstats
    ```

    > [!NOTE]
    > La primera vez que se ejecuta una consulta que usa SQL On-Demand, se tardarán unos 10 segundos en recopilar los recursos de SQL necesarios para ejecutar las consultas. Las consultas posteriores serán mucho más rápidas.
  
## <a name="orchestrate-activities-with-pipelines"></a>Organización de actividades con canalizaciones

Puede organizar una gran variedad de tareas en Azure Synapse.

1. En Synapse Studio, vaya al centro **Orchestrate** (Organización).
1. Seleccione **+**  > **Canalización** para crear una canalización.
1. Vaya al centro **Develop** (Desarrollo) y busque el cuaderno que creó anteriormente.
1. Arrastre el cuaderno a la canalización.
1. En la canalización, seleccione **Agregar desencadenador** > **Nuevo/editar**.
1. En **Choose trigger** (Elegir desencadenador), seleccione **Nuevo** y, después, en el apartado en que se elige la **periodicidad**, configure el desencadenador para que se ejecute cada hora.
1. Seleccione **Aceptar**.
1. Seleccione **Publish All** (Publicar todo). La canalización se ejecuta cada hora.
1. Para que la canalización se ejecute ahora sin esperar a la próxima hora, seleccione **Agregar desencadenador** > **Nuevo/editar**.

## <a name="work-with-data-in-a-storage-account"></a>Uso de los datos de una cuenta de almacenamiento

Hasta ahora, hemos descrito escenarios en los que los datos residían en las bases de datos del área de trabajo. Ahora le mostraremos cómo trabajar con archivos que residen en cuentas de almacenamiento. En este escenario, se usará la cuenta de almacenamiento principal del área de trabajo y el contenedor que se especificó al crear el área de trabajo.

* El nombre de la cuenta de almacenamiento: **contosolake**
* El nombre del contenedor en la cuenta de almacenamiento: **users**

### <a name="create-csv-and-parquet-files-in-your-storage-account"></a>Creación de archivos CSV y Parquet en la cuenta de almacenamiento

Ejecute el siguiente código en un cuaderno. Crea un archivo .csv y un archivo Parquet en la cuenta de almacenamiento.

```py
%%pyspark
df = spark.sql("SELECT * FROM nyctaxi.passengercountstats")
df = df.repartition(1) # This ensure we'll get a single file during write()
df.write.mode("overwrite").csv("/NYCTaxi/PassengerCountStats.csv")
df.write.mode("overwrite").parquet("/NYCTaxi/PassengerCountStats.parquet")
```

### <a name="analyze-data-in-a-storage-account"></a>Análisis de datos en una cuenta de almacenamiento

1. En Synapse Studio, vaya al centro **Data** (Datos) y, a continuación, seleccione **Vinculado**.
1. Vaya a **Cuentas de almacenamiento** > **myworkspace (Principal: contosolake)** .
1. Seleccione **users (Principal)"** . Debería ver la carpeta **NYCTaxi**. Dentro debería ver dos carpetas llamadas **PassengerCountStats.csv** y **PassengerCountStats.parquet**.
1. Abra la carpeta **PassengerCountStats.parquet**. Dentro de ella verá un archivo Parquet con un nombre parecido a *part-00000-2638e00c-0790-496b-a523-578da9a15019-c000.snappy.parquet*.
1. Haga clic con el botón secundario en el archivo **.parquet** y, a continuación, seleccione **Nuevo cuaderno**. Se crea un cuaderno que tiene una celda similar a la siguiente:

    ```py
    %%pyspark
    data_path = spark.read.load('abfss://users@contosolake.dfs.core.windows.net/NYCTaxi/PassengerCountStats.parquet/part-00000-1f251a58-d8ac-4972-9215-8d528d490690-c000.snappy.parquet', format='parquet')
    data_path.show(100)
    ```

1. Ejecute la celda.
1. Haga clic con el botón derecho en el archivo Parquet que está dentro y seleccione **New SQL Script** > **Select TOP 100 Rows** (Nuevo script SQL > Seleccionar 100 primeras filas). Se crea un script SQL similar al siguiente:

    ```sql
    SELECT TOP 100 *
    FROM OPENROWSET(
        BULK 'https://contosolake.dfs.core.windows.net/users/NYCTaxi/PassengerCountStats.parquet/part-00000-1f251a58-d8ac-4972-9215-8d528d490690-c000.snappy.parquet',
        FORMAT='PARQUET'
    ) AS [r];
    ```

     En el script, el campo **Adjuntar a** se establecerá en **SQL On-Demand**.

1. Ejecute el script.

## <a name="visualize-data-with-power-bi"></a>Visualización de datos con Power BI

A partir de los datos de taxis de Nueva York creamos conjuntos de datos agregados en dos tablas:
- **nyctaxi.passengercountstats**
- **SQLDB1.dbo.PassengerCountStats**

Puede vincular un área de trabajo de Power BI a su área de trabajo de Azure Synapse. Esto le permite obtener fácilmente datos en el área de trabajo de Power BI. Puede editar los informes de Power BI directamente en el área de trabajo de Azure Synapse.

### <a name="create-a-power-bi-workspace"></a>Creación de un área de trabajo de Power BI

1. Inicie sesión en [powerbi.microsoft.com](https://powerbi.microsoft.com/).
1. Cree una nueva área de trabajo de Power BI denominada **NYCTaxiWorkspace1**.

### <a name="link-your-azure-synapse-workspace-to-your-new-power-bi-workspace"></a>Vinculación de un área de trabajo de Azure Synapse a un área de trabajo de Power BI

1. En Synapse Studio, vaya a **Administrar** > **Servicios vinculados**.
1. Seleccione **Nuevo** > **Conectar con Power BI** y, después, configure estos campos:

    |Configuración | Valor sugerido | 
    |---|---|
    |**Nombre**|**NYCTaxiWorkspace1**|
    |**Workspace name** (Nombre del área de trabajo)|**NYCTaxiWorkspace1**|

1. Seleccione **Crear**.

### <a name="create-a-power-bi-dataset-that-uses-data-in-your-azure-synapse-workspace"></a>Creación de un conjunto de datos de Power BI que use los datos del área de trabajo de Azure Synapse

1. En Synapse Studio, vaya al centro **Develop**(Desarrollo) > **Power BI**.
1. Vaya a **NYCTaxiWorkspace1** > **Conjuntos de datos de Power BI** y seleccione **New Power BI dataset** (Nuevo conjunto de datos de Power BI).
1. Mantenga el puntero sobre la base de datos **SQLDB1** y seleccione **Download .pbids file** (Descargar archivo .pbids).
1. Abra el archivo **.pbids** que ha descargado. Power BI Desktop se abre y se conecta automáticamente a **SQLDB1** en el área de trabajo de Azure Synapse.
1. Si aparece un cuadro de diálogo denominado **Base de datos SQL Server**:
    1. Seleccione **Microsoft account** (Cuenta de Microsoft).
    1. Seleccione **Iniciar sesión** e inicie sesión en su cuenta.
    1. Seleccione **Conectar**.
1. Después de abrir el cuadro de diálogo **Navegador**, compruebe la tabla **PassengerCountStats** y seleccione **Cargar**.
1. Después de que aparezca el cuadro de diálogo **Configuración de la conexión**, seleccione **DirectQuery** > **Aceptar**.
1. Seleccione el botón **Informar** de la izquierda.
1. Agregue **Line Chart** (Gráfico de líneas) al informe.
    1. Arrastre la columna **PassengerCount** a **Visualizaciones** > **Eje**.
    1. Arrastre las columnas **SumTripDistance** y **AvgTripDistance** a **Visualizaciones** > **Valores**.
1. En la pestaña **Inicio**, seleccione **Publicar**.
1. Haga clic en **Guardar** para guardar los cambios.
1. Elija el nombre de archivo **PassengerAnalysis.pbix** y, después, seleccione **Guardar**.
1. En **Seleccionar un destino**, elija **NYCTaxiWorkspace1** y, a continuación, haga clic en **Seleccionar**.
1. Espere a que finalice la publicación.

### <a name="configure-authentication-for-your-dataset"></a>Configuración de la autenticación del conjunto de datos

1. Abra [powerbi.microsoft.com](https://powerbi.microsoft.com/) y seleccione **Iniciar sesión**.
1. En el lado izquierdo, en **Áreas de trabajo**, seleccione el área de trabajo **NYCTaxiWorkspace1**.
1. Dentro de esa área de trabajo, busque un conjunto de datos llamado **Passenger Analysis** (Análisis de pasajeros) y un informe con el mismo nombre.
1. Mantenga el puntero sobre el conjunto de datos **PassengerAnalysis**, seleccione el botón de puntos suspensivos (...) y, a continuación, seleccione **Configuración**.
1. En **Credenciales de origen de datos**, en **Método de autenticación**, seleccione **OAuth2** y, después, seleccione **Iniciar sesión**.

### <a name="edit-a-report-in-synapse-studio"></a>Edición de informes en Synapse Studio

1. Vuelva a Synapse Studio y seleccione **Close and refresh** (Cerrar y actualizar).
1. Vaya al centro **Develop** (Desarrollo).
1. Mantenga el puntero sobre **Power BI** y seleccione el nodo **Informes de Power BI**.
1. En **Power BI** debería ver:
    1. En **NYCTaxiWorkspace1Power** > **Conjuntos de datos de Power BI**, un nuevo conjunto de datos denominado **PassengerAnalysis**.
    1. En **NYCTaxiWorkspace1** > **Informes de Power BI**, un nuevo informe denominado **PassengerAnalysis**.
1. Seleccione el informe **PassengerAnalysis**. El informe se abre y puede editarlo directamente en Synapse Studio.

## <a name="monitor-activities"></a>Actividad de supervisión

1. En Synapse Studio, vaya al centro **Monitor** (Supervisión).
1. En esta ubicación, puede ver un historial de todas las actividades que se realizan en el área de trabajo y las que están activas ahora.
1. Explore **Ejecuciones de canalizaciones**, **Apache Spark applications** (Aplicaciones de Apache Spark) y **SQL requests** (Solicitudes SQL) y podrá ver lo que ya ha hecho en el área de trabajo.

## <a name="next-steps"></a>Pasos siguientes

Conozca más información acerca de [Azure Synapse Analytics (versión preliminar de las áreas de trabajo)](overview-what-is.md).


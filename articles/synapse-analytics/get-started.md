---
title: 'Tutorial: Introducción a Azure Synapse Analytics'
description: Información paso a paso para conocer rápidamente los conceptos básicos de Azure Synapse
services: synapse-analytics
author: saveenr
ms.author: saveenr
manager: julieMSFT
ms.reviewer: jrasnick
ms.service: synapse-analytics
ms.topic: quickstart
ms.date: 05/19/2020
ms.openlocfilehash: 00f93086fec62c08c5241d868fc5104a1197cff3
ms.sourcegitcommit: 1de57529ab349341447d77a0717f6ced5335074e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/09/2020
ms.locfileid: "84605415"
---
# <a name="getting-started-with-azure-synapse-analytics"></a>Introducción a Azure Synapse Analytics

Este documento le guiará por los pasos básicos necesarios para configurar y usar Azure Synapse Analytics.

## <a name="prepare-a-storage-account-for-use-with-a-synapse-workspace"></a>Preparación de una cuenta de almacenamiento para usarla en un área de trabajo de Synapse

* Abra [Azure Portal](https://portal.azure.com).
* Cree una cuenta de almacenamiento con la siguiente configuración:

    |Pestaña|Configuración | Valor sugerido | Descripción |
    |---|---|---|---|
    |Aspectos básicos|**Nombre de cuenta de almacenamiento**| Puede asignarle cualquier nombre.|En este documento, usaremos el nombre `contosolake`.|
    |Aspectos básicos|**Tipo de cuenta**|Se debe establecer en `StorageV2`||
    |Aspectos básicos|**Ubicación**|Puede seleccionar cualquier ubicación| Se recomienda que el área de trabajo de Synapse y la cuenta de Azure Data Lake Storage (ADLS) Gen2 se encuentren en la misma región.|
    |Avanzado|**Data Lake Storage Gen2**|`Enabled`| Azure Synapse solo funciona con cuentas de almacenamiento en las que esté habilitado este valor.|

1. Una vez creada la cuenta de almacenamiento, seleccione **Control de acceso (IAM)** en el panel de navegación izquierdo. Luego, asigne los siguientes roles o asegúrese de que ya están asignados. 

    a. * Asígnese el rol **Propietario** en la cuenta de almacenamiento b. * Asígnese el rol **Propietario de datos de Storage Blob** en la cuenta de almacenamiento

1. En el panel de navegación izquierdo, seleccione **Contenedores** y cree un contenedor. Puede asignarle cualquier nombre. Acepte el valor predeterminado de **Nivel de acceso público**. En este documento, llamaremos al contenedor `users`. Seleccione **Crear**. 

En el paso siguiente, configurará el área de trabajo de Synapse para usar esta cuenta de almacenamiento como su cuenta de almacenamiento "principal" y el contenedor para almacenar los datos del área de trabajo. El área de trabajo almacenará los datos en tablas de Apache Spark y en los registros de aplicaciones de Spark en esta cuenta, en una carpeta denominada `/synapse/workspacename`.

## <a name="create-a-synapse-workspace"></a>Creación de un área de trabajo de Synapse

* Abra [Azure Portal](https://portal.azure.com) y, en la parte superior, busque `Synapse`.
* En los resultados de la búsqueda, en **Servicios**, seleccione **Azure Synapse Analytics (versión preliminar de las áreas de trabajo)** .
* Seleccione **+ Agregar** para crear un área de trabajo con esta configuración.

    |Pestaña|Configuración | Valor sugerido | Descripción |
    |---|---|---|---|
    |Aspectos básicos|**Workspace name** (Nombre del área de trabajo)|Puede darle cualquier nombre.| En este documento, se utiliza `myworkspace`.|
    |Aspectos básicos|**Región**|Coincidencia de la región de la cuenta de almacenamiento|

1. En **Select Data Lake Storage Gen 2** (Seleccionar Data Lake Storage Gen 2), seleccione la cuenta y el contenedor que creó anteriormente.

1. Seleccione **Revisar + crear**. Seleccione **Crear**. El área de trabajo estará lista en unos minutos.

## <a name="verify-the-synapse-workspace-msi-has-access-to-the-storage-account"></a>Comprobación de que el MSI del área de trabajo de Synapse tiene acceso a la cuenta de almacenamiento

Es posible que ya se haya hecho. En cualquier caso, debería comprobarlo.

1. Abra [Azure Portal](https://portal.azure.com) y, después, la cuenta de almacenamiento principal elegida para el área de trabajo.
1. Seleccione **Control de acceso (IAM)** en el panel de navegación izquierdo. Luego, asigne los siguientes roles o asegúrese de que ya están asignados. 
    a. Asigne la identidad del área de trabajo al rol **Colaborador de datos de blobs de almacenamiento** de la cuenta de almacenamiento. La identidad del área de trabajo se llama igual que el área de trabajo. En este documento, el nombre del área de trabajo es `myworkspace` por lo que la identidad del área de trabajo es `myworkspaced`.
1. Seleccione **Guardar**.
    
## <a name="launch-synapse-studio"></a>Inicio de Synapse Studio

Una vez creada el área de trabajo de Synapse, hay dos maneras de abrir Synapse Studio:
* Abrir el área de trabajo de Synapse en [Azure Portal](https://portal.azure.com) y, en la parte superior de la sección **Información general**, seleccionar **Iniciar Synapse Studio**.
* Ir directamente a https://web.azuresynapse.net e iniciar sesión en el área de trabajo.

## <a name="create-a-sql-pool"></a>Creación de un grupo de SQL

1. En Synapse Studio, en el panel de navegación izquierdo, seleccione **Manage > SQL pools** (Administrar > Grupos de SQL).
1. Seleccione **+Nuevo** y especifique estos valores:

    |Configuración | Valor sugerido | 
    |---|---|
    |**Nombre del grupo de SQL**| `SQLDB1`|
    |**Nivel de rendimiento**|`DW100C`|

1. Seleccione **Revisar y crear** y, después, seleccione **Crear**.
1. El grupo de SQL estará listo en unos minutos. Cuando se cree el grupo de SQL, se asociará con una base de datos del grupo de SQL también denominada **SQLDB1**.

Los grupos de SQL consumen recursos facturables mientras están activos. Puede pausar los grupos más adelante para reducir los costos.

## <a name="create-an-apache-spark-pool"></a>Creación de un grupo de Apache Spark

1. En Synapse Studio, en el lado izquierdo, seleccione **Manage > Apache Spark pools** (Administrar > Grupos de Apache Spark).
1. Seleccione **+Nuevo** y especifique estos valores:

    |Configuración | Valor sugerido | 
    |---|---|
    |**Nombre del grupo de Apache Spark**|`Spark1`
    |**Tamaño del nodo**| `Small`|
    |**Número de nodos**| Establezca el valor mínimo en 3 y el máximo en 3|

1. Seleccione **Revisar y crear** y, después, seleccione **Crear**.
1. El grupo de Apache Spark estará listo en unos segundos.

> [!NOTE]
> A pesar del nombre, un grupo de Apache Spark no es como un grupo de SQL. Es solo un conjunto de metadatos básicos que se usan para informar al área de trabajo de Synapse cómo interactuar con Spark. 

Al ser metadatos, los grupos de Spark no se pueden iniciar ni detener. 

Cuando se realiza cualquier actividad de Spark en Synapse, se especifica el grupo de Spark que se va a usar. El grupo informa a Synapse del número de recursos de Spark que se van a usar. Solo paga por los recursos que se utilizan. Al dejar de usar el grupo de forma activa, los recursos agotarán el tiempo de espera y se reciclarán automáticamente.

> [!NOTE]
> Las bases de datos de Spark se crean de forma independiente de los grupos de Spark. Un área de trabajo siempre tiene una base de datos de Spark denominada **default** y se pueden crear bases de datos de Spark adicionales.

## <a name="the-sql-on-demand-pool"></a>El grupo de SQL a petición

Todas las áreas de trabajo incluyen un grupo precompilado y que no se puede eliminar llamado **SQL On-Demand**. El grupo de SQL On-Demand permite trabajar con SQL sin tener que crear ni pensar en la administración de un grupo de Synapse SQL. A diferencia de los otros tipos de grupos, la facturación de SQL On-Demand se basa en la cantidad de datos que se examinan para ejecutar la consulta (no en el número de recursos usados para ello).

* SQL On-Demand también tiene sus propias bases de datos de SQL On-Demand, que existen de forma independiente de cualquier grupo de SQL On-Demand.
* Actualmente, un área de trabajo siempre tiene exactamente un grupo de SQL On-Demand denominado **SQL On-Demand**.

## <a name="load-the-nyc-taxi-sample-data-into-the-sqldb1-database"></a>Carga de los datos del ejemplo de taxis de Nueva York en la base de datos SQLDB1

1. En Synapse Studio, en el menú azul de la parte superior, seleccione **?** . .
1. Seleccione **Getting started > Getting started hub** (Introducción > Introducción al centro).
1. En la tarjeta con la etiqueta **Query sample data** (Consultar datos de ejemplo), seleccione el grupo de SQL llamado `SQLDB1`.
1. Seleccione **Query data** (Consultar datos). Aparecerá la notificación "Loading sample data" (Cargando datos de ejemplo) y, posteriormente, desaparecerá.
1. Verá una barra de notificación de color azul claro cerca de la parte superior de Synapse Studio que indica que se están cargando los datos en SQLDB1. Cuando se vuelva verde descártela.

## <a name="explore-the-nyc-taxi-data-in-the-sql-pool"></a>Exploración de los datos de taxis de Nueva York en el grupo de SQL

1. En Synapse Studio, vaya al centro **Data** (Datos).
1. Vaya a **SQLDB1 > Tables** (Tablas). Verá que se han cargado varias tablas.
1. Haga clic con el botón derecho en la tabla **dbo.Trip** y seleccione **New SQL Script > Select TOP 100 Rows** (Nuevo script SQL > Seleccionar 100 primeras filas).
1. Se creará un nuevo script SQL y se ejecutará automáticamente.
1. Observe que en la parte superior del script SQL, en **Connect to** (Conectar a) está establecido automáticamente en el grupo de SQL llamado `SQLDB1`.
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

1. En esta consulta se muestra la relación entre las distancias recorridas totales y la distancia media recorrida, y el número de pasajeros.
1. En la ventana de resultados del script de SQL, cambie el valor de **View** (Vista) a **Chart** (Gráfico) para ver los resultados en un gráfico de líneas.

## <a name="load-the-nyc-taxi-sample-data-into-the-spark-nyctaxi-database"></a>Cargue los datos del ejemplo de los taxis de Nueva York en la base de datos nyctaxi de Spark

Los datos están disponibles en una tabla en `SQLDB1`. Ahora se cargan en una base de datos de Spark denominada `nyctaxi`.

1. En Synapse Studio, vaya al centro **Develop** (Desarrollar).
1. Seleccione **+** y seleccione **Notebook**.
1. En la parte superior del cuaderno, establezca el valor de **Attach to**  (Adjuntar a) en `Spark1`.
1. Seleccione **Add code** (Agregar código) para agregar una celda de código del cuaderno y pegue el siguiente texto:

    ```scala
    %%spark
    spark.sql("CREATE DATABASE IF NOT EXISTS nyctaxi")
    val df = spark.read.sqlanalytics("SQLDB1.dbo.Trip") 
    df.write.mode("overwrite").saveAsTable("nyctaxi.trip")
    ```

1. Vaya al centro **Data** (Datos), haga clic con el botón derecho en **Databases** (Bases de datos) y seleccione **Refresh** (Actualizar).
1. Debería ver estas bases de datos:
    - SQLDB1 (grupo de SQL)
    - nyctaxi (Spark)
      
## <a name="analyze-the-nyc-taxi-data-using-spark-and-notebooks"></a>Análisis de los datos de los taxis de Nueva York mediante Spark y cuadernos

1. Vuelva al cuaderno.
1. Cree una nueva celda de código, escriba el texto siguiente y ejecute la celda para obtener ejemplos de los datos de los taxis de Nueva York que se han cargado en la base de datos de Spark `nyctaxi`.

   ```py
   %%pyspark
   df = spark.sql("SELECT * FROM nyctaxi.trip") 
   display(df)
   ```

1. Ejecute el código siguiente para realizar el mismo análisis que hicimos anteriormente con el grupo de SQL `SQLDB1`. Este código también guarda los resultados del análisis en una tabla denominada `nyctaxi.passengercountstats` y visualiza los resultados.

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
 
## <a name="customize-data-visualization-data-with-spark-and-notebooks"></a>Personalización de datos de visualización de datos con Spark y cuadernos

Con los cuadernos, se puede controlar cómo se representan los gráficos. El código siguiente muestra un ejemplo sencillo en el que se usan las conocidas bibliotecas `matplotlib` y `seaborn`. Se representará el mismo tipo de gráfico de líneas que vio al ejecutar las consultas SQL anteriores.

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

Antes copiamos datos de una tabla de grupos de SQL `SQLDB1.dbo.Trip` en una tabla de Spark `nyctaxi.trip`. Luego, mediante Spark, agregamos los datos a la tabla de Spark `nyctaxi.passengercountstats`. Ahora copiaremos los datos de `nyctaxi.passengercountstats` en una tabla de grupos de SQL llamada `SQLDB1.dbo.PassengerCountStats`. 

Ejecute la celda siguiente en el cuaderno. Volverá a copiar la tabla de Spark agregada en la tabla de grupos de SQL.

```scala
%%spark
val df = spark.sql("SELECT * FROM nyctaxi.passengercountstats")
df.write.sqlanalytics("SQLDB1.dbo.PassengerCountStats", Constants.INTERNAL )
```

## <a name="analyze-nyc-taxi-data-in-spark-databases-using-sql-on-demand"></a>Análisis de los datos de taxis de Nueva York en las bases de datos de Spark mediante SQL On-Demand 

SQL On-Demand ve y puede consultar automáticamente las tablas de las bases de datos de Spark.

1. En Synapse Studio, vaya al centro **Develop** (Desarrollar) y cree un script de SQL.
1. En **Connect to** (Conectarse a) seleccione **SQL On-Demand**. 
1. Pegue el siguiente texto en el script y ejecute el script.

    ```sql
    SELECT *
    FROM nyctaxi.dbo.passengercountstats
    ```
    > [!NOTE]
    > La primera vez que se ejecuta una consulta que usa SQL On-Demand, se tardarán unos 10 segundos en recopilar los recursos de SQL necesarios para ejecutar las consultas. Las consultas posteriores no requerirán este tiempo, por lo que serán mucho más rápidas.
  
## <a name="orchestrate-activities-with-pipelines"></a>Organización de actividades con canalizaciones

Puede organizar una gran variedad de tareas en Azure Synapse. En esta sección, verá lo fácil que es.

1. En Synapse Studio, vaya al centro **Orchestrate** (Organización).
1. Seleccione **+** y, después, seleccione **Pipeline** (Canalización). Se creará una canalización.
1. Vaya al centro Develop (Desarrollar) y busque el cuaderno que creó anteriormente.
1. Arrastre el cuaderno a la canalización.
1. En la canalización, seleccione **Add trigger > New/edit** (Agregar desencadenador > Nuevo/editar).
1. En **Choose trigger** (Elegir desencadenador), seleccione **New** (Nuevo) y, después, en el apartado en que se elige la periodicidad, configure el desencadenador para que se ejecute cada hora.
1. Seleccione **Aceptar**.
1. Seleccione **Publish All** (Publicar todo) y la canalización se ejecutará cada hora.
1. Si desea que la canalización se ejecute ahora sin esperar a la próxima hora, seleccione **Add trigger > New/edit** (Agregar desencadenador > Nuevo/editar).

## <a name="working-with-data-in-a-storage-account"></a>Manipulación de los datos en una cuenta de almacenamiento

Hasta ahora, hemos explicado escenarios en los que los datos residían en las bases de datos del área de trabajo. Ahora mostraremos cómo trabajar con archivos que residen en cuentas de almacenamiento. En este escenario, se usará la cuenta de almacenamiento principal del área de trabajo y el contenedor que se especificó al crear el área de trabajo.

* El nombre de la cuenta de almacenamiento: `contosolake`
* El nombre del contenedor en la cuenta de almacenamiento: `users`

### <a name="creating-csv-and-parquet-files-in-your-storage-account"></a>Creación de archivos CSV y Parquet en la cuenta de almacenamiento

Ejecute el siguiente código en un cuaderno. Crea un archivo .csv y un archivo Parquet en la cuenta de almacenamiento

```py
%%pyspark
df = spark.sql("SELECT * FROM nyctaxi.passengercountstats")
df = df.repartition(1) # This ensure we'll get a single file during write()
df.write.mode("overwrite").csv("/NYCTaxi/PassengerCountStats.csv")
df.write.mode("overwrite").parquet("/NYCTaxi/PassengerCountStats.parquet")
```

### <a name="analyzing-data-in-a-storage-account"></a>Análisis de datos en una cuenta de almacenamiento

1. En Synapse Studio, vaya al centro **Data** (Datos).
1. Seleccione **Linked** (Vinculado).
1. Vaya a **Storage accounts > myworkspace (Primary - contosolake)** [Cuentas de almacenamiento > myworkspace (Principal: contosolake)].
1. Seleccione **users (Primary)"** [usuarios (Principal)].
1. Debería ver una carpeta denominada `NYCTaxi`. Dentro de ella debería ver dos carpetas `PassengerCountStats.csv` y `PassengerCountStats.parquet`.
1. Vaya a la carpeta `PassengerCountStats.parquet`.
1. Haga clic con el botón derecho en el archivo `.parquet` de dentro y seleccione **Nuevo cuaderno**. Se creará un cuaderno con una celda similar a esta:

    ```py
    %%pyspark
    data_path = spark.read.load('abfss://users@contosolake.dfs.core.windows.net/NYCTaxi/PassengerCountStats.parquet/part-00000-1f251a58-d8ac-4972-9215-8d528d490690-c000.snappy.parquet', format='parquet')
    data_path.show(100)
    ```

1. Ejecute la celda.
1. Haga clic con el botón derecho en el archivo Parquet de dentro y seleccione **New SQL script > SELECT TOP 100 rows** (Nuevo script de SQL > SELECCIONAR 100 PRIMERAS filas), y se creará un script de SQL como el siguiente:

    ```sql
    SELECT TOP 100 *
    FROM OPENROWSET(
        BULK 'https://contosolake.dfs.core.windows.net/users/NYCTaxi/PassengerCountStats.parquet/part-00000-1f251a58-d8ac-4972-9215-8d528d490690-c000.snappy.parquet',
        FORMAT='PARQUET'
    ) AS [r];
    ```
    
1. En el script, el campo **Attach to** (Adjuntar a) se establecerá en **SQL On-Demand**.
1. Ejecute el script.

## <a name="visualize-data-with-power-bi"></a>Visualización de datos con Power BI

A partir de los datos de taxis de Nueva York creamos conjuntos de datos agregados en dos tablas:
* `nyctaxi.passengercountstats`
* `SQLDB1.dbo.PassengerCountStats`

Puede vincular un área de trabajo de Power BI a su área de trabajo de Synapse. Esto le permite obtener fácilmente datos en el área de trabajo de Power BI y editar los informes de Power BI directamente en el área de trabajo de Synapse.

### <a name="create-a-power-bi-workspace"></a>Creación de un área de trabajo de Power BI

1. Inicie sesión en [powerbi.microsoft.com](https://powerbi.microsoft.com/).
1. Cree una nueva área de trabajo de Power BI denominada `NYCTaxiWorkspace1`.

### <a name="link-your-synapse-workspace-to-your-new-power-bi-workspace"></a>Vinculación de un área de trabajo de Synapse a un área de trabajo de Power BI

1. En Synapse Studio, vaya a **Manage > Linked Services** (Administrar > Servicios vinculados).
1. Seleccione **+ New** (+Nuevo) y seleccione **Connect to Power BI** (Conectarse a Power BI) y configure estos campos:

    |Configuración | Valor sugerido | 
    |---|---|
    |**Nombre**|`NYCTaxiWorkspace1`|
    |**Workspace name** (Nombre del área de trabajo)|`NYCTaxiWorkspace1`|
        
1. Seleccione **Crear**.

### <a name="create-a-power-bi-dataset-that-uses-data-in-your-synapse-workspace"></a>Creación de un conjunto de datos de Power BI que use los datos del área de trabajo de Synapse

1. En Synapse Studio, vaya a **Develop > Power BI** (Desarrollar > Power BI).
1. Vaya a **NYCTaxiWorkspace1 > Power BI datasets** (NYCTaxiWorkspace1 > Conjuntos de datos de Power BI) y seleccione **New Power BI dataset** (Nuevo conjunto de datos de Power BI).
1. Mantenga el puntero sobre la base de datos `SQLDB1` y seleccione **Download .pbids file** (Descargar archivo .pbids).
1. Abra el archivo `.pbids` que ha descargado. 
1. Se iniciará Power BI Desktop y se conectará automáticamente a `SQLDB1` en el área de trabajo de Synapse.
1. Si aparece un cuadro de diálogo denominado **SQL server database** (Base de datos de SQL Server): a. Seleccione **Microsoft account** (Cuenta de Microsoft). 
    b. Seleccione **Sign-in** (Inicio de sesión) e inicie sesión.
    c. Seleccione **Conectar**.
1. Se abrirá el cuadro de diálogo **Navigator** (Navegador). Cuando lo haga, compruebe la tabla **PassengerCountStats** y seleccione **Load** (Cargar).
1. Aparecerá el cuadro de diálogo **Connection settings** (Configuración de conexión). Seleccione **DirectQuery** y, después, seleccione **OK** (Aceptar).
1. Seleccione el botón **Report** (Informe) de la izquierda.
1. Agregue **Line Chart** (Gráfico de líneas) al informe.
    a. Arrastre la columna **PassengerCount** a **Visualizations > Axis** (Visualizaciones > Eje) b. Arrastre las columnas **SumTripDistance** y **AvgTripDistance** a **Visualizations > Values** (Visualizaciones > Valores).
1. En la pestaña **Home** (Inicio), seleccione **Publish** (Publicar).
1. Se le preguntará si desea guardar los cambios. Seleccione **Guardar**.
1. Se le pedirá que elija un nombre de archivo. Elija `PassengerAnalysis.pbix` y seleccione **Save** (Guardar).
1. En el campo **Select a destination** (Seleccionar un destino), seleccione `NYCTaxiWorkspace1` y, después, haga clic en **Select** (Seleccionar).
1. Espere a que finalice la publicación.

### <a name="configure-authentication-for-your-dataset"></a>Configuración de la autenticación del conjunto de datos

1. Abra [powerbi.microsoft.com](https://powerbi.microsoft.com/) y seleccione **Iniciar sesión**.
1. A la izquierda, en **Áreas de trabajo**, seleccione el área de trabajo `NYCTaxiWorkspace1`.
1. Dentro del área de trabajo debería ver un conjunto de datos denominado `Passenger Analysis` y un informe denominado `Passenger Analysis`.
1. Mantenga el puntero sobre el conjunto de datos `PassengerAnalysis`, seleccione el icono con los tres puntos y seleccione **Configuración**.
1. En **Credenciales de origen de datos**, en **Método de autenticación**, seleccione **OAuth2** y, después, seleccione **Iniciar sesión**.

### <a name="edit-a-report-in-synapse-studio"></a>Edición de informes en Synapse Studio

1. Vuelva a Synapse Studio y seleccione **Close and refresh** (Cerrar y actualizar). 
1. Vaya al centro **Develop** (Desarrollar). 
1. Mantenga el puntero sobre **Power BI** y haga clic en el nodo **Power BI reports** (Informes de Power BI).
1. Ahora, en **Power BI** debería ver: a. * En **NYCTaxiWorkspace1 > Power BI datasets** (NYCTaxiWorkspace1 > Conjuntos de datos de Power BI), un nuevo conjunto de datos denominado **PassengerAnalysis**.
    b. * En **NYCTaxiWorkspace1 > Power BI reports** (NYCTaxiWorkspace1 > Informes de Power BI), un nuevo informe denominado **PassengerAnalysis**.
1. Seleccione el informe **PassengerAnalysis**. 
1. El informe se abrirá y podrá editarlo directamente en Synapse Studio.

## <a name="monitor-activities"></a>Actividad de supervisión

1. En Synapse Studio, vaya al centro Monitor (Supervisar).
1. En esta ubicación, puede ver un historial de todas las actividades que se realizan en el área de trabajo y las que están activas ahora.
1. Explore **Pipeline runs** (Ejecuciones de canalización), **Apache Spark applications** (Aplicaciones de Apache Spark) y **SQL requests** (Solicitudes SQL) y podrá ver lo que ya ha hecho en el área de trabajo.

## <a name="next-steps"></a>Pasos siguientes

Más información sobre [Azure Synapse Analytics (versión preliminar)](overview-what-is.md)


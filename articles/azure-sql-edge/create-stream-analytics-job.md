---
title: Creación de un trabajo de streaming de T-SQL en Azure SQL Edge (versión preliminar)
description: Más información sobre la creación de trabajos de Stream Analytics en Azure SQL Edge (versión preliminar)
keywords: ''
services: sql-database-edge
ms.service: sql-database-edge
ms.topic: conceptual
author: SQLSourabh
ms.author: sourabha
ms.reviewer: sstein
ms.date: 05/19/2020
ms.openlocfilehash: 5e0043ebba1a317dcc6798d6be74aac051d97012
ms.sourcegitcommit: bb0afd0df5563cc53f76a642fd8fc709e366568b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/19/2020
ms.locfileid: "83595394"
---
# <a name="create-stream-analytics-job-in-azure-sql-edge-preview"></a>Creación de un trabajo de Stream Analytics en Azure SQL Edge (versión preliminar) 

En este artículo se explica cómo crear un trabajo de streaming de T-SQL en Azure SQL Edge (versión preliminar). Para crear un trabajo de streaming en SQL Edge, se deben llevar a cabo los pasos siguientes:

1. Creación de los objetos de entrada y salida de transmisión externa
2. Defina la consulta de trabajo de streaming como parte de la creación del trabajo de streaming.

## <a name="configure-an-external-stream-input-and-output-object"></a>Configuración de un objeto de entrada y salida de transmisión externa

El streaming de T-SQL utiliza la funcionalidad de origen de datos externo de SQL Server para definir los orígenes de datos asociados a las entradas y salidas de transmisión externa del trabajo de streaming. Los siguientes comandos de T-SQL son necesarios para crear un objeto de entrada o salida de transmisión externa.

[CREATE EXTERNAL FILE FORMAT (Transact-SQL)](https://docs.microsoft.com/sql/t-sql/statements/create-external-file-format-transact-sql)

[CREATE EXTERNAL DATA SOURCE (Transact-SQL)](https://docs.microsoft.com/sql/t-sql/statements/create-external-data-source-transact-sql)

[CREATE EXTERNAL STREAM (Transact-SQL)](#example-create-an-external-stream-object-sql-database)

Además, en el caso de que se use SQL Edge (o SQL Server, Azure SQL) como transmisión de salida, será necesario el comando [CREATE DATABASE SCOPED CREDENTIAL (Transact-SQL)](https://docs.microsoft.com/sql/t-sql/statements/create-database-scoped-credential-transact-sql) de T-SQL para definir las credenciales de acceso a SQL Database.

### <a name="supported-input-and-output-stream-data-sources"></a>Orígenes de datos de transmisión de entrada y salida admitidos

Azure SQL Edge actualmente solo admite los siguientes orígenes de datos como entradas y salidas de transmisión.

| Tipo de origen de datos | Entrada | Output | Descripción |
|------------------|-------|--------|------------------|
| Centro de Azure IoT Edge | Y | Y | Origen de datos para leer y escribir datos de streaming en una centro de Azure IoT Edge. Para más información sobre el centro de Azure IoT Edge, consulte [Centro de IoT Edge](https://docs.microsoft.com/azure/iot-edge/iot-edge-runtime#iot-edge-hub).|
| SQL Database | N | Y | Conexión de origen de datos para escribir datos de streaming en SQL Database. SQL Database puede ser una base de datos local de SQL Edge o una instancia remota de SQL Server o Azure SQL Database.|
| Azure Blob Storage | N | Y | Origen de datos para escribir datos en un blob en una cuenta de almacenamiento de Azure. |
| Kafka | Y | N | Origen de datos para leer datos de streaming de un tema de Kafka. Este adaptador solo está disponible actualmente para la versión Intel/AMD de Azure SQL Edge y no está disponible para la versión ARM64 de SQL Edge.|

### <a name="example-create-an-external-stream-inputoutput-object-for-azure-iot-edge-hub"></a>Ejemplo: Creación de un objeto de entrada o salida de transmisión externa para el centro de Azure IoT Edge

En el ejemplo siguiente se crea un objeto de transmisión externa para el centro de Edge. Para crear un origen de datos de entrada/salida de transmisión externa para el centro de Azure IoT Edge, primero debe crear un formato de archivo externo para SQL con el fin de comprender también el diseño de los datos que se leen o escriben.

1. Cree un formato de archivo externo con el tipo de formato JSON.

    ```sql
    Create External file format InputFileFormat
    WITH (  
       format_type = JSON,
    )
    go
    ```

2. Cree un origen de datos externo para el centro de IoT Edge. El siguiente script de T-SQL crea una conexión de origen de datos a un centro de Edge que se ejecuta en el mismo host de Docker que SQL Edge.

    ```sql
    CREATE EXTERNAL DATA SOURCE EdgeHubInput WITH (
    LOCATION = 'edgehub://'
    )
    go
    ```

3. Cree el objeto de transmisión externa para el centro de IoT Edge. El script de T-SQL siguiente crea un objeto de transmisión para el centro de Edge. En el caso de un objeto de transmisión del centro de Edge, el parámetro LOCATION es el nombre del tema o canal del centro de Edge en el que se lee o se escribe.

    ```sql
    CREATE EXTERNAL STREAM MyTempSensors WITH (
    DATA_SOURCE = EdgeHubInput,
    FILE_FORMAT = InputFileFormat,
    LOCATION = N'TemperatureSensors',
    INPUT_OPTIONS = N'',
    OUTPUT_OPTIONS = N''
    )
    go
    ```

### <a name="example-create-an-external-stream-object-sql-database"></a>Ejemplo: Creación de una instancia de SQL Database de objetos de transmisión externos

En el ejemplo siguiente se crea un objeto de transmisión externo a la base de datos local de SQL Edge. 

1. Cree una clave maestra en la base de datos. Esto es necesario para cifrar el secreto de credencial.

    ```sql
    CREATE MASTER KEY ENCRYPTION BY PASSWORD = '<<Strong_Password_For_Master_Key_Encryption>>';
    ```

2. Cree una credencial de ámbito de base de datos para acceder al origen de SQL Server. En el ejemplo siguiente se crea una credencial para el origen de datos externo con IDENTITY = 'username' y SECRET = 'password'.

    ```sql
    CREATE DATABASE SCOPED CREDENTIAL SQLCredential
    WITH IDENTITY = '<SQL_Login>', SECRET = '<SQL_Login_PASSWORD>'
    go
    ```

3. Cree un origen de datos externo con CREATE EXTERNAL DATA SOURCE. En el ejemplo siguiente:

    * Se crea un origen de datos externo denominado LocalSQLOutput.
    * Se identifica el origen de datos externo (LOCATION = '<vendor>://<server>[:<port>]'). En el ejemplo, se apunta a una instancia local de SQL Edge.
    * Por último, en el ejemplo se usa la credencial creada antes.

    ```sql
    CREATE EXTERNAL DATA SOURCE LocalSQLOutput WITH (
    LOCATION = 'sqlserver://tcp:.,1433'
    ,CREDENTIAL = SQLCredential
    )
    go
    ```

4. Cree el objeto de transmisión externo. En el ejemplo siguiente se crea un objeto de transmisión externo que apunta a una tabla *dbo.TemperatureMeasurements* en la base de datos *MySQLDatabase*.

    ```sql
    CREATE EXTERNAL STREAM TemperatureMeasurements WITH (
    DATA_SOURCE = LocalSQLOutput,
    LOCATION = N'MySQLDatabase.dbo.TemperatureMeasurements',
    INPUT_OPTIONS = N'',
    OUTPUT_OPTIONS = N''
    )
    ```

## <a name="create-the-streaming-job-and-the-streaming-queries"></a>Creación del trabajo de streaming y las consultas de streaming

Use el procedimiento almacenado del sistema **sys.sp_create_streaming_job** para definir las consultas de streaming y crear el trabajo de streaming. El procedimiento almacenado **sp_create_streaming_job** toma dos parámetros

- job_name: nombre del trabajo de streaming. Los nombres de los trabajos de streaming son únicos en la instancia.
- statement: instrucciones de consultas de streaming basadas en el [lenguaje de consulta de Stream Analytics](https://docs.microsoft.com/stream-analytics-query/stream-analytics-query-language-reference?).

En el ejemplo siguiente se crea un trabajo de streaming simple con una consulta de streaming. Esta consulta lee las entradas del centro de Edge y escribe en el objeto *dbo.TemperatureMeasurements* de la base de datos.

```sql
EXEC sys.sp_create_streaming_job @name=N'StreamingJob1',
@statement= N'Select * INTO TemperatureMeasurements from MyEdgeHubInput'
```

En el ejemplo siguiente se crea un trabajo de streaming más complejo con varias consultas diferentes, incluida una consulta que utiliza la función integrada AnomalyDetection_ChangePoint para identificar anomalías en los datos de temperatura.

```sql
EXEC sys.sp_create_streaming_job @name=N'StreamingJob2', @statement=
N' Select * INTO TemperatureMeasurements1 from MyEdgeHubInput1

Select * Into TemperatureMeasurements2 from MyEdgeHubInput2

Select * Into TemperatureMeasurements3 from MyEdgeHubInput3

SELECT
Timestamp as [Time],
[Temperature] As [Temperature],
GetRecordPropertyValue(AnomalyDetection_ChangePoint(Temperature, 80, 1200) OVER(LIMIT DURATION(minute, 20)), ''Score'') as ChangePointScore,
GetRecordPropertyValue(AnomalyDetection_ChangePoint(Temperature, 80, 1200) OVER(LIMIT DURATION(minute, 20)), ''IsAnomaly'') as IsChangePointAnomaly,
INTO TemperatureAnomalies FROM MyEdgeHubInput2;
'
go
```

## <a name="start-stop-drop-and-monitor-streaming-jobs"></a>Inicio, detención, abandono y supervisión de trabajos de streaming

Para iniciar un trabajo de streaming en SQL Edge, ejecute el procedimiento almacenado **sys.sp_start_streaming_job**. El procedimiento almacenado requiere que se inicie el mismo trabajo de streaming como entrada.

```sql
exec sys.sp_start_streaming_job @name=N'StreamingJob1'
go
```

Para detener un trabajo de streaming en SQL Edge, ejecute el procedimiento almacenado **sys.sp_stop_streaming_job**. El procedimiento almacenado requiere que se detenga el mismo trabajo de streaming como entrada.

```sql
exec sys.sp_stop_streaming_job @name=N'StreamingJob1'
go
```

Para abandonar (o eliminar) un trabajo de streaming en SQL Edge, ejecute el procedimiento almacenado **sys.sp_drop_streaming_job**. El procedimiento almacenado requiere que se abandone el mismo trabajo de streaming como entrada.

```sql
exec sys.sp_drop_streaming_job @name=N'StreamingJob1'
go
```

Para obtener el estado actual de un trabajo de streaming en SQL Edge, ejecute el procedimiento almacenado **sys.sp_get_streaming_job**. El procedimiento almacenado requiere que se abandone el mismo del trabajo de streaming como entrada, y genera el nombre y el estado actual del trabajo de streaming.

```sql
exec sys.sp_get_streaming_job @name=N'StreamingJob1'
        WITH RESULT SETS
(
       (
       name nvarchar(256),
       status nvarchar(256)
       )
)
```

El trabajo de streaming puede estar en cualquiera de los siguientes estados:

| Status | Descripción |
|--------| ------------|
| Creado | El trabajo de streaming se ha creado, pero aún no se ha iniciado. |
| Iniciando | El trabajo de streaming se encuentra en la fase de inicio. |
| Inactivo | El trabajo de streaming se está ejecutando, pero no hay ninguna entrada para procesar. |
| Processing | El trabajo de streaming se está ejecutando y está procesando entradas. Este estado indica un estado de mantenimiento del trabajo de streaming. |
| Degradado | El trabajo de streaming se está ejecutando, pero se produjeron errores de serialización o deserialización de entrada o salida no graves durante el procesamiento de entrada. El trabajo de entrada seguirá ejecutándose, pero quitará las entradas que encuentren errores. |
| Detenido | El trabajo de streaming se ha detenido. |
| Con error | Error del trabajo de streaming. Suele indicar un error grave durante el procesamiento. |

## <a name="next-steps"></a>Pasos siguientes

- [Visualización de los metadatos asociados a los trabajos de streaming en Azure SQL Edge (versión preliminar)](streaming-catalog-views.md) 
- [Creación de una transmisión externa](create-external-stream-transact-sql.md)

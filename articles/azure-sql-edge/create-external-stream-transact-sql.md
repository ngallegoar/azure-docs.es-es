---
title: 'CREATE EXTERNAL STREAM (Transact-SQL): Azure SQL Edge (versión preliminar)'
description: Más información sobre la instrucción CREATE EXTERNAL STREAM en Azure SQL Edge (versión preliminar)
keywords: ''
services: sql-edge
ms.service: sql-edge
ms.topic: conceptual
author: SQLSourabh
ms.author: sourabha
ms.reviewer: sstein
ms.date: 05/19/2020
ms.openlocfilehash: 9e1f672a62ee7687fec9cea96ca03240c893ba95
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "84233329"
---
# <a name="create-external-stream-transact-sql"></a>CREATE EXTERNAL STREAM (Transact-SQL)

El objeto EXTERNAL STREAM tiene una finalidad doble de entrada y salida. Se puede usar como entrada para consultar datos de streaming de servicios de ingesta de eventos, como Azure Event Hub o IoT Hub, o bien como salida para especificar dónde y cómo almacenar los resultados de una consulta de streaming.

También se puede especificar y crear un parámetro EXTERNAL STREAM como salida y como entrada para servicios como Event Hubs o Blob Storage. Se trata de escenarios de encadenamiento en los que una consulta de streaming conserva los resultados en la transmisión externa como salida y otra consulta de streaming los lee de la misma transmisión externa como entrada. 


## <a name="syntax"></a>Sintaxis

```sql
CREATE EXTERNAL STREAM {external_stream_name}  
(column definition [,column definitions ] * ) --Used for input - optional 
WITH  
(  
  DATA_SOURCE = <data_source_name>, 
  LOCATION = <location_name>, 
  EXTERNAL_FILE_FORMAT = <external_file_format_name>, --Used for input - optional 
   
  INPUT_OPTIONS =  
    ‘CONSUMER_GROUP=<consumer_group_name>; 
    ‘TIME_POLICY=<time_policy>; 
    LATE_EVENT_TOLERANCE=<late_event_tolerance_value>; 
    OUT_OF_ORDER_EVENT_TOLERANCE=<out_of_order_tolerance_value> 
     
    /* Edge options */ 
  , 
  OUTPUT_OPTIONS =  
    ‘REJECT_POLICY=<reject_policy>; 
    MINIMUM_ROWS=<row_value>; 
    MAXIMUM_TIME=<time_value_minutes>; 
    PARTITION_KEY_COLUMN=<partition_key_column_name>; 
    PROPERTY_COLUMNS=(); 
    SYSTEM_PROPERTY_COLUMNS=(); 
    PARTITION_KEY=<partition_key_name>; 
    ROW_KEY=<row_key_name>; 
    BATCH_SIZE=<batch_size_value>; 
    MAXIMUM_BATCH_COUNT=<batch_value>;  
    STAGING_AREA=<blob_data_source>’ 
     
    /* Edge options */ TAGS=<tag_column_value> 
); 
```


## <a name="arguments"></a>Argumentos


- [EXTERNAL DATA SOURCE](/sql/t-sql/statements/create-external-data-source-transact-sql/)
- [EXTERNAL FILE FORMAT](/sql/t-sql/statements/create-external-file-format-transact-sql/)
- **LOCATION**: especifica el nombre de los datos o la ubicación reales en el origen de datos. En el caso de un objeto de transmisión del centro de Edge o de Kafka, la ubicación especifica el nombre del centro de Edge o el tema Kafka del que se va a leer o en el que se va a escribir.
- **INPUT_OPTIONS**: especifique opciones como pares clave-valor para servicios como Event Hubs e IoT Hub que sean entradas para las consultas de streaming.
    - CONSUMER_GROUP: Event Hubs e IoT Hub limitan el número de lectores dentro de un grupo de consumidores (a 5). Si deja este campo en blanco se usará el grupo de consumidores "$Default".
      - Se aplica a Event Hubs e IoT Hub.
    - TIME_POLICY: describe si se deben quitar eventos o ajustar la hora del evento cuando eventos con demora o eventos desordenados superen el valor de tolerancia.
      - Se aplica a Event Hubs e IoT Hub.
    - LATE_EVENT_TOLERANCE: retraso máximo aceptable. El retraso representa la diferencia entre la marca de tiempo del evento y el reloj del sistema.
      - Se aplica a Event Hubs e IoT Hub.
    - OUT_OF_ORDER_EVENT_TOLERANCE: Los eventos pueden llegar desordenados después de haber realizado el viaje desde la entrada a la consulta de streaming. Estos eventos se pueden aceptar tal cual, o puede optar por pausarlos durante un período establecido para reordenarlos.
      - Se aplica a Event Hubs e IoT Hub.
- **OUTPUT_OPTIONS**: especifique opciones como pares clave-valor para los servicios admitidos que sean entradas para las consultas de streaming. 
  - REJECT_POLICY:  DROP | RETRY especifica directivas de control de errores de datos cuando se producen errores de conversión de datos. 
    - Se aplica a todas las salidas admitidas. 
  - MINIMUM_ROWS:  
    Filas mínimas requeridas por lote escrito en una salida. En el caso de Parquet, cada lote creará un archivo. 
    - Se aplica a todas las salidas admitidas. 
  - MAXIMUM_TIME:  
    Tiempo de espera máximo por lote. Después de este tiempo, el lote se escribirá en la salida aunque no se cumpla el requisito de filas mínimas. 
    - Se aplica a todas las salidas admitidas. 
  - PARTITION_KEY_COLUMN:  
    columna que se usa para la clave de partición. 
    - Se aplica a Event Hubs y al tema de Service Bus. 
  - PROPERTY_COLUMNS:  
    lista separada por comas de los nombres de las columnas de salida que se adjuntarán a los mensajes como propiedades personalizadas, si se proporcionan.  
    - Se aplica a Event Hubs y al tema y a la cola de Service Bus. 
  - SYSTEM_PROPERTY_COLUMNS:  
    colección con formato JSON de pares nombre-valor de los nombres de propiedades del sistema y columnas de salida que se rellenarán en los mensajes de Service Bus. P. ej.: { "MessageId": "column1", "PartitionKey": "column2"}. 
    - Se aplica al tema y a la cola de Service Bus. 
  - PARTITION_KEY:  
    El nombre de la columna de salida que contiene la clave de partición. La clave de partición es un identificador único de la partición dentro de una tabla determinada que constituye la primera parte de la clave principal de la entidad. Es un valor de cadena que puede tener un tamaño de hasta 1 KB. 
    - Se aplica a Table Storage y a Azure Functions. 
  - ROW_KEY:  
    El nombre de la columna de salida que contiene la clave de la fila. La clave de fila es un identificador único de una identidad dentro de una partición determinada. Forma la segunda parte de la clave principal de la entidad. La clave de fila es un valor de cadena que puede tener un tamaño de hasta 1 KB. 
    - Se aplica a Table Storage y a Azure Functions. 
  - BATCH_SIZE:  
    representa el número de transacciones de Table Storage donde el máximo puede ascender a 100 registros. Para Azure Functions, representa el tamaño del lote en bytes enviados a la función por llamada; el valor predeterminado es 256 kB. 
    - Se aplica a Table Storage y a Azure Functions. 
  - MAXIMUM_BATCH_COUNT:  
    número máximo de eventos enviados a la función por cada llamada para Azure Functions. El valor predeterminado es de 100. Para SQL Database, representa el número máximo de registros que se envía con cada transacción de inserción masiva. El valor predeterminado es 10 000. 
    - Se aplica a SQL Database y Azure Functions. 
  - STAGING_AREA: objeto EXTERNAL DATA SOURCE para Blob Storage. Área de almacenamiento provisional para la ingesta de datos de alto rendimiento en SQL Data Warehouse. 
    - Se aplica a SQL Data Warehouse.


## <a name="examples"></a>Ejemplos

### <a name="example-1---edgehub"></a>Ejemplo 1: centro de Edge

Escriba:  Entrada o salida<br>
Parámetros:
- Entrada o salida
  - Alias 
  - Formato de serialización de eventos 
  - Encoding 
- Solo entrada: 
  - Tipo de compresión de eventos 

Sintaxis:

```sql
CREATE EXTERNAL DATA SOURCE MyEdgeHub 
WITH  
(      
  LOCATION = 'edgehub://'       
); 
 
CREATE EXTERNAL FILE FORMAT myFileFormat  
WITH (  
   FORMAT_TYPE = 'JSON', 
); 
 
CREATE EXTERNAL STREAM Stream_A  
WITH    
(   
   DATA_SOURCE = MyEdgeHub, 
   EXTERNAL_FILE_FORMAT = myFileFormat, 
   LOCATION = ‘<mytopicname>’, 
   OUTPUT_OPTIONS =   
     ‘REJECT_TYPE: Drop’ 
);
```


### <a name="example-2---azure-sql-database-azure-sql-edge-sql-server"></a>Ejemplo 2: Azure SQL Database, Azure SQL Edge, SQL Server

Escriba:  Output<br>
Parámetros:
- Alias de salida  
- Base de datos (necesaria para SQL Database) 
- Servidor (necesario para SQL Database) 
- Nombre de usuario (necesario para SQL Database) 
- Contraseña (necesaria para SQL Database) 
- Tabla 
- Combinación de todas las particiones de entrada en una sola escritura o heredar el esquema de partición de la entrada o el paso de consulta anterior (necesario para SQL Database) 
- Número máximo de lotes 

Sintaxis:

```sql
CREATE DATABASE SCOPED CREDENTIAL SQLCredName 
WITH IDENTITY = '<user>’, 
SECRET = '<password>'; 
 
-- Azure SQL Database 
CREATE EXTERNAL DATA SOURCE MyTargetSQLTabl 
WITH 
(     
  LOCATION = ' <my_server_name>.database.windows.net’, 
  CREDENTIAL = SQLCredName 
); 
 
--SQL Server or Azure SQL Edge
CREATE EXTERNAL DATA SOURCE MyTargetSQLTabl 
WITH 
(     
  LOCATION = ' <sqlserver://<ipaddress>,<port>’, 
  CREDENTIAL = SQLCredName 
); 
 
--SQL Database/Edge 
CREATE EXTERNAL STREAM Stream_A 
WITH   
(  
    DATA_SOURCE = MyTargetSQLTable, 
    LOCATION = ‘<DatabaseName>.<SchemaName>.<TableName>’ 
   --Note: If table is container in the database, <TableName> should be sufficient 
   --Note: Do not need external file format in this case 
    EXTERNAL_FILE_FORMAT = myFileFormat,  
    OUTPUT_OPTIONS =  
      ‘REJECT_TYPE: Drop 
); 
```

### <a name="example-3---kafka"></a>Ejemplo 3: Kafka

Escriba:  Entrada<br>
Parámetros:

- Servidor de arranque de Kafka 
- Nombre del tema de Kafka 
- Recuento de particiones de origen 

Sintaxis:

```sql
CREATE EXTERNAL DATA SOURCE MyKafka_tweets 
WITH 
( 
  --The location maps to KafkaBootstrapServer 
  LOCATION = ' kafka://<kafkaserver>:<ipaddress>’, 
  CREDENTIAL = kafkaCredName 
 
); 
 
CREATE EXTERNAL FILE FORMAT myFileFormat 
WITH ( 
    FORMAT_TYPE = 'CSV', 
    DATA_COMPRESSION = 'GZIP', 
    ENCODING = ‘UTF-8’, 
    DELIMITER = ‘|’ 
); 
 
 
CREATE EXTERNAL STREAM Stream_A (user_id VARCHAR, tweet VARCHAR) 
WITH   
(  
    DATA_SOURCE = MyKafka_tweets, 
    LOCATION = ‘<KafkaTopicName>’, 
   --JSON: Format, CSV: Delimiter and Encoding, AVRO: None 
    EXTERNAL_FILE_FORMAT = myFileFormat,  
    INPUT_OPTIONS =  
      ‘PARTITIONS: 5’ 
); 
```

### <a name="example-4---blob-storage"></a>Ejemplo 4: Blob Storage

Escriba:  Entrada o salida<br>
Parámetros:
- Entrada o salida:
  - Alias 
  - Cuenta de almacenamiento 
  - Clave de cuenta de almacenamiento 
  - Contenedor 
  - Patrón de la ruta de acceso 
  - Formato de fecha 
  - Formato de hora 
  - Formato de serialización de eventos 
  - Encoding 
- Solo entrada: 
  - Particiones (entrada) 
  - Tipo de compresión de eventos (entrada) 
- Solo salida: 
  - Número mínimo de filas (salida) 
  - Tiempo máximo (salida) 
  - Modo de autenticación (salida) 

Sintaxis:

```sql
CREATE DATABASE SCOPED CREDENTIAL StorageAcctCredName 
WITH IDENTITY = 'Shared Access Signature’, 
SECRET = 'AccountKey'; 
 
CREATE DATABASE SCOPED CREDENTIAL StorageAcctCredNameMSI 
WITH IDENTITY = 'Managed Identity’; 
 
CREATE EXTERNAL DATA SOURCE MyBlobStorage_tweets 
WITH 
(     
  LOCATION = 'sb://my-sb-namespace.servicebus.windows.net’, 
  CREDENTIAL = eventHubCredName 
); 
 
CREATE EXTERNAL FILE FORMAT myFileFormat 
WITH ( 
    FORMAT_TYPE = 'CSV', --Event serialization format 
    DATE_FORMAT = 'YYYY/MM/DD HH', --Both Date and Time format 
    ENCODING = ‘UTF-8’ 
); 
 
CREATE EXTERNAL STREAM Stream_A (user_id VARCHAR, tweet VARCHAR) 
WITH   
(  
    DATA_SOURCE = MyBlobStorage_tweets, 
    LOCATION = ‘<path_pattern>’, 
    EXTERNAL_FILE_FORMAT = myFileFormat,  
    INPUT_OPTIONS =  
      ‘PARTITIONS: 1’, 
  
    OUTPUT_OPTIONS =  
      ‘REJECT_TYPE: Drop, 
      PARTITION_KEY_COLUMN: , 
      PROPERTY_COLUMNS: (), 
      MINUMUM_ROWS: 100000, 
      MAXIMUM_TIME: 60’ 
); 
```

### <a name="example-5---event-hub"></a>Ejemplo 5: Event Hubs

Escriba:  Entrada o salida<br>
Parámetros:
- Entrada o salida:
  - Alias 
  - Espacio de nombres de Service Bus 
  - Nombre del centro de eventos 
  - Nombre de la directiva del centro de eventos 
  - Clave de la directiva del Centro de eventos 
  - Formato de serialización de eventos 
  - Encoding 
- Solo entrada: 
  - Grupo de consumidores de Event Hubs 
  - Tipo de compresión de eventos 
- Solo salida: 
  - Columna de clave de partición 
  - Columnas de propiedades 

Sintaxis:

```sql
CREATE DATABASE SCOPED CREDENTIAL eventHubCredName 
WITH IDENTITY = 'Shared Access Signature’, 
SECRET = '<policyName>'; 
 
CREATE EXTERNAL DATA SOURCE MyEventHub_tweets 
WITH 
(     
  LOCATION = 'sb://my-sb-namespace.servicebus.windows.net’, 
  CREDENTIAL = eventHubCredName 
); 
 
CREATE EXTERNAL FILE FORMAT myFileFormat 
WITH ( 
    FORMAT_TYPE = 'CSV', 
    DATA_COMPRESSION = 'GZIP', 
    ENCODING = ‘UTF-8’, 
    DELIMITER = ‘|’ 
); 
 
 
CREATE EXTERNAL STREAM Stream_A (user_id VARCHAR, tweet VARCHAR) 
WITH   
(  
    DATA_SOURCE = MyEventHub_tweets, 
    LOCATION = ‘<topicname>’, 
   --JSON: Format, CSV: Delimiter and Encoding, AVRO: None 
    EXTERNAL_FILE_FORMAT = myFileFormat,  
 
    INPUT_OPTIONS =  
      ‘CONSUMER_GROUP: FirstConsumerGroup’, 
          
    OUTPUT_OPTIONS =  
      ‘REJECT_TYPE: Drop, 
      PARTITION_KEY_COLUMN: , 
      PROPERTY_COLUMNS: ()’ 
);
```

### <a name="example-6---iot-hub"></a>Ejemplo 6: IoT Hub

Escriba:  Entrada<br>
Parámetros:

- Alias de entrada 
- IoT Hub 
- Punto de conexión 
- Nombre de directiva de acceso compartido 
- Clave de directiva de acceso compartido 
- Grupo de consumidores 
- Formato de serialización de eventos 
- Encoding 
- Tipo de compresión de eventos 

Sintaxis:

```sql
CREATE DATABASE SCOPED CREDENTIAL IoTHubCredName 
WITH IDENTITY = 'Shared Access Signature’, 
SECRET = '<policyName>'; 
 
CREATE EXTERNAL DATA SOURCE MyIoTHub_tweets 
WITH 
(     
  LOCATION = ' iot://iot_hub_name.azure-devices.net’, 
  CREDENTIAL = IoTHubCredName 
);  

CREATE EXTERNAL FILE FORMAT myFileFormat 
WITH ( 
    FORMAT_TYPE = 'CSV', --Event serialization format 
    DATA_COMPRESSION = 'GZIP', 
    ENCODING = ‘UTF-8’ 
); 
 
CREATE EXTERNAL STREAM Stream_A (user_id VARCHAR, tweet VARCHAR) 
WITH   
(  
    DATA_SOURCE = MyIoTHub_tweets, 
    LOCATION = ‘<name>’, 
    EXTERNAL_FILE_FORMAT = myFileFormat,  
    INPUT_OPTIONS =  
      ‘ENDPOINT: Messaging, 
      CONSUMER_GROUP: ‘FirstConsumerGroup’ 
); 
```

### <a name="example-7---azure-synapse-analytics-formerly-sql-data-warehouse"></a>Ejemplo 7: Azure Synapse Analytics (anteriormente SQL Data Warehouse)

Escriba:  Output<br>
Parámetros:
- Alias de salida 
- Base de datos 
- Server 
- Nombre de usuario 
- Contraseña 
- Tabla 
- Área de almacenamiento provisional (para COPY) 

Sintaxis:

```sql
CREATE DATABASE SCOPED CREDENTIAL SQLCredName 
WITH IDENTITY = '<user>’, 
SECRET = '<password>'; 
 
CREATE EXTERNAL DATA SOURCE MyTargetSQLTable 
WITH 
(     
  LOCATION = ' <my_server_name>.database.windows.net’, 
  CREDENTIAL = SQLCredName 
); 
 
CREATE EXTERNAL STREAM MySQLTableOutput 
WITH ( 
   DATA_SOURCE = MyTargetSQLTable, 
   LOCATION = ‘<TableName>’ 
   --Note: Do not need external file format in this case 
   OUTPUT_OPTIONS =  
     ‘REJECT_TYPE: Drop, 
     STAGING_AREA: staging_area_data_source’, 
); 
```


### <a name="example-8---table-storage"></a>Ejemplo 8: Table Storage

Escriba:  Output<br>
Parámetros:
- Alias de salida 
- Cuenta de almacenamiento 
- Clave de cuenta de almacenamiento 
- Nombre de la tabla 
- Clave de partición 
- Clave de fila 
- Tamaño de lote 

Sintaxis:

```sql
CREATE DATABASE SCOPED CREDENTIAL TableStorageCredName 
WITH IDENTITY = ‘Storage account Key’, 
SECRET = '<storage_account_key>'; 
 
CREATE EXTERNAL DATA SOURCE MyTargetTableStorage 
WITH 
(     
  LOCATION = 'abfss://<storage_account>.dfs.core.windows.net’, 
  CREDENTIAL = TableStorageCredName 
); 
 
CREATE EXTERNAL STREAM MyTargetTableStorageOutput 
WITH ( 
   DATA_SOURCE = MyTargetTableStorage, 
   LOCATION = ‘<TableName>’, 
   OUTPUT_OPTIONS =  
     ‘REJECT_TYPE: Drop, 
     PARTITION_KEY: <column_partition_key>, 
     ROW_KEY: <column_row_key>, 
     BATCH_SIZE: 100’ 
); 
```


### <a name="example-9---service-bus-topic-same-properties-as-queue"></a>Ejemplo 9: tema de Service Bus (las mismas propiedades que la cola)

Escriba:  Output<br>
Parámetros:
- Alias de salida 
- Espacio de nombres de Service Bus 
- Nombre del tema 
- Nombre de la directiva de temas 
- Clave de la directiva de temas 
- Columnas de propiedades 
- Columnas de propiedades del sistema 
- Formato de serialización de eventos 
- Encoding 

Sintaxis:

```sql
CREATE DATABASE SCOPED CREDENTIAL serviceBusCredName 
WITH IDENTITY = 'Shared Access Signature’, 
SECRET = '<policyName>'; 
 
CREATE EXTERNAL DATA SOURCE MyServiceBus_tweets 
WITH 
(     
  LOCATION = 'sb://my-sb-namespace.servicebus.windows.net’, 
  CREDENTIAL = serviceBusCredName 
); 
 
CREATE EXTERNAL FILE FORMAT myFileFormat 
WITH ( 
    FORMAT_TYPE = 'CSV', --Event serialization format 
    DATA_COMPRESSION = 'GZIP', 
    ENCODING = ‘UTF-8’ 
); 
 
CREATE EXTERNAL STREAM MyServiceBusOutput 
WITH ( 
   DATA_SOURCE = MyServiceBus_tweets, 
   LOCATION = ‘<topic_name>’, 
   EXTERNAL_FILE_FORMAT = myFileFormat 
       OUTPUT_OPTIONS =  
     ‘REJECT_TYPE: Drop, 
     PARTITION_KEY_COLUMN: , 
     PROPERTY_COLUMNS: (), 
     SYSTEM_PROPERTY_COLUMNS: ()’ 
   --JSON: Format, CSV: Delimiter and Encoding, AVRO: None 
           
); 
```


### <a name="example-10---cosmos-db"></a>Ejemplo 10: Cosmos DB

Escriba:  Output<br>
Parámetros:
- Alias de salida 
- Identificador de cuenta 
- Clave de cuenta 
- Base de datos 
- Nombre del contenedor 
- Id. de documento 


Sintaxis:

```sql
CREATE DATABASE SCOPED CREDENTIAL cosmosDBCredName 
WITH IDENTITY = ‘Storage Account Key’, 
SECRET = '<accountKey>'; 
 
CREATE EXTERNAL DATA SOURCE MyCosmosDB_tweets 
WITH 
(     
  LOCATION = ' cosmosdb://accountid.documents.azure.com:443/ database’, 
  CREDENTIAL = cosmosDBCredName 
); 
 
CREATE EXTERNAL STREAM MyCosmosDBOutput 
WITH ( 
   DATA_SOURCE = MyCosmosDB_tweets, 
   LOCATION = ‘<container/documentID>’ 
   OUTPUT_OPTIONS =  
     ‘REJECT_TYPE: Drop', 
     --Note: Do not need external file format in this case 
          
);
```



### <a name="example-11---power-bi"></a>Ejemplo 11: Power BI

Escriba:  Output<br>
Parámetros:
- Alias de salida 
- Nombre del conjunto de datos 
- Nombre de la tabla 


Sintaxis:

```sql
CREATE DATABASE SCOPED CREDENTIAL PBIDBCredName 
WITH IDENTITY = ‘Managed Identity’; 
 
CREATE EXTERNAL DATA SOURCE MyPbi_tweets 
WITH 
( 
  LOCATION = 'pbi://dataset/’, 
  CREDENTIAL = PBIDBCredName 
 
); 
 
CREATE EXTERNAL STREAM MyPbiOutput 
WITH ( 
   DATA_SOURCE = MyPbi_tweets, 
   LOCATION = 'tableName', 
   OUTPUT_OPTIONS =  
     ‘REJECT_TYPE: Drop' 
        
);
```

### <a name="example-12---azure-function"></a>Ejemplo 12: Azure Functions

Escriba:  Output<br>
Parámetros:
- Aplicación de función 
- Función 
- Clave 
- Tamaño máximo de lote 
- Número máximo de lotes 

Sintaxis:

```sql
CREATE DATABASE SCOPED CREDENTIAL AzureFunctionCredName 
WITH IDENTITY = ‘Function Key’, 
SECRET = '<function_key>'; 
 
CREATE EXTERNAL DATA SOURCE MyTargetTableStorage 
WITH 
(     
  LOCATION = 'abfss://<storage_account>.dfs.core.windows.net’, 
  CREDENTIAL = TableStorageCredName 
); 
 
CREATE EXTERNAL STREAM MyTargetTableStorageOutput 
WITH ( 
   DATA_SOURCE = MyTargetTableStorage, 
   LOCATION = ‘<TableName>’, 
   OUTPUT_OPTIONS =  
     ‘REJECT_TYPE: 'Drop'      
     PARTITION_KEY: ‘<column_partition_key>, 
     ROW_KEY: <column_row_key>, 
     BATCH_SIZE: 100’ 
); 
```


## <a name="see-also"></a>Consulte también

- [ALTER EXTERNAL STREAM (Transact-SQL)](alter-external-stream-transact-sql.md) 
- [DROP EXTERNAL STREAM (Transact-SQL)](drop-external-stream-transact-sql.md) 


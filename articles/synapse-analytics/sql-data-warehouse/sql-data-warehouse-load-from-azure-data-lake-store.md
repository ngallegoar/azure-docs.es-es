---
title: Tutorial sobre la carga de datos de Azure Data Lake Storage
description: Use tablas externas de PolyBase para cargar datos de Azure Data Lake Storage para SQL de Synapse.
services: synapse-analytics
author: kevinvngo
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: ''
ms.date: 04/08/2020
ms.author: kevin
ms.reviewer: igorstan
ms.custom: azure-synapse
ms.openlocfilehash: f26aafc771998ea73d1a4f97f0e960a94f6775c3
ms.sourcegitcommit: 1895459d1c8a592f03326fcb037007b86e2fd22f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82626724"
---
# <a name="load-data-from-azure-data-lake-storage-for-sql-analytics"></a>Carga de datos desde Azure Data Lake Storage para SQL Analytics

En esta guía se indica cómo usar tablas externas de PolyBase para cargar datos de Azure Data Lake Storage. Aunque puede ejecutar consultas ad hoc en los datos almacenados en Data Lake Storage, se recomienda importar los datos para obtener un mejor rendimiento.

> [!NOTE]  
> Una alternativa a la carga es la [instrucción COPY](/sql/t-sql/statements/copy-into-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest) actualmente en versión preliminar pública.  La instrucción COPY proporciona más flexibilidad. Para realizar comentarios sobre la instrucción COPY, envíe un mensaje de correo electrónico a la siguiente lista de distribución: sqldwcopypreview@service.microsoft.com.
>
> [!div class="checklist"]
>
> * Cree los objetos de base de datos necesarios para cargar datos desde Data Lake Storage.
> * Conéctese al directorio de Data Lake Storage.
> * Carga de datos en un almacenamiento de datos.

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/) antes de empezar.

## <a name="before-you-begin"></a>Antes de empezar

Antes de completar este tutorial, descargue e instale la versión más reciente de [SQL Server Management Studio](/sql/ssms/download-sql-server-management-studio-ssms?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest) (SSMS).

Para ejecutar este tutorial, necesitará:

* Un grupo de SQL. Consulte [Creación de un grupo de SQL y consulta de datos](create-data-warehouse-portal.md).
* Una cuenta de Data Lake Storage. Consulte [Introducción a Azure Data Lake Storage](../../data-lake-store/data-lake-store-get-started-portal.md?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json). Para esta cuenta de almacenamiento, tendrá que configurar o especificar una de las siguientes credenciales para cargar: Una clave de cuenta de almacenamiento, un usuario de la aplicación de Azure Directory o un usuario de AAD que tenga el rol adecuado de control de acceso basado en rol para la cuenta de almacenamiento.

## <a name="create-a-credential"></a>Creación de una credencial

Puede ignorar esta sección y continuar con "Creación del origen de datos externo" al autenticar mediante el paso a través de AAD. No es necesario crear ni especificar una credencial con ámbito de base de datos al usar el paso a través de AAD, pero asegúrese de que el usuario de AAD tiene el rol de RBAC adecuado (los roles Lector de datos de blobs de almacenamiento, Colaborador o Propietario) en la cuenta de almacenamiento. [Aquí](https://techcommunity.microsoft.com/t5/Azure-SQL-Data-Warehouse/How-to-use-PolyBase-by-authenticating-via-AAD-pass-through/ba-p/862260) se destacan más detalles.

Para acceder a su cuenta de Data Lake Storage, debe crear una clave maestra de base de datos para cifrar su secreto de credencial. Luego, cree una credencial con ámbito de base de datos para almacenar el secreto. Al autenticarse con las entidades de servicio (el usuario de una aplicación de Azure Active Directory), la credencial con ámbito de base de datos almacena las credenciales de la entidad de servicio configuradas en AAD. También puede usar la credencial con ámbito de base de datos para almacenar la clave de la cuenta de almacenamiento de Gen2.

Para conectarse a Data Lake Storage con entidades de servicio, **primero** debe crear una aplicación de Azure Active Directory, crear una clave de acceso y conceder acceso a la aplicación a la cuenta de Data Lake Storage Gen1. Para obtener instrucciones, consulte [Autenticarse en Azure Data Lake Storage mediante Active Directory](../../data-lake-store/data-lake-store-service-to-service-authenticate-using-active-directory.md?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json).

Inicie sesión en el grupo de SQL con un usuario que tenga permisos de nivel de CONTROL y ejecute las siguientes instrucciones SQL en la base de datos:

```sql
-- A: Create a Database Master Key.
-- Only necessary if one does not already exist.
-- Required to encrypt the credential secret in the next step.
-- For more information on Master Key: https://docs.microsoft.com/sql/t-sql/statements/create-master-key-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest

CREATE MASTER KEY;


-- B (for service principal authentication): Create a database scoped credential
-- IDENTITY: Pass the client id and OAuth 2.0 Token Endpoint taken from your Azure Active Directory Application
-- SECRET: Provide your AAD Application Service Principal key.
-- For more information on Create Database Scoped Credential: https://docs.microsoft.com/sql/t-sql/statements/create-database-scoped-credential-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest

CREATE DATABASE SCOPED CREDENTIAL ADLSCredential
WITH
    -- Always use the OAuth 2.0 authorization endpoint (v1)
    IDENTITY = '<client_id>@<OAuth_2.0_Token_EndPoint>',
    SECRET = '<key>'
;

-- B (for Gen2 storage key authentication): Create a database scoped credential
-- IDENTITY: Provide any string, it is not used for authentication to Azure storage.
-- SECRET: Provide your Azure storage account key.

CREATE DATABASE SCOPED CREDENTIAL ADLSCredential
WITH
    IDENTITY = 'user',
    SECRET = '<azure_storage_account_key>'
;

-- It should look something like this when authenticating using service principal:
CREATE DATABASE SCOPED CREDENTIAL ADLSCredential
WITH
    IDENTITY = '536540b4-4239-45fe-b9a3-629f97591c0c@https://login.microsoftonline.com/42f988bf-85f1-41af-91ab-2d2cd011da47/oauth2/token',
    SECRET = 'BjdIlmtKp4Fpyh9hIvr8HJlUida/seM5kQ3EpLAmeDI='
;
```

## <a name="create-the-external-data-source"></a>Creación del origen de datos externo

Utilice el comando [CREATE EXTERNAL DATA SOURCE](/sql/t-sql/statements/create-external-data-source-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest) para almacenar la ubicación de los datos. Si va a autenticarse con el paso a través de AAD, el parámetro CREDENTIAL no es necesario. Si va a realizar la autenticación con Identidad administrada para los puntos de conexión de servicio, siga esta [documentación](../../sql-database/sql-database-vnet-service-endpoint-rule-overview.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json#azure-synapse-analytics-polybase) para configurar el origen de datos externo.

```sql
-- C (for Gen1): Create an external data source
-- TYPE: HADOOP - PolyBase uses Hadoop APIs to access data in Azure Data Lake Storage.
-- LOCATION: Provide Data Lake Storage Gen1 account name and URI
-- CREDENTIAL: Provide the credential created in the previous step.

CREATE EXTERNAL DATA SOURCE AzureDataLakeStorage
WITH (
    TYPE = HADOOP,
    LOCATION = 'adl://<datalakestoregen1accountname>.azuredatalakestore.net',
    CREDENTIAL = ADLSCredential
);

-- C (for Gen2): Create an external data source
-- TYPE: HADOOP - PolyBase uses Hadoop APIs to access data in Azure Data Lake Storage.
-- LOCATION: Provide Data Lake Storage Gen2 account name and URI
-- CREDENTIAL: Provide the credential created in the previous step.

CREATE EXTERNAL DATA SOURCE AzureDataLakeStorage
WITH (
    TYPE = HADOOP,
    LOCATION='abfs[s]://<container>@<AzureDataLake account_name>.dfs.core.windows.net', -- Please note the abfss endpoint for when your account has secure transfer enabled
    CREDENTIAL = ADLSCredential
);
```

## <a name="configure-data-format"></a>Configuración del formato de datos

Para importar los datos de Data Lake Storage, es preciso especificar el formato de archivo externo. Este objeto define cómo se escriben los archivos en Data Lake Storage.
Para obtener una lista completa, consulte la documentación de T-SQL [CREATE EXTERNAL FILE FORMAT](/sql/t-sql/statements/create-external-file-format-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)

```sql
-- D: Create an external file format
-- FIELD_TERMINATOR: Marks the end of each field (column) in a delimited text file
-- STRING_DELIMITER: Specifies the field terminator for data of type string in the text-delimited file.
-- DATE_FORMAT: Specifies a custom format for all date and time data that might appear in a delimited text file.
-- Use_Type_Default: Store missing values as default for datatype.

CREATE EXTERNAL FILE FORMAT TextFileFormat
WITH
(   FORMAT_TYPE = DELIMITEDTEXT
,    FORMAT_OPTIONS    (   FIELD_TERMINATOR = '|'
                    ,    STRING_DELIMITER = ''
                    ,    DATE_FORMAT         = 'yyyy-MM-dd HH:mm:ss.fff'
                    ,    USE_TYPE_DEFAULT = FALSE
                    )
);
```

## <a name="create-the-external-tables"></a>Creación de las tablas externas

Ahora que ha especificado el origen de datos y el formato de archivo, está listo para crear las tablas externas. Las tablas externas indican cómo se interactúa con los datos externos. El parámetro de ubicación puede especificar un archivo o un directorio. Si especifica un directorio, se cargarán todos los archivos que contenga.

```sql
-- D: Create an External Table
-- LOCATION: Folder under the Data Lake Storage root folder.
-- DATA_SOURCE: Specifies which Data Source Object to use.
-- FILE_FORMAT: Specifies which File Format Object to use
-- REJECT_TYPE: Specifies how you want to deal with rejected rows. Either Value or percentage of the total
-- REJECT_VALUE: Sets the Reject value based on the reject type.

-- DimProduct
CREATE EXTERNAL TABLE [dbo].[DimProduct_external] (
    [ProductKey] [int] NOT NULL,
    [ProductLabel] [nvarchar](255) NULL,
    [ProductName] [nvarchar](500) NULL
)
WITH
(
    LOCATION='/DimProduct/'
,   DATA_SOURCE = AzureDataLakeStorage
,   FILE_FORMAT = TextFileFormat
,   REJECT_TYPE = VALUE
,   REJECT_VALUE = 0
)
;

```

## <a name="external-table-considerations"></a>Consideraciones de las tablas externas

Crear una tabla externa es fácil, pero hay algunos matices que deben tratarse.

Las tablas externas están fuertemente tipadas. Esto significa que cada fila de los datos que se van a ingerir debe satisfacer la definición de esquema de tabla.
Si una fila no coincide con la definición de esquema, se rechaza de la carga.

Las opciones REJECT_TYPE y REJECT_VALUE le permiten definir cuántas filas o qué porcentaje de los datos deben estar presentes en la tabla final. Durante la carga, si se alcanza el valor de rechazo, se produce un error en la carga. La causa más común de filas rechazadas es un error de coincidencia de la definición de esquema. Por ejemplo, si una columna especifica incorrectamente el esquema de int cuando los datos del archivo son una cadena, cada fila se producirá un error al cargar.

Data Lake Storage Gen1 usa el control de acceso basado en rol (RBAC) para controlar el acceso a los datos. Esto significa que la entidad de servicio debe tener permisos de lectura para los directorios que se definen en el parámetro de ubicación y para los elementos secundarios de los archivos y los directorios finales. De esta forma, PolyBase puede autenticar y cargar esos datos.

## <a name="load-the-data"></a>Carga de los datos

Para cargar los datos de Data Lake Storage, use la instrucción [CREATE TABLE AS SELECT (Transact-SQL)](/sql/t-sql/statements/create-table-as-select-azure-sql-data-warehouse?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest).

CTAS crea una nueva tabla y la rellena con los resultados de una instrucción SELECT. CTAS define la nueva tabla para tener las mismas columnas y tipos de datos que los resultados de la instrucción SELECT. Si selecciona todas las columnas de una tabla externa, la nueva tabla es una réplica de las columnas y los tipos de datos de la tabla externa.

En este ejemplo, vamos a crear una tabla de hash distribuida llamada DimProduct desde nuestra tabla externa DimProduct_external.

```sql

CREATE TABLE [dbo].[DimProduct]
WITH (DISTRIBUTION = HASH([ProductKey]  ) )
AS
SELECT * FROM [dbo].[DimProduct_external]
OPTION (LABEL = 'CTAS : Load [dbo].[DimProduct]');
```

## <a name="optimize-columnstore-compression"></a>Optimización de compresión de almacén de columnas

De forma predeterminada, las tablas se definen como un índice de almacén de columnas en clúster. Una vez completada una carga, puede que algunas de las filas de datos no se compriman en el almacén de columnas.  Existen varios motivos por los que esto puede ocurrir. Para aprender más, consulte el artículo sobre [administración de índices de almacén de columnas](sql-data-warehouse-tables-index.md).

Para optimizar el rendimiento de las consultas y la compresión de almacén de columnas después de una carga, vuelva a crear la tabla para obligar al índice de almacén de columnas a comprimir todas las filas.

```sql

ALTER INDEX ALL ON [dbo].[DimProduct] REBUILD;

```

## <a name="optimize-statistics"></a>Optimización de estadísticas

Es mejor crear estadísticas de columna única inmediatamente después de una carga. Hay algunas opciones para las estadísticas. Por ejemplo, si crea estadísticas de columna única en cada columna, la recompilación de todas las estadísticas puede llevar mucho tiempo. Si sabe que algunas columnas no van a estar en predicados de consulta, puede omitir la creación de estadísticas en dichas columnas.

Si decide crear estadísticas de columna única en todas las columnas de cada una de las tablas, puede usar el ejemplo de código de procedimiento almacenado `prc_sqldw_create_stats` del artículo sobre [estadísticas](sql-data-warehouse-tables-statistics.md).

El ejemplo siguiente es un buen punto de partida para la creación de estadísticas. Crea estadísticas de columna única en cada una de las columnas de la tabla de dimensiones y en cada una de las columnas de combinación de las tablas de hechos. Siempre puede agregar estadísticas de columna única o de varias columnas a otras columnas de la tabla de hechos más adelante.

## <a name="achievement-unlocked"></a>Logro conseguido.

Ha cargado correctamente datos en el almacenamiento de datos. Buen trabajo.

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, creó tablas externas para definir la estructura de los datos almacenados en Data Lake Storage Gen1 y, luego, utilizó la instrucción CREATE TABLE AS SELECT de PolyBase para cargar datos en el almacenamiento de datos.

Hizo todo esto:
> [!div class="checklist"]
>
> * Creó los objetos de base de datos necesarios para cargar datos desde Data Lake Storage.
> * Se conectó al directorio de Data Lake Storage.
> * Cargó datos en un almacenamiento de datos.
>

La carga de datos es el primer paso para desarrollar una solución de almacenamiento de datos mediante Azure Synapse Analytics. Consulte nuestros recursos de desarrollo.

> [!div class="nextstepaction"]
> [Obtenga información acerca de cómo desarrollar tablas para almacenar datos](sql-data-warehouse-tables-overview.md)

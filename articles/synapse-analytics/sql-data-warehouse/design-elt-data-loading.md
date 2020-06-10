---
title: En lugar de ETL, diseño de ELT
description: Implementación de estrategias flexibles de carga de datos para el grupo de SQL de Synapse en Azure Synapse Analytics
services: synapse-analytics
author: kevinvngo
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: ''
ms.date: 05/13/2020
ms.author: kevin
ms.reviewer: igorstan
ms.custom: azure-synapse
ms.openlocfilehash: fc5316e2d6509f3e4db9a6cba150efc42c8bc548
ms.sourcegitcommit: 309cf6876d906425a0d6f72deceb9ecd231d387c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/01/2020
ms.locfileid: "84266379"
---
# <a name="data-loading-strategies-for-synapse-sql-pool"></a>Estrategias de carga de datos para el grupo de SQL de Synapse

Los grupos de SQL de SMP tradicionales usan un proceso de extracción, transformación y carga (ETL) para cargar los datos. El grupo de SQL de Synapse de Azure Synapse Analytics tienen una arquitectura de procesamiento paralelo masivo (MPP) que aprovecha la escalabilidad y la flexibilidad de los recursos de proceso y almacenamiento.

El uso de un proceso de extracción, carga y transformación (ELT) aprovecha MPP y elimina los recursos necesarios para la transformación de datos antes de la carga.

Aunque el grupo de SQL admite muchos métodos de carga, entre los que se incluyen las conocidas opciones de SQL Server como [bcp](/sql/tools/bcp-utility?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest) y [SqlBulkCopy API](/dotnet/api/system.data.sqlclient.sqlbulkcopy?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json), la manera más rápida y escalable de cargar datos es mediante tablas externas de PolyBase y la [instrucción COPY](/sql/t-sql/statements/copy-into-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest) (versión preliminar).

Con PolyBase y la instrucción COPY, es posible acceder a datos externos almacenados en Azure Blob Storage o Azure Data Lake Store mediante el lenguaje T-SQL. Para obtener la máxima flexibilidad al realizar la carga, se recomienda usar la instrucción COPY.

> [!NOTE]  
> Actualmente, la instrucción COPY está en versión preliminar pública. Para realizar los comentarios que desee, envíe un mensaje de correo electrónico a la siguiente lista de distribución: sqldwcopypreview@service.microsoft.com.

> [!VIDEO https://www.youtube.com/embed/l9-wP7OdhDk]

## <a name="what-is-elt"></a>¿Qué es ELT?

Extracción, carga y transformación (ELT) es un proceso mediante el que se extraen datos de un sistema de origen, se cargan en un grupo de SQL y, después, se transforman.

Los pasos básicos para implementar ELT son los siguientes:

1. Extraer los datos de origen en archivos de texto.
2. Llevar los datos a Azure Blob Storage o Azure Data Lake Store.
3. Preparar los datos para la carga.
4. Cargar los datos en las tablas de almacenamiento provisional con PolyBase o el comando COPY.
5. Transformar los datos.
6. Insertar los datos en tablas de producción.

Para obtener un tutorial sobre la carga, vea [Carga de datos desde Azure Blob Storage](load-data-from-azure-blob-storage-using-polybase.md).

## <a name="1-extract-the-source-data-into-text-files"></a>1. Extraer los datos de origen en archivos de texto

La obtención de datos del sistema de origen depende de la ubicación del almacenamiento. El objetivo consiste en mover los datos a archivos de texto delimitados compatibles o CSV.

### <a name="supported-file-formats"></a>Formatos de archivos admitidos

Con PolyBase y la instrucción COPY, puede cargar datos a archivos CSV o de texto delimitados con codificación UTF-8 y UTF-16. Además de los archivos CSV o de texto delimitados, carga datos desde formatos de archivos Hadoop, como ORC y Parquet. PolyBase y la instrucción COPY también pueden cargar datos desde archivos comprimidos Gzip y Snappy.

No se admiten el formato ASCII extendido, el formato de ancho fijo ni los formatos anidados como WinZip o XML. Si va a exportar desde SQL Server, puede usar la [herramienta de línea de comandos bcp](/sql/tools/bcp-utility?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest) para exportar los datos en archivos de texto delimitados.

## <a name="2-land-the-data-into-azure-blob-storage-or-azure-data-lake-store"></a>2. Llevar los datos a Azure Blob Storage o Azure Data Lake Store

Para llevar los datos a Azure Storage, puede moverlos a [Azure Blob Storage](../../storage/blobs/storage-blobs-introduction.md?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json) o [Azure Data Lake Store Gen2](../../data-lake-store/data-lake-store-overview.md?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json). Los datos deben almacenarse en archivos de texto en ambas ubicaciones. PolyBase y la instrucción COPY se pueden cargar desde cualquier ubicación.

Herramientas y servicios que puede usar para mover datos a Azure Storage:

- El servicio [Azure ExpressRoute](../../expressroute/expressroute-introduction.md?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json) mejora el rendimiento de la red, el rendimiento en general y la capacidad de predicción. ExpressRoute es un servicio que enruta los datos a Azure a través de una conexión privada dedicada. Las conexiones ExpressRoute no enrutan datos a través de la red pública de Internet. Estas conexiones son más fiables y ofrecen velocidades más altas, menores latencias y mayor seguridad que las conexiones habituales a través de Internet.
- La [utilidad AZCopy](../../storage/common/storage-choose-data-transfer-solution.md?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json) lleva los datos a Azure Storage a través de la red pública de Internet. Esto funciona si el tamaño de los datos es de menos de 10 TB. Para realizar cargas de forma regular con AZCopy, pruebe la velocidad de la red para ver si es aceptable.
- [Azure Data Factory (ADF)](../../data-factory/introduction.md?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json) tiene una puerta de enlace que se puede instalar en el servidor local. A continuación, puede crear una canalización para llevar los datos desde el servidor local hasta Azure Storage. Para usar Data Factory con el grupo de SQL, vea [Carga de datos para el grupo de SQL](../../data-factory/load-azure-sql-data-warehouse.md?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json).

## <a name="3-prepare-the-data-for-loading"></a>3. Preparar los datos para la carga

Deberá preparar y limpiar los datos de la cuenta de almacenamiento antes de cargarlos. Puede preparar los datos mientras están en el origen y a medida que los exporta a archivos de texto o después de que estén en Azure Storage.  Si trabaja con los datos en la parte inicial el proceso, será más fácil manejarlos.  

### <a name="define-the-tables"></a>Definición de las tablas

En primer lugar, se deben definir las tablas que se van a cargar en el grupo de SQL cuando se use la instrucción COPY.

Si usa PolyBase, tiene que definir tablas externas en el grupo de SQL antes de realizar la carga. PolyBase emplea tablas externas para obtener acceso y definir los datos en Azure Storage. Una tabla externa es similar a una vista de base de datos. La tabla externa contiene el esquema de tabla y apunta a los datos que se almacenan fuera del grupo de SQL.

Si define tablas externas debe especificar el origen de datos, el formato de los archivos de texto y las definiciones de tabla. Los artículos de referencia de sintaxis de T-SQL que necesitará son los siguientes:

- [CREATE EXTERNAL DATA SOURCE](/sql/t-sql/statements/create-external-data-source-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
- [CREATE EXTERNAL FILE FORMAT](/sql/t-sql/statements/create-external-file-format-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
- [CREATE EXTERNAL TABLE](/sql/t-sql/statements/create-external-table-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)

Use la asignación siguiente de tipo de datos SQL al cargar archivos de Parquet:

|                         Tipo de Parquet                         |   Tipo lógico de Parquet (anotación)   |  Tipo de datos de SQL   |
| :----------------------------------------------------------: | :-----------------------------------: | :--------------: |
|                           BOOLEAN                            |                                       |       bit        |
|                     BINARY/BYTE_ARRAY                      |                                       |    varbinary     |
|                            DOUBLE                            |                                       |      FLOAT       |
|                            FLOAT                             |                                       |       real       |
|                            INT32                             |                                       |       int        |
|                            INT64                             |                                       |      bigint      |
|                            INT96                             |                                       |    datetime2     |
|                     FIXED_LEN_BYTE_ARRAY                     |                                       |      binary      |
|                            BINARY                            |                 UTF8                  |     NVARCHAR     |
|                            BINARY                            |                STRING                 |     NVARCHAR     |
|                            BINARY                            |                 ENUM                  |     NVARCHAR     |
|                            BINARY                            |                 UUID                  | UNIQUEIDENTIFIER |
|                            BINARY                            |                DECIMAL                |     Decimal      |
|                            BINARY                            |                 JSON                  |  nvarchar(MAX)   |
|                            BINARY                            |                 BSON                  |  varbinary(max)  |
|                     FIXED_LEN_BYTE_ARRAY                     |                DECIMAL                |     Decimal      |
|                          BYTE_ARRAY                          |               INTERVAL                |  varchar(max),   |
|                            INT32                             |             INT(8, true)              |     SMALLINT     |
|                            INT32                             |            INT(16,   true)            |     SMALLINT     |
|                            INT32                             |             INT(32, true)             |       int        |
|                            INT32                             |            INT(8,   false)            |     TINYINT      |
|                            INT32                             |            INT(16, false)             |       int        |
|                            INT32                             |           INT(32,   false)            |      bigint      |
|                            INT32                             |                 DATE                  |       date       |
|                            INT32                             |                DECIMAL                |     Decimal      |
|                            INT32                             |            TIME (MILLIS)             |       time       |
|                            INT64                             |            INT(64,   true)            |      bigint      |
|                            INT64                             |           INT(64, false  )            |  decimal(20,0)   |
|                            INT64                             |                DECIMAL                |     Decimal      |
|                            INT64                             |         TIME (MICROS / NANOS)         |       time       |
|                            INT64                             | TIMESTAMP   (MILLIS / MICROS / NANOS) |    datetime2     |
| [Tipo complejo](https://nam06.safelinks.protection.outlook.com/?url=https%3A%2F%2Fgithub.com%2Fapache%2Fparquet-format%2Fblob%2Fmaster%2FLogicalTypes.md%23lists&data=02\|01\|kevin%40microsoft.com\|19f74d93f5ca45a6b73c08d7d7f5f111\|72f988bf86f141af91ab2d7cd011db47\|1\|0\|637215323617803168&sdata=6Luk047sK26ijTzfvKMYc%2FNu%2Fz0AlLCX8lKKTI%2F8B5o%3D&reserved=0) |                 LISTA                  |   ntext   |
| [Tipo complejo](https://nam06.safelinks.protection.outlook.com/?url=https%3A%2F%2Fgithub.com%2Fapache%2Fparquet-format%2Fblob%2Fmaster%2FLogicalTypes.md%23maps&data=02\|01\|kevin%40microsoft.com\|19f74d93f5ca45a6b73c08d7d7f5f111\|72f988bf86f141af91ab2d7cd011db47\|1\|0\|637215323617803168&sdata=FiThqXxjgmZBVRyigHzfh5V7Z%2BPZHjud2IkUUM43I7o%3D&reserved=0) |                  MAP                  |   ntext   |



Para obtener un ejemplo de creación de objetos externos, vea [Creación de tablas externas](https://docs.microsoft.com/azure/synapse-analytics/sql/develop-tables-external-tables?tabs=sql-pool).

### <a name="format-text-files"></a>Formato de los archivos de texto

Si utiliza PolyBase, los objetos externos definidos necesitan alinear las filas de los archivos de texto con la tabla externa y la definición de formato de archivo. Los datos de cada fila del archivo de texto deben alinearse con la definición de tabla.
Para dar formato a los archivos de texto:

- Si los datos provienen de un origen no relacional, debe transformarlos en filas y columnas. Si los datos son de un origen relacional o no relacional, se deben transformar para alinearlos con las definiciones de columna de la tabla en la que va a cargar los datos.
- Debe dar formato a datos en el archivo de texto que se alineará con los tipos de datos y columnas en la tabla de destino. Si se desalinean los tipos de datos en los archivos de texto externos y la tabla del grupo de SQL, las filas se rechazarán durante la carga.
- Debe separar los campos en el archivo de texto con un terminador.  Asegúrese de usar un carácter o una secuencia de caracteres que no se encuentre en los datos de origen. Use el terminador que especificó con la instrucción [CREATE EXTERNAL FILE FORMAT](/sql/t-sql/statements/create-external-file-format-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest).

## <a name="4-load-the-data-using-polybase-or-the-copy-statement"></a>4. Cargar los datos mediante PolyBase o la instrucción COPY

Es una práctica recomendada para cargar datos en una tabla de almacenamiento provisional. Las tablas de almacenamiento provisional le permiten controlar los errores sin interferir con las tablas de producción. Asimismo, una tabla de almacenamiento provisional también ofrece la posibilidad de usar la arquitectura de procesamiento paralelo del grupo de SQL para transformaciones de datos antes de insertar estos datos en tablas de producción.

### <a name="options-for-loading"></a>Opciones de carga

Para cargar datos, puede usar cualquiera de estas opciones de carga:

- La [instrucción COPY](https://docs.microsoft.com/sql/t-sql/statements/copy-into-transact-sql?view=azure-sqldw-latest) es la utilidad de carga recomendada, ya que permite cargar datos de forma fluida y flexible. La instrucción tiene muchas capacidades de carga adicionales que PolyBase no proporciona. 
- [Polybase con T-SQL](load-data-from-azure-blob-storage-using-polybase.md) requiere que se definan los objetos de datos externos.
- [PolyBase y la instrucción COPY con Azure Data Factory (ADF)](../../data-factory/load-azure-sql-data-warehouse.md?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json) es otra herramienta de orquestación.  Define una canalización y programa trabajos.
- [Polybase con SSIS](/sql/integration-services/load-data-to-sql-data-warehouse?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest) funciona bien cuando los datos de origen están en SQL Server. SSIS define las asignaciones de la tabla de origen a la de destino y también organiza la carga. Si ya dispone de paquetes SSIS, puede modificar los paquetes con los que vaya a trabajar con el nuevo destino del almacenamiento de datos.
- [PolyBase con Azure Databricks](../../azure-databricks/databricks-extract-load-sql-data-warehouse.md?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json) transfiere los datos de una tabla para una trama de datos de Databricks o escribe datos de una trama de datos de Databricks en una tabla con PolyBase.

### <a name="other-loading-options"></a>Otras opciones de carga

Además de PolyBase y la instrucción COPY, puede usar [bcp](/sql/tools/bcp-utility?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest) o [SqlBulkCopy API](/dotnet/api/system.data.sqlclient.sqlbulkcopy?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json). El formato bcp carga datos directamente sin tener que pasar por Azure Blob Storage y está diseñado únicamente para cargas pequeñas.

> [!NOTE]
> El rendimiento de la carga de estas opciones es más lento que el de PolyBase y la instrucción COPY.

## <a name="5-transform-the-data"></a>5. Transformar los datos

Mientras los datos estén en una tabla de almacenamiento provisional, podrá realizar las transformaciones que sean necesarias para la carga de trabajo. A continuación, lleve los datos a una tabla de producción.

## <a name="6-insert-the-data-into-production-tables"></a>6. Insertar los datos en tablas de producción

La instrucción INSERT INTO ... SELECT lleva los datos de la tabla de almacenamiento provisional a la tabla permanente.

Cuando diseñe un proceso ETL, intente ejecutar el proceso con una pequeña muestra de prueba. Intente extraer 1000 filas de la tabla a un archivo, muévalo a Azure y, a continuación, intente cargarlo en una tabla de almacenamiento provisional.

## <a name="partner-loading-solutions"></a>Soluciones de carga de asociados

Muchos de nuestros asociados tienen soluciones de carga. Para obtener más información, consulte una lista de nuestros [asociados de soluciones](sql-data-warehouse-partner-business-intelligence.md).

## <a name="next-steps"></a>Pasos siguientes

Para obtener una guía de carga, consulte la [guía para cargar datos](guidance-for-loading-data.md).

---
title: Uso de OPENROWSET en SQL a petición (versión preliminar)
description: En este artículo se describe la sintaxis de OPENROWSET en SQL a petición (versión preliminar) y se explica cómo usar los argumentos.
services: synapse-analytics
author: filippopovic
ms.service: synapse-analytics
ms.topic: overview
ms.subservice: ''
ms.date: 04/15/2020
ms.author: fipopovi
ms.reviewer: jrasnick
ms.openlocfilehash: 6325d5555b01373b148dce69731ec64896d6e1fd
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/21/2020
ms.locfileid: "81680492"
---
# <a name="how-to-use-openrowset-with-sql-on-demand-preview"></a>Uso de OPENROWSET con SQL a petición (versión preliminar)

La función OPENROWSET (BULK...) permite acceder a archivos en Azure Storage. En el recurso de SQL a petición (versión preliminar), para acceder al proveedor de conjuntos de filas BULK de OPENROWSET se llama a la función OPENROWSET y se especifica la opción BULK.  

Se puede hacer referencia a la función OPENROWSET en la cláusula FROM de una consulta, como si fuera una tabla llamada OPENROWSET. Admite operaciones masivas a través de un proveedor integrado BULK que permite que los datos se lean y se devuelvan en forma de conjunto de filas.

OPENROWSET no se admite actualmente en el grupo de SQL.

## <a name="syntax"></a>Sintaxis

```syntaxsql
--OPENROWSET syntax for reading Parquet files
OPENROWSET  
( { BULK 'unstructured_data_path' , 
    FORMAT='PARQUET' }  
)  
[WITH ( {'column_name' 'column_type' }) ]
[AS] table_alias(column_alias,...n)

--OPENROWSET syntax for reading delimited text files
OPENROWSET  
( { BULK 'unstructured_data_path' , 
    FORMAT = 'CSV'
    [ <bulk_options> ] }  
)  
WITH ( {'column_name' 'column_type' [ 'column_ordinal'] })  
[AS] table_alias(column_alias,...n)
 
<bulk_options> ::=  
[ , FIELDTERMINATOR = 'char' ]    
[ , ROWTERMINATOR = 'char' ] 
[ , ESCAPE_CHAR = 'char' ] 
[ , FIRSTROW = 'first_row'  ]     
[ , FIELDQUOTE = 'quote_characters']
```

## <a name="arguments"></a>Argumentos

Hay dos opciones para los archivos de entrada que contienen los datos de destino para realizar consultas. Los valores válidos son:

- "CSV": incluye cualquier archivo de texto delimitado con separadores de filas o columnas. Se puede usar cualquier carácter como separador de campo, como TSV: FIELDTERMINATOR = pestaña.

- "PARQUET": archivo binario en formato Parquet 

**"unstructured_data_path"**

El elemento unstructured_data_path que establece una ruta de acceso a los datos se estructura de la siguiente manera:  
"\<prefijo>://\<rutaDeLaCuentaDeAlmacenamiento>/\<rutaDeAlmacenamiento>"
 
 
 A continuación, encontrará las rutas de acceso de las cuentas de almacenamiento pertinentes que se vincularán a su origen de datos externo concreto. 

| Origen de datos externo       | Prefijo | Ruta de acceso a la cuenta de almacenamiento                                 |
| -------------------------- | ------ | ---------------------------------------------------- |
| Azure Blob Storage         | https  | \<cuentaDeAlmacenamiento>.blob.core.windows.net             |
| Azure Data Lake Store Gen1 | https  | \<cuentaDeAlmacenamiento>.azuredatalakestore.net/webhdfs/v1 |
| Azure Data Lake Store Gen2 | https  | \<cuentaDeAlmacenamiento>.dfs.core.windows.net              |
||||

"\<rutaDeAlmacenamiento>"

 Especifica una ruta de acceso en el almacenamiento que apunta a la carpeta o archivo que desea leer. Si la ruta de acceso apunta a un contenedor o a una carpeta, se leerán todos los archivos de ese contenedor o carpeta. No se incluirán los archivos de las subcarpetas. 
 
 Puede usar caracteres comodín si el destino son varios archivos o carpetas. Se permite el uso de varios caracteres comodín no consecutivos.
A continuación se muestra un ejemplo en el que se leen todos los archivos *csv* que comienzan por *population* de todas las carpetas a partir de */csv/population*:  
`https://sqlondemandstorage.blob.core.windows.net/csv/population*/population*.csv`

Si especifica que el elemento unstructured_data_path sea una carpeta, una consulta de SQL a petición recuperará los archivos de esa carpeta. 

> [!NOTE]
> A diferencia de Hadoop y PolyBase, SQL a petición no devuelve subcarpetas. También a diferencia de Hadoop y Polybase, SQL a petición devuelve los archivos cuyo nombre comienza por un carácter de subrayado (_) o un punto (.).

En el ejemplo siguiente, si unstructured_data_path = `https://mystorageaccount.dfs.core.windows.net/webdata/`, una consulta de SQL a petición devolverá las filas de mydata.txt y _hidden.txt. No devolverá mydata2.txt y mydata3.txt porque se encuentran en una subcarpeta.

![Datos recursivos para tablas externas](./media/develop-openrowset/folder-traversal.png)

`[WITH ( {'column_name' 'column_type' [ 'column_ordinal'] }) ]`

La cláusula WITH le permite especificar las columnas que desea leer de los archivos.

- En el caso de los archivos de datos con la extensión .csv, para leer todas las columnas, especifique los nombres de columna y sus tipos de datos. Si desea un subconjunto de columnas, use números ordinales para seleccionar las columnas en los archivos de datos de origen por ordinal. Las columnas se enlazarán por la designación ordinal. 

> [!IMPORTANT]
> La cláusula WITH es obligatoria para los archivos .csv.
- En el caso de los archivos de datos con formato Parquet, especifique nombres de columna que coincidan con los de los archivos de datos de origen. Las columnas se enlazarán por nombre. Si se omite la cláusula WITH, se devolverán todas las columnas de los archivos con formato Parquet.

column_name = Nombre de la columna de salida. Si se especifica, este nombre anula el nombre de la columna del archivo de origen.

column_type = Tipo de datos de la columna de salida. La conversión implícita del tipo de datos tendrá lugar aquí.

column_ordinal = Número ordinal de la columna en los archivos de origen. Este argumento se omite para los archivos con formato Parquet, ya que el enlace se realiza por nombre. En el ejemplo siguiente se devuelve una segunda columna solo desde un archivo .csv:

```sql
WITH (
    --[country_code] VARCHAR (5) COLLATE Latin1_General_BIN2,
    [country_name] VARCHAR (100) COLLATE Latin1_General_BIN2 2
    --[year] smallint,
    --[population] bigint
)
```

**\<bulk_options>**

FIELDTERMINATOR ='field_terminator'

Especifica el terminador de campo que se va a usar. El terminador de campo predeterminado es una coma (" **,** ").

ROWTERMINATOR ="row_terminator"

Especifica el terminador de fila que se va a usar. El terminador de fila predeterminado es un carácter de nueva línea, como \r\n.

ESCAPE_CHAR = "char"

Especifica el carácter del archivo que se usa como carácter de escape de sí mismo y de todos los valores de delimitador del archivo. Si el carácter de escape va seguido de un valor distinto de sí mismo o de cualquiera de los valores de delimitador, se quita al leer el valor. 

El parámetro ESCAPE_CHAR se aplicará independientemente de que FIELDQUOTE esté habilitado o no. No se utilizará como carácter de escape el carácter de comillas. El carácter de escape de las comillas simples es el carácter de comillas dobles, igual que sucede en los archivos .csv de Excel.

FIRSTROW = "first_row" 

Especifica el número de la primera fila que se va a cargar. El valor predeterminado es 1. Indica la primera fila del archivo de datos especificado. Los números de fila vienen determinados por el recuento de terminadores de fila. FIRSTROW está en base 1.

FIELDQUOTE = "field_quote" 

Especifica un carácter que se usará como carácter de comillas en el archivo CSV. Si no se especifica, se usará el carácter de comillas ("). 

## <a name="examples"></a>Ejemplos

En el ejemplo siguiente se devuelven solo dos columnas con los números ordinales 1 y 4 de los archivos population*.csv. Al no haber fila de encabezado en los archivos, empieza a leer desde la primera línea:

```sql
/* make sure you have credentials for storage account access created
IF EXISTS (SELECT * FROM sys.credentials WHERE name = 'https://azureopendatastorage.blob.core.windows.net/censusdatacontainer')
DROP CREDENTIAL [https://azureopendatastorage.blob.core.windows.net/censusdatacontainer]
GO

CREATE CREDENTIAL [https://azureopendatastorage.blob.core.windows.net/censusdatacontainer]  
WITH IDENTITY='SHARED ACCESS SIGNATURE',  
SECRET = ''
GO
*/

SELECT * 
FROM OPENROWSET(
        BULK 'https://sqlondemandstorage.blob.core.windows.net/csv/population/population*.csv',
        FORMAT = 'CSV',
        FIRSTROW = 1
    )
WITH (
    [country_code] VARCHAR (5) COLLATE Latin1_General_BIN2 1,
    [population] bigint 4
) AS [r]
```



En el ejemplo siguiente se devuelven todas las columnas de la primera fila del conjunto de datos del censo en formato Parquet sin especificar los nombres de columna ni los tipos de datos: 

```sql
/* make sure you have credentials for storage account access created
IF EXISTS (SELECT * FROM sys.credentials WHERE name = 'https://azureopendatastorage.blob.core.windows.net/censusdatacontainer')
DROP CREDENTIAL [https://azureopendatastorage.blob.core.windows.net/censusdatacontainer]
GO

CREATE CREDENTIAL [https://azureopendatastorage.blob.core.windows.net/censusdatacontainer]  
WITH IDENTITY='SHARED ACCESS SIGNATURE',  
SECRET = ''
GO
*/

SELECT 
    TOP 1 *
FROM  
    OPENROWSET(
        BULK 'https://azureopendatastorage.blob.core.windows.net/censusdatacontainer/release/us_population_county/year=20*/*.parquet',
        FORMAT='PARQUET'
    ) AS [r]
```



## <a name="next-steps"></a>Pasos siguientes

Para más ejemplos, vaya a los [inicios rápidos](query-data-storage.md) o guarde los resultados de la consulta en Azure Storage mediante [CETAS](develop-tables-cetas.md).

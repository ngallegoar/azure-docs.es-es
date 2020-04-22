---
title: Uso de SQL a petición (versión preliminar)
description: En este inicio rápido, verá y aprenderá lo fácil que es consultar varios tipos de archivos mediante SQL a petición (versión preliminar).
services: synapse-analytics
author: azaricstefan
ms.service: synapse-analytics
ms.topic: quickstart
ms.subservice: ''
ms.date: 04/15/2020
ms.author: v-stazar
ms.reviewer: jrasnick
ms.openlocfilehash: 0d543abc88c1e45f2c1f5503473d8e92566fc582
ms.sourcegitcommit: b55d7c87dc645d8e5eb1e8f05f5afa38d7574846
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/16/2020
ms.locfileid: "81457389"
---
# <a name="quickstart-using-sql-on-demand"></a>Inicio rápido: Uso de SQL a petición

Synapse SQL a petición (versión preliminar) es un servicio de consulta sin servidor que permite ejecutar consultas SQL en los archivos colocados en Azure Storage. En este inicio rápido, obtendrá información sobre cómo consultar varios tipos de archivos mediante SQL a petición.

Se admiten los tipos de archivo siguientes: JSON, CSV, Apache Parquet

## <a name="prerequisites"></a>Prerrequisitos

Elija un cliente SQL para emitir consultas:

- [Azure Synapse Studio](quickstart-synapse-studio.md) es una herramienta web que puede usar para examinar archivos en el almacenamiento y crear consultas SQL.
- [Azure Data Studio](sql/get-started-azure-data-studio.md) es una herramienta de cliente que permite ejecutar consultas SQL y cuadernos en la base de datos a petición.
- [SQL Server Management Studio](sql/get-started-ssms.md) es una herramienta de cliente que permite ejecutar consultas SQL en la base de datos a petición.

Parámetros de inicio rápido:

| Parámetro                                 | Descripción                                                   |
| ----------------------------------------- | ------------------------------------------------------------- |
| Dirección de un punto de conexión del servicio de SQL a petición.    | Se usa como nombre de servidor.                                   |
| Región de punto de conexión del servicio de SQL a petición.     | Se usará para determinar qué almacenamiento se utilizará en los ejemplos. |
| Nombre de usuario y contraseña para el acceso al punto de conexión. | Se usa para acceder al punto de conexión.                               |
| La base de datos que se utiliza para crear vistas.         | La base de datos que se utiliza como punto de partida en ejemplos.       |

## <a name="first-time-setup"></a>Primera configuración

Antes de usar los ejemplos hay que hacer lo siguiente:

- Crear una base de datos para las vistas (en caso de que quiera usar vistas).
- Crear las credenciales que va a usar SQL a petición para acceder a los archivos en el almacenamiento.

### <a name="create-database"></a>Crear base de datos

Cree su propia base de datos para fines de demostración. Esta es la base de datos en la que se crean las vistas. Use esta base de datos en las consultas de ejemplo de este artículo.

> [!NOTE]
> Las bases de datos se usan solo para los metadatos de la vista, no para los datos reales.
>
> Anote el nombre de la base de datos que usa para usarlo más adelante en el artículo de inicio rápido.

Use la siguiente consulta, cambiando `mydbname` por un nombre de su elección:

```sql
CREATE DATABASE mydbname
```

### <a name="create-credentials"></a>Crear credenciales

Para ejecutar consultas con SQL a petición, cree credenciales para SQL a petición a fin de usarlas para tener acceso a los archivos del almacenamiento.

> [!NOTE]
> Tenga en cuenta que debe crear credenciales para el acceso a la cuenta de almacenamiento. Aunque SQL a petición puede acceder a los almacenamientos desde distintas regiones, tener el almacenamiento y el área de trabajo de Azure Synapse en la misma región proporcionará una mejor experiencia de rendimiento.

Modifique el siguiente fragmento de código para crear credenciales para los contenedores CSV, JSON y Parquet:

```sql
-- create credentials for containers in our demo storage account
IF EXISTS
   (SELECT * FROM sys.credentials
   WHERE name = 'https://sqlondemandstorage.blob.core.windows.net')
   DROP CREDENTIAL [https://sqlondemandstorage.blob.core.windows.net]
GO

CREATE CREDENTIAL [https://sqlondemandstorage.blob.core.windows.net]
WITH IDENTITY='SHARED ACCESS SIGNATURE',  
SECRET = 'sv=2018-03-28&ss=bf&srt=sco&sp=rl&st=2019-10-14T12%3A10%3A25Z&se=2061-12-31T12%3A10%3A00Z&sig=KlSU2ullCscyTS0An0nozEpo4tO5JAgGBvw%2FJX2lguw%3D'
GO
```

## <a name="querying-csv-files"></a>Consulta de archivos CSV

La imagen siguiente es una vista previa del archivo que se va a consultar:

![Primeras 10 filas del archivo CSV sin encabezado, nueva línea al estilo de Windows.](./sql/media/query-single-csv-file/population.png)

La consulta siguiente muestra cómo leer un archivo CSV que no contiene una fila de encabezado, con una nueva línea al estilo de Windows y columnas delimitadas por comas.

```sql
SELECT TOP 10 *
FROM OPENROWSET
  (
      BULK 'https://sqlondemandstorage.blob.core.windows.net/csv/population/*.csv'
    , FORMAT = 'CSV'
  )
WITH
  (
      country_code VARCHAR (5)
    , country_name VARCHAR (100)
    , year smallint
    , population bigint
  ) AS r
WHERE
  country_name = 'Luxembourg' AND year = 2017
```

Puede especificar el esquema en el momento de la compilación de la consulta.
Para obtener más ejemplos, consulte el artículo sobre cómo [consultar un archivo CSV](sql/query-single-csv-file.md).

## <a name="querying-parquet-files"></a>Consulta de archivos Parquet

En el ejemplo siguiente se muestran las funcionalidades de inferencia automática del esquema para los archivos Parquet. Devuelve el número de filas de septiembre de 2017 sin especificar un esquema.

> [!NOTE]
> No es necesario especificar columnas en la cláusula `OPENROWSET WITH` al leer los archivos Parquet. En este caso, SQL a petición utilizará los metadatos del archivo de Parquet y enlazará las columnas por nombre.

```sql
SELECT COUNT_BIG(*)
FROM OPENROWSET
  (
      BULK 'https://sqlondemandstorage.blob.core.windows.net/parquet/taxi/year=2017/month=9/*.parquet'
    , FORMAT='PARQUET'
  ) AS nyc
```

Obtenga más información sobre cómo [consultar archivos Parquet](sql/query-parquet-files.md)].

## <a name="querying-json-files"></a>Consulta de archivos JSON

### <a name="json-sample-file"></a>Archivo de ejemplo de JSON

Los archivos se almacenan en un contenedor *json*, carpeta *books*y contienen una entrada con un único libro con la siguiente estructura:

```json
{  
   "_id":"ahokw88",
   "type":"Book",
   "title":"The AWK Programming Language",
   "year":"1988",
   "publisher":"Addison-Wesley",
   "authors":[  
      "Alfred V. Aho",
      "Brian W. Kernighan",
      "Peter J. Weinberger"
   ],
   "source":"DBLP"
}
```

### <a name="querying-json-files"></a>Consulta de archivos JSON

En la consulta siguiente se muestra cómo usar [JSON_VALUE](/sql/t-sql/functions/json-value-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) para recuperar valores escalares (título, editor) de un libro con el título *Probabilistic and Statistical Methods in Cryptology, An Introduction by Selected articles*:

```sql
SELECT
    JSON_VALUE(jsonContent, '$.title') AS title
  , JSON_VALUE(jsonContent, '$.publisher') as publisher
  , jsonContent
FROM OPENROWSET
  (
      BULK 'https://sqlondemandstorage.blob.core.windows.net/json/books/*.json'
    , FORMAT='CSV'
    , FIELDTERMINATOR ='0x0b'
    , FIELDQUOTE = '0x0b'
    , ROWTERMINATOR = '0x0b'
  )
WITH
  ( jsonContent varchar(8000) ) AS [r]
WHERE
  JSON_VALUE(jsonContent, '$.title') = 'Probabilistic and Statistical Methods in Cryptology, An Introduction by Selected Topics'
```

> [!IMPORTANT]
> Estamos leyendo todo el archivo JSON como una sola fila o columna, por lo que FIELDTERMINATOR, FIELDQUOTE y ROWTERMINATOR se establecen en 0x0B porque no esperamos encontrarlo en el archivo.

## <a name="next-steps"></a>Pasos siguientes

Ya está listo para empezar con los siguientes artículos de inicio rápido:

- [Consulta de archivos CSV](sql/query-single-csv-file.md)
- [Consulta de carpetas y varios archivos .csv](sql/query-folders-multiple-csv-files.md)
- [Consulta de archivos específicos](sql/query-specific-files.md)
- [Consulta de archivos Parquet](sql/query-parquet-files.md)
- [Consulta de tipos anidados de Parquet](sql/query-parquet-nested-types.md)
- [Consulta de archivos JSON](sql/query-json-files.md)
- [Creación y uso de vistas en SQL a petición (versión preliminar) en Azure Synapse Analytics](sql/create-use-views.md)
- [Creación y uso de tablas externas en SQL a petición (versión preliminar) en Azure Synapse Analytics](sql/create-use-external-tables.md)
- [Almacenamiento de resultados de búsqueda en el almacenamiento mediante SQL a petición (versión preliminar) en Azure Synapse Analytics](sql/create-external-table-as-select.md)

Pase al siguiente artículo para obtener más información sobre cómo consultar un único archivo CSV.
> [!div class="nextstepaction"]
> [Consulta de archivos CSV](sql/query-single-csv-file.md)

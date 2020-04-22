---
title: 'Introducción: consulta de los datos en el almacenamiento mediante SQL a petición (versión preliminar)'
description: Esta sección contiene consultas de ejemplo que puede usar para probar el recurso SQL a petición (versión preliminar) en Azure Synapse Analytics.
services: synapse analytics
author: azaricstefan
ms.service: synapse-analytics
ms.topic: overview
ms.subservice: ''
ms.date: 04/15/2020
ms.author: v-stazar
ms.reviewer: jrasnick, carlrab
ms.openlocfilehash: cdad95b1a910a45629e85bcc716218b272afd9de
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/16/2020
ms.locfileid: "81421299"
---
# <a name="overview-query-data-in-storage"></a>Introducción: consulta de los datos en el almacenamiento

Esta sección contiene consultas de ejemplo que puede usar para probar el recurso de SQL a petición (versión preliminar) en Azure Synapse Analytics.
Estos son los archivos que se admiten actualmente: 
- CSV
- Parquet
- JSON

## <a name="prerequisites"></a>Prerrequisitos

Las herramientas necesarias para emitir consultas:

- El cliente SQL que prefiera:
    - Azure Synapse Studio (versión preliminar)
    - Azure Data Studio
    - SQL Server Management Studio

Además, estos son los parámetros:

| Parámetro                                 | Descripción                                                   |
| ----------------------------------------- | ------------------------------------------------------------- |
| Dirección de un punto de conexión del servicio de SQL a petición    | Se usará como nombre del servidor.                                   |
| Región de punto de conexión del servicio de SQL a petición     | Se utilizará para determinar el almacenamiento que se usa en los ejemplos. |
| Nombre de usuario y contraseña para el acceso al punto de conexión | Se usará para acceder al punto de conexión.                               |
| La base de datos que se va a usar para crear vistas     | Esta base de datos se utilizará como punto de partida de los ejemplos.       |

## <a name="first-time-setup"></a>Primera configuración

Antes de usar los ejemplos que se incluyen más adelante en este artículo, tiene dos pasos:

- Cree una base de datos para las vistas (en caso de que quiera usar vistas)
- Cree las credenciales que va a usar SQL a petición para acceder a los archivos en el almacenamiento

### <a name="create-database"></a>Crear base de datos

Para crear vistas, necesita una base de datos. Usará esta base de datos para algunas de las consultas de ejemplo de esta documentación.

> [!NOTE]
> Las bases de datos se usan solo para ver metadatos, no para datos reales.  Anote el nombre de la base de datos que use, ya que lo necesitará más adelante.

```sql
CREATE DATABASE mydbname;
```

### <a name="create-credentials"></a>Crear credenciales

Debe crear credenciales para poder ejecutar consultas. El servicio de SQL a petición usará esta credencial para acceder a los archivos del almacenamiento.

> [!NOTE]
> Para ejecutar correctamente los procedimientos de esta sección, es preciso usar un token de SAS.
>
> Para empezar a usar tokens de SAS, debe quitar UserIdentity, lo que se explica en el siguiente [artículo](develop-storage-files-storage-access-control.md#disable-forcing-azure-ad-pass-through).
>
> De manera predeterminada SQL a petición siempre usa el paso a través de AAD.

Para más información sobre cómo administrar el control de acceso de almacenamiento, consulte este [vínculo](develop-storage-files-storage-access-control.md).

> [!WARNING]
> Necesita crear credenciales para una cuenta de almacenamiento que se encuentre en la región del punto de conexión. Aunque SQL a petición puede acceder a los almacenamientos desde distintas regiones, tener el almacenamiento y el punto de conexión en la misma región proporcionará una mejor experiencia de rendimiento.

Para crear credenciales para contenedores CSV, JSON y Parquet, ejecute el siguiente código:

```sql
-- create credentials for CSV container in our demo storage account
IF EXISTS (SELECT * FROM sys.credentials WHERE name = 'https://sqlondemandstorage.blob.core.windows.net/csv')
DROP CREDENTIAL [https://sqlondemandstorage.blob.core.windows.net/csv];
GO

CREATE CREDENTIAL [https://sqlondemandstorage.blob.core.windows.net/csv]
WITH IDENTITY='SHARED ACCESS SIGNATURE',  
SECRET = 'sv=2018-03-28&ss=bf&srt=sco&sp=rl&st=2019-10-14T12%3A10%3A25Z&se=2061-12-31T12%3A10%3A00Z&sig=KlSU2ullCscyTS0An0nozEpo4tO5JAgGBvw%2FJX2lguw%3D';
GO

-- create credentials for JSON container in our demo storage account
IF EXISTS (SELECT * FROM sys.credentials WHERE name = 'https://sqlondemandstorage.blob.core.windows.net/json')
DROP CREDENTIAL [https://sqlondemandstorage.blob.core.windows.net/json];
GO

CREATE CREDENTIAL [https://sqlondemandstorage.blob.core.windows.net/json]
WITH IDENTITY='SHARED ACCESS SIGNATURE',  
SECRET = 'sv=2018-03-28&ss=bf&srt=sco&sp=rl&st=2019-10-14T12%3A10%3A25Z&se=2061-12-31T12%3A10%3A00Z&sig=KlSU2ullCscyTS0An0nozEpo4tO5JAgGBvw%2FJX2lguw%3D';
GO

-- create credentials for PARQUET container in our demo storage account
IF EXISTS (SELECT * FROM sys.credentials WHERE name = 'https://sqlondemandstorage.blob.core.windows.net/parquet')
DROP CREDENTIAL [https://sqlondemandstorage.blob.core.windows.net/parquet];
GO

CREATE CREDENTIAL [https://sqlondemandstorage.blob.core.windows.net/parquet]
WITH IDENTITY='SHARED ACCESS SIGNATURE',  
SECRET = 'sv=2018-03-28&ss=bf&srt=sco&sp=rl&st=2019-10-14T12%3A10%3A25Z&se=2061-12-31T12%3A10%3A00Z&sig=KlSU2ullCscyTS0An0nozEpo4tO5JAgGBvw%2FJX2lguw%3D';
GO
```

## <a name="provided-demo-data"></a>Datos de demostración proporcionados

Los datos de demostración contienen los siguientes conjuntos de datos:

- NYC Taxi - Yellow Taxi Trip Records: una parte del conjunto de datos públicos de Nueva York
  - Formato CSV
  - Formato Parquet
- Conjunto de datos de población
  - Formato CSV
- Archivos con formato Parquet de ejemplo con columnas anidadas
  - Formato Parquet
- Libros JSON
  - Formato JSON

| Ruta de acceso a la carpeta                                                  | Descripción                                                  |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| /csv/                                                        | Carpeta principal para los datos en formato CSV                         |
| /csv/population/<br />/csv/population-unix/<br />/csv/population-unix-hdr/<br />/csv/population-unix-hdr-escape<br />/csv/population-unix-hdr-quoted | Carpetas con archivos de datos de población en diferentes formatos CSV. |
| /csv/taxi/                                                   | Carpeta con archivos de datos públicos de Nueva York en formato CSV              |
| /parquet/                                                    | Carpeta principal para los datos en formato Parquet                     |
| /parquet/taxi                                                | Archivos de datos públicos de Nueva York en formato Parquet, divididos por año y mes mediante el esquema de partición Hive/Hadoop. |
| /parquet/nested/                                             | Archivos con formato Parquet de ejemplo con columnas anidadas                     |
| /json/                                                       | Carpeta principal para los datos en formato JSON                        |
| /json/books/                                                 | Archivos JSON con datos de libros                                   |

## <a name="validation"></a>Validación

Ejecute las tres consultas siguientes y compruebe si las credenciales se crean correctamente.

> [!NOTE]
> Todos los identificadores URI de las consultas de ejemplo usa una cuenta de almacenamiento ubicada en la región Norte de Europa de Azure. Asegúrese de que ha creado la credencial adecuada. Ejecute la consulta siguiente y asegúrese de que la cuenta de almacenamiento aparece en la lista.

```sql
SELECT name
FROM sys.credentials
WHERE
     name IN ( 'https://sqlondemandstorage.blob.core.windows.net/csv',
     'https://sqlondemandstorage.blob.core.windows.net/parquet',
     'https://sqlondemandstorage.blob.core.windows.net/json');
```

Si no encuentra la credencial adecuada, consulte la sección en la que se explica la [primera configuración](#first-time-setup).

### <a name="sample-query"></a>Consulta de ejemplo

El último paso de la validación es ejecutar la siguiente consulta:

```sql
SELECT
    COUNT_BIG(*)
FROM  
    OPENROWSET(
        BULK 'https://sqlondemandstorage.blob.core.windows.net/parquet/taxi/year=2017/month=9/*.parquet',
        FORMAT='PARQUET'
    ) AS nyc;
```

La consulta anterior debería devolver este número: **8945574**.

## <a name="next-steps"></a>Pasos siguientes

Ya está preparado para continuar con los siguientes artículos de procedimientos:

- [Consulta de archivos .csv](query-single-csv-file.md)

- [Consulta de carpetas y varios archivos .csv](query-folders-multiple-csv-files.md)

- [Consulta de archivos específicos](query-specific-files.md)

- [Consulta de archivos con formato Parquet](query-parquet-files.md)

- [Consulta de tipos anidados con formato Parquet ](query-parquet-nested-types.md)

- [Consulta de archivos JSON](query-json-files.md)

- [Creación y uso de vistas](create-use-views.md)

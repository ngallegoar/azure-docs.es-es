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
ms.openlocfilehash: 8501f9d07ffa2d04915d4d1a351317cc145f9844
ms.sourcegitcommit: 6a9f01bbef4b442d474747773b2ae6ce7c428c1f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/27/2020
ms.locfileid: "84118264"
---
# <a name="overview-query-data-in-storage"></a>Introducción: consulta de los datos en el almacenamiento

Esta sección contiene consultas de ejemplo que puede usar para probar el recurso de SQL a petición (versión preliminar) en Azure Synapse Analytics.
Los formatos de archivo compatibles actualmente son los siguientes:  
- CSV
- Parquet
- JSON

## <a name="prerequisites"></a>Requisitos previos

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

El primer paso es **crear una base de datos** en la que se ejecutarán las consultas. Luego, se inicializan los objetos, para lo que hay que ejecutar un [script de instalación](https://github.com/Azure-Samples/Synapse/blob/master/SQL/Samples/LdwSample/SampleDB.sql) en esa base de datos. Este script de instalación creará los orígenes de datos, las credenciales con ámbito de base de datos y los formatos de archivo externos que se usan para leer datos en estos ejemplos.

> [!NOTE]
> Las bases de datos se usan solo para ver metadatos, no para datos reales.  Anote el nombre de la base de datos que use, ya que lo necesitará más adelante.

```sql
CREATE DATABASE mydbname;
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

### <a name="sample-query"></a>Consulta de ejemplo

El último paso de la validación es ejecutar la siguiente consulta:

```sql
SELECT
    COUNT_BIG(*)
FROM  
    OPENROWSET(
        BULK 'parquet/taxi/year=2017/month=9/*.parquet',
        DATA_SOURCE = 'sqlondemanddemo',
        FORMAT='PARQUET'
    ) AS nyc;
```

La consulta anterior debería devolver este número: **8945574**.

## <a name="next-steps"></a>Pasos siguientes

Ya está preparado para continuar con los siguientes artículos de procedimientos:

- [Consulta de archivos .csv](query-single-csv-file.md)

- [Consulta de carpetas y varios archivos .csv](query-folders-multiple-csv-files.md)

- [Consulta de archivos específicos](query-specific-files.md)

- [Consulta de archivos Parquet](query-parquet-files.md)

- [Consulta de tipos anidados de Parquet](query-parquet-nested-types.md)

- [Consulta de archivos JSON](query-json-files.md)

- [Creación y uso de vistas](create-use-views.md)

---
title: Procedimientos recomendados de SQL a petición (versión preliminar) en Azure Synapse Analytics
description: Recomendaciones y procedimientos recomendados que debe saber para trabajar con SQL a petición (versión preliminar).
services: synapse-analytics
author: mlee3gsd
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: ''
ms.date: 04/15/2020
ms.author: martinle
ms.reviewer: igorstan
ms.openlocfilehash: 1d4203141973c10fe7673f6ab9dedbc3bfdc8999
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/16/2020
ms.locfileid: "81426784"
---
# <a name="best-practices-for-sql-on-demand-preview-in-azure-synapse-analytics"></a>Procedimientos recomendados de SQL a petición (versión preliminar) en Azure Synapse Analytics

En este artículo, encontrará una colección de procedimientos recomendados para usar SQL a petición (versión preliminar). SQL a petición es un recurso adicional incluido en Azure Synapse Analytics.

## <a name="general-considerations"></a>Consideraciones generales

SQL a petición permite consultar archivos de las cuentas de almacenamiento de Azure. No tiene funcionalidades de ingesta o almacenamiento local. Por ello, todos los archivos de destino de la consulta son externos a SQL a petición. Todo lo relacionado con la lectura de archivos desde el almacenamiento puede afectar el rendimiento de las consultas.

## <a name="colocate-azure-storage-account-and-sql-on-demand"></a>Colocación de una cuenta de Azure Storage y SQL a petición

Para minimizar la latencia, colocalice su cuenta de Azure Storage y el punto de conexión de SQL a petición. Las cuentas de almacenamiento y los puntos de conexión aprovisionados durante la creación del área de trabajo se encuentran en la misma región.

Para obtener un rendimiento óptimo, si tiene acceso a otras cuentas de almacenamiento con SQL a petición, asegúrese de que se encuentran en la misma región. Si no están en la misma región, aumentará la latencia de la transferencia de red de los datos entre la región remota y la del punto de conexión.

## <a name="azure-storage-throttling"></a>Limitaciones de Azure Storage

Varias aplicaciones y servicios pueden acceder a la cuenta de almacenamiento. Las limitaciones de Storage se producen cuando las IOPS o el rendimiento combinados que generan las aplicaciones, los servicios y la carga de trabajo a petición de SQL superan los límites de la cuenta de almacenamiento. Como resultado, se producirá un efecto negativo significativo en el rendimiento de las consultas.

Una vez detectada la limitación, SQL a petición tiene controles integrados para este escenario. SQL a petición realizará solicitudes al almacenamiento a un ritmo más lento hasta que la limitación se resuelva.

> [!TIP]
> Para que la ejecución de las consultas sea óptima, no debe forzar la cuenta de almacenamiento con otras cargas de trabajo durante la ejecución de consultas.

## <a name="prepare-files-for-querying"></a>Preparación de los archivos para la consulta

Si es posible, puede preparar los archivos para mejorar el rendimiento:

- Convierta los archivos CSV a Parquet: Parquet es un formato de columnas. Dado que está comprimido, sus tamaños de archivo son más pequeños que los archivos CSV con los mismos datos. SQL a petición necesitará menos tiempo y solicitudes de almacenamiento para leerlos.
- Si una consulta tiene como destino un solo archivo de gran tamaño, se beneficiará de dividirlo en varios archivos más pequeños.
- Pruebe a mantener el tamaño del archivo CSV por debajo de los 10 GB.
- Es mejor tener archivos de igual tamaño para una sola ruta de acceso OPENROWSET o una ubicación de tabla externa.
- Particione los datos al almacenar las particiones en diferentes carpetas o con nombres de archivo distintos: consulte [Uso de las funciones filename y filepath para seleccionar particiones específicas](#use-fileinfo-and-filepath-functions-to-target-specific-partitions).

## <a name="use-fileinfo-and-filepath-functions-to-target-specific-partitions"></a>Uso de las funciones fileinfo y filepath para seleccionar particiones específicas

A menudo, los datos se organizan en particiones. Puede indicar a SQL a petición que consulte archivos y carpetas concretos. Esta función reduce el número de archivos y la cantidad de datos que la consulta necesita leer y procesar. Como ventaja adicional, logrará un mejor rendimiento.

Para obtener más información, consulte las funciones [filename](develop-storage-files-overview.md#filename-function) y [filepath](develop-storage-files-overview.md#filepath-function), así como ejemplos sobre cómo [consultar archivos específicos](query-specific-files.md).

Si los datos almacenados no tienen particiones, considere la posibilidad de crear particiones para usar estas funciones en la optimización de las consultas que están dirigidas a esos archivos. Cuando [consulte las tablas de Spark con particiones](develop-storage-files-spark-tables.md) de SQL a petición, la consulta tendrá como destino automático solo los archivos necesarios.

## <a name="use-cetas-to-enhance-query-performance-and-joins"></a>Uso de CETAS para mejorar el rendimiento de las consultas y las combinaciones

[CETAS](develop-tables-cetas.md) es una de las características más importantes que están disponibles en SQL a petición. CETAS es una operación en paralelo que crea metadatos de tabla externa y exporta los resultados de la consulta SELECT a un conjunto de archivos de la cuenta de almacenamiento.

Puede usar CETAS para almacenar en un nuevo conjunto de archivos las partes más frecuentemente usadas de las consultas, como las tablas de referencia combinadas. A continuación, puede realizar combinaciones con esta tabla externa única, en lugar de repetir combinaciones comunes en varias consultas.

Cuando CETAS genera archivos Parquet, las estadísticas se crean automáticamente cuando la primera consulta selecciona como destino a esta tabla externa, lo que mejora el rendimiento.

## <a name="next-steps"></a>Pasos siguientes

Consulte el artículo [Solución de problemas](../sql-data-warehouse/sql-data-warehouse-troubleshoot.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json) para conocer los problemas comunes y sus soluciones. Si va a trabajar con el grupo de SQL en lugar de SQL a petición, consulte el artículo [Procedimientos recomendados para el grupo de SQL](best-practices-sql-pool.md) para obtener instrucciones específicas.

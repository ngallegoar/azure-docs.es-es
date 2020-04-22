---
title: Procedimientos recomendados de desarrollo para SQL de Synapse
description: Recomendaciones y procedimientos recomendados que debe conocer para el desarrollo en SQL de Synapse.
services: synapse-analytics
author: XiaoyuMSFT
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: ''
ms.date: 04/15/2020
ms.author: xiaoyul
ms.reviewer: igorstan
ms.openlocfilehash: 3356817fbaef955e05358f9c92619f3c04f747d7
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/16/2020
ms.locfileid: "81426952"
---
# <a name="development-best-practices-for-synapse-sql"></a>Procedimientos recomendados de desarrollo para SQL de Synapse
En este artículo se describen la guía y los procedimientos recomendados a medida que desarrolla una solución de almacenamiento de datos. 

## <a name="development-best-practices-for-synapse-sql"></a>Procedimientos recomendados de desarrollo para SQL de Synapse

### <a name="reduce-cost-with-pause-and-scale"></a>Menos costos gracias a las características de pausa y escalado

Para más información acerca de cómo reducir los costos mediante la pausa y el escalado, consulte [Administración de procesos](../sql-data-warehouse/sql-data-warehouse-manage-compute-overview.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json).

### <a name="maintain-statistics"></a>Mantenimiento de estadísticas

Asegúrese de actualizar las estadísticas diariamente o después de cada carga.  Existen inconvenientes entre el rendimiento y el costo de crear y actualizar las estadísticas. Si cree que tarda demasiado en realizar el mantenimiento de todas las estadísticas, sea más selectivo sobre qué columnas tienen estadísticas o cuáles necesitan actualizarse con frecuencia.  

Por ejemplo, puede actualizar las columnas de fecha, en las que se pueden añadir valores nuevos todos los días. **Sacará el máximo provecho con las estadísticas en las columnas relacionadas con combinaciones, las columnas que se usan en la cláusula WHERE y las columnas de GROUP BY.**

Consulte también [Administración de estadísticas en tablas](develop-tables-statistics.md), [CREATE STATISTICS](develop-tables-statistics.md) y [UPDATE STATISTICS][UPDATE STATISTICS].

### <a name="hash-distribute-large-tables"></a>Distribución Hash para tablas grandes

De forma predeterminada, las tablas se distribuyen según el patrón Round Robin.  Esto facilita a los usuarios la tarea de comenzar a crear tablas sin tener que decidir sobre la distribución.  El rendimiento de las tablas round robin puede ser suficiente para algunas cargas de trabajo. Sin embargo, en la mayoría de los casos, seleccionar una columna de distribución funcionará mucho mejor.  

El ejemplo más común de tabla distribuida por una columna que supera con creces a una Round Robin es al combinarse dos tablas grandes de hechos.  Por ejemplo, si tiene una tabla de pedidos que se distribuye por order_id, y una tabla de transacciones que también se distribuye por order_id, al unir la tabla de pedidos a la de transacciones en order_id, esta consulta se convierte en una consulta de paso a través. Esto significa que se eliminan las operaciones de movimiento de datos.  Menos pasos suponen consultas más rápidas.  Menos movimiento de datos también se traduce en consultas más rápidas.

Al cargar una tabla con distribución, asegúrese de que no se ordenan los datos entrantes en la clave de distribución, ya que esto ralentizará la carga.  Consulte los siguientes vínculos para conocer detalles adicionales sobre cómo la selección de una columna de distribución mejora el rendimiento y cómo definir una tabla distribuida en la cláusula WITH de la instrucción CREATE TABLES.

Consulte también [Información general de tablas][Table overview], [Distribución de tablas][Table distribution], [Selección de la distribución de tablas][Selecting table distribution], [CREATE TABLE][CREATE TABLE], [CREATE TABLE AS SELECT][CREATE TABLE AS SELECT].

### <a name="do-not-over-partition"></a>Sin particiones excesivas
Crear particiones de datos puede resultar eficaz para el mantenimiento de los datos mediante la modificación de particiones o exámenes de optimización, pero el exceso de particiones puede ralentizar las consultas.  A menudo una estrategia de creación de particiones con granularidad alta que puede funcionar bien en SQL Server no funciona correctamente en el grupo de SQL.  

El exceso de particiones también puede reducir la eficacia de los índices de almacén de columnas agrupadas si cada partición tiene menos de 1 millón de filas. El grupo de SQL crea particiones de datos en su nombre en 60 bases de datos. Por lo tanto, si crea una tabla con 100 particiones, el resultado serán 6000 particiones.  Cada carga de trabajo es diferente, por lo mejor es probar con las particiones para ver qué funciona mejor para la suya.  

Una opción que debe tener en cuenta consiste en usar una granularidad inferior a la que le funcionaba en SQL Server.  Por ejemplo, puede usar particiones semanales o mensuales, en lugar de diarias.

Consulte también [Creación de particiones de tablas][Table partitioning].

### <a name="minimize-transaction-sizes"></a>Reducción del tamaño de las transacciones

Las instrucciones INSERT, UPDATE Y DELETE se ejecutan en las transacciones y, cuando fallan, deben deshacerse.  Para que no se tarde tanto en deshacer, reduzca el tamaño de las transacciones siempre que pueda.  Puede hacerlo si divide las instrucciones INSERT, UPDATE y DELETE en partes.  Por ejemplo, si tiene una instrucción INSERT que se suele tardar 1 hora, puede dividirla en cuatro partes, por lo que cada ejecución duraría 15 minutos.

Aproveche los casos de registro mínimo, como CTAS, TRUNCATE, DROP TABLE o INSERT para vaciar las tablas y así reducir el riesgo de reversión.  Otra manera de eliminar reversiones es usar funciones de solo metadatos, como la modificación de particiones para la administración de datos.  Por ejemplo, en lugar de ejecutar una instrucción DELETE para eliminar todas las filas de una tabla cuyo order_date fuera octubre de 2001, podría dividir los datos mensualmente y desactivar la división con los datos de una partición vacía de otra tabla (consulte los ejemplos de ALTER TABLE).  

Para tablas sin divisiones, puede usar CTAS en lugar de DELETE para escribir los datos que quiera mantener en una tabla.  Si CTAS tarda lo mismo, es una operación mucho más segura, ya que su registro de transacciones es mínimo y se puede cancelar rápidamente si es necesario.

Consulte también [Transacciones en SQL Data Warehouse][Understanding transactions], [Optimización de transacciones][Optimizing transactions], [Creación de particiones de tablas][Table partitioning], [TRUNCATE TABLE][TRUNCATE TABLE], [ALTER TABLE][ALTER TABLE], [Create table as select (CTAS)][Create table as select (CTAS)].

### <a name="use-the-smallest-possible-column-size"></a>Uso del tamaño de columna mínimo

Al definir el DDL, usar el tipo de datos mínimo compatible con los datos mejorará el rendimiento de la consulta.  Esto tiene especial importancia para las columnas CHAR y VARCHAR.  Si el valor mayor máximo de una columna es 25 caracteres, defina la columna como VARCHAR(25).  Evite definir todas las columnas de caracteres con una longitud predeterminada de gran tamaño.  Defina las columnas como VARCHAR en lugar de NVARCHAR cuando no se necesite nada más.

Consulte también [Información general sobre las tablas][Table overview], [Tipos de datos de tabla](develop-tables-data-types.md), [CREATE TABLE][CREATE TABLE].

### <a name="optimize-clustered-columnstore-tables"></a>Optimización de tablas de almacén de columnas agrupadas

Los índices de almacén de columnas agrupadas son una de las maneras más eficaces para almacenar datos en el grupo de SQL.  De forma predeterminada, las tablas del grupo de SQL se crean como almacén de columnas agrupadas.  Para conseguir el máximo rendimiento de las consultas en las tablas de almacén de columnas, es importante la calidad de los segmentos.  Escriben filas en las tablas de almacén de columnas bajo presión de memoria afecta a la calidad de segmento.  

La calidad de segmento se puede medir por el número de filas de un grupo de filas comprimido.  Consulte la sección [Causas de una calidad deficiente del índice de almacén de columnas][Causes of poor columnstore index quality] del artículo [Índices de tablas][Table indexes] para obtener instrucciones paso a paso sobre la detección y mejora de la calidad de los segmentos para las tablas de almacén de columnas agrupadas.  Como es importante que los segmentos de almacén de columnas sean de una buena calidad, es conveniente usar identificadores de usuario que se encuentren en la clase de recursos grande o mediana para cargar los datos. El uso de [unidades de almacenamiento de datos](azure -synapse-resource-consumption-models.md) inferiores significa que desea asignar una clase de recurso mayor para el usuario que realiza la carga.

Dado que las tablas de almacén de columnas, generalmente, no insertan datos en un segmento del almacén de columnas comprimido hasta que hay más de 1 millón de filas por tabla y cada tabla del grupo de SQL se divide en 60 partes, las tablas de almacén de columnas no serán útiles para las consultas a menos que la tabla tenga más de 60 millones de filas.  En el caso de las tablas con menos de 60 millones de filas, un índice de almacén de columnas puede no ser la solución óptima.  

Además, si divide los datos, recuerde que cada parte deberá tener 1 millón de filas para beneficiarse de un índice de almacén de columnas agrupadas.  Si una tabla tiene 100 particiones, deberá tener al menos 6 mil millones de filas para beneficiarse del almacén de columnas agrupadas (60 distribuciones *100 particiones* 1 millón de filas).  

Si la tabla no tiene 6 mil millones de filas, reduzca el número de particiones o considere la posibilidad de usar una tabla de montón en su lugar.  También puede experimentar para ver si consigue un mejor rendimiento mediante una tabla de montón con índices secundarios, en lugar de con una tabla de almacén de columnas.

Al consultar una tabla de almacén de columnas, las consultas se ejecutarán más rápido si selecciona solo las que necesita.  

Consulte también [Índices de tablas][Table indexes], [Guía de índices de almacén de columnas][Columnstore indexes guide] y [Recompilación de índices de almacén de columnas][Rebuilding columnstore indexes].

### <a name="next-steps"></a>Pasos siguientes

Si no encuentra lo que buscaba en este artículo, pruebe a usar la "búsqueda de documentos" en la parte izquierda de esta página para buscar todos los documentos del grupo de Azure SQL.  El [foro del grupo de Azure SQL][Azure SQL pool MSDN Forum] es un lugar para plantear preguntas a otros usuarios y al grupo de productos del grupo de SQL.  

Supervisamos continuamente este foro para garantizar que sus preguntas las responde otro usuario o alguno de nosotros.  Si prefiere formular sus preguntas en Stack Overflow, también hay disponible un [foro de Stack Overflow acerca del grupo de Azure SQL][Azure SQL pool Stack Overflow Forum].

Use la página de [comentarios del grupo de Azure SQL][Azure SQL pool Feedback] para realizar solicitudes de características.  Añadir sus solicitudes o valoraciones positivas sobre otras solicitudes realmente nos ayuda a priorizar las características.

<!--Image references-->

<!--Article references-->
[Create a support ticket]: ../sql-data-warehouse/sql-data-warehouse-get-started-create-support-ticket.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json
[Create table as select (CTAS)]: ../sql-data-warehouse/sql-data-warehouse-develop-ctas.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json
[Table overview]:develop-tables-overview.md
[Table distribution]: ../sql-data-warehouse/sql-data-warehouse-tables-distribute.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json
[Table indexes]: ../sql-data-warehouse/sql-data-warehouse-tables-index.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json
[Causes of poor columnstore index quality]: ../sql-data-warehouse/sql-data-warehouse-tables-index.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json#causes-of-poor-columnstore-index-quality
[Rebuilding columnstore indexes]: ../sql-data-warehouse/sql-data-warehouse-tables-index.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json#rebuilding-indexes-to-improve-segment-quality
[Table partitioning]: ../sql-data-warehouse/sql-data-warehouse-tables-partition.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json
[Temporary tables]:develop-tables-temporary.md
[Guide for using PolyBase]:../sql-analytics/data-loading-best-practices.md
[Load data]: ../sql-data-warehouse/esign-elt-data-loading.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json
[Move data with Azure Data Factory]: ../../data-factory/transform-data-using-machine-learning.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json
[Load data with Azure Data Factory]: ../../data-factory/load-azure-sql-data-warehouse.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json
[Load data with bcp]: /sql/tools/bcp-utility
[Load data with PolyBase]: ../sql-data-warehouse/load-data-wideworldimportersdw.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json
[Monitor your workload using DMVs]: ../sql-data-warehouse/sql-data-warehouse-manage-monitor.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json
[Pause compute resources]: ../sql-data-warehouse/sql-data-warehouse-manage-compute-overview.md#pause-compute-bk?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json
[Resume compute resources]: ../sql-data-warehouse/sql-data-warehouse-manage-compute-overview.md#resume-compute-bk?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json
[Scale compute resources]: ../sql-data-warehouse/sql-data-warehouse-manage-compute-overview.md#scale-compute?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json
[Understanding transactions]:develop-transactions.md
[Optimizing transactions]: ../sql-data-warehouse/sql-data-warehouse-develop-best-practices-transactions.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json
[Troubleshooting]: ../sql-data-warehouse/sql-data-warehouse-troubleshoot.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json
[LABEL]:develop-label.md

<!--MSDN references-->
[ALTER TABLE]: /sql/t-sql/statements/alter-table-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest
[CREATE EXTERNAL FILE FORMAT]: /sql/t-sql/statements/create-external-file-format-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest
[CREATE STATISTICS]: /sql/t-sql/statements/create-statistics-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest
[CREATE TABLE]: /sql/t-sql/statements/create-table-azure-sql-data-warehouse?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest
[CREATE TABLE AS SELECT]: /sql/t-sql/statements/create-table-as-select-azure-sql-data-warehouse?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest
[DBCC PDW_SHOWEXECUTIONPLAN]: /sql/t-sql/database-console-commands/dbcc-pdw-showexecutionplan-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest
[INSERT]: /sql/t-sql/statements/insert-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest
[OPTION]: /sql/t-sql/queries/option-clause-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest
[TRUNCATE TABLE]: /sql/t-sql/statements/truncate-table-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest
[UPDATE STATISTICS]: /sql/t-sql/statements/update-statistics-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest
[sys.dm_exec_sessions]: /sql/relational-databases/system-dynamic-management-views/sys-dm-exec-sessions-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest
[sys.dm_pdw_exec_requests]: /sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-exec-requests-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest
[sys.dm_pdw_request_steps]: /sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-request-steps-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest
[sys.dm_pdw_sql_requests]: /sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-sql-requests-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest
[sys.dm_pdw_dms_workers]: /sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-dms-workers-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest
[sys.dm_pdw_waits]: /sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-waits-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest
[Columnstore indexes guide]: /sql/relational-databases/indexes/columnstore-indexes-overview?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest

<!--Other Web references-->
[Selecting table distribution]: https://blogs.msdn.microsoft.com/sqlcat/20../../choosing-hash-distributed-table-vs-round-robin-distributed-table-in-azure-sql-dw-service/
[Azure SQL pool Feedback]: https://feedback.azure.com/forums/307516-sql-data-warehouse
[Azure SQL pool MSDN Forum]: https://social.msdn.microsoft.com/Forums/sqlserver/home?forum=AzureSQLDataWarehouse
[Azure SQL pool Stack Overflow Forum]:  https://stackoverflow.com/questions/tagged/azure-sqldw
[Azure SQL pool loading patterns and strategies]: https://blogs.msdn.microsoft.com/sqlcat/20../../azure-sql-data-warehouse-loading-patterns-and-strategies/

## <a name="development-best-practices-for-sql-on-demand-preview"></a>Procedimientos recomendados de desarrollo para SQL a petición (versión preliminar)

### <a name="general-considerations"></a>Consideraciones generales

SQL a petición permite consultar archivos de las cuentas de almacenamiento de Azure. No tiene funcionalidades de almacenamiento local o ingesta, lo que significa que todos los archivos que la consulta tiene como destino son externos a SQL a petición. Por lo tanto, todo lo relacionado con la lectura de archivos desde el almacenamiento puede afectar el rendimiento de las consultas.

### <a name="colocate-azure-storage-account-and-sql-on-demand"></a>Colocación de una cuenta de Azure Storage y SQL a petición

 
Para minimizar la latencia, coloque su cuenta de Azure Storage y el punto de conexión de SQL a petición. Las cuentas de almacenamiento y los puntos de conexión aprovisionados durante la creación del área de trabajo se encuentran en la misma región. 
 
Para obtener un rendimiento óptimo, si tiene acceso a otras cuentas de almacenamiento con SQL a petición, asegúrese de que se encuentran en la misma región. De lo contrario, aumentará la latencia de la transferencia de red de los datos de la región remota a la región del punto de conexión.

### <a name="azure-storage-throttling"></a>Limitaciones de Azure Storage

Varias aplicaciones y servicios pueden acceder a la cuenta de almacenamiento. Cuando las IOPS o el rendimiento combinados que generan las aplicaciones, los servicios y la carga de trabajo a petición de SQL superan los límites de la cuenta de almacenamiento, se producen las limitaciones de Storage. Cuando se produce la limitación de almacenamiento, hay un efecto negativo significativo en el rendimiento de las consultas. 
 
Una vez detectada la limitación, SQL a petición tiene controles integrados para este escenario. SQL a petición realizará solicitudes al almacenamiento a un ritmo más lento hasta que la limitación se resuelva. Sin embargo, para que la ejecución de las consultas sea óptima, se recomienda que no fuerce la cuenta de almacenamiento con otras cargas de trabajo durante la ejecución de consultas.

### <a name="prepare-files-for-querying"></a>Preparación de los archivos para la consulta

Si es posible, puede preparar los archivos para mejorar el rendimiento:

- Convierta los archivos CSV a Parquet: Parquet es un formato de columnas. Dado que se trata de un formato comprimido, los tamaños de archivo son menores que los de los archivos CSV con los mismos datos, y SQL a petición necesita menos tiempo y solicitudes de almacenamiento para leerlos.
- Si una consulta tiene como destino un solo archivo de gran tamaño, se beneficiará de dividirlo en varios archivos más pequeños.
- Pruebe a mantener el tamaño del archivo CSV por debajo de los 10 GB.
- Es preferible tener archivos de igual tamaño para una sola ruta de acceso OPENROWSET o una ubicación de tabla externa.
- Particione los datos al almacenar las particiones en diferentes carpetas o con nombres de archivo distintos: consulte [Uso de las funciones filename y filepath para seleccionar particiones específicas](#use-fileinfo-and-filepath-functions-to-target-specific-partitions).

### <a name="use-fileinfo-and-filepath-functions-to-target-specific-partitions"></a>Uso de las funciones fileinfo y filepath para seleccionar particiones específicas

A menudo, los datos se organizan en particiones. Puede indicar a SQL a petición que consulte archivos y carpetas concretos. Esta acción reduce el número de archivos y la cantidad de datos que la consulta necesita leer y procesar. Por lo tanto, conseguirá un mejor rendimiento. Para obtener más información, consulte las funciones [filename](develop-storage-files-overview.md#filename-function) y [filepath](develop-storage-files-overview.md#filepath-function), así como ejemplos sobre cómo [consultar archivos específicos](query-specific-files.md).

Si los datos en el almacenamiento no tienen particiones, considere la posibilidad de crear particiones para usar estas funciones en la optimización de las consultas que están dirigidas a esos archivos.

Cuando [consulte las tablas de Spark con particiones](develop-storage-files-spark-tables.md) de SQL a petición, la consulta tendrá como destino automático solo los archivos necesarios.

### <a name="use-cetas-to-enhance-query-performance-and-joins"></a>Uso de CETAS para mejorar el rendimiento de las consultas y las combinaciones

[CETAS](develop-tables-cetas.md) es una de las características más importantes que están disponibles en SQL a petición. CETAS es una operación en paralelo que crea metadatos de tabla externa y exporta el resultado de la consulta SELECT a un conjunto de archivos de la cuenta de almacenamiento.

Puede usar CETAS para almacenar en un nuevo conjunto de archivos las partes de las consultas que se usan frecuentemente, como las tablas de referencia combinadas. Más adelante, podrá realizar combinaciones con esta tabla externa única, en lugar de repetir combinaciones comunes en varias consultas. Cuando CETAS genera archivos Parquet, las estadísticas se crean automáticamente cuando la primera consulta selecciona como destino a esta tabla externa y se mejora el rendimiento.

---
title: Procedimientos recomendados para el grupo de SQL de Synapse en Azure Synapse Analytics (anteriormente SQL DW)
description: Recomendaciones y procedimientos recomendados para el desarrollo de soluciones para el grupo de SQL en Azure Synapse Analytics (anteriormente SQL DW).
services: synapse-analytics
author: mlee3gsd
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: ''
ms.date: 11/04/2019
ms.author: martinle
ms.reviewer: igorstan
ms.openlocfilehash: ce1121a4a006e4208c76193a38262d4309e67584
ms.sourcegitcommit: 0b80a5802343ea769a91f91a8cdbdf1b67a932d3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/25/2020
ms.locfileid: "83834448"
---
# <a name="best-practices-for-synapse-sql-pool-in-azure-synapse-analytics-formerly-sql-dw"></a>Procedimientos recomendados para el grupo de SQL de Synapse en Azure Synapse Analytics (anteriormente SQL DW)

Este artículo es una recopilación de procedimientos recomendados que le ayudará a conseguir un rendimiento óptimo de la implementación del [grupo de SQL](sql-data-warehouse-overview-what-is.md).  El propósito de este artículo es proporcionarle algunas instrucciones básicas y resaltar las áreas de atención importantes.  

## <a name="reduce-cost-with-pause-and-scale"></a>Menos costos gracias a las características de pausa y escalado

Para más información acerca de cómo reducir los costos mediante la pausa y el escalado, consulte [Administración de procesos](sql-data-warehouse-manage-compute-overview.md).

## <a name="maintain-statistics"></a>Mantenimiento de estadísticas

El grupo de SQL se puede configurar para detectar y crear estadísticas automáticamente con información de las columnas.  Los planes de consulta que crea el optimizador son igual de buenos que las estadísticas disponibles.  

Se recomienda habilitar AUTO_CREATE_STATISTICS para las bases de datos y mantener actualizadas las estadísticas a diario o después de cada carga. Así, se asegurará de que las estadísticas de las columnas utilizadas en las consultas siempre estén actualizadas.

Si cree que tarda demasiado en realizar la actualización de todas las estadísticas, puede intentar ser más selectivo acerca de las columnas que necesitan actualizar sus estadísticas con frecuencia. Por ejemplo, puede actualizar las columnas de fecha, donde se añadan valores todos los días.

> [!TIP]
> Sacará el máximo provecho con las estadísticas actualizadas en columnas relacionadas con combinaciones, columnas que se usan en la cláusula WHERE y columnas de GROUP BY.

Consulte también [Administración de estadísticas en tablas](sql-data-warehouse-tables-statistics.md), [CREATE STATISTICS](/sql/t-sql/statements/create-statistics-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest) y [UPDATE STATISTICS](/sql/t-sql/statements/update-statistics-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest).

## <a name="use-dmvs-to-monitor-and-optimize-your-queries"></a>Uso de vistas de administración dinámica (DMV) para supervisar y optimizar las consultas

El grupo de SQL tiene varias DMV que sirven para supervisar la ejecución de consultas.  En el artículo sobre supervisión de la carga de trabajo mediante DMV se detallan las instrucciones paso a paso para ver los detalles de una consulta en ejecución.  

Usar la opción LABEL con las consultas puede ayudar a encontrar rápidamente las consultas en estas DMV.

Consulte también [Supervisión de la carga de trabajo mediante DMV](sql-data-warehouse-manage-monitor.md), [LABEL](sql-data-warehouse-develop-label.md), [OPTION](/sql/t-sql/queries/option-clause-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest), [sys.dm_exec_sessions](/sql/relational-databases/system-dynamic-management-views/sys-dm-exec-sessions-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest), [sys.dm_pdw_exec_requests](/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-exec-requests-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest), [sys.dm_pdw_request_steps](/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-request-steps-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest), [sys.dm_pdw_sql_requests](/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-sql-requests-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest), [sys.dm_pdw_dms_workers](/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-dms-workers-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest), [DBCC PDW_SHOWEXECUTIONPLAN](/sql/t-sql/database-console-commands/dbcc-pdw-showexecutionplan-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest) y [sys.dm_pdw_waits](/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-waits-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest).

## <a name="tune-query-performance-with-new-product-enhancements"></a>Optimización del rendimiento de las consultas con nuevas mejoras en el producto

- [Optimización del rendimiento con vistas materializadas](performance-tuning-materialized-views.md)
- [Optimización del rendimiento con el índice de almacén de columnas agrupado ordenado](performance-tuning-ordered-cci.md)
- [Ajuste del rendimiento con la copia en caché del conjunto de resultados](performance-tuning-result-set-caching.md)

## <a name="group-insert-statements-into-batches"></a>Agrupación de instrucciones INSERT en lotes

Una carga única en una tabla pequeña con una instrucción INSERT o incluso una recarga periódica de una búsqueda, puede ser adecuada para sus necesidades con una instrucción como `INSERT INTO MyLookup VALUES (1, 'Type 1')`.  

Sin embargo, si necesita cargar miles o millones de filas a lo largo del día, es posible que las instrucciones INSERT sencillas no sean suficientes.  En su lugar, desarrolle sus procesos para que se escriban en un archivo y otro proceso que se ejecute periódicamente y lo cargue.

Consulte también [INSERT](/sql/t-sql/statements/insert-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest).

## <a name="use-polybase-to-load-and-export-data-quickly"></a>Uso de PolyBase para cargar y exportar los datos rápidamente

El grupo de SQL admite la carga y exportación de datos con varias herramientas, como Azure Data Factory, PolyBase y BCP.  Para pequeñas cantidades de datos donde el rendimiento no es clave, cualquier herramienta le sirve.  Sin embargo, para cargar o exportar grandes volúmenes de datos o si se necesita un rendimiento rápido, PolyBase es la mejor opción.  

PolyBase está diseñado para aprovechar la estructura MPP (procesamiento masivo en paralelo) y carga y exporta grandes cantidades de datos más rápido que cualquier otra herramienta.  Lo que haya cargado con PolyBase se ejecuta con la consulta CTAS o de selección.  

> [!TIP]
> CTAS reduce el registro de transacciones y es la manera más rápida de cargar datos.

Azure Data Factory también admite cargas de PolyBase y puede lograr un rendimiento similar a CTAS.  PolyBase admite distintos de formatos de archivo, como Gzip.  
  
> [!NOTE]
> Con el fin de conseguir un mayor rendimiento al usar archivos de texto gzip, divídalos en 60 o más archivos para aumentar el paralelismo de la carga.  Para conseguir un rendimiento total más rápido, cargue los datos simultáneamente.

Consulte también [Carga de datos](design-elt-data-loading.md), [Guía para el uso de PolyBase](guidance-for-loading-data.md), [Patrones y estrategias de carga de grupo de SQL](https://blogs.msdn.microsoft.com/sqlcat/20../../), [Carga de datos con Azure Data Factory]( ../../data-factory/load-azure-sql-data-warehouse.md), [Movimiento de datos mediante Azure Data Factory](../../data-factory/transform-data-using-machine-learning.md), [CREATE EXTERNAL FILE FORMAT](/sql/t-sql/statements/create-external-file-format-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest) y [CREATE TABLE AS SELECT (CTAS)](sql-data-warehouse-develop-ctas.md).

## <a name="load-then-query-external-tables"></a>Carga y consulta de tablas externas

Aunque Polybase, también conocido ahora como tablas externas, puede ser la manera más rápida de cargar datos, no resulta óptimo para las consultas. Las tablas de Polybase solo admiten actualmente archivos de blobs de Azure y almacenamiento de Azure Data Lake Store. Estos archivos no tienen recursos de proceso que los respalde.  

Como resultado, el grupo de SQL no puede descargar este trabajo y, por tanto, para leer los datos del archivo entero, tiene que cargarlo en tempdb.  Por lo tanto, si tiene varias consultas que van a consultar estos datos, es mejor cargarlos una vez que las consultas usen la tabla local.

Consulte también [Guía para el uso de PolyBase](guidance-for-loading-data.md).

## <a name="hash-distribute-large-tables"></a>Distribución Hash para tablas grandes

De forma predeterminada, las tablas se distribuyen según el patrón Round Robin.  Esto facilita a los usuarios empezar a crear tablas sin tener que decidir sobre la distribución.  Las tablas round robin pueden ser suficientes para algunas cargas de trabajo, pero en la mayoría de los casos, la selección de una columna de distribución funcionará mucho opción.  

El ejemplo más común de tabla distribuida por una columna que supera con creces a una Round Robin es al combinarse dos tablas grandes de hechos.  

Por ejemplo, si tiene una tabla de pedidos, que se distribuye por order_id, y una tabla de transacciones, que también se distribuye por order_id, al unir la tabla de pedidos a la de transacciones en order_id, esta consulta se convierte en una consulta de paso a través, lo que significa que se eliminan las operaciones de movimiento de datos.  Menos pasos suponen consultas más rápidas.  Menos movimiento de datos también se traduce en consultas más rápidas.  

> [!TIP]
> Al cargar una tabla con distribución, asegúrese de que no se ordenan los datos entrantes en la clave de distribución, ya que esto ralentizará la carga.  

En los vínculos a continuación se muestra con más detalle cómo la selección de una columna de distribución puede mejorar el rendimiento y la manera de definir una tabla distribuida en la cláusula WITH de la instrucción CREATE TABLE.

Consulte también [Información general de tablas](sql-data-warehouse-tables-overview.md), [Distribución de tablas](sql-data-warehouse-tables-distribute.md), [Selección de la distribución de tablas](https://blogs.msdn.microsoft.com/sqlcat/20../../choosing-hash-distributed-table-vs-round-robin-distributed-table-in-azure-sql-dw-service/), [CREATE TABLE](/sql/t-sql/statements/create-table-azure-sql-data-warehouse?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest), [CREATE TABLE AS SELECT](/sql/t-sql/statements/create-table-as-select-azure-sql-data-warehouse?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest).

## <a name="do-not-over-partition"></a>Sin particiones excesivas

Crear particiones de datos puede resultar eficaz para el mantenimiento de los datos mediante la modificación de particiones o exámenes de optimización, pero el exceso de particiones puede ralentizar las consultas.  A menudo una estrategia de creación de particiones con granularidad alta que puede funcionar bien en SQL Server, no funciona correctamente en el grupo de SQL.  

El exceso de particiones también puede reducir la eficacia de los índices de almacén de columnas agrupadas si cada partición tiene menos de 1 millón de filas.  Tenga en cuenta que, en segundo plano, el grupo de SQL particiona los datos automáticamente en 60 bases de datos, por lo que si crea una tabla con 100 particiones, se generan realmente 6000 particiones.  

Cada carga de trabajo es diferente, por lo mejor es probar con las particiones para ver qué funciona mejor para la suya.  Considere la posibilidad de reducir la granularidad respecto a lo que le funcionaba en SQL Server.  Por ejemplo, puede usar particiones semanales o mensuales, en lugar de diarias.

Consulte también [Creación de particiones de tablas](sql-data-warehouse-tables-partition.md).

## <a name="minimize-transaction-sizes"></a>Reducción del tamaño de las transacciones

Las instrucciones INSERT, UPDATE Y DELETE se ejecutan en las transacciones y, cuando fallan, deben deshacerse.  Para que no se tarde tanto en deshacer, reduzca el tamaño de las transacciones siempre que pueda.  Puede hacerlo si divide las instrucciones INSERT, UPDATE y DELETE en partes.  

Por ejemplo, si tiene una instrucción INSERT que se suele tardar 1 hora, si puede, divídala en cuatro partes de 15 minutos cada una.  Aproveche los casos de registro mínimo, como CTAS, TRUNCATE, DROP TABLE o INSERT para vaciar las tablas y así reducir el riesgo de reversión.  

Otra manera de eliminar reversiones es usar funciones de solo metadatos, como la modificación de particiones para la administración de datos.  Por ejemplo, en lugar de ejecutar una instrucción DELETE para eliminar todas las filas de una tabla cuyo order_date fuera octubre de 2001, podría dividir los datos mensualmente y desactivar la división con los datos de una partición vacía de otra tabla (consulte los ejemplos de [ALTER TABLE](/sql/t-sql/statements/alter-table-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)).  

Para tablas sin particiones, puede usar CTAS en lugar de DELETE para escribir los datos que quiera mantener en una tabla.  Si CTAS tarda lo mismo, es una operación mucho más segura, ya que su registro de transacciones es mínimo y se puede cancelar rápidamente si es necesario.

Consulte también [Introducción a las transacciones](sql-data-warehouse-develop-transactions.md), [Optimización de transacciones](sql-data-warehouse-develop-best-practices-transactions.md), [Creación de particiones de tablas](sql-data-warehouse-tables-partition.md), [TRUNCATE TABLE](/sql/t-sql/statements/truncate-table-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest), [ALTER TABLE](/sql/t-sql/statements/alter-table-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest) y [CREATE TABLE AS SELECT (CTAS)](sql-data-warehouse-develop-ctas.md).

## <a name="reduce-query-result-sizes"></a>Reducción del tamaño de los resultados de consultas

Este paso le ayuda a evitar problemas en el lado del cliente a causa de unos resultados de consulta grandes.  Puede editar la consulta para reducir el número de filas devueltas. Algunas herramientas de generación de consultas le permiten agregar sintaxis de "N principal" a cada consulta.  También puede aplicar una instrucción CETAS al resultado de la consulta en una tabla temporal y, a continuación, usar la exportación de PolyBase para el procesamiento de nivel inferior.

## <a name="use-the-smallest-possible-column-size"></a>Uso del tamaño de columna mínimo

Al definir el DDL, usar el tipo de datos mínimo compatible con los datos mejorará el rendimiento de la consulta.  Este enfoque tiene especial importancia para las columnas CHAR y VARCHAR.  

Si el valor mayor máximo de una columna es 25 caracteres, defina la columna como VARCHAR(25).  Evite definir todas las columnas de caracteres con una longitud predeterminada de gran tamaño.  Defina las columnas como VARCHAR en lugar de NVARCHAR cuando no se necesite nada más.

Consulte también [Información general de tablas](sql-data-warehouse-tables-overview.md), [Tipos de datos de las tablas](sql-data-warehouse-tables-data-types.md), [CREATE TABLE](/sql/t-sql/statements/create-table-azure-sql-data-warehouse?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest).

## <a name="use-temporary-heap-tables-for-transient-data"></a>Uso de tablas de apilamiento temporal para datos transitorios

Cuando almacene datos temporalmente, las tablas de apilamiento pueden agilizar el proceso global.  Si está cargando datos solo para colocarlos en etapa antes de ejecutar más transformaciones, será mucho más rápido cargar la tabla en una tabla del montón que cargar los datos en una tabla agrupada de almacén de columnas.  

Además, los datos de una tabla temporal también se cargarán mucho más rápido que las tablas de almacenamiento permanente.  Las tablas temporales empiezan por "#" y solo se puede acceder a ellas desde la sesión en la que se crean, por lo que pueden no funcionar en algunas situaciones.

Las tablas de apilamiento se definen en la cláusula WITH de CREATE TABLE.  Si usa una tabla temporal, no olvide crear estadísticas en ella también.

Consulte también [Tablas temporales](sql-data-warehouse-tables-temporary.md), [CREATE TABLE](/sql/t-sql/statements/create-table-azure-sql-data-warehouse?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest), [CREATE TABLE AS SELECT](/sql/t-sql/statements/create-table-as-select-azure-sql-data-warehouse?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest).

## <a name="optimize-clustered-columnstore-tables"></a>Optimización de tablas de almacén de columnas agrupadas

Los índices de almacén de columnas agrupadas son una de las maneras más eficaces para almacenar datos en el grupo de SQL.  De forma predeterminada, las tablas del grupo de SQL se crean como almacén de columnas agrupadas.  Para conseguir el máximo rendimiento de las consultas en las tablas de almacén de columnas, es importante la calidad de los segmentos.  

Escriben filas en las tablas de almacén de columnas bajo presión de memoria afecta a la calidad de segmento.  La calidad de segmento se puede medir por el número de filas de un grupo de filas comprimido.  Para obtener instrucciones detalladas acerca de la detección y mejora de la calidad de los segmentos en las tablas de almacén de columnas agrupadas, consulte la sección [Causas de una calidad deficiente del índice de almacén de columnas](sql-data-warehouse-tables-index.md#causes-of-poor-columnstore-index-quality) del artículo sobre [Indexación de tablas](sql-data-warehouse-tables-index.md).  

Como es importante que los segmentos de almacén de columnas sean de una buena calidad, es conveniente usar identificadores de usuario que se encuentren en la clase de recursos grande o mediana para cargar los datos. El uso de [unidades de almacenamiento de datos](what-is-a-data-warehouse-unit-dwu-cdwu.md) inferiores significa que desea asignar una clase de recurso mayor para el usuario que realiza la carga.

Dado que las tablas de almacén de columnas generalmente no insertan datos en un segmento del almacén de columnas comprimido hasta que hay más de 1 millón de filas por tabla y cada tabla del grupo de SQL se divide en 60 partes, como norma general, las tablas de almacén de columnas no serán útiles para las consultas a menos que la tabla tenga más de 60 millones de filas.  Para las tablas con menos de 60 millones de filas, podría no tener sentido el índice de almacén de columnas.  Pero tampoco molesta.  

Además, si divide los datos, recuerde que cada parte deberá tener 1 millón de filas para beneficiarse de un índice de almacén de columnas agrupadas.  Si una tabla tiene 100 particiones, tendrá que tener al menos 6 mil millones de filas para beneficiarse de un almacén de columnas agrupadas (60 distribuciones *100 particiones* 1 millón de filas).  

Si la tabla no tiene 6 mil millones de filas en este ejemplo, reduzca el número de particiones o considere la posibilidad de usar una tabla de apilamiento en su lugar.  También puede experimentar para ver si consigue un mejor rendimiento con una tabla de apilamiento con índices secundarios, en lugar de con una tabla de almacén de columnas.

> [!TIP]
> Al consultar una tabla de almacén de columnas, las consultas se ejecutarán más rápido si selecciona solo las que necesita.  

Consulte también [Índices de tablas](sql-data-warehouse-tables-index.md), [Guía de índices de almacén de columnas](/sql/relational-databases/indexes/columnstore-indexes-overview?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest) y [Regeneración de índices de almacén de columnas](sql-data-warehouse-tables-index.md#rebuilding-indexes-to-improve-segment-quality).

## <a name="use-larger-resource-class-to-improve-query-performance"></a>Uso de clases de recursos más grandes para mejorar el rendimiento de las consultas

El grupo de SQL usa grupos de recursos para asignar memoria a las consultas.  De manera predeterminada, todos los usuarios se asignan a los recursos de la clase pequeña, que concede 100 MB de memoria por distribución.  Dado que siempre hay 60 distribuciones y cada distribución tiene un mínimo de 100 MB, la asignación de memoria total del sistema es de 6 000 MB o justo por debajo de 6 GB.  

Algunas consultas, como las combinaciones de gran tamaño o las cargas a las tablas de almacén de columnas agrupadas, se beneficiarán de las mayores asignaciones de memoria.  Algunas consultas, como los exámenes puros, no ofrecerá ventajas.  De todas formas, usar clases de recursos mayores reduce la simultaneidad, por lo que deberá tener en cuenta este impacto antes de cambiar todos los usuarios a una clase de recursos grande.

Consulte también [Clases de recursos para la administración de cargas de trabajo](resource-classes-for-workload-management.md).

## <a name="use-smaller-resource-class-to-increase-concurrency"></a>Menor clase de recursos para aumentar la simultaneidad

Si observa que las consultas de usuario se retrasan bastante, es posible que los usuarios estén realizando ejecuciones en clases de recursos mayores y consuman varios espacios de simultaneidad, lo que pone en cola otras consultas.  Para ver si hay consultas de usuarios en cola, ejecute `SELECT * FROM sys.dm_pdw_waits` para ver si se devuelven filas.

Consulte también [Clases de recursos para la administración de cargas de trabajo](resource-classes-for-workload-management.md), [sys.dm_pdw_waits](/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-waits-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest).

## <a name="other-resources"></a>Otros recursos

Consulte también el artículo [Solución de problemas](sql-data-warehouse-troubleshoot.md) para más información sobre problemas comunes y soluciones.

Si no encontró lo que busca en este artículo, pruebe a usar la "búsqueda de documentos" en la parte izquierda de esta página para buscar todos los documentos de Azure Synapse.  En la [página de preguntas y respuestas de Microsoft para Azure Synapse](https://docs.microsoft.com/answers/topics/azure-synapse-analytics.html) puede publicar preguntas para otros usuarios y para el grupo del producto de Azure Synapse. Supervisamos continuamente este foro para garantizar que sus preguntas las responde otro usuario o alguno de nosotros.  

Si prefiere formular sus preguntas en Stack Overflow, también tenemos un [foro de Stack Overflow acerca de Azure Synapse](https://stackoverflow.com/questions/tagged/azure-sqldw).

Use la página [de comentarios de Azure Synapse](https://feedback.azure.com/forums/307516-sql-data-warehouse) para realizar solicitudes de características.  Añadir sus solicitudes o valoraciones positivas sobre otras solicitudes realmente nos ayuda a priorizar las características.

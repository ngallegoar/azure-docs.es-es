---
title: Procedimientos recomendados para grupos de SQL
description: Recomendaciones y procedimientos recomendados que debe saber para trabajar con grupos de SQL.
services: synapse-analytics
author: mlee3gsd
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: ''
ms.date: 04/15/2020
ms.author: martinle
ms.reviewer: igorstan
ms.openlocfilehash: b3dc111fe62cbae857f3369165ba29cf40e90342
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/16/2020
ms.locfileid: "81426392"
---
# <a name="best-practices-for-sql-pools-in-azure-synapse-analytics"></a>Procedimientos recomendados para grupos de SQL en Azure Synapse Analytics

En este artículo se proporciona una colección de procedimientos recomendados que le ayudará a conseguir un rendimiento óptimo para los grupos de SQL en Azure Synapse Analytics. A continuación, encontrará instrucciones básicas y áreas importantes en las que centrar su atención durante la creación de la solución. En cada sección se presenta un concepto y se le indican artículos que lo desarrollan más en detalle.

## <a name="sql-pools-loading"></a>Carga de grupos de SQL

Para obtener una guía de carga de grupos de SQL, consulte la [Guía para cargar datos](data-loading-best-practices.md).

## <a name="reduce-cost-with-pause-and-scale"></a>Menos costos gracias a las características de pausa y escalado

Para obtener más información acerca de cómo reducir los costos mediante la pausa y el escalado, consulte [Administración de procesos](../sql-data-warehouse/sql-data-warehouse-manage-compute-overview.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json).

## <a name="maintain-statistics"></a>Mantenimiento de estadísticas

Aunque SQL Server detecta y crea o actualiza automáticamente las estadísticas en columnas, los grupos de SQL requieren de mantenimiento manual de las estadísticas. Debería dar mantenimiento a las estadísticas para asegurarse de que los planes del grupo de SQL están optimizados.  Los planes que crea el optimizador son igual de buenos que las estadísticas disponibles.

> [!TIP]
> Crear estadísticas de muestra en cada columna es una forma sencilla de empezar a trabajar con las estadísticas.  

Es igualmente importante actualizar las estadísticas cuando se produzcan cambios significativos en los datos.  Un enfoque conservador puede ser actualizar las estadísticas diariamente o después de cada carga.  Existen inconvenientes entre el rendimiento y el costo de crear y actualizar las estadísticas.

Para acortar el tiempo de mantenimiento de las estadísticas, sea selectivo sobre qué columnas tienen estadísticas o cuáles necesitan actualizaciones más frecuentes. Por ejemplo, debería actualizar las columnas de fecha en las que se añaden valores todos los días. Céntrese en tener estadísticas sobre las columnas que intervienen en combinaciones, las columnas que se usan en la cláusula WHERE y las columnas que se encuentran en GROUP BY.

Puede encontrar información adicional sobre las estadísticas en los artículos [Administración de estadísticas de tabla](develop-tables-statistics.md), [CREATE STATISTICS](/sql/t-sql/statements/create-statistics-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) y [UPDATE STATISTICS](/sql/t-sql/statements/update-statistics-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest).

## <a name="group-insert-statements-into-batches"></a>Agrupación de instrucciones INSERT en lotes

Una carga única en una tabla pequeña con una instrucción INSERT como `INSERT INTO MyLookup VALUES (1, 'Type 1')` puede ser el mejor enfoque en función de sus necesidades. Sin embargo, si necesita cargar miles o millones de filas a lo largo del día, es posible que las instrucciones INSERT sencillas no sean óptimas.

Una manera de solucionar este problema consiste en desarrollar un proceso que escriba en un archivo y, a continuación, otro proceso para cargar periódicamente este archivo. Consulte el artículo sobre [INSERT](/sql/t-sql/statements/insert-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) para más información.

## <a name="use-polybase-to-load-and-export-data-quickly"></a>Uso de PolyBase para cargar y exportar los datos rápidamente

El grupo de SQL admite la carga y exportación de datos con varias herramientas, como Azure Data Factory, PolyBase y BCP.  Para pequeñas cantidades de datos donde el rendimiento no es clave, cualquier herramienta le sirve.  

> [!NOTE]
> PolyBase es la mejor opción para cargar o exportar grandes volúmenes de datos o si se necesita un rendimiento rápido.

Lo que haya cargado con PolyBase se ejecuta con la consulta CTAS o de selección. CTAS reducirá el registro de transacciones y es la manera más rápida de cargar datos. Azure Data Factory también admite cargas de PolyBase y puede lograr un rendimiento parecido a CTAS. PolyBase admite varios formatos de archivo, como Gzip.

Con el fin de conseguir un mayor rendimiento al usar archivos de texto Gzip, divídalos en 60 o más archivos para aumentar el paralelismo de la carga. Para conseguir un rendimiento total más rápido, cargue los datos simultáneamente. En los siguientes artículos se incluye información adicional sobre los temas pertinentes para esta sección:

- [Carga de datos](data-loading-overview.md)
- [Guía para el uso de PolyBase](data-loading-best-practices.md)
- [Modelos y estrategias de carga en el grupo de Azure SQL](https://blogs.msdn.microsoft.com/sqlcat/20../../azure-sql-data-warehouse-loading-patterns-and-strategies/)
- [Carga de datos con Azure Data Factory](../../data-factory/load-azure-sql-data-warehouse.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json)
- [Movimiento de datos con Azure Data Factory](../../data-factory/transform-data-using-machine-learning.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json)
- [CREATE EXTERNAL FILE FORMAT](/sql/t-sql/statements/create-external-file-format-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest)
- [Create Table As Select (CTAS) en Almacenamiento de datos SQL](../sql-data-warehouse/sql-data-warehouse-develop-ctas.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json)

## <a name="load-then-query-external-tables"></a>Carga y consulta de tablas externas

PolyBase no es óptimo para las consultas. Las tablas de Polybase para grupos de SQL solo admiten actualmente archivos de blobs de Azure y almacenamiento de Azure Data Lake. Estos archivos no tienen ningún recurso de proceso que los respalde. Como resultado, los grupos de SQL no pueden descargar este trabajo y, para leer los datos del archivo entero, tienen que cargarlo en tempdb.

Si tiene varias consultas que usan estos datos, es mejor cargarlos una vez que las consultas usen la tabla local. Se incluyen más instrucciones sobre PolyBase en el artículo [Guía de uso de PolyBase](data-loading-best-practices.md).

## <a name="hash-distribute-large-tables"></a>Distribución Hash para tablas grandes

De forma predeterminada, las tablas se distribuyen según el patrón Round Robin.   Esta opción predeterminada facilita a los usuarios la tarea de comenzar a crear tablas sin tener que decidir sobre la distribución. El rendimiento de las tablas round robin puede ser suficiente para algunas cargas de trabajo. Sin embargo, en la mayoría de los casos, una columna de distribución ofrece un mejor rendimiento.  

El ejemplo más común de una tabla distribuida por una columna que supera a una round robin es al combinar dos tablas grandes de hechos.  

Por ejemplo, si tiene una tabla de pedidos que se distribuye por order_id, y una tabla de transacciones que también se distribuye por order_id, cuando se una la tabla de pedidos a la de transacciones en order_id, esta consulta se convertirá en una consulta de paso a través. A continuación, se eliminan las operaciones de movimiento de datos. Menos pasos suponen consultas más rápidas. Menos movimiento de datos también se traduce en consultas más rápidas.

> [!NOTE]
> Al cargar una tabla distribuida, los datos entrantes no se deben ordenar con la clave de distribución. Si se ordena así, las cargas se ralentizarán.

Los vínculos a los artículos que se proporcionan a continuación le proporcionarán más detalles sobre cómo mejorar el rendimiento al seleccionar una columna de distribución. Además, encontrará información sobre cómo definir una tabla distribuida en la cláusula WITH de la instrucción CREATE TABLE:

- [Información general sobre las tablas](develop-tables-overview.md)
- [Distribución de tablas](../sql-data-warehouse/sql-data-warehouse-tables-distribute.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json)
- [Selección de la distribución de tablas](https://blogs.msdn.microsoft.com/sqlcat/20../../choosing-hash-distributed-table-vs-round-robin-distributed-table-in-azure-sql-dw-service/)
- [CREATE TABLE](/sql/t-sql/statements/create-table-azure-sql-data-warehouse?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest)
- [CREATE TABLE AS SELECT](/sql/t-sql/statements/create-table-as-select-azure-sql-data-warehouse?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest)

## <a name="do-not-over-partition"></a>Sin particiones excesivas

Crear particiones de datos puede resultar eficaz para el mantenimiento de los datos mediante la modificación de particiones o exámenes de optimización, pero el exceso de particiones puede ralentizar las consultas.  A menudo una estrategia de creación de particiones con granularidad alta que puede funcionar bien en SQL Server no funciona correctamente en el grupo de SQL.  

El exceso de particiones puede reducir la eficacia de los índices de almacén de columnas agrupadas si cada partición tiene menos de 1 millón de filas. Los grupos de SQL particionan automáticamente los datos en 60 bases de datos. Por lo tanto, si crea una tabla con 100 particiones, el resultado serán 6000 particiones. Cada carga de trabajo es diferente, así que lo mejor es probar las particiones para ver qué funciona mejor para su carga.  

Una opción que debe tener en cuenta consiste en usar una granularidad inferior a la que estaba implementada al usar SQL Server. Por ejemplo, puede usar particiones semanales o mensuales, en lugar de particiones diarias.

Encontrará más información sobre las particiones en el artículo [Creación de particiones de tablas](../sql-data-warehouse/sql-data-warehouse-tables-partition.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json).

## <a name="minimize-transaction-sizes"></a>Reducción del tamaño de las transacciones

Las instrucciones SELECT, UPDATE y DELETE se ejecutan en una transacción. Si devuelven un error, se deben revertir. Para que la reversión no se tarde tanto, minimice el tamaño de las transacciones siempre que pueda.  Puede realizar esta operación si divide las instrucciones INSERT, UPDATE y DELETE en partes. Por ejemplo, si tiene una instrucción INSERT que se suele tardar 1 hora, puede dividirla en cuatro partes. Cada ejecución durará 15 minutos.  

> [!TIP]
> Aproveche los casos de registro mínimo, como CTAS, TRUNCATE, DROP TABLE o INSERT para vaciar las tablas y así reducir el riesgo de reversión.  

Otra manera de eliminar reversiones es usar funciones de solo metadatos, como la modificación de particiones para la administración de datos.  Por ejemplo, en lugar de ejecutar una instrucción DELETE para eliminar todas las filas de una tabla en la que order_date fue en octubre de 2001, podría particionar los datos mensualmente. A continuación, puede cambiar la partición con datos por una partición vacía de otra tabla (consulte ejemplos de ALTER TABLE).  

Para tablas sin particiones, puede usar CTAS en lugar de DELETE para escribir los datos que quiera mantener en una tabla.  Si CTAS tarda lo mismo, es mucho más seguro, ya que su registro de transacciones es mínimo y se puede cancelar rápidamente si es necesario.

En los artículos siguientes se incluye más información sobre el contenido relacionado con esta sección:

- [Create Table As Select (CTAS) en Almacenamiento de datos SQL](../sql-data-warehouse/sql-data-warehouse-develop-ctas.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json)
- [Descripción de las transacciones](develop-transactions.md)
- [Optimización de transacciones para Almacenamiento de datos SQL](../sql-data-warehouse/sql-data-warehouse-develop-best-practices-transactions.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json)
- [Partición de tabla](../sql-data-warehouse/sql-data-warehouse-tables-partition.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json)
- [TRUNCATE TABLE](/sql/t-sql/statements/truncate-table-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest)
- [ALTER TABLE](/sql/t-sql/statements/alter-table-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest)

## <a name="reduce-query-result-sizes"></a>Reducción del tamaño de los resultados de consultas

Esta operación le ayuda a evitar problemas en el lado del cliente a causa de resultados de consulta de gran tamaño.  Puede editar la consulta para reducir el número de filas devueltas. Algunas herramientas de generación de consultas le permiten agregar sintaxis de "N principal" a cada consulta.  También puede aplicar una instrucción CETAS al resultado de la consulta en una tabla temporal y, a continuación, usar la exportación de PolyBase para el procesamiento de nivel inferior.

## <a name="use-the-smallest-possible-column-size"></a>Uso del tamaño de columna mínimo

Al definir el DDL, use el tipo de datos mínimo compatible con los datos, ya que esto mejorará el rendimiento de la consulta.  Esta recomendación tiene especial importancia para las columnas CHAR y VARCHAR.  Si el valor mayor máximo de una columna es 25 caracteres, defina la columna como VARCHAR(25).  Evite definir todas las columnas de caracteres con una longitud predeterminada de gran tamaño.  Además, defina las columnas como VARCHAR en lugar de NVARCHAR cuando no se necesite nada más.

Consulte los artículos [Información general sobre tablas](develop-tables-overview.md), [Tipos de datos de tabla](develop-tables-data-types.md) y [CREATE TABLE](/sql/t-sql/statements/create-table-azure-sql-data-warehouse?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) para obtener un repaso más detallado de los conceptos esenciales relacionados con la información anterior.

## <a name="use-temporary-heap-tables-for-transient-data"></a>Uso de tablas de apilamiento temporal para datos transitorios

Cuando dirija temporalmente los datos a los grupos de SQL, las tablas de montón normalmente harán que el proceso general sea más rápido.  Si va a cargar datos solo para almacenarlos temporalmente antes de ejecutar más transformaciones, será más rápido cargar la tabla en una tabla de montón que cargar los datos en una tabla agrupada de almacén de columnas.  

Los datos de una tabla temporal también se cargarán mucho más rápido que las tablas de almacenamiento permanente.  Las tablas temporales empiezan por "#" y solo se puede acceder a ellas desde la sesión en la que se crean. Por lo tanto, pueden no funcionar en algunas situaciones. Las tablas de apilamiento se definen en la cláusula WITH de CREATE TABLE.  Si usa una tabla temporal, no olvide crear estadísticas en ella también.

Para obtener instrucciones adicionales, consulte los artículos [Tablas temporales](/sql/t-sql/statements/alter-table-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest), [CREATE TABLE](/sql/t-sql/statements/create-table-azure-sql-data-warehouse?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) y [CREATE TABLE AS SELECT](/sql/t-sql/statements/create-table-as-select-azure-sql-data-warehouse?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest).

## <a name="optimize-clustered-columnstore-tables"></a>Optimización de tablas de almacén de columnas agrupadas

Los índices de almacén de columnas agrupadas son una de las maneras más eficaces para almacenar datos en el grupo de SQL.  De forma predeterminada, las tablas del grupo de SQL se crean como almacén de columnas agrupadas.  Para conseguir el máximo rendimiento de las consultas en las tablas de almacén de columnas, es importante la calidad de los segmentos.  Escriben filas en las tablas de almacén de columnas bajo presión de memoria afecta a la calidad de segmento.  

La calidad de los segmentos se puede medir por el número de filas en un grupo de filas comprimido. Para obtener instrucciones detalladas acerca de la detección y mejora de la calidad de los segmentos en las tablas de almacén de columnas agrupadas, consulte la sección [Causas de una calidad deficiente del índice de almacén de columnas](../sql-data-warehouse/sql-data-warehouse-tables-index.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json#causes-of-poor-columnstore-index-quality) del artículo [Indexación de tablas](../sql-data-warehouse/sql-data-warehouse-tables-index.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json)  

Como es importante que los segmentos de almacén de columnas sean de una buena calidad, es conveniente usar identificadores de usuario que se encuentren en la clase de recursos grande o mediana para cargar los datos. El uso de [unidades de almacenamiento de datos](resource-consumption-models.md) inferiores significa que desea asignar una clase de recurso mayor para el usuario que realiza la carga.

Normalmente, las tablas de almacén de columnas no envían datos en un segmento de almacén de columnas comprimido hasta que haya más de 1 millón de filas por tabla. Cada tabla del grupo de SQL se divide en 60 tablas. Por lo tanto, las tablas de almacén de columnas no beneficiarán a una consulta, a menos que la tabla tenga más de 60 millones de filas.  

> [!TIP]
> En el caso de las tablas con menos de 60 millones de filas, un índice de almacén de columnas puede no ser la solución óptima.  

Si divide los datos, cada partición deberá tener 1 millón de filas para beneficiarse de un índice de almacén de columnas agrupadas.  En el caso de una tabla con 100 particiones, serán necesarias al menos 6 mil millones de filas para beneficiarse de un almacén de columnas agrupadas (60 distribuciones *100 particiones* 1 millón de filas).  

Si la tabla no tiene 6 mil millones filas, tiene dos opciones principales. Reduzca el número de particiones o considere la posibilidad de usar una tabla de montón en su lugar.  También puede experimentar para ver si consigue un mejor rendimiento mediante una tabla de montón con índices secundarios, en lugar de con una tabla de almacén de columnas.

Al consultar una tabla de almacén de columnas, las consultas se ejecutarán más rápido si selecciona solo las que necesita.  Puede encontrar más información sobre los índices de almacén de columnas y de tabla en los artículos [Índices de tabla](../sql-data-warehouse/sql-data-warehouse-tables-index.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json), [Guía de índices de almacén de columnas](/sql/relational-databases/indexes/columnstore-indexes-overview?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) y [Recompilación de índices de almacén de columnas](../sql-data-warehouse/sql-data-warehouse-tables-index.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest#rebuilding-indexes-to-improve-segment-quality).

## <a name="use-larger-resource-class-to-improve-query-performance"></a>Uso de clases de recursos más grandes para mejorar el rendimiento de las consultas

Los grupos de SQL usan grupos de recursos para asignar memoria a las consultas. Inicialmente, todos los usuarios se asignan a los recursos de la clase pequeña, que concede 100 MB de memoria por distribución.  Siempre hay 60 distribuciones. Cada distribución recibe un mínimo de 100 MB. La asignación de memoria total en todo el sistema es de 6000 MB o poco menos de 6 GB.  

Algunas consultas, como las combinaciones de gran tamaño o las cargas a las tablas de almacén de columnas agrupadas, se beneficiarán de las mayores asignaciones de memoria.  Algunas consultas, como los exámenes puros, no sufrirán cambios. El uso de clases de recursos más grandes afecta a la simultaneidad. Por lo tanto, deberá tener estos datos en cuenta antes de mover todos los usuarios a una clase de recursos grande.

Para obtener información adicional sobre las clases de recursos, consulte el artículo [Clases de recursos para la administración de cargas de trabajo](../sql-data-warehouse/resource-classes-for-workload-management.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json).

## <a name="use-smaller-resource-class-to-increase-concurrency"></a>Uso de clases de recursos más pequeñas para aumentar la simultaneidad

Si observa retrasos prolongados en las consultas de usuario, es posible que los usuarios estén ejecutándose en clases de recursos más grandes. Esta situación promueve el consumo de espacios de simultaneidad, que puede ocasionar la puesta en cola de otras consultas.  Para determinar si hay consultas de usuarios en cola, ejecute `SELECT * FROM sys.dm_pdw_waits` para ver si se devuelve alguna fila.

Los artículos [Clases de recursos para la administración de cargas de trabajo](../sql-data-warehouse/resource-classes-for-workload-management.md) y [sys.dm_pdw_waits](/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-waits-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) le proporcionarán más información.

## <a name="use-dmvs-to-monitor-and-optimize-your-queries"></a>Uso de vistas de administración dinámica (DMV) para supervisar y optimizar las consultas

Los grupos de SQL tiene varias DMV que sirven para supervisar la ejecución de consultas.  El siguiente artículo de supervisión le guía con instrucciones paso a paso acerca de cómo revisar los detalles de una consulta en curso.  Usar la opción LABEL con las consultas puede ayudar a encontrar rápidamente las consultas en estas DMV. Para obtener información detallada adicional, consulte los artículos incluidos en la siguiente lista:

- [Supervisión de la carga de trabajo mediante DMV](../sql-data-warehouse/sql-data-warehouse-manage-monitor.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json)

- [LABEL](develop-label.md)
- [OPTION](/sql/t-sql/queries/option-clause-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest)
- [sys.dm_exec_sessions](/sql/relational-databases/system-dynamic-management-views/sys-dm-exec-sessions-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest)
- [sys.dm_pdw_exec_requests](/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-exec-requests-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest)
- [sys.dm_pdw_request_steps](/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-request-steps-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest)
- [sys.dm_pdw_sql_requests](/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-sql-requests-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest)
- [sys.dm_pdw_dms_workers](/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-dms-workers-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest)
- [DBCC PDW_SHOWEXECUTIONPLAN](/sql/t-sql/database-console-commands/dbcc-pdw-showexecutionplan-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest)
- [sys.dm_pdw_waits](/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-waits-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest)

## <a name="next-steps"></a>Pasos siguientes

Consulte también el artículo sobre [Solución de problemas](../sql-data-warehouse/sql-data-warehouse-troubleshoot.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json) para conocer los problemas comunes y sus soluciones.

Si necesita información que no esté incluida en este artículo, use la "búsqueda de documentación" en el lado izquierdo de esta página para buscar en todos los documentos del grupo de SQL.  El [foro del grupo de SQL](https://social.msdn.microsoft.com/Forums/sqlserver/home?forum=AzureSQLDataWarehouse) es un lugar para plantear preguntas a otros usuarios y al grupo de productos del grupo de SQL.  

Supervisamos continuamente este foro para garantizar que sus preguntas las responde otro usuario o alguno de nosotros.  Si prefiere formular sus preguntas en Stack Overflow, también hay disponible un [foro de Stack Overflow acerca del grupo de Azure SQL](https://stackoverflow.com/questions/tagged/azure-sqldw).

Para realizar solicitudes de características, use la página de [comentarios del grupo de Azure SQL](https://feedback.azure.com/forums/307516-sql-data-warehouse).  Al agregar solicitudes o votar por otras nos ayuda a centrarnos en las características más demandadas.

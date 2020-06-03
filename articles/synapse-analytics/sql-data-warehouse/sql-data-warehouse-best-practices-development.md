---
title: Procedimientos recomendados para el desarrollo
description: Recomendaciones y procedimientos recomendados que debe saber para desarrollar soluciones del grupo de SQL de Synapse.
services: synapse-analytics
author: XiaoyuMSFT
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: ''
ms.date: 09/04/2018
ms.author: xiaoyul
ms.reviewer: igorstan
ms.custom: seo-lt-2019
ms.openlocfilehash: 13e149c45c720a5da6b051b7ce9581d74c64fa35
ms.sourcegitcommit: 0b80a5802343ea769a91f91a8cdbdf1b67a932d3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/25/2020
ms.locfileid: "83834482"
---
# <a name="development-best-practices-for-synapse-sql-pool"></a>Procedimientos recomendados de desarrollo para el grupo de SQL de SYNAPSE

En este artículo se describen la guía y los procedimientos recomendados a medida que desarrolla una solución de grupo de SQL.

## <a name="tune-query-performance-with-new-product-enhancements"></a>Optimización del rendimiento de las consultas con nuevas mejoras en el producto

- [Optimización del rendimiento con vistas materializadas](performance-tuning-materialized-views.md)
- [Optimización del rendimiento con el índice de almacén de columnas agrupado ordenado](performance-tuning-ordered-cci.md)
- [Ajuste del rendimiento con la copia en caché del conjunto de resultados](performance-tuning-result-set-caching.md)

## <a name="reduce-cost-with-pause-and-scale"></a>Menos costos gracias a las características de pausa y escalado

Para más información acerca de cómo reducir los costos mediante la pausa y el escalado, consulte el artículo [Administración de procesos](sql-data-warehouse-manage-compute-overview.md).

## <a name="maintain-statistics"></a>Mantenimiento de estadísticas

El grupo de SQL se puede configurar para detectar y crear estadísticas automáticamente con información de las columnas.  Los planes de consulta que crea el optimizador son igual de buenos que las estadísticas disponibles.  

Se recomienda habilitar AUTO_CREATE_STATISTICS para las bases de datos y mantener actualizadas las estadísticas a diario o después de cada carga. Así, se asegurará de que las estadísticas de las columnas utilizadas en las consultas siempre estén actualizadas.

Si cree que tarda demasiado en realizar la actualización de todas las estadísticas, puede intentar ser más selectivo acerca de las columnas que necesitan actualizar sus estadísticas con frecuencia. Por ejemplo, puede actualizar las columnas de fecha, donde se añadan valores todos los días.

> [!TIP]
> Sacará el máximo provecho con las estadísticas actualizadas en columnas relacionadas con combinaciones, columnas que se usan en la cláusula WHERE y columnas de GROUP BY.

Consulte también [Administración de estadísticas en tablas](sql-data-warehouse-tables-statistics.md), [CREATE STATISTICS](sql-data-warehouse-tables-statistics.md) y [UPDATE STATISTICS](sql-data-warehouse-tables-statistics.md#update-statistics).

## <a name="hash-distribute-large-tables"></a>Distribución Hash para tablas grandes

De forma predeterminada, las tablas se distribuyen según el patrón Round Robin.  Este diseño facilita a los usuarios empezar a crear tablas sin tener que decidir sobre la distribución.  

Las tablas round robin pueden ser suficientes para algunas cargas de trabajo, pero en la mayoría de los casos, la selección de una columna de distribución funcionará mucho opción.  El ejemplo más común de tabla distribuida por una columna que supera con creces a una Round Robin es al combinarse dos tablas grandes de hechos.  

Por ejemplo, si tiene una tabla de pedidos, que se distribuye por order_id, y una tabla de transacciones, que también se distribuye por order_id, al unir la tabla de pedidos a la de transacciones en order_id, esta consulta se convierte en una consulta de paso a través, lo que significa que se eliminan las operaciones de movimiento de datos.  Menos pasos suponen consultas más rápidas.  Menos movimiento de datos también se traduce en consultas más rápidas.  

Al cargar una tabla con distribución, asegúrese de que no se ordenan los datos entrantes en la clave de distribución, ya que esto ralentizará la carga.  En los artículos siguientes se explica en detalle cómo mejorar el rendimiento mediante la selección de una columna de distribución y cómo definir una tabla distribuida en la cláusula WITH de la instrucción CREATE TABLES.

Consulte también [Información general de tablas](sql-data-warehouse-tables-overview.md), [Distribución de tablas](sql-data-warehouse-tables-distribute.md), [Selección de distribución de tablas](https://blogs.msdn.microsoft.com/sqlcat/20../../choosing-hash-distributed-table-vs-round-robin-distributed-table-in-azure-sql-dw-service/), [CREATE TABLE](sql-data-warehouse-tables-overview.md) y [CREATE TABLE AS SELECT](sql-data-warehouse-develop-ctas.md)

## <a name="do-not-over-partition"></a>Sin particiones excesivas

Crear particiones de datos puede resultar eficaz para el mantenimiento de los datos mediante la modificación de particiones o exámenes de optimización, pero el exceso de particiones puede ralentizar las consultas.  

A menudo, una estrategia de creación de particiones con granularidad alta que puede funcionar bien en SQL Server no funciona correctamente en el grupo de SQL.  El exceso de particiones también puede reducir la eficacia de los índices de almacén de columnas agrupadas si cada partición tiene menos de 1 millón de filas.  

Tenga en cuenta que, en segundo plano, el grupo de SQL particiona los datos automáticamente en 60 bases de datos, por lo que si crea una tabla con 100 particiones, se generan realmente 6000 particiones.  Cada carga de trabajo es diferente, por lo mejor es probar con las particiones para ver qué funciona mejor para la suya.  

> [!TIP]
> Considere la posibilidad de usar una granularidad inferior a la que le funcionaba en SQL Server.  Por ejemplo, puede usar particiones semanales o mensuales, en lugar de diarias.

Consulte también [Creación de particiones de tablas](sql-data-warehouse-tables-partition.md).

## <a name="minimize-transaction-sizes"></a>Reducción del tamaño de las transacciones

Las instrucciones INSERT, UPDATE Y DELETE se ejecutan en las transacciones y, cuando fallan, deben deshacerse.  Para que no se tarde tanto en deshacer, reduzca el tamaño de las transacciones siempre que pueda.  Puede hacerlo si divide las instrucciones INSERT, UPDATE y DELETE en partes.  

Por ejemplo, si tiene una instrucción INSERT que se suele tardar 1 hora, si puede, divídala en cuatro partes de 15 minutos cada una.  Aproveche los casos de registro mínimo, como CTAS, TRUNCATE, DROP TABLE o INSERT, para vaciar las tablas y así reducir el riesgo de reversión.  

Otra manera de eliminar reversiones es usar funciones de solo metadatos, como la modificación de particiones para la administración de datos.  Por ejemplo, en lugar de ejecutar una instrucción DELETE para eliminar todas las filas de una tabla cuyo order_date fuera octubre de 2001, podría dividir los datos mensualmente y desactivar la división con los datos de una partición vacía de otra tabla (consulte los ejemplos de ALTER TABLE).  

Para tablas sin particiones, puede usar CTAS en lugar de DELETE para escribir los datos que quiera mantener en una tabla.  Si CTAS tarda lo mismo, es una operación mucho más segura, ya que su registro de transacciones es mínimo y se puede cancelar rápidamente si es necesario.

Consulte también [Introducción a las transacciones](sql-data-warehouse-develop-transactions.md), [Optimización de transacciones](sql-data-warehouse-develop-best-practices-transactions.md), [Creación de particiones de tablas](sql-data-warehouse-tables-partition.md), [TRUNCATE TABLE](/sql/t-sql/statements/truncate-table-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest), [ALTER TABLE](/sql/t-sql/statements/alter-table-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest) y [Create table as select (CTAS)](sql-data-warehouse-develop-ctas.md).

## <a name="use-the-smallest-possible-column-size"></a>Uso del tamaño de columna mínimo

Al definir el DDL, usar el tipo de datos mínimo compatible con los datos mejorará el rendimiento de la consulta.  Este enfoque tiene especial importancia para las columnas CHAR y VARCHAR.  

Si el valor mayor máximo de una columna es 25 caracteres, defina la columna como VARCHAR(25).  Evite definir todas las columnas de caracteres con una longitud predeterminada de gran tamaño.  Defina las columnas como VARCHAR en lugar de NVARCHAR cuando no se necesite nada más.

Consulte también [Información general de tablas](sql-data-warehouse-tables-overview.md), [Tipos de datos de las tablas](sql-data-warehouse-tables-data-types.md) y [CREATE TABLE](sql-data-warehouse-tables-overview.md).

## <a name="optimize-clustered-columnstore-tables"></a>Optimización de tablas de almacén de columnas agrupadas

Los índices de almacén de columnas agrupadas son una de las maneras más eficaces para almacenar datos en el grupo de SQL.  De forma predeterminada, las tablas del grupo de SQL se crean como almacén de columnas agrupadas.  

> [!NOTE]
> Para conseguir un rendimiento óptimo de las consultas en las tablas de almacén de columnas, es importante la calidad de los segmentos.  

Escriben filas en las tablas de almacén de columnas bajo presión de memoria afecta a la calidad de segmento.  La calidad de segmento se puede medir por el número de filas de un grupo de filas comprimido.  

Para obtener instrucciones detalladas acerca de la detección y mejora de la calidad de los segmentos en las tablas de almacén de columnas agrupadas, consulte la sección [Causas de una calidad deficiente del índice de almacén de columnas](sql-data-warehouse-tables-index.md#causes-of-poor-columnstore-index-quality) del artículo sobre [Indexación de tablas](sql-data-warehouse-tables-index.md).  

Como es importante que los segmentos de almacén de columnas sean de una buena calidad, es conveniente usar identificadores de usuario que se encuentren en la clase de recursos grande o mediana para cargar los datos. El uso de [unidades de almacenamiento de datos](what-is-a-data-warehouse-unit-dwu-cdwu.md) inferiores significa que desea asignar una clase de recurso mayor para el usuario que realiza la carga.

Dado que las tablas de almacén de columnas, generalmente, no insertan datos en un segmento del almacén de columnas comprimido hasta que hay más de 1 millón de filas por tabla y cada tabla del grupo de SQL se divide en 60 partes, como norma general, las tablas de almacén de columnas no serán útiles para las consultas a menos que la tabla tenga más de 60 millones de filas.  

Para una tabla con menos de 60 millones de filas, podría no tener sentido el índice de almacén de columnas.  Pero tampoco molesta.  

Además, si divide los datos, recuerde que cada parte deberá tener 1 millón de filas para beneficiarse de un índice de almacén de columnas agrupadas.  Si una tabla tiene 100 particiones, tendrá que tener al menos 6 mil millones de filas para beneficiarse de un almacén de columnas agrupadas (60 distribuciones *100 particiones* 1 millón de filas).  

Si la tabla no tiene 6 mil millones de filas en este ejemplo, reduzca el número de particiones o considere la posibilidad de usar una tabla de apilamiento en su lugar.  También puede experimentar para ver si consigue un mejor rendimiento con una tabla de apilamiento con índices secundarios, en lugar de con una tabla de almacén de columnas.

> [!TIP]
> Al consultar una tabla de almacén de columnas, las consultas se ejecutarán más rápido si selecciona solo las que necesita.  

Consulte también [Indexación de tablas](sql-data-warehouse-tables-index.md), [Guía de índices de almacén de columnas](/sql/relational-databases/indexes/columnstore-indexes-overview?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest), y [Regeneración de índices de almacén de columnas](sql-data-warehouse-tables-index.md#rebuilding-indexes-to-improve-segment-quality).

## <a name="next-steps"></a>Pasos siguientes

Si no encuentra lo que busca en este artículo, pruebe a usar la "búsqueda de documentos" en la parte izquierda de esta página para buscar todos los documentos de Azure Synapse.  

En la [página de preguntas y respuestas de Microsoft para Azure Synapse](https://docs.microsoft.com/answers/topics/azure-synapse-analytics.html) puede publicar preguntas para otros usuarios y para el grupo del producto de Azure Synapse.  Supervisamos continuamente este foro para garantizar que sus preguntas las responde otro usuario o alguno de nosotros.  

Si prefiere formular sus preguntas en Stack Overflow, también tenemos un [foro de Stack Overflow sobre Azure SQL Data Warehouse](https://stackoverflow.com/questions/tagged/azure-sqldw).

Use la página de [comentarios de Azure Synapse](https://feedback.azure.com/forums/307516-sql-data-warehouse) para realizar solicitudes de características.  Añadir sus solicitudes o valoraciones positivas sobre otras solicitudes realmente nos ayuda a priorizar las características.

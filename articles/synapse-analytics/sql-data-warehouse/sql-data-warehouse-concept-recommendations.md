---
title: Recomendaciones de SQL de Synapse
description: Información acerca de las recomendaciones de SQL de Synapse y cómo se generan
services: synapse-analytics
author: kevinvngo
manager: craigg-msft
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: ''
ms.date: 04/30/2020
ms.author: kevin
ms.reviewer: igorstan
ms.custom: azure-synapse
ms.openlocfilehash: 546945d70554adbb28f19a3153faa67495e55f04
ms.sourcegitcommit: 50ef5c2798da04cf746181fbfa3253fca366feaa
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/30/2020
ms.locfileid: "82607762"
---
# <a name="synapse-sql-recommendations"></a>Recomendaciones de SQL de Synapse

En este artículo se describen las recomendaciones de SQL de Synapse atendidas mediante Azure Advisor.  

SQL Analytics proporciona recomendaciones para garantizar que la carga de trabajo de almacenamiento de datos está optimizado de forma coherente para el rendimiento. Las recomendaciones están totalmente integradas con [Azure Advisor](../../advisor/advisor-performance-recommendations.md?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json) para ofrecerle los procedimientos recomendados directamente en [Azure Portal](https://aka.ms/Azureadvisor). SQL Analytics recopila las recomendaciones de telemetría y superficies de la carga de trabajo activa diariamente. Los escenarios de recomendaciones admitidas se describen a continuación junto con instrucciones sobre cómo aplicar las acciones recomendadas.

[Compruebe sus recomendaciones](https://aka.ms/Azureadvisor) ya mismo. 

## <a name="data-skew"></a>Asimetría de datos

La asimetría de datos puede provocar cuellos de botella de recursos o movimientos de datos adicionales al ejecutar una carga de trabajo. En la siguiente documentación se describe cómo identificar la asimetría de datos y evitar que suceda al seleccionar una clave de distribución óptima.

- [Identificación y eliminación de la asimetría](sql-data-warehouse-tables-distribute.md#how-to-tell-if-your-distribution-column-is-a-good-choice)

## <a name="no-or-outdated-statistics"></a>Ninguna estadística o estadísticas obsoletas

La existencia de estadísticas deficientes puede afectar gravemente al rendimiento de las consultas, ya que puede dar lugar a que el optimizador de consultas de SQL genere planes de consulta deficientes. En la documentación siguiente se describen los procedimientos recomendados para crear y actualizar estadísticas:

- [Creación y actualización de estadísticas de tabla](sql-data-warehouse-tables-statistics.md)

Para ver la lista de las tablas afectadas por estas recomendaciones, ejecute el [script de T-SQL](https://github.com/Microsoft/sql-data-warehouse-samples/blob/master/samples/sqlops/MonitoringScripts/ImpactedTables) siguiente. Advisor ejecuta continuamente el mismo script de T-SQL para generar estas recomendaciones.

## <a name="replicate-tables"></a>Replicación de tablas

Para obtener recomendaciones sobre tablas replicadas, Advisor detecta los candidatos de tabla en función de las siguientes características físicas:

- Tamaño de la tabla replicada
- Número de columnas
- Tipo de distribución de la tabla
- Número de particiones

Advisor aprovecha continuamente la heurística basada en la carga de trabajo, como la frecuencia de acceso a la tabla, las filas devueltas como promedio y los umbrales relativos al tamaño y la actividad del almacenamiento de datos para asegurarse de que se generaron recomendaciones de alta calidad.

La sección siguiente describe la heurística basada en la carga de trabajo que puede encontrar en Azure Portal para cada recomendación de tabla replicada:

- Promedio de análisis: porcentaje promedio de filas devueltas de la tabla para cada acceso a la tabla durante los siete últimos días.
- Lectura frecuente, sin actualización: indica que la tabla no se actualizó durante los siete últimos días, mientras se muestra la actividad de acceso.
- Relación de lectura y actualización: la relación entre la frecuencia con que se accedió a la tabla y cuándo se actualizó durante los siete últimos días.
- Actividad: mide el uso basado en la actividad de acceso. Esta actividad compara la actividad de acceso a la tabla en relación con la actividad de acceso promedio a la tabla en el almacenamiento de datos durante los siete últimos días.

Actualmente Advisor solo mostrará como máximo cuatro candidatos de tabla replicada simultáneamente con índices de almacén de columnas agrupados, dando prioridad a la máxima actividad.

> [!IMPORTANT]
> La recomendación de tabla replicada no es una prueba completa y no tiene en cuenta las operaciones de movimiento de datos de las cuentas. Estamos trabajando para agregar esto como un método heurístico, pero mientras tanto, debe validar siempre la carga de trabajo después de aplicar la recomendación. Para más información sobre las tablas replicadas, visite la siguiente [documentación](design-guidance-for-replicated-tables.md#what-is-a-replicated-table).


## <a name="adaptive-gen2-cache-utilization"></a>Uso de la memoria caché adaptable (Gen2)
Si tiene un gran espacio de trabajo, puede experimentar un porcentaje de aciertos de caché bajo y un uso elevado de la memoria caché. En este escenario, debe escalar verticalmente para aumentar la capacidad de la memoria caché y volver a ejecutar la carga de trabajo. Para más información, visite la siguiente [documentación](https://docs.microsoft.com/azure/synapse-analytics/sql-data-warehouse/sql-data-warehouse-how-to-monitor-cache). 

## <a name="tempdb-contention"></a>Contención de tempdb

El rendimiento de las consultas puede reducirse si hay una gran contención de tempdb.  La contención de tempdb puede producirse a través de tablas temporales definidas por el usuario o cuando hay una gran cantidad de movimiento de datos. En este escenario, puede escalar para obtener más asignación de tempdb y [configurar las clases de recursos y la administración de cargas de trabajo](https://docs.microsoft.com/azure/synapse-analytics/sql-data-warehouse/sql-data-warehouse-workload-management) para proporcionar más memoria a las consultas. 

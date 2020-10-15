---
title: Registros en Azure Monitor | Microsoft Docs
description: Describe los registros de Azure Monitor que se usan para realizar análisis avanzados de los datos de supervisión.
documentationcenter: ''
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.date: 09/09/2020
ms.author: bwren
ms.openlocfilehash: e08c649b9a1d7e8b909a413ee435fce30a8d7e48
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "90032793"
---
# <a name="azure-monitor-logs-overview"></a>Introducción a los registros de Azure Monitor
Los registros de Azure Monitor son una característica de Azure Monitor que recopila y organiza los datos de registros provenientes de una variedad de orígenes y hacen que estén disponibles para su análisis mediante un lenguaje de consulta sofisticado. Los datos que provienen de distintos orígenes se pueden consolidar en una sola área de trabajo y se analizan en conjunto para realizar dichas tareas y visualización, alertas y análisis de tendencias.

## <a name="relationship-to-azure-monitor-metrics"></a>Relación con las métricas de Azure Monitor
Las métricas de Azure Monitor almacenan datos numéricos en una base de datos de serie temporal, lo que hace que los datos sean más ligeros que los registros de Azure Monitor y puedan admitir escenarios casi en tiempo real, lo que los hace especialmente útiles para las alertas y la detección rápida de problemas. Sin embargo, las métricas solo pueden almacenar datos numéricos en una estructura determinada, mientras que los registros pueden almacenar una variedad de tipos de datos distintos, cada uno con su propia estructura. También puede hacer análisis complejos en los datos de los registros mediante el uso de consultas de registros que no se pueden usar para el análisis de los datos de las métricas.

Por lo general, los datos numéricos se envían desde los orígenes de los datos a los registros, además de las métricas. Si bien hay un cargo adicional por recopilar y conservar estos datos en los registros, esto le permite incluir datos de métricas en las consultas de registros y analizarlos con sus otros datos de supervisión.

## <a name="relationship-to-azure-data-explorer"></a>Relación con Azure Data Explorer
Los registros de Azure Monitor se basa en Azure Data Explorer. Un área de trabajo de Log Analytics es casi equivalente a una base de datos de Azure Data Explorer, las tablas están estructuradas de la misma manera y ambas usan el mismo lenguaje de consulta de Kusto (KQL). La experiencia de usar Log Analytics para trabajar con las consultas de Azure Monitor en Azure Portal es similar a la experiencia de usar la interfaz de usuario web de Azure Data Explorer. Incluso puede [incluir datos de un área de trabajo de Log Analytics en una consulta de Azure Data Explorer](/azure/data-explorer/query-monitor-data). 


## <a name="structure-of-data"></a>Estructura de los datos
Los datos recopilados por los registros de Azure Monitor se almacenan en un [área de trabajo de Log Analytics](./design-logs-deployment.md) que contiene varias tablas, donde cada una de las cuales almacena datos de un origen determinado. El área de trabajo define la ubicación geográfica de los datos, derechos de acceso que definen qué usuarios pueden acceder a los datos y opciones de configuración, como el plan de tarifa y la retención de datos. Puede usar una sola área de trabajo para todos los datos de supervisión o crear varias áreas de trabajo en función de sus requisitos. Consulte [Diseño de la implementación de registros de Azure Monitor](design-logs-deployment.md) para información sobre la creación de varias áreas de trabajo.

Cada área de trabajo contiene varias tablas que están organizadas en columnas independientes con varias filas de datos. Cada tabla se define mediante un conjunto único de columnas compartidas por las filas de datos que proporciona el origen de datos. 

[![Estructura de los registros de Azure Monitor](media/data-platform-logs/logs-structure.png)](media/data-platform-logs/logs-structure.png#lightbox)


Los datos de registros de Application Insights también se almacenan en los registros de Azure Monitor, pero de manera distinta según la configuración de la aplicación. Si se trata de una aplicación basada en áreas de trabajo, los datos se almacenan en un área de trabajo de Log Analytics en un conjunto de tablas estándar para contener datos como solicitudes de aplicación, excepciones y vistas de página. Varias aplicaciones pueden usar la misma área de trabajo. Si se trata de una aplicación clásica, los datos no se almacenan en un área de trabajo de Log Analytics. Usa el mismo lenguaje de consulta y se crean y ejecutan consultas con la misma herramienta de Log Analytics en Azure Portal. Sin embargo, los datos de las aplicaciones clásicas se almacenan separados entre sí. Su estructura general es igual que la de las aplicaciones basadas en áreas de trabajo, a pesar de que los nombres de tabla y columna son distintos. Consulte [Cambios en los recursos basados en áreas de trabajo](../app/apm-tables.md) para ver una comparación detallada de ambas.


> [!NOTE]
> Todavía se proporciona compatibilidad total con versiones anteriores con las consultas de recursos clásicas, los libros y las alertas basadas en registro de Application Insights dentro de la experiencia de Application Insights. Para consultar o ver en la [nueva estructura o esquema de tabla basados en área de trabajo](../app/apm-tables.md), primero debe ir al área de trabajo de Log Analytics. Durante la versión preliminar, al seleccionar **Registros** en los paneles de Application Insights, se proporciona acceso a la experiencia de consulta clásica de Application Insights. Consulte [Ámbito de la consulta](../log-query/scope.md) para más detalles.


[![Estructura de los registros de Azure Monitor para Application Insights](media/data-platform-logs/logs-structure-ai.png)](media/data-platform-logs/logs-structure-ai.png#lightbox)

## <a name="log-queries"></a>Consultas de registros
Los datos se recuperar de un área de trabajo de Log Analytics mediante una [consulta de registros](../log-query/log-query-overview.md), que es una solicitud de solo lectura para procesar datos y devolver resultados. Las consultas de registros se escriben en el [lenguaje de consulta Kusto (KQL)](/azure/data-explorer/kusto/query/), que es el lenguaje de consulta que usa Azure Data Explorer. Use Log Analytics, que es una herramienta de Azure Portal para editar y ejecutar consultas de registros y analizar los resultados de forma interactiva. Luego puede usar las consultas que cree para admitir otras características en Azure Monitor como los libros y las alertas de consultas de registro.


## <a name="sources-of-data-for-azure-monitor-logs"></a>Orígenes de los datos para los registros de Azure Monitor
Azure Monitor recopila datos de registros de diversos orígenes, incluidos los recursos de Azure Monitor y agentes que se ejecutan en máquinas virtuales. Consulte [¿Qué supervisa Azure Monitor?](../monitor-reference.md) para una lista completa de los orígenes de datos que envían datos a un área de trabajo de Log Analytics.



## <a name="next-steps"></a>Pasos siguientes

- Obtenga información sobre las [consultas de registros](../log-query/log-query-overview.md) para recuperar y analizar datos provenientes de un área de trabajo de Log Analytics.
- Obtenga más información acerca de las [métricas en Azure Monitor](data-platform-metrics.md).
- Obtenga información sobre la [supervisión de datos disponible](data-sources.md) para diferentes recursos en Azure.


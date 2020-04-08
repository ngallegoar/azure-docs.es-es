---
title: Optimización de la supervisión y el rendimiento
description: Información general sobre las funcionalidades de optimización de la supervisión y del rendimiento y la metodología en Azure SQL Database.
services: sql-database
ms.service: sql-database
ms.subservice: performance
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: jovanpop-msft
ms.author: jovanpop
ms.reviewer: jrasnick, carlrab
ms.date: 03/10/2020
ms.openlocfilehash: 837d88665c1fdffe902c9c478e5d6dc65a2e402a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "79232472"
---
# <a name="monitoring-and-performance-tuning-in-azure-sql-database"></a>Optimización de la supervisión y el rendimiento de Azure SQL Database

Para supervisar el rendimiento de una base de datos en Azure SQL Database, empiece por supervisar los recursos de CPU y de E/S usados por la carga de trabajo en relación con el nivel de rendimiento de la base de datos elegido al seleccionar un nivel de servicio y un nivel de rendimiento determinados. Para ello, Azure SQL Database emite métricas de recursos que se pueden ver en Azure Portal o mediante una de estas herramientas de administración de SQL: [Azure Data Studio](https://docs.microsoft.com/sql/azure-data-studio/what-is) o [SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/sql-server-management-studio-ssms) (SSMS).

En el caso de las bases de datos únicas y agrupadas, Azure SQL Database proporciona una serie de asesores de base de datos para proporcionar recomendaciones de optimización del rendimiento y opciones de ajuste automáticas para mejorar el rendimiento. Además, en Información de rendimiento de consultas se muestran los detalles de las consultas responsables de la mayor parte del uso de la CPU y la E/S de las bases de datos individuales y agrupadas.

Azure SQL Database proporciona funcionalidades adicionales de supervisión y optimización respaldadas por inteligencia artificial para ayudarle a solucionar los problemas y maximizar el rendimiento de las bases de datos y soluciones. Puede optar por configurar la [exportación de streaming](sql-database-metrics-diag-logging.md) de los resultados de [Intelligent Insights](sql-database-intelligent-insights.md) y de otras métricas y registros de recursos de SQL Database a uno de varios destinos para su consumo y análisis, especialmente mediante [SQL Analytics](../azure-monitor/insights/azure-sql.md)). Azure SQL Analytics es una solución de supervisión en la nube que se utiliza para supervisar el rendimiento de todas las bases de datos de Azure SQL Database a escala, entre varias suscripciones y en una vista única. Para obtener una lista de los registros y las métricas que puede exportar, consulte el artículo sobre la [telemetría de diagnóstico para la exportación](sql-database-metrics-diag-logging.md#diagnostic-telemetry-for-export-for-azure-sql-database).

Por último, SQL tiene sus propias funcionalidades de supervisión y diagnóstico con el [Almacén de consultas de SQL Server](https://docs.microsoft.com/sql/relational-databases/performance/monitoring-performance-by-using-the-query-store) y las [vistas de administración dinámica (DMV)](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/system-dynamic-management-views). Consulte [Supervisión del rendimiento en Azure SQL Database con vistas de administración dinámica](sql-database-monitoring-with-dmvs.md) para que los scripts supervisen diversos problemas de rendimiento.

## <a name="monitoring-and-tuning-capabilities-in-the-azure-portal"></a>Funcionalidades de supervisión y optimización en Azure Portal

En Azure Portal, Azure SQL Database proporciona supervisión de las métricas de recursos para todos los tipos de implementación. Además, en el caso de bases de datos únicas y agrupadas, los asesores de bases de datos y Información de rendimiento de consultas proporcionan recomendaciones para la optimización y análisis de rendimiento de las consultas. Por último, en Azure Portal, lo puede habilitar de forma automática para los servidores lógicos y sus bases de datos individuales y agrupadas.

### <a name="sql-database-resource-monitoring"></a>Supervisión de recursos de SQL Database

Puede supervisar rápidamente las siguientes métricas de recursos en Azure Portal en la vista **Métricas**:

- **Uso de DTU**

  Compruebe si la base de datos o grupo elástico alcanza el 100 % del uso de DTU durante un período de tiempo prolongado. Un uso elevado de DTU indica que la carga de trabajo podría necesitar más recursos de CPU o de E/S. También puede indicar consultas que deben optimizarse.
- **Uso de CPU**

  Compruebe si una base de datos, un grupo elástico o una instancia administrada alcanza el 100 % del uso de CPU durante un período de tiempo prolongado. Una CPU alta indica que la carga de trabajo podría necesitar más recursos de CPU o de E/S. También puede indicar consultas que deben optimizarse.
- **Uso de E/S**

  Compruebe si una base de datos, un grupo elástico o una instancia administrada están llegando a los límites de E/S del almacenamiento subyacente. Un uso elevado de E/S indica que la carga de trabajo podría necesitar más recursos de CPU o de E/S. También puede indicar consultas que deben optimizarse.

  ![Métricas de los recursos](./media/sql-database-monitor-tune-overview/resource-metrics.png)

### <a name="database-advisors"></a>Asesores de bases de datos
Azure SQL Database incluye [asesores de bases de datos](sql-database-advisor.md) que proporcionan recomendaciones para la optimización del rendimiento de bases de datos únicas y agrupadas. Estas recomendaciones están disponibles en Azure Portal y en [PowerShell](https://docs.microsoft.com/powershell/module/az.sql/get-azsqldatabaseadvisor). También puede habilitar el [ajuste automático](sql-database-automatic-tuning.md) para que SQL Database pueda implementar automáticamente estas recomendaciones de optimización.

### <a name="query-performance-insight"></a>Información de rendimiento de consultas

En [Información de rendimiento de consultas](sql-database-query-performance.md) se muestra el rendimiento en Azure Portal de las consultas que más consumen y de mayor tamaño para bases de datos únicas y agrupadas.

## <a name="generate-intelligent-assessments-of-performance-issues"></a>Generación de evaluaciones inteligentes de problemas de rendimiento

[Intelligent Insights](sql-database-intelligent-insights.md) de Azure SQL Database usa inteligencia integrada para supervisar continuamente el uso de la base de datos mediante inteligencia artificial y detectar eventos potencialmente perjudiciales que provoquen un rendimiento bajo. Intelligent Insights detecta automáticamente los problemas de rendimiento con bases de datos de Azure SQL Database en función de los tiempos de espera, los errores o el agotamiento de los tiempos de espera. Una vez detectados, se realiza un análisis detallado que genera un registro de recursos (denominado SQLInsights) con una [evaluación inteligente de los problemas](sql-database-intelligent-insights-troubleshoot-performance.md). Esta evaluación está formada por un análisis de la causa raíz del problema de rendimiento de la base de datos y, si es posible, recomendaciones para mejorar el rendimiento.

Intelligent Insights es una funcionalidad única de inteligencia integrada de Azure que permite lo siguiente:

- Supervisión proactiva
- Información reveladora personalizada acerca del rendimiento
- Detección temprana de degradación del rendimiento de la base de datos
- El análisis de la causa raíz de los problemas detectados
- Recomendaciones de mejora del rendimiento
- Funcionalidad de escalabilidad horizontal de cientos de miles de bases de datos
- Impacto positivo para los recursos de DevOps y el costo total de propiedad

## <a name="enable-the-streaming-export-of-metrics-and-resource-logs"></a>Habilitación de la exportación de streaming de métricas y registros de recursos

Puede habilitar y configurar la [exportación de streaming de la telemetría de diagnóstico](sql-database-metrics-diag-logging.md) a uno de varios destinos, incluido el registro de recursos de Intelligent Insights. Use [SQL Analytics](../azure-monitor/insights/azure-sql.md) y otras funcionalidades para consumir esta telemetría de diagnóstico adicional para identificar y resolver problemas de rendimiento.

Puede configurar ajustes de diagnóstico para transmitir categorías de métricas y registros de recursos a bases de datos únicas, bases de datos agrupadas, grupos elásticos, instancias administradas y bases de datos de instancia a uno de los siguientes recursos de Azure.

### <a name="log-analytics-workspace-in-azure-monitor"></a>Área de trabajo de Log Analytics en Azure Monitor

Puede transmitir métricas y registros de recursos a un [área de trabajo de Log Analytics en Azure Monitor](../azure-monitor/platform/resource-logs-collect-workspace.md). Los datos transmitidos aquí los puede consumir [SQL Analytics](../azure-monitor/insights/azure-sql.md), que es una solución de supervisión en la nube que proporciona supervisión inteligente de las bases de datos, lo que incluye informes de rendimiento, alertas y recomendaciones de mitigación. Los datos que se transmiten a un área de trabajo de Log Analytics se pueden analizar con otros datos de supervisión recopilados y también permiten aprovechar otras características de Azure Monitor, como las alertas y las visualizaciones.

### <a name="azure-event-hubs"></a>Azure Event Hubs

Puede transmitir las métricas y los registros de recursos a [Azure Event Hubs](../azure-monitor/platform/resource-logs-stream-event-hubs.md). Transmita la telemetría de diagnóstico a los centros de eventos para proporcionar la siguiente funcionalidad:

- **Transmisión de registros a sistemas de registro y telemetría de terceros**

  Transmita todas sus métricas y todos sus registros de recursos a un centro de eventos único para canalizar datos de registro en una herramienta SIEM o de análisis de registros de terceros.
- **Creación de una plataforma personalizada de registro y telemetría**

  La naturaleza altamente escalable de publicación y suscripción de los centros de eventos otorga la flexibilidad necesaria para ingerir métricas y registros de recursos en una plataforma de telemetría personalizada. Consulte [Designing and Sizing a Global Scale Telemetry Platform on Azure Event Hubs](https://azure.microsoft.com/documentation/videos/build-2015-designing-and-sizing-a-global-scale-telemetry-platform-on-azure-event-Hubs/) (Diseño y cambio de tamaño de una plataforma de telemetría a escala global en Azure Event Hubs) para más información.
- **Visualización del estado del servicio mediante la transmisión de datos a Power BI**

  Use Event Hubs, Stream Analytics y Power BI para transformar los datos de diagnóstico en información sobre los servicios de Azure prácticamente en tiempo real. Consulte [Stream Analytics y Power BI: panel de análisis en tiempo real de flujo de datos](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-power-bi-dashboard) para detalles sobre esta solución.

### <a name="azure-storage"></a>Azure Storage

Transmita las métricas y los registros de recursos a [Azure Storage](../azure-monitor/platform/resource-logs-collect-storage.md). Utilice el almacenamiento de Azure para archivar gran cantidad de información de telemetría de diagnóstico por una fracción del costo de las dos opciones anteriores de streaming.

## <a name="use-extended-events-in-the-sql-database-engine"></a>Uso de los eventos extendidos en el motor de SQL Database

Además, puede usar [eventos extendidos](https://docs.microsoft.com/sql/relational-databases/extended-events/extended-events) en SQL para llevar a cabo una supervisión avanzada y solución de problemas adicionales. La arquitectura de eventos extendidos permite a los usuarios recopilar tantos datos como sea necesario para solucionar o identificar un problema de rendimiento. Para más información sobre los eventos extendidos en SQL Database, consulte [Eventos extendidos en SQL Database](sql-database-xevent-db-diff-from-svr.md).

## <a name="next-steps"></a>Pasos siguientes

- Para más información sobre las recomendaciones de rendimiento inteligentes para bases de datos únicas y agrupadas, consulte [Recomendaciones de rendimiento para SQL Database](sql-database-advisor.md).
- Para más información sobre cómo supervisar de forma automática el rendimiento de la base de datos con diagnósticos automatizados y análisis de causa principal de problemas de rendimiento, consulte [Azure SQL Intelligent Insights](sql-database-intelligent-insights.md).
'''''''''
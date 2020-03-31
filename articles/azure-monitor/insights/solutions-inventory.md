---
title: Inventario de las soluciones de supervisión en Azure | Microsoft Docs
description: Las soluciones de supervisión de Azure Monitor son una colección de reglas de lógica, visualización y adquisición de datos que proporcionan métricas que giran en torno a una determinada área de problemas.  Este artículo proporciona una lista de las soluciones de supervisión disponibles de Microsoft y los detalles del método y la frecuencia de recopilación de datos.
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 06/26/2018
ms.openlocfilehash: 7b88d957bce45bf518fc77584f1691de8010459a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "77663137"
---
# <a name="inventory-and-data-collection-details-for-monitoring-solutions-in-azure"></a>Detalles del inventario y la recopilación de datos para las soluciones de supervisión en Azure
Las [soluciones de supervisión](solutions.md) aprovechan los servicios de Azure para proporcionar información adicional sobre el funcionamiento de una aplicación o servicio determinados. Las soluciones de supervisión suelen recopilar datos de registro y proporcionar consultas y vistas para analizar los datos recopilados. Puede agregar soluciones de supervisión a su instancia de Azure Monitor para todas las aplicaciones y los servicios que use. Suelen estar disponibles sin costo, pero la recopilación de datos puede suponer cargos por uso.

En este artículo se incluye una lista de las [soluciones de supervisión](solutions.md) disponibles de Microsoft con vínculos a la documentación detallada.  También se proporciona información sobre el método y la frecuencia de recopilación de datos en Azure Monitor.  Puede usar la información de este artículo para identificar las diferentes soluciones disponibles y comprender los requisitos de conexión y flujo de datos de las distintas soluciones de supervisión.

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../../includes/azure-monitor-log-analytics-rebrand.md)]

## <a name="list-of-monitoring-solutions"></a>Lista de soluciones de supervisión.

En la siguiente tabla se recogen las [soluciones de supervisión](solutions.md) en Azure que proporciona Microsoft. Una entrada en la columna significa que la solución recopila datos en Azure Monitor con ese método.  Si una solución no tiene columnas seleccionadas, eso significa que escribe directamente en Azure Monitor desde otro servicio de Azure. Siga el vínculo a la documentación detallada de cada uno de ellos para más información.

Explicaciones de las columnas:

- **Agente de supervisión de Microsoft**: agente que se usa en Windows y Linux para ejecutar el módulo de administración de SCOM y las soluciones de supervisión de Azure. En esta configuración, el agente está conectado directamente a Azure Monitor sin conexión a un grupo de administración de Operations Manager. 
- **Operations Manager**: agente idéntico al de supervisión de Microsoft. En esta configuración, se [conecta a un grupo de administración de Operations Manager](../platform/om-agents.md) conectado a su vez a Azure Monitor. 
-  **Azure Storage**: la solución recopila datos de una cuenta de Azure Storage. 
- **¿Se requiere Operations Manager?** Se necesita un grupo de administración de Operations Manager conectado para que la solución de supervisión recopile los datos. 
- **Los datos del agente de Operations Manager se envían a través del grupo de administración**: si el agente está [conectado a un grupo de administración de SCOM](../platform/om-agents.md), los datos se envían a Azure Monitor desde el servidor de administración. En este caso, el agente no necesita conectarse directamente a Azure Monitor. Si esta casilla no está seleccionada, los datos se envían desde el agente directamente a Azure Monitor, aunque el agente esté conectado a un grupo de administración de SCOM. Tendrá que comunicarse con Azure Monitor mediante la [puerta de enlace de Log Analytics](../platform/gateway.md).
- **Frecuencia de recopilación**: especifica la frecuencia con la que la solución de supervisión recopila los datos. 



| **Solución de supervisión** | **Plataforma** | **Agente de supervisión de Microsoft** | **Agente de Operations Manager** | **Almacenamiento de Azure** | **¿Se requiere Operations Manager?** | **Se envían los datos del agente de Operations Manager a través del grupo de administración** | **Frecuencia de recopilación** |
| --- | --- | --- | --- | --- | --- | --- | --- |
| [Análisis de Activity Log](../platform/activity-log-collect.md) | Azure | | | | | | según notificación |
| [Evaluación de AD](ad-assessment.md) |Windows |&#8226; |&#8226; | | |&#8226; |7 días |
| [Estado de replicación de AD](ad-replication-status.md) |Windows |&#8226; |&#8226; | | |&#8226; |5 días |
| [Agent Health](solution-agenthealth.md) | Windows y Linux | &#8226; | &#8226; | | | &#8226; | 1 minuto. |
| [Alert Management](../platform/alert-management-solution.md) (Nagios) |Linux |&#8226; | | | | |a la llegada |
| [Alert Management](../platform/alert-management-solution.md) (Zabbix) |Linux |&#8226; | | | | |1 minuto. |
| [Alert Management](../platform/alert-management-solution.md) (Operations Manager) |Windows | |&#8226; | |&#8226; |&#8226; |3 minutos |
| [Azure Site Recovery](../../site-recovery/site-recovery-overview.md) | Azure | | | | | | N/D |
| [Application Insights Connector (en desuso)](../platform/app-insights-connector.md) | Azure | | | |  |  | según notificación |
| [Automation Hybrid Worker](../../automation/automation-hybrid-runbook-worker.md) | Windows | &#8226; | &#8226; |  |  |  | N/D |
| [Azure Application Gateway Analytics](azure-networking-analytics.md) | Azure |  |  |  |  |  | según notificación |
| **Solución de supervisión** | **Plataforma** | **Agente de supervisión de Microsoft** | **Agente de Operations Manager** | **Almacenamiento de Azure** | **¿Se requiere Operations Manager?** | **Se envían los datos del agente de Operations Manager a través del grupo de administración** | **Frecuencia de recopilación** |
| [Azure Network Security Group Analytics (en desuso)](azure-networking-analytics.md) | Azure |  |  |  |  |  | según notificación |
| [Azure SQL Analytics (versión preliminar)](azure-sql.md) | Windows | | | | | | 1 minuto. |
| [Backup](https://azure.microsoft.com/resources/templates/101-backup-oms-monitoring/) | Azure |  |  |  |  |  | según notificación |
| [Capacity and Performance (versión preliminar)](capacity-performance.md) |Windows |&#8226; |&#8226; | | |&#8226; |a la llegada |
| [Seguimiento de cambios](../../automation/change-tracking.md) |Windows |&#8226; |&#8226; | | |&#8226; |[varía](../../automation/change-tracking.md#change-tracking-data-collection-details) |
| [Seguimiento de cambios](../../automation/change-tracking.md) |Linux |&#8226; | | | | |[varía](../../automation/change-tracking.md#change-tracking-data-collection-details) |
| [Contenedores](containers.md) | Windows y Linux | &#8226; | &#8226; |  |  |  | 3 minutos |
| [Análisis de Key Vault](azure-key-vault.md) |Windows | | | | | |según notificación |
| [Evaluación de malware](../../security-center/security-center-install-endpoint-protection.md) |Windows |&#8226; |&#8226; | | |&#8226; |Cada hora |
| [Monitor de rendimiento de red](network-performance-monitor.md) | Windows | &#8226; | &#8226; |  |  |  | Protocolos de enlace TCP cada 5 segundos; se envían datos cada 3 minutos. |
| [Office 365 Analytics (versión preliminar)](solution-office-365.md) |Windows | | | | | |según notificación |
| **Solución de supervisión** | **Plataforma** | **Agente de supervisión de Microsoft** | **Agente de Operations Manager** | **Almacenamiento de Azure** | **¿Se requiere Operations Manager?** | **Se envían los datos del agente de Operations Manager a través del grupo de administración** | **Frecuencia de recopilación** |
| [Análisis de Service Fabric](../../service-fabric/service-fabric-diagnostics-oms-setup.md) |Windows | | |&#8226; | | |5 minutos |
| [Mapa de servicio](service-map.md) | Windows y Linux | &#8226; | &#8226; |  |  |  | 15 segundos |
| [Evaluación de SQL](sql-assessment.md) |Windows |&#8226; |&#8226; | | |&#8226; |7 días |
| [SurfaceHub](surface-hubs.md) |Windows |&#8226; | | | | |a la llegada |
| [System Center Operations Manager Assessment (versión preliminar)](scom-assessment.md) | Windows | &#8226; | &#8226; |  |  | &#8226; | siete días |
| [Administración de actualizaciones](../../automation/automation-update-management.md) | Windows |&#8226; |&#8226; | | |&#8226; |al menos 2 veces al día y 15 minutos después de instalar una actualización |
| [Preparación para la actualización](https://docs.microsoft.com/windows/deployment/upgrade/upgrade-readiness-get-started) | Windows | &#8226; |  |  |  |  | 2 días |
| [VMware Monitoring (en desuso)](vmware.md) | Linux | &#8226; |  |  |  |  | 3 minutos |
| [Wire Data 2.0 (versión preliminar)](wire-data.md) |Windows (2012 R2 / 8.1 o posterior) |&#8226; |&#8226; | | | | 1 minuto. |




## <a name="next-steps"></a>Pasos siguientes
* Aprenda a [instalar y a usar las soluciones de supervisión](solutions.md).
* Aprenda a [crear consultas](../log-query/log-query-overview.md) para analizar los datos recopilados por las soluciones de supervisión.

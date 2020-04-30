---
title: Análisis de dependencias en Azure Migrate Server Assessment
description: Se describe cómo usar el análisis de dependencias para realizar evaluaciones mediante Azure Migrate Server Assessment.
ms.topic: conceptual
ms.date: 04/15/2020
ms.openlocfilehash: f0b956620895ae2264b53916015d440f5e586eb2
ms.sourcegitcommit: 75089113827229663afed75b8364ab5212d67323
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/22/2020
ms.locfileid: "82024768"
---
# <a name="dependency-analysis"></a>Análisis de dependencias

En este artículo se describe el análisis de dependencias en Azure Migrate Server Assessment.

## <a name="overview"></a>Información general

El análisis de dependencias le ayuda a identificar las dependencias entre las máquinas locales que quiere evaluar y migrar a Azure. 

- En Azure Migrate: Server Assessment, puede recopilar las máquinas en un grupo y, después, evaluar el grupo. El análisis de dependencias le ayuda a agrupar las máquinas con más precisión y con una confianza alta para la evaluación.
- El análisis de dependencias le permite identificar las máquinas que se deben migrar juntas. Puede identificar si las máquinas están en uso o si se pueden retirar en lugar de migrarlas.
- El análisis de dependencias le ayuda a garantizar que no se olvida de nada y a evitar interrupciones inesperadas durante la migración.
- Es especialmente útil si no está seguro de si las máquinas forman parte de la implementación de una aplicación que quiera migrar a Azure.
- [Revise](common-questions-discovery-assessment.md#what-is-dependency-visualization) las preguntas habituales sobre el análisis de dependencias.

Hay dos opciones para implementar el análisis de dependencias:

- **Basada en agentes**: el análisis de dependencias basado en agente requiere que se instalen agentes en cada máquina local que quiera analizar.
- **Sin agentes**: con el análisis sin agente, no es necesario instalar agentes en las máquinas que quiera comprobar. Esta opción se encuentra actualmente en versión preliminar y solo está disponible para máquinas virtuales de VMware.

> [!NOTE]
> El análisis de dependencias basado en agente no está disponible en Azure Government. Puede usar el análisis de dependencias sin agente.

## <a name="agentless-analysis"></a>Análisis sin agente

El análisis de dependencias sin agente funciona mediante la captura de los datos de conexión TCP de las máquinas en las que se ha habilitado. No hay ningún agente instalado en las máquinas que quiera analizar.

### <a name="collected-data"></a>Datos recopilados

Una vez iniciada la detección de dependencias, el dispositivo sondea los datos de las máquinas cada cinco minutos para recopilar datos. Estos datos se recopilan de las máquinas virtuales invitadas a través de vCenter Server, con las API de vSphere. Los datos recopilados se procesan en la aplicación de Azure Migrate para deducir la información de identidad, y se envían a Azure Migrate cada seis horas.

El sondeo recopila estos datos de las máquinas: 
- Nombre de los procesos que tienen conexiones activas.
- Nombre de la aplicación que ejecuta los procesos que tienen conexiones activas.
- Puerto de destino en las conexiones activas.

## <a name="agent-based-analysis"></a>Análisis basado en agente

En el análisis basado en agente, Server Assessment emplea la [solución Service Map](../azure-monitor/insights/service-map.md) de Azure Monitor para permitir la visualización y el análisis de dependencias. [Microsoft Monitoring Agent o el agente de Log Analytics](../azure-monitor/platform/agents-overview.md#log-analytics-agent) y el [agente de dependencias](../azure-monitor/platform/agents-overview.md#dependency-agent) deben estar instalados en cada máquina que quiera analizar.

### <a name="collected-data"></a>Datos recopilados

En el análisis basado en agente, se recopilan los datos siguientes:

- Nombre de aplicación, proceso y nombre del servidor de la máquina de origen.
- Puerto, nombre de aplicación, proceso y nombre del servidor de la máquina de destino.
- Se recopila la información sobre el número de conexiones, la latencia y la transferencia de datos, y está disponible para las consultas de Log Analytics. 


## <a name="compare-agentless-and-agent-based"></a>Comparación basada en agente y sin agente

La diferencia entre la visualización sin agente y la visualización basada en agente se resume en la siguiente tabla.

**Requisito** | **Sin agente** | **Basado en agente**
--- | --- | ---
Soporte técnico | Esta opción se encuentra actualmente en versión preliminar y solo está disponible para máquinas virtuales de VMware. [Revise](migrate-support-matrix-vmware.md#agentless-dependency-analysis-requirements) los sistemas operativos compatibles. | En disponibilidad general (GA).
Agente | No es necesario instalar agentes en las máquinas que quiere comprobar. | Agentes que debe instalar en las máquinas locales que quiere analizar: [Microsoft Monitoring Agent (MMA)](https://docs.microsoft.com/azure/log-analytics/log-analytics-agent-windows) y el [agente de dependencia](https://docs.microsoft.com/azure/azure-monitor/platform/agents-overview#dependency-agent). 
Log Analytics | No se requiere. | Azure Migrate usa la solución [Service Map](https://docs.microsoft.com/azure/operations-management-suite/operations-management-suite-service-map) de los [registros de Azure Monitor](https://docs.microsoft.com/azure/log-analytics/log-analytics-overview) para el análisis de dependencias. 
Funcionamiento | Captura los datos de conexión TCP en las máquinas habilitadas para la visualización de dependencias. Después de la detección, recopila datos en intervalos de cinco minutos. | Los agentes de Service Map instalados en una máquina recopilan datos acerca de los procesos de TCP, así como de las conexiones de entrada o salida para cada proceso.
data | Nombre de aplicación, proceso y nombre del servidor de la máquina de origen.<br/><br/> Puerto, nombre de aplicación, proceso y nombre del servidor de la máquina de destino. | Nombre de aplicación, proceso y nombre del servidor de la máquina de origen.<br/><br/> Puerto, nombre de aplicación, proceso y nombre del servidor de la máquina de destino.<br/><br/> Se recopila la información sobre el número de conexiones, la latencia y la transferencia de datos, y está disponible para las consultas de Log Analytics. 
Visualización | El mapa de dependencias de un solo servidor se puede ver durante un plazo de entre una hora y 30 días. | Mapa de dependencia de un solo servidor.<br/><br/> El mapa puede verse solo durante una hora.<br/><br/> Mapa de dependencias de un grupo de servidores.<br/><br/> Agregue y quite servidores de un grupo desde la vista de mapa.
Exportación de datos | Actualmente no se puede descargar en formato tabular. | Los datos se pueden consultar con Log Analytics.



## <a name="next-steps"></a>Pasos siguientes
- Revise los requisitos para configurar el análisis basado en agente para [máquinas virtuales de VMware](migrate-support-matrix-vmware.md#agent-based-dependency-analysis-requirements), [servidores físicos](migrate-support-matrix-physical.md#agent-based-dependency-analysis-requirements) y [máquinas virtuales de Hyper-V](migrate-support-matrix-hyper-v.md#agent-based-dependency-analysis-requirements).
- [Revise](migrate-support-matrix-vmware.md#agentless-dependency-analysis-requirements) los requisitos para el análisis sin agente de máquinas virtuales de VMware.
- [Configure](how-to-create-group-machine-dependencies.md) la visualización de dependencias basada en agente.
- [Pruebe](how-to-create-group-machine-dependencies-agentless.md) la visualización de dependencias sin agente para máquinas virtuales de VMware.
- Revise las [preguntas habituales](common-questions-discovery-assessment.md#what-is-dependency-visualization) sobre la visualización de dependencias.



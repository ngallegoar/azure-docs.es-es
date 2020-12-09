---
title: Análisis de dependencias en Azure Migrate Server Assessment
description: Se describe cómo usar el análisis de dependencias para realizar evaluaciones mediante Azure Migrate Server Assessment.
ms.topic: conceptual
author: vineetvikram
ms.author: vivikram
ms.manager: abhemraj
ms.date: 09/15/2020
ms.openlocfilehash: 1f198d47191e7893e74b072ae8fd10546e3a6ee7
ms.sourcegitcommit: ea551dad8d870ddcc0fee4423026f51bf4532e19
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/07/2020
ms.locfileid: "96752216"
---
# <a name="dependency-analysis"></a>Análisis de dependencias

En este artículo se describe el análisis de dependencias en Azure Migrate Server Assessment.


El análisis de dependencias identifica las dependencias entre las máquinas locales detectadas. Proporciona estas ventajas: 

- Puede recopilar máquinas en grupos para su evaluación de manera más precisa y con mayor confianza.
- Puede identificar las máquinas que se deben migrar juntas. Esto resulta especialmente útil si no está seguro de qué máquinas forman parte de la implementación de una aplicación que quiere migrar a Azure.
- Puede identificar si las máquinas están en uso y cuáles se pueden retirar en lugar de migrarlas.
- El análisis de dependencias le ayuda a garantizar que no se olvida de nada y a evitar interrupciones inesperadas después de la migración.
- [Revise](common-questions-discovery-assessment.md#what-is-dependency-visualization) las preguntas habituales sobre el análisis de dependencias.


## <a name="analysis-types"></a>Tipos de análisis

Hay dos opciones para implementar el análisis de dependencias:

**Opción** | **Detalles** | **Nube pública** | **Azure Government**
----  |---- | ---- 
**Sin agente** | Sondea los datos de las máquinas virtuales de VMware mediante las API de vSphere.<br/><br/> No es necesario instalar agentes en las máquinas virtuales.<br/><br/> Esta opción se encuentra actualmente en versión preliminar y solo para máquinas virtuales de VMware. | Compatible. | Compatible.
**Análisis basado en agente** | Usa la [solución Service Map](../azure-monitor/insights/service-map.md) de Azure Monitor para habilitar la visualización y el análisis de dependencias.<br/><br/> Tiene que instalar agentes en cada máquina local que desee analizar. | Compatible | No compatible.


## <a name="agentless-analysis"></a>Análisis sin agente

El análisis de dependencias sin agente funciona mediante la captura de los datos de conexión TCP de las máquinas en las que se ha habilitado. No hay ningún agente instalado en las máquinas virtuales. Las conexiones con el mismo servidor y proceso de origen, y con el servidor, proceso y puerto de destino se agrupan lógicamente en una dependencia. Puede visualizar los datos de dependencia capturados en una vista de mapa o exportarlos como CSV. No hay ningún agente instalado en las máquinas que quiera analizar.

### <a name="dependency-data"></a>Datos de dependencia

Una vez que se inicia la detección de datos de dependencia, comienza el sondeo:

- El dispositivo de Azure Migrate sondea los datos de conexión TCP de las máquinas cada cinco minutos para recopilar datos.
- Los datos se recopilan de las máquinas virtuales invitadas a través de vCenter Server, con las API de vSphere.
- El sondeo recopila estos datos:

    - Nombre de los procesos que tienen conexiones activas.
    - Nombre de la aplicación que ejecuta los procesos que tienen conexiones activas.
    - Puerto de destino en las conexiones activas.

- Los datos recopilados se procesan en el dispositivo de Azure Migrate para deducir la información de identidad, y se envían a Azure Migrate cada seis horas.


## <a name="agent-based-analysis"></a>Análisis basado en agente

En el análisis basado en agente, Server Assessment emplea la [solución Service Map](../azure-monitor/insights/service-map.md) de Azure Monitor. Puede instalar [Microsoft Monitoring Agent o el agente de Log Analytics](../azure-monitor/platform/agents-overview.md#log-analytics-agent) y el [agente de dependencias](../azure-monitor/platform/agents-overview.md#dependency-agent) en cada máquina que quiera analizar.

### <a name="dependency-data"></a>Datos de dependencia

El análisis basado en agente proporciona estos datos:

- Nombre de aplicación, proceso y nombre del servidor de la máquina de origen.
- Puerto, nombre de aplicación, proceso y nombre del servidor de la máquina de destino.
- Se recopila la información sobre el número de conexiones, la latencia y la transferencia de datos, y está disponible para las consultas de Log Analytics. 



## <a name="compare-agentless-and-agent-based"></a>Comparación basada en agente y sin agente

La diferencia entre la visualización sin agente y la visualización basada en agente se resume en la siguiente tabla.

**Requisito** | **Sin agente** | **Basado en agente**
--- | --- | ---
**Soporte técnico** | Solo en versión preliminar para máquinas virtuales de VMware. [Revise](migrate-support-matrix-vmware.md#dependency-analysis-requirements-agentless) los sistemas operativos compatibles. | En disponibilidad general (GA).
**Agent** | No se requiere ningún agente en las máquinas que desea analizar. | Se requieren agentes en cada máquina local que quiera analizar.
**Log Analytics** | No se requiere. | Azure Migrate usa la solución [Service Map](../azure-monitor/insights/service-map.md) de los [registros de Azure Monitor](../azure-monitor/log-query/log-query-overview.md) para el análisis de dependencias.<br/><br/> Puede asociar un área de trabajo de Log Analytics con un proyecto de Azure Migrate. El área de trabajo debe residir en las regiones Este de EE. UU., Sudeste Asiático u Oeste de Europa. El área de trabajo debe estar en una región en la que [se admita Service Map](../azure-monitor/insights/vminsights-configure-workspace.md#supported-regions).
**Proceso** | Captura datos de conexión TCP. Después de la detección, recopila datos en intervalos de cinco minutos. | Los agentes de Service Map instalados en una máquina recopilan datos acerca de los procesos de TCP, así como de las conexiones de entrada o salida de cada proceso.
**Data** | Nombre de aplicación, proceso y nombre del servidor de la máquina de origen.<br/><br/> Puerto, nombre de aplicación, proceso y nombre del servidor de la máquina de destino. | Nombre de aplicación, proceso y nombre del servidor de la máquina de origen.<br/><br/> Puerto, nombre de aplicación, proceso y nombre del servidor de la máquina de destino.<br/><br/> Se recopila la información sobre el número de conexiones, la latencia y la transferencia de datos, y está disponible para las consultas de Log Analytics. 
**Visualización** | El mapa de dependencias de un solo servidor se puede ver durante un plazo de entre una hora y 30 días. | Mapa de dependencia de un solo servidor.<br/><br/> Mapa de dependencias de un grupo de servidores.<br/><br/>  El mapa puede verse solo durante una hora.<br/><br/> Agregue y quite servidores de un grupo desde la vista de mapa.
Exportación de datos | Los datos de los últimos 30 días se pueden descargar en formato CSV. | Los datos se pueden consultar con Log Analytics.



## <a name="next-steps"></a>Pasos siguientes

- [Configure](how-to-create-group-machine-dependencies.md) la visualización de dependencias basada en agente.
- [Pruebe](how-to-create-group-machine-dependencies-agentless.md) la visualización de dependencias sin agente para máquinas virtuales de VMware.
- Revise las [preguntas habituales](common-questions-discovery-assessment.md#what-is-dependency-visualization) sobre la visualización de dependencias.

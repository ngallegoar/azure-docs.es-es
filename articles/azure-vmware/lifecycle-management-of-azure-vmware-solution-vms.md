---
title: Administración del ciclo de vida de las máquinas virtuales de Azure VMware Solution
description: Aprenda a administrar todos los aspectos del ciclo de vida de las máquinas virtuales de Azure VMware Solution con herramientas nativas de Microsoft Azure.
ms.topic: conceptual
ms.date: 09/11/2020
ms.openlocfilehash: 928a632a34dd31272c7c3bf92f6dc6dda97cb6cc
ms.sourcegitcommit: 8d8deb9a406165de5050522681b782fb2917762d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/20/2020
ms.locfileid: "92216256"
---
# <a name="lifecycle-management-of-azure-vmware-solution-vms"></a>Administración del ciclo de vida de las máquinas virtuales de Azure VMware Solution

Las herramientas nativas de Microsoft Azure le permiten supervisar y administrar las máquinas virtuales (VM) en el entorno de Azure. Sin embargo, también le permiten supervisar y administrar las máquinas virtuales en Azure VMware Solution y las máquinas virtuales locales. En esta información general, veremos la arquitectura de supervisión integrada que ofrece Azure y cómo puede usar sus herramientas nativas para administrar las máquinas virtuales de Azure VMware Solution a lo largo de su ciclo de vida.

## <a name="benefits"></a>Ventajas

- Los servicios nativos de Azure se pueden usar para administrar las máquinas virtuales en un entorno híbrido (Azure, Azure VMware Solution y local).
- Supervisión y visibilidad integradas de las máquinas virtuales de Azure, Azure VMware Solution y locales.
- Con Azure Update Management en Azure Automation, puede administrar las actualizaciones del sistema operativo de las máquinas Windows y Linux. 
- Azure Security Center proporciona Advanced Threat Protection, incluyendo lo siguiente:
    - Supervisión de la integridad de los archivos
    - Alertas de seguridad sin archivos
    - Evaluación de revisiones del sistema operativo
    - Evaluación de configuraciones de seguridad incorrectas
    - Evaluación de EndPoint Protection 
- Implemente fácilmente Microsoft Monitoring Agent (MMA) mediante Azure ARC para nuevas máquinas virtuales. 
- El área de trabajo de Log Analytics en Azure Monitor habilita la recopilación de registros y la recopilación de contadores de rendimiento mediante MMA o extensiones. Recopile datos y registros en un único punto y presente dichos datos en diferentes servicios nativos de Azure. 
- Entre las ventajas agregadas de Azure Monitor se incluyen: 
    - Supervisión fluida 
    - Mejor visibilidad de la infraestructura 
    - Notificaciones instantáneas 
    - Resolución automática 
    - Rentabilidad 

## <a name="integrated-azure-monitoring-architecture"></a>Arquitectura de supervisión de Azure integrada

En el siguiente diagrama se muestra la arquitectura de supervisión integrada de las máquinas virtuales de Azure VMware Solution.

![Arquitectura de supervisión de Azure integrada](media/lifecycle-management-azure-vmware-solutions-virtual-machines/integrated-azure-monitoring-architecture.png)

## <a name="integrating-and-deploying-azure-native-services"></a>Integración e implementación de servicios nativos de Azure

**Azure ARC** amplía la administración de Azure a cualquier infraestructura, incluidos Azure VMware Solution, el entorno local u otras plataformas en la nube. Azure ARC se puede implementar instalando un clúster de Azure Kubernetes Service (AKS) en el entorno de Azure VMware Solution. Para más información, consulte [Conexión de un clúster de Kubernetes habilitado para Azure Arc](../azure-arc/kubernetes/connect-cluster.md).

Las máquinas virtuales de Azure VMware Solution se pueden supervisar a través de MMA (también se conoce como agente de Log Analytics o agente de Linux de OMS). MMA se puede instalar automáticamente cuando se aprovisionan máquinas virtuales a través de flujos de trabajo del ciclo de vida de máquina virtual de ARC. MMA también se puede instalar al implementar máquinas virtuales desde una plantilla en vCenter; de nuevo, con máquinas virtuales aprovisionadas a través de flujos de trabajo de ARC. Todas las máquinas virtuales de Azure VMware Solution aprovisionadas pueden tener MMA instalado y enviar registros al área de trabajo de Azure Log Analytics. Para obtener más información, consulte [Introducción al agente de Log Analytics](../azure-monitor/platform/log-analytics-agent.md).

El **área de trabajo de Log Analytics** habilita la recopilación de registros y la recopilación de contadores de rendimiento mediante MMA o extensiones. Para crear el área de trabajo de Log Analytics, consulte [Creación de un área de trabajo de Log Analytics en Azure Portal](../azure-monitor/learn/quick-create-workspace.md).
- Para agregar máquinas virtuales Windows al área de trabajo de Log Analytics, consulte [Instalación del agente de Log Analytics en equipos Windows](../azure-monitor/platform/agent-windows.md).
- Para agregar máquinas virtuales Linux al área de trabajo de Log Analytics, consulte [Instalación del agente de Log Analytics en equipos Linux](../azure-monitor/platform/agent-linux.md).

**Azure Update Management** en Azure Automation administra las actualizaciones del sistema operativo de las máquinas Windows y Linux en un entorno híbrido. Supervisa el cumplimiento de la aplicación de revisiones y reenvía alertas de desviación de la aplicación de revisiones a Azure Monitor para su corrección. Azure Update Management debe conectarse al área de trabajo de Log Analytics para usar datos almacenados con el fin de evaluar el estado de las actualizaciones en las máquinas virtuales.
- Para agregar Log Analytics a Azure Update Management, primero debe [crear una cuenta de Azure Automation](../automation/automation-create-standalone-account.md).
- Para vincular el área de trabajo de Log Analytics con la cuenta de Automation, consulte [Área de trabajo de Log Analytics y cuenta de Automation](../azure-monitor/insights/solutions.md#log-analytics-workspace-and-automation-account).
- Para habilitar Azure Update Management para las máquinas virtuales, consulte [Habilitación de Update Management desde una cuenta de Automation](../automation/update-management/enable-from-automation-account.md).
- Una vez que haya agregado máquinas virtuales a Azure Update Management, puede [implementar actualizaciones en máquinas virtuales y revisar los resultados](../automation/update-management/deploy-updates.md). 

**Azure Security Center** proporciona Advanced Threat Protection en todas las cargas de trabajo híbridas de la nube y del entorno local. Evaluará la vulnerabilidad de las máquinas virtuales de Azure VMware Solution y generará alertas según sea necesario. Estas alertas de seguridad se pueden reenviar a Azure Monitor para su resolución.
- Azure Security Center no requiere implementación. Para obtener más información, consulte una lista de [características admitidas para máquinas virtuales](../security-center/security-center-services.md).
- Para agregar máquinas virtuales de Azure VMware Solution y máquinas virtuales que no son de Azure a Azure Security Center, consulte [Incorporación de equipos Windows a Azure Security Center](../security-center/quickstart-onboard-machines.md) e [Incorporación de equipos Linux a Azure Security Center](../security-center/quickstart-onboard-machines.md).
- Tras la incorporación de máquinas virtuales, Azure Security Center analiza el estado de seguridad de los recursos para identificar posibles vulnerabilidades. También proporciona recomendaciones en la pestaña Información general. Para más información, consulte [Recomendaciones de seguridad de Azure Security Center](../security-center/security-center-recommendations.md).
- Puede definir las directivas de seguridad en Azure Security Center. Para obtener información sobre cómo configurar las directivas de seguridad, consulte [Uso de directivas de seguridad](../security-center/tutorial-security-policy.md).

**Azure Monitor** es una solución completa para recopilar, analizar y actuar en la telemetría desde los entornos local y en la nube. No requiere ninguna implementación.
- Azure Monitor le permite recopilar datos de diferentes orígenes para su supervisión y análisis. Para obtener más información, consulte [Orígenes de datos de supervisión para Azure Monitor](../azure-monitor/platform/data-sources.md).
- También puede recopilar diferentes tipos de datos para su análisis, visualización y alertas. Para más información, consulte [Plataforma de datos de Azure Monitor](../azure-monitor/platform/data-platform.md).
- Para configurar Azure Monitor con el área de trabajo de Log Analytics, consulte [Configuración del área de trabajo de Log Analytics para Azure Monitor para VM](../azure-monitor/insights/vminsights-configure-workspace.md).
- Puede crear reglas de alerta para identificar problemas en su entorno, como, por ejemplo, uso elevado de recursos, revisiones que faltan, espacio en disco bajo y latido de las máquinas virtuales. También puede establecer una respuesta automatizada a eventos detectados enviando una alerta a las herramientas de Administración de servicios de TI (ITSM). La notificación de detección de alertas también se puede enviar por correo electrónico. Para crear dichas reglas, consulte:
    - [Creación, visualización y administración de alertas de métricas mediante Azure Monitor](../azure-monitor/platform/alerts-metric.md).
    - [Creación, visualización y administración de alertas de registro mediante Azure Monitor](../azure-monitor/platform/alerts-log.md).
    - [Reglas de acción](../azure-monitor/platform/alerts-action-rules.md) para establecer acciones y notificaciones automatizadas.
    - [Conexión de Azure a las herramientas de ITSM mediante el Conector de Administración de servicios de TI](../azure-monitor/platform/itsmc-overview.md).

La **plataforma como servicio (PaaS) de Azure** es un entorno de desarrollo e implementación en la nube, con recursos que le permiten proporcionar aplicaciones basadas en la nube. Por ejemplo, puede integrar Azure SQL Database con las máquinas virtuales de Azure VMware Solution. Las alertas de SQL se pueden correlacionar con las alertas de máquina virtual de Azure VMware Solution. Por ejemplo, supongamos que el segmento de SQL Database de la aplicación está dentro de PAAS de Azure y la capa de aplicación web de la misma aplicación se hospeda en las máquinas virtuales de Azure VMware Solution. Las alertas de base de datos se pueden correlacionar con alertas de aplicación web. La solución de problemas se ha simplificado con una visibilidad única e integrada de máquinas virtuales de Azure, Azure VMware Solution y locales.
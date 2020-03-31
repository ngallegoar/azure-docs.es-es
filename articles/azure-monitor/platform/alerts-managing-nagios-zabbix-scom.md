---
title: Administración de alertas de System Center Operations Manager, Zabbix y Nagios en Azure Monitor
description: Administración de alertas de System Center Operations Manager, Zabbix y Nagios en Azure Monitor
ms.topic: conceptual
ms.date: 09/24/2018
ms.subservice: alerts
ms.openlocfilehash: 5ef03f522fb91302bfdec985f1e3c5cf8138ae97
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "77667455"
---
# <a name="manage-alerts-from-system-center-operations-manager-zabbix-and-nagios-in-azure-monitor"></a>Administración de alertas de System Center Operations Manager, Zabbix y Nagios en Azure Monitor

Ahora puede ver las alertas de Nagios, Zabbix y System Center Operations Manager en [Azure Monitor](https://aka.ms/azure-alerts-overview). Estas alertas proceden de integraciones con servidores Nagios o Zabbix o con System Center Operations Manager en Log Analytics. 

## <a name="prerequisites"></a>Prerrequisitos
Los registros del repositorio de Log Analytics con un tipo de alerta se importarán a Azure Monitor, por lo que debe realizar las configuraciones que sean necesarias para recopilar estos registros.
1. Para las alertas de **Nagios** y **Zabbix**, [configure esos servidores](https://docs.microsoft.com/azure/log-analytics/log-analytics-linux-agents) para que [envíen alertas](https://docs.microsoft.com/azure/azure-monitor/platform/data-sources-alerts-nagios-zabbix?toc=%2Fazure%2Fazure-monitor%2Ftoc.json) a Log Analytics.
1. Para las alertas de **System Center Operations Manager**,[conecte el grupo de administración de Operations Manager al área de trabajo de Log Analytics](https://docs.microsoft.com/azure/log-analytics/log-analytics-om-agents). A continuación, implemente la solución [Alert Management](https://docs.microsoft.com/azure/azure-monitor/platform/alert-management-solution) desde Azure Marketplace. Una vez hecho esto, las alertas creadas en System Center Operations Manager se importan en Log Analytics.

## <a name="view-your-alert-instances"></a>Visualización de las instancias de alertas
Una vez que haya configurado la importación en Log Analytics, puede empezar a ver instancias de alertas de estos servicios de supervisión en [Azure Monitor](https://aka.ms/azure-alerts-overview). Una vez que están presentes en Azure Monitor, podrá [administrar las instancias de alertas](https://aka.ms/managing-alert-instances), [administrar los grupos inteligentes creados en estas alertas](https://aka.ms/managing-smart-groups) y [cambiar el estado de las alertas y de los grupos inteligentes](https://aka.ms/managing-alert-smart-group-states).

> [!NOTE]
>  1. Esta solución solo le permite ver instancias de alertas desencadenadas por System Center Operations Manager, Zabbix y Nagios en Azure Monitor. La configuración de la regla de alertas solo se puede ver y editar en las correspondientes herramientas de supervisión. 
>  1. Todas las instancias de alertas desencadenadas estarán disponibles en Azure Monitor y en Azure Log Analytics. En la actualidad, no hay manera de elegir entre los dos o de ingerir solo alertas desencadenadas específicas.
>  1. Todas las alertas de System Center Operations Manager, Zabbix y Nagios tienen el tipo de señal "Desconocido", ya que el tipo de datos de telemetría subyacente no está disponible.
>  1. Las alertas de Nagios no tienen estado; por ejemplo, la [condición de supervisión](https://aka.ms/azure-alerts-overview) de una alerta no pasará de "Activada" a "Resuelta". En su lugar, "Activada" y "Resuelta" se muestran como instancias independientes de la alerta. 


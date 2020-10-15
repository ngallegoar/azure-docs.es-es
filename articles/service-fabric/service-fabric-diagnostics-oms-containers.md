---
title: Supervisión de contenedores con los registros de Azure Monitor
description: Use los registros de Azure Monitor para supervisar los contenedores que se ejecutan en clústeres de Azure Service Fabric.
author: srrengar
ms.topic: conceptual
ms.date: 02/25/2019
ms.author: srrengar
ms.openlocfilehash: c1d118fa1fb65663f37e33f739bf4a72a14e1c52
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "86258701"
---
# <a name="monitor-containers-with-azure-monitor-logs"></a>Supervisión de contenedores con los registros de Azure Monitor
 
En este artículo se describen los pasos necesarios para configurar la solución de supervisión de contenedores de registros de Azure Monitor para ver los eventos de contenedor. Para configurar el clúster a fin de recopilar eventos de contenedor, consulte este [tutorial paso a paso](service-fabric-tutorial-monitoring-wincontainers.md). 

[!INCLUDE [log-analytics-agent-note.md](../../includes/log-analytics-agent-note.md)]

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../includes/azure-monitor-log-analytics-rebrand.md)]

## <a name="set-up-the-container-monitoring-solution"></a>Configuración de la solución de supervisión de contenedores

> [!NOTE]
> Los registros de Azure Monitor deben estar configurados en el clúster y el agente de Log Analytics implementado en los nodos. De lo contrario, siga en primer lugar los pasos descritos en [Set up Azure Monitor logs](service-fabric-diagnostics-oms-setup.md) (Configuración de los registros de Azure Monitor) y [Add the Log Analytics agent to a cluster](service-fabric-diagnostics-oms-agent.md) (Incorporación del agente de Log Analytics a un clúster).

1. Una vez que el clúster esté configurado con los registros de Azure Monitor y el agente de Log Analytics, implemente los contenedores. Espere a que los contenedores se implementen antes de pasar al paso siguiente.

2. En Azure Marketplace, busque *Container Monitoring Solution* y haga clic en el recurso **Container Monitoring Solution** que aparece debajo de la categoría Supervisión y administración.

    ![Adición de la solución Containers](./media/service-fabric-diagnostics-event-analysis-oms/containers-solution.png)

3. Cree la solución dentro de la misma área de trabajo que ya se ha creado para el clúster. Este cambio desencadena automáticamente el agente para iniciar la recopilación de datos de Docker en los contenedores. En unos 15 minutos, debería ver la solución iluminada con registros y estadísticas entrantes, como se muestra en la imagen siguiente.

    ![Panel básico de Log Analytics](./media/service-fabric-diagnostics-event-analysis-oms/oms-containers-dashboard.png)

El agente habilita la recopilación de varios registros específicos de contenedores que se pueden consultar en los registros de Azure Monitor o se usan para visualizar indicadores de rendimiento. Los tipos de registro que se recopilan son:

* ContainerInventory: muestra información acerca de la ubicación, nombre e imágenes del contenedor
* ContainerImageInventory: información acerca de las imágenes implementadas, lo que incluye sus identificadores o tamaños
* ContainerLog: registros de error concretos, registros de docker (stdout, etc.) y otras entradas
* ContainerServiceLog: comandos de demonio de docker que se han ejecutado
* Perf: contadores de rendimiento, entre los que se incluyen la cpu del contenedor, la memoria, el tráfico de red, la e/s de disco y métricas personalizadas de los equipos host



## <a name="next-steps"></a>Pasos siguientes
* Obtenga más información sobre la [solución de contenedores de registros de Azure Monitor](../azure-monitor/insights/containers.md).
* Obtenga más información sobre la orquestación de contenedores en Service Fabric en [Service Fabric y contenedores](service-fabric-containers-overview.md)
* Familiarícese con las características de [consultas y búsqueda de registros](../azure-monitor/log-query/log-query-overview.md) que se ofrecen como parte de los registros de Azure Monitor.
* Configure los registros de Azure Monitor para configurar [alertas automáticas](../azure-monitor/platform/alerts-overview.md) que ayuden a la detección y el diagnóstico.

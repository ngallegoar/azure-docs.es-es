---
title: Análisis de eventos de Azure Service Fabric con registros de Azure Monitor
description: Aprenda a visualizar y analizar eventos con los registros de Azure Monitor para la supervisión y el diagnóstico de clústeres de Azure Service Fabric.
author: srrengar
ms.topic: conceptual
ms.date: 02/21/2019
ms.author: srrengar
ms.openlocfilehash: 40dd930aa21e3056d5ecc908359215d6874ed8ae
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "75464741"
---
# <a name="event-analysis-and-visualization-with-azure-monitor-logs"></a>Análisis y visualización de eventos con los registros de Azure Monitor
 Los registros de Azure Monitor recopilan y analizan la telemetría de las aplicaciones y los servicios hospedados en la nube y proporcionan herramientas de análisis para ayudarle a maximizar su disponibilidad y rendimiento. En este artículo se describe cómo ejecutar consultas en los registros de Azure Monitor para obtener información de lo que está sucediendo en el clúster y solucionar problemas. Se tratan las siguientes preguntas habituales:

* ¿Cómo se solucionan los eventos de mantenimiento?
* ¿Cómo se puede saber si un nodo deja de funcionar?
* ¿Cómo se puede saber si los servicios de la aplicación se han iniciado o detenido?

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../includes/azure-monitor-log-analytics-rebrand.md)]

## <a name="overview-of-the-log-analytics-workspace"></a>Introducción al área de trabajo de Log Analytics

>[!NOTE] 
>Mientras que el almacenamiento de diagnóstico está habilitado de forma predeterminada en el momento de creación del clúster, debe configurar el área de trabajo de Log Analytics para que lea desde el almacenamiento de diagnósticos.

Los registros de Azure Monitor recopilan datos de recursos administrados, incluidos un agente o una tabla de almacenamiento de Azure, y los mantienen en un repositorio central. Estos datos pueden utilizarse posteriormente para análisis, alertas, visualizaciones y tareas ulteriores de exportación. Los registros de Azure Monitor admiten eventos, datos de rendimiento u otros datos personalizados. Consulte los [pasos para configurar la extensión de diagnósticos para agregar eventos](service-fabric-diagnostics-event-aggregation-wad.md) y los [pasos para crear un área de trabajo de Log Analytics para leer de los eventos de almacenamiento](service-fabric-diagnostics-oms-setup.md) con el fin de asegurarse de que los datos fluyen hacia los registros de Azure Monitor.

Una vez que los registros de Azure Monitor han recibido los datos, Azure dispone de varias *soluciones de supervisión*; son soluciones preempaquetadas o paneles operativos para supervisar los datos de entrada, personalizadas para varios escenarios. Puede tratarse de una solución de *Service Fabric Analytics* y una solución de *Containers*, que son las dos opciones más importantes para diagnosticar y supervisar el uso de los clústeres de Service Fabric. En este artículo se describe cómo utilizar la solución de Service Fabric Analytics, que se crea con el área de trabajo.

## <a name="access-the-service-fabric-analytics-solution"></a>Acceso a la solución de Service Fabric Analytics

En [Azure Portal](https://portal.azure.com), vaya al grupo de recursos donde creó la solución Service Fabric Analytics.

Seleccione el recurso **ServiceFabric\<nameOfOMSWorkspace\>** .

En `Summary`, verá iconos en forma de grafo para cada una de las soluciones habilitadas, entre ellos uno para Service Fabric. Haga clic en el grafo de **Service Fabric** para ir a la solución Service Fabric Analytics.

![Solución Service Fabric](media/service-fabric-diagnostics-event-analysis-oms/oms_service_fabric_summary.PNG)

En la siguiente imagen se muestra la página principal de la solución Service Fabric Analytics. Esta página principal proporciona una instantánea de lo que sucede en el clúster.

![Solución Service Fabric](media/service-fabric-diagnostics-event-analysis-oms/oms_service_fabric_solution.PNG)

 Si habilitó el diagnóstico durante la creación del clúster, puede ver eventos de 

* [Eventos de clúster de Service Fabric](service-fabric-diagnostics-event-generation-operational.md)
* [Eventos del modelo de programación de Reliable Actors](service-fabric-reliable-actors-diagnostics.md)
* [Eventos del modelo de programación de Reliable Services](service-fabric-reliable-services-diagnostics.md)

>[!NOTE]
>Además de los eventos de Service Fabric estándar, se pueden recopilar eventos del sistema más detallados mediante la [actualización de la configuración de la extensión de diagnósticos](service-fabric-diagnostics-event-aggregation-wad.md#log-collection-configurations).

## <a name="view-service-fabric-events-including-actions-on-nodes"></a>Visualización de eventos de Service Fabric, como acciones en nodos

En la página de Service Fabric Analytics, haga clic en el grafo de **eventos de Service Fabric**.

![Canal operativo de la solución Service Fabric](media/service-fabric-diagnostics-event-analysis-oms/oms_service_fabric_events_selection.png)

Haga clic en **Lista** para ver los eventos en una lista. Una vez aquí, verá todos los eventos del sistema que se han recopilado. Como referencia, proceden de **WADServiceFabricSystemEventsTable** en la cuenta de Azure Storage y, de manera similar, los eventos de Reliable Services y Reliable Actors que ve a continuación provienen de esas tablas respectivas.
    
![Canal operativo de consulta](media/service-fabric-diagnostics-event-analysis-oms/oms_service_fabric_events.png)

También puede hacer clic en la lupa de la izquierda y usar el lenguaje de consulta Kusto para encontrar lo está buscando. Por ejemplo, para buscar todas las acciones realizadas en los nodos del clúster, puede usar la consulta siguiente. Los identificadores de evento que se usan a continuación se encuentran en la [referencia de eventos del canal operativo](service-fabric-diagnostics-event-generation-operational.md).

```kusto
ServiceFabricOperationalEvent
| where EventId < 25627 and EventId > 25619 
```

Puede consultar en muchos más campos, como los nodos específicos (Computer), el servicio del sistema (TaskName).

## <a name="view-service-fabric-reliable-service-and-actor-events"></a>Visualización de eventos de Reliable Services y Reliable Actors

En la página de Service Fabric Analytics, haga clic en el grafo de **Reliable Services**.

![Reliable Services de la solución Service Fabric](media/service-fabric-diagnostics-event-analysis-oms/oms_reliable_services_events_selection.png)

Haga clic en **Lista** para ver los eventos en una lista. Aquí puede ver eventos de Reliable Services. Puede ver eventos diferentes para cuando el servicio runasync se inicia y se completa, lo que ocurre habitualmente en las implementaciones y las actualizaciones. 

![Reliable Services de consulta](media/service-fabric-diagnostics-event-analysis-oms/oms_reliable_service_events.png)

Los eventos de Reliable Actors pueden verse de forma similar. Para configurar más eventos detallados para Reliable Actors, necesita cambiar `scheduledTransferKeywordFilter` en la configuración de la extensión de diagnóstico (se muestra a continuación). Los detalles de los valores de estos están en la [referencia de eventos de Reliable Actors](service-fabric-reliable-actors-diagnostics.md#keywords).

```json
"EtwEventSourceProviderConfiguration": [
                {
                    "provider": "Microsoft-ServiceFabric-Actors",
                    "scheduledTransferKeywordFilter": "1",
                    "scheduledTransferPeriod": "PT5M",
                    "DefaultEvents": {
                    "eventDestination": "ServiceFabricReliableActorEventTable"
                    }
                },
```

El lenguaje de consulta Kusto es eficaz. Otra consulta valiosa que puede ejecutar consiste en averiguar qué nodos están generando la mayoría de los eventos. La consulta de la captura de pantalla siguiente muestra eventos operativos de Service Fabric con el servicio y el nodo específicos.

![Eventos de consulta por nodo](media/service-fabric-diagnostics-event-analysis-oms/oms_kusto_query.png)

## <a name="next-steps"></a>Pasos siguientes

* Para habilitar la supervisión de la infraestructura, es decir, los contadores de rendimiento, vea cómo [agregar el agente de Log Analytics](service-fabric-diagnostics-oms-agent.md). El agente recopila los contadores de rendimiento y los agrega al área de trabajo existente.
* Si se trata de clústeres locales, los registros de Azure Monitor ofrecen una puerta de enlace (proxy de reenvío HTTP) que puede usarse para enviar datos a estos registros. Para más información, vea [Conexión de equipos sin acceso a Internet a los registros de Azure Monitor mediante la puerta de enlace de Log Analytics](../azure-monitor/platform/gateway.md).
* Configure [alertas automáticas](../log-analytics/log-analytics-alerts.md) para facilitar la detección y el diagnóstico.
* Familiarícese con las característica de [búsqueda de registros y consulta](../log-analytics/log-analytics-log-searches.md) que se ofrecen como parte de los registros de Azure Monitor.
* Lea en [¿Qué son los registros de Azure Monitor?](../operations-management-suite/operations-management-suite-overview.md) una introducción más detallada de los registros de Azure Monitor y conozca qué ofrecen.

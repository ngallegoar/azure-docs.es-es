---
title: Supervisión del rendimiento con registros de Azure Monitor
description: Obtenga información sobre cómo configurar el Agente de Log Analytics para supervisar los contenedores y los contadores de rendimiento de los clústeres de Azure Service Fabric.
author: srrengar
ms.topic: conceptual
ms.date: 04/16/2018
ms.author: srrengar
ms.openlocfilehash: b91a8a8742a5bdc9454ebcbd8894889084a12a79
ms.sourcegitcommit: dabd9eb9925308d3c2404c3957e5c921408089da
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/11/2020
ms.locfileid: "86258684"
---
# <a name="performance-monitoring-with-azure-monitor-logs"></a>Supervisión del rendimiento con registros de Azure Monitor

En este artículo se describen los pasos para agregar el Agente de Log Analytics como una extensión del conjunto de escalado de máquinas virtuales al clúster y conectarlo al área de trabajo de Azure Log Analytics existente. Esto permite recopilar datos de diagnóstico sobre los contenedores, las aplicaciones y la supervisión de rendimiento. Si lo agrega como una extensión al recurso del conjunto de escalado de máquinas virtuales, Azure Resource Manager garantiza la instalación en todos los nodos, incluso cuando se ajusta la escala del clúster.

> [!NOTE]
> En este artículo se da por supuesto que tiene un área de trabajo de Azure Log Analytics que ya ha configurado. De lo contrario, vea [Configuración de registros de Azure Monitor](service-fabric-diagnostics-oms-setup.md).

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../includes/azure-monitor-log-analytics-rebrand.md)]

## <a name="add-the-agent-extension-via-azure-cli"></a>Incorporación de la extensión del Agente mediante la CLI de Azure

La mejor manera de agregar el agente de Log Analytics al clúster es a través de las API del conjunto de escalado de máquinas virtuales disponibles con la CLI de Azure. Si no tiene la CLI de Azure configurada aún, diríjase a Azure Portal y abra una instancia de [Cloud Shell](../cloud-shell/overview.md) o [instale la CLI de Azure](/cli/azure/install-azure-cli).

1. Tras solicitar Cloud Shell, asegúrese de que trabaja en la misma suscripción que el recurso. Compruébelo con `az account show` y asegúrese de que el valor "nombre" coincide con el de la suscripción del clúster.

2. En Azure Portal, desplácese hasta el grupo de recursos donde se encuentra el área de trabajo de Log Analytics. Haga clic en el recurso de Log Analytics (el tipo del recurso será área de trabajo de Log Analytics). Una vez que esté en la página de información general de recursos, haga clic en **Configuración avanzada** en la sección Configuración del menú izquierdo.

    ![Página de propiedades de Log Analytics](media/service-fabric-diagnostics-oms-agent/oms-advanced-settings.png)

3. Haga clic en **Servidores Windows** si va a configurar un clúster con Windows y **Servidores Linux** si va a crear un clúster con Linux. Esta página le mostrará `workspace ID` y `workspace key` (que se muestran como Clave principal en el portal). Los necesitará para el siguiente paso.

4. Ejecute el comando para instalar el agente de Log Analytics en el clúster, usando la API `vmss extension set`:

    En un clúster con Windows:

    ```azurecli
    az vmss extension set --name MicrosoftMonitoringAgent --publisher Microsoft.EnterpriseCloud.Monitoring --resource-group <nameOfResourceGroup> --vmss-name <nameOfNodeType> --settings "{'workspaceId':'<Log AnalyticsworkspaceId>'}" --protected-settings "{'workspaceKey':'<Log AnalyticsworkspaceKey>'}"
    ```

    En un clúster con Linux:

    ```azurecli
    az vmss extension set --name OmsAgentForLinux --publisher Microsoft.EnterpriseCloud.Monitoring --resource-group <nameOfResourceGroup> --vmss-name <nameOfNodeType> --settings "{'workspaceId':'<Log AnalyticsworkspaceId>'}" --protected-settings "{'workspaceKey':'<Log AnalyticsworkspaceKey>'}"
    ```

    Este es un ejemplo de incorporación del agente de Log Analytics a un clúster con Windows.

    ![Comando de la CLI del agente de Log Analytics](media/service-fabric-diagnostics-oms-agent/cli-command.png)

5. La incorporación satisfactoria del Agente a los nodos debería durar menos de 15 minutos. Puede verificar que los agentes se han agregado mediante el uso de la API `az vmss extension list`:

    ```azurecli
    az vmss extension list --resource-group <nameOfResourceGroup> --vmss-name <nameOfNodeType>
    ```

## <a name="add-the-agent-via-the-resource-manager-template"></a>Incorporación del Agente a través de la plantilla de Resource Manager

Las plantillas de ejemplo de Resource Manager que implementan un área de trabajo de Azure Log Analytics e incorporan un agente a cada uno de los nodos se encuentran disponibles para [Windows](https://github.com/Azure-Samples/service-fabric-cluster-templates/tree/master/5-VM-Windows-OMS-UnSecure) o [Linux](https://github.com/Azure-Samples/service-fabric-cluster-templates/tree/master/5-VM-Ubuntu-1-NodeType-Secure-OMS).

Puede descargar y modificar esta plantilla para implementar un clúster que satisfaga mejor sus necesidades.

## <a name="view-performance-counters"></a>Ver contadores de rendimiento

Ahora que ha agregado el agente de Log Analytics, vaya al portal de Log Analytics para elegir los contadores de rendimiento que desea recopilar.

1. En Azure Portal, vaya al grupo de recursos donde creó la solución Service Fabric Analytics. Seleccione **ServiceFabric\<nameOfLog AnalyticsWorkspace\>** .

2. Haga clic en **Log Analytics**.

3. Haga clic en **Configuración avanzada**.

4. Haga clic en **Datos** y luego en **Contadores de rendimiento de Windows o Linux**. Existe una lista de contenedores predeterminados que puede elegir para habilitar, y también puede establecer el intervalo para la recopilación. También puede agregar [contadores de rendimiento adicionales](service-fabric-diagnostics-event-generation-perf.md) para recopilar. En este [artículo](/windows/win32/perfctrs/specifying-a-counter-path) puede comprobar el formato adecuado.

5. Haga clic en **Guardar** y luego en **Aceptar**.

6. Cierre la hoja Configuración avanzada.

7. Bajo el encabezado General, haga clic en **Resumen del área de trabajo**.

8. Verá iconos en forma de grafo para cada una de las soluciones habilitadas, entre ellos uno para Service Fabric. Haga clic en el grafo de **Service Fabric** para ir a la solución Service Fabric Analytics.

9. Verá algunos iconos con grafos para los eventos de canal operativo y Reliable Services. La representación gráfica de los datos que fluyen para los contadores que seleccionó aparecerán bajo la métricas de nodo.

10. Haga clic en el gráfico Métrica de contenedor para ver detalles adicionales. También puede consultar los datos del contador de rendimiento de forma similar a los eventos de clúster y filtrar en los nodos, el nombre del contador de rendimiento y los valores mediante el lenguaje de consulta Kusto.

![Consulta de contador de rendimiento de Log Analytics](media/service-fabric-diagnostics-event-analysis-oms/oms_node_metrics_table.PNG)

## <a name="next-steps"></a>Pasos siguientes

* Recopile los [contadores de rendimiento](service-fabric-diagnostics-event-generation-perf.md) correspondientes. Para configurar el agente de Log Analytics para recopilar contadores de rendimiento específicos, revise [Configuración orígenes de datos](../azure-monitor/platform/agent-data-sources.md#configuring-data-sources).
* Configure los registros de Azure Monitor para configurar [alertas automáticas](../azure-monitor/platform/alerts-overview.md) que ayuden a la detección y el diagnóstico.
* Como alternativa, puede recopilar los contadores de rendimiento a través de la [extensión de Azure Diagnostics y enviarlos a Application Insights](service-fabric-diagnostics-event-aggregation-wad.md#add-the-application-insights-sink-to-the-resource-manager-template).

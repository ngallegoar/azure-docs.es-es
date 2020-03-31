---
title: Uso de los registros de Azure Monitor para supervisar clústeres de Azure HDInsight
description: Aprenda a usar los registros de Azure Monitor para supervisar trabajos que se ejecutan en un clúster de HDInsight.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 02/06/2020
ms.openlocfilehash: e4b33e132e660fba7d06ff33c7db06c7727dd26c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "77162793"
---
# <a name="use-azure-monitor-logs-to-monitor-hdinsight-clusters"></a>Uso de los registros de Azure Monitor para supervisar clústeres de HDInsight

Obtenga información sobre cómo habilitar los registros de Azure Monitor para supervisar las operaciones de clúster de Hadoop en HDInsight y cómo agregar una solución de supervisión de HDInsight.

Los [registros de Azure Monitor](../log-analytics/log-analytics-overview.md) son un servicio de Azure Monitor que supervisa los entornos local y en la nube para mantener su disponibilidad y rendimiento. Recopila los datos generados por los recursos en los entornos local y de nube y mediante otras herramientas de supervisión, para proporcionar análisis entre varios orígenes.

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../includes/azure-monitor-log-analytics-rebrand.md)]

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/) antes de empezar.

## <a name="prerequisites"></a>Prerequisites

* Un área de trabajo de Log Analytics. Considere el área de trabajo como un entorno de registros de Azure Monitor único, con su propio repositorio de datos, sus propios orígenes de datos y sus propias soluciones. Para más instrucciones, consulte [Creación de un área de trabajo de Log Analytics](../azure-monitor/learn/quick-collect-azurevm.md#create-a-workspace).

* Un clúster de HDInsight de Azure. Actualmente, puede usar los registros de Azure Monitor con los siguientes tipos de clúster de HDInsight:

  * Hadoop
  * HBase
  * Interactive Query
  * Kafka
  * Spark
  * Storm

  Para instrucciones sobre cómo crear un clúster en HDInsight, consulte el artículo de [introducción a Azure HDInsight](hadoop/apache-hadoop-linux-tutorial-get-started.md).  

* Módulo Az de Azure PowerShell.  Consulte [Presentación del nuevo módulo Az de Azure PowerShell](https://docs.microsoft.com/powershell/azure/new-azureps-module-az). Asegúrese de disponer de la versión más reciente. Si es necesario, ejecute `Update-Module -Name Az`.

> [!NOTE]  
> Se recomienda colocar el clúster de HDInsight y el área de trabajo de Log Analytics en la misma región para mejorar el rendimiento. Los registros de Azure Monitor no están disponible en todas las regiones de Azure.

## <a name="enable-azure-monitor-logs-by-using-the-portal"></a>Habilitación de los registros de Azure Monitor mediante el portal

En esta sección, configurará un clúster de HDInsight Hadoop existente para usar un área de trabajo de Azure Log Analytics para supervisar trabajos, depurar registros, etc.

1. En [Azure Portal](https://portal.azure.com/), seleccione su clúster.  Consulte [Enumeración y visualización de clústeres](./hdinsight-administer-use-portal-linux.md#showClusters) para obtener instrucciones. El clúster se abre en una nueva página del portal.

1. En el menú de la izquierda, en **Supervisión**, seleccione **Azure Monitor**.

1. En la vista principal, en **Integración de Azure Monitor**, seleccione **Habilitar**.

1. En la lista desplegable **Seleccionar un área de trabajo**, seleccione un área de trabajo de Log Analytics existente.

1. Seleccione **Guardar**.  Guardar la configuración tarda unos minutos.

    ![Habilitación de la supervisión para clústeres de HDInsight](./media/hdinsight-hadoop-oms-log-analytics-tutorial/azure-portal-monitoring.png "Habilitación de la supervisión para clústeres de HDInsight")

## <a name="enable-azure-monitor-logs-by-using-azure-powershell"></a>Habilitación de los registros de Azure Monitor mediante Azure PowerShell

Puede habilitar los registros de Azure Monitor con el cmdlet [Enable-AzHDInsightMonitoring](https://docs.microsoft.com/powershell/module/az.hdinsight/enable-azhdinsightmonitoring) del módulo Az de Azure PowerShell.

```powershell
# Enter user information
$resourceGroup = "<your-resource-group>"
$cluster = "<your-cluster>"
$LAW = "<your-Log-Analytics-workspace>"
# End of user input

# obtain workspace id for defined Log Analytics workspace
$WorkspaceId = (Get-AzOperationalInsightsWorkspace `
                    -ResourceGroupName $resourceGroup `
                    -Name $LAW).CustomerId

# obtain primary key for defined Log Analytics workspace
$PrimaryKey = (Get-AzOperationalInsightsWorkspace `
                    -ResourceGroupName $resourceGroup `
                    -Name $LAW | Get-AzOperationalInsightsWorkspaceSharedKeys).PrimarySharedKey

# Enables monitoring and relevant logs will be sent to the specified workspace.
Enable-AzHDInsightMonitoring `
    -ResourceGroupName $resourceGroup `
    -Name $cluster `
    -WorkspaceId $WorkspaceId `
    -PrimaryKey $PrimaryKey

# Gets the status of monitoring installation on the cluster.
Get-AzHDInsightMonitoring `
    -ResourceGroupName $resourceGroup `
    -Name $cluster
```

Para deshabilitarlos, use el cmdlet [Disable-AzHDInsightMonitoring](https://docs.microsoft.com/powershell/module/az.hdinsight/disable-azhdinsightmonitoring):

```powershell
Disable-AzHDInsightMonitoring -Name "<your-cluster>"
```

## <a name="install-hdinsight-cluster-management-solutions"></a>Instalación de soluciones de administración de clústeres de HDInsight

HDInsight proporciona soluciones de administración específicas de clúster que se pueden agregar a los registros de Azure Monitor. Las [soluciones de administración](../log-analytics/log-analytics-add-solutions.md) agregan funcionalidad a los registros de Azure Monitor, que proporcionan datos y herramientas de análisis adicionales. Estas soluciones recopilan importantes métricas de rendimiento de los clústeres de HDInsight y proporcionan las herramientas para buscar las métricas. También proporcionan visualizaciones y paneles para la mayoría de los tipos de clúster admitidos en HDInsight. Mediante el uso de las métricas que se recopilan con la solución, puede crear alertas y reglas de supervisión personalizadas.

Estas son las soluciones de HDInsight disponibles:

* Supervisión de HDInsight Hadoop
* Supervisión de HDInsight HBase
* Supervisión de HDInsight Interactive Query
* Supervisión de HDInsight Kafka
* Supervisión de HDInsight Spark
* Supervisión de HDInsight Storm

Para instrucciones para instalar una solución de administración, consulte [Soluciones de administración en Azure](../azure-monitor/insights/solutions.md#install-a-monitoring-solution). Para experimentar, instale una solución de supervisión de HDInsight Hadoop. Cuando haya finalizado, verá un icono **HDInsightHadoop** en **Resumen**. Seleccione el icono **HDInsightHadoop**. La solución HDInsightHadoop tiene el siguiente aspecto:

![Vista de solución de supervisión de HDInsight](media/hdinsight-hadoop-oms-log-analytics-tutorial/hdinsight-oms-hdinsight-hadoop-monitoring-solution.png)

Dado que el clúster es un clúster nuevo de marca, el informe no muestra ninguna actividad.

## <a name="configuring-performance-counters"></a>Configuración de contadores de rendimiento

Azure Monitor también admite la recopilación y el análisis de las métricas de rendimiento de los nodos del clúster. Para obtener más información sobre cómo habilitar y configurar esta característica, consulte [Orígenes de datos de rendimiento de Linux en Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/platform/data-sources-performance-counters#linux-performance-counters).

## <a name="cluster-auditing"></a>Auditoría de clústeres

HDInsight admite la auditoría de clústeres con registros de Azure Monitor, mediante la importación de los siguientes tipos de registros:

* `log_gateway_audit_CL`: esta tabla proporciona registros de auditoría de nodos de puerta de enlace de clúster que muestran intentos de inicio de sesión correctos y erróneos.
* `log_auth_CL`: esta tabla proporciona registros SSH con intentos de inicio de sesión correctos y erróneos.
* `log_ambari_audit_CL`: esta tabla proporciona registros de auditoría de Ambari.
* `log_ranger_audti_CL`: esta tabla proporciona registros de auditoría de Apache Ranger en clústeres ESP.

## <a name="next-steps"></a>Pasos siguientes

* [Consulta a los registros de Azure Monitor para supervisar clústeres de HDInsight](hdinsight-hadoop-oms-log-analytics-use-queries.md)

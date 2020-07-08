---
title: Consulta a los registros de Azure Monitor para supervisar clústeres de Azure HDInsight
description: Aprenda a ejecutar consultas en los registros de Azure Monitor para supervisar los trabajos que se ejecutan en un clúster de HDInsight.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: how-to
ms.custom: hdinsightactive
ms.date: 12/02/2019
ms.openlocfilehash: c2ddcd8ea3524b4afdfa7f70d21f0cba96975f72
ms.sourcegitcommit: 124f7f699b6a43314e63af0101cd788db995d1cb
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/08/2020
ms.locfileid: "86085368"
---
# <a name="query-azure-monitor-logs-to-monitor-hdinsight-clusters"></a>Consulta a los registros de Azure Monitor para supervisar clústeres de HDInsight

Conozca algunos escenarios básicos de uso de los registros de Azure Monitor para supervisar clústeres de Azure HDInsight:

* [Análisis de métricas de clúster de HDInsight](#analyze-hdinsight-cluster-metrics)
* [Creación de alertas de eventos](#create-alerts-for-tracking-events)

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../includes/azure-monitor-log-analytics-rebrand.md)]

## <a name="prerequisites"></a>Prerequisites

Debe haber configurado un clúster de HDInsight para usar registros de Azure Monitor y agregado las soluciones de supervisión de registros de Azure Monitor específicas del clúster de HDInsight al área de trabajo. Puede encontrar instrucciones para ello en [Uso de los registros de Azure Monitor con clústeres de HDInsight](hdinsight-hadoop-oms-log-analytics-tutorial.md).

## <a name="analyze-hdinsight-cluster-metrics"></a>Análisis de métricas de clúster de HDInsight

Aprenda a buscar métricas específicas de su clúster de HDInsight.

1. Abra el área de trabajo de Log Analytics que está asociada a su clúster de HDInsight desde Azure Portal.
1. En **General**, seleccione **Registros**.
1. Escriba la siguiente consulta en el cuadro de texto para buscar todas las métricas disponibles para todos los clústeres de HDInsight que están configurados para usar los registros de Azure Monitor y, luego, seleccione **Ejecutar**. Consulte los resultados.

    ```kusto
    search *
    ```

    ![Análisis de Apache Ambari: buscar todas las métricas](./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-search-all-metrics.png "Búsqueda en todas las métricas")

1. En el menú izquierdo, seleccione la pestaña **Filtrar**.

1. En **Tipo**, seleccione **Latido**. Después, seleccione **Aplicar y ejecutar**.

    ![métricas específicas de la búsqueda de Log Analytics](./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-search-specific-metrics.png "Búsqueda de métricas específicas")

1. Observe que la consulta del cuadro de texto cambia a:

    ```kusto
    search *
    | where Type == "Heartbeat"
    ```

1. Puede profundizar mediante las opciones disponibles en el menú izquierdo. Por ejemplo:

    - Para ver los registros de un nodo específico:

        ![Búsqueda de errores específicos output1](./media/hdinsight-hadoop-oms-log-analytics-use-queries/log-analytics-specific-node.png "Búsqueda de errores específicos output1")

    - Para ver los registros en ciertos momentos:

        ![Búsqueda de errores específicos output2](./media/hdinsight-hadoop-oms-log-analytics-use-queries/log-analytics-specific-time.png "Búsqueda de errores específicos output2")

1. Seleccione **Aplicar y ejecutar**, y revise los resultados. Tenga en cuenta también que la consulta se actualizó a:

    ```kusto
    search *
    | where Type == "Heartbeat"
    | where (Computer == "zk2-myhado") and (TimeGenerated == "2019-12-02T23:15:02.69Z" or TimeGenerated == "2019-12-02T23:15:08.07Z" or TimeGenerated == "2019-12-02T21:09:34.787Z")
    ```

### <a name="additional-sample-queries"></a>Consultas de ejemplo adicionales

Una consulta de ejemplo basada en el promedio de los recursos usados en un intervalo de 10 minutos, clasificados por nombre del clúster:

```kusto
search in (metrics_resourcemanager_queue_root_default_CL) * 
| summarize AggregatedValue = avg(UsedAMResourceMB_d) by ClusterName_s, bin(TimeGenerated, 10m)
```

En lugar de restringir en función de los recursos medios usados, puede usar la siguiente consulta para restringir los resultados según el número máximo de recursos que se usaron (así como los percentiles 90 y 95) en una ventana de 10 minutos:

```kusto
search in (metrics_resourcemanager_queue_root_default_CL) * 
| summarize ["max(UsedAMResourceMB_d)"] = max(UsedAMResourceMB_d), ["pct95(UsedAMResourceMB_d)"] = percentile(UsedAMResourceMB_d, 95), ["pct90(UsedAMResourceMB_d)"] = percentile(UsedAMResourceMB_d, 90) by ClusterName_s, bin(TimeGenerated, 10m)
```

## <a name="create-alerts-for-tracking-events"></a>Creación de alertas para el seguimiento de eventos

El primer paso para crear una alerta es llegar a una consulta en función de la cual se desencadena la alerta. Puede usar cualquier consulta que desee para crear una alerta.

1. Abra el área de trabajo de Log Analytics que está asociada a su clúster de HDInsight desde Azure Portal.
1. En **General**, seleccione **Registros**.
1. Ejecute la siguiente consulta en la que quiere crear una alerta y luego seleccione **Ejecutar**.

    ```kusto
    metrics_resourcemanager_queue_root_default_CL | where AppsFailed_d > 0
    ```

    La consulta proporciona una lista de aplicaciones con errores que se ejecutan en clústeres de HDInsight.

1. Seleccione **Nueva regla de alertas** en la parte superior de la página.

    ![Escriba la consulta para crear una alerta alert1](./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-create-alert-query.png "Escriba la consulta para crear una alerta alert1")

1. En la ventana **Crear regla**, escriba la consulta y otros detalles para crear una alerta; a continuación, haga clic en **Crear regla de alertas**.

    ![Escriba la consulta para crear una alerta alert2](./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-create-alert.png "Escriba la consulta para crear una alerta alert2")

### <a name="edit-or-delete-an-existing-alert"></a>Edición o eliminación de una alerta existente

1. Abra el área de trabajo de Log Analytics desde Azure Portal.

1. En el menú de la izquierda, en **Supervisión**, seleccione **Alertas**.

1. En la parte superior, seleccione **Administrar reglas de alerta**.

1. Seleccione la alerta que desea editar o eliminar.

1. Tiene las siguientes opciones: **Guardar**, **Descartar**, **Deshabilitar** y **Eliminar**.

    ![Alerta para editar o eliminar registros de Azure Monitor para HDInsight](media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-edit-alert.png)

Para más información, vea [Creación, visualización y administración de alertas de métricas mediante Azure Monitor](../azure-monitor/platform/alerts-metric.md).

## <a name="see-also"></a>Consulte también

* [Introducción a las consultas de registro en Azure Monitor](../azure-monitor/log-query/get-started-queries.md)
* [Creación de vistas personalizadas mediante el Diseñador de vistas en Azure Monitor](../azure-monitor/platform/view-designer.md)

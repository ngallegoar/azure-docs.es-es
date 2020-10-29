---
title: 'Registros de Azure Monitor para Apache Kafka: Azure HDInsight'
description: Aprenda a usar registros de Azure Monitor para analizar registros de clúster de Apache Kafka en Azure HDInsight.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: how-to
ms.custom: hdinsightactive
ms.date: 02/17/2020
ms.openlocfilehash: 5128ac7608dfce08471f7a7f97ed28a7971e62b2
ms.sourcegitcommit: d767156543e16e816fc8a0c3777f033d649ffd3c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/26/2020
ms.locfileid: "92534454"
---
# <a name="analyze-logs-for-apache-kafka-on-hdinsight"></a>Análisis de registros para Apache Kafka en HDInsight

Aprenda a usar registros de Azure Monitor para analizar registros generados por Apache Kafka en HDInsight.

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../../includes/azure-monitor-log-analytics-rebrand.md)]

## <a name="logs-location"></a>Ubicación de los registros

Los registros de Apache Kafka del clúster se encuentran en `/var/log/kafka`. Los registros de Kafka no se guardan ni se conservan entre ciclos de vida del clúster, con independencia de si se usan discos administrados. En la siguiente tabla se muestran los registros disponibles.

|Log |Descripción |
|---|---|
|kafka.out|stdout y stderr del proceso de Kafka. Encontrará los registros de inicio y cierre de Kafka en este archivo.|
|server.log|El registro principal del servidor de Kafka. Todos los registros del agente de Kafka terminan aquí.|
|controller.log|Registros de controlador si el agente actúa como controlador.|
|statechange.log|Todos los eventos de cambio de estado en los agentes se registran en este archivo.|
|kafka-gc.log|Estadísticas de recolección de elementos no utilizados de Kafka.|

## <a name="enable-azure-monitor-logs-for-apache-kafka"></a>Habilitar registros de Azure Monitor para Apache Kafka

Los pasos para habilitar registros de Azure Monitor para HDInsight son los mismos que para todos los clústeres de HDInsight. Use los vínculos siguientes para aprender a crear y configurar los servicios necesarios:

1. Cree un área de trabajo de Log Analytics. Para obtener más información, vea el documento [Registros en Azure Monitor](../../azure-monitor/platform/data-platform-logs.md).

2. Cree Kafka en un clúster de HDInsight. Para obtener más información, consulte el documento [Introducción a Apache Kafka (versión preliminar) en HDInsight](apache-kafka-get-started.md).

3. Configure el clúster de Kafka para usar registros de Azure Monitor. Para obtener más información, vea el documento [Uso de registros de Azure Monitor para supervisar HDInsight](../hdinsight-hadoop-oms-log-analytics-tutorial.md).

> [!IMPORTANT]  
> Pueden pasar unos 20 minutos hasta que los datos estén disponibles para los registros de Azure Monitor.

## <a name="query-logs"></a>Registros de consultas

1. En [Azure Portal](https://portal.azure.com), seleccione el área de trabajo de Log Analytics.

2. En el menú izquierdo, en **General** , seleccione **Registros** . Aquí podrá buscar los datos recopilados de Kafka. Escriba una consulta en la ventana de consulta y seleccione **Ejecutar** . A continuación se presentan algunos ejemplos de búsqueda:

* Uso de disco:

    ```kusto
    Perf
    | where ObjectName == "Logical Disk" and CounterName == "Free Megabytes" and InstanceName == "_Total" and ((Computer startswith_cs "hn" and Computer contains_cs "-") or (Computer startswith_cs "wn" and Computer contains_cs "-")) 
    | summarize AggregatedValue = avg(CounterValue) by Computer, bin(TimeGenerated, 1h)
    ```

* Uso de CPU:

    ```kusto
    Perf 
    | where CounterName == "% Processor Time" and InstanceName == "_Total" and ((Computer startswith_cs "hn" and Computer contains_cs "-") or (Computer startswith_cs "wn" and Computer contains_cs "-")) 
    | summarize AggregatedValue = avg(CounterValue) by Computer, bin(TimeGenerated, 1h)
    ```

* Mensajes entrantes por segundo: (Reemplace `your_kafka_cluster_name` por el nombre del clúster).

    ```kusto
    metrics_kafka_CL 
    | where ClusterName_s == "your_kafka_cluster_name" and InstanceName_s == "kafka-BrokerTopicMetrics-MessagesInPerSec-Count" 
    | summarize AggregatedValue = avg(kafka_BrokerTopicMetrics_MessagesInPerSec_Count_value_d) by HostName_s, bin(TimeGenerated, 1h)
    ```

* Bytes entrantes por segundo: (Reemplace `wn0-kafka` por un nombre de host del nodo de trabajo).

    ```kusto
    metrics_kafka_CL 
    | where HostName_s == "wn0-kafka" and InstanceName_s == "kafka-BrokerTopicMetrics-BytesInPerSec-Count" 
    | summarize AggregatedValue = avg(kafka_BrokerTopicMetrics_BytesInPerSec_Count_value_d) by bin(TimeGenerated, 1h)
    ```

* Bytes salientes por segundo: (Reemplace `your_kafka_cluster_name` por el nombre del clúster).

    ```kusto
    metrics_kafka_CL 
    | where ClusterName_s == "your_kafka_cluster_name" and InstanceName_s == "kafka-BrokerTopicMetrics-BytesOutPerSec-Count" 
    | summarize AggregatedValue = avg(kafka_BrokerTopicMetrics_BytesOutPerSec_Count_value_d) by bin(TimeGenerated, 1h)
    ```

    También puede escribir `*` para buscar todos los tipos registrados. Actualmente están disponibles los registros siguientes para las consultas:

    | Tipo de registro | Descripción |
    | ---- | ---- |
    | log\_kafkaserver\_CL | Kafka broker server.log |
    | log\_kafkacontroller\_CL | Kafka broker controller.log |
    | metrics\_kafka\_CL | Kafka JMX metrics |

    ![Apache Kafka: uso de la CPU de Log Analytics](./media/apache-kafka-log-analytics-operations-management/apache-kafka-cpu-usage.png)

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información sobre Azure Monitor, vea [Introducción a Azure Monitor](../../azure-monitor/overview.md) y [Uso de consultas con registros de Azure Monitor](../hdinsight-hadoop-oms-log-analytics-use-queries.md).

Para información sobre cómo trabajar con Apache Kafka, consulte los documentos siguientes:

* [Uso de MirrorMaker para replicar temas de Apache Kafka con Kafka en HDInsight](apache-kafka-mirroring.md)
* [Aumento de la escala de Apache Kafka en HDInsight](apache-kafka-scalability.md)
* [Ejemplo de streaming de Apache Spark (DStream) con Apache Kafka en HDInsight](../hdinsight-apache-spark-with-kafka.md)
* [Tutorial: Uso de Apache Spark Structured Streaming con Apache Kafka en HDInsight](../hdinsight-apache-kafka-spark-structured-streaming.md)
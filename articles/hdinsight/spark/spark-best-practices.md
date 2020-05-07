---
title: Instrucciones de Apache Spark en Azure HDInsight
description: Obtenga instrucciones para usar Apache Spark en Azure HDInsight.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 04/28/2020
ms.openlocfilehash: 01cdc121abded954c2443599c5d69689acd69b62
ms.sourcegitcommit: 856db17a4209927812bcbf30a66b14ee7c1ac777
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/29/2020
ms.locfileid: "82562345"
---
# <a name="apache-spark-guidelines"></a>Instrucciones de Apache Spark

En este artículo se proporcionan instrucciones para usar Apache Spark en Azure HDInsight.

## <a name="how-do-i-run-or-submit-spark-jobs"></a>¿Cómo se ejecutan o envían los trabajos de Spark?

| Opción | Documentos |
|---|---|
| VSCode | [Usar las herramientas Spark y Hive para Visual Studio Code](../hdinsight-for-vscode.md) |
| Cuadernos de Jupyter Notebook | [Tutorial: Carga de datos y ejecución de consultas en un clúster de Apache Spark en Azure HDInsight](./apache-spark-load-data-run-query.md) |
| IntelliJ | [Tutorial: Uso de Azure Toolkit for IntelliJ con el fin de crear aplicaciones Apache Spark para un clúster de HDInsight](./apache-spark-intellij-tool-plugin.md) |
| IntelliJ | [Tutorial: Creación de una aplicación de Scala Maven para Apache Spark en HDInsight mediante IntelliJ](./apache-spark-create-standalone-application.md) |
| Zeppelin Notebooks | [Uso de cuadernos de Apache Zeppelin con un clúster Apache Spark en Azure HDInsight](./apache-spark-zeppelin-notebook.md) |
| Envío de trabajos remotos con Livy | [Uso de la API REST de Apache Spark para enviar trabajos remotos a un clúster Spark de HDInsight](./apache-spark-livy-rest-interface.md) |
|[Apache Oozie](../hdinsight-use-oozie-linux-mac.md)|Oozie es un sistema de coordinación y flujos de trabajo que administra trabajos de Hadoop.|
|[Apache Livy](./apache-spark-livy-rest-interface.md)|Puede usar Livy para ejecutar shells de Spark interactivos o enviar trabajos por lotes que se ejecutarán en Spark.|
|[Azure Data Factory para Apache Spark](../../data-factory/transform-data-using-spark.md)|La actividad de Spark en una canalización de Data Factory ejecuta un programa de Spark en su clúster de HDInsight propio o [a petición.|
|[Azure Data Factory para Apache Hive](../../data-factory/transform-data-using-hadoop-hive.md)|La actividad de Hive de HDInsight en una canalización de Data Factory ejecuta consultas de Hive en su propio clúster de HDInsight o en uno a petición.|

## <a name="how-do-i-monitor-and-debug-spark-jobs"></a>¿Cómo se supervisan y depuran los trabajos de Spark?

| Opción | Documentos |
|---|---|
| Kit de herramientas de Azure para IntelliJ | [Error de depuración de trabajos de Spark con Azure Toolkit for IntelliJ (versión preliminar)](apache-spark-intellij-tool-failure-debug.md) |
| Azure Toolkit for IntelliJ a través de SSH | [Depuración de aplicaciones de Apache Spark de forma local o remota en un clúster de HDInsight con Azure Toolkit for IntelliJ mediante SSH](apache-spark-intellij-tool-debug-remotely-through-ssh.md) |
| Azure Toolkit for IntelliJ a través de VPN | [Uso de Azure Toolkit for IntelliJ para depurar de forma remota aplicaciones de Apache Spark en HDInsight mediante VPN](apache-spark-intellij-tool-plugin-debug-jobs-remotely.md) |
| Gráfico de trabajo en el servidor de historial de Apache Spark | [Uso del servidor de historial de Apache Spark extendido para depurar y diagnosticar las aplicaciones de Spark](./apache-azure-spark-history-server.md) |

## <a name="how-do-i-make-my-spark-jobs-run-more-efficiently"></a>¿Cómo consigo que mis trabajos de Spark se ejecuten de manera más eficaz?

| Opción | Documentos |
|---|---|
| Caché de E/S | [Mejora del rendimiento de las cargas de trabajo de Apache Spark con la memoria caché de E/S de Azure HDInsight (versión preliminar)](./apache-spark-improve-performance-iocache.md) |
| Opciones de configuración | [Optimización de trabajos de Apache Spark](./apache-spark-perf.md) |

## <a name="how-do-i-connect-to-other-azure-services"></a>¿Cómo me conecto a otros servicios de Azure?

| Opción | Documentos |
|---|---|
| Apache Hive en HDInsight | [Integración de Apache Spark y Apache Hive con el conector de Hive Warehouse](../interactive-query/apache-hive-warehouse-connector.md) |
| Apache HBase en HDInsight | [Uso de Apache Spark para leer y escribir datos de Apache HBase](../hdinsight-using-spark-query-hbase.md) |
| Apache Kafka en HDInsight | [Tutorial: Uso del flujo estructurado de Apache Spark con Apache Kafka en HDInsight](../hdinsight-apache-kafka-spark-structured-streaming.md) |
| Azure Cosmos DB | [Azure Cosmos DB: Implementación de una arquitectura lambda en la plataforma Azure](../../cosmos-db/lambda-architecture.md) |

## <a name="what-are-my-storage-options"></a>¿Cuáles son mis opciones de almacenamiento?

| Opción | Documentos |
|---|---|
| Data Lake Storage Gen2 | [Uso de Data Lake Storage Gen2 con clústeres de Azure HDInsight](../hdinsight-hadoop-use-data-lake-storage-gen2.md) |
| Data Lake Storage Gen1 | [Usar Data Lake Storage Gen1 con clústeres de Azure HDInsight](../hdinsight-hadoop-use-data-lake-store.md) |
| Azure Blob Storage | [Uso de Azure Storage con clústeres de Azure HDInsight](../hdinsight-hadoop-use-blob-storage.md) |

## <a name="next-steps"></a>Pasos siguientes

* [Configuración de opciones de Apache Spark](apache-spark-settings.md)
* [Optimización de trabajos de Apache Spark en HDInsight](apache-spark-perf.md)

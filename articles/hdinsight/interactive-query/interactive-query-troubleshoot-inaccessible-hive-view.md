---
title: 'Conexiones de Apache Hive con Apache Zookeeper: Azure HDInsight'
description: No se puede acceder a la vista de Apache Hive debido a problemas de Apache Zookeeper en Azure HDInsight
ms.service: hdinsight
ms.topic: troubleshooting
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.date: 07/30/2019
ms.openlocfilehash: 1ac237edeea777225b7cf99946d359c921331b18
ms.sourcegitcommit: 7863fcea618b0342b7c91ae345aa099114205b03
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/03/2020
ms.locfileid: "93288904"
---
# <a name="scenario-apache-hive-fails-to-establish-a-connection-to-apache-zookeeper-in-azure-hdinsight"></a>Escenario: Apache Hive no puede establecer una conexión con Apache Zookeeper en Azure HDInsight

En este artículo se describen los pasos para la solución de problemas y las posibles soluciones para los problemas que se producen al usar componentes de Interactive Query en clústeres de Azure HDInsight.

## <a name="issue"></a>Problema

No se puede tener acceso a la vista de Hive y los registros de `/var/log/hive` muestran un error similar al siguiente:

```
ERROR [Curator-Framework-0]: curator.ConnectionState (ConnectionState.java:checkTimeouts(200)) - Connection timed out for connection string (zk0-cluster.cloud.wbmi.com:2181,zk1-cluster.cloud.wbmi.com:2181,zk2-cluster.cloud.wbmi.com:2181) and timeout (15000) / elapsed (21852)
```

## <a name="cause"></a>Causa

Es posible que Hive no pueda establecer una conexión con Zookeeper, lo que impide que se inicie la vista de Hive.

## <a name="resolution"></a>Solución

1. Compruebe que el servicio Zookeeper está en buen estado.

1. Compruebe si el servicio Zookeeper tiene una entrada ZNode para Hive Server2. Falta el valor o no es correcto.

    ```
    /usr/hdp/2.6.2.25-1/zookeeper/bin/zkCli.sh -server zk1-wbwdhs
    [zk: zk0-cluster(CONNECTED) 0] ls /hiveserver2-hive2
    ```

1. Para volver a establecer la conectividad, reinicie los nodos de Zookeeper y reinicie HiveServer2.

## <a name="next-steps"></a>Pasos siguientes

[!INCLUDE [troubleshooting next steps](../../../includes/hdinsight-troubleshooting-next-steps.md)]
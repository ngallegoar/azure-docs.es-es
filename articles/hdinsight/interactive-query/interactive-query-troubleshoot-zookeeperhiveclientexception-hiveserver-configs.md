---
title: 'Error del intérprete de Zeppelin para Apache Hive: Azure HDInsight'
description: El intérprete JDBC de Zeppelin para Apache Hive apunta a la dirección URL equivocada en Azure HDInsight
ms.service: hdinsight
ms.topic: troubleshooting
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.date: 07/30/2019
ms.openlocfilehash: 8e66db96f895d0d29d6c5018478e98c1f39e62ad
ms.sourcegitcommit: 7863fcea618b0342b7c91ae345aa099114205b03
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/03/2020
ms.locfileid: "93288711"
---
# <a name="scenario-apache-hive-zeppelin-interpreter-gives-a-zookeeper-error-in-azure-hdinsight"></a>Escenario: El intérprete de Zeppelin para Apache Hive genera un error de Zookeeper en Azure HDInsight

En este artículo se describen los pasos para la solución de problemas y las posibles soluciones para los problemas que se producen al usar componentes de Interactive Query en clústeres de Azure HDInsight.

## <a name="issue"></a>Problema

En un clúster de LLAP de Apache Hive, el intérprete de Zeppelin devuelve el siguiente mensaje de error al intentar ejecutar una consulta:

```
java.sql.SQLException: org.apache.hive.jdbc.ZooKeeperHiveClientException: Unable to read HiveServer2 configs from ZooKeeper
```

## <a name="cause"></a>Causa

El intérprete JDBC de Hive de Zeppelin apunta a la dirección URL equivocada.

## <a name="resolution"></a>Solución

1. Navegue hasta el resumen de componente de Hive y copie la "Dirección URL de JDBC de Hive" en el portapapeles.

1. Vaya a `https://clustername.azurehdinsight.net/zeppelin/#/interpreter`.

1. Edite la configuración de JDBC: actualice el valor de Hive.url a la dirección URL de JDBC de Hive copiada en el paso 1.

1. Guarde y vuelva a intentar la consulta

## <a name="next-steps"></a>Pasos siguientes

[!INCLUDE [troubleshooting next steps](../../../includes/hdinsight-troubleshooting-next-steps.md)]
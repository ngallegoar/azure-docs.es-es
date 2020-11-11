---
title: 'Solución de problemas: Los registros de Apache Hive llenan el espacio en disco: Azure HDInsight'
description: En este artículo se proporcionan los pasos para la solución de problemas que se deben seguir cuando los registros de Apache Hive llenan el espacio en disco de los nodos principales de Azure HDInsight.
ms.service: hdinsight
ms.topic: troubleshooting
author: nisgoel
ms.author: nisgoel
ms.reviewer: jasonh
ms.date: 10/05/2020
ms.openlocfilehash: 107ec012bf2ff76ee1cbe4c5f8252566a5a16127
ms.sourcegitcommit: 7863fcea618b0342b7c91ae345aa099114205b03
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/03/2020
ms.locfileid: "93288926"
---
# <a name="scenario-apache-hive-logs-are-filling-up-the-disk-space-on-the-head-nodes-in-azure-hdinsight"></a>Escenario: Los registros de Apache Hive llenan el espacio en disco de los nodos principales de Azure HDInsight.

En este artículo se describen los pasos para la solución de problemas y las posibles opciones para problemas relacionados con el espacio en disco insuficiente en los nodos principales de los clústeres de Azure HDInsight.

## <a name="issue"></a>Problema

En un clúster de Apache Hive/LLAP, los registros no deseados ocupan todo el espacio en disco de los nodos principales. Esta condición podría provocar los problemas siguientes:

- No se puede acceder a SSH porque no queda espacio en el nodo principal.
- Ambari produce el *ERROR HTTP: 503: Servicio no disponible*.
- HiveServer2 Interactive no se puede reiniciar.

Los registros de `ambari-agent` incluirán las siguientes entradas cuando se produzca el problema:
```
ambari_agent - Controller.py - [54697] - Controller - ERROR - Error:[Errno 28] No space left on device
```
```
ambari_agent - HostCheckReportFileHandler.py - [54697] - ambari_agent.HostCheckReportFileHandler - ERROR - Can't write host check file at /var/lib/ambari-agent/data/hostcheck.result
```

## <a name="cause"></a>Causa

En las configuraciones avanzadas de Hive log4j, la programación de eliminación predeterminada actual es borrar los archivos de más de 30 días, según la fecha de la última modificación.

## <a name="resolution"></a>Solución

1. Vaya al resumen de componentes de Hive en el portal de Ambari y seleccione la pestaña **Configs** (Configuraciones).

2. Vaya a la sección `Advanced hive-log4j` en **Advanced settings** (Configuración avanzada).

3. Establezca el parámetro `appender.RFA.strategy.action.condition.age` en una antigüedad de su elección. En este ejemplo se establecerá la antigüedad en 14 días: `appender.RFA.strategy.action.condition.age = 14D`

4. Si no ve ninguna configuración relacionada, anexe estos valores:
    ```
    # automatically delete hive log
    appender.RFA.strategy.action.type = Delete
    appender.RFA.strategy.action.basePath = ${sys:hive.log.dir}
    appender.RFA.strategy.action.condition.type = IfLastModified
    appender.RFA.strategy.action.condition.age = 30D
    appender.RFA.strategy.action.PathConditions.type = IfFileName
    appender.RFA.strategy.action.PathConditions.regex = hive*.*log.*
    ```

5. Establezca `hive.root.logger` en `INFO,RFA`, tal como se muestra en el ejemplo siguiente. La configuración predeterminada es `DEBUG`, lo que aumenta el tamaño de los registros.

    ```
    # Define some default values that can be overridden by system properties
    hive.log.threshold=ALL
    hive.root.logger=INFO,RFA
    hive.log.dir=${java.io.tmpdir}/${user.name}
    hive.log.file=hive.log
    ```

6. Guarde las configuraciones y reinicie los componentes necesarios.

## <a name="next-steps"></a>Pasos siguientes

[!INCLUDE [troubleshooting next steps](../../../includes/hdinsight-troubleshooting-next-steps.md)]

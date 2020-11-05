---
title: REST de Apache HBase no responde a las solicitudes en Azure HDInsight
description: Resolución del problema con REST de Apache HBase que no responde a las solicitudes en Azure HDInsight.
ms.service: hdinsight
ms.topic: troubleshooting
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.date: 08/01/2019
ms.openlocfilehash: c4a0ef82b951fa43eb4c58050d3148fd2d695026
ms.sourcegitcommit: 7863fcea618b0342b7c91ae345aa099114205b03
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/03/2020
ms.locfileid: "93286969"
---
# <a name="scenario-apache-hbase-rest-not-responding-to-requests-in-azure-hdinsight"></a>Escenario: REST de Apache HBase no responde a las solicitudes en Azure HDInsight

En este artículo se describen los pasos de solución de problemas y las posibles soluciones para los problemas que se producen al usar clústeres de Azure HDInsight.

## <a name="issue"></a>Problema

El servicio REST de Apache HBase no responde a las solicitudes en Azure HDInsight.

## <a name="cause"></a>Causa

La causa posible es que el servicio REST de Apache HBase esté perdiendo sockets, lo que es especialmente común cuando el servicio se ha estado ejecutando durante mucho tiempo (por ejemplo, meses). En el SDK de cliente, es posible que vea un mensaje de error similar al siguiente:

```
System.Net.WebException : Unable to connect to the remote server --->
System.Net.Sockets.SocketException : A connection attempt failed because the connected party did not properly respond after a period of time, or established connection failed because connected host has failed to respond 10.0.0.19:8090
```

## <a name="resolution"></a>Solución

Reinicie REST de HBase con el siguiente comando después de hacer SSH al host. También puede usar acciones de script para reiniciar este servicio en todos los nodos de trabajo:

```bash
sudo service hdinsight-hbrest restart
```

Este comando detendrá el servidor de regiones de HBase en el mismo host. Puede iniciar manualmente el servidor de regiones de HBase con Ambari o bien dejar que la funcionalidad de reinicio automático de Ambari lo recupere automáticamente.

Si el problema persiste, puede instalar el siguiente script de mitigación como un trabajo CRON que se ejecute cada cinco minutos en cada nodo de trabajo. Este script de mitigación hace ping al servicio REST y lo reinicia en caso de que no responda.

```bash
#!/bin/bash
nc localhost 8090 < /dev/null
if [ $? -ne 0 ]
    then
    echo "RESTServer is not responding. Restarting"
    sudo /usr/hdp/current/hbase-regionserver/bin/hbase-daemon.sh restart rest
fi
```

## <a name="next-steps"></a>Pasos siguientes

[!INCLUDE [troubleshooting next steps](../../../includes/hdinsight-troubleshooting-next-steps.md)]
---
title: REST de Apache HBase no responde a las solicitudes en Azure HDInsight
description: Resolución del problema con REST de Apache HBase que no responde a las solicitudes en Azure HDInsight.
ms.service: hdinsight
ms.topic: troubleshooting
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.date: 08/01/2019
ms.openlocfilehash: 49b547829a369ea6df35e2f1c2f7d60458e41040
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "75887179"
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

Si su problema no aparece o es incapaz de resolverlo, visite uno de nuestros canales para obtener ayuda adicional:

* Obtenga respuestas de expertos de Azure mediante el [soporte técnico de la comunidad de Azure](https://azure.microsoft.com/support/community/).

* Póngase en contacto con [@AzureSupport](https://twitter.com/azuresupport), la cuenta oficial de Microsoft Azure para mejorar la experiencia del cliente, que pone en contacto a la comunidad de Azure con los recursos adecuados: respuestas, soporte técnico y expertos.

* Si necesita más ayuda, puede enviar una solicitud de soporte técnico desde [Azure Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/). Seleccione **Soporte técnico** en la barra de menús o abra la central **Ayuda + soporte técnico**. Para obtener información más detallada, revise [Creación de una solicitud de soporte técnico de Azure](https://docs.microsoft.com/azure/azure-portal/supportability/how-to-create-azure-support-request). La suscripción a Microsoft Azure incluye acceso al soporte técnico para facturación y administración de suscripciones. El soporte técnico se proporciona a través de uno de los [planes de soporte técnico de Azure](https://azure.microsoft.com/support/plans/).

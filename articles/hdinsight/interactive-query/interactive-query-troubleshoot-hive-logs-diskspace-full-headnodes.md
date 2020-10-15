---
title: 'Los registros de Apache Hive llenan el espacio en disco: Azure HDInsight'
description: Los registros de Apache Hive están llenando el espacio en disco de los nodos principales de Azure HDInsight.
ms.service: hdinsight
ms.topic: troubleshooting
author: nisgoel
ms.author: nisgoel
ms.reviewer: jasonh
ms.date: 03/05/2020
ms.openlocfilehash: d843b942702d335065a5f3798572e34c71b4cd0e
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "78943962"
---
# <a name="scenario-apache-hive-logs-are-filling-up-the-disk-space-on-the-head-nodes-in-azure-hdinsight"></a>Escenario: Los registros de Apache Hive están llenando el espacio en disco de los nodos principales de Azure HDInsight

En este artículo se describen los pasos para la solución de problemas y las posibles opciones para cuestiones relacionadas con la falta de espacio en disco en los nodos principales de los clústeres de Azure HDInsight.

## <a name="issue"></a>Problema

En un clúster de Apache Hive/LLAP, los registros no deseados ocupan todo el espacio en disco de los nodos principales. Debido a esto, se han podido ver los siguientes problemas.

1. No se puede acceder a SSH porque no queda espacio en el nodo principal.
2. Ambari muestra un *ERROR HTTP: 503: Servicio no disponible*.

Los registros de `ambari-agent` muestran lo siguiente cuando se produce el problema.
```
ambari_agent - Controller.py - [54697] - Controller - ERROR - Error:[Errno 28] No space left on device
```
```
ambari_agent - HostCheckReportFileHandler.py - [54697] - ambari_agent.HostCheckReportFileHandler - ERROR - Can't write host check file at /var/lib/ambari-agent/data/hostcheck.result
```

## <a name="cause"></a>Causa

En las configuraciones avanzadas de Hive-Log4J, se omite el parámetro *log4j.appender.RFA.MaxBackupIndex*. Esto provoca una generación interminable de archivos de registro.

## <a name="resolution"></a>Solución

1. Vaya al resumen de componentes de Hive en el portal de Ambari y haga clic en la pestaña `Configs`.

2. Vaya a la sección `Advanced hive-log4j` en Advanced settings (Configuración avanzada).

3. Establezca el parámetro `log4j.appender.RFA` como RollingFileAppender. 

4. Establezca `log4j.appender.RFA.MaxFileSize` y `log4j.appender.RFA.MaxBackupIndex` como se indica a continuación.

```
log4jhive.log.maxfilesize=1024MB
log4jhive.log.maxbackupindex=10

log4j.appender.RFA=org.apache.log4j.RollingFileAppender
log4j.appender.RFA.File=${hive.log.dir}/${hive.log.file}
log4j.appender.RFA.MaxFileSize=${log4jhive.log.maxfilesize}
log4j.appender.RFA.MaxBackupIndex=${log4jhive.log.maxbackupindex}
log4j.appender.RFA.layout=org.apache.log4j.PatternLayout
log4j.appender.RFA.layout.ConversionPattern=%d{ISO8601} %-5p [%t] %c{2}: %m%n
```
5. Establezca `hive.root.logger` en `INFO,RFA`, como se indica a continuación. La configuración predeterminada es DEBUG, lo que provoca que los registros sean muy grandes.

```
# Define some default values that can be overridden by system properties
hive.log.threshold=ALL
hive.root.logger=INFO,RFA
hive.log.dir=${java.io.tmpdir}/${user.name}
hive.log.file=hive.log
```

6. Guarde las configuraciones y reinicie los componentes necesarios.

## <a name="next-steps"></a>Pasos siguientes

Si su problema no aparece o es incapaz de resolverlo, visite uno de nuestros canales para obtener ayuda adicional:

* Obtenga respuestas de expertos de Azure mediante el [soporte técnico de la comunidad de Azure](https://azure.microsoft.com/support/community/).

* Póngase en contacto con [@AzureSupport](https://twitter.com/azuresupport), la cuenta oficial de Microsoft Azure para mejorar la experiencia del cliente, que pone en contacto a la comunidad de Azure con los recursos adecuados: respuestas, soporte técnico y expertos.

* Si necesita más ayuda, puede enviar una solicitud de soporte técnico desde [Azure Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/). Seleccione **Soporte técnico** en la barra de menús o abra la central **Ayuda + soporte técnico**. Para obtener información más detallada, revise [Creación de una solicitud de soporte técnico de Azure](https://docs.microsoft.com/azure/azure-portal/supportability/how-to-create-azure-support-request). La suscripción a Microsoft Azure incluye acceso al soporte técnico para facturación y administración de suscripciones. El soporte técnico se proporciona a través de uno de los [planes de soporte técnico de Azure](https://azure.microsoft.com/support/plans/).

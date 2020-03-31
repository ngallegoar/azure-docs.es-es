---
title: Error 502 de la interfaz de usuario de Apache Ambari en Azure HDInsight
description: Error 502 de la interfaz de usuario de Apache Ambari al intentar acceder al clúster de Azure HDInsight
ms.service: hdinsight
ms.topic: troubleshooting
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.date: 08/05/2019
ms.openlocfilehash: 2b17c2488e47148e8845433f9c7613e1127fbffa
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "75895759"
---
# <a name="scenario-apache-ambari-ui-502-error-in-azure-hdinsight"></a>Escenario: Error 502 de la interfaz de usuario de Apache Ambari en Azure HDInsight

En este artículo se describen los pasos de solución de problemas y las posibles soluciones para los problemas que se producen al usar clústeres de Azure HDInsight.

## <a name="issue"></a>Problema

Al intentar acceder a la interfaz de usuario de Apache Ambari del clúster de HDInsight, recibe un mensaje parecido a este: "502: el servidor web ha recibido una respuesta no válida mientras actuaba como puerta de enlace o servidor proxy".

## <a name="cause"></a>Causa

En general, el código de estado HTTP 502 significa que el servidor de Ambari no se está ejecutando correctamente en el nodo principal activo. Las causas principales pueden ser varias.

## <a name="resolution"></a>Solución

En la mayoría de los casos, para mitigar el problema, puede reiniciar el nodo principal activo. O bien, puede elegir un tamaño de máquina virtual mayor para el nodo principal.

### <a name="ambari-server-failed-to-start"></a>No se pudo iniciar el servidor de Ambari

Puede comprobar los registros de ambari-server para averiguar por qué no se pudo iniciar. el servidor de Ambari. Uno de los motivos más habituales es el error de comprobación de coherencia de la base de datos. Este error lo puede encontrar en este archivo de registro: `/var/log/ambari-server/ambari-server-check-database.log`.

Si realizó alguna modificación en el nodo de clúster, deshaga la operación. Use siempre la interfaz de usuario de Ambari para modificar cualquier configuración relacionada con Hadoop o Spark.

### <a name="ambari-server-taking-100-cpu-utilization"></a>El servidor de Ambari llega al 100 % de uso de la CPU

En raras ocasiones, hemos visto que el proceso ambari-server se acerca continuamente a un uso de la CPU del 100 %. Como medida de mitigación, puede ejecutar ssh en el nodo principal activo y terminar el proceso de servidor de Ambari y volver a iniciarlo.

```bash
ps -ef | grep AmbariServer
top -p <ambari-server-pid>
kill -9 <ambari-server-pid>
service ambari-server start
```

### <a name="ambari-server-killed-by-oom-killer"></a>El servidor de Ambari se terminó con oom-killer

En algunos escenarios, el nodo principal se queda sin memoria y oom-killer de Linux comienza a elegir los procesos que se van a terminar. Para confirmar esta situación, busque el identificador de proceso de AmbariServer, que no debería encontrarse. A continuación, examine `/var/log/syslog` y busque algo parecido a esto:

```
Jul 27 15:29:30 xxx-xxxxxx kernel: [874192.703153] java invoked oom-killer: gfp_mask=0x23201ca, order=0, oom_score_adj=0
```

Luego, identifique qué procesos ocupan las memorias e intente determinar la causa principal.

### <a name="other-issues-with-ambari-server"></a>Otros problemas con el servidor de Ambari

En raras ocasiones, el servidor de Ambari no puede controlar la solicitud entrante; para encontrar más información, examine los registros de ambari-server para ver si hay algún error. Uno de estos casos es un error similar al siguiente:

```
Error Processing URI: /api/v1/clusters/xxxxxx/host_components - (java.lang.OutOfMemoryError) Java heap space
```

## <a name="next-steps"></a>Pasos siguientes

Si su problema no aparece o es incapaz de resolverlo, visite uno de nuestros canales para obtener ayuda adicional:

* Obtenga respuestas de expertos de Azure mediante el [soporte técnico de la comunidad de Azure](https://azure.microsoft.com/support/community/).

* Póngase en contacto con [@AzureSupport](https://twitter.com/azuresupport), la cuenta oficial de Microsoft Azure para mejorar la experiencia del cliente, que pone en contacto a la comunidad de Azure con los recursos adecuados: respuestas, soporte técnico y expertos.

* Si necesita más ayuda, puede enviar una solicitud de soporte técnico desde [Azure Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/). Seleccione **Soporte técnico** en la barra de menús o abra la central **Ayuda + soporte técnico**. Para obtener información más detallada, revise [Creación de una solicitud de soporte técnico de Azure](https://docs.microsoft.com/azure/azure-portal/supportability/how-to-create-azure-support-request). La suscripción a Microsoft Azure incluye acceso al soporte técnico para facturación y administración de suscripciones. El soporte técnico se proporciona a través de uno de los [planes de soporte técnico de Azure](https://azure.microsoft.com/support/plans/).

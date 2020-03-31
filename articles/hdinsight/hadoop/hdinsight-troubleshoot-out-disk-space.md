---
title: El nodo de clúster se queda sin espacio en disco en Azure HDInsight.
description: Solución de problemas de espacio en disco del nodo de clúster de Apache Hadoop en Azure HDInsight.
ms.service: hdinsight
ms.topic: troubleshooting
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.date: 08/05/2019
ms.openlocfilehash: fbfd82473b68f5032d19834ac809191d498a5a67
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "75894124"
---
# <a name="scenario-cluster-node-runs-out-of-disk-space-in-azure-hdinsight"></a>Escenario: El nodo de clúster se queda sin espacio en disco en Azure HDInsight.

En este artículo se describen los pasos de solución de problemas y las posibles soluciones para los problemas que se producen al usar clústeres de Azure HDInsight.

## <a name="issue"></a>Problema

Un trabajo puede producir un error con un mensaje similar al siguiente: `/usr/hdp/2.6.3.2-14/hadoop/libexec/hadoop-config.sh: fork: No space left on device.`

O bien, se puede recibir una alerta de Apache Ambari similar a la siguiente: `local-dirs usable space is below configured utilization percentage`.

## <a name="cause"></a>Causa

La memoria caché de las aplicaciones de Apache Yarn puede haber consumido todo el espacio disponible en disco. Es probable que la aplicación Spark se ejecute de forma ineficaz.

## <a name="resolution"></a>Solución

1. Utilice la interfaz de usuario de Ambari para determinar qué nodo se está quedando sin espacio en disco.

1. Determine qué carpeta del nodo en cuestión contribuye a la mayor parte del espacio en disco. Primero realice un SSH en el nodo y, a continuación, ejecute `df` para mostrar el uso de disco en todos los montajes. Normalmente es `/mnt`, que es un disco temporal que usa OSS. Puede introducirlo en una carpeta y, después, escribir `sudo du -hs` para mostrar los tamaños de archivo resumidos en una carpeta. Si ve una carpeta similar a `/mnt/resource/hadoop/yarn/local/usercache/livy/appcache/application_1537280705629_0007`, significa que la aplicación todavía se está ejecutando. Esto puede deberse a la persistencia de RDD o a los archivos intermedios aleatorios.

1. Para mitigar el problema, elimine la aplicación; de esta forma, liberará el espacio en disco utilizado por ella.

1. Para resolver finalmente el problema, optimice la aplicación.

## <a name="next-steps"></a>Pasos siguientes

Si su problema no aparece o es incapaz de resolverlo, visite uno de nuestros canales para obtener ayuda adicional:

* Obtenga respuestas de expertos de Azure mediante el [soporte técnico de la comunidad de Azure](https://azure.microsoft.com/support/community/).

* Póngase en contacto con [@AzureSupport](https://twitter.com/azuresupport), la cuenta oficial de Microsoft Azure para mejorar la experiencia del cliente, que pone en contacto a la comunidad de Azure con los recursos adecuados: respuestas, soporte técnico y expertos.

* Si necesita más ayuda, puede enviar una solicitud de soporte técnico desde [Azure Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/). Seleccione **Soporte técnico** en la barra de menús o abra la central **Ayuda + soporte técnico**. Para obtener información más detallada, revise [Creación de una solicitud de soporte técnico de Azure](https://docs.microsoft.com/azure/azure-portal/supportability/how-to-create-azure-support-request). La suscripción a Microsoft Azure incluye acceso al soporte técnico para facturación y administración de suscripciones. El soporte técnico se proporciona a través de uno de los [planes de soporte técnico de Azure](https://azure.microsoft.com/support/plans/).

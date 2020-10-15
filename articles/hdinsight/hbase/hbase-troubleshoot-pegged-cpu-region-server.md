---
title: 'CPU fija en el clúster de Apache HBase: Azure HDInsight'
description: Solución de problemas de CPU fija en el servidor de regiones del clúster de Apache HBase en Azure HDInsight
ms.service: hdinsight
ms.topic: troubleshooting
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.date: 08/01/2019
ms.openlocfilehash: 16c994029e91d743f1c2a7e2eab51eb86fc378e8
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "75887315"
---
# <a name="scenario-pegged-cpu-on-region-server-in-apache-hbase-cluster-in-azure-hdinsight"></a>Escenario: CPU fija en el servidor de regiones del clúster de Apache HBase en Azure HDInsight

En este artículo se describen los pasos de solución de problemas y las posibles soluciones para los problemas que se producen al usar clústeres de Azure HDInsight.

## <a name="issue"></a>Problema

El proceso del servidor de regiones de Apache HBase comienza a ocupar cerca del 200 % de la CPU, lo que provoca que las alertas se activen en el proceso de HBase Master y que el clúster no funcione al máximo de su capacidad.

## <a name="cause"></a>Causa

Si ejecuta un clúster de HBase versión 3.4, es posible que se haya encontrado con un posible error causado por la actualización de JDK a la versión 1.7.0_151. El síntoma visible es que el proceso del servidor de regiones comienza a ocupar cerca del 200 % de CPU (para comprobar esta situación ejecute el comando `top`; si hay un proceso que ocupa cerca del 200 % de CPU, obtenga su PID y confirme que es el proceso del servidor de regiones mediante `ps -aux | grep`).

## <a name="resolution"></a>Solución

1. Instale JDK 1.8 en todos los nodos del clúster como se indica a continuación:

    * Ejecute la acción de script `https://raw.githubusercontent.com/Azure/hbase-utils/master/scripts/upgradetojdk18allnodes.sh`. Asegúrese de seleccionar la opción para ejecutar en todos los nodos.

    * También puede iniciar sesión en cada nodo individual y ejecutar el comando `sudo add-apt-repository ppa:openjdk-r/ppa -y && sudo apt-get -y update && sudo apt-get install -y openjdk-8-jdk`.

1. Vaya a la interfaz de usuario de Ambari: `https://<clusterdnsname>.azurehdinsight.net`.

1. Vaya a **HBase->Configs->Advanced->Advanced`hbase-env configs`**  (HBase-> Configuración -> Avanzada -> Avanzada) y cambie la variable `JAVA_HOME` a `export JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64`. Guarde el cambio de configuración.

1. [Acción opcional pero recomendada] [Vacíe todas las tablas del clúster](https://blogs.msdn.microsoft.com/azuredatalake/2016/09/19/hdinsight-hbase-how-to-improve-hbase-cluster-restart-time-by-flushing-tables/).

1. En la interfaz de usuario de Ambari, reinicie todos los servicios de HBase que necesiten reiniciarse.

1. En función de los datos del clúster, el clúster puede tardar unos minutos o hasta una hora en alcanzar un estado estable. La forma de confirmar que el clúster alcanza el estado estable es comprobar la interfaz de usuario de HMaster (todos los servidores de regiones deben estar activos) desde Ambari (actualice) o, desde el nodo principal, ejecutar el shell de HBase y, a continuación, ejecutar el comando de estado.

Para comprobar que la actualización se realizó correctamente, compruebe que se han iniciado los procesos de HBase correspondientes con la versión de Java adecuada. Por ejemplo, compruebe el servidor de regiones como se muestra a continuación:

```
ps -aux | grep regionserver, and verify the version like '''/usr/lib/jvm/java-8-openjdk-amd64/bin/java
```

## <a name="next-steps"></a>Pasos siguientes

Si su problema no aparece o es incapaz de resolverlo, visite uno de nuestros canales para obtener ayuda adicional:

* Obtenga respuestas de expertos de Azure mediante el [soporte técnico de la comunidad de Azure](https://azure.microsoft.com/support/community/).

* Póngase en contacto con [@AzureSupport](https://twitter.com/azuresupport), la cuenta oficial de Microsoft Azure para mejorar la experiencia del cliente, que pone en contacto a la comunidad de Azure con los recursos adecuados: respuestas, soporte técnico y expertos.

* Si necesita más ayuda, puede enviar una solicitud de soporte técnico desde [Azure Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/). Seleccione **Soporte técnico** en la barra de menús o abra la central **Ayuda + soporte técnico**. Para obtener información más detallada, revise [Creación de una solicitud de soporte técnico de Azure](https://docs.microsoft.com/azure/azure-portal/supportability/how-to-create-azure-support-request). La suscripción a Microsoft Azure incluye acceso al soporte técnico para facturación y administración de suscripciones. El soporte técnico se proporciona a través de uno de los [planes de soporte técnico de Azure](https://azure.microsoft.com/support/plans/).

---
title: Problemas de latido de Apache Ambari en Azure HDInsight
description: Revisión de varias causas para los problemas de latido de Apache Ambari en Azure HDInsight
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: troubleshooting
ms.date: 02/06/2020
ms.openlocfilehash: ab88f65d535be2aef5f0b26fa1171c03276466e8
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "77057080"
---
# <a name="apache-ambari-heartbeat-issues-in-azure-hdinsight"></a>Problemas de latido de Apache Ambari en Azure HDInsight

En este artículo se describen los pasos de solución de problemas y las posibles soluciones para los problemas que se producen al usar clústeres de Azure HDInsight.

## <a name="scenario-high-cpu-utilization"></a>Escenario: Uso elevado de CPU

### <a name="issue"></a>Problema

El agente de Ambari tiene un uso elevado de CPU. Como resultado, se generan alertas de la interfaz de usuario de Ambari informando que se perdió el latido del agente de Ambari para algunos nodos. La alerta de pérdida de latido suele ser transitoria.

### <a name="cause"></a>Causa

Debido a varios errores de ambari-agent, en raras ocasiones, ambari-agent puede tener un uso elevado de CPU (cercano al 100).

### <a name="resolution"></a>Solución

1. Identifique el ID. de proceso (PID) de ambari-agent:

    ```bash
    ps -ef | grep ambari_agent
    ```

1. A continuación, ejecute el comando siguiente para mostrar el uso de la CPU:

    ```bash
    top -p <ambari-agent-pid>
    ```

1. Reinicie ambari-agent para mitigar el problema:

    ```bash
    service ambari-agent restart
    ```

1. Si el reinicio no funciona, termine el proceso ambari-agent y, a continuación, inícielo:

    ```bash
    kill -9 <ambari-agent-pid>
    service ambari-agent start
    ```

---

## <a name="scenario-ambari-agent-not-started"></a>Escenario: El agente de Ambari no se ha iniciado

### <a name="issue"></a>Problema

El agente de Ambari no se ha iniciado. Como resultado, se generan alertas de la interfaz de usuario de Ambari que informan de que se ha perdido el latido del agente de Ambari para algunos nodos.

### <a name="cause"></a>Causa

Las alertas se deben a que el agente de Ambari no se está ejecutando.

### <a name="resolution"></a>Solución

1. Confirme el estado de ambari-agent:

    ```bash
    service ambari-agent status
    ```

1. Confirme si se están ejecutando los servicios de controlador de conmutación por error:

    ```bash
    ps -ef | grep failover
    ```

    Si los servicios de controlador de conmutación por error no se están ejecutando, probablemente se deba a un problema que impide que hdinsight-agent inicie el controlador de conmutación por error. Compruebe el registro de hdinsight-agent desde el archivo `/var/log/hdinsight-agent/hdinsight-agent.out`.

## <a name="scenario-heartbeat-lost-for-ambari"></a>Escenario: Pérdida de latido para Ambari

### <a name="issue"></a>Problema

Se perdió el agente de latido de Ambari.

### <a name="cause"></a>Causa

Los registros de OMS están causando un uso elevado de la CPU.

### <a name="resolution"></a>Solución

* Deshabilite el registro de Azure Monitor mediante el cmdlet [Disable-AzHDInsightMonitoring](https://docs.microsoft.com/powershell/module/az.hdinsight/disable-azhdinsightmonitoring) de PowerShell.
* Elimine el archivo de registro `mdsd.warn`.

---

## <a name="next-steps"></a>Pasos siguientes

Si su problema no aparece o es incapaz de resolverlo, visite uno de nuestros canales para obtener ayuda adicional:

* Obtenga respuestas de expertos de Azure mediante el [soporte técnico de la comunidad de Azure](https://azure.microsoft.com/support/community/).

* Póngase en contacto con [@AzureSupport](https://twitter.com/azuresupport), la cuenta oficial de Microsoft Azure para mejorar la experiencia del cliente, que pone en contacto a la comunidad de Azure con los recursos adecuados: respuestas, soporte técnico y expertos.

* Si necesita más ayuda, puede enviar una solicitud de soporte técnico desde [Azure Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/). Seleccione **Soporte técnico** en la barra de menús o abra la central **Ayuda + soporte técnico**. Para obtener información más detallada, revise [Creación de una solicitud de soporte técnico de Azure](https://docs.microsoft.com/azure/azure-portal/supportability/how-to-create-azure-support-request). La suscripción a Microsoft Azure incluye acceso al soporte técnico para facturación y administración de suscripciones. El soporte técnico se proporciona a través de uno de los [planes de soporte técnico de Azure](https://azure.microsoft.com/support/plans/).

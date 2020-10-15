---
title: Administración del espacio en disco en Azure HDInsight
description: Pasos de solución de problemas y posibles soluciones para administrar los problemas de espacio en disco que se producen al interactuar con clústeres de Azure HDInsight.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: troubleshooting
ms.date: 02/17/2020
ms.openlocfilehash: 0628033ecf6f2e51cf18f61c6e5b36042557dc7b
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "90530168"
---
# <a name="manage-disk-space-in-azure-hdinsight"></a>Administración del espacio en disco en Azure HDInsight

En este artículo se describen los pasos de solución de problemas y las posibles soluciones para los problemas que se producen al usar clústeres de Azure HDInsight.

## <a name="hive-log-configurations"></a>Configuraciones de registro de Hive

1. En un explorador web, vaya a `https://CLUSTERNAME.azurehdinsight.net`, donde `CLUSTERNAME` es el nombre del clúster.

1. Vaya a **Hive** > **Configs** > **Advanced** > **Advanced hive-log4j** (Hive > Configuraciones > Avanzado > Advanced hive-log4j). Revise la siguiente configuración:

    * `hive.root.logger=DEBUG,RFA`. Este es el valor predeterminado; modifique el [nivel de registro](https://logging.apache.org/log4j/2.x/log4j-api/apidocs/org/apache/logging/log4j/Level.html) a `INFO` para imprimir menos entradas de registros.

    * `log4jhive.log.maxfilesize=1024MB`. Este es el valor predeterminado, modifíquelo según sea necesario.

    * `log4jhive.log.maxbackupindex=10`. Este es el valor predeterminado, modifíquelo según sea necesario. Si se ha omitido el parámetro, los archivos de registro generados serán infinitos.

## <a name="yarn-log-configurations"></a>Configuraciones de registros de YARN

Revise las configuraciones siguientes:

* Apache Ambari

    1. En un explorador web, vaya a `https://CLUSTERNAME.azurehdinsight.net`, donde `CLUSTERNAME` es el nombre del clúster.

    1. Vaya a **Hive** > **Configs** > **Advanced** > **Resource Manager** (Hive > Configuraciones > Avanzadas > Administrador de recursos). Asegúrese de que **Enable Log Aggregation** (Habilitar agregación de registros) está activada. Si está deshabilitada, los nodos de nombres mantendrán los registros localmente y no los agregarán en el almacén remoto al finalizar o terminar la aplicación.

* Asegúrese de que el tamaño del clúster sea adecuado para la carga de trabajo. Es posible que la carga de trabajo haya cambiado recientemente o que el tamaño del clúster haya cambiado. [Escale verticalmente](../hdinsight-scaling-best-practices.md) el clúster para que coincida con una carga de trabajo mayor.

* `/mnt/resource` se puede rellenar con archivos huérfanos (como en el caso del reinicio de Resource Manager). Si es necesario, limpie manualmente `/mnt/resource/hadoop/yarn/log` y `/mnt/resource/hadoop/yarn/local`.

## <a name="next-steps"></a>Pasos siguientes

Si su problema no aparece o es incapaz de resolverlo, visite uno de nuestros canales para obtener ayuda adicional:

* Obtenga respuestas de expertos de Azure mediante el [soporte técnico de la comunidad de Azure](https://azure.microsoft.com/support/community/).

* Póngase en contacto con [@AzureSupport](https://twitter.com/azuresupport), la cuenta oficial de Microsoft Azure para mejorar la experiencia del cliente. Esta cuenta pone en contacto a la comunidad de Azure con los recursos adecuados: respuestas, soporte técnico y expertos.

* Si necesita más ayuda, puede enviar una solicitud de soporte técnico desde [Azure Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/). Seleccione **Soporte técnico** en la barra de menús o abra la central **Ayuda + soporte técnico**. Para obtener información más detallada, revise [Creación de una solicitud de soporte técnico de Azure](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request). La suscripción a Microsoft Azure incluye acceso al soporte técnico para facturación y administración de suscripciones. El soporte técnico se proporciona a través de uno de los [planes de soporte técnico de Azure](https://azure.microsoft.com/support/plans/).

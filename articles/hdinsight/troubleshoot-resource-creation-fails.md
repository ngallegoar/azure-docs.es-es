---
title: Solución de problemas de creación de recursos en Azure HDInsight
description: En este artículo se indican errores comunes debidos a problemas de capacidad, así como técnicas para mitigarlos.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: troubleshooting
ms.custom: seoapr2020
ms.date: 04/22/2020
ms.openlocfilehash: 527d2d8cb8086ed6b5e87417e2bc80dd52aa6e63
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/28/2020
ms.locfileid: "82188419"
---
# <a name="troubleshoot-resource-creation-failures-in-azure-hdinsight"></a>Solución de problemas de creación de recursos en Azure HDInsight

En este artículo se describen los pasos para solucionar los problemas que se producen al interactuar con Azure HDInsight y posibles soluciones.

## <a name="error-the-deployment-would-exceed-the-quota-of-800"></a>Error: the deployment would exceed the quota of '800' (la implementación superaría la cuota de '800')

Azure tiene una cuota máxima de 800 implementaciones por grupo de recursos. Se aplican diferentes cuotas por grupo de recursos, suscripciones, cuentas u otros ámbitos. Por ejemplo, la suscripción puede configurarse para limitar el número de núcleos para una región. Al intentar implementar una máquina virtual con más núcleos de los permitidos, se genera un mensaje de error que indica que se superó la cuota.

Para resolver este problema, elimine las implementaciones que ya no son necesarias mediante Azure Portal, la CLI o PowerShell.

Para más información, consulte [Resolver errores de cuota de recursos](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-quota-errors).

## <a name="error-the-maximum-node-exceeded-the-available-cores-in-this-region"></a>Error: the maximum node exceeded the available cores in this region (el nodo máximo superó los núcleos disponibles en esta región)

La suscripción puede configurarse para limitar el número de núcleos para una región. Al intentar implementar un recurso con más núcleos de los permitidos, se genera un mensaje de error que indica que se superó la cuota.

Para solicitar un aumento de la cuota, siga estos pasos:

1. Vaya a [Azure Portal](https://portal.azure.com) y seleccione **Ayuda y soporte técnico**.

1. Seleccione **Nueva solicitud de soporte técnico**.

1. En la página **Nueva solicitud de soporte técnico**, en la pestaña **Datos básicos**, proporcione la información siguiente:

   * **Tipo de problema**: Seleccione **Límites de servicio y suscripción (cuotas)** .
   * **Subscription** (Suscripción): Seleccione la suscripción que desea modificar.
   * **Tipo de cuota**: Seleccione **HDInsight**.

Para más información, consulte [Creación de una incidencia de soporte técnico para aumentar el núcleo](hdinsight-capacity-planning.md#quotas).

## <a name="next-steps"></a>Pasos siguientes

Si su problema no aparece o es incapaz de resolverlo, visite uno de nuestros canales para obtener ayuda adicional:

* Obtenga respuestas de expertos de Azure mediante el [soporte técnico de la comunidad de Azure](https://azure.microsoft.com/support/community/).

* Póngase en contacto con [@AzureSupport](https://twitter.com/azuresupport), la cuenta oficial de Microsoft Azure para mejorar la experiencia del cliente. Esta cuenta pone en contacto a la comunidad de Azure con los recursos adecuados: respuestas, soporte técnico y expertos.

* Si necesita más ayuda, puede enviar una solicitud de soporte técnico desde [Azure Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/). Seleccione **Soporte técnico** en la barra de menús o abra la central **Ayuda + soporte técnico**. Para obtener información más detallada, revise [Creación de una solicitud de soporte técnico de Azure](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request). La suscripción a Microsoft Azure incluye acceso al soporte técnico para facturación y administración de suscripciones. El soporte técnico se proporciona a través de uno de los [planes de soporte técnico de Azure](https://azure.microsoft.com/support/plans/).

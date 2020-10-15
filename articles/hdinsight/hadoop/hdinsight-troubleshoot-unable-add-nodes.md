---
title: No se pueden agregar nodos a un clúster Azure HDInsight
description: Solución de los problemas por los que no se pueden agregar nodos al clúster de Apache Hadoop en Azure HDInsight
ms.service: hdinsight
ms.topic: troubleshooting
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.date: 07/31/2019
ms.openlocfilehash: 97d7f34fff324a9959292460e534c15110c3e532
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "75894093"
---
# <a name="scenario-unable-to-add-nodes-to-azure-hdinsight-cluster"></a>Escenario: No se pueden agregar nodos a un clúster Azure HDInsight

En este artículo se describen los pasos de solución de problemas y las posibles soluciones para los problemas que se producen al usar clústeres de Azure HDInsight.

## <a name="issue"></a>Problema

No se pueden agregar nodos al clúster de Azure HDInsight.

## <a name="cause"></a>Causa

Los motivos pueden variar.

## <a name="resolution"></a>Solución

Mediante la característica [Tamaño del clúster](../hdinsight-scaling-best-practices.md), calcule el número de núcleos adicionales necesarios para el clúster. La cifra se basa en el número total de núcleos de los nuevos nodos de trabajo. Luego, pruebe uno o varios de los pasos siguientes:

* Compruebe si hay algún núcleo disponible en la ubicación del clúster.

* Compruebe los núcleos disponibles en otras ubicaciones. Considere la posibilidad de volver a crear el clúster en otra ubicación con suficientes núcleos disponibles.

* Si desea aumentar la cuota de núcleos para una determinada ubicación, registre una incidencia de soporte técnico para solicitar el aumento de la cuota de núcleos de HDInsight.

## <a name="next-steps"></a>Pasos siguientes

Si su problema no aparece o es incapaz de resolverlo, visite uno de nuestros canales para obtener ayuda adicional:

* Obtenga respuestas de expertos de Azure mediante el [soporte técnico de la comunidad de Azure](https://azure.microsoft.com/support/community/).

* Póngase en contacto con [@AzureSupport](https://twitter.com/azuresupport), la cuenta oficial de Microsoft Azure para mejorar la experiencia del cliente, que pone en contacto a la comunidad de Azure con los recursos adecuados: respuestas, soporte técnico y expertos.

* Si necesita más ayuda, puede enviar una solicitud de soporte técnico desde [Azure Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/). Seleccione **Soporte técnico** en la barra de menús o abra la central **Ayuda + soporte técnico**. Para obtener información más detallada, revise [Creación de una solicitud de soporte técnico de Azure](https://docs.microsoft.com/azure/azure-portal/supportability/how-to-create-azure-support-request). La suscripción a Microsoft Azure incluye acceso al soporte técnico para facturación y administración de suscripciones. El soporte técnico se proporciona a través de uno de los [planes de soporte técnico de Azure](https://azure.microsoft.com/support/plans/).

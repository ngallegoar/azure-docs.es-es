---
title: 'Error de InvalidNetworkSecurityGroupSecurityRules: Azure HDInsight'
description: Error de creación del clúster con el código de error InvalidNetworkSecurityGroupSecurityRules
ms.service: hdinsight
ms.topic: troubleshooting
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.date: 07/31/2019
ms.openlocfilehash: 2cf4859d3bf4c34fff4cb076eec11bcd2d81e4ab
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "82780783"
---
# <a name="scenario-invalidnetworksecuritygroupsecurityrules---cluster-creation-fails-in-azure-hdinsight"></a>Escenario: InvalidNetworkSecurityGroupSecurityRules: error de creación del clúster en Azure HDInsight

En este artículo se describen los pasos de solución de problemas y las posibles soluciones para los problemas que se producen al usar clústeres de Azure HDInsight.

## <a name="issue"></a>Problema

Recibirá el código de error `InvalidNetworkSecurityGroupSecurityRules` con una descripción similar a esta: "Las reglas de seguridad en el grupo de seguridad de red configurado con subred no permiten la conectividad entrante o saliente requerida".

## <a name="cause"></a>Causa

Es probable que haya un problema con las reglas del [grupo de seguridad de red](../../virtual-network/virtual-network-vnet-plan-design-arm.md) entrantes configuradas para el clúster.

## <a name="resolution"></a>Solución

Vaya a Azure Portal e identifique el grupo de seguridad de red que está asociado a la subred donde se va a implementar el clúster. En la sección **Reglas de seguridad de entrada**, asegúrese de que las reglas permiten el acceso entrante al puerto 443 para las direcciones IP que se mencionan [aquí](../control-network-traffic.md).

## <a name="next-steps"></a>Pasos siguientes

Si su problema no aparece o es incapaz de resolverlo, visite uno de nuestros canales para obtener ayuda adicional:

* Obtenga respuestas de expertos de Azure mediante el [soporte técnico de la comunidad de Azure](https://azure.microsoft.com/support/community/).

* Póngase en contacto con [@AzureSupport](https://twitter.com/azuresupport), la cuenta oficial de Microsoft Azure para mejorar la experiencia del cliente, que pone en contacto a la comunidad de Azure con los recursos adecuados: respuestas, soporte técnico y expertos.

* Si necesita más ayuda, puede enviar una solicitud de soporte técnico desde [Azure Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/). Seleccione **Soporte técnico** en la barra de menús o abra la central **Ayuda + soporte técnico**. Para obtener información más detallada, revise [Creación de una solicitud de soporte técnico de Azure](https://docs.microsoft.com/azure/azure-portal/supportability/how-to-create-azure-support-request). La suscripción a Microsoft Azure incluye acceso al soporte técnico para facturación y administración de suscripciones. El soporte técnico se proporciona a través de uno de los [planes de soporte técnico de Azure](https://azure.microsoft.com/support/plans/).

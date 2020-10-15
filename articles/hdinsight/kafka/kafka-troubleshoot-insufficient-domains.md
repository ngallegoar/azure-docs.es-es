---
title: No hay suficientes dominios de error en el error de la región de Azure HDInsight
description: No se pudo crear el clúster debido a que no hay suficientes dominios de error en la región de Azure HDInsight
ms.service: hdinsight
ms.topic: troubleshooting
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.date: 08/09/2019
ms.openlocfilehash: 8be7e05ac85ce0b1b10edf18d3885a07e016b9ae
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "75895025"
---
# <a name="scenario-cluster-creation-failed-due-to-not-sufficient-fault-domains-in-region-in-azure-hdinsight"></a>Escenario: No se pudo crear el clúster debido a `not sufficient fault domains in region` en Azure HDInsight

En este artículo se describen los pasos de solución de problemas y las posibles soluciones para los problemas que se producen al usar clústeres de Azure HDInsight.

## <a name="issue"></a>Problema

Recibo un mensaje de error parecido a `not sufficient fault domains in region` cuando intento crear un clúster de Apache Kafka.

## <a name="cause"></a>Causa

Un dominio de error es una agrupación lógica del hardware subyacente en un centro de datos de Azure. Todos los dominios de error comparten la fuente de energía y el conmutador de red. Las máquinas virtuales y los discos administrados que implementan los nodos en un clúster de HDInsight se distribuyen por estos dominios de error. Esta arquitectura limita el impacto potencial de errores del hardware físico.

Cada región de Azure tiene un número concreto de dominios de error. Para obtener una lista de los dominios y el número de dominios de error que contienen, consulte la documentación sobre los [conjuntos de disponibilidad](../../virtual-machines/windows/manage-availability.md).

En HDInsight, los clústeres de Kafka se deben aprovisionar en una región con al menos tres dominios de error.

## <a name="resolution"></a>Solución

Si la región en la que desea crear el clúster no tiene suficientes dominios de error, póngase en contacto con el equipo del producto para permitir el aprovisionamiento del clúster, incluso aunque no haya tres dominios de error.

## <a name="next-steps"></a>Pasos siguientes

Si su problema no aparece o es incapaz de resolverlo, visite uno de nuestros canales para obtener ayuda adicional:

* Obtenga respuestas de expertos de Azure mediante el [soporte técnico de la comunidad de Azure](https://azure.microsoft.com/support/community/).

* Póngase en contacto con [@AzureSupport](https://twitter.com/azuresupport), la cuenta oficial de Microsoft Azure para mejorar la experiencia del cliente. Esta cuenta pone en contacto a la comunidad de Azure con los recursos adecuados: respuestas, soporte técnico y expertos.

* Si necesita más ayuda, puede enviar una solicitud de soporte técnico desde [Azure Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/). Seleccione **Soporte técnico** en la barra de menús o abra la central **Ayuda + soporte técnico**. Para obtener información más detallada, revise [Creación de una solicitud de soporte técnico de Azure](https://docs.microsoft.com/azure/azure-portal/supportability/how-to-create-azure-support-request). La suscripción a Microsoft Azure incluye acceso al soporte técnico para facturación y administración de suscripciones. El soporte técnico se proporciona a través de uno de los [planes de soporte técnico de Azure](https://azure.microsoft.com/support/plans/).

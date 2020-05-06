---
title: API REST para consultar de Apache HBase en Azure HDInsight
description: ''
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: troubleshooting
ms.date: 04/08/2020
ms.openlocfilehash: 3bf63aa08ec4c1deff2551cfcc0cf188a75261bc
ms.sourcegitcommit: eaec2e7482fc05f0cac8597665bfceb94f7e390f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/29/2020
ms.locfileid: "82515974"
---
# <a name="rest-api-to-query-apache-hbase-in-azure-hdinsight"></a>API REST para consultar de Apache HBase en Azure HDInsight

En este artículo se describen los pasos de solución de problemas y las posibles soluciones para los problemas que se producen al usar clústeres de Azure HDInsight.

## <a name="issue"></a>Problema

El uso de la interfaz de REST Apache HBase para consultar una tabla en un espacio de nombres distinto del predeterminado produce un error en tiempo de ejecución (estado HTTP 500).

## <a name="cause"></a>Causa

La API REST de HBase solo se admite cuando se usa el espacio de nombres predeterminado. Se trata de un problema conocido relacionado con el uso de espacios de nombres de HBase o con la realización de llamadas que hacen referencia a operaciones GET en columnas específicas con familias de columnas con el servidor REST en HDInsight. Esto se debe a un problema de seguridad con la puerta de enlace de HDInsight. Cuando se usa la API para crear una tabla con un espacio de nombres, que tiene acceso a las columnas a través de familias de columnas, debe especificar el carácter `:`, lo que se considera un problema de seguridad en el módulo IIS Gateway.

## <a name="mitigation"></a>Mitigación

Para omitir el servidor de puerta de enlace o REST, implemente la aplicación en una máquina virtual que se encuentre en la misma red virtual de Azure que el clúster de HDInsight. Después, puede comunicarse con HBase ya sea directamente a través de RPC (omitiendo completamente el servidor REST) o con servidores REST individuales que se ejecuten en nodos de trabajo que omitan las puertas de enlace de HDInsight.

## <a name="next-steps"></a>Pasos siguientes

Si su problema no aparece o es incapaz de resolverlo, visite uno de nuestros canales para obtener ayuda adicional:

* Obtenga respuestas de expertos de Azure mediante el [soporte técnico de la comunidad de Azure](https://azure.microsoft.com/support/community/).

* Póngase en contacto con [@AzureSupport](https://twitter.com/azuresupport), la cuenta oficial de Microsoft Azure para mejorar la experiencia del cliente. Esta cuenta pone en contacto a la comunidad de Azure con los recursos adecuados: respuestas, soporte técnico y expertos.

* Si necesita más ayuda, puede enviar una solicitud de soporte técnico desde [Azure Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/). Seleccione **Soporte técnico** en la barra de menús o abra la central **Ayuda + soporte técnico**. Para obtener información más detallada, revise [Creación de una solicitud de soporte técnico de Azure](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request). La suscripción a Microsoft Azure incluye acceso al soporte técnico para facturación y administración de suscripciones. El soporte técnico se proporciona a través de uno de los [planes de soporte técnico de Azure](https://azure.microsoft.com/support/plans/).

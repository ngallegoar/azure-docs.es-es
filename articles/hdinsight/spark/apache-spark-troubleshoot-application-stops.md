---
title: La aplicación Apache Spark Streaming se detiene después de 24 días en Azure HDInsight
description: Una aplicación Apache Spark Streaming se detiene después de ejecutarse durante 24 días y no hay errores en los archivos de registro.
ms.service: hdinsight
ms.topic: troubleshooting
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.date: 07/29/2019
ms.openlocfilehash: ff410ea1b6c54d2f58babeb20c68fe95033e9728
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "75894433"
---
# <a name="scenario-apache-spark-streaming-application-stops-after-executing-for-24-days-in-azure-hdinsight"></a>Escenario: La aplicación Apache Spark Streaming se detiene después de ejecutarse durante 24 días en Azure HDInsight

En este artículo se describen los pasos de solución de problemas y las posibles soluciones para los problemas que se producen al usar componentes de Apache Spark en clústeres de Azure HDInsight.

## <a name="issue"></a>Problema

Una aplicación Apache Spark Streaming se detiene después de ejecutarse durante 24 días y no hay errores en los archivos de registro.

## <a name="cause"></a>Causa

El valor `livy.server.session.timeout` controla el tiempo que Apache Livy debe esperar hasta que se complete una sesión. Cuando la duración de la sesión alcanza el valor de `session.timeout`, la sesión de Livy y la aplicación se terminan automáticamente.

## <a name="resolution"></a>Solución

Para los trabajos de larga duración, aumente el valor `livy.server.session.timeout` con la interfaz de usuario de Ambari. Puede acceder a la configuración de Livy desde la interfaz de usuario de Ambari mediante la dirección URL `https://<yourclustername>.azurehdinsight.net/#/main/services/LIVY/configs`.

Reemplace `<yourclustername>` por el nombre de su clúster de HDInsight que se muestra en el portal.

## <a name="next-steps"></a>Pasos siguientes

Si su problema no aparece o es incapaz de resolverlo, visite uno de nuestros canales para obtener ayuda adicional:

* Obtenga respuestas de expertos de Azure mediante el [soporte técnico de la comunidad de Azure](https://azure.microsoft.com/support/community/).

* Póngase en contacto con [@AzureSupport](https://twitter.com/azuresupport), la cuenta oficial de Microsoft Azure para mejorar la experiencia del cliente, que pone en contacto a la comunidad de Azure con los recursos adecuados: respuestas, soporte técnico y expertos.

* Si necesita más ayuda, puede enviar una solicitud de soporte técnico desde [Azure Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/). Seleccione **Soporte técnico** en la barra de menús o abra la central **Ayuda + soporte técnico**. Para obtener información más detallada, revise [Creación de una solicitud de soporte técnico de Azure](https://docs.microsoft.com/azure/azure-portal/supportability/how-to-create-azure-support-request). La suscripción a Microsoft Azure incluye acceso al soporte técnico para facturación y administración de suscripciones. El soporte técnico se proporciona a través de uno de los [planes de soporte técnico de Azure](https://azure.microsoft.com/support/plans/).

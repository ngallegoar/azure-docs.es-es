---
title: 'Error de IllegalArgumentException para Apache Spark: Azure HDInsight'
description: IllegalArgumentException para una actividad de Apache Spark en Azure HDInsight para Azure Data Factory
ms.service: hdinsight
ms.topic: troubleshooting
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.date: 07/29/2019
ms.openlocfilehash: df62dbd8db7d41eb11207c7741aed76cec0ac7a8
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "75894378"
---
# <a name="scenario-illegalargumentexception-for-apache-spark-activity-in-azure-hdinsight"></a>Escenario: IllegalArgumentException para una actividad de Apache Spark en Azure HDInsight

En este artículo se describen los pasos de solución de problemas y las posibles soluciones para los problemas que se producen al usar componentes de Apache Spark en clústeres de Azure HDInsight.

## <a name="issue"></a>Problema

Al intentar ejecutar una actividad de Spark en una canalización de Azure Data Factory, recibirá la excepción siguiente:

```error
Exception in thread "main" java.lang.IllegalArgumentException:
Wrong FS: wasbs://additional@xxx.blob.core.windows.net/spark-examples_2.11-2.1.0.jar, expected: wasbs://wasbsrepro-2017-11-07t00-59-42-722z@xxx.blob.core.windows.net
```

## <a name="cause"></a>Causa

Se producirá un error en un trabajo de Spark si el archivo jar de la aplicación no se encuentra en el almacenamiento principal o predeterminado del clúster de Spark.

Este es un problema conocido con el marco de código abierto de Spark, y está registrado en este error: [Se produce un error en el trabajo de Spark si fs.defaultFS y el jar de la aplicación tienen direcciones URL diferentes](https://issues.apache.org/jira/browse/SPARK-22587).

Este problema se resolvió en Spark 2.3.0.

## <a name="resolution"></a>Solución

Asegúrese de que el jar de la aplicación está almacenado en el almacenamiento principal o predeterminado para el clúster de HDInsight. En el caso de Azure Data Factory, asegúrese de que el servicio vinculado ADF apunta al contenedor predeterminado de HDInsight en lugar de a un contenedor secundario.

## <a name="next-steps"></a>Pasos siguientes

Si su problema no aparece o es incapaz de resolverlo, visite uno de nuestros canales para obtener ayuda adicional:

* Obtenga respuestas de expertos de Azure mediante el [soporte técnico de la comunidad de Azure](https://azure.microsoft.com/support/community/).

* Póngase en contacto con [@AzureSupport](https://twitter.com/azuresupport), la cuenta oficial de Microsoft Azure para mejorar la experiencia del cliente, que pone en contacto a la comunidad de Azure con los recursos adecuados: respuestas, soporte técnico y expertos.

* Si necesita más ayuda, puede enviar una solicitud de soporte técnico desde [Azure Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/). Seleccione **Soporte técnico** en la barra de menús o abra la central **Ayuda + soporte técnico**. Para obtener información más detallada, revise [Creación de una solicitud de soporte técnico de Azure](https://docs.microsoft.com/azure/azure-portal/supportability/how-to-create-azure-support-request). La suscripción a Microsoft Azure incluye acceso al soporte técnico para facturación y administración de suscripciones. El soporte técnico se proporciona a través de uno de los [planes de soporte técnico de Azure](https://azure.microsoft.com/support/plans/).

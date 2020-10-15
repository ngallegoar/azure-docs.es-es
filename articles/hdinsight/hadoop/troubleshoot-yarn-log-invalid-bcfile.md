---
title: No se pueden leer los registros de Apache Yarn en Azure HDInsight
description: Pasos de solución de problemas y soluciones posibles para problemas que se producen al interactuar con clústeres de Azure HDInsight.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: troubleshooting
ms.date: 01/23/2020
ms.openlocfilehash: f7861ae4980f13fbd87780ab2d26fa3376f33532
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "76776041"
---
# <a name="scenario-unable-to-read-apache-yarn-log-in-azure-hdinsight"></a>Escenario: No se pueden leer los registros de Apache Yarn en Azure HDInsight

En este artículo se describen los pasos de solución de problemas y las posibles soluciones para los problemas que se producen al usar clústeres de Azure HDInsight.

## <a name="issue"></a>Problema

Los registros de Apache Yarn que se encuentran en la cuenta de almacenamiento no son legibles para el usuario. El analizador de archivos no funciona y genera el siguiente mensaje de error:

```
java.io.IOException: Not a valid BCFile.
```

## <a name="cause"></a>Causa

El registro de Apache Yarn se ha agregado en formato `IndexFile`, que no es compatible con el analizador de archivos.

## <a name="resolution"></a>Solución

1. En un explorador web, vaya a `https://CLUSTERNAME.azurehdinsight.net`, donde `CLUSTERNAME` es el nombre del clúster.

1. En la interfaz de usuario de Ambari, vaya a **YARN** > **Configs** > **Advanced** > **Advanced yarn-site** (YARN > Configuraciones > Avanzado > Yarn-site avanzado).

1. Para el almacenamiento en Azure Storage Blob: El valor predeterminado de `yarn.log-aggregation.file-formats` es `IndexedFormat,TFile`. Cambie el valor a `TFile`.

1. Para Azure Data Lake Storage: El valor predeterminado de `yarn.nodemanager.log-aggregation.compression-type` es `gz`. Cambie el valor a `none`.

1. Guarde los cambios y reinicie todos los servicios afectados.

## <a name="next-steps"></a>Pasos siguientes

Si su problema no aparece o es incapaz de resolverlo, visite uno de nuestros canales para obtener ayuda adicional:

* Obtenga respuestas de expertos de Azure mediante el [soporte técnico de la comunidad de Azure](https://azure.microsoft.com/support/community/).

* Póngase en contacto con [@AzureSupport](https://twitter.com/azuresupport), la cuenta oficial de Microsoft Azure para mejorar la experiencia del cliente. Esta cuenta pone en contacto a la comunidad de Azure con los recursos adecuados: respuestas, soporte técnico y expertos.

* Si necesita más ayuda, puede enviar una solicitud de soporte técnico desde [Azure Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/). Seleccione **Soporte técnico** en la barra de menús o abra la central **Ayuda + soporte técnico**. Para obtener información más detallada, revise [Creación de una solicitud de soporte técnico de Azure](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request). La suscripción a Microsoft Azure incluye acceso al soporte técnico para facturación y administración de suscripciones. El soporte técnico se proporciona a través de uno de los [planes de soporte técnico de Azure](https://azure.microsoft.com/support/plans/).

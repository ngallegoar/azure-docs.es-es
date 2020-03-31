---
title: La cuenta a la que se está accediendo no es compatible con los errores http en Azure HDInsight
description: En este artículo se describen los pasos de solución de problemas y las posibles soluciones para los problemas que se producen al usar clústeres de Azure HDInsight.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: troubleshooting
ms.date: 02/06/2020
ms.openlocfilehash: 14c43e4557275d6a425127acfee7495f68d1d354
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "77163833"
---
# <a name="the-account-being-accessed-does-not-support-http-error-in-azure-hdinsight"></a>La cuenta a la que se está accediendo no es compatible con los errores http en Azure HDInsight

En este artículo se describen los pasos de solución de problemas y las posibles soluciones para los problemas que se producen al usar clústeres de Azure HDInsight.

## <a name="issue"></a>Problema

Se recibe un mensaje de error que indica lo siguiente:

```
com.microsoft.azure.storage.StorageException: The account being accessed does not support http.
```

## <a name="cause"></a>Causa

Hay varias razones por las que se recibe este mensaje de error:

* La cuenta de almacenamiento tiene habilitada la [transferencia segura](../../storage/common/storage-require-secure-transfer.md) y se está usando el [esquema de URI](../hdinsight-hadoop-linux-information.md#URI-and-scheme) incorrecto.

* Se creó un clúster con una cuenta de almacenamiento que tenía *deshabilitada* la transferencia segura. A partir de ese momento, la transferencia segura se habilitó en la cuenta de almacenamiento.

## <a name="resolution"></a>Solución

Si la transferencia segura está habilitada para Azure Storage o Data Lake Storage Gen2, el URI será `wasbs://` o `abfss://`, respectivamente.  Consulte también el artículo acerca de la [transferencia segura](../../storage/common/storage-require-secure-transfer.md).

En el caso de los clústeres nuevos, use una cuenta de almacenamiento que ya tenga la configuración de transferencia segura deseada. No cambie la configuración de transferencia segura para una cuenta de almacenamiento que un clúster existente esté usando.

## <a name="next-steps"></a>Pasos siguientes

Si su problema no aparece o es incapaz de resolverlo, visite uno de nuestros canales para obtener ayuda adicional:

* Obtenga respuestas de expertos de Azure mediante el [soporte técnico de la comunidad de Azure](https://azure.microsoft.com/support/community/).

* Póngase en contacto con [@AzureSupport](https://twitter.com/azuresupport), la cuenta oficial de Microsoft Azure para mejorar la experiencia del cliente. Esta cuenta pone en contacto a la comunidad de Azure con los recursos adecuados: respuestas, soporte técnico y expertos.

* Si necesita más ayuda, puede enviar una solicitud de soporte técnico desde [Azure Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/). Seleccione **Soporte técnico** en la barra de menús o abra la central **Ayuda + soporte técnico**. Para obtener información más detallada, revise [Creación de una solicitud de soporte técnico de Azure](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request). La suscripción a Microsoft Azure incluye acceso al soporte técnico para facturación y administración de suscripciones. El soporte técnico se proporciona a través de uno de los [planes de soporte técnico de Azure](https://azure.microsoft.com/support/plans/).

---
title: Excepción de almacenamiento tras el restablecimiento de la conexión en Azure HDInsight
description: Excepción de almacenamiento tras el restablecimiento de la conexión en Azure HDInsight
ms.service: hdinsight
ms.topic: troubleshooting
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.date: 08/08/2019
ms.openlocfilehash: a7af6407191577112f936bfb9048985e85c868ea
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "75887230"
---
# <a name="scenario-storage-exception-after-connection-reset-in-azure-hdinsight"></a>Escenario: Excepción de almacenamiento tras el restablecimiento de la conexión en Azure HDInsight

En este artículo se describen los pasos de solución de problemas y las posibles soluciones para los problemas que se producen al usar clústeres de Azure HDInsight.

## <a name="issue"></a>Problema

No se puede crear otra tabla de Apache HBase.

## <a name="cause"></a>Causa

Durante un proceso de truncamiento de tabla, ha habido un problema de conexión de almacenamiento. La entrada de tabla se ha eliminado de la tabla de metadatos de HBase. Se han eliminado todos los archivos de blob menos uno.

Aunque no había ninguna carpeta de blobs llamada `/hbase/data/default/ThatTable` en el almacén. El controlador WASB ha detectado la existencia del archivo blob anterior y no ha permitido crear ningún blob llamado `/hbase/data/default/ThatTable` porque ha dado por hecho que las carpetas principales existen, por lo que se producirá un error al crear la tabla.

## <a name="resolution"></a>Solución

1. En la interfaz de usuario de Apache Ambari, reinicie el HMaster activo. Esto permitirá que uno de los dos HMaster en espera se convierta en el activo, y el nuevo HMaster activo volverá a cargar la información de la tabla de metadatos. Por lo tanto, no verá la tabla `already-deleted` en la interfaz de usuario de HMaster.

1. Puede encontrar el archivo de blob huérfano con herramientas de la interfaz de usuario como Cloud Explorer o ejecutar un comando como `hdfs dfs -ls /xxxxxx/yyyyy`. Ejecute `hdfs dfs -rmr /xxxxx/yyyy` para eliminar ese blob. Por ejemplo, `hdfs dfs -rmr /hbase/data/default/ThatTable/ThatFile`.

Ahora puede crear una tabla con el mismo nombre en HBase.

## <a name="next-steps"></a>Pasos siguientes

Si su problema no aparece o es incapaz de resolverlo, visite uno de nuestros canales para obtener ayuda adicional:

* Obtenga respuestas de expertos de Azure mediante el [soporte técnico de la comunidad de Azure](https://azure.microsoft.com/support/community/).

* Póngase en contacto con [@AzureSupport](https://twitter.com/azuresupport), la cuenta oficial de Microsoft Azure para mejorar la experiencia del cliente. Esta cuenta pone en contacto a la comunidad de Azure con los recursos adecuados: respuestas, soporte técnico y expertos.

* Si necesita más ayuda, puede enviar una solicitud de soporte técnico desde [Azure Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/). Seleccione **Soporte técnico** en la barra de menús o abra la central **Ayuda + soporte técnico**. Para obtener información más detallada, revise [Creación de una solicitud de soporte técnico de Azure](https://docs.microsoft.com/azure/azure-portal/supportability/how-to-create-azure-support-request). La suscripción a Microsoft Azure incluye acceso al soporte técnico para facturación y administración de suscripciones. El soporte técnico se proporciona a través de uno de los [planes de soporte técnico de Azure](https://azure.microsoft.com/support/plans/).

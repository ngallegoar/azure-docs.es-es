---
title: Depuración de operaciones de archivo WASB en Azure HDInsight
description: Describe los pasos de solución de problemas y las posibles soluciones para los problemas que se producen al interactuar con clústeres de Azure HDInsight.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: troubleshooting
ms.date: 02/18/2020
ms.openlocfilehash: 85e974d51f49bbb6742683ed253c077bb3ff69de
ms.sourcegitcommit: d767156543e16e816fc8a0c3777f033d649ffd3c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/26/2020
ms.locfileid: "92545555"
---
# <a name="debug-wasb-file-operations-in-azure-hdinsight"></a>Depuración de operaciones de archivo WASB en Azure HDInsight

En ocasiones querrá saber qué operaciones ha iniciado el controlador WASB con Azure Storage. En el lado del cliente, el controlador WASB genera registros para cada operación del sistema de archivos en el nivel **DEBUG** . El controlador WASB usa log4j para controlar el nivel de registro, y el valor predeterminado es el nivel **INFO** . Para obtener registros de análisis del lado del servidor de Azure Storage, consulte [Registro de análisis de Azure Storage](../../storage/common/storage-analytics-logging.md).

Los registros generados tendrán un aspecto similar al siguiente:

```log
18/05/13 04:15:55 DEBUG NativeAzureFileSystem: Moving wasb://xxx@yyy.blob.core.windows.net/user/livy/ulysses.txt/_temporary/0/_temporary/attempt_20180513041552_0000_m_000000_0/part-00000 to wasb://xxx@yyy.blob.core.windows.net/user/livy/ulysses.txt/part-00000
```

## <a name="turn-on-wasb-debug-log-for-file-operations"></a>Activación del registro de depuración de WASB para operaciones de archivo

1. En un explorador web, vaya a `https://CLUSTERNAME.azurehdinsight.net/#/main/services/SPARK2/configs`, donde `CLUSTERNAME` es el nombre del clúster de Spark.

1. Vaya a **advanced spark2-log4j-properties** .

    1. Cambie `log4j.appender.console.Threshold=INFO` por `log4j.appender.console.Threshold=DEBUG`.

    1. Agregue `log4j.logger.org.apache.hadoop.fs.azure.NativeAzureFileSystem=DEBUG`.

1. Vaya a **Advanced livy2-log4j-properties** .

    Agregue `log4j.logger.org.apache.hadoop.fs.azure.NativeAzureFileSystem=DEBUG`.

1. Guarde los cambios.

## <a name="additional-logging"></a>Registro adicional

Los registros anteriores proporcionarán una descripción general de las operaciones del sistema de archivos. Si los registros anteriores no proporcionan información útil, o si desea investigar las llamadas API de Blob Storage, agregue `fs.azure.storage.client.logging=true` a `core-site`. Esta opción de configuración habilitará los registros del SDK de Java para el controlador de almacenamiento de WASB e imprimirá cada llamada en el servidor de Blob Storage. Quite la opción de configuración después de terminar las investigaciones porque podría llenar el disco rápidamente y ralentizar el proceso.

Si el back-end se basa en Azure Data Lake, use la siguiente configuración de log4j para el componente (por ejemplo, spark/tez/hdfs):

```
log4j.logger.com.microsoft.azure.datalake.store=ALL,adlsFile
log4j.additivity.com.microsoft.azure.datalake.store=true
log4j.appender.adlsFile=org.apache.log4j.FileAppender
log4j.appender.adlsFile.File=/var/log/adl/adl.log
log4j.appender.adlsFile.layout=org.apache.log4j.PatternLayout
log4j.appender.adlsFile.layout.ConversionPattern=%p\t%d{ISO8601}\t%r\t%c\t[%t]\t%m%n
```

Busque los registros en `/var/log/adl/adl.log`.

## <a name="next-steps"></a>Pasos siguientes

Si su problema no aparece o es incapaz de resolverlo, visite uno de nuestros canales para obtener ayuda adicional:

* Obtenga respuestas de expertos de Azure mediante el [soporte técnico de la comunidad de Azure](https://azure.microsoft.com/support/community/).

* Póngase en contacto con [@AzureSupport](https://twitter.com/azuresupport), la cuenta oficial de Microsoft Azure para mejorar la experiencia del cliente. Esta cuenta pone en contacto a la comunidad de Azure con los recursos adecuados: respuestas, soporte técnico y expertos.

* Si necesita más ayuda, puede enviar una solicitud de soporte técnico desde [Azure Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/). Seleccione **Soporte técnico** en la barra de menús o abra la central **Ayuda + soporte técnico** . Para obtener información más detallada, revise [Creación de una solicitud de soporte técnico de Azure](../../azure-portal/supportability/how-to-create-azure-support-request.md). La suscripción a Microsoft Azure incluye acceso al soporte técnico para facturación y administración de suscripciones. El soporte técnico se proporciona a través de uno de los [planes de soporte técnico de Azure](https://azure.microsoft.com/support/plans/).
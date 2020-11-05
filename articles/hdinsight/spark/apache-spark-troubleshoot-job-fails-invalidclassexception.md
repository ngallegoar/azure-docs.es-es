---
title: 'Error InvalidClassException en Apache Spark: Azure HDInsight'
description: Error del trabajo de Apache Spark con InvalidClassException, la versión de la clase no coincide, en Azure HDInsight
ms.service: hdinsight
ms.topic: troubleshooting
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.date: 07/29/2019
ms.openlocfilehash: 6220c328d05e7cd68460b7bfd0708a9d393a290f
ms.sourcegitcommit: 7863fcea618b0342b7c91ae345aa099114205b03
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/03/2020
ms.locfileid: "93287884"
---
# <a name="apache-spark-job-fails-with-invalidclassexception-class-version-mismatch-in-azure-hdinsight"></a>Error del trabajo de Apache Spark con InvalidClassException, la versión de la clase no coincide, en Azure HDInsight

En este artículo se describen los pasos de solución de problemas y las posibles soluciones para los problemas que se producen al usar componentes de Apache Spark en clústeres de Azure HDInsight.

## <a name="issue"></a>Problema

Intenta crear un trabajo de Apache Spark en un clúster de Spark 2. x. Se produce un error similar al siguiente:

```
18/09/18 09:32:26 WARN TaskSetManager: Lost task 0.0 in stage 1.0 (TID 1, wn7-dev-co.2zyfbddadfih0xdq0cdja4g.ax.internal.cloudapp.net, executor 4): java.io.InvalidClassException:
org.apache.commons.lang3.time.FastDateFormat; local class incompatible: stream classdesc serialVersionUID = 2, local class serialVersionUID = 1
        at java.io.ObjectStreamClass.initNonProxy(ObjectStreamClass.java:699)
        at java.io.ObjectInputStream.readNonProxyDesc(ObjectInputStream.java:1885)
        at java.io.ObjectInputStream.readClassDesc(ObjectInputStream.java:1751)
        at java.io.ObjectInputStream.readOrdinaryObject(ObjectInputStream.java:2042)
        at java.io.ObjectInputStream.readObject0(ObjectInputStream.java:1573)
```

## <a name="cause"></a>Causa

Este error puede deberse a la incorporación de un archivo jar adicional a la configuración `spark.yarn.jars`, especialmente si se trata de un archivo jar "sombreado" que incluye una versión diferente del paquete `commons-lang3` e introduce una discordancia de clase. De forma predeterminada, Spark 2.1/2/3 usa la versión 3,5 de `commons-lang3`.

> [!TIP]
> Para sombrear una biblioteca, debe incluir el contenido en un archivo jar propio y cambiar el paquete. Este procedimiento es diferente al del empaquetamiento de la biblioteca, que incluye la biblioteca en un archivo jar propio sin volver a empaquetar.

## <a name="resolution"></a>Solución

Quite el archivo jar o vuelva a compilar el archivo jar personalizado (AzureLogAppender) y use [maven-shade-plugin](https://maven.apache.org/plugins/maven-shade-plugin/examples/class-relocation.html) para reubicar las clases.

## <a name="next-steps"></a>Pasos siguientes

[!INCLUDE [troubleshooting next steps](../../../includes/hdinsight-troubleshooting-next-steps.md)]
---
title: Error de permiso denegado con una tabla de Apache Hive en Azure HDInsight
description: Error de permiso denegado al intentar crear una tabla de Apache Hive en Azure HDInsight
ms.service: hdinsight
ms.topic: troubleshooting
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.date: 08/09/2019
ms.openlocfilehash: b6e71f5f2c389926a2e6267fadeedebdce1c51d9
ms.sourcegitcommit: 7863fcea618b0342b7c91ae345aa099114205b03
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/03/2020
ms.locfileid: "93288899"
---
# <a name="scenario-permission-denied-error-when-trying-to-create-an-apache-hive-table-in-azure-hdinsight"></a>Escenario: Error de permiso denegado al intentar crear una tabla de Apache Hive en Azure HDInsight

En este artículo se describen los pasos para la solución de problemas y las posibles soluciones para los problemas que se producen al usar componentes de Interactive Query en clústeres de Azure HDInsight.

## <a name="issue"></a>Problema

Recibirá el siguiente error cuando intente crear una tabla:

```
java.sql.SQLException: Error while compiling statement: FAILED: HiveAccessControlException Permission denied: user [hdiuser] does not have [ALL] privilege on [wasbs://data@xxxxx.blob.core.windows.net/path/table]
```

Recibirá un mensaje de error similar si ejecuta el siguiente comando de almacenamiento HDFS:

```
hdfs dfs -mkdir wasbs://data@xxxxx.blob.core.windows.net/path/table
```

## <a name="cause"></a>Causa

La capacidad de crear una tabla en Apache Hive la deciden los permisos que se aplican a la cuenta de almacenamiento del clúster. Si los permisos de la cuenta de almacenamiento del clúster no son correctos, no podrá crear tablas. Esto significa que podría tener las directivas de Ranger correctas para la creación de tablas y, a pesar de eso, seguir recibiendo errores de "Permiso denegado".

## <a name="resolution"></a>Solución

Esto se debe a la falta de permisos suficientes en el contenedor de almacenamiento que se está usando. El usuario que crea la tabla de Hive necesita permisos de lectura, escritura y ejecución en el contenedor. Para más información, consulte [Procedimientos recomendados para la autorización de Hive mediante Apache Ranger en HDP 2.2](https://hortonworks.com/blog/best-practices-for-hive-authorization-using-apache-ranger-in-hdp-2-2/).

## <a name="next-steps"></a>Pasos siguientes

[!INCLUDE [troubleshooting next steps](../../../includes/hdinsight-troubleshooting-next-steps.md)]
---
title: Rendimiento deficiente en las consultas Apache Hive LLAP en Azure HDInsight
description: Las consultas de Apache Hive LLAP se ejecutan más lentamente de lo esperado en Azure HDInsight.
ms.service: hdinsight
ms.topic: troubleshooting
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.date: 07/30/2019
ms.openlocfilehash: ee7ce401f889dd9c06b14860f4fc9674c5350b52
ms.sourcegitcommit: 7863fcea618b0342b7c91ae345aa099114205b03
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/03/2020
ms.locfileid: "93288873"
---
# <a name="scenario-poor-performance-in-apache-hive-llap-queries-in-azure-hdinsight"></a>Escenario: Rendimiento deficiente en las consultas Apache Hive LLAP en Azure HDInsight

En este artículo se describen los pasos para la solución de problemas y las posibles soluciones para los problemas que se producen al usar componentes de Interactive Query en clústeres de Azure HDInsight.

## <a name="issue"></a>Problema

Las configuraciones de clúster predeterminadas no están suficientemente ajustadas para la carga de trabajo. Las consultas de Hive LLAP se ejecutan más lentamente de lo esperado.

## <a name="cause"></a>Causa

Se puede deber a varios motivos.

## <a name="resolution"></a>Solución

LLAP está optimizado para consultas que implican combinaciones y agregados. Las consultas como las siguientes no funcionan bien en un clúster de Hive interactivo:

```
select * from table where column = "columnvalue"
```

Para mejorar el rendimiento de las consultas puntuales en Hive LLAP, establezca las siguientes configuraciones:

```
hive.llap.io.enabled=false; (disable LLAP IO)
hive.optimize.index.filter=false; (disable ORC row index)
hive.exec.orc.split.strategy=BI; (to avoid recombining splits)
```

También puede aumentar el uso de la caché de LLAP para mejorar el rendimiento con el siguiente cambio de configuración:

```
hive.fetch.task.conversion=none
```

## <a name="next-steps"></a>Pasos siguientes

[!INCLUDE [troubleshooting next steps](../../../includes/hdinsight-troubleshooting-next-steps.md)]
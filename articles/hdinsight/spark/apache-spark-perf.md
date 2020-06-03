---
title: Optimización de trabajos de Spark para mejorar el rendimiento en Azure HDInsight
description: Se muestran estrategias comunes para obtener el mejor rendimiento de los clústeres de Apache Spark en Azure HDInsight.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 05/21/2020
ms.openlocfilehash: f92a351087670ce0b37921a496eabfa883a3b1fc
ms.sourcegitcommit: a9784a3fd208f19c8814fe22da9e70fcf1da9c93
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/22/2020
ms.locfileid: "83780108"
---
# <a name="optimize-apache-spark-jobs-in-hdinsight"></a>Optimización de trabajos de Apache Spark en HDInsight

En este artículo se proporciona información general sobre las estrategias para optimizar trabajos Apache Spark en Azure HDInsight.

## <a name="overview"></a>Información general

El rendimiento de los trabajos de Apache Spark depende de varios factores. Estos factores de rendimiento incluyen: cómo se almacenan los datos, cómo se configura el clúster y las operaciones que se usan al procesar los datos.

Entre las dificultades más comunes que pueden surgir se incluyen las restricciones de memoria debido al tamaño incorrecto de los ejecutores, las operaciones de larga duración y las tareas que dan lugar a operaciones cartesianas.

También hay varias estrategias que pueden ayudarle a superar estas dificultades, como el almacenamiento en caché y la posibilidad de sesgar los datos.

En cada uno de los siguientes artículos, puede encontrar dificultades y soluciones comunes para diferentes aspectos de la optimización de Spark.

* [Optimización del almacenamiento de datos](optimize-data-storage.md)
* [Optimización del procesamiento de datos](optimize-data-processing.md)
* [Optimización del uso de la memoria](optimize-memory-usage.md)
* [Optimización de la configuración de clústeres](optimize-cluster-configuration.md)

## <a name="next-steps"></a>Pasos siguientes

* [Depuración de trabajos de Apache Spark que se ejecutan en Azure HDInsight](apache-spark-job-debugging.md)
* [Administración de recursos de un clúster Apache Spark en Azure HDInsight](apache-spark-resource-manager.md)
* [Configuración de opciones de Apache Spark](apache-spark-settings.md)

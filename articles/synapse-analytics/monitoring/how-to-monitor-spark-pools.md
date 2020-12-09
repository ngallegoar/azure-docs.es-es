---
title: Supervisión de grupos de Apache Spark en Synapse Studio
description: Obtenga información sobre cómo supervisar los grupos de Apache Spark mediante Synapse Studio.
services: synapse-analytics
author: matt1883
ms.service: synapse-analytics
ms.topic: how-to
ms.subservice: monitoring
ms.date: 11/30/2020
ms.author: mahi
ms.reviewer: mahi
ms.openlocfilehash: 8d95897e0c2d58b2a3955918be945800eed9ba56
ms.sourcegitcommit: 6a350f39e2f04500ecb7235f5d88682eb4910ae8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/01/2020
ms.locfileid: "96465799"
---
# <a name="use-synapse-studio-to-monitor-your-apache-spark-pools"></a>Uso de Synapse Studio para supervisar los grupos de Apache Spark

Con Azure Synapse Analytics, puede usar Spark para ejecutar cuadernos, trabajos y otros tipos de aplicaciones en grupos de Spark en su área de trabajo.

En este artículo se explica cómo supervisar los grupos de Apache Spark, lo que le permite vigilar el estado de los grupos, incluido el número de núcleos virtuales que usan los distintos usuarios del área de trabajo.

## <a name="access-spark-pools-list"></a>Acceso a la lista de grupos de Spark

Para ver la lista de grupos de Apache Spark en el área de trabajo, primero [abra Synapse Studio](https://web.azuresynapse.net/) y seleccione el área de trabajo.

![Inicio de sesión en el área de trabajo](./media/common/login-workspace.png)

Una vez que haya abierto el área de trabajo, seleccione la sección **Supervisar** a la izquierda.

![Selección del centro Supervisar](./media/common/left-nav.png)

Seleccione **grupos de Apache Spark** para ver la lista de grupos de Apache Spark.

 ![Selección de grupos de Apache Spark](./media/how-to-monitor-spark-pools/monitor-hub-nav-spark-pools.png)

## <a name="filter-your-spark-pools"></a>Filtrado de los grupos de Spark

Puede filtrar la lista de grupos de Spark por los que le interesen. Los filtros en la parte superior de la pantalla le permiten especificar un campo por el que desea filtrar.

Por ejemplo, puede filtrar la vista para ver solo los grupos de Spark que contienen el nombre "dataprep":

![Filtro de ejemplo](./media/how-to-monitor-spark-pools/filter-example.png)

## <a name="view-details-about-a-specific-spark-pool"></a>Visualización de detalles acerca de un grupo de Spark específico

Para ver los detalles acerca de uno de los grupos de Spark, seleccione el grupo de Spark para ver los detalles.

![Detalles del grupo de Apache Spark](./media/how-to-monitor-spark-pools/spark-pool-details.png)

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre la supervisión de ejecuciones de canalización, consulte el artículo [Supervisión de ejecuciones de canalización en Synapse Studio](how-to-monitor-pipeline-runs.md). 

Para más información sobre la supervisión de las aplicaciones de Apache Spark, consulte el artículo sobre la [Supervisión de aplicaciones de Apache Spark en Synapse Studio](how-to-monitor-spark-applications.md).

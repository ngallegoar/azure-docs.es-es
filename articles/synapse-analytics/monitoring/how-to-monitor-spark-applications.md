---
title: Procedimientos para supervisar aplicaciones Apache Spark
description: Use Azure Synapse Studio para supervisar sus aplicaciones de Apache Spark.
services: synapse-analytics
author: matt1883
ms.service: synapse-analytics
ms.topic: how-to
ms.subservice: monitoring
ms.date: 04/15/2020
ms.author: mahi
ms.reviewer: mahi
ms.openlocfilehash: e646f1bc1a25f58dd54437cbd77750479ac9ae99
ms.sourcegitcommit: f684589322633f1a0fafb627a03498b148b0d521
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/06/2020
ms.locfileid: "85970913"
---
# <a name="use-the-azure-synapse-studio-preview-to-monitor-your-apache-spark-applications"></a>Uso de Azure Synapse Studio (versión preliminar) para supervisar sus aplicaciones de Apache Spark

Con Azure Synapse Analytics, puede usar Spark para ejecutar cuadernos, trabajos y otros tipos de aplicaciones en los grupos de Spark en su área de trabajo.

En este artículo se explica cómo supervisar las aplicaciones Apache Spark, lo que le permite vigilar el estado más reciente, los problemas y el progreso.

## <a name="accessing-the-list-of-apache-spark-applications"></a>Acceso a la lista de aplicaciones Apache Spark

Para ver la lista de aplicaciones Apache Spark en el área de trabajo, primero [abra Azure Synapse Studio](https://web.azuresynapse.net/) y seleccione el área de trabajo.

![Inicio de sesión en el área de trabajo](./media/common/login-workspace.png)

Una vez que haya abierto el área de trabajo, seleccione la sección **Supervisar** a la izquierda.

![Selección del centro Supervisar](./media/common/left-nav.png)

Seleccione **Apache Spark applications** (Aplicaciones Apache Spark) para ver la lista de aplicaciones Spark.

 ![Selección de aplicaciones Spark](./media/how-to-monitor-spark-applications/monitor-hub-nav-sparkapplications.png)

## <a name="filtering-your-apache-spark-applications"></a>Filtrado de las aplicaciones Apache Spark

Puede filtrar la lista de aplicaciones Apache Spark por las que le interesan. Los filtros en la parte superior de la pantalla le permiten especificar un campo por el que desea filtrar.

Por ejemplo, puede filtrar la vista para ver solo las aplicaciones Apache Spark que contienen el nombre "sales":

![Botón de filtro](./media/common/filter-button.png)

![Filtro de ejemplo](./media/how-to-monitor-spark-applications/filter-example.png)

## <a name="viewing-details-about-a-specific-apache-spark-application"></a>Visualización de detalles sobre una aplicación Apache Spark específica

Para ver los detalles de una de las aplicaciones Apache Spark, seleccione la aplicación y vea los detalles. Si la aplicación de Apache Spark todavía está en ejecución, puede supervisar el progreso. [Más información](apache-spark-applications.md).

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información sobre la supervisión de ejecuciones de canalizaciones, consulte el artículo [Supervisión de ejecuciones de canalizaciones en Azure Synapse Studio](how-to-monitor-pipeline-runs.md). 

Para más información sobre la depuración de la aplicación Apache Spark, consulte el artículo sobre la [supervisión de aplicaciones Apache Spark en Azure Synapse Studio](apache-spark-applications.md).
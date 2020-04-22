---
title: Supervisión de aplicaciones de Apache Spark
description: Use Azure Synapse Studio para supervisar sus aplicaciones de Apache Spark.
services: synapse-analytics
author: matt1883
ms.service: synapse-analytics
ms.topic: how-to
ms.subservice: ''
ms.date: 04/15/2020
ms.author: mahi
ms.reviewer: mahi
ms.openlocfilehash: f231693fdcf3519e29eed38e47db52d92acc00fa
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/16/2020
ms.locfileid: "81427300"
---
# <a name="use-the-azure-synapse-studio-preview-to-monitor-your-apache-spark-applications"></a>Uso de Azure Synapse Studio (versión preliminar) para supervisar sus aplicaciones de Apache Spark

Con Azure Synapse Analytics, puede usar Spark para ejecutar cuadernos, trabajos y otros tipos de aplicaciones en los grupos de Spark en su área de trabajo.

En este artículo se explica cómo supervisar las aplicaciones Spark, lo que le permite vigilar el estado más reciente, los problemas y el progreso.

## <a name="accessing-the-list-of-spark-applications"></a>Acceso a la lista de aplicaciones Spark

Para ver la lista de aplicaciones Spark en su área de trabajo, primero [abra Azure Synapse Studio](https://web.azuresynapse.net/) y seleccione el área de trabajo.

  > [!div class="mx-imgBorder"]
  > ![Inicio de sesión en el área de trabajo](./media/common/login-workspace.png)

Una vez que haya abierto el área de trabajo, seleccione la sección **Supervisar** a la izquierda.

  > [!div class="mx-imgBorder"]
  > ![Selección del centro Supervisar](./media/common/left-nav.png)

Seleccione **Aplicaciones de Spark** para ver la lista de aplicaciones Spark.

  > [!div class="mx-imgBorder"]
  > ![Selección de aplicaciones de Spark](./media/how-to-monitor-spark-applications/monitor-hub-nav-sparkapplications.png)

## <a name="filtering-your-spark-applications"></a>Filtrado de las aplicaciones Spark

Puede filtrar la lista de aplicaciones Spark a las que le interesan. Los filtros en la parte superior de la pantalla le permiten especificar un campo por el que desea filtrar.

Por ejemplo, puede filtrar la vista para ver solo las aplicaciones Spark que contienen el nombre "sales":

  > [!div class="mx-imgBorder"]
  > ![Botón de filtro](./media/common/filter-button.png)

  > [!div class="mx-imgBorder"]
  > ![Filtro de ejemplo](./media/how-to-monitor-spark-applications/filter-example.png)

## <a name="viewing-details-about-a-specific-spark-application"></a>Visualización de detalles sobre una aplicación Spark específica

Para ver los detalles de una de sus aplicaciones Spark, seleccione la aplicación Spark y vea los detalles. Si la aplicación Spark todavía está en ejecución, puede supervisar el progreso.

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información sobre la supervisión de ejecuciones de canalizaciones, consulte el artículo [Supervisión de ejecuciones de canalizaciones en Azure Synapse Studio](how-to-monitor-pipeline-runs.md).  

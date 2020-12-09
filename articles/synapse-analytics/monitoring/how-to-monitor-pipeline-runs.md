---
title: Supervisión de ejecuciones de canalización con Synapse Studio
description: Use Synapse Studio para supervisar las ejecuciones de canalización del área de trabajo.
services: synapse-analytics
author: matt1883
ms.service: synapse-analytics
ms.topic: how-to
ms.subservice: monitoring
ms.date: 11/30/2020
ms.author: mahi
ms.reviewer: mahi
ms.openlocfilehash: dbd890117c78274392d5745e0563332371b404c5
ms.sourcegitcommit: 6a350f39e2f04500ecb7235f5d88682eb4910ae8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/01/2020
ms.locfileid: "96452577"
---
# <a name="use-synapse-studio-to-monitor-your-workspace-pipeline-runs"></a>Uso de Synapse Studio para supervisar las ejecuciones de canalización del área de trabajo

Con Azure Synapse Analytics, puede crear canalizaciones complejas que pueden automatizar e integrar el movimiento de datos, la transformación de datos y las actividades de proceso dentro de su solución. Puede crear y supervisar estas canalizaciones mediante Synapse Studio.

En este artículo se explica cómo supervisar las ejecuciones de canalización, lo que le permite vigilar el estado más reciente, los problemas y el progreso de sus canalizaciones.

## <a name="access-pipeline-runs-list"></a>Acceso a la lista de ejecuciones de canalización

Para ver la lista de ejecuciones de canalización en su área de trabajo, primero [abra Synapse Studio](https://web.azuresynapse.net/) y seleccione el área de trabajo.

![Inicio de sesión en el área de trabajo](./media/common/login-workspace.png)

Una vez que haya abierto el área de trabajo, seleccione la sección **Supervisar** a la izquierda.

![Selección del centro Supervisar](./media/common/left-nav.png)

Seleccione **Ejecuciones de canalización** para ver la lista de ejecuciones de canalización.

![Selección de ejecuciones de canalización](./media/how-to-monitor-pipeline-runs/monitor-hub-nav-pipelineruns.png)

## <a name="filter-your-pipeline-runs"></a>Filtrado de ejecuciones de canalización

Puede filtrar la lista de ejecuciones de canalización a las que le interesan. Los filtros en la parte superior de la pantalla le permiten especificar un campo por el que desea filtrar.

Por ejemplo, puede filtrar la vista para ver solo las ejecuciones de canalización para la canalización denominada "holiday":

![Filtro de ejemplo](./media/how-to-monitor-pipeline-runs/filter-example.png)

## <a name="view-details-about-a-specific-pipeline-run"></a>Visualización de detalles sobre una ejecución de una canalización específica

Para ver los detalles sobre la ejecución de canalización, seleccione la ejecución de la canalización. A continuación, vea las ejecuciones de actividad asociadas a la ejecución de la canalización. Si la canalización todavía está en ejecución, puede supervisar el progreso. 
  
## <a name="next-steps"></a>Pasos siguientes

Para obtener más información sobre la supervisión de aplicaciones, consulte el artículo [Supervisión de aplicaciones de Apache Spark](how-to-monitor-spark-applications.md). 

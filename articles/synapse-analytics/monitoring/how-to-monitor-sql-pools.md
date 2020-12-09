---
title: Supervisión de grupos de SQL en Synapse Studio
description: Obtenga información sobre cómo supervisar los grupos de SQL mediante Synapse Studio.
services: synapse-analytics
author: matt1883
ms.service: synapse-analytics
ms.topic: how-to
ms.subservice: monitoring
ms.date: 11/30/2020
ms.author: mahi
ms.reviewer: mahi
ms.openlocfilehash: 63b63526333435387c3ff5b5c9d5599ec851c1a8
ms.sourcegitcommit: 6a350f39e2f04500ecb7235f5d88682eb4910ae8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/01/2020
ms.locfileid: "96465828"
---
# <a name="use-synapse-studio-to-monitor-your-sql-pools"></a>Uso de Synapse Studio para supervisar los grupos de SQL

Con Synapse Studio, puede ejecutar scripts SQL en los grupos de SQL del área de trabajo.

En este artículo se explica cómo supervisar los grupos de SQL, lo que le permite vigilar el estado y la actividad de los grupos.

## <a name="access-sql-pools-list"></a>Acceso a la lista de grupos de SQL

Para ver la lista de grupos de SQL en su área de trabajo, primero [abra Synapse Studio](https://web.azuresynapse.net/) y seleccione el área de trabajo.

![Inicio de sesión en el área de trabajo](./media/common/login-workspace.png)

Una vez que haya abierto el área de trabajo, seleccione la sección **Supervisar** a la izquierda.

![Selección del centro Supervisar](./media/common/left-nav.png)

Seleccione los **grupos de SQL** para ver la lista de grupos de SQL.

 ![Selección de grupos de SQL](./media/how-to-monitor-sql-pools/monitor-hub-nav-sql-pools.png)

## <a name="filter-your-sql-pools"></a>Filtrado de los grupos de SQL

Puede filtrar la lista de grupos de SQL por los que le interesen. Los filtros en la parte superior de la pantalla le permiten especificar un campo por el que desea filtrar.

Por ejemplo, puede filtrar la vista para ver solo los grupos de SQL que contienen el nombre "salesrecords":

![Filtro de ejemplo](./media/how-to-monitor-sql-pools/filter-example.png)

## <a name="view-details-about-a-specific-sql-pool"></a>Visualización de detalles acerca de un grupo de SQL específico

Para ver los detalles acerca de uno de los grupos de SQL, seleccione el grupo de SQL para ver los detalles.

![Detalles del grupo de SQL](./media/how-to-monitor-sql-pools/sql-pool-details.png)

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre la supervisión de ejecuciones de canalización, consulte el artículo [Supervisión de ejecuciones de canalización en Synapse Studio](how-to-monitor-pipeline-runs.md). 

Para obtener más información sobre la supervisión de solicitudes de SQL, consulte el artículo [Supervisión de solicitudes de SQL en Synapse Studio](how-to-monitor-sql-requests.md).
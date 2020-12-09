---
title: Supervisión de solicitudes SQL en Synapse Studio
description: Obtenga información sobre cómo supervisar las solicitudes SQL mediante Synapse Studio.
services: synapse-analytics
author: matt1883
ms.service: synapse-analytics
ms.topic: how-to
ms.subservice: monitoring
ms.date: 11/30/2020
ms.author: mahi
ms.reviewer: mahi
ms.openlocfilehash: 7296fec91decaf1bd80829f9f3a743a518fbb7a7
ms.sourcegitcommit: 6a350f39e2f04500ecb7235f5d88682eb4910ae8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/01/2020
ms.locfileid: "96465825"
---
# <a name="use-synapse-studio-to-monitor-your-sql-requests"></a>Uso de Synapse Studio para supervisar las solicitudes SQL

Con Synapse Studio, puede ejecutar scripts SQL en los grupos de SQL del área de trabajo.

En este artículo se explica cómo supervisar las solicitudes SQL, lo que le permite supervisar el estado de las solicitudes en ejecución y descubrir los detalles de las solicitudes históricas.

## <a name="access-sql-requests-list"></a>Acceso a la lista de solicitudes de SQL

Para ver la lista de solicitudes SQL en su área de trabajo, primero [abra Synapse Studio](https://web.azuresynapse.net/) y seleccione el área de trabajo.

![Inicio de sesión en el área de trabajo](./media/common/login-workspace.png)

Una vez que haya abierto el área de trabajo, seleccione la sección **Supervisar** a la izquierda.

![Selección del centro Supervisar](./media/common/left-nav.png)

Seleccione **solicitudes SQL** para ver la lista de solicitudes SQL.

 ![Selección de solicitudes SQL](./media/how-to-monitor-sql-requests/monitor-hub-nav-sql-requests.png)

## <a name="select-a-sql-pool"></a>Selección de un grupo de SQL

De manera predeterminada, en esta vista se muestra el historial de solicitudes SQL para el grupo de SQL sin servidor integrado. Puede seleccionar uno de los grupos de SQL dedicados para ver el historial de solicitudes SQL de ese grupo.

![Selección del grupo de SQL](./media/how-to-monitor-sql-requests/select-pool.png)

## <a name="filter-your-sql-requests"></a>Filtrado de las solicitudes SQL

Puede filtrar la lista de solicitudes SQL por las que le interesen. Los filtros en la parte superior de la pantalla le permiten especificar un campo por el que desea filtrar.

Por ejemplo, puede filtrar la vista para ver solo las solicitudes SQL que ha enviado `maria@contoso.com`:

![Filtro de ejemplo](./media/how-to-monitor-sql-requests/filter-example.png)

## <a name="view-details-about-a-specific-sql-request"></a>Visualización de detalles acerca de una solicitud SQL específica

Para ver los detalles sobre una de las solicitudes SQL, abra la solicitud SQL para navegar a la vista de detalles. En el caso de las solicitudes complejas que se ejecutan en grupos de SQL dedicados, puede supervisar el progreso.

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre la supervisión de ejecuciones de canalización, consulte el artículo [Supervisión de ejecuciones de canalización en Synapse Studio](how-to-monitor-pipeline-runs.md). 

Para más información sobre la supervisión de las aplicaciones de Apache Spark, consulte el artículo sobre la [Supervisión de aplicaciones de Apache Spark en Synapse Studio](how-to-monitor-spark-applications.md).
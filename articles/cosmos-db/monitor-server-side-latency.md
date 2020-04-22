---
title: Cómo supervisar la latencia del servidor para las operaciones de Azure Cosmos DB
description: Obtenga información acerca de cómo supervisar la latencia del servidor para las operaciones en una cuenta o en un contenedor de Azure Cosmos DB. Los propietarios de una cuenta de Azure Cosmos DB pueden comprender los problemas de latencia del servidor con las cuentas de Azure Cosmos.
ms.service: cosmos-db
ms.topic: conceptual
author: kanshiG
ms.author: govindk
ms.date: 04/07/2020
ms.openlocfilehash: 0f1e6d07afb3b7b4d26081bc9e34ac257b280d0f
ms.sourcegitcommit: fb23286d4769442631079c7ed5da1ed14afdd5fc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/10/2020
ms.locfileid: "81113922"
---
# <a name="how-to-monitor-the-server-side-latency-for-operations-in-an-azure-cosmos-db-container-or-account"></a>Cómo supervisar la latencia del servidor para las operaciones en una cuenta o en un contenedor de Azure Cosmos DB

Azure Monitor para Azure Cosmos DB proporciona una vista de métricas para supervisar la cuenta y crear paneles. Las métricas de Azure Cosmos DB se recopilan de forma predeterminada, esta característica no requiere que habilite ni configure nada explícitamente. La métrica de latencia del servidor se usa para ver la latencia del servidor de una operación. Azure Cosmos DB proporciona un acuerdo de nivel de servicio de menos de 10 ms para operaciones de lectura y escritura puntuales con conectividad directa. En el caso de las operaciones de lectura y escritura puntuales, los acuerdos de nivel de servicio se calculan como se detalla en el documento [SLA](https://azure.microsoft.com/support/legal/sla/cosmos-db/v1_3/).

Si ve una latencia inusualmente grande para operaciones puntuales como:

* Una operación get o set con clave de partición e identificador en modo directo
* Una operación de lectura o escritura
* Una consulta

Puede buscar en el registro de diagnóstico para ver el tamaño de los datos devueltos. Si observa una latencia elevada sostenida para las operaciones de consulta, puede buscar en el registro de diagnóstico el tamaño de los datos devueltos, el [rendimiento o RU/s](cosmosdb-monitor-resource-logs.md#diagnostic-queries) utilizadas o el número de operaciones en un período determinado. De este modo, puede depurar los problemas de latencia del servidor.

## <a name="view-the-server-side-latency-metric"></a>Visualización de las métricas de latencia del servidor

1. Inicie sesión en [Azure Portal](https://portal.azure.com/).

1. Seleccione **Monitor** en la barra de navegación izquierda y, a continuación, seleccione **Métricas**.

   ![Panel Métricas en Azure Monitor](./media/monitor-server-side-latency/monitor-metrics-blade.png)

1. En el panel **Métricas** panel > **Seleccionar un recurso** > elija la **suscripción**, y el **grupo de recursos** requeridos. En **Tipo de recurso**, seleccione **Cuentas de Azure Cosmos DB**, elija una de las cuentas de Azure Cosmos existentes y seleccione **Aplicar**.
   
   ![Elección de la cuenta de Azure Cosmos DB para ver las métricas](./media/monitor-server-side-latency/select-cosmos-db-account.png)

1. A continuación, seleccione la métrica **Latencia del servidor** en la lista de métricas disponibles. Para conocer los detalles de todas las métricas disponibles en esta lista, consulte el artículo [Métricas por categoría](monitor-cosmos-db-reference.md). En este ejemplo, vamos a seleccionar **Latencia de servidor** y **Prom** como valor de agregación. Además de estos detalles, también puede seleccionar los valores de **Intervalo de tiempo** y **Granularidad de tiempo** de las métricas. Como máximo, puede ver las métricas de los últimos 30 días.  Después de aplicar el filtro, se muestra un gráfico basado en dicho filtro. Puede ver la latencia del servidor por minuto para el período seleccionado.  

   ![Elección de la métrica de latencia del servidor en Azure Portal](./media/monitor-server-side-latency/server-side-latency-metric.png)

## <a name="filters-for-server-side-latency"></a>Filtros para la latencia del servidor

También puede filtrar las métricas y hacer que el gráfico se muestre por una propiedad específica, como **CollectionName**, **ConnectionMode**, **DatabaseName**, **OperationType**, **Region** o **PublicAPIType**. 

Para filtrar las métricas, seleccione **Agregar filtro** y elija la propiedad necesaria, como por ejemplo **PublicAPIType** y seleccione el valor **sql**. Agregue otro filtro para **OperationType**. A continuación, el gráfico muestra la latencia del servidor para distintas operaciones durante el período seleccionado. Las operaciones ejecutadas a través del procedimiento almacenado no se registran, por lo que no están disponibles en la métrica OperationType.

La métrica **Latencia del servidor** de cada operación se muestra en la siguiente imagen:

![Filtros para las métricas de latencia del servidor](./media/monitor-server-side-latency/server-side-latency-filters.png)

Para agrupar las métricas, también puede usar la opción **Apply splitting** (Aplicar división).  

## <a name="next-steps"></a>Pasos siguientes

* Supervisión de datos de Azure Cosmos DB mediante la [configuración de diagnóstico](cosmosdb-monitor-resource-logs.md) en Azure
* [Auditoría de las operaciones del plano de control de Azure Cosmos DB](audit-control-plane-logs.md)
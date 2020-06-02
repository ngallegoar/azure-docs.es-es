---
title: Supervisar RU/s normalizadas para un contenedor de Azure Cosmos o una cuenta
description: Obtenga información sobre cómo supervisar el uso de unidades de solicitud normalizadas de una operación en Azure Cosmos DB. Los propietarios de una cuenta de Azure Cosmos DB pueden conocer qué operaciones consumen más unidades de solicitud.
ms.service: cosmos-db
ms.topic: conceptual
author: kanshiG
ms.author: govindk
ms.date: 05/10/2020
ms.openlocfilehash: 23001bdaab0732dbeb088ebadefa90a27e622b19
ms.sourcegitcommit: a8ee9717531050115916dfe427f84bd531a92341
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/12/2020
ms.locfileid: "83118788"
---
# <a name="how-to-monitor-normalized-rus-for-an-azure-cosmos-container-or-an-account"></a>Cómo supervisar RU/s normalizadas para un contenedor de Azure Cosmos o una cuenta

Azure Monitor para Azure Cosmos DB proporciona una vista de métricas para supervisar la cuenta y crear paneles. Las métricas de Azure Cosmos DB se recopilan de forma predeterminada, esta característica no requiere que habilite ni configure nada explícitamente.

La métrica del **Consumo de RU normalizado** se usa para ver el grado de saturación de las réplicas en relación con el consumo de unidades de solicitud en los rangos de claves de partición. Azure Cosmos DB distribuye el rendimiento de forma uniforme entre todas las particiones físicas. Esta métrica proporciona una vista por segundo del uso máximo de rendimiento dentro de un conjunto de réplicas. Al usar esta métrica, si ve un porcentaje elevado del uso de unidades de solicitud, debe aumentar el rendimiento para satisfacer las necesidades de la carga de trabajo.

## <a name="what-to-expect-and-do-when-normalized-rus-is-higher"></a>Qué esperar y hacer cuando el consumo de RU/s normalizado es mayor

Cuando el consumo de RU/s normalizado alcanza el 100 %, el cliente recibe errores de limitación de velocidad. El cliente debe respetar el tiempo de espera y volver a intentarlo. Si hay un pequeño pico que alcanza el 100 % de uso, significa que el rendimiento de la réplica ha alcanzado su límite máximo de rendimiento. Por ejemplo, una sola operación, como un procedimiento almacenado que consume todas las RU/s en una réplica, dará lugar a un breve aumento en el consumo de RU/s normalizado. En tales casos, no habrá errores de limitación de velocidad inmediata si la tasa de solicitudes es baja. Esto se debe a que, Azure Cosmos DB permite que las solicitudes se carguen más que las RU/s aprovisionadas para la solicitud específica y que otras solicitudes dentro de ese período de tiempo tengan una velocidad limitada.

Las métricas de Azure Monitor ayudan a encontrar las operaciones por código de estado mediante el uso de la métrica del **Total de solicitudes**. Posteriormente, puede filtrar estas solicitudes por el código de estado 429 y dividirlas por **Tipo de operación**.

Para buscar las solicitudes que tienen una velocidad limitada, el método recomendado es obtener esta información a través de registros de diagnóstico.

Si hay un pico continuo del consumo de RU/s normalizado del 100 % o cercano al 100 %, se recomienda aumentar el rendimiento. Puede averiguar qué operaciones son de consumo intenso y su uso máximo al usar las métricas de Azure Monitor y los registros de Azure Monitor.

La métrica de **Consumo de RU normalizado**  también se usa para ver qué rango de claves de partición es más eficiente en términos de uso; por lo tanto, le permite sesgar el rendimiento hacia un rango de claves de partición. Después, puede consultar el registro **PartitionKeyRUConsumption** en los registros de Azure Monitor para obtener información acerca de las claves de partición lógicas que están activas en términos de uso.

## <a name="view-the-normalized-request-unit-consumption-metric"></a>Ver la métrica de consumo normalizado de unidades de solicitud

1. Inicie sesión en [Azure Portal](https://portal.azure.com/).

2. Seleccione **Monitor** en la barra de navegación izquierda y, a continuación, seleccione **Métricas**.

   ![Panel Métricas en Azure Monitor](./media/monitor-normalized-request-units/monitor-metrics-blade.png)

3. En el panel **Métricas** panel > **Seleccionar un recurso** > elija la **suscripción**, y el **grupo de recursos** requeridos. En **Tipo de recurso**, seleccione **Cuentas de Azure Cosmos DB**, elija una de las cuentas de Azure Cosmos existentes y seleccione **Aplicar**.

   ![Elección de una cuenta de Azure Cosmos DB para ver las métricas](./media/monitor-normalized-request-units/select-cosmos-db-account.png)

4. A continuación, puede seleccionar una métrica de la lista de métricas disponibles. Puede seleccionar métricas específicas de unidades de solicitud, almacenamiento, latencia, disponibilidad, Cassandra, etc. Para conocer los detalles de todas las métricas disponibles en esta lista, consulte el artículo [Métricas por categoría](monitor-cosmos-db-reference.md). En este ejemplo, vamos a seleccionar la métrica de **Consumo de RU normalizado** y **Max** como valor de agregación.

   Además de estos detalles, también puede seleccionar los valores de **Intervalo de tiempo** y **Granularidad de tiempo** de las métricas. Como máximo, puede ver las métricas de los últimos 30 días.  Después de aplicar el filtro, se muestra un gráfico basado en dicho filtro.

   ![Elegir una métrica en Azure Portal](./media/monitor-normalized-request-units/normalized-request-unit-usage-metric.png)

### <a name="filters-for-normalized-request-unit-consumption"></a>Filtros para el consumo normalizado de unidades de solicitud

También puede filtrar las métricas y el gráfico que se muestra por una propiedad **CollectionName**, **DatabaseName**, **PartitionKeyRangeID** y **Region** concreta. Para filtrar las métricas, seleccione **Agregar filtro** y elija la propiedad necesaria, como **CollectionName** y el valor correspondiente que le interese. A continuación, el gráfico muestra las unidades de consumo de RU normalizado utilizadas para el contenedor durante el período seleccionado.  

Para agrupar las métricas puede usar la opción **Apply splitting** (Aplicar división).  

La métrica de consumo normalizado de unidades de solicitud para cada contenedor aparece como se muestra en la siguiente imagen:

![Aplicar filtros a la métrica de consumo normalizado de unidades de solicitud](./media/monitor-normalized-request-units/normalized-request-unit-usage-filters.png)

## <a name="next-steps"></a>Pasos siguientes

* Supervise los datos de Azure Cosmos DB mediante la [configuración de diagnóstico](cosmosdb-monitor-resource-logs.md) en Azure.
* [Auditoría de las operaciones del plano de control de Azure Cosmos DB](audit-control-plane-logs.md)
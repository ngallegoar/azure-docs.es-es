---
title: archivo de inclusión
description: archivo de inclusión
services: digital-twins
ms.service: digital-twins
ms.topic: include
ms.date: 07/09/2020
author: deepakpalled
ms.author: dpalled
manager: diviso
ms.custom: include file
ms.openlocfilehash: 7259e1981f873c8385a02fe4f353dcdda495f823
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/25/2020
ms.locfileid: "91287427"
---
### <a name="property-limits"></a>Límites de propiedad

Los límites de propiedad de Azure Time Series Insights han aumentado a 1000 desde un límite máximo de 800 en Gen1. Las propiedades de evento proporcionadas tienen columnas JSON, CSV y de gráfico correspondientes que puede ver en el [explorador de Azure Time Series Insights Gen2](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-update-quickstart).

| SKU | Propiedades máximas |
| --- | --- |
| Gen2 (L1) | Propiedades de 1000 (columnas) |
| Gen1 (S1) | 600 propiedades (columnas) |
| Gen1 (S2) | 800 propiedades (columnas) |

### <a name="streaming-ingestion"></a>Ingesta de streaming

* Hay un máximo de dos [orígenes de eventos](../articles/time-series-insights/concepts-streaming-ingestion-event-sources.md) por entorno.

* Los procedimientos recomendados y la orientación general para los orígenes de eventos está disponible [aquí](../articles/time-series-insights/concepts-streaming-ingestion-event-sources.md#streaming-ingestion-best-practices).

* De forma predeterminada, Azure Time Series Insights Gen2 puede ingerir los datos entrantes a una velocidad de **hasta 1 megabyte por segundo (MBps) por entorno de Azure Time Series Insights Gen2**. Existen limitaciones adicionales [por partición de centro de conectividad](../articles/time-series-insights/concepts-streaming-ingress-throughput-limits.md#hub-partitions-and-per-partition-limits). Se pueden obtener velocidades de hasta 8 MBps mediante el envío de una incidencia de soporte técnico a través de Azure Portal. Para obtener más información, consulte [Límites de rendimiento de ingesta de streaming](../articles/time-series-insights/concepts-streaming-ingress-throughput-limits.md).

### <a name="api-limits"></a>Límites de API

Los límites de la API de REST para Azure Time Series Insights Gen2 están especificadas en la [documentación de referencia de la API de REST](https://docs.microsoft.com/rest/api/time-series-insights/preview#limits-1).

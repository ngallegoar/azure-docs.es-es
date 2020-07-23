---
title: archivo de inclusión
description: archivo de inclusión
services: digital-twins
ms.service: digital-twins
ms.topic: include
ms.date: 02/07/2020
author: deepakpalled
ms.author: dpalled
manager: cshankar
ms.custom: include file
ms.openlocfilehash: 1171c07a754b056fb9df2cee73ddf224427037cc
ms.sourcegitcommit: f7e160c820c1e2eb57dc480b2a8fd6bef7053e91
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/10/2020
ms.locfileid: "86277835"
---
### <a name="general-availability-and-preview-comparison"></a>Comparación entre disponibilidad general y versión preliminar

En la tabla siguiente se resumen varias diferencias clave entre las instancias de disponibilidad general (GA) y versión preliminar de Azure Time Series Insights.

| | Disponibilidad general | Versión preliminar |
| --- | --- | ---|
| **Ciudadano de primera clase** | Centrada en eventos | Centrada en series temporales |
| **Razonamiento semántico** | Bajo nivel (datos de referencia) | Alto nivel (modelos) |
| **Contextualización de datos** | Nivel no de dispositivo | Nivel de dispositivo y no de dispositivo |
| **Almacenamiento de lógica de proceso** | No | Almacenado en la parte de variables de tipo del modelo |
| **Control de almacenamiento y acceso** | No | Habilitado a través del modelo |
| **Agregaciones/muestreo** | No | Evento ponderado y tiempo ponderado |
| **Reconstrucción de señal** | No | Interpolación |
| **Producción de series temporales derivadas** | No | Sí, combinaciones y uniones |
| **Flexibilidad de lenguaje** | No admite composición | Admite composición |
| **Lenguaje de expresiones** | Cadena de predicados | Expresiones de series temporales (cadenas de predicado, valores, expresiones y funciones) |

### <a name="property-limits"></a>Límites de propiedad

Los límites de propiedad de Time Series Insights han aumentado a 1000 desde un límite máximo de 800 en GA. Las propiedades de evento proporcionadas tienen columnas JSON, CSV y de gráfico correspondientes que puede ver en el [explorador de la versión preliminar de Time Series Insights](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-update-quickstart).

| SKU | Propiedades máximas |
| --- | --- |
| Versión preliminar de pago por uso | Propiedades de 1000 (columnas) |
| GA S1 | 600 propiedades (columnas) |
| GA S2 | 800 propiedades (columnas) |

### <a name="event-sources"></a>Orígenes de eventos

Se admite un máximo de dos orígenes de evento por instancia. 

* Obtenga información sobre cómo [agregar un origen de Event Hub](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-how-to-add-an-event-source-eventhub).
* Configure [un origen de centro de IoT](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-how-to-add-an-event-source-iothub).

De forma predeterminada, [los entornos de versión preliminar admiten tasas de entrada](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-update-storage-ingress#ingress-scale-and-preview-limitations) de hasta **1 megabyte por segundo (MB/s) por entorno** . Los clientes pueden escalar sus entornos de versión preliminar hasta un rendimiento de **16 MB/s** si es necesario. Además, hay un límite por partición de **0,5 MB/s**. 

### <a name="api-limits"></a>Límites de API

Los límites de la API de REST para la versión preliminar de Time Series Insights están especificadas en la [documentación de referencia de la API de REST](https://docs.microsoft.com/rest/api/time-series-insights/preview-query#limits).

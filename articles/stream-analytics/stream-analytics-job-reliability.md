---
title: Evitar interrupciones de servicio en trabajos de Azure Stream Analytics
description: En este artículo se describe la guía sobre cómo realizar la actualización resistente de los trabajos de Stream Analytics.
author: jseb225
ms.author: sidram
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: how-to
ms.date: 06/21/2019
ms.custom: seodec18
ms.openlocfilehash: 889e298f64689748340713de6318f8ffcd181001
ms.sourcegitcommit: 857859267e0820d0c555f5438dc415fc861d9a6b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2020
ms.locfileid: "93123853"
---
# <a name="guarantee-stream-analytics-job-reliability-during-service-updates"></a>Garantía de la confiabilidad del trabajo de Stream Analytics durante las actualizaciones del servicio

Parte de ser un servicio completamente administrado es la capacidad de introducir nuevas funcionalidades de servicio y mejoras a un ritmo rápido. Como resultado, Stream Analytics puede tener una actualización de servicio implementada de forma semanal (o más frecuentemente). Con independencia de cuántas pruebas se realicen, sigue siendo un riesgo que un trabajo existente y en ejecución pueda interrumpirse debido a la introducción de un error. Si ejecuta trabajos de misión crítica, estos riesgos deben evitarse. Puede reducir este riesgo al seguir el modelo de **[región emparejada](../best-practices-availability-paired-regions.md)** de Azure. 

## <a name="how-do-azure-paired-regions-address-this-concern"></a>¿Cómo solucionan este problema las regiones emparejadas de Azure?

Stream Analytics garantiza que los trabajos de las regiones emparejadas se actualicen en lotes separados. Como resultado, hay un intervalo de tiempo suficiente entre las actualizaciones para identificar posibles errores y corregirlos.

_A excepción del centro de la India_ (cuya región emparejada, Sur de la India, no tiene presencia de Stream Analytics), la implementación de una actualización para Stream Analytics no se producirá al mismo tiempo en un conjunto de regiones emparejadas. Pueden producirse implementaciones en varias regiones **del mismo grupo** **al mismo tiempo**.

El artículo sobre **[disponibilidad y regiones emparejadas](../best-practices-availability-paired-regions.md)** tiene la información más reciente sobre qué regiones están emparejadas.

Se recomienda implementar trabajos idénticos en ambas regiones emparejadas. Luego, debería [supervisar estos trabajos](./stream-analytics-set-up-alerts.md#scenarios-to-monitor) para recibir notificaciones cuando ocurre algo inesperado. Si uno de estos trabajos termina en un [estado de error](./job-states.md) después de una actualización del servicio de Stream Analytics, puede ponerse en contacto con el soporte al cliente para ayudar a identificar la causa raíz. También debería realizar la conmutación por error de los consumidores descendentes a la salida de trabajo con estado correcto.

## <a name="next-steps"></a>Pasos siguientes

* [¿Qué es Stream Analytics?](stream-analytics-introduction.md)
* [Introducción a Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Escalado de trabajos de Stream Analytics](stream-analytics-scale-jobs.md)
* [Referencia de lenguaje de consulta de Stream Analytics](/stream-analytics-query/stream-analytics-query-language-reference)
* [Referencia de API de REST de administración de Stream Analytics](/rest/api/streamanalytics/)
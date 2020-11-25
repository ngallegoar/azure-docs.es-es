---
title: Estados de trabajo de Azure Stream Analytics
description: 'En este artículo se describen los cuatro estados de un trabajo de Stream Analytics: en ejecución, detenido, degradado y erróneo.'
author: sidram
ms.author: sidram
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 06/21/2019
ms.openlocfilehash: 008493c6eb8840752be3815d30a5df5bad42aff2
ms.sourcegitcommit: a43a59e44c14d349d597c3d2fd2bc779989c71d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/25/2020
ms.locfileid: "96023864"
---
# <a name="azure-stream-analytics-job-states"></a>Estados de trabajo de Azure Stream Analytics

Un trabajo de Stream Analytics puede encontrarse en uno de cuatro estados en un momento dado: en ejecución, detenido, degradado y erróneo. Puede averiguar el estado de un trabajo en la página de información general del trabajo de Stream Analytics en Azure Portal. 

| State | Descripción | Acciones recomendadas |
| --- | --- | --- |
| **Ejecución** | El trabajo se ejecuta en los eventos de lectura de Azure procedentes de los orígenes de entrada definidos, los procesa y escribe los resultados en los receptores de salida configurados. | Es una práctica recomendada para realizar el seguimiento del rendimiento mediante la supervisión de las [métricas clave](./stream-analytics-set-up-alerts.md#scenarios-to-monitor). |
| **Stopped** | El trabajo está detenido y no procesa eventos. | N/D | 
| **Degradado** | Puede haber problemas intermitentes con las conexiones de entrada y salida. Estos errores se denominan errores transitorios, y podrían provocar que su trabajo entrara en un estado degradado. Stream Analytics intentará recuperarse inmediatamente de estos errores y volver a un estado en ejecución (en un plazo de unos minutos). Estos errores se pueden producir debido a problemas de red, disponibilidad de otros recursos de Azure, errores de deserialización, etc. El rendimiento del trabajo puede verse afectado cuando el trabajo se encuentra en estado degradado.| Puede mirar los [registros de diagnóstico o actividad](./stream-analytics-job-diagnostic-logs.md#debugging-using-activity-logs) para obtener una información más detallada sobre la causa de estos errores transitorios. En casos como el de los errores de deserialización, se recomienda realizar acciones correctivas para garantizar que los eventos no tienen un formato incorrecto. Si el trabajo alcanza repetidamente el límite de uso de los recursos, intente aumentar el número de unidades de almacenamiento o [paralelice el trabajo](./stream-analytics-parallelization.md). En otros casos en los no se puede realizar ninguna acción, Stream Analytics intentará recuperar a un estado de *ejecución*. <br> Puede usar la métrica de [retraso de la marca de agua](./stream-analytics-set-up-alerts.md#scenarios-to-monitor) para comprender si estos errores transitorios afectan al rendimiento de su trabajo.|
| **Erróneo** | Su trabajo encontró un error crítico, lo que resulta en un estado erróneo. Los eventos no se leen ni se procesan. Los errores en tiempo de ejecución son una causa común de que los trabajos acaben con un estado erróneo. | Puede [configurar alertas](./stream-analytics-set-up-alerts.md#set-up-alerts-in-the-azure-portal) para recibir notificaciones cuando trabajo entra en estado erróneo. <br> <br>Puede depurar utilizando los [registros de actividad y recursos](./stream-analytics-job-diagnostic-logs.md#debugging-using-activity-logs) para identificar la causa principal y solucionar el problema.|

## <a name="next-steps"></a>Pasos siguientes
* [Configuración de alertas en Azure Stream Analytics](stream-analytics-set-up-alerts.md)
* [Métricas disponibles para Stream Analytics](./stream-analytics-monitoring.md#metrics-available-for-stream-analytics)
* [Solución de problemas mediante registros de recursos y actividad](./stream-analytics-job-diagnostic-logs.md)
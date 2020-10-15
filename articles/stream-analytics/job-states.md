---
title: Estados de trabajo de Azure Stream Analytics
description: 'En este artículo se describen los cuatro estados de un trabajo de Stream Analytics: en ejecución, detenido, degradado y erróneo.'
author: sidram
ms.author: sidram
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 06/21/2019
ms.openlocfilehash: fc46b4f24c3c21cda190f80556373991f6c711c3
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "82130942"
---
# <a name="azure-stream-analytics-job-states"></a>Estados de trabajo de Azure Stream Analytics

Un trabajo de Stream Analytics puede encontrarse en uno de cuatro estados en un momento dado: en ejecución, detenido, degradado y erróneo. Puede averiguar el estado de un trabajo en la página de información general del trabajo de Stream Analytics en Azure Portal. 

| State | Descripción | Acciones recomendadas |
| --- | --- | --- |
| **Ejecución** | El trabajo se ejecuta en los eventos de lectura de Azure procedentes de los orígenes de entrada definidos, los procesa y escribe los resultados en los receptores de salida configurados. | Es una práctica recomendada para realizar el seguimiento del rendimiento mediante la supervisión de las [métricas clave](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-set-up-alerts#scenarios-to-monitor). |
| **Stopped** | El trabajo está detenido y no procesa eventos. | N/D | 
| **Degradado** | Puede haber problemas intermitentes con las conexiones de entrada y salida. Estos errores se denominan errores transitorios, y podrían provocar que su trabajo entrara en un estado degradado. Stream Analytics intentará recuperarse inmediatamente de estos errores y volver a un estado en ejecución (en un plazo de unos minutos). Estos errores se pueden producir debido a problemas de red, disponibilidad de otros recursos de Azure, errores de deserialización, etc. El rendimiento del trabajo puede verse afectado cuando el trabajo se encuentra en estado degradado.| Puede mirar los [registros de diagnóstico o actividad](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-job-diagnostic-logs#debugging-using-activity-logs) para obtener una información más detallada sobre la causa de estos errores transitorios. En casos como el de los errores de deserialización, se recomienda realizar acciones correctivas para garantizar que los eventos no tienen un formato incorrecto. Si el trabajo alcanza repetidamente el límite de uso de los recursos, intente aumentar el número de unidades de almacenamiento o [paralelice el trabajo](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-parallelization). En otros casos en los no se puede realizar ninguna acción, Stream Analytics intentará recuperar a un estado de *ejecución*. <br> Puede usar la métrica de [retraso de la marca de agua](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-set-up-alerts#scenarios-to-monitor) para comprender si estos errores transitorios afectan al rendimiento de su trabajo.|
| **Erróneo** | Su trabajo encontró un error crítico, lo que resulta en un estado erróneo. Los eventos no se leen ni se procesan. Los errores en tiempo de ejecución son una causa común de que los trabajos acaben con un estado erróneo. | Puede [configurar alertas](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-set-up-alerts#set-up-alerts-in-the-azure-portal) para recibir notificaciones cuando trabajo entra en estado erróneo. <br> <br>Puede depurar utilizando los [registros de actividad y recursos](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-job-diagnostic-logs#debugging-using-activity-logs) para identificar la causa principal y solucionar el problema.|

## <a name="next-steps"></a>Pasos siguientes
* [Configuración de alertas en Azure Stream Analytics](stream-analytics-set-up-alerts.md)
* [Métricas disponibles para Stream Analytics](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-monitoring#metrics-available-for-stream-analytics)
* [Solución de problemas mediante registros de recursos y actividad](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-job-diagnostic-logs)

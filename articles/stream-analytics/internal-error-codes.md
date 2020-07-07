---
title: Solución de problemas con códigos de error de Azure Stream Analytics
description: Solucione problemas de Azure Stream Analytics con códigos de error interno.
ms.author: mamccrea
author: mamccrea
ms.topic: conceptual
ms.date: 05/07/2020
ms.service: stream-analytics
ms.openlocfilehash: 2e101747322baac860cbde02aca341171aa0cc1d
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "83658352"
---
# <a name="azure-stream-analytics-internal-error-codes"></a>Códigos de error interno de Azure Stream Analytics

Puede usar registros de actividad y registros de recursos para ayudar a depurar comportamientos inesperados desde el trabajo de Azure Stream Analytics. En este artículo se muestra la descripción de cada código de error interno. Los errores internos son errores genéricos que se inician en la plataforma Stream Analytics cuando Stream Analytics no puede distinguir si el error es un error de disponibilidad interno o un error en el sistema.

## <a name="cosmosdboutputbatchsizetoolarge"></a>CosmosDBOutputBatchSizeTooLarge

* **Causa**: el tamaño del lote usado para escribir en Cosmos DB es demasiado grande. 
* **Recomendación:** vuelva a intentarlo con un tamaño de lote más pequeño.

## <a name="next-steps"></a>Pasos siguientes

* [Solución de problemas de conexiones de entrada](stream-analytics-troubleshoot-input.md)
* [Solución de problemas en las salidas de Azure Stream Analytics](stream-analytics-troubleshoot-output.md)
* [Solución de problemas en las consultas de Azure Stream Analytics](stream-analytics-troubleshoot-query.md)
* [Solución de problemas de Azure Stream Analytics mediante registros de recursos](stream-analytics-job-diagnostic-logs.md)
* [Errores de datos de Azure Stream Analytics](data-errors.md)
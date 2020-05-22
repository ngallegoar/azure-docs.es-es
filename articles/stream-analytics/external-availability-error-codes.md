---
title: 'Códigos de error externo de disponibilidad: Azure Stream Analytics'
description: Solucionar problemas de Azure Stream Analytics con códigos de error externo de disponibilidad.
ms.author: mamccrea
author: mamccrea
ms.topic: conceptual
ms.date: 05/07/2020
ms.service: stream-analytics
ms.openlocfilehash: 092b548e73330fe80ef10b2e25d9ab7883b256ac
ms.sourcegitcommit: bb0afd0df5563cc53f76a642fd8fc709e366568b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/19/2020
ms.locfileid: "83595204"
---
# <a name="azure-stream-analytics-external-availability-error-codes"></a>Códigos de error externo de disponibilidad de Azure Stream Analytics

Puede usar registros de actividad y registros de recursos para ayudar a depurar comportamientos inesperados desde el trabajo de Azure Stream Analytics. En este artículo se muestra la descripción de cada código de error externo de disponibilidad. Los errores externos de disponibilidad se producen cuando un servicio dependiente no está disponible.

## <a name="externalserviceunavailable"></a>ExternalServiceUnavailable

* **Causa**: el servicio no está disponible temporalmente.
* **Recomendación:** Stream Analytics seguirá intentando acceder al servicio. 

## <a name="kafkaservernotavailable"></a>KafkaServerNotAvailable

* **Causa**: el servidor de Kafka no está disponible.

## <a name="eventhubmessagingerror"></a>EventHubMessagingError

* **Causa**: Stream Analytics detectó un error al comunicarse con el centro de eventos. 


## <a name="next-steps"></a>Pasos siguientes

* [Solución de problemas de conexiones de entrada](stream-analytics-troubleshoot-input.md)
* [Solución de problemas en las salidas de Azure Stream Analytics](stream-analytics-troubleshoot-output.md)
* [Solución de problemas en las consultas de Azure Stream Analytics](stream-analytics-troubleshoot-query.md)
* [Solución de problemas de Azure Stream Analytics mediante registros de recursos](stream-analytics-job-diagnostic-logs.md)
* [Errores de datos de Azure Stream Analytics](data-errors.md)

---
title: 'Códigos de error externo: Azure Stream Analytics'
description: Solucione problemas de Azure Stream Analytics con códigos de error externo.
ms.author: mamccrea
author: mamccrea
ms.topic: troubleshooting
ms.date: 05/07/2020
ms.service: stream-analytics
ms.openlocfilehash: f963f4c3538146b7f9929960092a18294659be5d
ms.sourcegitcommit: e132633b9c3a53b3ead101ea2711570e60d67b83
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/07/2020
ms.locfileid: "86045253"
---
# <a name="azure-stream-analytics-external-error-codes"></a>Códigos de error externo de Azure Stream Analytics

Puede usar registros de actividad y registros de recursos para ayudar a depurar comportamientos inesperados desde el trabajo de Azure Stream Analytics. En este artículo se muestra la descripción de cada código de error externo. Los errores externos son errores genéricos generados por un servicio ascendente o descendente que Stream Analytics no puede distinguir como un error de datos, un error de configuración o un error de disponibilidad externo.

## <a name="adapterinitializationerror"></a>AdapterInitializationError

* **Causa**: Error al inicializar un adaptador.

## <a name="adapterfailedtowriteevents"></a>AdapterFailedToWriteEvents

* **Causa**: Error al escribir datos en un adaptador.

## <a name="azurefunctionhttperror"></a>AzureFunctionHttpError

* **Causa**: Se ha devuelto un error HTTP de Azure Functions.

## <a name="azurefunctionfailedtosendmessage"></a>AzureFunctionFailedToSendMessage

* **Causa**: Stream Analytics no pudo escribir los eventos en Azure Functions.

## <a name="azurefunctionredirecterror"></a>AzureFunctionRedirectError

* **Causa**: Se produce un error de redirección al generar Azure Functions.

## <a name="azurefunctionclienterror"></a>AzureFunctionClientError

* **Causa**: Se produce un error de cliente al generar Azure Functions.

## <a name="azurefunctionservererror"></a>AzureFunctionServerError

* **Causa**: Se produce un error de servidor al generar Azure Functions.

## <a name="azurefunctionhttptimeouterror"></a>AzureFunctionHttpTimeOutError

* **Causa**: Error al escribir en Azure Functions porque la solicitud HTTP superó el tiempo de espera. 
* **Recomendación:** Compruebe los registros de Azure Functions para detectar posibles retrasos.

## <a name="eventhubargumenterror"></a>EventHubArgumentError

* **Causa**: Los desplazamientos de entrada no son válidos. Esto puede deberse a una conmutación por error.
* **Recomendación:** Reinicie el trabajo de Stream Analytics desde la última hora de salida.

## <a name="eventhubfailedtowriteevents"></a>EventHubFailedToWriteEvents

* **Causa**: Error al enviar datos al centro de eventos.

## <a name="cosmosdbconnectionfailureaftermaxretries"></a>CosmosDBConnectionFailureAfterMaxRetries

* **Causa**: Stream Analytics no se pudo conectar a una cuenta de Cosmos DB después de alcanzar el número máximo de reintentos.

## <a name="cosmosdbfailureaftermaxretries"></a>CosmosDBFailureAfterMaxRetries

* **Causa**: Stream Analytics no pudo consultar la colección y la base de datos de Cosmos DB después de alcanzar el número máximo de reintentos.

## <a name="cosmosdbfailedtocreatestoredprocedure"></a>CosmosDBFailedToCreateStoredProcedure

* **Causa**: CosmosDB no puede crear el procedimiento almacenado después de varios reintentos.

## <a name="cosmosdboutputrequesttimeout"></a>CosmosDBOutputRequestTimeout

* **Causa**: El procedimiento almacenado de upsert devolvió un error. 

## <a name="sqldatabaseoutputinitializationerror"></a>SQLDatabaseOutputInitializationError

* **Causa**: Stream Analytics no puede inicializar la salida de SQL Database.

## <a name="sqldatabaseoutputwriteerror"></a>SQLDatabaseOutputWriteError

* **Causa**: Stream Analytics no puede escribir eventos en la salida de SQL Database.

## <a name="sqldwoutputinitializationerror"></a>SQLDWOutputInitializationError

* **Causa**: Ha habido un error al inicializar una salida del grupo de Synapse SQL.

## <a name="sqldwoutputwriteerror"></a>SQLDWOutputWriteError

* **Causa**: Ha habido un error al escribir la salida en un grupo de Synapse SQL.

## <a name="next-steps"></a>Pasos siguientes

* [Solución de problemas de conexiones de entrada](stream-analytics-troubleshoot-input.md)
* [Solución de problemas en las salidas de Azure Stream Analytics](stream-analytics-troubleshoot-output.md)
* [Solución de problemas en las consultas de Azure Stream Analytics](stream-analytics-troubleshoot-query.md)
* [Solución de problemas de Azure Stream Analytics mediante registros de recursos](stream-analytics-job-diagnostic-logs.md)
* [Errores de datos de Azure Stream Analytics](data-errors.md)

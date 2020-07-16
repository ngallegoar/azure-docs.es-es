---
title: 'Códigos de error de datos: Azure Stream Analytics'
description: Solucione problemas de Azure Stream Analytics con códigos de error de datos.
ms.author: mamccrea
author: mamccrea
ms.topic: troubleshooting
ms.date: 05/07/2020
ms.service: stream-analytics
ms.openlocfilehash: 56d7527eebb91bd09895f6cd0238721574df1015
ms.sourcegitcommit: e132633b9c3a53b3ead101ea2711570e60d67b83
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/07/2020
ms.locfileid: "86037331"
---
# <a name="azure-stream-analytics-data-error-codes"></a>Códigos de error de datos de Azure Stream Analytics

Puede usar registros de actividad y registros de recursos para ayudar a depurar comportamientos inesperados desde el trabajo de Azure Stream Analytics. En este artículo se muestra la descripción de cada código de error de datos. Los errores de datos se producen cuando hay datos incorrectos en el flujo, como un esquema de registro inesperado.

## <a name="inputdeserializationerror"></a>InputDeserializationError

* **Causa**: Error al deserializar los datos de entrada.

## <a name="inputeventtimestampnotfound"></a>InputEventTimestampNotFound

* **Causa**: Stream Analytics no puede obtener una marca de tiempo para el recurso. 

## <a name="inputeventtimestampbyovervaluenotfound"></a>InputEventTimestampByOverValueNotFound

* **Causa**: Stream Analytics no puede obtener el valor de `TIMESTAMP BY OVER COLUMN`.

## <a name="inputeventlatebeyondthreshold"></a>InputEventLateBeyondThreshold

* **Causa**: Un evento de entrada se envió más tarde que la tolerancia configurada.

## <a name="inputeventearlybeyondthreshold"></a>InputEventEarlyBeyondThreshold

* **Causa**: La hora de llegada del evento de entrada es anterior al umbral de la marca de tiempo de aplicación del evento de entrada.

## <a name="azurefunctionmessagesizeexceeded"></a>AzureFunctionMessageSizeExceeded

* **Causa**: La salida del mensaje a Azure Functions supera el límite de tamaño.

## <a name="eventhuboutputrecordexceedssizelimit"></a>EventHubOutputRecordExceedsSizeLimit

* **Causa**: Un registro de salida supera el límite de tamaño máximo al escribir en el centro de eventos.

## <a name="cosmosdboutputinvalidid"></a>CosmosDBOutputInvalidId

* **Causa**: El valor o el tipo de una columna determinada no es válido.
* **Recomendación:** Proporcione cadenas no vacías únicas que no tengan más de 255 caracteres.

## <a name="cosmosdboutputinvalididcharacter"></a>CosmosDBOutputInvalidIdCharacter

* **Causa**: El id. de documento del registro de salida contiene un carácter no válido.

## <a name="cosmosdboutputmissingid"></a>CosmosDBOutputMissingId

* **Causa**: El registro de salida no contiene la columna \[id] que se va a usar como propiedad de clave principal.

## <a name="cosmosdboutputmissingidcolumn"></a>CosmosDBOutputMissingIdColumn

* **Causa**: El registro de salida no contiene la propiedad de id. de documento. 
* **Recomendación:** Asegúrese de que el resultado de la consulta contenga la columna con una cadena única que no está vacía y que tenga menos de 255 caracteres.

## <a name="cosmosdboutputmissingpartitionkey"></a>CosmosDBOutputMissingPartitionKey

* **Causa**: En el registro de salida falta la columna que se va a usar como propiedad de clave de partición.

## <a name="cosmosdboutputsinglerecordtoolarge"></a>CosmosDBOutputSingleRecordTooLarge

* **Causa**: Una escritura de un solo registro en Cosmos DB es demasiado grande.

## <a name="sqldatabaseoutputdataerror"></a>SQLDatabaseOutputDataError

* **Causa**: Stream Analytics no puede escribir eventos en SQL Database debido a ciertos problemas detectados en los datos.

## <a name="next-steps"></a>Pasos siguientes

* [Solución de problemas de conexiones de entrada](stream-analytics-troubleshoot-input.md)
* [Solución de problemas en las salidas de Azure Stream Analytics](stream-analytics-troubleshoot-output.md)
* [Solución de problemas en las consultas de Azure Stream Analytics](stream-analytics-troubleshoot-query.md)
* [Solución de problemas de Azure Stream Analytics mediante registros de recursos](stream-analytics-job-diagnostic-logs.md)
* [Errores de datos de Azure Stream Analytics](data-errors.md)

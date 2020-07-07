---
title: 'Códigos de error de configuración: Azure Stream Analytics'
description: Solucione problemas de Azure Stream Analytics con códigos de error de configuración.
ms.author: mamccrea
author: mamccrea
ms.topic: conceptual
ms.date: 05/07/2020
ms.service: stream-analytics
ms.openlocfilehash: 5aa15ae4a234a56a172a0166070c32be4f822910
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "83650077"
---
# <a name="azure-stream-analytics-configuration-error-codes"></a>Códigos de error de configuración de Azure Stream Analytics

Puede usar registros de actividad y registros de recursos para ayudar a depurar comportamientos inesperados desde el trabajo de Azure Stream Analytics. En este artículo se muestra la descripción de cada código de error de configuración. Los errores de configuración están relacionados con la configuración del trabajo o con las configuraciones de entrada y salida.

## <a name="eventhubunauthorizedaccess"></a>EventHubUnauthorizedAccess

* **Causa**: Event Hubs produjo un error de *acceso no autorizado*.

## <a name="eventhubreceiverepochconflict"></a>EventHubReceiverEpochConflict

* **Causa**: hay más de un receptor de Event Hubs con valores de épocas diferentes.
* **Recomendación:** asegúrese de que una aplicación *Service Bus Explorer* o *EventProcessorHost* no esté conectada mientras se ejecuta el trabajo de Stream Analytics.

## <a name="eventhubreceiverquotaexceeded"></a>EventHubReceiverQuotaExceeded

* **Causa**: Stream Analytics no se puede conectar a una partición porque se alcanzó el número máximo de receptores permitidos por partición en un grupo de consumidores.
* **Recomendación:** asegúrese de que otros trabajos de Stream Analytics o Service Bus Explorer no utilicen el mismo grupo de consumidores.

## <a name="eventhuboutputthrottled"></a>EventHubOutputThrottled

* **Causa**: se produjo un error al escribir datos en Event Hubs debido a la limitación.
* **Recomendación:** si esto ocurre de forma constante, actualice el rendimiento.

## <a name="eventhuboutputinvalidconnectionconfig"></a>EventHubOutputInvalidConnectionConfig

* **Causa**: la configuración de conexión proporcionada es incorrecta.
* **Recomendación:** corrija la configuración y reinicie el trabajo.

## <a name="eventhuboutputinvalidhostname"></a>EventHubOutputInvalidHostname

* **Causa**: no se puede acceder al host de Event Hubs.
* **Recomendación:** asegúrese de que el nombre de host proporcionado es correcto.

## <a name="eventhuboutputunexpectedpartitioncount"></a>EventHubOutputUnexpectedPartitionCount

* **Causa**: el remitente de EventHub encontró un recuento de particiones de EventHub inesperado.
* **Recomendación:** reinicie el trabajo de Stream Analytics si el número de particiones de EventHub ha cambiado.

## <a name="cosmosdbpartitionkeynotfound"></a>CosmosDBPartitionKeyNotFound

* **Causa**: Stream Analytics no encontró la clave de partición de una colección Cosmos DB determinada en la base de datos.
* **Recomendación:** asegúrese de que hay una clave de partición válida especificada para la colección en Cosmos DB.

## <a name="cosmosdbinvalidpartitionkeycolumn"></a>CosmosDBInvalidPartitionKeyColumn

* **Causa**: se produce cuando una clave de partición no es un nodo hoja ni se encuentra en el nivel superior.

## <a name="cosmosdbinvalididcolumn"></a>CosmosDBInvalidIdColumn

* **Causa**: la salida de la consulta no puede contener la columna \[id] si se elige una columna diferente como propiedad de la clave principal.

## <a name="cosmosdbdatabasenotfound"></a>CosmosDBDatabaseNotFound

* **Causa**: Stream Analytics no encuentra ninguna base de datos de CosmosDB.

## <a name="cosmosdbcollectionnotfound"></a>CosmosDBCollectionNotFound

* **Causa**: Stream Analytics no encuentra una colección de Cosmos DB determinada en una base de datos.

## <a name="cosmosdboutputwritethrottling"></a>CosmosDBOutputWriteThrottling

* **Causa**: se produjo un error al escribir datos debido a la limitación de Cosmos DB.
* **Recomendación:** actualice el nivel de rendimiento de la colección y ajuste el rendimiento de la base de datos.

## <a name="sqldatabaseconnectionstringerror"></a>SQLDatabaseConnectionStringError

* **Causa**: el trabajo de Stream Analytics detectó un error de autenticación.
* **Recomendación:** Asegúrese de que la cadena de conexión de SQL Database sea correcta.

## <a name="sqldatabasemanagedidentityauthenticationerror"></a>SQLDatabaseManagedIdentityAuthenticationError

* **Causa**: el trabajo de Stream Analytics ha encontrado un error de autenticación. 
* **Recomendación:** asegúrese de que el nombre de la cuenta esté configurado correctamente y de que la identidad administrada del trabajo tenga acceso a SQL Database.

## <a name="sqldatabaseoutputnotableerror"></a>SQLDatabaseOutputNoTableError

* **Causa**: Stream Analytics no encuentra la información de esquema de una tabla determinada.

## <a name="sqldwoutputinvalidserviceedition"></a>SQLDWOutputInvalidServiceEdition

* **Causa**: SQL Database no se admite.
* **Recomendación:** use el grupo de Synapse SQL.

## <a name="next-steps"></a>Pasos siguientes

* [Solución de problemas de conexiones de entrada](stream-analytics-troubleshoot-input.md)
* [Solución de problemas en las salidas de Azure Stream Analytics](stream-analytics-troubleshoot-output.md)
* [Solución de problemas en las consultas de Azure Stream Analytics](stream-analytics-troubleshoot-query.md)
* [Solución de problemas de Azure Stream Analytics mediante registros de recursos](stream-analytics-job-diagnostic-logs.md)
* [Errores de datos de Azure Stream Analytics](data-errors.md)

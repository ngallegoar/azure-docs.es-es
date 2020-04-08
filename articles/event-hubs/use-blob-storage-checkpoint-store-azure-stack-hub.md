---
title: Uso de Blob Storage como almacén de puntos de comprobación en Azure Stack Hub (versión preliminar)
description: En este artículo se describe cómo usar Blob Storage como almacén de puntos de comprobación en Event Hubs en Azure Stack Hub (versión preliminar).
services: event-hubs
documentationcenter: na
author: spelluru
ms.service: event-hubs
ms.topic: how-to
ms.date: 03/18/2020
ms.author: spelluru
ms.openlocfilehash: 1f0e4dea44007ef82cb4b700ff0be4a5579541d8
ms.sourcegitcommit: 632e7ed5449f85ca502ad216be8ec5dd7cd093cb
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/30/2020
ms.locfileid: "80398757"
---
# <a name="use-blob-storage-as-checkpoint-store---event-hubs-on-azure-stack-hub-preview"></a>Uso de Blob Storage como almacén de puntos de comprobación: Event Hubs en Azure Stack Hub (versión preliminar)
Si usa Azure Blob Storage como almacén de puntos de comprobación en un entorno que admite una versión diferente del SDK de blobs de almacenamiento que las que normalmente están disponibles en Azure, tendrá que utilizar código para cambiar la versión de la API del servicio de almacenamiento a la versión admitida por ese entorno. Por ejemplo, si ejecuta [Event Hubs en una instancia de Azure Stack Hub versión 2002](https://docs.microsoft.com/azure-stack/user/event-hubs-overview), la versión más alta disponible para el servicio de almacenamiento es la 2017-11-09. En este caso, tendrá que usar código para establecer como destino la versión de la API del servicio de almacenamiento en 2017-11-09. Para obtener un ejemplo de cómo establecer como destino una versión específica de la API de almacenamiento, vea estos ejemplos en GitHub: 

- [.NET](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/eventhub/Azure.Messaging.EventHubs.Processor/samples/Sample10_RunningWithDifferentStorageVersion.cs)
- [Java](https://github.com/Azure/azure-sdk-for-java/blob/master/sdk/eventhubs/azure-messaging-eventhubs-checkpointstore-blob/src/samples/java/com/azure/messaging/eventhubs/checkpointstore/blob/EventProcessorWithOlderStorageVersion.java). 
- [JavaScript](https://github.com/Azure/azure-sdk-for-js/blob/master/sdk/eventhub/eventhubs-checkpointstore-blob/samples/receiveEventsWithDownleveledStorage.js) o [TypeScript](https://github.com/Azure/azure-sdk-for-js/blob/master/sdk/eventhub/eventhubs-checkpointstore-blob/samples/receiveEventsWithDownleveledStorage.ts), 
- Python: [sincrónico](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/eventhub/azure-eventhub-checkpointstoreblob/samples/event_processor_blob_storage_example_with_storage_api_version.py), [asincrónico](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/eventhub/azure-eventhub-checkpointstoreblob-aio/samples/event_processor_blob_storage_example_with_storage_api_version.py)

> [!IMPORTANT]
> Event Hubs en Azure Stack Hub se encuentra actualmente en [versión preliminar](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) y es gratuito. 

Si ejecuta un receptor de Event Hubs que usa Blob Storage como almacén de puntos de comprobación sin tener como destino la versión que admite Azure Stack Hub, recibirá el mensaje de error siguiente:

```
The value for one of the HTTP headers is not in the correct format
```


## <a name="sample-error-message-in-python"></a>Ejemplo de mensaje de error en Python
En el caso de Python, se pasa un error de `azure.core.exceptions.HttpResponseError` al controlador de errores `on_error(partition_context, error)` de `EventHubConsumerClient.receive()`. Sin embargo, el método `receive()` no inicia una excepción. `print(error)` imprimirá la siguiente información de excepción:

```bash
The value for one of the HTTP headers is not in the correct format.

RequestId:f048aee8-a90c-08ba-4ce1-e69dba759297
Time:2020-03-17T22:04:13.3559296Z
ErrorCode:InvalidHeaderValue
Error:None
HeaderName:x-ms-version
HeaderValue:2019-07-07
```

El registrador registrará dos advertencias como las siguientes:

```bash
WARNING:azure.eventhub.extensions.checkpointstoreblobaio._blobstoragecsaio: 
An exception occurred during list_ownership for namespace '<namespace-name>.eventhub.<region>.azurestack.corp.microsoft.com' eventhub 'python-eh-test' consumer group '$Default'. 

Exception is HttpResponseError('The value for one of the HTTP headers is not in the correct format.\nRequestId:f048aee8-a90c-08ba-4ce1-e69dba759297\nTime:2020-03-17T22:04:13.3559296Z\nErrorCode:InvalidHeaderValue\nError:None\nHeaderName:x-ms-version\nHeaderValue:2019-07-07')

WARNING:azure.eventhub.aio._eventprocessor.event_processor:EventProcessor instance '26d84102-45b2-48a9-b7f4-da8916f68214' of eventhub 'python-eh-test' consumer group '$Default'. An error occurred while load-balancing and claiming ownership. 

The exception is HttpResponseError('The value for one of the HTTP headers is not in the correct format.\nRequestId:f048aee8-a90c-08ba-4ce1-e69dba759297\nTime:2020-03-17T22:04:13.3559296Z\nErrorCode:InvalidHeaderValue\nError:None\nHeaderName:x-ms-version\nHeaderValue:2019-07-07'). Retrying after 71.45254944090853 seconds
```



## <a name="next-steps"></a>Pasos siguientes

Vea el artículo siguiente para obtener información sobre la creación de particiones y puntos de comprobación: [Equilibrio de carga de particiones entre varias instancias de una aplicación](event-processor-balance-partition-load.md)

---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 09/04/2018
ms.author: glenga
ms.openlocfilehash: 8f3a58d3a7470867ab23249bbd645289e010ad89
ms.sourcegitcommit: a43a59e44c14d349d597c3d2fd2bc779989c71d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/25/2020
ms.locfileid: "95997325"
---
### <a name="functions-2x-and-higher"></a>Functions 2.x y superiores

```json
{
    "version": "2.0",
    "extensions": {
        "eventHubs": {
            "batchCheckpointFrequency": 5,
            "eventProcessorOptions": {
                "maxBatchSize": 256,
                "prefetchCount": 512
            }
        }
    }
}  
```

|Propiedad  |Valor predeterminado | Descripción |
|---------|---------|---------|
|maxBatchSize|10|Número máximo de eventos recibido por cada bucle de recepción.|
|prefetchCount|300|Número predeterminado de capturas previas utilizado por el elemento `EventProcessorHost` subyacente. El valor mínimo permitido es 10.|
|batchCheckpointFrequency|1|Número de lotes de eventos que se va a procesar antes de crear un punto de comprobación de cursor de EventHub.|

> [!NOTE]
> Para una referencia de host.json en Azure Functions 2.x y versiones posteriores, consulte [Referencia de host.json para Azure Functions](../articles/azure-functions/functions-host-json.md).

### <a name="functions-1x"></a>Functions 1.x

```json
{
    "eventHub": {
      "maxBatchSize": 64,
      "prefetchCount": 256,
      "batchCheckpointFrequency": 1
    }
}
```

|Propiedad  |Valor predeterminado | Descripción |
|---------|---------|---------| 
|maxBatchSize|64|Número máximo de eventos recibido por cada bucle de recepción.|
|prefetchCount|N/D|Número predeterminado de capturas previas que utilizará el elemento `EventProcessorHost` subyacente.| 
|batchCheckpointFrequency|1|Número de lotes de eventos que se va a procesar antes de crear un punto de comprobación de cursor de EventHub.| 

> [!NOTE]
> Para una referencia de host.json en Azure Functions 1.x, consulte [Referencia de host.json para Azure Functions 1.x](../articles/azure-functions/functions-host-json-v1.md).

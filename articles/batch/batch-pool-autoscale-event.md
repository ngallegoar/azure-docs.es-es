---
title: Evento de escalabilidad automática de grupo de Azure Batch
description: Referencia del evento de escalabilidad automática de grupo de Batch, que se emite una vez que se ejecuta la escalabilidad automática de un grupo. El contenido del registro expondrá la fórmula de la escalabilidad automática y los resultados de la evaluación para el grupo.
ms.topic: reference
ms.date: 10/08/2020
ms.openlocfilehash: e548ed484e5f683a8f79434ce4095ac66900f01a
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "91852080"
---
# <a name="pool-autoscale-event"></a>Evento de escalabilidad automática de grupo

 Este evento se emite una vez que se ejecuta el escalado automático de un grupo. El contenido del registro expondrá la fórmula de la escalabilidad automática y los resultados de la evaluación para el grupo.

 En el ejemplo siguiente se muestra el cuerpo de un evento de escalabilidad automática para el escalado automático de un grupo que no se pudo realizar debido a que los datos de ejemplo eran insuficientes.

```
{
    "id": "myPool1",
    "timestamp": "2020-09-21T18:59:56.204Z",
    "formula": "...",
    "results": "...",
    "error": {
        "code": "InsufficientSampleData",
        "message": "Autoscale evaluation failed due to insufficient sample data",
        "values": [{
                "name": "Message",
                "value": "Line 15, Col 44: Insufficient data from data set: $RunningTasks wanted 70%, received 50%"
            }
        ]
    }
}
```

|Elemento|Tipo|Notas|
|-------------|----------|-----------|
|`id`|String|El identificador del grupo.|
|`timestamp`|DateTime|Marca de tiempo del momento en el que se ejecuta el escalado automático.|
|`formula`|String|Fórmula definida para el escalado automático.|
|`results`|String|Resultados de la evaluación de todas las variables utilizadas en la fórmula.|
|[`error`](#error)|Tipo complejo|Error detallado para el escalado automático.|

###  <a name="error"></a><a name="error"></a> con error

|Nombre del elemento|Tipo|Notas|
|------------------|----------|-----------|
|`code`|String|Identificador del error de escalado automático. Los códigos son invariables y están diseñados para consumirse mediante programación.|
|`message`|String|Mensaje que describe el error de escalado automático, diseñado para que sea adecuado para su presentación en una interfaz de usuario.|
|`values`|Array|Lista de pares nombre-valor que describen más detalles del error de escalado automático.|

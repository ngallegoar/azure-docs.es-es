---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 10/18/2020
ms.author: glenga
ms.openlocfilehash: 9213bdd8e63fc1e8ab7bdba8ac7a569be0dfe6be
ms.sourcegitcommit: 7cc10b9c3c12c97a2903d01293e42e442f8ac751
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/06/2020
ms.locfileid: "93424850"
---
Ejecute el siguiente comando para ver [registros de streaming](../articles/azure-functions/functions-run-local.md#enable-streaming-logs) casi en tiempo real:

```console
func azure functionapp logstream <APP_NAME> 
```

En una ventana de terminal independiente o en el explorador, vuelva a llamar a la función remota. En la ventana de terminal, se mostrará un registro detallado de la ejecución de la función en Azure. 
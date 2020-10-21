---
title: Evento de error en programación de tareas de Azure Batch
description: Referencia del evento de error en programación de tareas de Azure Batch. Este evento se emite cuando no se pudo programar una tarea y se volverá a intentar más tarde.
ms.topic: reference
ms.date: 09/20/2020
ms.openlocfilehash: 549281d2b2c371e8f09c584e771cf44f7abc8a00
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "91852082"
---
# <a name="task-schedule-fail-event"></a>Evento de error en programación de tareas

 Este evento se emite cuando no se pudo programar una tarea y se volverá a intentar más tarde. Se trata de un error temporal en el momento de la programación de tareas debido a la limitación de recursos, como que no haya suficientes ranuras disponibles en los nodos para ejecutar una tarea con `requiredSlots` especificado.

 En el ejemplo siguiente se muestra el cuerpo de un evento de error en programación de tareas.

```
{
    "jobId": "job-01",
    "id": "task-01",
    "taskType": "User",
    "systemTaskVersion": 665378862,
    "requiredSlots": 1,
    "nodeInfo": {
        "poolId": "pool-01",
        "nodeId": " "
    },
    "multiInstanceSettings": {
        "numberOfInstances": 1
    },
    "constraints": {
        "maxTaskRetryCount": 0
    },
    "schedulingError": {
        "category": "UserError",
        "code": "JobPreparationTaskFailed",
        "message": "Task cannot run because the job preparation task failed on node"
    }
}
```

|Nombre del elemento|Tipo|Notas|
|------------------|----------|-----------|
|`jobId`|String|Identificador del trabajo que contiene la tarea.|
|`id`|String|Identificador de la tarea.|
|`taskType`|String|Tipo de la tarea. Puede ser "JobManager", que indica que es una tarea del administrador de trabajos, o "User", que indica que no lo es. Este evento no se emite para tareas de preparación de trabajos, tareas de liberación de trabajo ni tareas de inicio.|
|`systemTaskVersion`|Int32|Se trata del contador interno de reintentos de una tarea. De manera interna, el servicio de Batch puede reintentar una tarea para tener en cuenta los problemas transitorios. Estos problemas pueden incluir errores internos de programación o intentos de recuperación a partir de nodos de proceso en estado no válido.|
|`requiredSlots`|Int32|Ranuras necesarias para ejecutar la tarea.|
|[`nodeInfo`](#nodeInfo)|Tipo complejo|Contiene información sobre el nodo de ejecución en que se ejecutó la tarea.|
|[`multiInstanceSettings`](#multiInstanceSettings)|Tipo complejo|Especifica que la tarea es una tarea de instancias múltiples que requiere varios nodos de proceso.  Pulse [`multiInstanceSettings`](/rest/api/batchservice/get-information-about-a-task) para ver los detalles.|
|[`constraints`](#constraints)|Tipo complejo|Restricciones de ejecución que se aplican a esta tarea.|
|[`schedulingError`](#schedulingError)|Tipo complejo|Contiene información sobre el error de programación de la tarea.|

###  <a name="nodeinfo"></a><a name="nodeInfo"></a> nodeInfo

|Nombre del elemento|Tipo|Notas|
|------------------|----------|-----------|
|`poolId`|String|Identificador del grupo en el que se ejecutó la tarea.|
|`nodeId`|String|Identificador del nodo en el que se ejecutó la tarea.|

###  <a name="multiinstancesettings"></a><a name="multiInstanceSettings"></a> multiInstanceSettings

|Nombre del elemento|Tipo|Notas|
|------------------|----------|-----------|
|`numberOfInstances`|Int32|Número de nodos de proceso que requiere la tarea.|

###  <a name="constraints"></a><a name="constraints"></a> constraints

|Nombre del elemento|Tipo|Notas|
|------------------|----------|-----------|
|`maxTaskRetryCount`|Int32|Número máximo de veces que se puede reintentar la tarea. El servicio de Batch reintenta una tarea su el código de salida es distinto de cero.<br /><br /> Tenga en cuenta que este valor controla específicamente el número de reintentos. El servicio de Batch intentará una vez la tarea y podría reintentarla hasta alcanzar este límite. Por ejemplo, si el conteo de reintentos máximo es 3, Batch intenta una tarea hasta 4 veces (un intento inicial y 3 reintentos).<br /><br /> Si el conteo de intentos máximo es 0, el servicio de Batch no reintenta las tareas.<br /><br /> Si el conteo de intentos máximo es -1, el servicio de Batch reintenta las tareas sin ningún límite.<br /><br /> El valor predeterminado es 0 (sin ningún reintento).|


###  <a name="schedulingerror"></a><a name="schedulingError"></a> schedulingError

|Nombre del elemento|Tipo|Notas|
|------------------|----------|-----------|
|`category`|String|Categoría del error.|
|`code`|String|Identificador del error de programación de tareas. Los códigos son invariables y están diseñados para consumirse mediante programación.|
|`message`|String|Mensaje que describe el error en la programación de tareas, diseñado para que sea adecuado para su presentación en una interfaz de usuario.|

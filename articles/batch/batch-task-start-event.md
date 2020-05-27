---
title: Evento de inicio de tarea de Azure Batch
description: Información de referencia del evento de inicio de tarea de Batch. Este evento se emite una vez que el programador programa que una tarea se inicie en un nodo de ejecución.
ms.topic: reference
ms.date: 04/20/2017
ms.openlocfilehash: f872e6c89e2f528729ae08125c6cc3eb5718e19a
ms.sourcegitcommit: 6fd8dbeee587fd7633571dfea46424f3c7e65169
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/21/2020
ms.locfileid: "83725795"
---
# <a name="task-start-event"></a>Evento de inicio de tarea

 Este evento se emite una vez que el programador programa que una tarea se inicie en un nodo de ejecución. Tenga en cuenta que si la tarea se reintenta o se vuelve a poner en cola, este evento se volverá a emitir para la misma tarea, pero el conteo de reintentos y la versión de tarea del sistema se actualizarán en consecuencia.


 En el ejemplo siguiente se muestra el cuerpo de un evento de inicio de tareas.

```
{
    "jobId": "myJob",
    "id": "myTask",
    "taskType": "User",
    "systemTaskVersion": 220192842,
    "nodeInfo": {
        "poolId": "pool-001",
        "nodeId": "tvm-257509324_1-20160908t162728z"
    },
    "multiInstanceSettings": {
        "numberOfInstances": 1
    },
    "constraints": {
        "maxTaskRetryCount": 2
    },
    "executionInfo": {
        "retryCount": 0
    }
}
```

|Nombre del elemento|Tipo|Notas|
|------------------|----------|-----------|
|`jobId`|String|Identificador del trabajo que contiene la tarea.|
|`id`|String|Identificador de la tarea.|
|`taskType`|String|Tipo de la tarea. Puede ser "JobManager", que indica que es una tarea del administrador de trabajos, o "User", que indica que no lo es.|
|`systemTaskVersion`|Int32|Se trata del contador interno de reintentos de una tarea. De manera interna, el servicio de Batch puede reintentar una tarea para tener en cuenta los problemas transitorios. Estos problemas pueden incluir errores internos de programación o intentos de recuperación a partir de nodos de proceso en estado no válido.|
|[`nodeInfo`](#nodeInfo)|Tipo complejo|Contiene información sobre el nodo de ejecución en que se ejecutó la tarea.|
|[`multiInstanceSettings`](#multiInstanceSettings)|Tipo complejo|Especifica que la tarea es una tarea de instancias múltiples que requiere varios nodos de proceso.  Consulte [multiInstanceSettings](https://docs.microsoft.com/rest/api/batchservice/get-information-about-a-task) para detalles.|
|[`constraints`](#constraints)|Tipo complejo|Restricciones de ejecución que se aplican a esta tarea.|
|[`executionInfo`](#executionInfo)|Tipo complejo|Contiene información sobre la ejecución de la tarea.|

###  <a name="nodeinfo"></a><a name="nodeInfo"></a> nodeInfo

|Nombre del elemento|Tipo|Notas|
|------------------|----------|-----------|
|`poolId`|String|Identificador del grupo en el que se ejecutó la tarea.|
|`nodeId`|String|Identificador del nodo en el que se ejecutó la tarea.|

###  <a name="multiinstancesettings"></a><a name="multiInstanceSettings"></a> multiInstanceSettings

|Nombre del elemento|Tipo|Notas|
|------------------|----------|-----------|
|`numberOfInstances`|Int|Número de nodos de proceso que requiere la tarea.|

###  <a name="constraints"></a><a name="constraints"></a> constraints

|Nombre del elemento|Tipo|Notas|
|------------------|----------|-----------|
|`maxTaskRetryCount`|Int32|Número máximo de veces que se puede reintentar la tarea. El servicio de Batch reintenta una tarea su el código de salida es distinto de cero.<br /><br /> Tenga en cuenta que este valor controla específicamente el número de reintentos. El servicio de Batch intentará una vez la tarea y podría reintentarla hasta alcanzar este límite. Por ejemplo, si el conteo de reintentos máximo es 3, Batch intenta una tarea hasta 4 veces (un intento inicial y 3 reintentos).<br /><br /> Si el conteo de intentos máximo es 0, el servicio de Batch no reintenta las tareas.<br /><br /> Si el conteo de intentos máximo es -1, el servicio de Batch reintenta las tareas sin ningún límite.<br /><br /> El valor predeterminado es 0 (sin ningún reintento).|

###  <a name="executioninfo"></a><a name="executionInfo"></a> executionInfo

|Nombre del elemento|Tipo|Notas|
|------------------|----------|-----------|
|`retryCount`|Int32|Cantidad de veces que el servicio de Batch reintentó la tarea. La tarea se reintenta si el código de salida es distinto de cero, hasta el valor MaxTaskRetryCount especificado|

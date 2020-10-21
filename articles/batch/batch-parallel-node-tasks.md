---
title: Ejecución de tareas en paralelo para optimizar los recursos de proceso
description: Aumente la eficiencia y reduzca los costos usando menos nodos de proceso y ejecutando tareas simultáneas en cada nodo de un grupo de Azure Batch
ms.topic: how-to
ms.date: 10/08/2020
ms.custom: H1Hack27Feb2017, devx-track-csharp
ms.openlocfilehash: 3c3a81aa624ccc67c0f9e8ec23e5ef9b8e61c724
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "91851006"
---
# <a name="run-tasks-concurrently-to-maximize-usage-of-batch-compute-nodes"></a>Ejecución simultánea de tareas para maximizar el uso de los nodos de proceso de Batch

A través de la ejecución simultánea de más de una tarea en cada nodo de proceso dentro del grupo de Azure Batch, puede maximizar el uso de recursos en un menor número de nodos en el grupo. Para algunas cargas de trabajo, esto puede reducir los costos y el tiempo dedicado al trabajo.

Aunque en algunos casos puede resultar beneficioso que todos los recursos de un nodo estén dedicados a una sola tarea, en otras situaciones será conveniente permitir que varias tareas compartan esos recursos:

* **Minimizar la transferencia de datos** cuando las tareas son capaces de compartir datos. En este escenario, puede reducir considerablemente los gastos de transferencia de datos copiando datos compartidos en un número menor de nodos y ejecutando tareas en paralelo en cada nodo. Esto es válido especialmente si los datos que se copian en cada nodo deben transferirse entre regiones geográficas.
* **Maximizar el uso de memoria** cuando las tareas requieren una gran cantidad de memoria, pero solo durante períodos breves y en momentos variables durante la ejecución. Puede emplear menos nodos de ejecución, pero de mayor tamaño y con más memoria, para controlar de forma eficaz dichos aumentos. Estos nodos tendrían varias tareas ejecutándose en paralelo en cada nodo, pero cada tarea aprovecharía la abundante memoria de los nodos en distintos momentos.
* **Mitigar los límites de número de nodos** cuando se requiere la comunicación entre nodos dentro de un grupo. Actualmente, los grupos configurados para la comunicación entre nodos están limitados a 50 nodos de ejecución. Si cada uno de los nodos de este tipo de grupo es capaz de ejecutar tareas en paralelo, el número de tareas que se podrán ejecutar simultáneamente será mayor.
* **Replicar en un clúster de proceso local**, como cuando mueve por primera vez un entorno de procesos a Azure. Si la solución local existente ejecuta varias tareas en cada nodo de proceso, puede aumentar el número máximo de tareas de nodo para que refleje con mayor fidelidad esa configuración.

## <a name="example-scenario"></a>Escenario de ejemplo
Para ilustrar las ventajas de la ejecución de tareas en paralelo, imaginemos que la aplicación de la tarea tiene unos requisitos de CPU y memoria que hacen que el tamaño de nodo [Standard\_StandardD1](../cloud-services/cloud-services-sizes-specs.md) sea suficiente. Pero, para ejecutar el trabajo en el tiempo requerido, se necesitan 1.000 nodos de ese tipo.

En lugar de utilizar nodos Standard\_D1 con un núcleo de CPU, podría utilizar nodos [Standard\_D14](../cloud-services/cloud-services-sizes-specs.md) con 16 núcleos en cada nodo y habilitar la ejecución de tareas en paralelo. En este caso, se podría usar un *número de nodos 16 veces menor* ; es decir, en lugar de 1000 nodos, solo serían necesarios 63. Además, si para cada nodo son necesarios datos de referencia o archivos de aplicación de gran tamaño, la eficiencia y la duración del trabajo también se mejoran, ya que los datos se copian en solo 63 nodos.

## <a name="enable-parallel-task-execution"></a>Habilitación de la ejecución en paralelo de tareas
Los nodos de proceso para la ejecución en paralelo de tareas se configuran a nivel de grupo. Con la biblioteca .NET de Batch, establezca la propiedad [CloudPool.TaskSlotsPerNode][maxtasks_net] al crear un grupo. Si usa la API REST de Batch, establezca el elemento [taskSlotsPerNode][rest_addpool] en el cuerpo de la solicitud durante la creación del grupo.

Azure Batch permite una configuración de ranuras de tarea por nodo que cuadriplica el número de núcleos de nodo. Por ejemplo, si el grupo está configurado con nodos de tamaño "Grande" (cuatro núcleos), `taskSlotsPerNode` se puede establecer en 16. Sin embargo, independientemente de cuántos núcleos tiene el nodo, no puede tener más de 256 ranuras de tarea por nodo. Para más información sobre el número de núcleos de cada uno de los tamaños de nodo, consulte [Tamaños de Cloud Services](../cloud-services/cloud-services-sizes-specs.md). Para más información sobre los límites del servicio, consulte [Cuotas y límites del servicio Azure Batch](batch-quota-limit.md).

> [!TIP]
> Asegúrese de tener en cuenta el valor `taskSlotsPerNode` al construir una [fórmula de escalado automático][enable_autoscaling] para el grupo. Por ejemplo, una fórmula que evalúe `$RunningTasks` podría verse afectada considerablemente por un aumento en las tareas por nodo. Consulte [Escalación automática de los nodos de ejecución en un grupo de Azure Batch](batch-automatic-scaling.md) para obtener más información.
>
>

> [!NOTE]
> Solo puede establecer el elemento `taskSlotsPerNode` y la propiedad [TaskSlotsPerNode][maxtasks_net] en el momento de crear el grupo. No se pueden modificar después de haberlos creado.
>
>

## <a name="distribution-of-tasks"></a>Distribución de tareas
Cuando los nodos de proceso dentro de un grupo pueden ejecutar tareas de forma simultánea, es importante especificar cómo desea que se distribuyan las tareas entre los nodos del grupo.

Mediante la propiedad [CloudPool.TaskSchedulingPolicy][task_schedule], puede especificar que las tareas se deberían asignar de manera uniforme entre todos los nodos del grupo ("propagación"). O bien, puede especificar que se deberían asignar todas las tareas posibles a cada nodo antes de asignarlas a otro nodo del grupo ("empaquetado").

Como ejemplo de por qué esta característica es importante, considere el grupo de nodos [Standard\_D14](../cloud-services/cloud-services-sizes-specs.md) (en el ejemplo anterior) configurado con un valor [CloudPool.TaskSlotsPerNode][maxtasks_net] de 16. Si [CloudPool.TaskSchedulingPolicy][task_schedule] se configura con un [ComputeNodeFillType][fill_type] de *Pack*, se podría maximizar el uso de los 16 núcleos de cada nodo y permitir que un [grupo con escalado automático](batch-automatic-scaling.md) elimine los nodos sin usar del grupo (nodos sin tareas asignadas). Esto minimiza el uso de recursos y permite ahorrar dinero.

## <a name="variable-slots-per-task"></a>Ranuras variables por tarea
La tarea se puede definir con la propiedad [CloudTask.RequiredSlots][taskslots_net] para especificar el número de ranuras que requiere para ejecutarse en un nodo de proceso, con el valor predeterminado de 1. Puede establecer ranuras de tareas variables si las tareas tienen pesos diferentes en cuanto al uso de recursos en el nodo de ejecución, de manera que cada nodo de proceso puede tener un número razonable de tareas en ejecución simultáneas sin saturar recursos del sistema, como la CPU o la memoria.

Por ejemplo, en el caso de un grupo con la propiedad `taskSlotsPerNode = 8`, puede enviar tareas de uso intensivo de CPU requeridas de varios núcleos con `requiredSlots = 8`, y otras tareas con `requiredSlots = 1`. Cuando esta carga de trabajo mixta está programada para el grupo, las tareas de uso intensivo de CPU se ejecutarán exclusivamente en el nodo de ejecución, mientras que otras tareas se pueden ejecutar simultáneamente (hasta ocho tareas) en otros nodos. Esto le ayudará a equilibrar la carga de trabajo entre los nodos de ejecución y mejorar la eficacia del uso de recursos.

> [!TIP]
> Cuando se usan ranuras de tareas variables, es posible que las tareas grandes con más ranuras necesarias no se puedan programar temporalmente debido a que no hay suficientes ranuras disponibles en ningún nodo de ejecución, incluso cuando todavía hay ranuras inactivas en algunos nodos. Puede subir la prioridad del trabajo para estas tareas con el fin de aumentar su oportunidad de competir por las ranuras disponibles en los nodos.
>
> El servicio Batch también emite [TaskScheduleFailEvent](batch-task-schedule-fail-event.md) cuando no puede programar una tarea para que se ejecute, y sigue reintentando la programación hasta que estén disponibles las ranuras necesarias. Puede escuchar ese evento para detectar el posible problema de programación de tareas atascada y llevar a cabo su mitigación en consecuencia.
>

> [!NOTE]
> No especifique `requiredSlots` de la tarea de forma que sea mayor que `taskSlotsPerNode`del grupo. Esto hará que la tarea nunca pueda ejecutarse. Actualmente, el servicio Batch no realiza esta validación cuando envía tareas, porque es posible que el trabajo no tenga un grupo enlazado en el momento del envío, o que se cambie a un grupo diferente mediante si se deshabilita o se vuelve a habilitar.
>

## <a name="batch-net-example"></a>Ejemplo de Batch .NET
En los siguientes fragmentos de código de la API de [.NET para Batch][api_net] se muestra cómo crear un grupo con varias ranuras de tarea por nodo y cómo enviar una tarea con las ranuras necesarias.

### <a name="create-pool"></a>Creación de un grupo
En este fragmento de código se muestra una solicitud para crear un grupo que contiene cuatro nodos con cuatro ranuras de tarea por nodo. Se especifica una directiva de programación de tareas que llenará cada nodo de tareas antes de asignarlas a otro nodo del grupo. Para obtener más información sobre cómo agregar grupos mediante la API de .NET para Batch, consulte [BatchClient.PoolOperations.CreatePool][poolcreate_net].

```csharp
CloudPool pool =
    batchClient.PoolOperations.CreatePool(
        poolId: "mypool",
        targetDedicatedComputeNodes: 4
        virtualMachineSize: "standard_d1_v2",
        cloudServiceConfiguration: new CloudServiceConfiguration(osFamily: "5"));

pool.TaskSlotsPerNode = 4;
pool.TaskSchedulingPolicy = new TaskSchedulingPolicy(ComputeNodeFillType.Pack);
pool.Commit();
```

### <a name="create-task-with-required-slots"></a>Creación de una tarea con las ranuras necesarias
Este fragmento de código crea una tarea con `requiredSlots` no predeterminado. Esta tarea solo se ejecutará cuando haya suficientes ranuras libres disponibles en el nodo de ejecución.
```csharp
CloudTask task = new CloudTask(taskId, taskCommandLine)
{
    RequiredSlots = 2
};
```

### <a name="list-compute-nodes-with-counts-for-running-tasks-and-slots"></a>Enumeración de los nodos de ejecución con recuentos para ejecutar tareas y ranuras
En este fragmento de código se enumeran todos los nodos de ejecución del grupo y se imprimen los recuentos para ejecutar tareas y ranuras de tarea por nodo.
```csharp
ODATADetailLevel nodeDetail = new ODATADetailLevel(selectClause: "id,runningTasksCount,runningTaskSlotsCount");
IPagedEnumerable<ComputeNode> nodes = batchClient.PoolOperations.ListComputeNodes(poolId, nodeDetail);

await nodes.ForEachAsync(node =>
{
    Console.WriteLine(node.Id + " :");
    Console.WriteLine($"RunningTasks = {node.RunningTasksCount}, RunningTaskSlots = {node.RunningTaskSlotsCount}");

}).ConfigureAwait(continueOnCapturedContext: false);
```

### <a name="list-task-counts-for-the-job"></a>Enumeración de recuentos de tarea para el trabajo
Este fragmento de código obtiene los recuentos de tarea para el trabajo, lo que incluye los recuentos de tareas y de ranuras de tarea por estado de tarea.
```csharp
TaskCountsResult result = await batchClient.JobOperations.GetJobTaskCountsAsync(jobId);

Console.WriteLine("\t\tActive\tRunning\tCompleted");
Console.WriteLine($"TaskCounts:\t{result.TaskCounts.Active}\t{result.TaskCounts.Running}\t{result.TaskCounts.Completed}");
Console.WriteLine($"TaskSlotCounts:\t{result.TaskSlotCounts.Active}\t{result.TaskSlotCounts.Running}\t{result.TaskSlotCounts.Completed}");
```

## <a name="batch-rest-example"></a>Ejemplo de REST Batch
En este fragmento de la API de [REST de Batch][api_rest], se muestra una solicitud para crear un grupo que contiene dos nodos de gran tamaño con un máximo de cuatro tareas por nodo. Para más información sobre cómo agregar grupos mediante la API REST, consulte [Agregar un grupo a una cuenta][rest_addpool].

```json
{
  "odata.metadata":"https://myaccount.myregion.batch.azure.com/$metadata#pools/@Element",
  "id":"mypool",
  "vmSize":"large",
  "cloudServiceConfiguration": {
    "osFamily":"4",
    "targetOSVersion":"*",
  },
  "targetDedicatedComputeNodes":2,
  "taskSlotsPerNode":4,
  "enableInterNodeCommunication":true,
}
```

En este fragmento de código se muestra una solicitud para agregar una tarea con `requiredSlots` no predeterminado. Esta tarea solo se ejecutará cuando haya suficientes ranuras libres disponibles en el nodo de ejecución.
```json
{
  "id": "taskId",
  "commandLine": "bash -c 'echo hello'",
  "userIdentity": {
    "autoUser": {
      "scope": "task",
      "elevationLevel": "nonadmin"
    }
  },
  "requiredSLots": 2
}
```

## <a name="code-sample"></a>Código de ejemplo
El proyecto [ParallelNodeTasks][parallel_tasks_sample] en GitHub muestra el uso de la propiedad [CloudPool.TaskSlotsPerNode][maxtasks_net].

Esta aplicación de consola de C# utiliza la biblioteca de [.NET de Batch][api_net] para crear un grupo con uno o más nodos de proceso. Ejecuta un número configurable de tareas en esos nodos para simular una carga variable. Los resultados de la aplicación especifican qué nodos han ejecutado cada tarea. La aplicación también proporciona un resumen de los parámetros de trabajo y la duración. Abajo se muestra la parte de resumen de los resultados de dos ejecuciones diferentes de la aplicación de ejemplo.

```
Nodes: 1
Node size: large
Task slots per node: 1
Max slots per task: 1
Tasks: 32
Duration: 00:30:01.4638023
```

La primera ejecución de la aplicación de ejemplo muestra que, con un solo nodo en el grupo y la configuración predeterminada de una tarea por nodo, la duración del trabajo es superior a 30 minutos.

```
Nodes: 1
Node size: large
Task slots per node: 4
Max slots per task: 1
Tasks: 32
Duration: 00:08:48.2423500
```

La segunda ejecución del ejemplo muestra una disminución notable en la duración del trabajo. Esto se debe a que el grupo se configuró con cuatro tareas por nodo, lo que permite la ejecución en paralelo de tareas de forma que el trabajo se completa en casi una cuarta parte del tiempo.

> [!NOTE]
> La duración del trabajo en los resúmenes anteriores no incluye el tiempo de creación del grupo. Cada uno de los trabajos anteriores se envió a grupos ya creados cuyos nodos de proceso se encontraban en el estado *inactivo* en el momento del envío.
>
>

## <a name="next-steps"></a>Pasos siguientes
### <a name="batch-explorer-heat-map"></a>Mapa térmico de Batch Explorer
[Batch Explorer][batch_labs] es una herramienta de cliente independiente, completa y gratuita que puede ayudarle a crear, depurar y supervisar aplicaciones de Azure Batch. Batch Explorer contiene una característica de *mapa térmico* que permite visualizar la ejecución de las tareas. Cuando ejecuta la aplicación de ejemplo [ParallelTasks][parallel_tasks_sample], puede usar la característica Mapa térmico para ver fácilmente la ejecución de tareas paralelas en cada nodo.


[api_net]: /dotnet/api/microsoft.azure.batch
[api_rest]: /rest/api/batchservice/
[batch_labs]: https://azure.github.io/BatchExplorer/
[cloudpool]: /dotnet/api/microsoft.azure.batch.cloudpool
[enable_autoscaling]: /rest/api/batchservice/pool/enableautoscale
[fill_type]: /dotnet/api/microsoft.azure.batch.common.computenodefilltype
[github_samples]: https://github.com/Azure/azure-batch-samples
[maxtasks_net]: /dotnet/api/microsoft.azure.batch.cloudpool
[rest_addpool]: /rest/api/batchservice/pool/add
[parallel_tasks_sample]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/ParallelTasks
[poolcreate_net]: /dotnet/api/microsoft.azure.batch.pooloperations
[task_schedule]: /dotnet/api/microsoft.azure.batch.cloudpool
[taskslots_net]: /dotnet/api/microsoft.azure.batch.cloudtask.requiredslots

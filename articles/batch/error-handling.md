---
title: Control y detección de errores en Azure Batch
description: Obtenga información sobre el control de errores en flujos de trabajo del servicio Batch desde un punto de vista del desarrollo.
ms.topic: article
ms.date: 05/15/2020
ms.openlocfilehash: 3bd460598dae08fa18415e1c9865249f3ca4c9c2
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "85964284"
---
# <a name="error-handling-and-detection-in-azure-batch"></a>Control y detección de errores en Azure Batch

A veces, esta operación puede ser necesaria para controlar los errores de las tareas y las aplicaciones en la solución de Batch. En este artículo se tratan los tipos de errores y cómo resolverlos.

## <a name="error-codes"></a>Códigos de error

Los tipos de errores generales incluyen:

- Errores de redes para solicitudes que nunca llegaron a Batch o cuando la respuesta de Batch no llegó al cliente a tiempo.
- Errores internos del servidor (respuesta estándar con código de estado HTTP 5xx).
- Errores relacionados con la limitación, como respuestas con código de estado HTTP 429 o 503 con el encabezado Retry-after.
- Errores 4xx, como AlreadyExists e InvalidOperation. Esto significa que el recurso no está en el estado correcto para la transición de estado.

Para obtener información detallada sobre los códigos de error específicos, incluidos los códigos de error de API REST, el servicio Batch y la programación y tareas de trabajo, consulte [Códigos de error y estado de Batch](/rest/api/batchservice/batch-status-and-error-codes).

## <a name="application-failures"></a>Errores de aplicación

Durante la ejecución, una aplicación produce resultados de diagnóstico que se pueden usar para la solución de problemas. Tal y como se describe en [Archivos y directorios](files-and-directories.md), el servicio Batch escribe resultados de salida estándar y error estándar en los archivos `stdout.txt` y `stderr.txt` ubicados en el directorio de la tarea en el nodo de proceso.

Para descargar estos archivos, puede usar Azure Portal, o bien uno de los SDK de Batch. Por ejemplo, puede recuperar estos y otros archivos para solucionar problemas con [ComputeNode.GetNodeFile](/dotnet/api/microsoft.azure.batch.computenode) y [CloudTask.GetNodeFile](/dotnet/api/microsoft.azure.batch.cloudtask) en la biblioteca de Batch para .NET.

## <a name="task-errors"></a>Errores de tarea

Los errores de tarea se dividen en varias categorías.

### <a name="pre-processing-errors"></a>Errores de procesamiento previo

Si una tarea no se inicia, se establece un error de procesamiento previo para la misma.  

Se pueden producir errores de procesamiento previo si se han movido los archivos de recursos de la tarea, la cuenta de almacenamiento deja de estar disponible o ha surgido algún otro problema que ha impedido la copia correcta de los archivos en el nodo.

### <a name="file-upload-errors"></a>Errores de carga de archivos

Si por cualquier motivo los archivos especificados para una tarea no se pueden cargar, se establece un error de carga de archivos para la tarea.

Los errores de carga de archivos se pueden producir si el SAS suministrado para acceder a Azure Storage no es válido o no proporciona permisos de escritura, si la cuenta de Storage no está disponible o si ha surgido cualquier otro problema que ha impedido la copia correcta de los archivos desde el nodo.

### <a name="application-errors"></a>Errores de aplicación

El proceso especificado por la línea de comandos de la tarea también puede presentar errores. Se considera que el proceso ha generado un error cuando el proceso ejecutado por la tarea devuelve un código de salida distinto de cero (consulte *Códigos de salida de la tarea* en la siguiente sección).

Para los errores de aplicaciones, puede configurar el servicio Batch para volver a intentar automáticamente la tarea hasta un número especificado de veces.

### <a name="constraint-errors"></a>Errores de restricción

Puede establecer una restricción que especifique la duración máxima de ejecución de un trabajo o una tarea con *maxWallClockTime*. Esto puede ser útil para terminar las tareas que no avanzan.

Cuando se supere la cantidad máxima de tiempo, la tarea se marcará como *completada* pero el código de salida se establecerá en `0xC000013A` y el campo *schedulingError* se marcará como `{ category:"ServerError", code="TaskEnded"}`.

## <a name="task-exit-codes"></a>Códigos de salida de la tarea

Como ya se mencionó anteriormente, el servicio Batch marcará una tarea como con errores si el proceso ejecutado por la tarea devuelve un código de salida distinto de cero. Cuando una tarea ejecuta un proceso, Batch rellena la propiedad de código de salida de la tarea con el código de retorno del proceso.

Es importante tener en cuenta que el código de salida de una tarea no lo determina el servicio Batch. El código de salida de una tarea lo determina el proceso en sí o el sistema operativo en que se ejecuta el proceso.

## <a name="task-failures-or-interruptions"></a>Errores o interrupciones de tareas

En ocasiones, las tareas pueden presentar errores o interrumpirse. Se podría producir un error en la propia aplicación de la tarea, se podría reiniciar el nodo donde se ejecuta la tarea o se podría quitar el nodo del grupo durante una operación de cambio de tamaño (si la directiva de desasignación del grupo está configurada para quitar los nodos de inmediato, sin esperar a que finalicen las tareas). En todos los casos, el servicio Batch puede volver a poner la tarea automáticamente en la cola para ejecutarla en otro nodo.

También es posible que un problema intermitente haga que una tarea deje de responder o tarde demasiado tiempo en ejecutarse. Puede establecer el intervalo de ejecución máximo para una tarea. Si se supera, el servicio Batch interrumpe la aplicación de la tarea.

## <a name="connect-to-compute-nodes"></a>Conexión a los nodos de proceso

Puede realizar tareas adicionales de depuración y solución de problemas si se inicia una sesión remota en un nodo de proceso. Puede usar Azure Portal para descargar un protocolo de escritorio remoto (RDP) para los nodos de Windows y obtener información de conexión de Secure Shell (SSH) para los nodos de Linux. También puede hacer esto con las API de Batch, como [.NET de Batch](/dotnet/api/microsoft.azure.batch.computenode) o [Python de Batch](batch-linux-nodes.md#connect-to-linux-nodes-using-ssh).

> [!IMPORTANT]
> Para conectarse a un nodo a través de RDP o SSH, primero debe crear un usuario en el nodo. Para ello, puede usar Azure Portal, [agregar una cuenta de usuario a un nodo](/rest/api/batchservice/computenode/adduser) con la API REST de Batch y llamar al método [ComputeNode.CreateComputeNodeUser](/dotnet/api/microsoft.azure.batch.computenode) en Batch para .NET o al método [add_user](batch-linux-nodes.md#connect-to-linux-nodes-using-ssh) en el módulo Python de Batch.

Si debe restringir o deshabilitar el acceso de los protocolos RDP o SSH a los nodos de proceso, consulte [Configure or disable remote access to compute nodes in an Azure Batch pool](pool-endpoint-configuration.md) (Configuración o deshabilitación del acceso remoto a los nodos de proceso de un grupo de Azure Batch).

## <a name="troubleshoot-problem-nodes"></a>Solución de problemas de nodos problemáticos

Si algunas de las tareas producen errores, el servicio o la aplicación de cliente de Batch pueden examinar los metadatos de las tareas con errores para identificar un nodo con un comportamiento incorrecto. Cada nodo de un grupo recibe un identificador único y el nodo en el que se ejecuta una tarea se incluye en los metadatos de la tarea. Una vez que haya identificado un "nodo problemático", puede llevar a cabo varias acciones con él:

- **Reiniciar el nodo** ([REST](/rest/api/batchservice/computenode/reboot) | [.NET](/dotnet/api/microsoft.azure.batch.computenode.reboot))

    Algunas veces, reiniciar el nodo puede solucionar problemas latentes tales como los procesos bloqueados. Si su grupo usa una tarea de inicio o el trabajo usa una tarea de preparación de trabajo, se ejecutarán cuando el nodo se reinicie.
- **Restablecer la imagen inicial del nodo** ([REST](/rest/api/batchservice/computenode/reimage) | [.NET](/dotnet/api/microsoft.azure.batch.computenode.reimage))

    Esto reinstala el sistema operativo en el nodo. Al igual que al reiniciar un nodo, las tareas de inicio y las tareas de preparación del trabajo se vuelven a ejecutar después de restablecer la imagen inicial del nodo.
- **Quitar el nodo del grupo** ([REST](/rest/api/batchservice/pool/removenodes) | [.NET](/dotnet/api/microsoft.azure.batch.pooloperations))

    A veces es necesario quitar completamente el nodo del grupo.
- **Deshabilitar la programación de tareas en el nodo** ([REST](/rest/api/batchservice/computenode/disablescheduling) | [.NET](/dotnet/api/microsoft.azure.batch.computenode.disablescheduling))

    Esto desconecta el nodo para que no se le asigne ninguna tarea adicional, pero permite que el nodo permanezca en ejecución y en el grupo. Esto permite seguir investigando la causa de los errores sin perder los datos de la tarea con errores, y sin que el nodo produzca más errores en la tarea. Por ejemplo, puede deshabilitar la programación de tareas en el nodo y después iniciar una sesión remota para examinar los registros de eventos del nodo o solucionar otros problemas. Una vez que haya terminado la investigación, puede volver a conectar el nodo, para lo que debe habilitar la programación de tareas ([REST](/rest/api/batchservice/computenode/enablescheduling) | [.NET](/dotnet/api/microsoft.azure.batch.computenode.enablescheduling)), o bien puede realizar una de las otras acciones mencionadas antes.

> [!IMPORTANT]
> Con las acciones descritas anteriormente, puede especificar cómo se controlan las tareas que se ejecutan actualmente en el nodo al realizar la acción. Por ejemplo, cuando deshabilita la programación de tareas en un nodo con la biblioteca de cliente .NET de Batch, puede indicar un valor de enumeración [DisableComputeNodeSchedulingOption](/dotnet/api/microsoft.azure.batch.common.disablecomputenodeschedulingoption) para especificar si se finalizarán las tareas en ejecución (**Terminate**), si se volverán a poner en la cola para que se programen en otros nodos (**Requeue**) o si se permitirá que se completen antes de realizar la acción (**TaskCompletion**).

## <a name="retry-after-errors"></a>Reintento después de errores

Las API de Batch le notificarán si se produce un error. Todos pueden volver a intentarse y todos incluyen un controlador de reintentos global para ese propósito. Es mejor utilizar este mecanismo integrado.

Después de un error, debe esperar un poco (varios segundos entre reintentos) antes de volver a intentarlo. Si realiza reintentos con demasiada frecuencia o demasiado rápido, el controlador de reintentos lo limitará.

## <a name="next-steps"></a>Pasos siguientes

- Aprenda a [comprobar errores de grupo y de nodo](batch-pool-node-error-checking.md).
- Aprenda a [comprobar errores de trabajos y tareas](batch-job-task-error-checking.md).
- Revise la lista de [Códigos de error y estado de Batch](/rest/api/batchservice/batch-status-and-error-codes).

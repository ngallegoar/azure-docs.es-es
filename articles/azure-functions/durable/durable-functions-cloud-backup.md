---
title: 'Escenarios de distribución ramificada de entrada/salida en Durable Functions: Azure'
description: Aprenda a implementar un escenario de distribución ramificada de entrada y salida en la extensión Durable Functions para Azure Functions.
ms.topic: conceptual
ms.date: 11/02/2019
ms.author: azfuncdf
ms.openlocfilehash: d61600801286126ea6ffb9a97bc5655b6f233816
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "77562197"
---
# <a name="fan-outfan-in-scenario-in-durable-functions---cloud-backup-example"></a>Escenario de distribución ramificada de entrada/salida en Durable Functions: ejemplo de copia de seguridad en la nube

La *distribución ramificada de entrada y salida* hace referencia al patrón de ejecución simultánea de varias funciones y la agregación de resultados. En este artículo se explica un ejemplo que usa [Durable Functions](durable-functions-overview.md) para implementar un escenario de distribución ramificada de entrada y salida. El ejemplo es una instancia de Durable Functions que realiza una copia de seguridad parcial o total del contenido de una aplicación en Azure Storage.

[!INCLUDE [durable-functions-prerequisites](../../../includes/durable-functions-prerequisites.md)]

## <a name="scenario-overview"></a>Información general de escenario

En este ejemplo, las funciones cargan todos los archivos de un directorio especificado de forma repetitiva en Blob Storage. También cuenta el número de bytes que se han cargado.

Es posible escribir una sola función que se encargue de todo. El problema principal que encontraría es la **escalabilidad**. La ejecución de una función solo se puede realizar en una única máquina virtual, por lo que el rendimiento se verá limitado por el de esa VM concreta. Otro problema es la **confiabilidad**. Si se produce un error a mitad de camino, o si todo el proceso tarda más de 5 minutos, se podría producir un error de copia de seguridad en un estado completado parcialmente. por lo que tendría que reiniciarse.

Un enfoque más sólido sería escribir dos funciones normales: una que enumere los archivos y agregue los nombres de archivo a una cola, y otra que lea de la cola y cargue los archivos en Blob Storage. Este enfoque es mejor en cuanto a rendimiento y confiabilidad, pero habrá que aprovisionar y administrar una cola. Más importante aún, se introduce una complejidad considerable en términos de **administración de estado** y **coordinación** si desea hacer algo más, como notificar el total de bytes cargados.

Los enfoques de Durable Functions proporcionan todas las ventajas mencionadas con muy poca sobrecarga.

## <a name="the-functions"></a>Funciones

En este artículo se explican las funciones siguientes en la aplicación de ejemplo:

* `E2_BackupSiteContent`: [función de orquestador](durable-functions-bindings.md#orchestration-trigger) que llama a `E2_GetFileList` para obtener una lista de archivos de los que se va a realizar una copia de seguridad y, a continuación, llama a `E2_CopyFileToBlob` para hacer una copia de seguridad de cada archivo.
* `E2_GetFileList`: [función de actividad](durable-functions-bindings.md#activity-trigger) que devuelve una lista de archivos de un directorio.
* `E2_CopyFileToBlob`: función de actividad que realiza una copia de seguridad de un único archivo en Azure Blob Storage.

### <a name="e2_backupsitecontent-orchestrator-function"></a>Función de orquestador E2_BackupSiteContent

Esta función de orquestador básicamente hace lo siguiente:

1. Toma un valor `rootDirectory` como parámetro de entrada.
2. Llama a una función para obtener una lista recursiva de los archivos de `rootDirectory`.
3. Realiza varias llamadas de función paralelas para cargar los archivos en Azure Blob Storage.
4. Espera a que finalicen todas las cargas.
5. Devuelve los bytes totales que se han cargado en Azure Blob Storage.

# <a name="c"></a>[C#](#tab/csharp)

Este es el código que implementa la función de orquestador:

[!code-csharp[Main](~/samples-durable-functions/samples/precompiled/BackupSiteContent.cs?range=16-42)]

Observe la línea `await Task.WhenAll(tasks);`. *No* se esperaba ninguna de las llamadas individuales a la función `E2_CopyFileToBlob`, lo que les permite ejecutarse en paralelo. Cuando se pasa esta matriz de tareas a `Task.WhenAll`, obtenemos una tarea que no finalizará *hasta que se completen todas las operaciones de copia*. Si está familiarizado con la biblioteca TPL en. NET, esto no supondrá una novedad para usted. La diferencia es que estas tareas se pueden ejecutar en varias máquinas virtuales al mismo tiempo y la extensión Durable Functions garantiza que la ejecución de un extremo a otro es resistente al reciclaje de procesos.

Después de espera a `Task.WhenAll`, sabemos que todas las llamadas de función han finalizado y nos han devuelto valores. Cada llamada a `E2_CopyFileToBlob` devuelve el número de bytes cargados, por lo que calcular el recuento total de bytes es cuestión de agregar todos los valores devueltos.

# <a name="javascript"></a>[JavaScript](#tab/javascript)

La función utiliza la norma *function.json* para las funciones de orquestador.

[!code-json[Main](~/samples-durable-functions/samples/javascript/E2_BackupSiteContent/function.json)]

Este es el código que implementa la función de orquestador:

[!code-javascript[Main](~/samples-durable-functions/samples/javascript/E2_BackupSiteContent/index.js)]

Observe la línea `yield context.df.Task.all(tasks);`. *No* se generó ninguna de las llamadas individuales a la función `E2_CopyFileToBlob`, lo que permite que se ejecuten en paralelo. Cuando se pasa esta matriz de tareas a `context.df.Task.all`, obtenemos una tarea que no finalizará *hasta que se completen todas las operaciones de copia*. Si está familiarizado con [`Promise.all`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise/all) en JavaScript, no le parecerá nada nuevo. La diferencia es que estas tareas se pueden ejecutar en varias máquinas virtuales al mismo tiempo y la extensión Durable Functions garantiza que la ejecución de un extremo a otro es resistente al reciclaje de procesos.

> [!NOTE]
> Aunque las tareas son conceptualmente similares a las promesas de JavaScript, las funciones de orquestador deben usar `context.df.Task.all` y `context.df.Task.any`, en lugar de `Promise.all` y `Promise.race`, para administrar la paralelización de la tarea.

Después de la generación desde `context.df.Task.all`, sabemos que todas las llamadas de función han finalizado y nos han devuelto valores. Cada llamada a `E2_CopyFileToBlob` devuelve el número de bytes cargados, por lo que calcular el recuento total de bytes es cuestión de agregar todos los valores devueltos.

---

### <a name="helper-activity-functions"></a>Funciones auxiliares de actividad

Las funciones auxiliares de actividad, al igual que otros ejemplos, son básicamente funciones normales que usan el desencadenador de enlace `activityTrigger`.

#### <a name="e2_getfilelist-activity-function"></a>Función de actividad E2_GetFileList

# <a name="c"></a>[C#](#tab/csharp)

[!code-csharp[Main](~/samples-durable-functions/samples/precompiled/BackupSiteContent.cs?range=44-54)]

# <a name="javascript"></a>[JavaScript](#tab/javascript)

El archivo *function.json* para `E2_GetFileList` tiene el siguiente aspecto:

[!code-json[Main](~/samples-durable-functions/samples/javascript/E2_GetFileList/function.json)]

Y esta es la implementación:

[!code-javascript[Main](~/samples-durable-functions/samples/javascript/E2_GetFileList/index.js)]

La función usa el módulo `readdirp` (versión 2.x) para leer de forma recursiva la estructura de directorios.

---

> [!NOTE]
> Tal vez se pregunte por qué no se podía poner directamente el código en la función de orquestador. Se podría, pero esto infringiría una de las reglas fundamentales de funciones de orquestador, que es que nunca hacen E/S, acceso al sistema de archivos local incluido. Para más información, consulte el [Restricciones de código de las funciones de orquestador](durable-functions-code-constraints.md).

#### <a name="e2_copyfiletoblob-activity-function"></a>Función de actividad E2_CopyFileToBlob

# <a name="c"></a>[C#](#tab/csharp)

[!code-csharp[Main](~/samples-durable-functions/samples/precompiled/BackupSiteContent.cs?range=56-81)]

> [!NOTE]
> Deberá instalar el paquete NuGet `Microsoft.Azure.WebJobs.Extensions.Storage` para ejecutar el código de ejemplo.

La función usa algunas características de enlace de Azure Functions avanzadas (por ejemplo, el [parámetro `Binder`](../functions-dotnet-class-library.md#binding-at-runtime)), pero no tiene que preocuparse de esos detalles para este tutorial.

# <a name="javascript"></a>[JavaScript](#tab/javascript)

El archivo *function.json* para `E2_CopyFileToBlob` es igual de simple:

[!code-json[Main](~/samples-durable-functions/samples/javascript/E2_CopyFileToBlob/function.json)]

La implementación de JavaScript usa [Azure Storage SDK for Node](https://github.com/Azure/azure-storage-node) para cargar los archivos en Azure Blob Storage.

[!code-javascript[Main](~/samples-durable-functions/samples/javascript/E2_CopyFileToBlob/index.js)]

---

Con la implementación se carga el archivo desde el disco y transmite de forma asincrónica el contenido a un blob con el mismo nombre en el contenedor "backups". El valor devuelto es el número de bytes copiados en el almacenamiento, que utilizará la función de orquestador para calcular el agregado total.

> [!NOTE]
> Es un ejemplo perfecto de cómo se mueven las operaciones de E/S a una función `activityTrigger`. No solo puede distribuir el trabajo en muchas máquinas virtuales diferentes, sino que también obtiene las ventajas de establecer puntos de control del progreso. Si el proceso de host se finaliza por alguna razón, sabrá qué cargas ya se han completado.

## <a name="run-the-sample"></a>Ejecución del ejemplo

Puede iniciar la orquestación mediante el envío de la siguiente solicitud HTTP POST.

```
POST http://{host}/orchestrators/E2_BackupSiteContent
Content-Type: application/json
Content-Length: 20

"D:\\home\\LogFiles"
```

> [!NOTE]
> La función `HttpStart` que se está invocando solo funciona con contenido con formato JSON. Por este motivo, se necesita el encabezado `Content-Type: application/json` y se codifica la ruta de acceso de directorio como cadena JSON. Además, el fragmento de código HTTP da por supuesto que hay una entrada en el archivo `host.json` que elimina el prefijo `api/` predeterminado de todas las direcciones URL de las funciones de activación de HTTP. Puede encontrar el marcador para esta configuración en el archivo `host.json` en los ejemplos.

Esta solicitud HTTP desencadena el orquestador `E2_BackupSiteContent` y pasa la cadena `D:\home\LogFiles` como un parámetro. La respuesta proporciona un vínculo para obtener el estado de esta operación de copia de seguridad:

```
HTTP/1.1 202 Accepted
Content-Length: 719
Content-Type: application/json; charset=utf-8
Location: http://{host}/runtime/webhooks/durabletask/instances/b4e9bdcc435d460f8dc008115ff0a8a9?taskHub=DurableFunctionsHub&connection=Storage&code={systemKey}

(...trimmed...)
```

Dependiendo de cuántos archivos de registro tenga en la aplicación de función, esta operación puede tardar varios minutos en completarse. Puede obtener el estado más reciente al consultar la dirección URL en el encabezado `Location` de la respuesta HTTP 202 anterior.

```
GET http://{host}/runtime/webhooks/durabletask/instances/b4e9bdcc435d460f8dc008115ff0a8a9?taskHub=DurableFunctionsHub&connection=Storage&code={systemKey}
```

```
HTTP/1.1 202 Accepted
Content-Length: 148
Content-Type: application/json; charset=utf-8
Location: http://{host}/runtime/webhooks/durabletask/instances/b4e9bdcc435d460f8dc008115ff0a8a9?taskHub=DurableFunctionsHub&connection=Storage&code={systemKey}

{"runtimeStatus":"Running","input":"D:\\home\\LogFiles","output":null,"createdTime":"2019-06-29T18:50:55Z","lastUpdatedTime":"2019-06-29T18:51:16Z"}
```

En este caso, la función todavía se está ejecutando. Es posible que vea la entrada que se guardó en el estado de orquestador y la hora de la última actualización. Se pueden seguir usando los valores del encabezado `Location` para sondear la finalización. Cuando el estado sea "Completado", verá un valor de respuesta HTTP similar al siguiente:

```
HTTP/1.1 200 OK
Content-Length: 152
Content-Type: application/json; charset=utf-8

{"runtimeStatus":"Completed","input":"D:\\home\\LogFiles","output":452071,"createdTime":"2019-06-29T18:50:55Z","lastUpdatedTime":"2019-06-29T18:51:26Z"}
```

Ahora verá que la orquestación ha finalizado y el tiempo aproximado que tardó en completarse. También verá un valor para el campo `output`, lo que indica que se han cargado alrededor de 450 KB de registros.

## <a name="next-steps"></a>Pasos siguientes

Este ejemplo ha mostrado cómo implementar el modelo de distribución ramificada de salida y entrada. En el ejemplo siguiente se muestra cómo implementar el patrón de supervisión con [temporizadores durables](durable-functions-timers.md).

> [!div class="nextstepaction"]
> [Ejecutar el ejemplo de supervisión](durable-functions-monitor.md)
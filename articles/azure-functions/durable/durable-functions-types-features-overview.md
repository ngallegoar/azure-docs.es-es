---
title: Tipos de función en Azure Durable Functions
description: Aprenda acerca de los tipos de funciones y roles que admiten la comunicación de función a función en una orquestación de Durable Functions en Azure Functions.
author: cgillum
ms.topic: conceptual
ms.date: 08/22/2019
ms.author: azfuncdf
ms.openlocfilehash: 35ef9d8731e169e890f5985ce01215fec5d6e3de
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "84697714"
---
# <a name="durable-functions-types-and-features"></a>Tipos y características de Durable Functions

Durable Functions es una extensión de [Azure Functions](../functions-overview.md). Puede usar Durable Functions para una orquestación con estado de la ejecución de funciones. Una aplicación de funciones duraderas es una solución compuesta de diferentes funciones de Azure Functions. Las funciones pueden desempeñar roles diferentes en una orquestación de Durable Functions. 

Actualmente existen cuatro tipos de Durable Functions en Azure Functions: actividad, orquestador, entidad y cliente. En el resto de esta sección se incluyen más detalles sobre los tipos de funciones que participan en una orquestación.

## <a name="orchestrator-functions"></a>Funciones de Orchestrator

Las funciones de Orchestrator describen cómo se ejecutan las acciones y el orden en que se ejecutan las acciones. Las funciones del orquestador describen la orquestación en código (C# o JavaScript) como se muestra en [Patrones de aplicación de Durable Functions](durable-functions-overview.md#application-patterns). Una orquestación puede tener muchos tipos diferentes de acciones, como [funciones de actividad](#activity-functions), [suborquestaciones](durable-functions-orchestrations.md#sub-orchestrations), [espera por eventos externos](durable-functions-orchestrations.md#external-events), [HTTP](durable-functions-http-features.md) y [temporizadores](durable-functions-orchestrations.md#durable-timers). Las funciones de orquestador también pueden interactuar con las [funciones de entidad](#entity-functions).

> [!NOTE]
> Las funciones de orquestador se escriben con código normal, pero hay requisitos estrictos sobre cómo escribir el código. En concreto, el código de la función de orquestador debe ser *determinista*. Si no se siguen estos requisitos de determinismo, es posible que las funciones de orquestador no se ejecuten correctamente. Puede consultar información detallada sobre estos requisitos y cómo trabajar para cumplirlos en el tema relacionado con las [restricciones de código](durable-functions-code-constraints.md).

Para obtener información más detallada sobre las funciones de orquestador y sus características, consulte el artículo sobre [orquestaciones duraderas](durable-functions-orchestrations.md).

## <a name="activity-functions"></a>Funciones de actividad

Las funciones de actividad son la unidad básica de trabajo en una orquestación de función durable. Las funciones de actividad son las funciones y tareas que se orquestan en el proceso. Por ejemplo, podría crear una función de orquestador para procesar un pedido. Las tareas implican comprobar el inventario, cobrar al cliente y crear un envío. Cada tarea sería una función de actividad independiente. Estas funciones de actividad se pueden ejecutar en serie, en paralelo, o en una combinación de ambos métodos.

A diferencia de las funciones de orquestador, las funciones de actividad no tienen ninguna restricción en relación al tipo de trabajo que puede realizar en ellas. Las funciones de actividad se utilizan con frecuencia para realizar llamadas de red o ejecutar operaciones de gran consumo de CPU. Una función de actividad también puede devolver datos a la función de orquestador. El marco de las tareas duraderas garantiza que cada función de actividad que se llame, se ejecutará *al menos una vez* durante una ejecución de la orquestación.

> [!NOTE]
> Dado que las funciones de actividad solo garantizan ejecutarse *al menos una vez*, se recomienda que la lógica de la función de actividad sea *idempotente* siempre que sea posible.

Use un [desencadenador de actividad](durable-functions-bindings.md#activity-trigger) para definir una función de actividad. Las funciones de .NET reciben `DurableActivityContext` como parámetro. También puede enlazar el desencadenador con cualquier otro objeto JSON serializable para pasar en entradas a la función. En JavaScript, puede acceder a la entrada mediante la propiedad `<activity trigger binding name>` del [objeto `context.bindings`](../functions-reference-node.md#bindings). Solo es posible pasar un único valor a las funciones de actividad. Para pasar varios valores, debe utilizar tuplas, matrices o tipos complejos.

> [!NOTE]
> Solo es posible desencadenar una función de actividad desde una función de orquestador.

## <a name="entity-functions"></a>Funciones de entidad

Las funciones de entidad definen las operaciones de lectura y actualización de pequeños fragmentos de estado. A menudo, hacemos referencia a estas entidades con estado como *entidades duraderas*. Al igual que las funciones de orquestador, las de entidad son funciones con un tipo especial de desencadenador, el *desencadenador de entidad*. También se pueden invocar desde funciones de cliente o desde funciones de orquestador. A diferencia de las funciones de orquestador, las funciones de entidad no tienen restricciones de código específicas. Las funciones de entidad también administran el estado de forma explícita, en lugar de representarlo de forma implícita a través del flujo de control.

> [!NOTE]
> Las funciones de entidad y la funcionalidad relacionada solo están disponibles en Durable Functions 2.0 y versiones superiores.

Para obtener más información sobre las funciones de entidad, consulte el artículo sobre [entidades duraderas](durable-functions-entities.md).

## <a name="client-functions"></a>Funciones de cliente

Las funciones de orquestador se desencadenan mediante un [enlace de desencadenador de orquestación](durable-functions-bindings.md#orchestration-trigger) y las funciones de entidad las desencadena un [enlace de desencadenador de entidad](durable-functions-bindings.md#entity-trigger). Ambos desencadenadores funcionan al reaccionar a los mensajes que se ponen en cola en un [centro de tareas](durable-functions-task-hubs.md). La principal forma de enviar estos mensajes es mediante un [enlace de cliente de orquestador](durable-functions-bindings.md#orchestration-client) o un [enlace de cliente de entidad](durable-functions-bindings.md#entity-client) desde una *función de cliente*. Todas las funciones que no son de orquestador puede ser *funciones de cliente*. Por ejemplo, puede desencadenar el orquestador desde una función desencadenada por HTTP, una función desencadenada por el centro de eventos de Azure, etc. Lo que convierte a una función en una *función de cliente* es el uso del enlace de salida de cliente duradero.

> [!NOTE]
> A diferencia de otros tipos de función, las funciones de orquestador y de entidad no se pueden desencadenar directamente con los botones de Azure Portal. Si quiere probar una función de orquestador o de entidad en Azure Portal, en su lugar debe ejecutar una *función de cliente* que inicie una función de orquestador o de entidad como parte de su implementación. Para obtener la experiencia de prueba más sencilla, se recomienda una función de *desencadenador manual*.

Además de desencadenar las funciones de orquestador o de entidad, el enlace de *cliente duradero* se puede usar para interactuar con las orquestaciones y entidades en ejecución. Por ejemplo, puede consultar, finalizar y generar eventos para las orquestaciones. Para obtener más información acerca de la administración de orquestaciones y entidades, consulte el artículo sobre [administración de instancias](durable-functions-instance-management.md).

## <a name="next-steps"></a>Pasos siguientes

Para empezar, cree la primera función durable en [C#](durable-functions-create-first-csharp.md) o [JavaScript](quickstart-js-vscode.md).

> [!div class="nextstepaction"]
> [Más información sobre las orquestaciones de Durable Functions](durable-functions-orchestrations.md)
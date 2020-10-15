---
title: Procesamiento de eventos fiable de Azure Functions
description: Evite que falten mensajes del Centro de eventos en Azure Functions
author: craigshoemaker
ms.topic: conceptual
ms.date: 09/12/2019
ms.author: cshoe
ms.openlocfilehash: 93a12d40e876293eb587ffba865a1d3b1f5f4983
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "86506033"
---
# <a name="azure-functions-reliable-event-processing"></a>Procesamiento de eventos fiable de Azure Functions

El procesamiento de eventos es uno de los escenarios más comunes asociados a la arquitectura sin servidor. En este artículo se describe cómo crear un procesador de mensajes fiable con Azure Functions para evitar que se pierdan mensajes.

## <a name="challenges-of-event-streams-in-distributed-systems"></a>Desafíos del flujo de eventos en sistemas distribuidos

Considere un sistema que envía eventos a una velocidad constante de 100 eventos por segundo. A esta velocidad, varias instancias de Functions paralelas pueden consumir en minutos 100 eventos entrantes por segundo.

Sin embargo, es posible que se produzca cualquiera de las siguientes condiciones menos óptimas:

- ¿Qué ocurre si el publicador de eventos envía un evento dañado?
- ¿Qué ocurre si la instancia de Functions encuentra excepciones no controladas?
- ¿Qué ocurre si un sistema de bajada se queda sin conexión?

¿Cómo se controlan estas situaciones a la vez que se conserva el rendimiento de la aplicación?

Con las colas, la mensajería de confianza se incluye de forma natural. Cuando se empareja con un desencadenador de funciones, la función crea un bloqueo en el mensaje de la cola. Si se produce un error de procesamiento, se libera el bloqueo para permitir que otra instancia vuelva a intentar el procesamiento. Seguidamente, el procesamiento continúa hasta que el mensaje se evalúa correctamente o se agrega a una cola de mensajes dudosos.

Aunque un único mensaje de la cola puede permanecer en un ciclo de reintento, otras ejecuciones en paralelo continúan manteniendo el proceso de eliminación de los mensajes restantes de la cola. El resultado es que un mensaje incorrecto no afecta en gran medida al rendimiento general. Sin embargo, las colas de almacenamiento no garantizan el orden y no están optimizadas para las demandas de alto rendimiento que requiere Event Hubs.

Por el contrario, Azure Event Hubs no incluye un concepto de bloqueo. Para permitir características tales como el alto rendimiento, varios grupos de consumidores y la capacidad de reproducción, los eventos de Event Hubs se comportan más como un reproductor de vídeo. Los eventos se leen desde un único punto en el flujo por partición. Desde el puntero puede leer hacia delante o hacia atrás desde esa ubicación, pero tendrá que decidir si quiere mover el puntero para que se procesen los eventos.

Cuando se producen errores en un flujo, si decide mantener el puntero en el mismo lugar, el procesamiento de eventos se bloquea hasta que el puntero se mueve hacia delante. En otras palabras, si se detiene el puntero para solucionar los problemas de procesamiento de un solo evento, los eventos sin procesar se empiezan a apilar.

Azure Functions evita esos interbloqueos al mover el puntero del flujo, independientemente de si el proceso se ha realizado correctamente o no. Dado que el puntero continúa avanzando, las funciones deben tratar los errores adecuadamente.

## <a name="how-azure-functions-consumes-event-hubs-events"></a>Cómo consume Azure Functions eventos de Event Hubs

Azure Functions consume eventos de Event Hubs mientras se realizan los pasos siguientes:

1. Se crea un puntero y se conserva en la instancia de Azure Storage de cada partición del centro de eventos.
2. Cuando se reciben nuevos mensajes (de forma predeterminada se reciben en un lote), el host intenta desencadenar la función con ese lote de mensajes.
3. Si la función se ejecuta correctamente (con o sin excepción), el puntero se mueve y se guarda un punto de control en la cuenta de almacenamiento.
4. Si las condiciones impiden que se complete la ejecución de la función, el host no podrá mover el puntero. Si el puntero no se mueve, las comprobaciones posteriores terminan procesando los mismos mensajes.
5. Repita los pasos 2 a 4.

Este comportamiento revela algunos puntos importantes:

- *Las excepciones no controladas pueden provocar la pérdida de mensajes.* Las ejecuciones que producen una excepción seguirán haciendo avanzar el puntero.
- *Las funciones garantizan la entrega al menos una vez.* Es posible que el código y sus sistemas dependientes necesiten [tener en cuenta el hecho de que el mismo mensaje podría recibirse dos veces](./functions-idempotent.md).

## <a name="handling-exceptions"></a>Control de excepciones

Como regla general, cada función debe incluir un [bloque de tipo try o catch](./functions-bindings-error-pages.md) en el nivel más alto del código. En concreto, todas las funciones que consumen eventos de Event Hubs deben tener un bloque `catch`. De ese modo, cuando se produce una excepción, el bloque catch controla el error antes de que el puntero avance.

### <a name="retry-mechanisms-and-policies"></a>Mecanismos de reintento y directivas

Algunas excepciones son transitorias por naturaleza y no vuelven a aparecer cuando se vuelve a intentar una operación más tarde. Este es el motivo por el que el primer paso que se aconseja dar es volver a intentar la operación. Puede escribir las reglas de procesamiento de reintentos usted mismo, pero son tan habituales que tiene herramientas disponibles para ello. El uso de estas bibliotecas le permite definir directivas de reintento sólidas, que también le pueden ayudar a conservar el orden de procesamiento.

Si introduce las bibliotecas de control de errores en las funciones, podrá definir directivas de reintento básicas y avanzadas. Por ejemplo, puede implementar una directiva que siga un flujo de trabajo basado en las siguientes reglas:

- Intente insertar un mensaje tres veces (esta opción tendrá potencialmente un retraso entre los reintentos).
- Si el resultado eventual de todos los reintentos es un error, agregue un mensaje a una cola para que el procesamiento pueda continuar en el flujo.
- Los mensajes dañados o no procesados se administrarán más adelante.

> [!NOTE]
> [Polly](https://github.com/App-vNext/Polly) es un ejemplo de una biblioteca de capacidad de recuperación y administración de errores transitorios para aplicaciones de C#.

Al trabajar con bibliotecas precompiladas de clase C#, los [filtros de excepción](/dotnet/csharp/language-reference/keywords/try-catch) le permiten ejecutar código cada vez que se produce una excepción no controlada.

Los ejemplos que muestran cómo usar filtros de excepción están disponibles en el repositorio del [SDK de Azure WebJobs](https://github.com/Azure/azure-webjobs-sdk/wiki).

## <a name="non-exception-errors"></a>Errores que no son de excepción

Algunos problemas surgen incluso cuando no existe un error. Por ejemplo, considere un error que se produce en medio de una ejecución. En este caso, si una función no finaliza la ejecución, el puntero de desplazamiento nunca progresa. Si el puntero no avanza, cualquier instancia que se ejecute después de un error de ejecución seguirá leyendo los mismos mensajes. Esta situación proporciona una garantía de "al menos una vez".

La garantía de que todos los mensajes se procesan al menos una vez implica que algunos mensajes se pueden procesar más de una vez. Las aplicaciones de funciones deben tener en cuenta esta posibilidad y deben crearse en función de los [principios de idempotencia](./functions-idempotent.md).

## <a name="stop-and-restart-execution"></a>Detener y reiniciar la ejecución

Aunque algunos errores pueden ser aceptables, ¿qué ocurre si la aplicación experimenta errores importantes? Puede que quiera detener la activación de eventos hasta que el estado del sistema sea correcto. Para tener la oportunidad de pausar el procesamiento debe usar un patrón de interruptor. El patrón de interruptor permite que la aplicación "interrumpa el circuito" del proceso de eventos y se reanude más adelante.

Hay dos partes necesarias para implementar un interruptor en un proceso de eventos:

- El estado compartido en todas las instancias para controlar y supervisar el estado del circuito.
- El proceso principal que puede administrar el estado del circuito (abierto o cerrado).

Los detalles de implementación pueden variar, pero para compartir el estado entre las instancias, necesita un mecanismo de almacenamiento. Puede optar por almacenar el estado en Azure Storage, en una caché en Redis o en cualquier otra cuenta a la que se pueda tener acceso mediante una colección de funciones.

[Azure Logic Apps](../logic-apps/logic-apps-overview.md) y [Durable Functions](./durable/durable-functions-overview.md) son una opción natural para administrar el flujo de trabajo y el estado del circuito. También puede usar otros servicios para ello, pero las aplicaciones lógicas se usarán en este ejemplo. Gracias a las aplicaciones lógicas, puede pausar y reiniciar la ejecución de una función, lo que le permitirá implementar el patrón del interruptor.

### <a name="define-a-failure-threshold-across-instances"></a>Definir un umbral de error entre instancias

Para tener en cuenta los eventos de procesamiento de varias instancias simultáneamente, es necesario conseguir un estado externo compartido persistente para poder supervisar el estado del circuito.

Si decide implementar una regla, se puede aplicar lo siguiente en la misma:

- Si hay más de 100 errores eventuales en 30 segundos y en todas las instancias, interrumpa el circuito y detenga el desencadenamiento en los mensajes nuevos.

Los detalles de la implementación variarán según sus necesidades, pero en general puede crear un sistema que haga lo siguiente:

1. Registrar errores en una cuenta de almacenamiento (Azure Storage, Redis, etc.).
1. Cuando se registre un nuevo error, puede inspeccionar el recuento acumulado para ver si se cumple el umbral (por ejemplo, más de 100 en los últimos 30 segundos).
1. Si se cumple el umbral, puede emitir un evento para Azure Event Grid que indique al sistema que interrumpa el circuito.

### <a name="managing-circuit-state-with-azure-logic-apps"></a>Administración del estado del circuito con Azure Logic Apps

En la descripción siguiente se destaca una manera de crear una aplicación lógica de Azure para detener el procesamiento de una aplicación de Functions.

Azure Logic Apps no solo incluye conectores integrados para diferentes servicios y características de orquestación con estado, sino que también es una opción natural para administrar el estado del circuito. Después de detectar que es necesario interrumpir el circuito, puede compilar una aplicación lógica para implementar el siguiente flujo de trabajo:

1. Desencadenar un flujo de trabajo de Event Grid y detener la función de Azure (con el conector de recursos de Azure).
1. Enviar un correo electrónico de notificación que incluya una opción para reiniciar el flujo de trabajo.

El destinatario del correo electrónico puede investigar el estado del circuito y, cuando sea necesario, reiniciarlo a través del vínculo del correo electrónico de notificación. Cuando el flujo de trabajo reinicie la función, los mensajes se procesan desde el último punto de comprobación del Centro de eventos.

Gracias a esta opción, no se pierden los mensajes, todos ellos se procesan en orden y puede interrumpir el circuito siempre que sea necesario.

## <a name="resources"></a>Recursos

- [Ejemplos de procesamiento de eventos fiables](https://github.com/jeffhollan/functions-csharp-eventhub-ordered-processing)
- [Interruptor del circuito de entidades duraderas de Azure](https://github.com/jeffhollan/functions-durable-actor-circuitbreaker)

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información, consulte los siguientes recursos:

- [Control de errores de Azure Functions](./functions-bindings-error-pages.md)
- [Automatización del cambio de tamaño de las imágenes cargadas mediante Event Grid](../event-grid/resize-images-on-storage-blob-upload-event.md?toc=%2Fazure%2Fazure-functions%2Ftoc.json&tabs=dotnet)
- [Creación de una función que se integre con Azure Logic Apps](./functions-twitter-email.md)

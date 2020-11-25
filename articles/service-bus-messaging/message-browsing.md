---
title: Exploración de mensajes de Azure Service Bus
description: Explorar e inspeccionar los mensajes de Service Bus permite a un cliente de Azure Service Bus enumerar todos los mensajes de una cola o suscripción.
ms.topic: article
ms.date: 11/11/2020
ms.openlocfilehash: c52c9c967d4eada1a931e188ed4d25f7691cfb91
ms.sourcegitcommit: dc342bef86e822358efe2d363958f6075bcfc22a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/12/2020
ms.locfileid: "94553648"
---
# <a name="message-browsing"></a>Exploración de mensajes

La exploración de mensajes, o la inspección, permite a un cliente de Service Bus enumerar todos los mensajes de una cola o una suscripción con fines de diagnóstico o depuración.

La operación de inspección en una cola devuelve todos los mensajes de la cola, no solo los disponibles para la adquisición inmediata con `Receive()` o el bucle de `OnMessage()`. La propiedad `State` de cada mensaje indica si el mensaje está activo (disponible para su recepción), se ha [aplazado](message-deferral.md) o está [programado](message-sequencing.md). La operación de inspección en una suscripción devuelve todos los mensajes excepto los mensajes programados en el registro de mensajes de la suscripción. 

Los mensajes consumidos y expirados se limpian mediante una ejecución de "recolección de elementos no utilizados" asincrónica. Este paso no se produce necesariamente inmediatamente después de que expiren los mensajes. Por eso, `Peek` puede devolver mensajes que ya han expirado. Estos mensajes se quitarán o se pondrán en la cola de mensajes fallidos cuando se invoque una operación de recepción en la cola o la suscripción la próxima vez. Tenga en cuenta este comportamiento al intentar recuperar los mensajes aplazados de la cola. Un mensaje expirado ya no es apto para la recuperación normal mediante ningún otro medio, incluso cuando se devuelve con Peek. Estos mensajes se devuelven por diseño dado que Peek es una herramienta de diagnóstico que refleja el estado actual del registro.

Peek también devuelve los mensajes que estaban bloqueados y que actualmente están procesando otros receptores. Sin embargo, dado que Peek devuelve una instantánea desconectada, el estado de bloqueo de un mensaje no se puede observar en los mensajes inspeccionados. Las propiedades [LockedUntilUtc](/dotnet/api/microsoft.azure.servicebus.message.systempropertiescollection.lockeduntilutc) y [LockToken](/dotnet/api/microsoft.azure.servicebus.message.systempropertiescollection.locktoken#Microsoft_Azure_ServiceBus_Message_SystemPropertiesCollection_LockToken) producen una excepción [InvalidOperationException](/dotnet/api/system.invalidoperationexception) cuando la aplicación intenta leerlas.

## <a name="peek-apis"></a>API de Peek

Los métodos [Peek/PeekAsync](/dotnet/api/microsoft.azure.servicebus.core.messagereceiver.peekasync#Microsoft_Azure_ServiceBus_Core_MessageReceiver_PeekAsync) y [PeekBatch/PeekBatchAsync](/dotnet/api/microsoft.servicebus.messaging.queueclient.peekbatchasync#Microsoft_ServiceBus_Messaging_QueueClient_PeekBatchAsync_System_Int64_System_Int32_) existen en las bibliotecas de cliente de .NET y Java y en los objetos receptores: **MessageReceiver**, **MessageSession**. Peek funciona en las colas y suscripciones y en sus respectivas colas de mensajes fallidos.

Cuando se llama de manera repetida, **Peek** enumera todos los mensajes que existen en el registro de la cola o suscripción, en orden, desde el número de secuencia más bajo disponible hasta el más alto. Este es el orden en que los mensajes se ponen en cola; no es el orden en que finalmente se podrían recuperar.

[PeekBatch](/dotnet/api/microsoft.servicebus.messaging.queueclient.peekbatch#Microsoft_ServiceBus_Messaging_QueueClient_PeekBatch_System_Int32_) recupera varios mensajes y los devuelve como una enumeración. Si no hay mensajes disponibles, el objeto de enumeración está vacío, no es null.

También puede usar una sobrecarga del método con un valor de [SequenceNumber](/dotnet/api/microsoft.azure.servicebus.message.systempropertiescollection.sequencenumber#Microsoft_Azure_ServiceBus_Message_SystemPropertiesCollection_SequenceNumber) con el que comenzar y, luego, llamar a la sobrecarga del método sin parámetros para enumerar más. **PeekBatch** funciona de forma equivalente, pero recupera un conjunto de mensajes a la vez.

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre la mensajería de Service Bus, consulte los siguientes temas:

* [Colas, temas y suscripciones de Service Bus](service-bus-queues-topics-subscriptions.md)
* [Introducción a las colas de Service Bus](service-bus-dotnet-get-started-with-queues.md)
* [Uso de temas y suscripciones de Service Bus](service-bus-dotnet-how-to-use-topics-subscriptions.md)

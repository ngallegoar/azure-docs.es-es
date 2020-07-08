---
title: Número de mensajes de Azure Service Bus
description: Recupere el número de mensajes incluidos en las colas y las suscripciones mediante las API NamespaceManager de Azure Resource Manager y Azure Service Bus.
ms.topic: article
ms.date: 06/23/2020
ms.openlocfilehash: d0e1a7a5c6eb0b281b4e6ac08135f41f28ecbec8
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85341274"
---
# <a name="message-counters"></a>Contadores de mensajes

Puede recuperar el recuento de mensajes contenidos en las colas y las suscripciones mediante las API [NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager) de Azure Resource Manager y Service Bus del SDK de .NET Framework.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Con PowerShell, puede obtener el recuento de la manera siguiente:

```powershell
(Get-AzServiceBusQueue -ResourceGroup mygrp -NamespaceName myns -QueueName myqueue).CountDetails
```

## <a name="message-count-details"></a>Detalles del recuento de mensajes

Conocer el número de mensajes activos resulta de utilidad para determinar si una cola crea un trabajo pendiente que requiere procesar más recursos que los que se han implementado actualmente. Los detalles de contador siguientes están disponibles en la clase [MessageCountDetails](/dotnet/api/microsoft.servicebus.messaging.messagecountdetails):

-   [ActiveMessageCount](/dotnet/api/microsoft.servicebus.messaging.messagecountdetails.activemessagecount#Microsoft_ServiceBus_Messaging_MessageCountDetails_ActiveMessageCount): mensajes de la cola o la suscripción que se encuentran en estado activo y listo para la entrega.
-   [DeadLetterMessageCount](/dotnet/api/microsoft.servicebus.messaging.messagecountdetails.deadlettermessagecount#Microsoft_ServiceBus_Messaging_MessageCountDetails_DeadLetterMessageCount): mensajes de la cola de mensajes fallidos.
-   [ScheduledMessageCount](/dotnet/api/microsoft.servicebus.messaging.messagecountdetails.scheduledmessagecount#Microsoft_ServiceBus_Messaging_MessageCountDetails_ScheduledMessageCount): mensajes en el estado programado.
-   [TransferDeadLetterMessageCount](/dotnet/api/microsoft.servicebus.messaging.messagecountdetails.transferdeadlettermessagecount#Microsoft_ServiceBus_Messaging_MessageCountDetails_TransferDeadLetterMessageCount): mensajes que no se pudieron transferir a otra cola u otro tema y se han movido a la cola de mensajes fallidos de transferencia.
-   [TransferMessageCount](/dotnet/api/microsoft.servicebus.messaging.messagecountdetails.transfermessagecount#Microsoft_ServiceBus_Messaging_MessageCountDetails_TransferMessageCount): mensajes pendientes de transferencia en otra cola u otro tema.

Si una aplicación desea escalar recursos en función de la longitud de la cola, debe hacerlo con un ritmo medido. La adquisición de los contadores de mensajes es una operación costosa en el agente de mensajes y ejecutarla con frecuencia afecta de forma directa y adversa al rendimiento de la entidad.

> [!NOTE]
> Los mensajes que se envían a un tema de Service Bus se reenvían a las suscripciones de ese tema. Por lo tanto, el número de mensajes activos en el propio tema es 0, ya que esos mensajes se han reenviado correctamente a la suscripción. Obtenga el número de mensajes en la suscripción y compruebe que es mayor que 0. Aunque vea mensajes en la suscripción, realmente se almacenan en un almacenamiento que pertenece al tema. 

Si observa las suscripciones, tendrán un número de mensajes distinto de cero (hasta un total de 323 MB de espacio para esta entidad completa).

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre la mensajería de Service Bus, consulte los siguientes temas:

* [Colas, temas y suscripciones de Service Bus](service-bus-queues-topics-subscriptions.md)
* [Introducción a las colas de Service Bus](service-bus-dotnet-get-started-with-queues.md)
* [Uso de temas y suscripciones de Service Bus](service-bus-dotnet-how-to-use-topics-subscriptions.md)

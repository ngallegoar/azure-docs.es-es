---
title: Entidades de mensajería de suspensión de Azure Service Bus
description: En este artículo se explica cómo suspender y reactivar temporalmente entidades de mensaje de Azure Service Bus (colas, temas y suscripciones).
ms.topic: article
ms.date: 09/29/2020
ms.openlocfilehash: f89e17e494cc777691b7f7ca47538cd29114d2dc
ms.sourcegitcommit: a422b86148cba668c7332e15480c5995ad72fa76
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/30/2020
ms.locfileid: "91575265"
---
# <a name="suspend-and-reactivate-messaging-entities-disable"></a>Suspensión y reactivación de entidades de mensajería (deshabilitar)

Las colas, los temas y las suscripciones se pueden suspender temporalmente. La suspensión coloca la entidad en un estado deshabilitado, en el que todos los mensajes se mantienen en el almacenamiento. Sin embargo, no se quitan ni se agregan mensajes, y las operaciones del protocolo respectivo producen error.

Normalmente la suspensión de una entidad se realiza por motivos administrativos urgentes. Una de estas situaciones puede darse cuando se tiene un receptor defectuoso que saca mensajes de la cola, no realiza el procesamiento y encima completa incorrectamente los mensajes y los quita. Si se diagnostica este comportamiento, la cola se puede deshabilitar para las recepciones hasta que se implemente el código correcto y pueda impedirse que se sigan perdiendo datos debido a código incorrecto.

El usuario o el sistema pueden realizar la suspensión o la reactivación. El sistema solo suspende las entidades por motivos administrativos graves como haber alcanzado el límite de gasto de suscripción. El usuario no puede reactivar las entidades deshabilitadas por el sistema, pero se restauran cuando se ha solucionado la causa de la suspensión.

## <a name="queue-status"></a>Estado de la cola 
Los estados que se pueden establecer para una cola son:

-   **Activa**: la cola está activa.
-   **Disabled**: la cola se ha suspendido. Es equivalente a establecer **SendDisabled** y **ReceiveDisabled**. 
-   **SendDisabled**: la cola se suspende parcialmente y se permite la recepción.
-   **ReceiveDisabled**: la cola se suspende parcialmente y se permite el envío.

### <a name="change-the-queue-status-in-the-azure-portal"></a>Cambie el estado de la cola en Azure Portal: 

1. En Azure Portal, vaya al espacio de nombres de Service Bus. 
1. Seleccione la cola para la que desea cambiar el estado. Verá las colas en el panel inferior, en la parte central. 
1. En la página **Cola de Service Bus**, vea el estado actual de la cola como un hipervínculo. Si no está seleccionada la opción de **información general** en el menú de la izquierda, selecciónela para ver el estado de la cola. Seleccione el estado actual de la cola para cambiarlo. 

    :::image type="content" source="./media/entity-suspend/select-state.png" alt-text="Selección del estado de la cola":::
4. Seleccione el nuevo estado de la cola y luego **Aceptar**. 

    :::image type="content" source="./media/entity-suspend/entity-state-change.png" alt-text="Selección del estado de la cola":::
    
El portal solo permite deshabilitar completamente las colas. También puede deshabilitar las operaciones de envío y recepción por separado mediante las API [NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager) de Service Bus en el SDK de .NET Framework, o con una plantilla de Azure Resource Manager mediante la CLI de Azure o Azure PowerShell.

### <a name="change-the-queue-status-using-azure-powershell"></a>Cambio del estado de la cola mediante Azure PowerShell
El comando de PowerShell para deshabilitar una cola se muestra en el ejemplo siguiente. El comando de reactivación es equivalente a establecer `Status` en **Activo**.

```powershell
$q = Get-AzServiceBusQueue -ResourceGroup mygrp -NamespaceName myns -QueueName myqueue

$q.Status = "Disabled"

Set-AzServiceBusQueue -ResourceGroup mygrp -NamespaceName myns -QueueName myqueue -QueueObj $q
```

## <a name="topic-status"></a>Estado del tema
Cambiar el estado del tema en el Azure Portal es similar a cambiar el estado de una cola. Cuando seleccione el estado actual del tema, verá la siguiente página que le permite cambiar el estado. 

:::image type="content" source="./media/entity-suspend/topic-state-change.png" alt-text="Selección del estado de la cola":::

Los estados que se pueden establecer para una tema son:
- **Activa**: El tema está activo.
- **Disabled**: El tema se ha suspendido.
- **SendDisabled**: El mismo efecto que **Disabled**.

## <a name="subscription-status"></a>Estado de la suscripción
Cambiar el estado de la suscripción en Azure Portal es similar a cambiar el estado de un tema o de una cola. Cuando seleccione el estado actual de la suscripción, verá la siguiente página que le permite cambiar el estado. 

:::image type="content" source="./media/entity-suspend/subscription-state-change.png" alt-text="Selección del estado de la cola":::

Los estados que se pueden establecer para una tema son:
- **Activa**: El tema está activo.
- **Disabled**: El tema se ha suspendido.
- **ReceiveDisabled**: El mismo efecto que **Disabled**.

## <a name="other-statuses"></a>Otros estados
La enumeración [EntityStatus](/dotnet/api/microsoft.servicebus.messaging.entitystatus) también define un conjunto de estados de transición que solo puede establecer el sistema. 


## <a name="next-steps"></a>Pasos siguientes

Para más información sobre la mensajería de Service Bus, consulte los siguientes temas:

* [Colas, temas y suscripciones de Service Bus](service-bus-queues-topics-subscriptions.md)
* [Introducción a las colas de Service Bus](service-bus-dotnet-get-started-with-queues.md)
* [Uso de temas y suscripciones de Service Bus](service-bus-dotnet-how-to-use-topics-subscriptions.md)

[1]: ./media/entity-suspend/entity-state-change.png


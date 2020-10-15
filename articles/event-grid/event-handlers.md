---
title: Controladores de eventos de Azure Event Grid
description: En este artículo se describen los controladores de eventos compatibles con Azure Event Grid. Azure Automation, Functions, Event Hubs, Conexiones híbridas, Logic Apps, Service Bus, Queue Storage, Webhooks.
ms.topic: conceptual
ms.date: 07/07/2020
ms.openlocfilehash: 76f7fff3e0a3e1c0c554e452987bf944503358b8
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "86117022"
---
# <a name="event-handlers-in-azure-event-grid"></a>Controladores de eventos de Azure Event Grid
Un controlador de eventos es el lugar al que se envía el evento. El controlador realiza alguna acción adicional para procesar el evento. Varios servicios de Azure se configuran automáticamente para controlar los eventos. También puede usar cualquier webhook para controlar los eventos. El webhook no necesita estar hospedado en Azure para controlar los eventos. Event Grid solo admite puntos de conexión de webhook HTTPS.

## <a name="supported-event-handlers"></a>Controladores de evento admitidos
Estos son los controladores de eventos admitidos: 

- [Webhooks](handler-webhooks.md). Los runbooks y aplicaciones lógicas de Azure Automation se admiten a través de webhooks. 
- [Azure Functions](handler-functions.md)
- [Event Hubs](handler-event-hubs.md)
- [Retransmisión de conexiones híbridas](handler-relay-hybrid-connections.md)
- [Colas y temas de Service Bus](handler-service-bus.md)
- [Colas de almacenamiento](handler-storage-queues.md)

## <a name="next-steps"></a>Pasos siguientes
- Para obtener una introducción a Event Grid, vea [Acerca de Event Grid](overview.md).

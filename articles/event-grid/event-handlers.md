---
title: Controladores de eventos de Azure Event Grid
description: En este artículo se describen los controladores de eventos compatibles con Azure Event Grid. Azure Automation, Functions, Event Hubs, Conexiones híbridas, Logic Apps, Service Bus, Queue Storage, Webhooks.
services: event-grid
author: spelluru
ms.service: event-grid
ms.topic: conceptual
ms.date: 01/21/2020
ms.author: spelluru
ms.openlocfilehash: c29526bdee5ad70b3acb23117ee003877f52a55e
ms.sourcegitcommit: bb0afd0df5563cc53f76a642fd8fc709e366568b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/19/2020
ms.locfileid: "83592464"
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

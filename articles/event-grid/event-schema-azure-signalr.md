---
title: Azure SignalR como origen de Event Grid
description: Describe las propiedades que se proporcionan para los eventos de Azure SignalR con Azure Event Grid.
services: event-grid
author: femila
ms.service: event-grid
ms.topic: conceptual
ms.date: 04/23/2020
ms.author: femila
ms.openlocfilehash: 8fbae3fad4aeb85022c804e1ac648060360c6531
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "84560631"
---
# <a name="azure-event-grid-event-schema-for-signalr-service"></a>Esquema de eventos de Azure Event Grid para SignalR Service

En este artículo se proporcionan las propiedades y los esquemas de los eventos de SignalR Service. Para una introducción a los esquemas de eventos, consulte [Esquema de eventos de Azure Event Grid](event-schema.md). También proporciona una lista de inicios rápidos y tutoriales para usar Azure SignalR como un origen de eventos.

## <a name="event-grid-event-schema"></a>Esquema de eventos de Event Grid

### <a name="available-event-types"></a>Tipos de eventos disponibles

SignalR Service emite los siguientes tipos de eventos:

| Tipo de evento | Descripción |
| ---------- | ----------- |
| Microsoft.SignalRService.ClientConnectionConnected | Se genera cuando una conexión de cliente está conectada. |
| Microsoft.SignalRService.ClientConnectionDisconnected | Se genera cuando una conexión de cliente está desconectada. |

### <a name="example-event"></a>Evento de ejemplo

En el ejemplo siguiente, se muestra el esquema de un evento de dispositivo conectado en la conexión de cliente: 

```json
[{
  "topic": "/subscriptions/{subscription-id}/resourceGroups/signalr-rg/providers/Microsoft.SignalRService/SignalR/signalr-resource",
  "subject": "/hub/chat",
  "eventType": "Microsoft.SignalRService.ClientConnectionConnected",
  "eventTime": "2019-06-10T18:41:00.9584103Z",
  "id": "831e1650-001e-001b-66ab-eeb76e069631",
  "data": {
    "timestamp": "2019-06-10T18:41:00.9584103Z",
    "hubName": "chat",
    "connectionId": "crH0uxVSvP61p5wkFY1x1A",
    "userId": "user-eymwyo23"
  },
  "dataVersion": "1.0",
  "metadataVersion": "1"
}]
```

El esquema de un evento de dispositivo desconectado en la conexión de cliente es parecido: 

```json
[{
  "topic": "/subscriptions/{subscription-id}/resourceGroups/signalr-rg/providers/Microsoft.SignalRService/SignalR/signalr-resource",
  "subject": "/hub/chat",
  "eventType": "Microsoft.SignalRService.ClientConnectionDisconnected",
  "eventTime": "2019-06-10T18:41:00.9584103Z",
  "id": "831e1650-001e-001b-66ab-eeb76e069631",
  "data": {
    "timestamp": "2019-06-10T18:41:00.9584103Z",
    "hubName": "chat",
    "connectionId": "crH0uxVSvP61p5wkFY1x1A",
    "userId": "user-eymwyo23",
    "errorMessage": "Internal server error."
  },
  "dataVersion": "1.0",
  "metadataVersion": "1"
}]
```

### <a name="event-properties"></a>Propiedades de evento

Un evento tiene los siguientes datos de nivel superior:

| Propiedad | Tipo | Descripción |
| -------- | ---- | ----------- |
| topic | string | Ruta de acceso completa a los recursos del origen del evento. En este campo no se puede escribir. Event Grid proporciona este valor. |
| subject | string | Ruta al asunto del evento definida por el anunciante. |
| eventType | string | Uno de los tipos de eventos registrados para este origen de eventos. |
| eventTime | string | La hora de generación del evento en función de la hora UTC del proveedor. |
| id | string | Identificador único para el evento |
| datos | object | Datos de eventos de SignalR Service. |
| dataVersion | string | Versión del esquema del objeto de datos. El publicador define la versión del esquema. |
| metadataVersion | string | Versión del esquema de los metadatos del evento. Event Grid define el esquema de las propiedades de nivel superior. Event Grid proporciona este valor. |

El objeto data tiene las siguientes propiedades:

| Propiedad | Tipo | Descripción |
| -------- | ---- | ----------- |
| timestamp | string | La hora de generación del evento en función de la hora UTC del proveedor. |
| hubName | string | El centro al que pertenece la conexión de cliente. |
| connectionId | string | El identificador único de la conexión de cliente. |
| userId | string | El identificador de usuario definido en la notificación. |
| errorMessage | string | El error que provoca el evento de dispositivo desconectado de la conexión. |

## <a name="tutorials-and-how-tos"></a>Tutoriales y procedimientos
|Título | Descripción |
|---------|---------|
| [Reaccionar a eventos de Azure SignalR Service mediante Event Grid](../azure-signalr/signalr-concept-event-grid-integration.md) | Información general sobre la integración de Azure SignalR Service con Event Grid. |
| [Cómo enviar eventos de Azure SignalR Service a Event Grid](../azure-signalr/signalr-howto-event-grid-integration.md) | Muestra cómo enviar eventos de Azure SignalR Service a una aplicación a través de Event Grid. |

## <a name="next-steps"></a>Pasos siguientes

* Para una introducción a Azure Event Grid, consulte [Introducción a Azure Event Grid](overview.md).
* Para más información acerca de la creación de una suscripción de Azure Event Grid, consulte [Esquema de suscripción de Event Grid](subscription-creation-schema.md).

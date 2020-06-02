---
title: Supervisar la entrega de mensajes de Azure Event Grid
description: En este artículo se describe cómo usar Azure Portal para ver el estado de entrega de los mensajes de Azure Event Grid.
services: event-grid
author: spelluru
manager: timlt
ms.service: event-grid
ms.topic: conceptual
ms.date: 01/23/2020
ms.author: spelluru
ms.openlocfilehash: 7a01ab91fe84aaa1fe55018754eddbf8b8f89643
ms.sourcegitcommit: b396c674aa8f66597fa2dd6d6ed200dd7f409915
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/07/2020
ms.locfileid: "82890853"
---
# <a name="monitor-event-grid-message-delivery"></a>Supervisar la entrega de mensajes de Event Grid 

En este artículo se describe cómo usar el portal para ver el estado de las entregas de eventos.

Event Grid ofrece entrega duradera. Entrega cada mensaje por lo menos una vez en cada suscripción. Los eventos se envían inmediatamente al webhook registrado de cada suscripción. Si un webhook no acusa recibo de un evento en los 60 segundos siguientes al primer intento, Event Grid reintenta la entrega del evento.

Para obtener información acerca de los reintentos y las entregas de eventos, consulte [Entrega y reintento de entrega de mensajes de Event Grid](delivery-and-retry.md).

## <a name="delivery-metrics"></a>Métricas de entrega

El portal muestra las métricas para el estado de la entrega de mensajes de eventos.

En el caso de los temas, estas son algunas de las métricas:

* **Publicación correcta**: el evento se envió correctamente al tema y se procesó con una respuesta 2xx.
* **Error al publicar**: el evento se envió al tema, pero se rechazó con un código de error.
* **Sin coincidencia**: el evento se publicó correctamente en el tema, pero no coincide con ninguna suscripción a eventos. Se descartó el evento.

En el caso de las suscripciones, estas son algunas de las métricas:

* **Entrega realizada**: el evento se entregó correctamente al punto de conexión de la suscripción y se recibió una respuesta 2xx.
* **Entrega no realizada**: Cada vez que el servicio intenta entregar y el controlador de eventos no devuelve un código de error 2xx, se incrementa el contador de **error de entrega**. Si intentamos entregar el mismo evento varias veces y se produce un error, se incrementa el contador de **error de entrega** por cada error.
* **Eventos expirados**: no se entregó el evento y se enviaron todos los reintentos. Se descartó el evento.
* **Eventos coincidentes**: la suscripción a eventos coincide con el evento del tema.

    > [!NOTE]
    > Para obtener la lista completa de las métricas, consulte [Métricas compatibles con Azure Event Grid](metrics.md).

## <a name="event-subscription-status"></a>Estado de la suscripción a eventos

Para ver las métricas para una suscripción de eventos, puede buscar por tipo de suscripción o por suscripciones para un recurso concreto.

Para buscar por tipo de suscripción de eventos, seleccione **Todos los servicios**.

![Seleccionar todos los servicios](./media/monitor-event-delivery/all-services.png)

Busque **Event Grid** y seleccione **Suscripciones de Event Grid** entre las opciones disponibles.

![Buscar suscripciones a eventos](./media/monitor-event-delivery/search-and-select.png)

Filtre por el tipo de evento, la suscripción y la ubicación. Seleccione **Métricas** para la suscripción que quiere consultar.

![Filtrar suscripciones a eventos](./media/monitor-event-delivery/filter-events.png)

Consulte las métricas para el tema del evento y la suscripción.

![Ver métricas de eventos](./media/monitor-event-delivery/subscription-metrics.png)

Para buscar las métricas de un recurso concreto, seleccione ese recurso. Después, seleccione **Eventos**.

![Seleccionar eventos para un recurso](./media/monitor-event-delivery/select-events.png)

Puede ver las métricas para las suscripciones para ese recurso.

## <a name="custom-event-status"></a>Estado de evento personalizado

Si ha publicado un tema personalizado, puede consultar sus métricas. Seleccione el grupo de recursos del tema y seleccione el tema.

![Seleccionar tema personalizado](./media/monitor-event-delivery/select-custom-topic.png)

Consulte las métricas para el tema de eventos personalizado.

![Ver métricas de eventos](./media/monitor-event-delivery/custom-topic-metrics.png)

## <a name="set-alerts"></a>Establecer alertas

Puede establecer alertas en las métricas de nivel de tema y de dominio para temas personalizados y dominios de eventos. En la hoja de información general, seleccione **Alertas** en el menú de recursos de la izquierda para ver, administrar y crear reglas de alertas. [Obtener más información sobre alertas de Azure Monitor](../azure-monitor/platform/alerts-overview.md)

![Ver métricas de eventos](./media/monitor-event-delivery/select-alerts.png)

## <a name="next-steps"></a>Pasos siguientes

* Para obtener información acerca de los reintentos y las entregas de eventos, consulte [Entrega y reintento de entrega de mensajes de Event Grid](delivery-and-retry.md).
* Para obtener una introducción a Event Grid, vea [Acerca de Event Grid](overview.md).
* Para comenzar a usar rápidamente Event Grid, vea [Creación y enrutamiento de eventos personalizados con Azure Event Grid](custom-event-quickstart.md).

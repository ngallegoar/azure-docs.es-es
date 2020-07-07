---
title: Suscripciones de Azure Event Grid a través del portal
description: En este artículo se explica cómo crear suscripciones a Event Grid para los orígenes admitidos, como Azure Blob Storage, mediante Azure Portal.
services: event-grid
author: spelluru
ms.service: event-grid
ms.topic: conceptual
ms.date: 01/23/2020
ms.author: spelluru
ms.openlocfilehash: 599f48ed241010d8551bd110c7f778c9ef508eac
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "81393173"
---
# <a name="subscribe-to-events-through-portal"></a>Suscripción a eventos a través del portal

En este artículo se describe cómo crear suscripciones de Event Grid a través del portal.

## <a name="create-event-subscriptions"></a>Creación de suscripciones de eventos

Para crear una suscripción de Event Grid para cualquiera de los [orígenes de eventos](overview.md#event-sources) admitidos, siga estos pasos. En este artículo se muestra cómo crear una suscripción de Event Grid para una suscripción a Azure.

1. Seleccione **Todos los servicios**.

   ![Seleccionar todos los servicios](./media/subscribe-through-portal/select-all-services.png)

1. Busque **Suscripciones de Event Grid** y selecciónela entre las opciones disponibles.

   ![Search](./media/subscribe-through-portal/search.png)

1. Seleccione **+ Suscripción de eventos**.

   ![Agregar una suscripción](./media/subscribe-through-portal/add-subscription.png)

1. Seleccione el tipo de suscripción que desea crear. Por ejemplo, para suscribirse a eventos desde su suscripción a Azure, seleccione **Suscripciones de Azure** y la suscripción de destino.

   ![Selección de una suscripción a Azure](./media/subscribe-through-portal/azure-subscription.png)

1. Para suscribirse a todos los tipos de evento para este origen del evento, mantenga activada la opción **Suscribirse a todos los tipos de evento**. En caso contrario, seleccione los tipos de evento para esta suscripción.

   ![Selección de los tipos de evento](./media/subscribe-through-portal/select-event-types.png)

1. Proporcione detalles adicionales sobre la suscripción a los eventos, como el punto de conexión para el control de eventos y un nombre de la suscripción.

   ![Indicación de detalles de la suscripción](./media/subscribe-through-portal/provide-subscription-details.png)

1. Para habilitar la cola de mensajes fallidos y personalizar las directivas de reintento, seleccione **Características adicionales**.

   ![Selección de características adicionales](./media/subscribe-through-portal/select-additional-features.png)

1. Seleccione un contenedor para almacenar los eventos que no se pueden entregar y establezca cómo se envían los reintentos.

   ![Habilitación de la cola de mensajes fallidos y reintentos](./media/subscribe-through-portal/set-deadletter-retry.png)

1. Cuando haya terminado, seleccione **Crear**.

## <a name="create-subscription-on-resource"></a>Creación de una suscripción en un recurso

Algunos orígenes de eventos admiten la creación de una suscripción de eventos a través de la interfaz del portal para ese recurso. Seleccione el origen del evento y busque **Eventos** en el panel izquierdo.

![Indicación de detalles de la suscripción](./media/subscribe-through-portal/resource-events.png)

El portal presenta opciones para crear una suscripción de eventos que sea pertinente para ese origen.

## <a name="next-steps"></a>Pasos siguientes

* Para obtener información acerca de los reintentos y las entregas de eventos, consulte [Entrega y reintento de entrega de mensajes de Event Grid](delivery-and-retry.md).
* Para obtener una introducción a Event Grid, vea [Acerca de Event Grid](overview.md).
* Para comenzar a usar rápidamente Event Grid, vea [Creación y enrutamiento de eventos personalizados con Azure Event Grid](custom-event-quickstart.md).

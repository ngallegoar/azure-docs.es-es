---
title: Visualización de las métricas de Azure Event Grid y configuración de alertas
description: En este artículo se describe cómo usar Azure Portal para ver las métricas de temas y suscripciones de Azure Event Grid, y cómo crear alertas en ellas.
ms.topic: conceptual
ms.date: 07/07/2020
ms.openlocfilehash: 518d34d39e6fbecc408fe9a44d899fe4745d60d0
ms.sourcegitcommit: a43a59e44c14d349d597c3d2fd2bc779989c71d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/25/2020
ms.locfileid: "96008963"
---
# <a name="monitor-event-grid-message-delivery"></a>Supervisar la entrega de mensajes de Event Grid 
En este artículo se describe cómo usar el portal para ver las métricas de temas y suscripciones de Event Grid, y cómo crear alertas en ellas. 

## <a name="metrics"></a>Métricas

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

## <a name="view-custom-topic-metrics"></a>Ver métricas de temas personalizados

Si ha publicado un tema personalizado, puede consultar sus métricas. 

1. Inicie sesión en el [portal de Azure](https://portal.azure.com/).
2. En la barra de búsqueda del tema, escriba **Temas de Event Grid** y, a continuación, seleccione **Temas de Event Grid** en la lista desplegable. 

    :::image type="content" source="./media/custom-event-quickstart-portal/select-event-grid-topics.png" alt-text="Búsqueda y selección de temas de Event Grid":::
3. Seleccione el tema personalizado en la lista de temas. 

    :::image type="content" source="./media/monitor-event-delivery/select-custom-topic.png" alt-text="Seleccione el tema personalizado":::
4. Vea las métricas del tema de evento personalizado en la página de **Temas de Event Grid**. En la imagen siguiente, se minimiza la sección de **Essentials** que muestra el grupo de recursos, la suscripción, etc. 

    :::image type="content" source="./media/monitor-event-delivery/custom-topic-metrics.png" alt-text="Visualización de las métricas de eventos":::

Puede crear gráficos con métricas compatibles mediante la pestaña de **Métricas** de la página de **Temas de Event Grid**.

:::image type="content" source="./media/monitor-event-delivery/topics-metrics-page.png" alt-text="Temas: página de métricas":::

Para obtener más información sobre las métricas, consulte [Métricas en Azure Monitor](../azure-monitor/platform/data-platform-metrics.md).

Por ejemplo, vea el gráfico de métricas para la métrica de **Eventos publicados**.

:::image type="content" source="./media/monitor-event-delivery/custom-topic-metrics-example.png" alt-text="Métrica de eventos publicados":::


## <a name="view-subscription-metrics"></a>Visualización de las métricas de suscripción
1. Desplácese hasta la página de **Temas de Event Grid** siguiendo los pasos de la sección anterior. 
2. Seleccione la suscripción en el panel inferior, tal y como se muestra en el ejemplo siguiente. 

    :::image type="content" source="./media/monitor-event-delivery/select-event-subscription.png" alt-text="Selección de la suscripción de eventos":::    

    También puede buscar **Suscripciones de Event Grid** en la barra de búsqueda de Azure Portal, seleccionar **Tipo de temas**, **Suscripción** y **Ubicación** para ver una suscripción de eventos. 

    :::image type="content" source="./media/monitor-event-delivery/event-subscriptions-page.png" alt-text="Seleccionar suscripción de eventos desde la página de suscripciones de Event Grid":::        

    Para los temas personalizados, seleccione **Temas de Event Grid** como **Tipo de temas**. Para los temas del sistema, seleccione el tipo de recurso de Azure, por ejemplo **Cuentas de almacenamiento (Blob, GPv2)** . 
3. Vea las métricas de la suscripción en la Página principal de la suscripción en un gráfico. Puede ver las métricas de **General**, **Error**, **Latencia** y **Mensajes no enviados** durante la hora, 6 horas, 12 horas, 1 día, 7 días o 30 días anteriores. 

    :::image type="content" source="./media/monitor-event-delivery/subscription-home-page-metrics.png" alt-text="Métricas en la página principal de la suscripción":::    

## <a name="view-system-topic-metrics"></a>Ver las métricas de los temas del sistema

1. Inicie sesión en el [portal de Azure](https://portal.azure.com/).
2. En la barra de búsqueda del tema, escriba **Temas del sistema de Event Grid** y, a continuación, seleccione **Temas de sistema de Event Grid** en la lista desplegable. 

    :::image type="content" source="./media/monitor-event-delivery/search-system-topics.png" alt-text="Búsqueda y selección de temas del sistema de Event Grid":::
3. Seleccione el tema del sistema en la lista de temas. 

    :::image type="content" source="./media/monitor-event-delivery/select-system-topic.png" alt-text="Seleccione el tema del sistema":::.
4. Vea las métricas de tema de sistema en la página de **Temas del sistema de Event Grid**. En la imagen siguiente, se minimiza la sección de **Essentials** que muestra el grupo de recursos, la suscripción, etc. 

    :::image type="content" source="./media/monitor-event-delivery/system-topic-overview-metrics.png" alt-text="Ver las métricas de los temas del sistema en la página información general":::

Puede crear gráficos con métricas compatibles mediante la pestaña de **Métricas** de la página de **Temas de Event Grid**.

:::image type="content" source="./media/monitor-event-delivery/system-topic-metrics-page.png" alt-text="Temas del sistema: página de métricas":::

Para obtener más información sobre las métricas, consulte [Métricas en Azure Monitor](../azure-monitor/platform/data-platform-metrics.md).


## <a name="next-steps"></a>Pasos siguientes
Vea los artículos siguientes:

- Para obtener información sobre cómo crear alertas de métricas y operaciones de registro de actividad, consulte [Establecer alertas](set-alerts.md).
- Para obtener información acerca de los reintentos y las entregas de eventos, consulte [Entrega y reintento de entrega de mensajes de Event Grid](delivery-and-retry.md).

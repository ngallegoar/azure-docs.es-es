---
title: Establecimiento de alertas para operaciones de registro de actividad y métricas de Azure Event Grid
description: En este artículo se describe cómo crear alertas en métricas y operaciones de registro de actividad de Azure Event Grid.
ms.topic: conceptual
ms.date: 07/07/2020
ms.openlocfilehash: 615ebef749be49822a09470eb0d47d8eb31b9a5a
ms.sourcegitcommit: d7008edadc9993df960817ad4c5521efa69ffa9f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/08/2020
ms.locfileid: "86119147"
---
# <a name="set-alerts-on-azure-event-grid-metrics-and-activity-logs"></a>Establecimiento de alertas en métricas y registros de actividad de Azure Event Grid
En este artículo se describe cómo crear alertas en métricas y operaciones de registro de actividad de Azure Event Grid. Puede crear alertas tanto en las métricas de publicación como de entrega para los recursos de Azure Event Grid (temas y dominios). En los temas del sistema, [cree alertas con la página **Métricas**](#create-alerts-using-the-metrics-page).

## <a name="create-alerts-on-dead-lettered-events"></a>Creación de alertas en eventos de la cola de mensajes fallidos
En el siguiente procedimiento se muestra cómo crear una alerta sobre la métrica de **eventos en la cola de mensajes fallidos** para un tema personalizado. En este ejemplo, se envía un correo electrónico al propietario del grupo de recursos de Azure cuando el recuento de eventos en la cola de mensajes fallidos para un tema supere los diez. 

1. En la página **Tema de Event Grid** del tema, seleccione **Alertas** en el menú de la izquierda y, a continuación, seleccione **+ Nueva regla de alertas**. 

    :::image type="content" source="./media/monitor-event-delivery/new-alert-button.png" alt-text="Página Alertas: botón Nueva regla de alertas":::
2. En la página **Crear regla de alerta**, compruebe que se ha seleccionado el tema para el recurso. A continuación, haga clic en **Seleccionar condición**. 

    :::image type="content" source="./media/monitor-event-delivery/alert-select-condition.png" alt-text="Página de alertas: Seleccionar condición":::    
3. En la página **Configurar lógica de señal**, siga estos pasos:
    1. Seleccione una métrica o una entrada del registro de actividad. En este ejemplo, se selecciona **Eventos en la cola de mensajes fallidos**. 

        :::image type="content" source="./media/monitor-event-delivery/select-dead-lettered-events.png" alt-text="Seleccionar eventos en la cola de mensajes fallidos":::        
    2. Seleccione las dimensiones (opcional). 
        
        :::image type="content" source="./media/monitor-event-delivery/configure-signal-logic.png" alt-text="Configurar lógica de señal":::        

        > [!NOTE]
        > Puede seleccionar el botón **+** para **EventSubscriptionName** si desea especificar un nombre de suscripción de evento para filtrar los eventos. 
    3. Desplácese hacia abajo. En la sección **Lógica de alerta**, seleccione un **Operador** y **Tipo de agregación**, escriba un **Valor de umbral** y seleccione **Listo**. En este ejemplo, se desencadena una alerta cuando el número total de eventos con mensajes fallidos sea mayor que diez. 
    
        :::image type="content" source="./media/monitor-event-delivery/alert-logic.png" alt-text="Lógica de alerta":::                
4. Ahora, en la página **Crear regla de alertas**, haga clic en **Seleccionar el grupo de acciones**.

    :::image type="content" source="./media/monitor-event-delivery/select-action-group-button.png" alt-text="Botón Seleccionar el grupo de acciones":::
5. Seleccione **Crear grupo de acciones** en la barra de herramientas para crear un nuevo grupo de acciones. También puede seleccionar un grupo existente.        
6. En la página **Agregar grupo de acciones**, siga estos pasos:
    1. Escriba un **nombre para el grupo de acciones**.
    1. Escriba un **nombre corto** para el grupo de acciones.
    1. Seleccione una **Suscripción de Azure** donde desee crear el grupo de acciones.
    1. Seleccione el **grupo de recursos de Azure** en el que desee crear el grupo de acciones.
    1. Escriba un **nombre para la acción**. 
    1. Seleccione el **tipo de acción**. En este ejemplo, se ha seleccionado el **rol de correo electrónico de Azure Resource Manager**; en concreto, el rol **Propietarios**. 
    1. Seleccione **Aceptar** para cerrar la página. 
    
        :::image type="content" source="./media/monitor-event-delivery/add-action-group-page.png" alt-text="Página Agregar grupo de acciones":::                   
7. De nuevo en la página **Crear regla de alertas**, escriba un nombre para la regla y, a continuación, seleccione **Crear regla de alertas**.

    :::image type="content" source="./media/monitor-event-delivery/alert-rule-name.png" alt-text="Nombre de la regla de alertas":::  
8. Ahora, en la página **Alertas** del tema, verá un vínculo para administrar las reglas de alertas si aún no hay ninguna alerta. Si las hay, seleccione **Administrar reglas de alerta** en la barra de herramientas.  

    :::image type="content" source="./media/monitor-event-delivery/manage-alert-rules.png" alt-text="Administrar alertas":::

## <a name="create-alerts-on-other-metrics-or-activity-log-operations"></a>Creación de alertas en otras métricas o operaciones de registro de actividad
En la sección anterior se mostró cómo crear alertas de eventos en la cola de mensajes fallidos. Los pasos para crear alertas en otras métricas o en operaciones de registro de actividad son similares. 

Por ejemplo, para crear una alerta en un evento de error de entrega, seleccione **Eventos de error en la entrega** en la página **Configurar lógica de señal**. 

:::image type="content" source="./media/set-alerts/delivery-failed-events.png" alt-text="Selección de entrega de eventos fallidos":::


## <a name="create-alerts-using-the-metrics-page"></a>Creación de alertas mediante la página Métricas
También puede crear alertas con la página **Métricas**. Los pasos son similares. En el caso de los temas del sistema, solo puede usar la página **Métricas** para crear alertas, ya que la página **Alertas** no está disponible. 

:::image type="content" source="./media/monitor-event-delivery/metric-page-create-alert-button.png" alt-text="Página Métricas: botón Crear alerta":::   
    

> [!NOTE]
> En este artículo no se tratan todos los pasos y combinaciones diferentes que puede usar para crear una alerta. Para información general sobre las alertas, consulte [Información general de las alertas](../azure-monitor/platform/alerts-metric.md).

## <a name="next-steps"></a>Pasos siguientes

* Para obtener información acerca de los reintentos y las entregas de eventos, consulte [Entrega y reintento de entrega de mensajes de Event Grid](delivery-and-retry.md).
* Para obtener una introducción a Event Grid, vea [Acerca de Event Grid](overview.md).
* Para comenzar a usar rápidamente Event Grid, vea [Creación y enrutamiento de eventos personalizados con Azure Event Grid](custom-event-quickstart.md).

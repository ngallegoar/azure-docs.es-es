---
title: Visualización de recomendaciones de Azure Advisor interesantes para usted
description: Vea y filtre las recomendaciones de Azure Advisor para reducir el ruido.
ms.topic: article
ms.date: 04/03/2019
ms.openlocfilehash: 43bdaba7d774bba8857a6eb3ef296d7ab8e1c264
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "90986863"
---
# <a name="view-azure-advisor-recommendations-that-matter-to-you"></a>Visualización de recomendaciones de Azure Advisor interesantes para usted

Azure Advisor proporciona recomendaciones para ayudarle a optimizar sus implementaciones de Azure. En Advisor, dispondrá de acceso a algunas características que le ayudarán a reducir las recomendaciones a solo aquellas interesantes para usted.

## <a name="configure-subscriptions-and-resource-groups"></a>Configuración de suscripciones y grupos de recursos

Advisor le permite seleccionar las suscripciones y grupos de recursos interesantes para usted y su organización. Solo verá las recomendaciones para las suscripciones y grupos de recursos que seleccione. De forma predeterminada, se seleccionan todas. Los valores de configuración se aplican a la suscripción o al grupo de recursos, por lo que se aplica la misma configuración a todas las personas que dispongan de acceso a esa suscripción o grupo de recursos. Los valores de configuración se pueden cambiar en Azure Portal o mediante programación.

Para realizar cambios en Azure Portal, realice el siguiente procedimiento:

1. Abra [Azure Advisor](https://aka.ms/azureadvisordashboard) en Azure Portal.

1. Seleccione **Configuración** del menú.

   ![Menú de configuración de Advisor](./media/view-recommendations/configuration.png)

1. Active la casilla en la columna **Incluir** para que las suscripciones o grupos de recursos reciban recomendaciones de Advisor. Si la casilla está atenuada, puede que no disponga de permiso para realizar un cambio de configuración en esa suscripción o grupo de recursos. Más información sobre [permisos en Azure Advisor](permissions.md).

1. Haga clic en **Aplicar** en la parte inferior después de realizar un cambio.

## <a name="filtering-your-view-in-the-azure-portal"></a>Filtrado de la vista en Azure Portal

Los valores de configuración siguen estando activos hasta que se cambian. Si desea limitar la vista de recomendaciones para una única visualización, puede usar los menús desplegables proporcionados en la parte superior del panel de Advisor. Desde los paneles Información general, Alta disponibilidad, Seguridad, Rendimiento, Costo y Todas las recomendaciones, puede seleccionar las suscripciones, los tipos de recurso y el estado de las recomendaciones que desee ver.

   :::image type="content" source="./media/view-recommendations/filtering.png" alt-text="Captura de pantalla de Azure Advisor que muestra las opciones de filtrado.":::

## <a name="dismissing-and-postponing-recommendations"></a>Descartar y posponer recomendaciones

Azure Advisor le permite descartar o posponer recomendaciones en un único recurso. Si descarta una recomendación, no la volverá a ver a no ser que la active manualmente. Sin embargo, posponer una recomendación le permite especificar un plazo, que una vez transcurrido, active automáticamente de nuevo la recomendación. Esta acción se puede realizar en Azure Portal o mediante programación.

### <a name="postpone-a-single-recommendation-in-the-azure-portal"></a>Posponer una recomendación única en Azure Portal 

1. Abra [Azure Advisor](https://aka.ms/azureadvisordashboard) en Azure Portal.
1. Seleccione una categoría de recomendaciones para ver sus recomendaciones.
1. Seleccione una recomendación de la lista de recomendaciones.
1. Seleccione Posponer o Descartar para la recomendación que desee posponer o descartar.

     :::image type="content" source="./media/view-recommendations/postpone-dismiss.png" alt-text="Captura de pantalla de Azure Advisor que muestra las opciones de filtrado.":::

### <a name="postpone-or-dismiss-a-multiple-recommendations-in-the-azure-portal"></a>Posponer o descartar varias recomendaciones en Azure Portal

1. Abra [Azure Advisor](https://aka.ms/azureadvisordashboard) en Azure Portal.
1. Seleccione una categoría de recomendaciones para ver sus recomendaciones.
1. Seleccione una recomendación de la lista de recomendaciones.
1. Active la casilla en la parte izquierda de la fila para todos los recursos para los que desee posponer o descartar la recomendación.
1. Seleccione **Posponer** o **Descartar** en la parte superior izquierda de la tabla.

     :::image type="content" source="./media/view-recommendations/postpone-dismiss-multiple.png" alt-text="Captura de pantalla de Azure Advisor que muestra las opciones de filtrado.":::

> [!NOTE]
> Necesita el permiso de un colaborador o propietario para descartar o posponer una recomendación. Más información sobre permisos en Azure Advisor.

> [!NOTE]
> Aunque el cuadro de selección esté desactivado, las recomendaciones pueden seguir cargándose. Espere a que se carguen todas las recomendaciones antes de intentar realizar las acciones de posponer o descartar.

### <a name="reactivate-a-postponed-or-dismissed-recommendation"></a>Reactivación de una recomendación pospuesta o descartada

Puede activar una recomendación pospuesta o descartada. Esta acción se puede realizar en Azure Portal o mediante programación. En Azure Portal:

1. Abra [Azure Advisor](https://aka.ms/azureadvisordashboard) en Azure Portal.

1. Cambie el filtro del panel Información general a **Pospuestas**. Advisor le mostrará a continuación las recomendaciones pospuestas o descartadas.

    :::image type="content" source="./media/view-recommendations/activate-postponed.png" alt-text="Captura de pantalla de Azure Advisor que muestra las opciones de filtrado.":::

1. Seleccione una categoría para ver recomendaciones **Pospuestas** y **Descartadas**.

1. Seleccione una recomendación de la lista de recomendaciones. Si realiza ese procedimiento, se abrirán las recomendaciones con la pestaña **Pospuestas y Descartadas** ya seleccionada para mostrar los recursos para los que se ha pospuesto o descartado esa recomendación.

1. Haga clic en **Activar** al final de la fila. Una vez que haya hecho clic, la recomendación estará activa para ese recurso y, por lo tanto, se eliminará de la tabla. La recomendación aparecerá ahora en la pestaña **Activo**.
 
     :::image type="content" source="./media/view-recommendations/activate-postponed-2.png" alt-text="Captura de pantalla de Azure Advisor que muestra las opciones de filtrado.":::

## <a name="next-steps"></a>Pasos siguientes

En este artículo se explica cómo puede ver recomendaciones interesantes para usted en Azure Advisor. Para más información sobre Advisor, consulte: 

- [¿Qué es Azure Advisor?](advisor-overview.md)
- [Introducción a Advisor](advisor-get-started.md)
- [Permisos en Azure Advisor](permissions.md)




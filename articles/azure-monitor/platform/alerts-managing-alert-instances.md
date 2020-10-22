---
title: Administración de instancias de alertas en Azure Monitor
description: Administración de instancias de alertas en Azure
ms.subservice: alerts
ms.topic: conceptual
ms.date: 09/24/2018
ms.openlocfilehash: 82905cba5f391365ada13f4e5df5ad139f4c121e
ms.sourcegitcommit: ae6e7057a00d95ed7b828fc8846e3a6281859d40
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/16/2020
ms.locfileid: "92102877"
---
# <a name="manage-alert-instances-with-unified-alerts"></a>Administrar instancias de alerta con alertas unificadas

Gracias a la [experiencia de alertas unificadas](./alerts-overview.md) en Azure Monitor, puede ver todos los tipos de alertas en Azure. Aquí se abarcan varias suscripciones en un solo panel. En este artículo se explica cómo puede ver sus instancias de alerta y cómo buscar instancias de alerta específicas para la solución de problemas.

> [!NOTE]
> Solo puede acceder a las alertas generadas en los últimos 30 días.

## <a name="go-to-the-alerts-page"></a>Ir a la página de alertas

Puede ir a la página de alertas de cualquiera de las siguientes maneras:

- En [Azure Portal](https://portal.azure.com/), seleccione **Monitor** > **Alertas**.  

     ![Captura de pantalla de las alertas de Monitor](media/alerts-managing-alert-instances/monitoring-alerts-managing-alert-instances-toc.jpg)
  
- Use el contexto de un recurso específico. Abra un recurso, vaya a la sección de **supervisión** y seleccione **Alertas**. La página de aterrizaje se filtrará previamente para mostrar las alertas de ese recurso específico.

     ![Captura de pantalla de las alertas de supervisión de recursos](media/alerts-managing-alert-instances/alert-resource.JPG)

- Use el contexto de un grupo de recursos específico. Abra un grupo de recursos, vaya a la sección de **supervisión** y seleccione **Alertas**. La página de aterrizaje se filtrará previamente para mostrar las alertas de ese grupo de recursos específico.    

     ![Captura de pantalla de alertas de supervisión del grupo de recursos](media/alerts-managing-alert-instances/alert-rg.JPG)

## <a name="find-alert-instances"></a>Buscar instancias de alertas

La página **Resumen de alertas** le proporciona información general de todas sus instancias de alerta en Azure. Puede modificar la vista de resumen seleccionando **varias suscripciones** (hasta un máximo de 5) o filtrando por **grupos de recursos**, ya sean **recursos** o **intervalos de tiempo** específicos. Haga clic en **Alertas totales** o en cualquiera de las bandas de gravedad para ir a la vista de lista de sus alertas.     

![Captura de pantalla de la página de resumen de alertas](media/alerts-managing-alert-instances/alerts-summary.jpg)
 
En la página **Todas las alertas** se muestran todas las instancias de alerta en Azure. Si accede al portal desde una notificación de alerta, puede usar los filtros disponibles para limitar esa instancia de alerta específica.

> [!NOTE]
> Si llegó a la página haciendo clic en cualquiera de las bandas de gravedad, la lista se filtrará previamente en función de esa gravedad.

Además de los filtros disponibles en la página anterior, también puede realizar procesos de filtrado en función del servicio de supervisión (por ejemplo, Plataforma para métricas), la condición de la supervisión (activada o resuelta), la gravedad, el estado de las alertas (nueva, confirmada y cerrada), o el id. de grupo inteligente.

![Captura de pantalla de la página de alertas](media/alerts-managing-alert-instances/all-alerts.jpg)

> [!NOTE]
> Si llegó a la página haciendo clic en cualquiera de las bandas de gravedad, la lista se filtrará previamente en función de esa gravedad.

Al seleccionar cualquier instancia de alerta, se abre la página **Detalles de alerta**, lo que le permite obtener más detalles sobre esa instancia de alerta específica.   

![Captura de pantalla de la página de detalles de alertas](media/alerts-managing-alert-instances/alert-details.jpg)
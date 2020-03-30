---
title: Use el streaming en vivo de búsqueda de Azure Sentinel para detectar amenazas | Microsoft Docs
description: En este artículo se describe cómo usar el streaming en vivo de búsqueda de Azure Sentinel para llevar un seguimiento de los datos.
services: sentinel
documentationcenter: na
author: yelevin
manager: rkarlin
editor: ''
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: conceptual
ms.custom: mvc
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/06/2019
ms.author: yelevin
ms.openlocfilehash: b392644e504fa8187e637278bef8718c9c2caa3f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "77582133"
---
# <a name="use-hunting-livestream-in-azure-sentinel-to-detect-threats"></a>Uso del streaming en vivo de búsqueda de Azure Sentinel para detectar amenazas

> [!IMPORTANT]
> El streaming en vivo de búsqueda de Azure Sentinel se encuentra actualmente en versión preliminar pública y se implementa gradualmente en los inquilinos.
> Esta característica se ofrece sin contrato de nivel de servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas. Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).


Use el streaming en vivo de búsqueda para crear sesiones interactivas que permitan probar las consultas recién creadas a medida que se produzcan eventos, recibir notificaciones de las sesiones cuando se encuentre una coincidencia e iniciar investigaciones si es necesario. Puede crear rápidamente una sesión de streaming en vivo mediante cualquier consulta de Log Analytics.

- **Pruebe las consultas recién creadas en tiempo real**
    
    Puede probar y ajustar las consultas sin conflictos con las reglas actuales que se aplican activamente a los eventos. Después de confirmar que estas nuevas consultas funcionan según lo previsto, es fácil promocionarlas en reglas de alerta personalizadas seleccionando una opción que eleva la sesión a una alerta.

- **Reciba una notificación cuando se produzcan amenazas**
    
    Puede comparar las fuentes de distribución de datos de amenazas con los datos de registro agregados y recibir una notificación cuando se produzca una coincidencia. Las fuentes de distribución de datos de amenazas son secuencias de datos en curso que están relacionadas con amenazas potenciales o actuales, por lo que la notificación podría indicar una amenaza potencial para la organización. Cree una sesión de streaming en vivo en lugar de una regla de alerta personalizada cuando quiera recibir la notificación de un problema potencial sin las sobrecargas del mantenimiento de una regla de alertas personalizada.

- **Inicio de investigaciones**
    
    Si hay una investigación activa que implique un recurso como un host o un usuario, puede ver la actividad específica (o cualquiera) en los datos de registro tal como se produce en dicho recurso. Puede recibir una notificación cuando se produzca esa actividad.


## <a name="create-a-livestream-session"></a>Creación de una sesión de streaming en vivo

Puede crear una sesión de streaming en vivo a partir de una consulta de búsqueda existente o crear la sesión desde cero.

1. En Azure Portal, vaya a **Sentinel** > **Administración de amenazas** > **Búsqueda**.

2. Para crear una sesión de streaming en vivo a partir de una consulta de búsqueda:
    
    1. En la pestaña **Consultas**, busque la consulta de búsqueda que se va a usar.
    2. Haga clic con el botón secundario en la consulta y seleccione **Agregar a streaming en vivo**. Por ejemplo:
    
    > [!div class="mx-imgBorder"]
    > ![creación de una sesión de streaming en vivo desde una consulta de búsqueda de Azure Sentinel](./media/livestream/livestream-from-query.png)

3. Para crear una sesión de streaming en vivo desde cero: 
    
    1. Seleccione la pestaña **Streaming en vivo**
    2. Seleccione **Ir a streaming en vivo**.
    
4. En el panel de **Streaming en vivo**:
    
    - Si inició streaming en vivo desde una consulta, revise la consulta y realice los cambios que desee realizar.
    - Si inició streaming en vivo desde cero, cree la consulta. 

5. En la barra de comandos, seleccione **Reproducir**.
    
    La barra de estado de la barra de comandos indica si la sesión de streaming en vivo está en ejecución o en pausa. En el ejemplo siguiente, la sesión se está ejecutando:
    
    > [!div class="mx-imgBorder"]
    > ![creación de una sesión de streaming en vivo desde una búsqueda de Azure Sentinel](./media/livestream/livestream-session.png)

6. En la barra de comandos, seleccione **Guardar**.
    
    A menos que seleccione **Pausar**, la sesión continuará ejecutándose hasta que salga del Azure Portal.

## <a name="view-your-livestream-sessions"></a>Visualización de las sesiones de streaming en vivo

1. En Azure Portal, vaya a **Sentinel** > **Administración de amenazas** > **Búsqueda** > **Streaming en vivo**.

2. Seleccione la sesión de streaming en vivo que desea ver o editar. Por ejemplo:
    
    > [!div class="mx-imgBorder"]
    > ![creación de una sesión de streaming en vivo desde una consulta de búsqueda de Azure Sentinel](./media/livestream/livestream-tab.png)
    
    Se abre la sesión de streaming en vivo seleccionada para que pueda reproducirla, pausarla, editarla, etc.

## <a name="receive-notifications-when-new-events-occur"></a>Recibir notificaciones cuando se produzcan nuevos eventos

Dado que las notificaciones de streaming en vivo para los nuevos eventos usan notificaciones de Azure Portal, verá estas notificaciones cada vez que use Azure Portal. Por ejemplo:

![Notificación de Azure Portal para el streaming en vivo](./media/livestream/notification.png)

Seleccione la notificación para abrir el panel de **Streaming en vivo**.
 
## <a name="elevate-a-livestream-session-to-an-alert"></a>Elevar una sesión de streaming en vivo a una alerta

Puede promover una sesión de streaming en vivo a una nueva alerta si selecciona **Elevar a alerta** en la barra de comandos de la sesión de streaming en vivo correspondiente:

> [!div class="mx-imgBorder"]
> ![Elevar la sesión de streaming en vivo a una alerta](./media/livestream/elevate-to-alert.png)

Esta acción abre el asistente para crear reglas, que se rellena previamente con la consulta que está asociada a la sesión de streaming en vivo.

## <a name="next-steps"></a>Pasos siguientes

En este artículo, aprendió cómo ejecutar un streaming en vivo de búsqueda en Azure Sentinel. Para más información sobre Azure Sentinel, consulte los siguientes artículos:

- [Búsqueda de amenazas con Azure Sentinel, versión preliminar](hunting.md)
- [Uso de cuadernos de Jupyter Notebook para buscar amenazas de seguridad](notebooks.md)

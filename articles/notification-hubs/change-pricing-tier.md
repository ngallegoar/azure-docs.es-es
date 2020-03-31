---
title: Cambio del plan de tarifa del espacio de nombres de Notification Hubs | Microsoft Docs
description: Aprenda a cambiar el plan de tarifa de un espacio de nombres de Azure Notification Hubs.
services: notification-hubs
author: sethmanheim
manager: femila
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: article
ms.date: 01/28/2019
ms.author: sethm
ms.reviewer: jowargo
ms.lastreviewed: 01/28/2019
ms.openlocfilehash: 51d0880b7f56d523a01fbc993993b3caf7328134
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "76261082"
---
# <a name="change-pricing-tier-of-an-azure-notification-hubs-namespace"></a>Cambio del plan de tarifa de un espacio de nombres de Azure Notification Hubs
Notification Hubs se ofrece en tres niveles: **gratis**, **básico** y **estándar**. En este artículo se muestra cómo cambiar el plan de tarifa para un espacio de nombres de Azure Notification Hubs. 

## <a name="overview"></a>Información general
En Azure Notification Hubs, un **centro** es la entidad o recurso más pequeño. Por lo general, se asigna a una aplicación y puede contener un certificado para cada Sistema de notificación de plataforma que se admite en la aplicación. La aplicación puede ser híbrida, nativa o una aplicación multiplataforma.

Un **espacio de nombres** es una colección de centros de notificaciones. Por lo general, cada espacio de nombres consta de centros relacionados y que se usan con un propósito específico. Por ejemplo, podría tener tres espacios de nombres distintos con fines de desarrollo, prueba y producción, respectivamente. 

Puede asociar un plan de tarifa en el nivel de espacio de nombres. Notification Hubs se ofrece en tres niveles: **gratis**, **básico** y **estándar**. Puede usar el nivel para un espacio de nombres que se adapte a sus requisitos. Las secciones siguientes muestran cómo cambiar el plan de tarifa de un espacio de nombres de Notification Hubs. 

## <a name="use-azure-portal"></a>Usar Azure Portal 
Al usar Azure Portal, puede cambiar el plan de tarifa de un espacio de nombres en la página del espacio de nombres o en la página de un centro.  Cuando lo cambie en la página de un centro, realmente lo cambia en el nivel de espacio de nombres. Cambia el plan de tarifa para el espacio de nombres y todos los centros del espacio de nombres. 

### <a name="change-tier-on-the-namespace-page"></a>Cambio del nivel en la página del espacio de nombres
El procedimiento siguiente le da los pasos para cambiar el plan de tarifa de un espacio de nombres en la página del espacio de nombres. Al cambiar el nivel de un espacio de nombres, se aplica a todos los centros del espacio de nombres.

1. Inicie sesión en [Azure Portal](https://portal.azure.com).
2. Seleccione **Todos los servicios** en el menú de la izquierda. 
3. Seleccione **Espacios de nombres del centro de notificaciones** en la sección **Internet de las cosas**. Si selecciona la estrella (`*`) que aparece junto al texto, se agrega en la barra de navegación de la izquierda bajo **FAVORITOS**. Ayuda a acceder a la página de espacios de nombres más rápido desde la próxima vez. Una vez que lo agregue a FAVORITOS, seleccione **Espacios de nombre del centro de notificaciones**. 

    ![Todos los servicios -> Espacios de nombre del centro de notificaciones](./media/change-pricing-tier/all-services-nhub.png)
1. En la página **Espacios de nombres del centro de notificaciones**, seleccione el espacio de nombres para el que quiere cambiar el plan de tarifa. 
2. En la página **Espacio de nombres del centro de notificaciones** correspondiente al espacio de nombres, puede ver el plan de tarifa actual para el espacio de nombres de la sección **Essentials**. En la imagen siguiente, puede ver que el plan de tarifa del espacio de nombres es **Gratis**. 

    ![Plan de tarifa actual en la página del espacio de nombres](./media/change-pricing-tier/pricing-tier-before.png)
1. En la página **Espacio de nombres del centro de notificaciones** correspondiente a su espacio de nombres, seleccione **Plan de tarifa** en la sección **Administrar**. 

    ![Selección del plan de tarifa en la página del espacio de nombres](./media/change-pricing-tier/namespace-select-pricing-menu.png)
6. Cambie el plan de tarifa y haga clic en el botón **Seleccionar**.    
7. Puede ver el estado de la acción de cambio de nivel en las **alertas**. 
8. Cámbiese a la página **Información general**. Confirme que se muestra el nivel nuevo para el campo **Plan de tarifa** en la sección **Essentials**.     
1. Este paso es opcional. Seleccione cualquier centro del espacio de nombres. Confirme que ve el mismo plan de tarifa en la sección **Essentials**. Debería ver el mismo plan de tarifa para todos los centros del espacio de nombres. 

### <a name="change-tier-on-the-hub-page"></a>Cambio del nivel en la página del centro
El procedimiento siguiente le da los pasos para cambiar el plan de tarifa de un espacio de nombres en la página del centro. Aunque lleve a cabo estos pasos desde la página del centro, en realidad cambia el plan de tarifa del espacio de nombres y todos los centros en el espacio de nombres. 

1. Inicie sesión en [Azure Portal](https://portal.azure.com).
2. Seleccione **Todos los servicios** en el menú de la izquierda.
3. Seleccione **Notification Hubs** en la sección **Internet de las cosas**. 
4. Seleccione su **centro** de notificaciones. 
5. Seleccione **Plan de tarifa** en el menú de la izquierda. 
6. Cambie el plan de tarifa y haga clic en el botón **Seleccionar**. Esta acción cambia la configuración del plan de tarifa para el espacio de nombres que contiene el centro. De este modo, verá el plan de tarifa nuevo en la página del espacio de nombres y en todas las páginas de los centros. 

## <a name="use-rest-api"></a>Use la API de REST
Puede usar las API REST de proveedor de recursos siguientes para obtener el plan de tarifa actual y actualizarlo. 

### <a name="get-current-pricing-tier-for-a-namespace"></a>Obtención del plan de tarifa actual para un espacio de nombres
Para obtener el **nivel de espacio de nombres actual**, envíe un comando GET como se muestra en el ejemplo siguiente: 

```REST
GET: https://management.core.windows.net/{subscription ID}/services/ServiceBus/Namespaces/{namespace name}/notificationhubplan
```

### <a name="update-pricing-tier-for-a-namespace"></a>Actualización del plan de tarifa de un espacio de nombres
Para **actualizar el nivel del espacio de nombres**, envíe un comando PUT como se muestra en el ejemplo siguiente: 

```REST
PUT: https://management.core.windows.net/{subscription ID}/services/ServiceBus/Namespaces/{namespace name}/notificationhubplan
Body: <NotificationHubPlan xmlns:i="https://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/netservices/2010/10/servicebus/connect"><SKU>Standard</SKU></NotificationHubPlan>
```



## <a name="next-steps"></a>Pasos siguientes
Para más información sobre estos niveles y precios, consulte [Precios de Notification Hubs](https://azure.microsoft.com/pricing/details/notification-hubs/).

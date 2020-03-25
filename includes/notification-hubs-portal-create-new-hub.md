---
title: archivo de inclusión
description: archivo de inclusión
services: notification-hubs
author: jwargo
ms.service: notification-hubs
ms.topic: include
ms.date: 01/17/2019
ms.author: jowargo
ms.custom: include file
ms.openlocfilehash: 5afcc8e4524a0e8353766ba239d5ab9161b29d86
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/24/2020
ms.locfileid: "67509106"
---
1. Inicie sesión en [Azure Portal](https://portal.azure.com).

1. Seleccione **Todos los servicios** en el menú de la izquierda y seleccione **Notification Hubs** en la sección **Móvil**. Seleccione el icono de estrella junto al nombre del servicio para agregarlo a la sección **FAVORITOS** en el menú de la izquierda. Cuando se agrega **Notification Hubs** a **FAVORITOS**, selecciónelo en el menú de la izquierda.

      ![Azure Portal: selección de Notification Hubs](./media/notification-hubs-portal-create-new-hub/all-services-select-notification-hubs.png)

1. En la página **Notification Hubs**, seleccione **Agregar** en la barra de herramientas.

      ![Notification Hubs: botón Agregar de la barra de herramientas](./media/notification-hubs-portal-create-new-hub/add-toolbar-button.png)

1. En la página **Notification Hubs**, realice los pasos siguientes:

    1. Escriba un nombre en el **centro de notificaciones**.  

    1. Escriba un nombre en **Crear un nuevo espacio de nombres**. Un espacio de nombres contiene uno o más centros.

    1. Seleccione un valor en el cuadro de lista desplegable **Ubicación**. Este valor especifica la ubicación en la que quiere crear el centro.

    1. Seleccione un grupo de recursos existente en **Grupo de recursos** o cree un nombre para el nuevo grupo.

    1. Seleccione **Crear**.

        ![Azure Portal: establecimiento de las propiedades del centro de notificaciones](./media/notification-hubs-portal-create-new-hub/notification-hubs-azure-portal-settings.png)

1. Seleccione **Notificaciones** (el icono de campana) y, luego, **Ir al recurso**. También puede actualizar la lista en la página **Notification Hubs** y seleccionar su centro.

      ![Azure Portal: notificaciones -> Go to resource (Ir al recurso)](./media/notification-hubs-portal-create-new-hub/go-to-notification-hub.png)

1. Seleccione **Directivas de acceso** en la lista. Observe que las dos cadenas de conexión están disponibles. Las necesitará más adelante para gestionar las notificaciones push.

      >[!IMPORTANT]
      >*No* use la directiva **DefaultFullSharedAccessSignature** en la aplicación. Está pensada para usarse solamente en el back-end.
      >

      ![Azure Portal: cadenas de conexión del centro de notificaciones](./media/notification-hubs-portal-create-new-hub/notification-hubs-connection-strings-portal.png)

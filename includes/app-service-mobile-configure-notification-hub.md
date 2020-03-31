---
author: conceptdev
ms.author: crdun
ms.service: app-service-mobile
ms.topic: include
ms.date: 08/23/2018
ms.openlocfilehash: da3793c428c624ce3a224cbd7606ab26c4a50803
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "67186818"
---
La característica Mobile Apps de Azure App Service usa [Centros de notificaciones de Azure] para enviar notificaciones push; por lo tanto, necesita un centro de notificaciones para su aplicación móvil.

1. En [Azure Portal], vaya a **App Services** y seleccione el back-end de la aplicación. En **Configuración**, seleccione **Insertar**.
2. Seleccione **Conectar** para agregar un recurso de Centro de notificaciones a la aplicación. Puede crear un centro o conectarse a uno existente.

    ![Configurar un centro](./media/app-service-mobile-create-notification-hub/configure-hub-flow.png)

Ya ha conectado un centro de notificaciones al proyecto de back-end de Mobile Apps. Más adelante configure este centro de notificaciones para que se conecte a un sistema de notificación de plataforma (PNS) que envíe notificaciones push a los dispositivos.

[Azure Portal]: https://portal.azure.com/
[Centros de notificaciones de Azure]: https://azure.microsoft.com/documentation/articles/notification-hubs-push-notification-overview/

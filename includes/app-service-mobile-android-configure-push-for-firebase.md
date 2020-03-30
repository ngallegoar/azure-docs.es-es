---
author: conceptdev
ms.author: crdun
ms.service: app-service-mobile
ms.topic: include
ms.date: 08/23/2018
ms.openlocfilehash: 51a75ee7bf87c38e3916bdbc8d85abcfb14dca8b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "67186820"
---
1. En [Azure Portal](https://portal.azure.com/), haga clic en **Examinar todo** > **App Services** y finalmente en el back-end de Mobile Apps. En **Configuración**, haga clic en **App Service Push** y, después, en el nombre del centro de notificaciones.
2. Vaya a **Google (GCM)** , escriba el valor de **Clave de servidor** que obtuvo de Firebase en el procedimiento anterior y, después, haga clic en **Guardar**.

    ![Establezca la clave de API en el portal](./media/app-service-mobile-android-configure-push/mobile-push-api-key.png)

El back-end de Mobile Apps ahora está configurado para usar Firebase Cloud Messaging. Esto permite enviar notificaciones push a la aplicación que se ejecuta en un dispositivo Android mediante el centro de notificaciones.

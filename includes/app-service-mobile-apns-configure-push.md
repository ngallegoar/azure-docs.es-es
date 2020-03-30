---
author: conceptdev
ms.author: crdun
ms.service: app-service-mobile
ms.topic: include
ms.date: 08/23/2018
ms.openlocfilehash: 852a3b00e8513f9ce68c5aae54c974505d0c9af6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "67186813"
---
1. En el equipo Mac, inicie **Acceso a llaves**. En la barra de navegación izquierda, en **Categoría**, abra **Mis certificados**. Busque el certificado SSL que descargó en la sección anterior y muestre su contenido. Seleccione solo el certificado (no seleccione la clave privada). A continuación, [expórtelo](https://support.apple.com/kb/PH20122?locale=en_US).
2. En [Azure Portal](https://portal.azure.com/), seleccione **Examinar todo** > **App Services**. A continuación, seleccione el back-end de Mobile Apps. 
3. En **Configuración**, seleccione **App Service Push**. A continuación, seleccione el nombre del centro de notificaciones. 
4. Vaya a **Apple Push Notification Service** > **Cargar certificado**. Cargue el archivo .p12 y seleccione el **Modo** correcto (según si el certificado SSL de cliente es para un espacio aislado o para producción). Guarde los cambios.

El servicio ahora está configurado para trabajar con las notificaciones push en iOS.

[1]: ./media/app-service-mobile-apns-configure-push/mobile-push-notification-hub.png

---
author: mikeparker104
ms.author: miparker
ms.date: 06/02/2020
ms.service: notification-hubs
ms.topic: include
ms.openlocfilehash: 98c9d83424e7d347bb6e1166a3b00304d39e0cce
ms.sourcegitcommit: 971a3a63cf7da95f19808964ea9a2ccb60990f64
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/19/2020
ms.locfileid: "85095452"
---
Se usa un backend de [API web de ASP.NET Core](https://dotnet.microsoft.com/apps/aspnet/apis) para controlar el [registro del dispositivo](https://docs.microsoft.com/azure/notification-hubs/notification-hubs-push-notification-registration-management#what-is-device-registration) del cliente mediante el enfoque de [instalación](https://docs.microsoft.com/azure/notification-hubs/notification-hubs-push-notification-registration-management#installations) más reciente y más adecuado. El servicio también enviará notificaciones push en modo multiplataforma. 

Estas operaciones se administran mediante el [SDK de Notification Hubs para operaciones de back-end](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/). Se proporcionan más detalles sobre el enfoque global en la documentación [Administración de registros desde un back-end](https://docs.microsoft.com/azure/notification-hubs/notification-hubs-push-notification-registration-management#registration-management-from-a-backend).

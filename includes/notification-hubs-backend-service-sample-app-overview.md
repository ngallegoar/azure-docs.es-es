---
author: mikeparker104
ms.author: miparker
ms.date: 06/02/2020
ms.service: notification-hubs
ms.topic: include
ms.openlocfilehash: 5c3277394350036b8863185e83b7cfcd4975be1c
ms.sourcegitcommit: 971a3a63cf7da95f19808964ea9a2ccb60990f64
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/19/2020
ms.locfileid: "85095462"
---
Le permite registrar y anular el registro de un centro de notificaciones a través del servicio back-end que ha creado. 

Cuando se especifica una acción y la aplicación está en primer plano se muestra una alerta. De lo contrario, las notificaciones aparecen en el centro de notificaciones.

> [!NOTE]
> Normalmente, realizará las acciones de registro (y de anulación de registro) durante el momento adecuado del ciclo de vida de la aplicación (o quizás como parte de la experiencia de primera ejecución) sin entradas explícitas de registro o anulación de registro del usuario. De todas formas, en este ejemplo se requerirá la intervención explícita del usuario para permitir que esta funcionalidad se pueda explorar y probar más fácilmente.

---
title: Configuración de Google Firebase Cloud Messaging en Azure Notification Hubs | Microsoft Docs
description: Aprenda a configurar un centro de notificaciones de Azure con la configuración de Google Firebase Cloud Messaging.
services: notification-hubs
author: sethmanheim
manager: femila
editor: jwargo
ms.service: notification-hubs
ms.workload: mobile
ms.topic: article
ms.date: 03/25/2019
ms.author: sethm
ms.reviewer: jowargo
ms.lastreviewed: 03/25/2019
ms.openlocfilehash: 1adbce654bc5c057270df9a874911731a0135034
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "80127466"
---
# <a name="configure-google-firebase-settings-for-a-notification-hub-in-the-azure-portal"></a>Configuración de valores de Google Firebase para un centro de notificaciones en Azure Portal

En este artículo se muestra cómo establecer la configuración de Google Firebase Cloud Messaging (FCM) para un centro de notificaciones de Azure mediante Azure Portal.  

## <a name="prerequisites"></a>Prerrequisitos
Si aún no ha creado un centro de notificaciones, cree uno ahora. Para más información, consulte [Creación de un centro de notificaciones de Azure en Azure Portal](create-notification-hub-portal.md). 

## <a name="configure-google-firebase-cloud-messaging-fcm"></a>Configuración de Google Firebase Cloud Messaging (FCM)

El siguiente procedimiento le proporciona los pasos para establecer la configuración de Google Firebase Cloud Messaging (FCM) para un centro de notificaciones: 

1. En Azure Portal, en la página **Centro de notificaciones**, seleccione **Google (GCM/FCM)** en el menú izquierdo. 
2. Pegue la **clave de API** para el proyecto FCM que ha guardado anteriormente. 
3. Seleccione **Guardar**. 

   ![Captura de pantalla que muestra cómo configurar Notification Hubs para Google FCM](./media/notification-hubs-android-push-notification-google-fcm-get-started/fcm-server-key.png)

## <a name="next-steps"></a>Pasos siguientes
Para ver un tutorial con instrucciones paso a paso para insertar notificaciones en dispositivos Android mediante Azure Notification Hubs y Google Firebase Cloud Messaging, consulte [Tutorial: Envío de notificaciones push a dispositivos Android con Azure Notification Hubs y Google Firebase Cloud Messaging](notification-hubs-android-push-notification-google-fcm-get-started.md).


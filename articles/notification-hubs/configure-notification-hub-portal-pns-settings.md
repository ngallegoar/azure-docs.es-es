---
title: Configuración de notificaciones push en Azure Notification Hubs | Microsoft Docs
description: Aprenda a configurar Azure Notification Hubs en Azure Portal mediante la configuración del sistema de notificación de plataforma (PNS).
services: notification-hubs
author: sethmanheim
manager: femila
editor: jwargo
ms.service: notification-hubs
ms.workload: mobile
ms.topic: quickstart
ms.date: 02/14/2019
ms.author: sethm
ms.reviewer: jowargo
ms.lastreviewed: 02/14/2019
ms.openlocfilehash: 951f03f581906e45946ef75742421ba27d405267
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/26/2020
ms.locfileid: "74406967"
---
# <a name="set-up-push-notifications-in-a-notification-hub-in-the-azure-portal"></a>Configuración de notificaciones push en un centro de notificaciones en Azure Portal

Azure Notification Hubs proporciona un motor de inserción que es fácil de usar y que se escala horizontalmente. Use Notification Hubs para enviar notificaciones a cualquier plataforma (iOS, Android, Windows, Baidu), desde cualquier back-end (en la nube o local). Para más información, consulte [¿Qué es Azure Notification Hubs?](notification-hubs-push-notification-overview.md).

En este inicio rápido, se usará la configuración del sistema de notificación de plataforma (PNS) en Notification Hubs para configurar las notificaciones push en varias plataformas. El inicio rápido muestra los pasos a seguir en Azure Portal.

Si aún no ha creado un centro de notificaciones, cree uno ahora. Para más información, consulte [Creación de un centro de notificaciones de Azure en Azure Portal](create-notification-hub-portal.md). 

## <a name="apple-push-notification-service"></a>Apple Push Notification Service

Para configurar Apple Push Notification Service (APNS):

1. En Azure Portal, en la página **Centro de notificaciones**, seleccione **Apple (APNS)** en el menú izquierdo.

1. Para **Modo de autenticación**, seleccione **Certificado** o **Token**.

   a. Si ha seleccionado **Certificado**:
   * Seleccione el icono de archivo, y luego el archivo *.p12* que desea cargar.
   * Escriba una contraseña.
   * Seleccione el modo **Espacio aislado**. O para enviar notificaciones push a los usuarios que compraron la aplicación en la tienda, seleccione el modo **Producción**.

     ![Captura de pantalla de una configuración del certificado APNS en Azure Portal](./media/notification-hubs-ios-get-started/notification-hubs-apple-config-cert.png)

   b. Si selecciona **Token**:

   * Especifique los valores **Id. de clave**, **Id. de agrupación**, **Id. de equipo** y **Token**.
   * Seleccione el modo **Espacio aislado**. O para enviar notificaciones push a los usuarios que compraron la aplicación en la tienda, seleccione el modo **Producción**.

     ![Captura de pantalla de una configuración del token APNS en Azure Portal](./media/configure-notification-hub-portal-pns-settings/notification-hubs-apple-config-token.png)

Para obtener más información, consulte [Envío de notificaciones push a aplicaciones iOS mediante Azure Notification Hubs](notification-hubs-ios-apple-push-notification-apns-get-started.md).

## <a name="google-firebase-cloud-messaging"></a>Google Firebase Cloud Messaging

Para configurar las notificaciones push para Google Firebase Cloud Messaging (FCM):

1. En Azure Portal, en la página **Centro de notificaciones**, seleccione **Google (GCM/FCM)** en el menú izquierdo. 
2. Pegue la **clave de API** para el proyecto FCM que ha guardado anteriormente. 
3. Seleccione **Guardar**. 

   ![Captura de pantalla que muestra cómo configurar Notification Hubs para Google FCM](./media/notification-hubs-android-push-notification-google-fcm-get-started/fcm-server-key.png)

Cuando haya completado estos pasos, una alerta le indica que el centro de notificaciones se ha actualizado correctamente. El botón **Save** (Guardar) está deshabilitado. 

Para más información consulte [Envío de notificaciones push a dispositivos Android con Notification Hubs y Google FCM](notification-hubs-android-push-notification-google-fcm-get-started.md).

## <a name="windows-push-notification-service"></a>Servicios de notificaciones de inserción de Windows

Para configurar Servicios de notificaciones de inserción de Windows (WNS):

1. En Azure Portal, en la página **Centro de notificaciones**, seleccione **Windows (WNS)** en el menú izquierdo.
2. Especifique los valores de **SID de paquete** y **Clave de seguridad**.
3. Seleccione **Guardar**.

   ![Captura de pantalla que muestra los cuadros de SID del paquete y Clave de seguridad](./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-configure-wns.png)

Para más información, consulte [Envío de notificaciones a aplicaciones de Plataforma universal de Windows mediante Azure Notification Hubs](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md).

## <a name="microsoft-push-notification-service-for-windows-phone"></a>Servicio de notificaciones push de Microsoft para Windows Phone

Para configurar el Servicio de notificaciones push de Microsoft (MPNS) para Windows Phone: 

1. En Azure Portal, en la página **Centro de notificaciones**, seleccione **Windows Phone (MPNS)** en el menú izquierdo.
1. Habilite las notificaciones push sin autenticar o autenticadas:

   a. Para habilitar las notificaciones push no autenticadas, seleccione **Habilitar notificaciones de inserción sin autenticar** > **Guardar**.

      ![Captura de pantalla que muestra cómo habilitar las notificaciones push no autenticadas](./media/notification-hubs-windows-phone-get-started/azure-portal-unauth.png)

   b. Para habilitar las notificaciones push no autenticadas:
      * Seleccione **Cargar certificado** en la barra de herramientas.
      * Seleccione el icono de archivo y el archivo de certificado.
      * Escriba la contraseña del certificado.
      * Seleccione **Aceptar**.
      * En la página **Windows Phone (MPNS)** , seleccione **Guardar**.

Para más información, consulte [Notificaciones push a aplicaciones de Windows Phone mediante Notification Hubs](notification-hubs-windows-mobile-push-notifications-mpns.md).
      

## <a name="baidu-android-china"></a>Baidu (Android China)

Para configurar las notificaciones push para Baidu:

1. En Azure Portal, en la página **Centro de notificaciones**, seleccione **Baidu (Android China)** en el menú izquierdo. 
2. Escriba la **clave de API** que ha obtenido en la consola de Baidu, en el proyecto Baidu Cloud Push. 
3. Escriba la **clave secreta** que ha obtenido en la consola de Baidu, en el proyecto Baidu Cloud Push. 
4. Seleccione **Guardar**. 

    ![Captura de pantalla de Notification Hubs que muestra la configuración de Baidu (Android China) para las notificaciones push](./media/notification-hubs-baidu-get-started/AzureNotificationServicesBaidu.png)

Cuando haya completado estos pasos, una alerta le indica que el centro de notificaciones se ha actualizado correctamente. El botón **Save** (Guardar) está deshabilitado. 

Para más información, consulte [Introducción a Notification Hubs con Baidu](notification-hubs-baidu-china-android-notifications-get-started.md).

## <a name="next-steps"></a>Pasos siguientes
En este inicio rápido, ha aprendido cómo configurar los valores para los sistemas de notificaciones de distintas plataformas para un centro de notificaciones en Azure Portal. 

Para más información acerca de cómo enviar notificaciones push a varias plataformas, consulte estos tutoriales:

- [Envío de notificaciones push a aplicaciones iOS mediante Notification Hubs y APNS](notification-hubs-ios-apple-push-notification-apns-get-started.md)
- [Envío de notificaciones push a dispositivos Android con Notification Hubs y Google FCM](notification-hubs-android-push-notification-google-fcm-get-started.md)
- [Envío de notificaciones push a una aplicación de UWP en un dispositivo Windows](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md)
- [Envío de notificaciones push a una aplicación Windows Phone 8 mediante MPNS](notification-hubs-windows-mobile-push-notifications-mpns.md)
- [Envío de notificaciones push mediante Notification Hubs y Baidu Cloud Push](notification-hubs-baidu-china-android-notifications-get-started.md)

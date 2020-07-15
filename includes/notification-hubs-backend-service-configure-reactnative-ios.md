---
author: alexeystrakh
ms.author: alstrakh
ms.date: 06/11/2020
ms.service: notification-hubs
ms.topic: include
ms.openlocfilehash: e67f53e26f2081e1aa12f7c0e6a219c240f5027a
ms.sourcegitcommit: bcb962e74ee5302d0b9242b1ee006f769a94cfb8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/07/2020
ms.locfileid: "86060468"
---
### <a name="configure-required-ios-packages"></a>Configuración de los paquetes iOS necesarios

El paquete se [vincula automáticamente](https://github.com/react-native-community/cli/blob/master/docs/autolinking.md) al compilar la aplicación. Lo único que debe hacer es instalar los pods nativos:

```bash
npx pod-install
```

### <a name="configure-infoplist-and-entitlementsplist"></a>Configuración de Info.plist y Entitlements.plist

1. Vaya a la carpeta "PushDemo/ios" y abra el área de trabajo "PushDemo.xcworkspace", seleccione el proyecto superior "PushDemo" y la pestaña "Signing & Capabilities" (Firma y funcionalidades).

1. Actualice el identificador de lote para que coincida con el valor usado en el perfil de aprovisionamiento.

1. Agregue dos nuevas capacidades con el botón "+":

    - Funcionalidad de modo en segundo plano y marque Remote Notifications (Notificaciones remotas).
    - Funcionalidad de notificaciones push.

### <a name="handle-push-notifications-for-ios"></a>Administración de las notificaciones push para iOS

1. Abra "AppDelegate.h" y agregue la siguiente importación:

    ```objective-c
    #import <UserNotifications/UNUserNotificationCenter.h>
    ```

1. Actualice la lista de protocolos admitidos por "AppDelegate" agregando `UNUserNotificationCenterDelegate`:

    ```objective-c
    @interface AppDelegate : UIResponder <UIApplicationDelegate, RCTBridgeDelegate, UNUserNotificationCenterDelegate>
    ```

1. Abra "AppDelegate.m" y configure todas las devoluciones de llamada de iOS necesarias:

    ```objective-c
    #import <UserNotifications/UserNotifications.h>
    #import <RNCPushNotificationIOS.h>

    ...

    // Required to register for notifications
    - (void)application:(UIApplication *)application didRegisterUserNotificationSettings:(UIUserNotificationSettings *)notificationSettings
    {
     [RNCPushNotificationIOS didRegisterUserNotificationSettings:notificationSettings];
    }

    // Required for the register event.
    - (void)application:(UIApplication *)application didRegisterForRemoteNotificationsWithDeviceToken:(NSData *)deviceToken
    {
     [RNCPushNotificationIOS didRegisterForRemoteNotificationsWithDeviceToken:deviceToken];
    }

    // Required for the notification event. You must call the completion handler after handling the remote notification.
    - (void)application:(UIApplication *)application didReceiveRemoteNotification:(NSDictionary *)userInfo
    fetchCompletionHandler:(void (^)(UIBackgroundFetchResult))completionHandler
    {
      [RNCPushNotificationIOS didReceiveRemoteNotification:userInfo fetchCompletionHandler:completionHandler];
    }

    // Required for the registrationError event.
    - (void)application:(UIApplication *)application didFailToRegisterForRemoteNotificationsWithError:(NSError *)error
    {
     [RNCPushNotificationIOS didFailToRegisterForRemoteNotificationsWithError:error];
    }

    // IOS 10+ Required for localNotification event
    - (void)userNotificationCenter:(UNUserNotificationCenter *)center
    didReceiveNotificationResponse:(UNNotificationResponse *)response
             withCompletionHandler:(void (^)(void))completionHandler
    {
      [RNCPushNotificationIOS didReceiveNotificationResponse:response];
      completionHandler();
    }

    // IOS 4-10 Required for the localNotification event.
    - (void)application:(UIApplication *)application didReceiveLocalNotification:(UILocalNotification *)notification
    {
     [RNCPushNotificationIOS didReceiveLocalNotification:notification];
    }

    //Called when a notification is delivered to a foreground app.
    -(void)userNotificationCenter:(UNUserNotificationCenter *)center willPresentNotification:(UNNotification *)notification withCompletionHandler:(void (^)(UNNotificationPresentationOptions options))completionHandler
    {
      completionHandler(UNAuthorizationOptionSound | UNAuthorizationOptionAlert | UNAuthorizationOptionBadge);
    }
    ```

---
title: Incorporación de notificaciones push a iOS
description: Obtenga información acerca de cómo usar las Azure Mobile Apps para enviar notificaciones push a su aplicación iOS.
ms.assetid: fa503833-d23e-4925-8d93-341bb3fbab7d
ms.tgt_pltfrm: mobile-ios
ms.devlang: objective-c
ms.topic: article
ms.date: 06/25/2019
ms.openlocfilehash: ad58ee158a2e1842c1b41db281bdd9f04b9b2ca4
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "77461511"
---
# <a name="add-push-notifications-to-your-ios-app"></a>Incorporación de notificaciones push a la aplicación iOS

[!INCLUDE [app-service-mobile-selector-get-started-push](../../includes/app-service-mobile-selector-get-started-push.md)]

## <a name="overview"></a>Información general

En este tutorial, se agregan notificaciones push al proyecto de [inicio rápido de iOS] para que, cada vez que se envíe una notificación de inserción al dispositivo, se inserte un registro.

Si no usa el proyecto de servidor de inicio rápido descargado, necesitará el paquete de extensión de notificaciones push. Para más información, consulte la guía [Trabajar con el SDK del servidor back-end de .NET para Azure Mobile Apps](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).

El [simulador de iOS no admite notificaciones de inserción](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/iOS_Simulator_Guide/TestingontheiOSSimulator.html). Necesita un dispositivo de iOS físico y una [suscripción de Apple Developer Program](https://developer.apple.com/programs/ios/).

## <a name="configure-notification-hub"></a><a name="configure-hub"></a>Configurar el Centro de notificaciones

[!INCLUDE [app-service-mobile-configure-notification-hub](../../includes/app-service-mobile-configure-notification-hub.md)]

## <a name="register-app-for-push-notifications"></a><a id="register"></a>Registro de aplicaciones para notificaciones push

[!INCLUDE [Enable Apple Push Notifications](../../includes/enable-apple-push-notifications.md)]

## <a name="configure-azure-to-send-push-notifications"></a>Configuración de Azure para enviar notificaciones push

[!INCLUDE [app-service-mobile-apns-configure-push](../../includes/app-service-mobile-apns-configure-push.md)]

## <a name="update-backend-to-send-push-notifications"></a><a id="update-server"></a>Actualizar el back-end para enviar notificaciones de inserción

[!INCLUDE [app-service-mobile-dotnet-backend-configure-push-apns](../../includes/app-service-mobile-dotnet-backend-configure-push-apns.md)]

## <a name="add-push-notifications-to-app"></a><a id="add-push"></a>Agregar notificaciones push a la aplicación

[!INCLUDE [app-service-mobile-add-push-notifications-to-ios-app.md](../../includes/app-service-mobile-add-push-notifications-to-ios-app.md)]

## <a name="test-push-notifications"></a><a id="test"></a>Probar notificaciones de inserción

[!INCLUDE [Test Push Notifications in App](../../includes/test-push-notifications-in-app.md)]

## <a name="more"></a><a id="more"></a>Más

* : las plantillas proporcionan flexibilidad para enviar inserciones multiplataforma e inserciones localizadas. [Uso de la biblioteca de cliente de iOS para Azure Mobile Apps](app-service-mobile-ios-how-to-use-client-library.md#templates) muestra cómo registrar plantillas.

<!-- Anchors.  -->

<!-- Images. -->

<!-- URLs. -->
[Inicio rápido de iOS]: app-service-mobile-ios-get-started.md

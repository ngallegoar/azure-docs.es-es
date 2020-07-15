---
title: Envío de notificaciones push a aplicaciones React Native con Azure Notification Hubs mediante un servicio back-end | Microsoft Docs
description: Aprenda a enviar notificaciones push a aplicaciones React Native que usan Azure Notification Hubs mediante un servicio back-end.
author: alexeystrakh
ms.service: notification-hubs
ms.topic: tutorial
ms.date: 06/11/2020
ms.author: alstrakh
ms.openlocfilehash: 30cf22d99a675d4525d972aeb889cec30a2e9b94
ms.sourcegitcommit: bcb962e74ee5302d0b9242b1ee006f769a94cfb8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/07/2020
ms.locfileid: "86060473"
---
# <a name="tutorial-send-push-notifications-to-react-native-apps-using-azure-notification-hubs-via-a-backend-service"></a>Tutorial: Envío de notificaciones push a aplicaciones React Native con Azure Notification Hubs mediante un servicio back-end  

[![Descargar el ejemplo](./media/notification-hubs-backend-service-react-native/download.png) Descargar el ejemplo](https://github.com/xamcat/mobcat-samples/tree/master/notification_hub_backend_service)  

> [!div class="op_single_selector"]
>
> * [Xamarin.Forms](notification-hubs-backend-service-xamarin-forms.md)
> * [React Native](notification-hubs-backend-service-react-native.md)

En este tutorial, usará [Azure Notification Hubs](https://docs.microsoft.com/azure/notification-hubs/notification-hubs-push-notification-overview) para enviar notificaciones push a una aplicación [React Native](https://reactnative.dev/) con destino **Android** e **iOS**.  

[!INCLUDE [Notification Hubs Backend Service Introduction](../../includes/notification-hubs-backend-service-introduction.md)]

Este tutorial le guía por los siguientes pasos:

> [!div class="checklist"]
>
> * [Configurar servicios de notificación push y Azure Notification Hubs](#set-up-push-notification-services-and-azure-notification-hub)
> * [Crear una aplicación de back-end de ASP.NET Core Web API](#create-an-aspnet-core-web-api-backend-application)
> * [Crear una aplicación React Native multiplataforma](#create-a-cross-platform-react-native-application)
> * [Configurar el proyecto de Android nativo para las notificaciones push](#configure-the-native-android-project-for-push-notifications)
> * [Configurar el proyecto de iOS nativo para las notificaciones push](#configure-the-native-ios-project-for-push-notifications)
> * [Probar la solución](#test-the-solution)

## <a name="prerequisites"></a>Requisitos previos

Para continuar, necesita lo siguiente:

* Una [suscripción a Azure](https://portal.azure.com) donde pueda crear y administrar los recursos.
* Un equipo Mac con [Visual Studio para Mac](https://visualstudio.microsoft.com/vs/mac/) instalado (o un equipo que ejecute [Visual Studio 2019](https://visualstudio.microsoft.com/vs) con la carga de trabajo **Desarrollo para dispositivos móviles con .NET**).
* La capacidad de ejecutar la aplicación en **Android** (dispositivos físicos o emuladores) o **iOS** (solo en dispositivos físicos).

En el caso de Android, debe tener lo siguiente:

* Un dispositivo físico desbloqueado para desarrolladores o un emulador *(que ejecute la API 26 y versiones posteriores con Google Play Services instalado)* .

En el caso de iOS, debe tener lo siguiente:

* Una [cuenta de desarrollador de Apple](https://developer.apple.com) activa.
* Un dispositivo iOS físico que esté [registrado en la cuenta de desarrollador](https://help.apple.com/developer-account/#/dev40df0d9fa) *(que ejecute iOS 13.0 y versiones posteriores)* .
* Un [certificado de desarrollo](https://help.apple.com/developer-account/#/dev04fd06d56) **.p12** instalado en el **llavero** que permita [ejecutar una aplicación en un dispositivo físico](https://help.apple.com/xcode/mac/current/#/dev5a825a1ca).

> [!NOTE]
> El simulador de iOS no admite notificaciones remotas y, por tanto, se necesita un dispositivo físico para aplicar este ejemplo en iOS. Sin embargo, no es necesario ejecutar la aplicación tanto en **Android** como en **iOS** para completar este tutorial.

Puede seguir los pasos de este ejemplo de primeros principios sin experiencia previa. No obstante, resulta beneficioso estar familiarizado con los aspectos siguientes.

* [Portal para desarrolladores de Apple](https://developer.apple.com)
* [ASP.NET Core](https://docs.microsoft.com/aspnet/core/introduction-to-aspnet-core?view=aspnetcore-3.1)
* [Consola de Google Firebase](https://console.firebase.google.com/u/0/)
* [Microsoft Azure](https://portal.azure.com) y [Envío de notificaciones push a aplicaciones iOS mediante Azure Notification Hubs](ios-sdk-get-started.md)
* [React Native](https://reactnative.dev/docs/getting-started).

Los pasos proporcionados corresponden a [Visual Studio para Mac](https://visualstudio.microsoft.com/vs/mac/) y [Visual Studio Code](https://code.visualstudio.com/download), pero es posible seguirlos con [Visual Studio 2019](https://visualstudio.microsoft.com/vs).

## <a name="set-up-push-notification-services-and-azure-notification-hub"></a>Configuración de servicios de notificación push y Azure Notification Hubs

En esta sección, configurará **[Firebase Cloud Messaging (FCM)](https://firebase.google.com/docs/cloud-messaging)** y **[Apple Push Notification Service (APNs)](https://developer.apple.com/library/archive/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/APNSOverview.html)** . Después, creará y configurará un centro de notificaciones para que funcione con esos servicios.

[!INCLUDE [Create a Firebase project and enable Firebase Cloud Messaging](../../includes/notification-hubs-common-enable-firebase-cloud-messaging.md)]

[!INCLUDE [Enable Apple Push Notifications](../../includes/notification-hubs-common-enable-apple-push-notifications.md)]

[!INCLUDE [Create a notification hub](../../includes/notification-hubs-common-create-notification-hub.md)]

[!INCLUDE [Configure your notification hub with APNs information](../../includes/notification-hubs-common-configure-with-apns-information.md)]

[!INCLUDE [Configure your notification hub with FCM information](../../includes/notification-hubs-common-configure-with-fcm-information.md)]

## <a name="create-an-aspnet-core-web-api-backend-application"></a>Creación de una aplicación de back-end de ASP.NET Core Web API

En esta sección, creará la aplicación de back-end de [ASP.NET Core API Web](https://dotnet.microsoft.com/apps/aspnet/apis) para administrar el [registro de dispositivos](https://docs.microsoft.com/azure/notification-hubs/notification-hubs-push-notification-registration-management#what-is-device-registration) y el envío de notificaciones a la aplicación móvil React Native.

[!INCLUDE [Create an ASP.NET Core Web API backend application](../../includes/notification-hubs-backend-service-web-api.md)]

## <a name="create-a-cross-platform-react-native-application"></a>Creación de una aplicación React Native multiplataforma

En esta sección, creará una aplicación móvil [React Native](https://reactnative.dev/) que implemente notificaciones push entre plataformas.

[!INCLUDE [Sample application generic overview](../../includes/notification-hubs-backend-service-sample-app-overview.md)]

[!INCLUDE [Create React Native application](../../includes/notification-hubs-backend-service-sample-app-reactnative.md)]

## <a name="configure-the-native-android-project-for-push-notifications"></a>Configuración del proyecto de Android nativo para las notificaciones push

[!INCLUDE [Configure the native Android project](../../includes/notification-hubs-backend-service-configure-reactnative-android.md)]

## <a name="configure-the-native-ios-project-for-push-notifications"></a>Configuración del proyecto de iOS nativo para las notificaciones push

[!INCLUDE [Configure the native iOS project](../../includes/notification-hubs-backend-service-configure-reactnative-ios.md)]

## <a name="test-the-solution"></a>Probar la solución

Ahora puede probar el envío de notificaciones mediante el servicio back-end.

[!INCLUDE [Testing the solution](../../includes/notification-hubs-backend-service-testing.md)]

## <a name="next-steps"></a>Pasos siguientes

Ahora debe contar con una aplicación React Native básica conectada a un centro de notificaciones mediante un servicio back-end y ya puede enviar y recibir notificaciones.

[!INCLUDE [Next steps](../../includes/notification-hubs-backend-service-next-steps.md)]

## <a name="troubleshooting"></a>Solución de problemas

[!INCLUDE [Troubleshooting](../../includes/notification-hubs-backend-service-troubleshooting.md)]

## <a name="related-links"></a>Vínculos relacionados

* [Introducción a Azure Notification Hubs](notification-hubs-push-notification-overview.md)
* [Instalación de Visual Studio para Mac](https://docs.microsoft.com/visualstudio/mac/installation?view=vsmac-2019)
* [Instalación de Visual Studio Code](https://code.visualstudio.com/download)
* [Configuración del entorno de desarrollo de React Native](https://reactnative.dev/docs/environment-setup)
* [SDK de Notification Hubs para operaciones de back-end](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/)
* [SDK de Notification Hubs en GitHub](https://github.com/Azure/azure-notificationhubs)
* [Registro con back-end de aplicación](notification-hubs-ios-aspnet-register-user-from-backend-to-push-notification.md)
* [Administración de registros](notification-hubs-push-notification-registration-management.md)
* [Trabajo con etiquetas](notification-hubs-tags-segment-push-message.md)
* [Trabajo con plantillas personalizadas](notification-hubs-templates-cross-platform-push-messages.md)

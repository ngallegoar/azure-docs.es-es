---
title: Disponibilidad y limitaciones de Microsoft Authenticator para Android en China | Microsoft Docs
description: Obtenga información sobre cómo obtener la aplicación Microsoft Authenticator, disponible en China.
services: active-directory
author: curtand
manager: daveba
ms.reviewer: sahenry
ms.service: active-directory
ms.subservice: user-help
ms.workload: identity
ms.topic: end-user-help
ms.date: 05/20/2020
ms.author: curtand
ms.openlocfilehash: 795c68fc063d98bdee6ccf59dba6ee718dc92d03
ms.sourcegitcommit: 58ff2addf1ffa32d529ee9661bbef8fbae3cddec
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/03/2020
ms.locfileid: "84323032"
---
# <a name="microsoft-authenticator-for-android-in-the-public-cloud-in-china"></a>Microsoft Authenticator para Android en la nube pública en China

La aplicación Microsoft Authenticator para Android está disponible para su descarga en China. Google Play Store no está disponible en China, por lo que la aplicación se debe descargar desde otros marketplaces de aplicaciones chinos. La aplicación Microsoft Authenticator para Android está disponible actualmente en las siguientes tiendas en China:

- [Baidu](https://shouji.baidu.com/software/26638379.html)
- [Lenovo](https://www.lenovomm.com/appdetail/com.azure.authenticator/20197724)
- [Huawei](https://appgallery.cloud.huawei.com/uowap/index.html#/detailApp/C100262999?source=appshare&subsource=C100262999&shareTo=weixin&locale=zh_CN)
- [Samsung Galaxy Store](http://apps.samsung.com/appquery/appDetail.as?appId=com.azure.authenticator)

La compilación más reciente de la aplicación se encuentra en Google Play Store, pero estamos actualizando la aplicación en el resto de las tiendas de aplicaciones lo más rápido posible. Dado que no hay ningún paquete de aplicación Android (APK) personalizado implementado en ninguna tienda de aplicaciones, la aplicación se puede actualizar sin problemas desde una de las siguientes ubicaciones:

- El almacén desde el que se descargó.
- Google Play Store, si el usuario cruza regiones.

## <a name="limitations"></a>Limitaciones

La aplicación Microsoft Authenticator para Android usa el sistema de mensajería en la nube Firebase de Google y Google Play Services para recibir notificaciones push. Dado que ninguno de los servicios está disponible en China, existen algunas limitaciones en cuanto a las funcionalidades de la aplicación:

- El registro de la aplicación Authenticator como método de autenticación multifactor (MFA) mediante notificaciones push no funciona.

- No se puede configurar el [inicio de sesión por teléfono](../authentication/howto-authentication-sms-signin.md). Requiere que el usuario configure la aplicación como un método de MFA mediante notificaciones push, que actualmente no funcionan.

Si un usuario había conseguido configurar el inicio de sesión por teléfono o la autenticación multifactor mediante la aplicación, puede realizar una comprobación manual de las solicitudes de notificaciones en la aplicación y usarla para la comprobación de identidades.

## <a name="multi-factor-authentication-workaround"></a>Solución alternativa a la autenticación multifactor.

En lugar de usar notificaciones push para la autenticación multifactor, los usuarios pueden [configurar la aplicación Authenticator para recibir códigos de verificación](multi-factor-authentication-setup-auth-app.md#set-up-the-microsoft-authenticator-app-to-use-verification-codes) en su dispositivo, que pueden usar para MFA a fin de verificar su identidad. Estos códigos de verificación son válidos durante 30 segundos y, para usarlos, los administradores deben permitir que su inquilino realice la comprobación mediante códigos de verificación de contraseña de un solo uso y duración definida (TOTP).

## <a name="availability"></a>Disponibilidad

Característica de Microsoft Authenticator | Disponibilidad en China
------------------------------- | ---------------------
Registro de MFA mediante notificaciones push | No
Cuenta de MFA existente que comprueba la identidad mediante notificaciones push | No
Cuenta de MFA existente que realiza una comprobación manual de las notificaciones | Sí
Registro o autenticación de MFA solo mediante códigos de verificación y TOTP | Sí
Registro de inicio de sesión por teléfono | No
Inicio de sesión por teléfono existente mediante notificaciones push | No
Verificación del inicio de sesión por teléfono existente mediante una comprobación manual de las solicitudes de autenticación | Sí

## <a name="next-steps"></a>Pasos siguientes

- [Descarga e instalación de la aplicación Microsoft Authenticator](user-help-auth-app-download-install.md)

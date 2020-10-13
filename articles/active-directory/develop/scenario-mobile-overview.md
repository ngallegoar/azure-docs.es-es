---
title: Compilación de una aplicación móvil que llama a las API web | Azure
titleSuffix: Microsoft identity platform | Azure
description: 'Obtener información sobre cómo compilar una aplicación móvil que llama a las API web: información general'
services: active-directory
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 05/07/2019
ms.author: jmprieur
ms.reviewer: brandwe
ms.custom: aaddev, identityplatformtop40
ms.openlocfilehash: 70ddc2191cc06a1bede87adc5940e0f7dea63384
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/25/2020
ms.locfileid: "91257544"
---
# <a name="scenario-mobile-application-that-calls-web-apis"></a>Escenario: Una aplicación móvil que llama a las API web

Obtenga información sobre cómo compilar una aplicación móvil que llama a las API web.

## <a name="getting-started"></a>Introducción

Si aún no lo ha hecho, cree su primera aplicación siguiendo un inicio rápido:

- [Inicio rápido: Adquisición de un token y llamada a Microsoft Graph API desde una aplicación de Android](./quickstart-v2-android.md)
- [Inicio rápido: Adquisición de un token y llamada a Microsoft Graph API desde una aplicación de iOS](./quickstart-v2-ios.md)
- [Inicio rápido: Adquisición de un token y llamada a Microsoft Graph API desde una aplicación de Android y Xamarin iOS](https://github.com/Azure-Samples/active-directory-xamarin-native-v2) (GitHub)

## <a name="overview"></a>Información general

Para las aplicaciones móviles es esencial una experiencia de usuario personalizada y sin problemas.  La plataforma de identidad de Microsoft permite a los desarrolladores de aplicaciones móviles crear esa misma experiencia para usuarios de iOS y Android. La aplicación puede iniciar la sesión de los usuarios de Azure Active Directory (Azure AD), los usuarios de cuentas personales de Microsoft y los usuarios de Azure AD B2C. También puede adquirir tokens para llamar a una API web en su nombre. Para implementar estos flujos, se usa la biblioteca de autenticación de Microsoft (MSAL). MSAL implementa el estándar del sector [Flujo de código de autorización de OAuth 2.0](v2-oauth2-auth-code-flow.md).

![Aplicaciones de demonio](./media/scenarios/mobile-app.svg)

Consideraciones para las aplicaciones móviles:

- **La experiencia del usuario es clave**: permita a los usuarios ver el valor de la aplicación antes de solicitarles iniciar sesión. Solicite únicamente los permisos necesarios.
- **Admitir todas las configuraciones de usuario**: muchos usuarios empresariales móviles deben cumplir directivas de acceso condicional y de cumplimiento del dispositivo. Asegúrese de que admite estos escenarios claves.
- **Implementación del inicio de sesión único (SSO)** : con MSAL y la plataforma de identidad de Microsoft, puede habilitar el inicio de sesión único a través del explorador del dispositivo o Microsoft Authenticator (Portal de empresa de Intune en Android).
- **Implementación del modo de dispositivo compartido**: habilite la aplicación para usarla en escenarios de dispositivos compartidos, por ejemplo, hospitales, fabricación, comercio minorista y finanzas. [Obtenga más información sobre la compatibilidad del modo de dispositivo compartido](msal-shared-devices.md).

## <a name="specifics"></a>Características específicas

Tenga en cuenta las siguientes consideraciones al compilar una aplicación móvil en la plataforma de identidad de Microsoft:

- En función de la plataforma, podría ser necesaria alguna interacción del usuario la primera vez que inicia sesión. Por ejemplo, iOS requiere que las aplicaciones muestren interacción del usuario cuando usan el inicio de sesión único por primera vez a través de Microsoft Authenticator (y Portal de empresa de Intune en Android).
- En iOS y Android, MSAL podría usar un explorador externo para iniciar la sesión de los usuarios. El explorador externo podría aparecer en la parte superior de la aplicación.
- Nunca use un secreto en una aplicación móvil. En estas aplicaciones, los secretos son accesibles para todos los usuarios.

## <a name="recommended-reading"></a>Lecturas recomendadas

[!INCLUDE [recommended-topics](../../../includes/active-directory-develop-scenarios-prerequisites.md)]

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Registro de aplicaciones](scenario-mobile-app-registration.md)

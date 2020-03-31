---
title: Compilación de una aplicación móvil que llama a las API web | Azure
titleSuffix: Microsoft identity platform
description: 'Obtener información sobre cómo compilar una aplicación móvil que llama a las API web: información general'
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/07/2019
ms.author: jmprieur
ms.reviwer: brandwe
ms.custom: aaddev, identityplatformtop40
ms.openlocfilehash: 6675d67299091325fcc3e12572a906716bf5b88d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "77132417"
---
# <a name="scenario-mobile-application-that-calls-web-apis"></a>Escenario: Una aplicación móvil que llama a las API web

Obtenga información sobre cómo compilar una aplicación móvil que llama a las API web.

## <a name="prerequisites"></a>Prerrequisitos

[!INCLUDE [Prerequisites](../../../includes/active-directory-develop-scenarios-prerequisites.md)]

## <a name="getting-started"></a>Introducción

Cree su primera aplicación móvil y pruebe una guía de inicio rápido.

> [!div class="nextstepaction"]
> [Inicio rápido: Adquisición de un token y llamada a Microsoft Graph API desde una aplicación de Android](./quickstart-v2-android.md)
>
> [Inicio rápido: Adquisición de un token y llamada a Microsoft Graph API desde una aplicación de iOS](./quickstart-v2-ios.md)
>
> [Inicio rápido: Adquisición de un token y llamada a Microsoft Graph API desde una aplicación de Android y Xamarin iOS](https://github.com/Azure-Samples/active-directory-xamarin-native-v2)

## <a name="overview"></a>Información general

Para las aplicaciones móviles es esencial una experiencia de usuario personalizada y sin problemas.  La plataforma de identidad de Microsoft permite a los desarrolladores de aplicaciones móviles crear esa misma experiencia para usuarios de iOS y Android. La aplicación puede iniciar la sesión de los usuarios de Azure Active Directory (Azure AD), los usuarios de cuentas personales de Microsoft y los usuarios de Azure AD B2C. También puede adquirir tokens para llamar a una API web en su nombre. Para implementar estos flujos, se usa la biblioteca de autenticación de Microsoft (MSAL). MSAL implementa el estándar del sector [Flujo de código de autorización de OAuth 2.0](v2-oauth2-auth-code-flow.md).

![Aplicaciones de demonio](./media/scenarios/mobile-app.svg)

Consideraciones para las aplicaciones móviles:

- **La experiencia del usuario es clave**: permita a los usuarios ver el valor de la aplicación antes de solicitarles iniciar sesión. Solicite únicamente los permisos necesarios.
- **Admitir todas las configuraciones de usuario**: muchos usuarios empresariales móviles deben cumplir directivas de acceso condicional y de cumplimiento del dispositivo. Asegúrese de que admite estos escenarios claves.
- **Implementación del inicio de sesión único (SSO)** : con MSAL y la plataforma de identidad de Microsoft, puede habilitar el inicio de sesión único a través del explorador del dispositivo o Microsoft Authenticator (Portal de empresa de Intune en Android).

## <a name="specifics"></a>Características específicas

Tenga en cuenta las siguientes consideraciones al compilar una aplicación móvil en la plataforma de identidad de Microsoft:

- En función de la plataforma, podría ser necesaria alguna interacción del usuario la primera vez que inicia sesión. Por ejemplo, iOS requiere que las aplicaciones muestren interacción del usuario cuando usan el inicio de sesión único por primera vez a través de Microsoft Authenticator (y Portal de empresa de Intune en Android).
- En iOS y Android, MSAL podría usar un explorador externo para iniciar la sesión de los usuarios. El explorador externo podría aparecer en la parte superior de la aplicación. Puede personalizar la configuración para que use vistas web en la aplicación en su lugar.
- Nunca use un secreto en una aplicación móvil. En estas aplicaciones, los secretos son accesibles para todos los usuarios.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Registro de aplicaciones](scenario-mobile-app-registration.md)

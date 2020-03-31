---
title: Migración a la Biblioteca de autenticación de Microsoft (MSAL)
titleSuffix: Microsoft identity platform
description: Conozca las diferencias entre la Biblioteca de autenticación de Microsoft (MSAL) y la Biblioteca de Autenticación de Azure AD (ADAL) y cómo migrar a MSAL.
services: active-directory
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 02/27/2020
ms.author: jmprieur
ms.reviewer: saeeda
ms.custom: aaddev
ms.openlocfilehash: 3af18eb09fd9906a0caaebda0b786795400467f3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "78165398"
---
# <a name="migrate-applications-to-microsoft-authentication-library-msal"></a>Migración de aplicaciones a la Biblioteca de autenticación de Microsoft (MSAL)

Tanto la Biblioteca de autenticación de Microsoft (MSAL) como la Biblioteca de Autenticación de Azure AD (ADAL) se usan para autenticar entidades de Azure AD y solicitar tokens a Azure AD. Hasta ahora, la mayoría de los desarrolladores ha usado la plataforma Azure AD para desarrolladores (v1.0) para autenticar identidades de Azure AD (cuentas profesionales y educativas) mediante la solicitud de tokens conAzure AD Authentication Library (ADAL). Con MSAL:

- Puede autenticar un conjunto más amplio de identidades de Microsoft (identidades de Azure AD y cuentas Microsoft, así como cuentas de redes sociales y locales mediante Azure AD B2C) puesto que se usa el punto de conexión de la plataforma de identidad de Microsoft.
- Los usuarios obtendrán la mejor experiencia de inicio de sesión único.
- La aplicación puede permitir el consentimiento incremental y es más fácil admitir el acceso condicional.
- Se beneficia de la innovación.

**MSAL es ahora la biblioteca de autenticación recomendada para la plataforma de identidad de Microsoft**. No se implementarán características nuevas en ADAL. Los trabajos se centran en mejorar MSAL.

En los artículos siguientes se describen las diferencias entre las bibliotecas MSAL y ADAL y lo ayudarán a migrar a MSAL:
- [Migración a MSAL.NET](msal-net-migration.md)
- [Migración a MSAL.js](msal-compare-msal-js-and-adal-js.md)
- [Migración a MSAL.Android](migrate-android-adal-msal.md)
- [Migración a MSAL.iOS o macOS](migrate-objc-adal-msal.md)
- [Migración a MSAL para Python](migrate-python-adal-msal.md)
- [Migración a MSAL para Java](migrate-adal-msal-java.md)
- [Migración de aplicaciones de Xamarin mediante agentes a MSAL.NET](msal-net-migration-ios-broker.md)

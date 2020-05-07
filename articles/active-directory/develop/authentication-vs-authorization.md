---
title: Autenticación frente a autorización | Azure
titleSuffix: Microsoft identity platform
description: Obtenga información sobre los conceptos básicos de la autenticación y la autorización en la Plataforma de identidad de Microsoft (v2.0).
services: active-directory
author: rwike77
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 04/28/2020
ms.author: ryanwi
ms.reviewer: jmprieur, saeeda, sureshja, hirsin
ms.custom: aaddev, identityplatformtop40, scenarios:getting-started
ms.openlocfilehash: 61c0202d12756201d45fe829078d84382e44b54e
ms.sourcegitcommit: b9d4b8ace55818fcb8e3aa58d193c03c7f6aa4f1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/29/2020
ms.locfileid: "82584164"
---
# <a name="authentication-vs-authorization"></a>Autenticación frente a autorización

En este artículo se definen la autenticación y la autorización, y se explica brevemente cómo usar la Plataforma de identidad de Microsoft para autenticar y autorizar a los usuarios de sus aplicaciones web, API web o aplicaciones que llaman a las API web protegidas. Si ve un término con el que no está familiarizado, pruebe nuestro [glosario](developer-glossary.md) o nuestros [vídeos de la plataforma de identidad de Microsoft](identity-videos.md) que tratan de conceptos básicos.

## <a name="authentication"></a>Authentication

La **autenticación** es el proceso mediante el que se comprueba que es quien dice ser. A veces, la autenticación se abrevia como AuthN. La Plataforma de identidad de Microsoft implementa el protocolo [OpenID Connect](https://openid.net/connect/) para controlar la autenticación.

## <a name="authorization"></a>Authorization

La **autorización** es el acto de conceder a una parte autenticada permiso para hacer algo. Especifica a qué datos se puede acceder y qué se puede hacer con ellos. A veces, la autorización se abrevia como AuthZ. La Plataforma de identidad de Microsoft implementa el protocolo [OAuth 2.0](https://oauth.net/2/) para controlar la autenticación.

## <a name="authentication-and-authorization-using-the-microsoft-identity-platform"></a>Autenticación y autorización con la Plataforma de identidad de Microsoft

En lugar de crear aplicaciones que tengan un nombre de usuario y una contraseña propios, lo que supone una gran carga administrativa cuando es necesario agregar o quitar usuarios en varias aplicaciones, las aplicaciones pueden delegar esa responsabilidad en un proveedor de identidades.

Azure Active Directory (Azure AD) es un proveedor de identidades centralizado en la nube. Al delegar la autenticación y la autorización en esta solución, se abren otras posibilidades, como el uso de directivas de acceso condicional que requieren que un usuario esté en una ubicación específica, el uso de la autenticación multifactor y la posibilidad de que un usuario solo tenga que iniciar sesión una vez para que, de forma automática, inicie sesión en todas las aplicaciones web que comparten el mismo directorio centralizado. Esta funcionalidad se conoce como **inicio de sesión único (SSO)** .

La Plataforma de identidad de Microsoft simplifica la autenticación y la autorización para los desarrolladores de aplicaciones, ya que ofrece identidad como servicio, con compatibilidad con protocolos estándar del sector, como OAuth 2.0 y OpenID Connect, además de bibliotecas de código abierto para distintas plataformas, lo que le permitirá empezar a programar rápidamente. Permite a los desarrolladores crear aplicaciones que inicien sesión en todas las identidades de Microsoft, obtener tokens para llamar a [Microsoft Graph](https://developer.microsoft.com/graph/), otras API de Microsoft o API que los desarrolladores hayan creado. Para más información, consulte [Evolución de la Plataforma de identidad de Microsoft](about-microsoft-identity-platform.md).

## <a name="next-steps"></a>Pasos siguientes

Para ver otros temas en los que se tratan aspectos básicos de la autenticación y la autorización:

* Consulte [Tokens de seguridad](security-tokens.md) para obtener información sobre el acceso a los tokens, la actualización de los tokens y el uso de tokens de identificador en la autenticación y la autorización.
* Consulte [Modelo de aplicación](application-model.md) para obtener información sobre el proceso de registro de la aplicación para que pueda integrarse con la Plataforma de identidad de Microsoft.
* Consulte [Flujo de inicio de sesión de aplicaciones](app-sign-in-flow.md) para obtener información sobre el flujo de inicio de sesión de las aplicaciones web, de escritorio y móviles en la Plataforma de identidad de Microsoft.

Para más información sobre los protocolos que implementa la Plataforma de identidad de Microsoft:

* Consulte [Protocolos OAuth 2.0 y OpenID Connect en la Plataforma de identidad de Microsoft](active-directory-v2-protocols.md) para más información sobre los estándares OpenID Connect y OAuth 2.0.
* Consulte [Protocolo SAML de inicio de sesión único](single-sign-on-saml-protocol.md) para más información sobre cómo admite el inicio de sesión único la Plataforma de identidad de Microsoft.

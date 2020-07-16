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
ms.date: 05/22/2020
ms.author: ryanwi
ms.reviewer: jmprieur, saeeda, sureshja, hirsin
ms.custom: aaddev, identityplatformtop40, scenarios:getting-started
ms.openlocfilehash: 095bc5594cf9b6eaf0df7929ea83c25a401793e2
ms.sourcegitcommit: dabd9eb9925308d3c2404c3957e5c921408089da
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/11/2020
ms.locfileid: "86252120"
---
# <a name="authentication-vs-authorization"></a>Autenticación frente a autorización

En este artículo se definen la autenticación y la autorización, y se explica brevemente cómo usar la Plataforma de identidad de Microsoft para autenticar y autorizar a los usuarios de sus aplicaciones web, API web o aplicaciones que llaman a las API web protegidas. Si ve un término con el que no está familiarizado, pruebe nuestro [glosario](developer-glossary.md) o nuestros [vídeos de la plataforma de identidad de Microsoft](identity-videos.md) que tratan de conceptos básicos.

## <a name="authentication"></a>Authentication

La **autenticación** es el proceso mediante el que se comprueba que es quien dice ser. A veces, la autenticación se abrevia como AuthN. La Plataforma de identidad de Microsoft implementa el protocolo [OpenID Connect](https://openid.net/connect/) para controlar la autenticación.

## <a name="authorization"></a>Authorization

La **autorización** es el acto de conceder a una parte autenticada permiso para hacer algo. Especifica a qué datos se puede acceder y qué se puede hacer con ellos. A veces, la autorización se abrevia como AuthZ. La plataforma de identidad de Microsoft implementa el protocolo [OAuth 2.0](https://oauth.net/2/) para controlar la autenticación.

## <a name="authentication-and-authorization-using-microsoft-identity-platform"></a>Autenticación y autorización mediante la Plataforma de identidad de Microsoft

En lugar de crear aplicaciones que tengan un nombre de usuario y una contraseña propios, lo que supone una gran carga administrativa cuando es necesario agregar o quitar usuarios en varias aplicaciones, las aplicaciones pueden delegar esa responsabilidad en un proveedor de identidades.

Azure Active Directory (Azure AD) es un proveedor de identidades centralizado en la nube. Al delegar la autenticación y la autorización en esta solución, se abren otras posibilidades, como el uso de directivas de acceso condicional que requieren que un usuario esté en una ubicación específica, el uso de la [autenticación multifactor](../authentication/concept-mfa-howitworks.md) (a veces denominada autenticación de dos factores o 2FA) y la posibilidad de que un usuario solo tenga que iniciar sesión una vez para que, de forma automática, inicie sesión en todas las aplicaciones web que comparten el mismo directorio centralizado. Esta funcionalidad se conoce como **inicio de sesión único (SSO)** .

La Plataforma de identidad de Microsoft simplifica la autenticación y la autorización para los desarrolladores de aplicaciones, ya que ofrece identidad como servicio, con compatibilidad con protocolos estándar del sector, como OAuth 2.0 y OpenID Connect, además de bibliotecas de código abierto para distintas plataformas, lo que le permitirá empezar a programar rápidamente. Permite a los desarrolladores crear aplicaciones que inicien sesión en todas las identidades de Microsoft, obtener tokens para llamar a [Microsoft Graph](https://developer.microsoft.com/graph/), otras API de Microsoft o API que los desarrolladores hayan creado.

La siguiente es una breve comparación de los diversos protocolos usados por la plataforma de Microsoft Identity:

* **OAuth frente a OpenID Connect**: OAuth se usa para la autorización y OpenID Connect (OIDC) se usa para la autenticación. OpenID Connect se basa en OAuth 2.0, por lo que la terminología y el flujo son similares entre los dos. Incluso puede autenticar a un usuario (mediante OpenID Connect) y obtener autorización para tener acceso a un recurso protegido que posee el usuario (mediante OAuth 2.0) en una solicitud. Para más información, consulte los [Protocolos OAuth 2.0 y OpenID Connect](active-directory-v2-protocols.md) y [Protocolo OpenID Connect](v2-protocols-oidc.md).
* **OAuth frente SAML**: OAuth se usa para la autorización y SAML se usa para la autenticación. Consulte [Plataforma de identidad de Microsoft y flujo de aserción de portador SAML de OAuth 2.0](v2-saml-bearer-assertion.md) para obtener más información sobre cómo se pueden usar los dos protocolos juntos para autenticar a un usuario (mediante SAML) y obtener autorización para tener acceso a un recurso protegido (mediante OAuth 2.0).
* **OpenID Connect frente SAML**: Tanto OpenID Connect como SAML se usan para autenticar a un usuario y para habilitar el inicio de sesión único. La autenticación SAML se usa normalmente con proveedores de identidades como Servicios de federación de Active Directory (AD FS) federado para Azure AD y, por lo tanto, se utiliza con frecuencia en aplicaciones empresariales. OpenID Connect se usa normalmente para las aplicaciones que se encuentran exclusivamente en la nube, como aplicaciones móviles, sitios web y APIs Web.

## <a name="next-steps"></a>Pasos siguientes

Para ver otros temas en los que se traten aspectos básicos de la autenticación y la autorización:

* Consulte [Tokens de seguridad](security-tokens.md) para obtener información sobre el acceso a los tokens, la actualización de los tokens y el uso de tokens de identificador en la autenticación y la autorización.
* Ver [Modelo de aplicación](application-model.md) para obtener información sobre el proceso de registro de la aplicación para que pueda integrarse con la plataforma de identidad de Microsoft.
* Ver [Flujo de inicio de sesión de aplicaciones](app-sign-in-flow.md) para obtener información sobre el flujo de inicio de sesión de las aplicaciones web, de escritorio y móviles en la plataforma de identidad de Microsoft.

* Para obtener más información sobre los protocolos que implementa la plataforma de identidad de Microsoft, consulte [Protocolos OAuth 2.0 y OpenID Connect en la Plataforma de identidad de Microsoft](active-directory-v2-protocols.md).
* Consulte [Protocolo SAML de inicio de sesión único](single-sign-on-saml-protocol.md) para más información sobre cómo admite el inicio de sesión único la Plataforma de identidad de Microsoft.
* Consulte [Inicio de sesión único en aplicaciones de Azure Active Directory](../manage-apps/what-is-single-sign-on.md) para obtener más información sobre las distintas formas en que puede implementar el inicio de sesión único en la aplicación.

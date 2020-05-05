---
title: Flujo de inicio de sesión de aplicaciones con la Plataforma de identidad de Microsoft | Azure
titleSuffix: Microsoft identity platform
description: Obtenga información sobre el flujo de inicio de sesión de las aplicaciones web, de escritorio y móviles en la Plataforma de identidad de Microsoft (v2.0).
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
ms.openlocfilehash: 7b326e17611b5f4b9520d8218a28a67afe9a851a
ms.sourcegitcommit: b9d4b8ace55818fcb8e3aa58d193c03c7f6aa4f1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/29/2020
ms.locfileid: "82584180"
---
# <a name="app-sign-in-flow-with-microsoft-identity-platform"></a>Flujo de inicio de sesión de aplicaciones con la Plataforma de identidad de Microsoft

En este tema se describe el flujo de inicio de sesión básico de aplicaciones web, de escritorio y móviles con la Plataforma de identidad de Microsoft. Consulte [Flujos de autenticación y escenarios de aplicaciones](authentication-flows-app-scenarios.md) para más información sobre los escenarios de inicio de sesión que admite la Plataforma de identidad de Microsoft.

## <a name="web-app-sign-in-flow"></a>Flujo de inicio de sesión de aplicaciones web

Cuando un usuario navega en el explorador a una aplicación web, ocurre lo siguiente:

* La aplicación web determina si el usuario está autenticado.
* Si no lo está, la aplicación web delega en Azure AD el inicio de sesión del usuario. Ese inicio de sesión tendrá que cumplir la directiva de la organización, lo que puede significar que se le pida al usuario que escriba sus credenciales, que utilice la autenticación multifactor o que ni siquiera tenga que especificar una contraseña (por ejemplo, si utiliza Windows Hello).
* Se pide al usuario que dé el consentimiento de acceso que necesita la aplicación cliente. Este es el motivo por el que las aplicaciones cliente deben registrarse con Azure AD, de modo que la Plataforma de identidad de Microsoft pueda proporcionar tokens que representen el acceso autorizado por el usuario.

Cuando el usuario se ha autenticado correctamente:

* La Plataforma de identidad de Microsoft envía un token a la aplicación web.
* Se guarda una cookie, asociada al dominio de Azure AD, que contiene la identidad del usuario en el archivo jar de la cookie del explorador. La próxima vez que una aplicación use el explorador para ir al punto de conexión de autorización de la Plataforma de identidad de Microsoft, el explorador presentará la cookie para que el usuario no tenga que volver a iniciar sesión. También es la mecanismo con el que se realiza el SSO. Azure AD genera la cookie y solo él puede entenderla.
* A continuación, la aplicación web valida el token. Si la validación se realiza correctamente, la aplicación web muestra la página protegida y guarda una cookie de sesión en el archivo jar de la cookie del explorador. Cuando el usuario accede a otra página, la aplicación web sabe que el usuario está autenticado por la cookie de sesión.

En el siguiente diagrama de secuencias, se resume esta interacción:

![Proceso de autenticación de aplicaciones web](media/authentication-scenarios/web-app-how-it-appears-to-be.png)

### <a name="how-a-web-app-determines-if-the-user-is-authenticated"></a>Cómo determina una aplicación web si el usuario está autenticado

Los desarrolladores de aplicaciones web pueden especificar si todas o solo algunas páginas requieren autenticación. Por ejemplo, en ASP.NET/ASP.NET Core, esto se hace agregando el atributo `[Authorize]` a las acciones del controlador.

Este atributo hace que ASP.NET compruebe la presencia de una cookie de sesión que contiene la identidad del usuario. Si esta cookie no está, ASP.NET redirige la autenticación al proveedor de identidades especificado. Si el proveedor de identidades es Azure AD, la aplicación web redirige la autenticación a `https://login.microsoftonline.com`, que muestra un cuadro de diálogo de inicio de sesión.

### <a name="how-a-web-app-delegates-sign-in-to-microsoft-identity-platform-and-obtains-a-token"></a>Procedimiento que realiza una aplicación web para delegar el inicio de sesión en la Plataforma de identidad de Microsoft y obtener un token

La autenticación del usuario se realiza mediante el explorador. El protocolo OpenID usa mensajes con el protocolo HTTP estándar.

* La aplicación web envía una solicitud HTTP 302 (redireccionamiento) al explorador para que utilice la Plataforma de identidad de Microsoft.
* Cuando se autentica el usuario, la Plataforma de identidad de Microsoft envía el token a la aplicación web utilizando un redireccionamiento mediante el explorador.
* La aplicación web proporciona el redireccionamiento en forma de un URI de redirección. Este URI de redirección se registra con el objeto de aplicación de Azure AD. Puede haber varios URI de redireccionamiento, ya que la aplicación puede estar implementada en varias direcciones URL. Por lo tanto, la aplicación web también tendrá que especificar el URI de redirección que va a utilizar.
* Azure AD verifica que el URI de redirección enviado por la aplicación web es uno de los URI de redirección registrados para la aplicación.

## <a name="desktop-and-mobile-app-sign-in-flow"></a>Flujo de inicio de sesión para aplicaciones móviles y de escritorio

El flujo descrito anteriormente se aplica, con ligeras diferencias, tanto a las aplicaciones móviles como a las aplicaciones de escritorio.

Las aplicaciones móviles y de escritorio pueden utilizar un control web integrado o un explorador del sistema para la autenticación. En el diagrama siguiente, se muestra cómo una aplicación móvil o de escritorio usa la biblioteca de autenticación de Microsoft (MSAL) para adquirir tokens de acceso y llamar a las API web.

![Aspecto que debería tener una aplicación de escritorio](media/authentication-scenarios/desktop-app-how-it-appears-to-be.png)

MSAL usa un explorador para obtener los tokens. Al igual que con las aplicaciones web, la autenticación se delega en la Plataforma de identidad de Microsoft.

Como Azure AD guarda en el explorador la misma cookie de identidad que utiliza con las aplicaciones web, si la aplicación nativa o móvil usa el explorador del sistema, obtendrá de inmediato SSO con la aplicación web correspondiente.

De forma predeterminada, MSAL usa el explorador del sistema. La excepción son las aplicaciones de escritorio de .NET Framework, en las que se usa un control integrado para proporcionar al usuario una experiencia más fluida.

## <a name="next-steps"></a>Pasos siguientes

Para ver otros temas en los que se traten aspectos básicos de la autenticación y la autorización:

* Vea [Autenticación frente a autorización](authentication-vs-authorization.md) para obtener información sobre los aspectos básicos de la autenticación y la autorización en la Plataforma de identidad de Microsoft.
* Consulte [Tokens de seguridad](security-tokens.md) para obtener información sobre el acceso a tokens, la actualización de tokens y el uso de tokens de identificación en la autenticación y la autorización.
* Vea [Modelo de aplicación](application-model.md) para obtener información sobre el proceso de registro de la aplicación para que pueda integrarse con la Plataforma de identidad de Microsoft.

Para más información sobre el flujo de inicio de sesión de aplicaciones:

* Consulte [Flujos de autenticación y escenarios de aplicaciones](authentication-flows-app-scenarios.md) para más información sobre otros escenarios de autenticación de usuarios compatibles con la Plataforma de identidad de Microsoft.
* Consulte las [bibliotecas MSAL](msal-overview.md) para más información sobre las bibliotecas de Microsoft, que le ayudarán a desarrollar aplicaciones que funcionan con cuentas Microsoft, cuentas de Azure AD y usuarios de Azure AD B2C en un modelo de programación simplificado y único.

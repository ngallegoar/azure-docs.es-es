---
title: Aplicaciones web en Azure Active Directory
description: Se describe qué son las aplicaciones web y los conceptos básicos sobre el flujo de protocolos, el registro y la expiración de tokens en este tipo de aplicación.
services: active-directory
documentationcenter: ''
author: rwike77
manager: CelesteDG
ms.service: active-directory
ms.subservice: azuread-dev
ms.workload: identity
ms.topic: conceptual
ms.date: 09/24/2018
ms.author: ryanwi
ms.reviewer: saeeda, jmprieur
ms.custom: aaddev
ROBOTS: NOINDEX
ms.openlocfilehash: e4a7fb72d40f5db65e8e30264e9d68b2727749e4
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/28/2020
ms.locfileid: "80154413"
---
# <a name="web-apps"></a>Aplicaciones web

[!INCLUDE [active-directory-azuread-dev](../../../includes/active-directory-azuread-dev.md)]

Las aplicaciones web son aplicaciones que autentican a un usuario de un explorador web en una aplicación web. En este escenario, la aplicación web dirige el explorador del usuario para que inicie sesión en Azure AD. Azure AD devuelve una respuesta de inicio de sesión a través el explorador del usuario, que contiene notificaciones sobre el usuario en un token de seguridad. Este escenario admite el inicio de sesión mediante los protocolos OpenID Connect, SAML 2.0 y WS-Federation.

## <a name="diagram"></a>Diagrama

![Flujo de autenticación de explorador a aplicación web](./media/authentication-scenarios/web-browser-to-web-api.png)

## <a name="protocol-flow"></a>Flujo de protocolos

1. Cuando un usuario visita la aplicación y debe iniciar sesión, se le redirige mediante una solicitud de inicio de sesión al extremo de autenticación de Azure AD.
1. El usuario inicia sesión en la página de inicio de sesión.
1. Si la autenticación es correcta, Azure AD crea un token de autenticación y devuelve una respuesta de inicio de sesión a la dirección URL de respuesta de la aplicación que se configuró en Azure Portal. En el caso de una aplicación de producción, esta URL de respuesta debe ser HTTPS. El token devuelto incluye notificaciones sobre el usuario y Azure AD, necesarias para que la aplicación valide el token.
1. La aplicación valida el token mediante una clave de firma pública y la información del emisor disponible en el documento de metadatos de federación para Azure AD. Cuando la aplicación haya validado el token, inicia una nueva sesión con el usuario. Hasta que expire, esta sesión permitirá al usuario tener acceso a la aplicación.

## <a name="code-samples"></a>Ejemplos de código

Consulte los ejemplos de código correspondientes a los escenarios de explorador web a aplicación web. Agregamos nuevos ejemplos continuamente, así que no olvide consultarlos con frecuencia.

## <a name="app-registration"></a>Registro de aplicación

Para registrar una aplicación web, vea [Registrar una aplicación](../develop/quickstart-register-app.md?toc=/azure/active-directory/azuread-dev/toc.json&bc=/azure/active-directory/azuread-dev/breadcrumb/toc.json).

* Inquilino único: si va a compilar una aplicación únicamente para la organización, esta debe registrarse en el directorio de la empresa mediante Azure Portal.
* Multiinquilino: si va a compilar una aplicación que puede ser utilizada por usuarios ajenos a la organización, esta debe registrarse en el directorio de la compañía, pero también en el directorio de cada organización que vaya a usar la aplicación. Para que la aplicación esté disponible en su directorio, puede incluir un proceso de inicio de sesión para que los clientes puedan dar su consentimiento a la aplicación. Cuando inicien sesión en la aplicación, se les presentará un cuadro de diálogo que muestra los permisos que la aplicación requiere y la opción de consentimiento. Según los permisos necesarios, es posible que se requiera que un administrador de la otra organización dé su consentimiento. Cuando el usuario o el administrador dan su consentimiento, la aplicación queda registrada en el directorio.

## <a name="token-expiration"></a>Expiración del token

La sesión del usuario expira cuando expira la duración del token emitido por Azure AD. Si lo desea, la aplicación puede reducir este período de tiempo; por ejemplo, cerrar sesiones de usuario en función de un período de inactividad. Cuando la sesión expire, se le pedirá al usuario que vuelva a iniciar sesión.

## <a name="next-steps"></a>Pasos siguientes

* Más información sobre otros [tipos y escenarios de aplicaciones](app-types.md).
* Más información sobre los [conceptos básicos de autenticación](v1-authentication-scenarios.md) de Azure AD.

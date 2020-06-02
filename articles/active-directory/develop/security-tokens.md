---
title: Tokens de seguridad | Azure
titleSuffix: Microsoft identity platform
description: Conozca los aspectos básicos de los tokens de seguridad de la Plataforma de identidad de Microsoft (v2.0).
services: active-directory
author: rwike77
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 05/11/2020
ms.author: ryanwi
ms.reviewer: jmprieur, saeeda, sureshja, hirsin
ms.custom: aaddev, identityplatformtop40, scenarios:getting-started
ms.openlocfilehash: def198a15710d0aff4a943300eedc338a7772e46
ms.sourcegitcommit: a8ee9717531050115916dfe427f84bd531a92341
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/12/2020
ms.locfileid: "83115802"
---
# <a name="security-tokens"></a>Tokens de seguridad

Un proveedor de identidades centralizado es especialmente útil para las aplicaciones que tienen usuarios repartidos por todo el mundo que no inician sesión necesariamente desde la red de la empresa. La Plataforma de identidad de Microsoft autentica a los usuarios y proporciona tokens de seguridad, como el [token de acceso](developer-glossary.md#access-token), el [token de actualización](developer-glossary.md#refresh-token) y el [token de identificador](developer-glossary.md#id-token), que permiten a una [aplicación cliente](developer-glossary.md#client-application) acceder a recursos protegidos en un [servidor de recursos](developer-glossary.md#resource-server).

Un **token de acceso** es un token de seguridad emitido por un [servidor de autorización](developer-glossary.md#authorization-server) como parte de un flujo de [OAuth 2.0](active-directory-v2-protocols.md). Contiene información sobre el usuario y la aplicación para la que se ha creado el token y se puede usar para acceder tanto a las API web como a otros recursos protegidos. Para más información sobre cómo emite la Plataforma de identidad de Microsoft los tokens de acceso, consulte [Tokens de acceso](access-tokens.md).

Los tokens de acceso solo son válidos durante un breve período de tiempo, por lo que los servidores de autorización a veces emiten un **token de actualización** al mismo tiempo que se emite el token de acceso. La aplicación cliente puede intercambiar este token de actualización por un nuevo token de acceso cuando es necesario. Para más información sobre cómo usa la plataforma de identidad de Microsoft los tokens de actualización para revocar permisos, consulte [Revocación de tokens](access-tokens.md#token-revocation).

Los **tokens de identificador** se envían a la aplicación cliente como parte de un flujo de [OpenID Connect](v2-protocols-oidc.md). Se pueden enviar con o en lugar de un token de acceso y el cliente los utiliza para autenticar al usuario. Para más información sobre cómo emite la Plataforma de identidad de Microsoft los tokens de identificador, consulte [tokens de identificador.](id-tokens.md)

> [!NOTE]
> En este artículo se describen los tokens de seguridad que usan los protocolos OAuth2 y OpenID Connect. Muchas aplicaciones empresariales usan SAML para autenticar a los usuarios. Consulte [Referencia de tokens SAML de Azure AD](reference-saml-tokens.md) para obtener información sobre las aserciones de SAML.

## <a name="validating-security-tokens"></a>Validación de tokens de seguridad

La validación del token depende de la aplicación para la que se generó el token, de la aplicación web en la que inició sesión el usuario o de la API web que se está invocando. El token está firmado por el **servicio de token de seguridad (STS)** con una clave privada. El STS publica la clave pública correspondiente. Para validar un token, la aplicación comprueba la firma utilizando la clave pública de STS a fin de validar que se creó con la clave privada.

Los tokens solo son válidos durante un período de tiempo limitado. Normalmente, el STS proporciona un par de tokens:

* un token de acceso para acceder a la aplicación o el recurso protegido, y
* un token de actualización que se usa para actualizar el token de acceso cuando este está próximo a expirar.

Los tokens de acceso se pasan a una API web en el encabezado de `Authorization` como un token de portador. Una aplicación puede proporcionar un token de actualización al STS y, si el acceso del usuario a la aplicación no se ha revocado, recibirá un nuevo token de acceso y un nuevo token de actualización. Así es cómo se administran los casos cuando alguien abandona la empresa. Cuando el STS recibe el token de actualización, si el usuario ya no está autorizado, no emitirá otro token de acceso válido.

## <a name="json-web-tokens-jwts-and-claims"></a>JSON Web Tokens (JWT) y notificaciones

La Plataforma de identidad de Microsoft implementa los tokens de seguridad como **JSON Web Tokens (JWT)** que contienen **notificaciones**. Dado que los JWT se usan como tokens de seguridad, esta forma de autenticación se denomina a veces **autenticación de JWT**.

Una [notificación](developer-glossary.md#claim) proporciona aserciones sobre una entidad, como una aplicación cliente o un [propietario de recursos](developer-glossary.md#resource-owner), a otra entidad, como un servidor de recursos. También se puede hacer referencia a una notificación como una notificación de JWT o de JSON Web Token.

Las notificaciones son pares nombre-valor que retransmiten hechos sobre el firmante del token. Por ejemplo, una notificación puede contener datos sobre la entidad de seguridad autenticada por el servidor de autorización. Las notificaciones presentes en un token determinado dependen de muchas cosas, como el tipo de token, el tipo de credencial usada para autenticar al firmante, la configuración de la aplicación, etc.

Las aplicaciones pueden usar notificaciones para varias tareas, como:

* Validar el token.
* Identificar el [inquilino](developer-glossary.md#tenant) del firmante del token
* Mostrar información sobre el usuario.
* Determinar la autorización del sujeto.

Una notificación se compone de pares clave-valor que proporcionan información, como:

* El servidor de tokens de seguridad que generó el token.
* La fecha en la que se generó el token.
* El firmante (por ejemplo, el usuario, excepto en el caso de los demonios)
* La audiencia, que es la aplicación para la que se generó el token.
* La aplicación (el cliente) que solicitó el token. En el caso de las aplicaciones web, puede ser igual a la audiencia.

Para más información sobre cómo implementa la plataforma de identidad de Microsoft los tokens y la información de notificaciones, consulte [tokens de acceso](access-tokens.md) y [tokens de identificador](id-tokens.md).

## <a name="how-each-flow-emits-tokens-and-codes"></a>Cómo emite cada flujo los tokens y los códigos

En función de cómo se compile el cliente, puede usar uno (o varios) de los flujos de autenticación admitidos por la plataforma de identidad de Microsoft. Estos flujos pueden generar una serie de tokens (de identificador, de actualización, de acceso) además de códigos de autorización, y necesitan distintos tokens que les hagan funcionar. En este gráfico se ofrece una información general:

|Flujo | Requiere | token de identificador | de la aplicación Twitter | Token de actualización | código de autorización |
|-----|----------|----------|--------------|---------------|--------------------|
|[Flujo de código de autorización](v2-oauth2-auth-code-flow.md) | | x | x | x | x|
|[Flujo implícito](v2-oauth2-implicit-grant-flow.md) | | x        | x    |      |                    |
|[Flujo de OIDC híbrido](v2-protocols-oidc.md#get-access-tokens)| | x  | |          |            x   |
|[Redención de token de actualización](v2-oauth2-auth-code-flow.md#refresh-the-access-token) | Token de actualización | x | x | x| |
|[Flujo en nombre de](v2-oauth2-on-behalf-of-flow.md) | de la aplicación Twitter| x| x| x| |
|[Credenciales de cliente](v2-oauth2-client-creds-grant-flow.md) | | | x (solo aplicación)| | |

Los tokens emitidos a través del modo implícito tienen una limitación de longitud debido a que se pasan al explorador a través de la dirección URL (donde `response_mode` es `query` o `fragment`).  Algunos exploradores tienen un límite en el tamaño de la dirección URL que se puede colocar en la barra del explorador y producen un error cuando es demasiado larga.  Por lo tanto, estos tokens no tienen notificaciones `groups` ni `wids`.

## <a name="next-steps"></a>Pasos siguientes

Para ver otros temas en los que se traten aspectos básicos de la autenticación y la autorización:

* Ver [Autenticación frente a autorización](authentication-vs-authorization.md) para obtener información sobre los aspectos básicos de la autenticación y la autorización en la plataforma de identidad de Microsoft.
* Ver [Modelo de aplicación](application-model.md) para obtener información sobre el proceso de registro de la aplicación para que pueda integrarse con la plataforma de identidad de Microsoft.
* Ver [Flujo de inicio de sesión de aplicaciones](app-sign-in-flow.md) para obtener información sobre el flujo de inicio de sesión de las aplicaciones web, de escritorio y móviles en la plataforma de identidad de Microsoft.

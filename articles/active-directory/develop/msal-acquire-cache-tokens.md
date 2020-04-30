---
title: Adquisición y almacenamiento en caché de tokens con MSAL | Azure
titleSuffix: Microsoft identity platform
description: Más información sobre la adquisición y el almacenamiento en caché de tokens mediante la biblioteca de autenticación de Microsoft (MSAL).
services: active-directory
author: mmacy
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 11/07/2019
ms.author: marsma
ms.reviewer: saeeda
ms.custom: aaddev
ms.openlocfilehash: 647dff9e6401322371ef795a25ca5ced2b517e9c
ms.sourcegitcommit: 31ef5e4d21aa889756fa72b857ca173db727f2c3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/16/2020
ms.locfileid: "81534591"
---
# <a name="acquire-and-cache-tokens-using-the-microsoft-authentication-library-msal"></a>Adquisición y almacenamiento en caché de tokens con la biblioteca de autenticación de Microsoft (MSAL)

Los [tokens de acceso](access-tokens.md) permiten a los clientes llamar a las API web protegidas por Azure de forma segura. Hay muchas maneras de adquirir un token mediante la biblioteca de autenticación de Microsoft (MSAL). Algunas de ellas requieren interacciones del usuario a través de un explorador web. Otras no. En general, la manera de adquirir un token depende de si la aplicación es una aplicación cliente pública (aplicación de escritorio o móvil) o una aplicación cliente confidencial (aplicación web, API web o aplicación de demonio como un servicio de Windows).

MSAL almacena en caché un token después de adquirirlo.  En primer lugar, el código de la aplicación debe intentar obtener un token de forma automática (desde la caché) antes de adquirirlo por otros medios.

También puede borrar la memoria caché de tokens, lo cual se consigue mediante la eliminación de las cuentas de la memoria caché. No obstante, esto no elimina la cookie de sesión que se encuentra en el explorador.

## <a name="scopes-when-acquiring-tokens"></a>Ámbitos al adquirir tokens

Los [ámbitos](v2-permissions-and-consent.md) son los permisos que expone una API web para que las aplicaciones cliente soliciten acceso. Las aplicaciones cliente solicitan el consentimiento del usuario para estos ámbitos al realizar solicitudes de autenticación para obtener los tokens de acceso a las API web. MSAL le permite obtener tokens para acceder a Azure AD para desarrolladores (v1.0) y a las API de la plataforma de identidad de Microsoft (v2.0). El protocolo de la versión v2.0 usa ámbitos en lugar de recursos en las solicitudes. Para más información, lea [la comparación entre las versiones 1.0 y 2.0](active-directory-v2-compare.md). Según la configuración de la API web de la versión de token que acepta, el punto de conexión v2.0 devolverá el token de acceso a MSAL.

Algunos métodos de adquisición de tokens de MSAL requieren un parámetro *scopes*. Este parámetro no es más que una lista de cadenas que declaran los permisos y recursos deseados que se solicitan. Los [permisos de Microsoft Graph](/graph/permissions-reference) son ámbitos muy conocidos.

En MSAL también se puede acceder a los recursos de v1.0. Para más información, lea [Ámbitos para una aplicación v1.0](msal-v1-app-scopes.md).

### <a name="request-specific-scopes-for-a-web-api"></a>Solicitud de ámbitos específicos para una API web

Si la aplicación necesita solicitar tokens con permisos específicos para una API de recurso, tendrá que pasar los ámbitos que contiene el URI del identificador de la aplicación en el siguiente formato: *&lt;URI del identificador de la aplicación&gt;/&lt;ámbito&gt;*

Por ejemplo, los ámbitos para Microsoft Graph API: `https://graph.microsoft.com/User.Read`

O también, por ejemplo, los ámbitos para una API web personalizada: `api://abscdefgh-1234-abcd-efgh-1234567890/api.read`

Solo para Microsoft Graph API, un valor de ámbito `user.read` se asigna al formato de `https://graph.microsoft.com/User.Read` y se pueden usar indistintamente.

> [!NOTE]
> Determinadas API web como la API de Azure Resource Manager (https://management.core.windows.net/) ) esperan una barra diagonal final "/" en la notificación de audiencia (aud) del token de acceso. En este caso, es importante pasar el ámbito como https://management.core.windows.net//user_impersonation (observe la doble barra diagonal) para que el token sea válido en la API.

### <a name="request-dynamic-scopes-for-incremental-consent"></a>Solicitud de ámbitos dinámicos para el consentimiento incremental

Cuando compilaba aplicaciones mediante v1.0, tenía que registrar el conjunto completo de permisos (ámbitos estáticos) que requería la aplicación para que el usuario diera su consentimiento en el momento del inicio de sesión. En v2.0 puede solicitar permisos adicionales según sea necesario mediante el parámetro de ámbito. Estos se denominan ámbitos dinámicos y permiten al usuario proporcionar un consentimiento incremental a los ámbitos.

Por ejemplo, puede hacer que el usuario inicie sesión inicialmente y después negarle todo tipo de permisos. Posteriormente, puede darle la posibilidad de leer el calendario del usuario solicitando el ámbito de calendario en los métodos de adquisición de tokens y obtener el consentimiento del usuario.

Por ejemplo, `https://graph.microsoft.com/User.Read` y `https://graph.microsoft.com/Calendar.Read`.

## <a name="acquiring-tokens-silently-from-the-cache"></a>Adquisición de tokens de forma automática (desde la memoria caché)

MSAL mantiene una memoria caché de tokens, o dos en el caso de las aplicaciones cliente confidenciales, y almacena en caché un token después de adquirirlo.  En muchos casos, al intentar obtener de forma automática un token adquirirá otro token con más ámbitos en función de un token de la caché. También puede actualizar un token cuando va a expirar (ya que la caché de tokens también contiene un token de actualización).

### <a name="recommended-call-pattern-for-public-client-applications"></a>Patrón de llamada recomendado para aplicaciones cliente públicas

En primer lugar, el código de la aplicación debe intentar obtener un token de forma automática (desde la caché).  Si la llamada al método devuelve un error o una excepción que indica que "se requiere una interfaz de usuario", intente adquirir un token por otros medios.

Sin embargo, hay dos flujos antes de los cuales **no debería** intentar adquirir un token de forma automática:

- El [flujo de credenciales del cliente](msal-authentication-flows.md#client-credentials), que no usa la caché de tokens de usuario sino una caché de tokens de aplicación. Este método se encarga de comprobar esta caché de tokens de aplicación antes de enviar una solicitud al servicio de token de seguridad.
- El [flujo de código de autorización](msal-authentication-flows.md#authorization-code) de las aplicaciones web, ya que canjea un código que la aplicación obtuvo al iniciar sesión el usuario y dar el consentimiento a más ámbitos. Puesto que un código se pasa como un parámetro, y no como una cuenta, el método no puede buscar en la caché antes de canjear el código, lo cual requiere, en cualquier caso, una llamada al servicio.

### <a name="recommended-call-pattern-in-web-apps-using-the-authorization-code-flow"></a>Patrón de llamada recomendado en aplicaciones web mediante el flujo de código de autorización

Para las aplicaciones web que usan el [flujo de código de autorización de OpenID Connect](v2-protocols-oidc.md), el patrón recomendado en los controladores consiste en:

- Crear una instancia de una aplicación cliente confidencial con una caché de tokens con serialización personalizada.
- Adquirir el token mediante el flujo de código de autorización

## <a name="acquiring-tokens"></a>Adquisición de tokens

Por lo general, el método de adquisición de un token depende de si es una aplicación cliente pública o confidencial.

### <a name="public-client-applications"></a>Aplicaciones cliente públicas

Para las aplicaciones cliente públicas (aplicación de escritorio o móvil):
- A menudo adquiere tokens de forma interactiva, haciendo que el usuario inicie sesión mediante una interfaz de usuario o una ventana emergente.
- Puede [obtener un token de forma automática](msal-authentication-flows.md#integrated-windows-authentication) mediante la autenticación integrada (IWA/Kerberos) si la aplicación de escritorio se ejecuta en un equipo de Windows unido a un dominio o a Azure.
- Puede [obtener un token con un nombre de usuario y contraseña](msal-authentication-flows.md#usernamepassword) en las aplicaciones cliente de escritorio de la plataforma .NET, pero no es recomendable. No utilice un nombre de usuario y contraseña en aplicaciones cliente confidenciales.
- Puede adquirir un token mediante el [flujo de código de dispositivo](msal-authentication-flows.md#device-code) en aplicaciones que se ejecutan en dispositivos que no tienen un explorador web. Se proporciona al usuario una dirección URL y un código y, a continuación, el usuario va a un explorador web de otro dispositivo y escribe el código e inicia sesión.  Después, Azure AD envía un token de nuevo al dispositivo sin explorador.

### <a name="confidential-client-applications"></a>Aplicaciones cliente confidenciales

Para las aplicaciones cliente confidenciales (aplicación web, API web o aplicación de demonio como servicio de Windows):
- Adquiere tokens **para la aplicación en sí** y no para un usuario mediante el [flujo de credenciales de cliente](msal-authentication-flows.md#client-credentials). Esto se puede usar para las herramientas de sincronización o las herramientas que procesan usuarios en general y no un usuario específico.
- Use el [flujo con derechos delegados](msal-authentication-flows.md#on-behalf-of) para que una API web llame a una API en nombre del usuario. La aplicación se identifica con las credenciales del cliente para adquirir un token basado en una aserción de usuario (SAML por ejemplo o un token de JWT). Este flujo lo usan las aplicaciones que necesitan acceder a recursos de un usuario determinado en llamadas de servicio a servicio.
- Adquiere tokens mediante el [flujo de código de autorización](msal-authentication-flows.md#authorization-code) en aplicaciones web después de que el usuario inicia sesión mediante la dirección URL de solicitud de autorización. La aplicación OpenID Connect usa este mecanismo normalmente, lo cual permite al usuario iniciar sesión mediante OpenID Connect y acceder, posteriormente, a las API web en nombre del usuario.

## <a name="authentication-results"></a>Resultados de autenticación

Cuando el cliente solicita un token de acceso, Azure AD también devuelve un resultado de autenticación que incluye algunos metadatos sobre el token de acceso. Esta información incluye la hora de expiración del token de acceso y los ámbitos para los que es válido. Estos datos permiten a la aplicación realizar un almacenamiento inteligente en caché de los tokens de acceso sin tener que analizar el mismo token de acceso.  El resultado de la autenticación expone:

- El [token de acceso](access-tokens.md) para que la API web acceda a los recursos. Se trata de una cadena, normalmente una aserción de JWT codificada en base64, pero el cliente no debería mirar nunca dentro del token de acceso. No se garantiza que el formato permanezca estable y se pueda cifrar para el recurso. Los usuarios que escriben código según el contenido de un token de acceso del cliente constituyen una de las fuentes de errores y de interrupciones de la lógica de cliente más habituales.
- El [token de identificador](id-tokens.md) del usuario (se trata de un JWT).
- La expiración del token, que le indica la fecha y hora cuando expira el token.
- El identificador de inquilino indica el inquilino en el que se encontró el usuario. Para los usuarios invitados (escenarios de Azure AD B2B), el identificador de inquilino es el inquilino invitado, no el inquilino único. Cuando el token se entrega en nombre de un usuario, el resultado de la autenticación también contiene información sobre este usuario. Para los flujos de cliente confidenciales en los que se solicitan tokens sin usuario (para la aplicación), esta información de usuario está vacía.
- Los ámbitos para los que el token se emitió.
- El identificador único del usuario.

## <a name="next-steps"></a>Pasos siguientes

Si usa MSAL para Java, obtenga información sobre [Serialización de la memoria caché de tokens personalizada en MSAL para Java](msal-java-token-cache-serialization.md).

Más información sobre el [control de errores y excepciones](msal-handling-exceptions.md).

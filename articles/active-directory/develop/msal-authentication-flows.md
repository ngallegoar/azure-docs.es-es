---
title: Flujos de autenticación de MSAL | Azure
titleSuffix: Microsoft identity platform
description: Obtenga información sobre los flujos y concesiones de autenticación utilizados por la biblioteca de autenticación de Microsoft (MSAL).
services: active-directory
author: mmacy
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 05/18/2020
ms.author: marsma
ms.reviewer: saeeda
ms.custom: aaddev
ms.openlocfilehash: ce81af90baeeda519f1b56d1e10a46923ebd22c2
ms.sourcegitcommit: 318d1bafa70510ea6cdcfa1c3d698b843385c0f6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/21/2020
ms.locfileid: "83772138"
---
# <a name="authentication-flows"></a>Flujos de autenticación

En este artículo se describen los distintos flujos de autenticación que proporciona la biblioteca de autenticación de Microsoft (MSAL).  Estos flujos se pueden usar en una variedad de escenarios de aplicación diferentes.

| Flujo | Descripción | Se usa en|  
| ---- | ----------- | ------- | 
| [Interactivo](#interactive) | Obtiene el token a través de un proceso interactivo que solicita las credenciales al usuario a través de un explorador o una ventana emergente. | [Aplicaciones de escritorio](scenario-desktop-overview.md), [aplicaciones móviles](scenario-mobile-overview.md) |
| [Concesión implícita](#implicit-grant) | Permite a la aplicación obtener tokens sin tener que realizar un intercambio de credenciales de servidor back-end. De esta forma, la aplicación puede iniciar la sesión del usuario, mantenerla y recibir tokens para otras API web, todo dentro del código de cliente de JavaScript.| [Aplicación de página única (SPA)](scenario-spa-overview.md) |
| [Código de autorización](#authorization-code) | Se usa en aplicaciones que están instaladas en un dispositivo para obtener acceso a recursos protegidos, como las API web. Esto permite agregar acceso de inicio de sesión y API a las aplicaciones de escritorio y móviles. | [Aplicaciones de escritorio](scenario-desktop-overview.md), [aplicaciones móviles](scenario-mobile-overview.md), [aplicaciones web](scenario-web-app-call-api-overview.md) | 
| [En nombre de](#on-behalf-of) | Una aplicación invoca un servicio o API web, que, a su vez, debe llamar a otro servicio o API web. La idea es propagar la identidad y los permisos del usuario delegado a través de la cadena de solicitud. | [API web](scenario-web-api-call-api-overview.md) |
| [Credenciales de cliente](#client-credentials) | Permite acceder a los recursos hospedados en la Web mediante la identidad de una aplicación. Se usa principalmente para las interacciones entre servidores que se deben ejecutar en segundo plano, sin la interacción inmediata con un usuario. | [Aplicaciones de demonio](scenario-daemon-overview.md) |
| [Código del dispositivo](#device-code) | Permite a los usuarios iniciar sesión en dispositivos con limitaciones de entrada, como un televisor inteligente, dispositivo IoT o impresora. | [Aplicaciones de escritorio o móviles](scenario-desktop-acquire-token.md#command-line-tool-without-a-web-browser) |
| [Autenticación integrada de Windows](scenario-desktop-acquire-token.md#integrated-windows-authentication) | Permite a los equipos de aplicaciones en equipos unidos a un dominio o a Azure Active Directory (Azure AD) adquirir un token de manera silenciosa (sin ninguna interacción de la interfaz de usuario del usuario).| [Aplicaciones de escritorio o móviles](scenario-desktop-acquire-token.md#integrated-windows-authentication) |
| [Nombre de usuario y contraseña](scenario-desktop-acquire-token.md#username-and-password) | Permite a una aplicación iniciar la sesión del usuario al controlar directamente la contraseña. Este flujo no es recomendable. | [Aplicaciones de escritorio o móviles](scenario-desktop-acquire-token.md#username-and-password) |

## <a name="how-each-flow-emits-tokens-and-codes"></a>Cómo emite cada flujo los tokens y los códigos
 
En función de cómo se compile el cliente, puede usar uno o varios de los flujos de autenticación admitidos por la Plataforma de identidad de Microsoft.  Estos flujos pueden generar diversos tokens (id_tokens, tokens de actualización, tokens de acceso) además de códigos de autorización, y requieren distintos tokens para que funcionen. En este gráfico se ofrece una información general:
 
|Flujo | Requiere | ID_token | de la aplicación Twitter | Token de actualización | código de autorización | 
|-----|----------|----------|--------------|---------------|--------------------|
|[Flujo de código de autorización](v2-oauth2-auth-code-flow.md) | | x | x | x | x|  
|[Flujo implícito](v2-oauth2-implicit-grant-flow.md) | | x        | x    |      |                    |
|[Flujo de OIDC híbrido](v2-protocols-oidc.md#get-access-tokens)| | x  | |          |            x   |
|[Redención de token de actualización](v2-oauth2-auth-code-flow.md#refresh-the-access-token) | Token de actualización | x | x | x| |
|[Flujo en nombre de](v2-oauth2-on-behalf-of-flow.md) | de la aplicación Twitter| x| x| x| |
|[Flujo de código de dispositivo](v2-oauth2-device-code.md) | | x| x| x| |
|[Credenciales de cliente](v2-oauth2-client-creds-grant-flow.md) | | | x (solo aplicación)| | |
 
Los tokens emitidos a través del modo implícito tienen una limitación de longitud debido a que se pasan al explorador a través de la dirección URL (donde `response_mode` es `query` o `fragment`).  Algunos exploradores tienen un límite en el tamaño de la dirección URL que se puede colocar en la barra del explorador y producen un error cuando es demasiado larga.  Por lo tanto, estos tokens no tienen notificaciones `groups` ni `wids`.

## <a name="interactive"></a>Interactive

MSAL admite la capacidad de pedir interactivamente al usuario sus credenciales para iniciar sesión y obtener un token mediante esas credenciales.

![Diagrama del flujo interactivo](media/msal-authentication-flows/interactive.png)

Para más información sobre el uso de MSAL.NET para adquirir tokens de manera interactiva en plataformas específicas, consulte:
- [Xamarin Android](msal-net-xamarin-android-considerations.md)
- [Xamarin iOS](msal-net-xamarin-ios-considerations.md)
- [Plataforma universal de Windows](msal-net-uwp-considerations.md)

Para más información sobre las llamadas interactivas en MSAL.js, consulte [Comportamiento de petición en solicitudes interactivas MSAL.js](msal-js-prompt-behavior.md).

## <a name="implicit-grant"></a>Concesión implícita

MSAL admite el [flujo de concesión implícita de OAuth 2](v2-oauth2-implicit-grant-flow.md), que permite a la aplicación obtener tokens de la plataforma de identidad de Microsoft sin tener que realizar un intercambio de credenciales del servidor back-end. De esta forma, la aplicación puede iniciar la sesión del usuario, mantenerla y recibir tokens para otras API web, todo dentro del código de cliente de JavaScript.

![Diagrama del flujo de concesión implícito](media/msal-authentication-flows/implicit-grant.svg)

Muchas aplicaciones web modernas se crean como aplicaciones cliente, aplicaciones de una sola página, escritas con JavaScript o un marco de SPA, como Angular, Vue.js y React.js. Estas aplicaciones se ejecutan en un explorador web y tienen características de autenticación diferentes que las aplicaciones web de servidor tradicionales. La plataforma de identidad de Microsoft permite que las aplicaciones de página única inicien la sesión de los usuarios y obtengan tokens para acceder a los servicios back-end o API web, mediante el flujo de concesión implícito. El flujo implícito permite a la aplicación obtener los tokens de identificador para representar al usuario autenticado y también acceder a los tokens que se necesitan para llamar a las API protegidas.

Este flujo de autenticación no incluye escenarios de aplicaciones que usan marcos de JavaScript multiplataforma, como Electron y React-Native, ya que requieren más funcionalidades para la interacción con las plataformas nativas.

## <a name="authorization-code"></a>Código de autorización

MSAL admite la [concesión de código de autorización de OAuth 2](v2-oauth2-auth-code-flow.md). Esta concesión se puede usar en aplicaciones que están instaladas en un dispositivo para obtener acceso a recursos protegidos, como las API web. Esto permite agregar acceso de inicio de sesión y API a las aplicaciones de escritorio y móviles. 

Cuando los usuarios inician sesión en aplicaciones web (sitios web), la aplicación web recibe un código de autorización.  El código de autorización se canjea para adquirir un token para llamar a las API web. En aplicaciones web ASP.NET y ASP.NET Core, el único objetivo del objeto `AcquireTokenByAuthorizationCode` es agregar un token a la memoria caché de tokens. La aplicación luego puede usar el token (normalmente en los controladores, que simplemente obtienen un token de una API mediante `AcquireTokenSilent`).

![Diagrama del flujo de código de autorización](media/msal-authentication-flows/authorization-code.png)

En el diagrama anterior, la aplicación hace lo siguiente:

1. Solicita un código de autorización, que se canjea por un token de acceso.
2. Usa el token de acceso para llamar a una API web.

### <a name="considerations"></a>Consideraciones

- Puede usar el código de autorización una sola vez para canjear un token. No intente adquirir un token varias veces con el mismo código de autorización (la especificación del protocolo estándar lo prohíbe explícitamente). Si canjea el código varias veces de manera intencionada o porque no es consciente de que un marco también lo hace, obtendrá el siguiente error: `AADSTS70002: Error validating credentials. AADSTS54005: OAuth2 Authorization code was already redeemed, please retry with a new valid code or use an existing refresh token.`

- Si está escribiendo una aplicación ASP.NET o ASP.NET Core, esto puede ocurrir si no indica al marco que ya ha canjeado el código de autorización. Para ello, debe llamar al método `context.HandleCodeRedemption()` del controlador de eventos `AuthorizationCodeReceived`.

- Evite compartir el token de acceso con ASP.NET, lo que podría impedir que el consentimiento incremental se realice correctamente. Para más información, consulte el [problema 693](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/issues/693).

## <a name="on-behalf-of"></a>En nombre de

MSAL admite el [flujo autenticación "en nombre de" de OAuth 2](v2-oauth2-on-behalf-of-flow.md).  Este flujo se usa cuando una aplicación invoca un servicio o API web, que, a su vez, debe llamar a otro servicio o API web. La idea es propagar la identidad y los permisos del usuario delegado a través de la cadena de solicitud. Para que el servicio de nivel intermedio realice solicitudes autenticadas al servicio de bajada, debe proteger un token de acceso de la plataforma de identidad de Microsoft en nombre del usuario.

![Diagrama del flujo "en nombre de"](media/msal-authentication-flows/on-behalf-of.png)

En el diagrama anterior:

1. La aplicación adquiere un token de acceso para la API web.
2. Un cliente (web, aplicación de escritorio, aplicación móvil o una página) llama a una API web protegida, agregando el token de acceso como token de portador en el encabezado de autenticación de la solicitud HTTP. La API web autentica al usuario.
3. Cuando el cliente llama a la API web, esta solicita otro token en nombre del usuario.  
4. La API web protegida usa este token para llamar a una API web descendente en nombre del usuario.  La API web también puede solicitar tokens más tarde para otras API descendentes (pero todavía en nombre del mismo usuario).

## <a name="client-credentials"></a>Credenciales de cliente

MSAL admite el [flujo de credenciales de cliente de OAuth 2](v2-oauth2-client-creds-grant-flow.md). Este flujo le permite acceder a los recursos hospedados en la Web mediante la identidad de una aplicación. Este tipo de concesión se usa principalmente para las interacciones entre servidores que se deben ejecutar en segundo plano, sin la interacción inmediata con un usuario. Estos tipos de aplicaciones suelen denominarse demonios o cuentas de servicio. 

El flujo de concesión de credenciales de cliente permite que un servicio web (cliente confidencial) use sus propias credenciales para autenticarse al llamar a otro servicio web, en lugar de suplantar a un usuario. En este escenario, el cliente suele ser un servicio web de nivel intermedio, un servicio demonio o un sitio web. Para conseguir un mayor nivel de control, la plataforma de identidad de Microsoft también permite que el servicio que realiza la llamada use un certificado (en lugar de un secreto compartido) como credencial.

> [!NOTE]
> El flujo de cliente confidencial no está disponible en las plataformas móviles (UWP, Xamarin.iOS y Xamarin.Android), porque estas solo admiten aplicaciones de cliente públicas. Las aplicaciones de cliente públicas no saben cómo demostrar la identidad de la aplicación al proveedor de identidades. Una conexión segura se puede lograr en los servidores back-end de aplicación web o API web mediante la implementación de un certificado.

MSAL.NET admite dos tipos de credenciales de cliente. Estas credenciales de cliente deben registrarse en Azure AD. Las credenciales se pasan a los constructores de la aplicación cliente confidencial en el código.

### <a name="application-secrets"></a>Secretos de aplicación

![Diagrama de cliente confidencial con contraseña](media/msal-authentication-flows/confidential-client-password.png)

En el diagrama anterior, la aplicación hace lo siguiente:

1. Adquiere un token al usar un secreto de aplicación o credenciales de contraseña.
2. Usa el token para hacer solicitudes al recurso.

### <a name="certificates"></a>Certificados

![Diagrama de cliente confidencial con certificado](media/msal-authentication-flows/confidential-client-certificate.png)

En el diagrama anterior, la aplicación hace lo siguiente:

1. Adquiere un token mediante el uso de credenciales de certificado.
2. Usa el token para hacer solicitudes al recurso.

Estas credenciales de cliente deben:
- Registrarse en Azure AD.
- Pasarse en la construcción de la aplicación de cliente confidencial en el código.

## <a name="device-code"></a>Código del dispositivo

MSAL admite el [flujo de código de dispositivo de OAuth 2](v2-oauth2-device-code.md), lo que permite que los usuarios inicien sesión en dispositivos con limitaciones de entrada, como un televisor inteligente, un dispositivo IoT o una impresora. La autenticación interactiva con Azure AD requiere un explorador web. El flujo de código de dispositivo permite al usuario usar otro dispositivo (por ejemplo, otro equipo o un teléfono móvil) para iniciar sesión de forma interactiva, donde el dispositivo o sistema operativo no proporciona un explorador web.

Al usar flujo de código de dispositivo, la aplicación obtiene los tokens a través de un proceso de dos pasos diseñado especialmente para estos dispositivos o sistemas operativos. Entre algunos ejemplos de estas aplicaciones se incluye aquellas que se ejecutan en dispositivos de IoT o herramientas de línea de comandos (CLI). 

![Diagrama del flujo de código de dispositivo](media/msal-authentication-flows/device-code.png)

En el diagrama anterior:

1. Cada vez que se requiera autenticación del usuario, la aplicación proporciona un código y pide al usuario que use otro dispositivo (por ejemplo, un smartphone conectado a Internet) para ir a una dirección URL (por ejemplo, `https://microsoft.com/devicelogin`). Luego, se le pide al usuario que especifique el código, y el usuario pasa por una experiencia de autenticación normal, como peticiones de consentimiento y [autenticación multifactor](../authentication/concept-mfa-howitworks.md), si es necesario.

2. Tras una autenticación correcta, la aplicación de línea de comandos recibe los tokens necesarios a través de un canal posterior y los usa para realizar las llamadas a la API web que necesita.

### <a name="constraints"></a>Restricciones

- El flujo de código de dispositivo solo está disponible en las aplicaciones de cliente públicas.
- La autoridad que se pasa al construir la aplicación cliente pública debe ser una de las siguientes:
  - Con inquilino (del tipo `https://login.microsoftonline.com/{tenant}/` donde `{tenant}` es el GUID que representa el identificador del inquilino o un dominio asociado con el inquilino).
  - Para las cuentas profesionales y educativas (`https://login.microsoftonline.com/organizations/`).
- El punto de conexión de Azure AD v2.0 aún no admite cuentas de Microsoft personales v2.0 de Azure AD (no se pueden usar los inquilinos `/common` ni `/consumers`).

## <a name="integrated-windows-authentication"></a>Autenticación integrada de Windows

MSAL admite la autenticación integrada de Windows (IWA) para aplicaciones de escritorio o aplicaciones móviles que se ejecutan en un equipo Windows unido a un dominio o a Azure AD. Mediante IWA, estas aplicaciones pueden adquirir un token de forma silenciosa (sin ninguna interacción de la interfaz de usuario del usuario). 

![Diagrama de la autenticación integrada de Windows](media/msal-authentication-flows/integrated-windows-authentication.png)

En el diagrama anterior, la aplicación hace lo siguiente:

1. Adquiere un token mediante el uso de la autenticación integrada de Windows.
2. Usa el token para hacer solicitudes al recurso.

### <a name="constraints"></a>Restricciones

IWA admite solo usuarios federados, lo que significa usuarios creados en Active Directory y respaldados por Azure AD. Los usuarios creados directamente en Azure AD, sin el respaldo de Active Directory (usuarios administrados) no pueden usar este flujo de autenticación. Esta limitación no afecta al [flujo de usuario y contraseña](#usernamepassword).

IWA es para las aplicaciones escritas para las plataformas .NET Framework, .NET Core y Plataforma universal de Windows.

IWA no omite la autenticación multifactor. Si se ha configurado la autenticación multifactor, podría producirse un error en IWA si se requiere un desafío de autenticación multifactor. La autenticación multifactor requiere la interacción del usuario.

No se puede controlar cuándo el proveedor de identidades solicita que se realice la autenticación en dos fases. Esto lo hace el administrador de inquilinos. Normalmente, la autenticación en dos fases se requiere al iniciar sesión desde un país o región diferente, cuando no está conectado a través de una VPN a una red corporativa y, a veces, incluso cuando se conecta a través de una VPN. Azure AD usa la inteligencia artificial para aprender continuamente si se requiere la autenticación en dos fases. Si IWA produce un error, debe revertir a una [solicitud interactiva de usuario] (#interactive).

La autoridad que se pasa al construir la aplicación cliente pública debe ser una de las siguientes:
- Con inquilino (del tipo `https://login.microsoftonline.com/{tenant}/` donde `tenant` es el GUID que representa el identificador del inquilino o un dominio asociado con el inquilino).
- Para las cuentas profesionales y educativas (`https://login.microsoftonline.com/organizations/`). No se admiten cuentas de Microsoft personales (no se pueden usar los inquilinos `/common` ni `/consumers`).

Dado que IWA es un flujo silencioso, debe cumplirse uno de los siguientes requisitos:
- El usuario de la aplicación debe haber consentido anteriormente para usar la aplicación. 
- El administrador de inquilinos debe haber consentido anteriormente a todos los usuarios en el inquilino para que usen la aplicación.

Esto significa que una de las siguientes condiciones es verdad:
- Usted, como desarrollador, seleccionó **Conceder** en Azure Portal para usted mismo.
- Un administrador de inquilinos seleccionó la pestaña **Conceder o revocar consentimiento del administrador para {dominio del inquilino}** en la pestaña **Permisos de API** del registro para la aplicación (consulte [Adición de credenciales a la aplicación web](quickstart-configure-app-access-web-apis.md#add-permissions-to-access-web-apis)).
- Se ha proporcionado una manera para que los usuarios den su consentimiento a la aplicación (consulte [Solicitud de consentimiento de usuario individual](v2-permissions-and-consent.md#requesting-individual-user-consent)).
- Se ha proporcionado una forma para que el administrador de inquilinos dé su consentimiento para la aplicación (consulte [Consentimiento de administrador](v2-permissions-and-consent.md#requesting-consent-for-an-entire-tenant)).

El flujo de IWA está habilitado para el escritorio. NET, .NET Core y aplicaciones de la Plataforma universal de Windows. En .NET Core, debe proporcionar el nombre de usuario a IWA, porque .NET Core no puede obtener los nombres de usuario del sistema operativo.
  
Para más información sobre el consentimiento, consulte [Permiso y consentimiento de v2.0](v2-permissions-and-consent.md).

## <a name="usernamepassword"></a>Nombre de usuario y contraseña

MSAL admite la [concesión de credenciales de contraseña de propietario de recurso OAuth 2](v2-oauth-ropc.md), que permite que una aplicación inicie la sesión del usuario al controlar directamente la contraseña. En la aplicación de escritorio, puede usar el flujo de usuario y contraseña para adquirir un token de forma silenciosa. No se requiere ninguna interfaz de usuario cuando se usa la aplicación.

![Diagrama del flujo de usuario y contraseña](media/msal-authentication-flows/username-password.png)

En el diagrama anterior, la aplicación hace lo siguiente:

1. Adquiere un token al enviar el nombre de usuario y contraseña al proveedor de identidades.
2. Llama a una API web mediante el token.

> [!WARNING]
> Este flujo no es recomendable. Requiere un alto grado de confianza y exposición del usuario.  Este flujo solo se debe usar cuando no se pueden usar otros flujos más seguros. Para más información, consulte [What's the solution to the growing problem of passwords?](https://news.microsoft.com/features/whats-solution-growing-problem-passwords-says-microsoft/) (¿Cuál es la solución al creciente problema de las contraseñas?). 

El flujo preferido para adquirir un token de forma silenciosa en equipos unidos a un dominio de Windows es la [autenticación integrada de Windows](#integrated-windows-authentication). En caso contrario, también puede usar el [flujo de código de dispositivo](#device-code).

Aunque esto es útil en algunos casos (escenarios de DevOps), si quiere usar el nombre de usuario y contraseña en escenarios interactivos donde proporciona su propia interfaz de usuario, intente evitar esta situación. Mediante el uso de nombre de usuario y contraseña:
- Los usuarios que necesitan realizar la autenticación multifactor no podrán iniciar sesión (ya que no hay ninguna interacción).
- Los usuarios no podrán realizar el inicio de sesión único.

### <a name="constraints"></a>Restricciones

Aparte de la [las restricciones de la autenticación integrada de Windows](#integrated-windows-authentication), también se aplican las siguientes restricciones:

- El flujo de usuario y contraseña no es compatible con el acceso condicional ni la autenticación multifactor. Como consecuencia, si la aplicación se ejecuta en un inquilino de Azure AD donde el administrador de inquilinos requiere la autenticación multifactor, no se puede usar este flujo. Muchas organizaciones hacen eso.
- Funciona únicamente para las cuentas profesionales y educativas (no las cuentas de Microsoft).
- El flujo está disponible en el escritorio de .NET y .NET Core, pero no en la Plataforma universal de Windows.

### <a name="azure-ad-b2c-specifics"></a>Información específica sobre Azure AD B2C

Para obtener más información sobre el uso de ROPC en MSAL.NET y Azure AD B2C, consulte [Credenciales de contraseña de propietario de recursos con Azure AD B2C](msal-net-aad-b2c-considerations.md#resource-owner-password-credentials-ropc).

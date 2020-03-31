---
title: Inicialización de aplicaciones cliente de MSAL.js | Azure
titleSuffix: Microsoft identity platform
description: Obtenga información sobre la inicialización de aplicaciones cliente mediante la biblioteca de autenticación de Microsoft para JavaScript (MSAL.js).
services: active-directory
author: mmacy
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 04/12/2019
ms.author: marsma
ms.reviewer: saeeda
ms.custom: aaddev
ms.openlocfilehash: a0a2c5fc971c3f1f3283d95c5617bdf1e88a6a58
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "77084030"
---
# <a name="initialize-client-applications-using-msaljs"></a>Inicialización de aplicaciones cliente con MSAL.js
En este artículo se describe la inicialización de la biblioteca de autenticación de Microsoft para JavaScript (MSAL.js) con una instancia de una aplicación de agente de usuario. La aplicación de agente de usuario es una forma de aplicación cliente pública en la que el código cliente se ejecuta en un agente de usuario, como un explorador web. Estos clientes no almacenan secretos, ya que el contexto del explorador es de acceso público. Para obtener más información sobre los tipos de aplicaciones cliente y las opciones de configuración de la aplicación, lea la [Introducción](msal-client-applications.md).

## <a name="prerequisites"></a>Prerrequisitos
Antes de inicializar una aplicación, primero tendrá que [registrarla en Azure Portal](scenario-spa-app-registration.md) para que se pueda integrar con la plataforma de identidad de Microsoft. Después del registro, es posible que necesite la información siguiente (que puede encontrar en Azure Portal):

- El identificador de cliente (una cadena que representa un GUID para la aplicación)
- La URL del proveedor de identidades (la instancia) y la audiencia de inicio de sesión para la aplicación. De forma conjunta, estos dos parámetros se conocen como la autoridad.
- El identificador del inquilino si va a escribir una aplicación de línea de negocio exclusivamente para la organización (también denominada aplicación de un único inquilino).
- Para las aplicaciones web, también tendrá que establecer el valor redirectUri, en el que el proveedor de identidades devolverá los tokens de seguridad a la aplicación.

## <a name="initializing-applications"></a>Inicialización de aplicaciones

Puede usar MSAL.js como se indica a continuación en una aplicación JavaScript o Typescript sin formato. Para inicializar el contexto de autenticación de MSAL, cree una instancia de `UserAgentApplication` con un objeto de configuración. La configuración mínima necesaria para inicializar MSAL.js es el valor clientID de la aplicación, que debe obtener en el portal de registro de la aplicación.

Para los métodos de autenticación con flujos de redirección (`loginRedirect` y `acquireTokenRedirect`), tendrá que registrar de forma explícita una devolución de llamada para los casos correctos o con error a través del método `handleRedirectCallback()`. Esto es necesario ya que los flujos de redirección no devuelven promesas como hacen los métodos con una experiencia emergente.

```javascript
// Configuration object constructed
const config = {
    auth: {
        clientId: "abcd-ef12-gh34-ikkl-ashdjhlhsdg"
    }
}

// create UserAgentApplication instance
const myMSALObj = new UserAgentApplication(config);

function authCallback(error, response) {
    //handle redirect response
}

// (optional when using redirect methods) register redirect call back for Success or Error
myMSALObj.handleRedirectCallback(authCallback);
```

MSAL.js está diseñado para tener una sola instancia y configuración de `UserAgentApplication` para representar un único contexto de autenticación. No se recomienda el uso de varias instancias, ya que generan entradas de caché y comportamientos en conflicto en el explorador.

## <a name="configuration-options"></a>Opciones de configuración

MSAL.js tiene un objeto de configuración (mostrado a continuación) que proporciona una agrupación de opciones configurables disponibles para crear una instancia de `UserAgentApplication`.

```javascript
type storage = "localStorage" | "sessionStorage";

// Protocol Support
export type AuthOptions = {
    clientId: string;
    authority?: string;
    validateAuthority?: boolean;
    redirectUri?: string | (() => string);
    postLogoutRedirectUri?: string | (() => string);
    navigateToLoginRequestUrl?: boolean;
};

// Cache Support
export type CacheOptions = {
    cacheLocation?: CacheLocation;
    storeAuthStateInCookie?: boolean;
};

// Library support
export type SystemOptions = {
    logger?: Logger;
    loadFrameTimeout?: number;
    tokenRenewalOffsetSeconds?: number;
    navigateFrameWait?: number;
};

// Developer App Environment Support
export type FrameworkOptions = {
    isAngular?: boolean;
    unprotectedResources?: Array<string>;
    protectedResourceMap?: Map<string, Array<string>>;
};

// Configuration Object
export type Configuration = {
    auth: AuthOptions,
    cache?: CacheOptions,
    system?: SystemOptions,
    framework?: FrameworkOptions
};
```

A continuación se muestra el conjunto total de opciones configurables que se admiten actualmente en el objeto de configuración:

- **clientID**: Necesario. El identificador de cliente de la aplicación, que debe obtener del portal de registro de la aplicación.

- **authority**: Opcional. Una dirección URL que indica un directorio desde el que MSAL puede solicitar tokens. El valor predeterminado es `https://login.microsoftonline.com/common`.
    * En Azure AD, tiene el formato https://&lt;instancia&gt;/&lt;audiencia&gt;, donde &lt;instancia&gt; es el dominio del proveedor de identidades (por ejemplo, `https://login.microsoftonline.com`) y &lt;audiencia&gt; es un identificador que representa la audiencia de inicio de sesión. Esto puede ser los valores siguientes:
        * `https://login.microsoftonline.com/<tenant>`: inquilino es un dominio asociado al inquilino, como contoso.onmicrosoft.com, o bien el GUID que representa la propiedad `TenantID` del directorio que solo se usa para el inicio de sesión de los usuarios de una organización específica.
        * `https://login.microsoftonline.com/common`: se usa para el inicio de sesión de los usuarios con cuentas profesionales y educativas, o una cuenta personal de Microsoft.
        * `https://login.microsoftonline.com/organizations/`: se usa para el inicio de sesión de los usuarios con cuentas profesionales y educativas.
        * `https://login.microsoftonline.com/consumers/`: se usa para el inicio de sesión de los usuarios solo con una cuenta personal de Microsoft (activa).
    * En Azure AD B2C, tiene el formato `https://<instance>/tfp/<tenant>/<policyName>/`, donde instancia es el dominio de Azure AD B2C (es decir, {nombre-del-inquilino}.b2clogin.com), inquilino es el nombre del inquilino de Azure AD B2C (es decir, {nombre-del-inquilino}.onmicrosoft.com) y policyName es el nombre de la directiva de B2C que se va a aplicar.


- **validateAuthority**: Opcional.  Valida el emisor de tokens. El valor predeterminado es `true`. Para las aplicaciones B2C, como el valor de autoridad es conocido y puede ser diferente según la directiva, la validación de la autoridad no funcionará y se tiene que establecer en `false`.

- **redirectUri**: Opcional.  El URI de redireccionamiento de la aplicación, adonde la aplicación puede enviar y recibir las respuestas de autenticación. Debe coincidir exactamente con uno de los URI de redirección que registró en el portal. Su valor predeterminado es `window.location.href`.

- **postLogoutRedirectUri**: Opcional.  Redirige al usuario a `postLogoutRedirectUri` después de cerrar sesión. El valor predeterminado es `redirectUri`.

- **navigateToLoginRequestUrl**: Opcional. La capacidad de desactivar la navegación predeterminada a la página de inicio después de iniciar sesión. El valor predeterminado es true. Esto solo se usa para los flujos de redirección.

- **cacheLocation**: Opcional.  Establece el almacenamiento del explorador en `localStorage` o `sessionStorage`. El valor predeterminado es `sessionStorage`.

- **storeAuthStateInCookie**: Opcional.  Esta marca se introdujo en MSAL.js v0.2.2 como una corrección para los [problemas de bucle de autenticación](https://github.com/AzureAD/microsoft-authentication-library-for-js/wiki/Known-issues-on-IE-and-Edge-Browser#1-issues-due-to-security-zones) en Microsoft Internet Explorer y Microsoft Edge. Habilite la marca `storeAuthStateInCookie` en True para aprovechar las ventajas de esta corrección. Cuando se habilita, MSAL.js almacenará el estado de la solicitud de autenticación necesario para la validación de los flujos de autenticación en las cookies del explorador. De manera predeterminada, esta marca se establece en `false`.

- **logger**: Opcional.  Un objeto Logger con una instancia de devolución de llamada que puede proporcionar el desarrollador para consumir y publicar registros de una manera personalizada. Para obtener más información sobre cómo pasar el objeto de registrador, vea [Registro con msal.js](msal-logging.md).

- **loadFrameTimeout**: Opcional.  El número de milisegundos de inactividad antes de que se deba considerar que una respuesta de renovación de token de Azure AD ha agotado el tiempo de espera. El valor predeterminado es 6 segundos.

- **tokenRenewalOffsetSeconds**: Opcional. El número de milisegundos que establece la ventana de desplazamiento necesario para renovar el token antes de que expire. El valor predeterminado es 300 milisegundos.

- **navigateFrameWait**: Opcional. El número de milisegundos que establece el tiempo de espera antes de que los iframe ocultos naveguen a su destino. El valor predeterminado es 500 milisegundos.

Solo son aplicables para pasarse desde la biblioteca de contenedores Angular de MSAL:
- **unprotectedResources**: Opcional.  Matriz de URI que son recursos desprotegidos. MSAL no asociará un token a las solicitudes salientes que tengan estos URI. Su valor predeterminado es `null`.

- **protectedResourceMap**: Opcional.  Es la asignación de recursos a los ámbitos que usa MSAL para asociar de forma automática tokens de acceso en las llamadas de API web. Se obtiene un único token de acceso para el recurso. Por tanto, puede asignar una ruta de acceso de recurso específica de esta forma: {"https://graph.microsoft.com/v1.0/me", ["user.read"]}, o bien la dirección URL de la aplicación del recurso como: {"https://graph.microsoft.com/", ["user.read", "mail.send"]}. Esto es necesario para las llamadas a CORS. Su valor predeterminado es `null`.

---
title: Paso del estado personalizado en solicitudes de autenticación (MSAL.js) | Azure
titleSuffix: Microsoft identity platform
description: Aprenda a pasar un valor de estado personalizado en la solicitud de autenticación con la Biblioteca de autenticación de Microsoft para JavaScript (MSAL.js).
services: active-directory
author: mmacy
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 01/16/2020
ms.author: marsma
ms.reviewer: saeeda
ms.custom: aaddev
ms.openlocfilehash: 1c05956f83ad3a6491627be8916fac2c8be2b7ff
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "77084940"
---
# <a name="pass-custom-state-in-authentication-requests-using-msaljs"></a>Paso del estado personalizado en solicitudes de autenticación con MSAL.js

El parámetro *state*, tal como se define en OAuth 2.0, se incluye en una solicitud de autenticación y también se devuelve en la respuesta del token para evitar ataques de falsificación de la solicitud entre sitios. De forma predeterminada, la Biblioteca de autenticación de Microsoft para JavaScript (MSAL.js) pasa un valor único del parámetro *state* generado al azar en las solicitudes de autenticación.

El parámetro state también se puede usar para codificar la información de estado de la aplicación antes de la redirección. Puede pasar el estado del usuario en la aplicación, por ejemplo, la página o vista en el que estuviera, como entrada para este parámetro. La biblioteca MSAL.js le permite pasar el estado personalizado como parámetro de estado en el objeto `Request`:

```javascript
// Request type
export type AuthenticationParameters = {
    scopes?: Array<string>;
    extraScopesToConsent?: Array<string>;
    prompt?: string;
    extraQueryParameters?: QPDict;
    claimsRequest?: string;
    authority?: string;
    state?: string;
    correlationId?: string;
    account?: Account;
    sid?: string;
    loginHint?: string;
    forceRefresh?: boolean;
};
```

> [!Note]
> Si desea omitir un token almacenado en caché e ir al servidor, pase el valor booleano `forceRefresh` al objeto AuthenticationParameters usado para realizar una solicitud de token o inicio de sesión.
> `forceRefresh` no debe usarse de forma predeterminada, debido al impacto en el rendimiento de la aplicación.
> La experiencia de los usuarios será mejor si se utiliza la caché.
> Solo se debe omitir la caché en los casos en los que se sepa que los datos almacenados actualmente en caché no tienen información actualizada.
> Por ejemplo, en el caso de una herramienta de administración que agrega roles a un usuario que necesita obtener un nuevo token con roles actualizados.

Por ejemplo:

```javascript
let loginRequest = {
    scopes: ["user.read", "user.write"],
    state: "page_url"
}

myMSALObj.loginPopup(loginRequest);
```

El estado pasado se anexa al GUID único establecido por MSAL.js al enviar la solicitud. Cuando se devuelve la respuesta, MSAL.js busca una coincidencia de estado y, a continuación, devuelve el estado personalizado pasado en el objeto `Response` como `accountState`.

```javascript
export type AuthResponse = {
    uniqueId: string;
    tenantId: string;
    tokenType: string;
    idToken: IdToken;
    accessToken: string;
    scopes: Array<string>;
    expiresOn: Date;
    account: Account;
    accountState: string;
};
```

Para más información, lea sobre cómo [crear una aplicación de página única (SPA)](scenario-spa-overview.md) mediante MSAL.js.
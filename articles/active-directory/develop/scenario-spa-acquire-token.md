---
title: Adquisición de un token para llamar a una API web (aplicaciones de página única) - Plataforma de identidad de Microsoft | Azure
description: Aprenda a crear una aplicación de página única (adquirir un token para llamar a una API).
services: active-directory
author: negoe
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 08/20/2019
ms.author: negoe
ms.custom: aaddev
ms.openlocfilehash: eeba01a609a1a21ed564c0b9cb78a28a4ad5c95a
ms.sourcegitcommit: d187fe0143d7dbaf8d775150453bd3c188087411
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80882325"
---
# <a name="single-page-application-acquire-a-token-to-call-an-api"></a>Aplicación de página única: Adquisición de un token para llamar a una API

El patrón para la adquisición de tokens de API con MSAL.js consiste en intentar realizar primero una solicitud de token silenciosa utilizando el método `acquireTokenSilent`. Cuando se llama a este método, la biblioteca comprueba primero la caché en el almacenamiento del explorador para ver si existe un token válido y, a continuación, lo devuelve. Cuando no hay ningún token válido en la caché, envía una solicitud de token silenciosa a Azure Active Directory (Azure AD) desde un iframe oculto. Este método también permite que la biblioteca renueve los tokens. Para más información sobre los valores de inicio de sesión único y duración del token en Azure AD, consulte la [vigencia de los tokens](active-directory-configurable-token-lifetimes.md).

Las solicitudes de token silenciosas a Azure AD pueden producir un error por algún motivo, como una sesión de Azure AD que haya expirado o un cambio de contraseña. En ese caso, puede invocar uno de los métodos interactivos (que se le pedirá al usuario) para adquirir tokens:

* [Ventana emergente](#acquire-a-token-with-a-pop-up-window), mediante `acquireTokenPopup`
* [Redirección](#acquire-a-token-with-a-redirect), mediante `acquireTokenRedirect`

## <a name="choose-between-a-pop-up-or-redirect-experience"></a>Decisión entre una experiencia de ventana emergente o redirección

 No puede usar juntos los métodos de ventana emergente y redirección en la aplicación. La elección de una u otra experiencia depende del flujo de la aplicación:

* Si no quiere que los usuarios salgan de la página principal de la aplicación durante la autenticación, se recomienda el método de ventana emergente. Dado que la redirección de la autenticación tiene lugar en una ventana emergente, se conserva el estado de la aplicación principal.

* Si los usuarios tienen restricciones del explorador o directivas donde las ventanas emergentes están deshabilitadas, puede usar los métodos de redirección. Use el método de redirección con Internet Explorer, ya que hay [problemas conocidos con las ventanas emergentes en este explorador](https://github.com/AzureAD/microsoft-authentication-library-for-js/wiki/Known-issues-on-IE-and-Edge-Browser).

Al crear la solicitud de token de acceso, puede establecer los ámbitos de API que quiere que incluya el token de acceso. Tenga en cuenta que puede que no se concedan todos los ámbitos solicitados en el token de acceso. Depende del consentimiento del usuario.

## <a name="acquire-a-token-with-a-pop-up-window"></a>Adquisición de un token con una ventana emergente

# <a name="javascript"></a>[JavaScript](#tab/javascript)

En el código siguiente se combina el patrón descrito anteriormente con los métodos de una experiencia de ventana emergente:

```javascript
const accessTokenRequest = {
    scopes: ["user.read"]
}

userAgentApplication.acquireTokenSilent(accessTokenRequest).then(function(accessTokenResponse) {
    // Acquire token silent success
    // Call API with token
    let accessToken = accessTokenResponse.accessToken;
}).catch(function (error) {
    //Acquire token silent failure, and send an interactive request
    if (error.errorMessage.indexOf("interaction_required") !== -1) {
        userAgentApplication.acquireTokenPopup(accessTokenRequest).then(function(accessTokenResponse) {
            // Acquire token interactive success
        }).catch(function(error) {
            // Acquire token interactive failure
            console.log(error);
        });
    }
    console.log(error);
});
```

# <a name="angular"></a>[Angular](#tab/angular)

El contenedor MSAL Angular proporciona el interceptor HTTP, que adquiere de forma automática y silenciosa los tokens de acceso y los asocia a las solicitudes HTTP de las API.

Puede especificar los ámbitos de las API en la opción de configuración `protectedResourceMap`. `MsalInterceptor` solicitará estos ámbitos cuando adquiera automáticamente tokens.

```javascript
// app.module.ts
@NgModule({
  declarations: [
    // ...
  ],
  imports: [
    // ...
    MsalModule.forRoot({
      auth: {
        clientId: 'Enter_the_Application_Id_Here',
      }
    },
    {
      popUp: !isIE,
      consentScopes: [
        'user.read',
        'openid',
        'profile',
      ],
      protectedResourceMap: [
        ['https://graph.microsoft.com/v1.0/me', ['user.read']]
      ]
    })
  ],
  providers: [
    {
      provide: HTTP_INTERCEPTORS,
      useClass: MsalInterceptor,
      multi: true
    }
  ],
  bootstrap: [AppComponent]
})
export class AppModule { }
```

Para conocer el éxito o fracaso de la adquisición de tokens silenciosa, MSAL Angular proporciona devoluciones de llamada a las que puede suscribirse. También es importante recordar esta información si quiere cancelar la suscripción.

```javascript
// In app.component.ts
 ngOnInit() {
    this.subscription=  this.broadcastService.subscribe("msal:acquireTokenFailure", (payload) => {
    });
}

ngOnDestroy() {
   this.broadcastService.getMSALSubject().next(1);
   if (this.subscription) {
     this.subscription.unsubscribe();
   }
 }
```

Como alternativa, también puede adquirir tokens de forma explícita mediante los métodos de adquisición de token, tal como se describe en la biblioteca principal MSAL.js.

---

## <a name="acquire-a-token-with-a-redirect"></a>Adquisición de un token con una redirección

# <a name="javascript"></a>[JavaScript](#tab/javascript)

El siguiente patrón es como se describió anteriormente, pero se muestra con un método de redirección para adquirir tokens de manera interactiva. Como se mencionó anteriormente, tendrá que registrar la devolución de llamada de redirección.

```javascript
function authCallback(error, response) {
    // Handle redirect response
}

userAgentApplication.handleRedirectCallback(authCallback);

const accessTokenRequest: AuthenticationParameters = {
    scopes: ["user.read"]
}

userAgentApplication.acquireTokenSilent(accessTokenRequest).then(function(accessTokenResponse) {
    // Acquire token silent success
    // Call API with token
    let accessToken = accessTokenResponse.accessToken;
}).catch(function (error) {
    //Acquire token silent failure, and send an interactive request
    console.log(error);
    if (error.errorMessage.indexOf("interaction_required") !== -1) {
        userAgentApplication.acquireTokenRedirect(accessTokenRequest);
    }
});
```

## <a name="request-optional-claims"></a>Solicitud de notificaciones opcionales

Puede usar notificaciones opcionales con los siguientes fines:

- Incluir notificaciones adicionales en los tokens de la aplicación.
- Cambiar el comportamiento de ciertas notificaciones que Azure AD devuelve en tokens.
- Agregar notificaciones personalizadas para la aplicación y acceder a ellas.

Para solicitar notificaciones opcionales en `IdToken`, puede enviar un objeto de notificaciones en cadena al campo `claimsRequest` de la clase `AuthenticationParameters.ts`.

```javascript
"optionalClaims":
   {
      "idToken": [
            {
                  "name": "auth_time",
                  "essential": true
             }
      ],

var request = {
    scopes: ["user.read"],
    claimsRequest: JSON.stringify(claims)
};

myMSALObj.acquireTokenPopup(request);
```

Para más información, consulte [Notificaciones opcionales](active-directory-optional-claims.md).

# <a name="angular"></a>[Angular](#tab/angular)

Este código es igual al que se ha descrito anteriormente.

---

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Llamada a una API web](scenario-spa-call-api.md)

---
title: 'Inicio y cierre de sesión en una aplicación de página única: Plataforma de identidad de Microsoft | Azure'
description: Aprenda a crear una aplicación de página única (inicio de sesión)
services: active-directory
documentationcenter: dev-center-name
author: navyasric
manager: CelesteDG
editor: ''
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/11/2020
ms.author: nacanuma
ms.custom: aaddev
ms.openlocfilehash: eb75aa53051e7e3c424ffe131cda61324fe86b1a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "77159971"
---
# <a name="single-page-application-sign-in-and-sign-out"></a>Aplicación de página única: Inicio y cierre de sesión

Aprenda a agregar el inicio de sesión al código de la aplicación de página única.

Antes de obtener tokens para acceder a las API de la aplicación, necesita un contexto de usuario autenticado. Puede iniciar la sesión de los usuarios en la aplicación con MSAL.js de dos maneras:

* [Ventana emergente](#sign-in-with-a-pop-up-window), mediante el método `loginPopup`
* [Redirección](#sign-in-with-redirect), mediante el método `loginRedirect`

Opcionalmente, también puede pasar los ámbitos de las API para las que necesita que el usuario dé su consentimiento en el momento de iniciar sesión.

> [!NOTE]
> Si su aplicación ya tiene acceso a un contexto de usuario autenticado o un token de identificación, puede omitir el paso de inicio de sesión y adquirir los tokens directamente. Para más información, consulte [Inicio de sesión único sin inicio de sesión en MSAL.js](msal-js-sso.md#sso-without-msaljs-login).

## <a name="choosing-between-a-pop-up-or-redirect-experience"></a>Elegir entre una experiencia de elemento emergente o redireccionamiento

No puede usar juntos los métodos de ventana emergente y redirección en la aplicación. La elección de una u otra experiencia depende del flujo de la aplicación:

* Si no quiere que los usuarios salgan de la página principal de la aplicación durante la autenticación, se recomienda el método de ventana emergente. Dado que la redirección de la autenticación tiene lugar en una ventana emergente, se conserva el estado de la aplicación principal.

* Si los usuarios tienen restricciones del explorador o directivas donde las ventanas emergentes están deshabilitadas, puede usar el método de redirección. Use el método de redirección con Internet Explorer, ya que hay [problemas conocidos con las ventanas emergentes en este explorador](https://github.com/AzureAD/microsoft-authentication-library-for-js/wiki/Known-issues-on-IE-and-Edge-Browser).

## <a name="sign-in-with-a-pop-up-window"></a>Inicio de sesión con una ventana emergente

# <a name="javascript"></a>[JavaScript](#tab/javascript)

```javascript
const loginRequest = {
    scopes: ["https://graph.microsoft.com/User.ReadWrite"]
}

userAgentApplication.loginPopup(loginRequest).then(function (loginResponse) {
    //login success
    let idToken = loginResponse.idToken;
}).catch(function (error) {
    //login failure
    console.log(error);
});
```

# <a name="angular"></a>[Angular](#tab/angular)

El contenedor MSAL Angular le permite proteger rutas específicas de la aplicación con solo agregar el valor `MsalGuard` a la definición de ruta. Esta protección invocará al método para iniciar sesión cuando se tenga acceso a esa ruta.

```javascript
// In app.routes.ts
{ path: 'product', component: ProductComponent, canActivate : [MsalGuard],
    children: [
      { path: 'detail/:id', component: ProductDetailComponent  }
    ]
   },
  { path: 'myProfile' ,component: MsGraphComponent, canActivate : [MsalGuard] },
```

Para usar la experiencia de ventana emergente, habilite la opción de configuración `popUp`. También puede pasar los ámbitos que requieren el consentimiento tal como se detalla a continuación:

```javascript
//In app.module.ts
@NgModule({
  imports: [ MsalModule.forRoot({
                clientID: 'your_app_id',
                popUp: true,
                consentScopes: ["https://graph.microsoft.com/User.ReadWrite"]
            })]
         })
```
---

## <a name="sign-in-with-redirect"></a>Inicio de sesión con redirección

# <a name="javascript"></a>[JavaScript](#tab/javascript)

Los métodos de redirección no devuelven una promesa, dado que se abandona la aplicación principal. Para procesar los tokens devueltos y acceder a ellos, debe registrar las devoluciones de llamada correctas y con error antes de llamar a los métodos de redirección.

```javascript
function authCallback(error, response) {
    //handle redirect response
}

userAgentApplication.handleRedirectCallback(authCallback);

const loginRequest = {
    scopes: ["https://graph.microsoft.com/User.ReadWrite"]
}

userAgentApplication.loginRedirect(loginRequest);
```

# <a name="angular"></a>[Angular](#tab/angular)

El código aquí es el mismo que el que se describió anteriormente en la sección sobre el inicio de sesión con una ventana emergente. El flujo predeterminado es de redirección.

> [!NOTE]
> El token de id. no contiene los ámbitos con consentimiento y solo representa al usuario autenticado. Se devuelven los ámbitos con consentimiento en el token de acceso que va a adquirir en el paso siguiente.

---

## <a name="sign-out"></a>Cierre de sesión

La biblioteca MSAL proporciona un método `logout` que borra la caché en el almacenamiento del explorador y envía una solicitud de cierre de sesión a Azure Active Directory (Azure AD). Después de cerrar la sesión, la biblioteca redirige de vuelta a la página de inicio de la aplicación de forma predeterminada.

Puede configurar el URI de redirección tras el cierre de sesión mediante el establecimiento del valor `postLogoutRedirectUri`. Este URI también se debe registrar como URI de cierre de sesión en el registro de la aplicación.

# <a name="javascript"></a>[JavaScript](#tab/javascript)

```javascript
const config = {

    auth: {
        clientID: 'your_app_id',
        redirectUri: "your_app_redirect_uri", //defaults to application start page
        postLogoutRedirectUri: "your_app_logout_redirect_uri"
    }
}

const userAgentApplication = new UserAgentApplication(config);
userAgentApplication.logout();

```

# <a name="angular"></a>[Angular](#tab/angular)

```javascript
//In app.module.ts
@NgModule({
  imports: [ MsalModule.forRoot({
                clientID: 'your_app_id',
                postLogoutRedirectUri: "your_app_logout_redirect_uri"
            })]
         })

// In app.component.ts
this.authService.logout();
```

---

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Adquisición de un token para la aplicación](scenario-spa-acquire-token.md)

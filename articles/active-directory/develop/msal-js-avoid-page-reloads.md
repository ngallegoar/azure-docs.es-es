---
title: Evitar las recargas de página (MSAL.js) | Azure
titleSuffix: Microsoft identity platform
description: Aprenda a evitar las recargas de página al adquirir y renovar tokens en modo silencioso utilizando Microsoft Authentication Library para JavaScript (MSAL.js).
services: active-directory
author: mmacy
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: how-to
ms.workload: identity
ms.date: 05/29/2019
ms.author: marsma
ms.reviewer: saeeda
ms.custom: aaddev
ms.openlocfilehash: 5eb30f7dcf4b459b0af0bd8de965971fbbe44863
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85477658"
---
# <a name="avoid-page-reloads-when-acquiring-and-renewing-tokens-silently-using-msaljs"></a>Evite las recargas de página al adquirir y renovar tokens en modo silencioso utilizando MSAL.js
La Biblioteca de autenticación de Microsoft para JavaScript (MSAL.js) usa elementos `iframe` ocultos para adquirir y renovar los tokens silenciosamente en segundo plano. Azure AD devuelve el token de vuelta a los redirect_uri registrados especificados en la solicitud de token (de forma predeterminada, es la página raíz de la aplicación). Puesto que la respuesta es 302, da como resultado el HTML correspondiente al `redirect_uri` que se carga en el `iframe`. Normalmente, el `redirect_uri` de la aplicación es la página raíz y esto provoca que se vuelva a cargar.

En otros casos, si ir a la página raíz de la aplicación requiere autenticación, podría provocar elementos `iframe` anidados o un error `X-Frame-Options: deny`.

Dado que MSAL.js no puede descartar el error 302 emitido por Azure AD y es necesario para procesar el token devuelto, no puede impedir que el `redirect_uri` se cargue en el `iframe`.

Para evitar que toda la aplicación vuelva a cargarse de nuevo u otros errores debidos a esto, siga estas soluciones.

## <a name="specify-different-html-for-the-iframe"></a>Especificación de HTML diferente para el iframe

Establezca la propiedad `redirect_uri` en la configuración a una página simple, que no requiera autenticación. Tiene que asegurarse de que coincide con el `redirect_uri` registrado en Azure Portal. Esto no afectará a la experiencia de inicio de sesión del usuario ya que MSAL guarda la página de inicio cuando el usuario comienza el proceso de inicio de sesión y se redirige a la ubicación exacta una vez completado el inicio de sesión.

## <a name="initialization-in-your-main-app-file"></a>Inicialización en el archivo de aplicación principal

Si la aplicación está estructurada de forma que hay un archivo Javascript central que define la inicialización de la aplicación, el enrutamiento y otras cuestiones, puede cargar condicionalmente los módulos de aplicación en función de si la aplicación se está cargando en un `iframe` o no. Por ejemplo:

En AngularJS: app.js

```javascript
// Check that the window is an iframe and not popup
if (window !== window.parent && !window.opener) {
angular.module('todoApp', ['ui.router', 'MsalAngular'])
    .config(['$httpProvider', 'msalAuthenticationServiceProvider','$locationProvider', function ($httpProvider, msalProvider,$locationProvider) {
        msalProvider.init(
            // msal configuration
        );

        $locationProvider.html5Mode(false).hashPrefix('');
    }]);
}
else {
    angular.module('todoApp', ['ui.router', 'MsalAngular'])
        .config(['$stateProvider', '$httpProvider', 'msalAuthenticationServiceProvider', '$locationProvider', function ($stateProvider, $httpProvider, msalProvider, $locationProvider) {
            $stateProvider.state("Home", {
                url: '/Home',
                controller: "homeCtrl",
                templateUrl: "/App/Views/Home.html",
            }).state("TodoList", {
                url: '/TodoList',
                controller: "todoListCtrl",
                templateUrl: "/App/Views/TodoList.html",
                requireLogin: true
            })

            $locationProvider.html5Mode(false).hashPrefix('');

            msalProvider.init(
                // msal configuration
            );
        }]);
}
```

En Angular: app.module.ts

```javascript
// Imports...
@NgModule({
  declarations: [
    AppComponent,
    MsalComponent,
    MainMenuComponent,
    AccountMenuComponent,
    OsNavComponent
  ],
  imports: [
    BrowserModule,
    AppRoutingModule,
    HttpClientModule,
    ServiceWorkerModule.register('ngsw-worker.js', { enabled: environment.production }),
    MsalModule.forRoot(environment.MsalConfig),
    SuiModule,
    PagesModule
  ],
  providers: [
    HttpServiceHelper,
    {provide: HTTP_INTERCEPTORS, useClass: MsalInterceptor, multi: true},
    AuthService
  ],
  entryComponents: [
    AppComponent,
    MsalComponent
  ]
})
export class AppModule {
  constructor() {
    console.log('APP Module Constructor!');
  }

  ngDoBootstrap(ref: ApplicationRef) {
    if (window !== window.parent && !window.opener)
    {
      console.log("Bootstrap: MSAL");
      ref.bootstrap(MsalComponent);
    }
    else
    {
    //this.router.resetConfig(RouterModule);
      console.log("Bootstrap: App");
      ref.bootstrap(AppComponent);
    }
  }
}
```

MsalComponent:

```javascript
import { Component} from '@angular/core';
import { MsalService } from '@azure/msal-angular';

// This component is used only to avoid Angular reload
// when doing acquireTokenSilent()

@Component({
  selector: 'app-root',
  template: '',
})
export class MsalComponent {
  constructor(private Msal: MsalService) {
  }
}
```

## <a name="next-steps"></a>Pasos siguientes
Lea más sobre cómo [compilar una aplicación de página única (SPA)](scenario-spa-overview.md) mediante MSAL.js.
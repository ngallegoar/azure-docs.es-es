---
title: 'Configuración de una aplicación de página única: Plataforma de identidad de Microsoft | Azure'
description: Aprenda a crear una aplicación de página única (configuración del código de la aplicación)
services: active-directory
documentationcenter: dev-center-name
author: navyasric
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/11/2020
ms.author: nacanuma
ms.custom: aaddev
ms.openlocfilehash: f1e0bf44515aab18019b19b4f0a6f84183e5aac3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "77160090"
---
# <a name="single-page-application-code-configuration"></a>Aplicación de página única: Configuración del código

Obtenga información sobre cómo configurar el código de la aplicación de página única (SPA).

## <a name="msal-libraries-that-support-implicit-flow"></a>Bibliotecas MSAL que admiten flujo implícito

La plataforma de identidad de Microsoft proporciona las siguientes bibliotecas de autenticación de Microsoft (MSAL) para admitir el flujo implícito mediante prácticas de seguridad recomendadas por el sector:  

| Biblioteca MSAL | Descripción |
|--------------|--------------|
| ![MSAL.js](media/sample-v2-code/logo_js.png) <br/> [MSAL.js](https://github.com/AzureAD/microsoft-authentication-library-for-js)  | Biblioteca de JavaScript sin formato para usar en cualquier aplicación web cliente compilada con marcos de JavaScript o SPA, como Angular, Vue.js y React.js. |
| ![MSAL Angular](media/sample-v2-code/logo_angular.png) <br/> [MSAL Angular](https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-angular/README.md) | Contenedor de la biblioteca básica de MSAL.js para simplificar el uso en aplicaciones de página única creadas con el marco Angular. Esta biblioteca está en versión preliminar y tiene [problemas conocidos](https://github.com/AzureAD/microsoft-authentication-library-for-js/issues?q=is%3Aopen+is%3Aissue+label%3Aangular) con determinadas versiones de Angular y ciertos exploradores. |

## <a name="application-code-configuration"></a>Configuración del código de la aplicación

En una biblioteca MSAL, la información de registro de aplicación se pasa como una configuración durante la inicialización de la biblioteca.

# <a name="javascript"></a>[JavaScript](#tab/javascript)

```javascript
// Configuration object constructed.
const config = {
    auth: {
        clientId: 'your_app_id',
        redirectUri: "your_app_redirect_uri" //defaults to application start page
    }
}

// create UserAgentApplication instance
const userAgentApplication = new UserAgentApplication(config);
```

Para más información sobre las opciones configurables, consulte [Inicialización de aplicaciones con MSAL.js](msal-js-initializing-client-applications.md).

# <a name="angular"></a>[Angular](#tab/angular)

```javascript
//In app.module.ts
import { MsalModule } from '@azure/msal-angular';

@NgModule({
  imports: [ MsalModule.forRoot({
                clientID: 'your_app_id'
            })]
         })

  export class AppModule { }
```

---

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Inicio y cierre de sesión](scenario-spa-sign-in.md)

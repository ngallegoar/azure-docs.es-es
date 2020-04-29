---
title: 'Configuración de una aplicación de página única: Plataforma de identidad de Microsoft | Azure'
description: Aprenda a crear una aplicación de página única (configuración del código de la aplicación)
services: active-directory
author: navyasric
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 02/11/2020
ms.author: nacanuma
ms.custom: aaddev
ms.openlocfilehash: f159105046231ba5fb4e458cdd70d930a411a920
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/28/2020
ms.locfileid: "80882342"
---
# <a name="single-page-application-code-configuration"></a>Aplicación de página única: Configuración del código

Obtenga información sobre cómo configurar el código de la aplicación de página única (SPA).

## <a name="msal-libraries-that-support-implicit-flow"></a>Bibliotecas MSAL que admiten flujo implícito

La plataforma de identidad de Microsoft proporciona las siguientes bibliotecas de autenticación de Microsoft (MSAL) para admitir el flujo implícito mediante prácticas de seguridad recomendadas por el sector:

| Biblioteca MSAL | Descripción |
|--------------|--------------|
| ![MSAL.js](media/sample-v2-code/logo_js.png) <br/> [MSAL.js](https://github.com/AzureAD/microsoft-authentication-library-for-js)  | Biblioteca de JavaScript sin formato para usar en cualquier aplicación web cliente compilada con marcos de JavaScript o SPA, como Angular, Vue.js y React.js. |
| ![MSAL Angular](media/sample-v2-code/logo_angular.png) <br/> [MSAL Angular](https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-angular/README.md) | Contenedor de la biblioteca básica de MSAL.js para simplificar el uso en aplicaciones de página única creadas con el marco Angular. |

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
// App.module.ts
import { MsalModule } from '@azure/msal-angular';

@NgModule({
    imports: [
        MsalModule.forRoot({
            auth: {
                clientId: 'your_app_id'
            }
        })
    ]
})

export class AppModule { }
```

---

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Inicio y cierre de sesión](scenario-spa-sign-in.md)

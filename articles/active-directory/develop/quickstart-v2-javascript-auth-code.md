---
title: Inicio de sesión de los usuarios en aplicaciones de página única de JavaScript con código de autorización | Azure
titleSuffix: Microsoft identity platform
description: Aprenda cómo una aplicación de JavaScript puede llamar a una API que requiera tokens de acceso mediante el uso de la Plataforma de identidad de Microsoft.
services: active-directory
author: hahamil
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: quickstart
ms.workload: identity
ms.date: 04/22/2020
ms.author: hahamil
ms.custom: aaddev, identityplatformtop40, scenarios:getting-started, languages:JavaScript
ROBOTS: NOINDEX
ms.openlocfilehash: 9663c11508b0478a67f528cb301d705a3125e4f6
ms.sourcegitcommit: f57297af0ea729ab76081c98da2243d6b1f6fa63
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/06/2020
ms.locfileid: "82871511"
---
# <a name="quickstart-sign-in-users-and-get-an-access-token-in-a-javascript-spa-using-the-auth-code-flow"></a>Inicio rápido: Inicio de sesión de los usuarios y obtención de un token de acceso en un SPA de JavaScript mediante el flujo de código de autorización 

> [!IMPORTANT]
> Esta funcionalidad actualmente está en su versión preliminar. Las versiones preliminares están a su disposición con la condición de que acepte los [términos de uso adicionales](https://azure.microsoft.com/support/legal/preview-supplemental-terms/). Es posible que algunos de los aspectos de esta característica cambien antes de ofrecer disponibilidad general (GA).


En este inicio rápido se usa MSAL.js 2.0 con el flujo de código de autorización. Para usar MSAL.js 1.0 con el flujo implícito, consulte [este inicio rápido](https://docs.microsoft.com/azure/active-directory/develop/quickstart-v2-javascript).

En este inicio rápido se usa un código de ejemplo que muestra cómo una aplicación de página única (SPA) de JavaScript puede iniciar sesión en usuarios de cuentas personales, profesionales y educativas. Una SPA de JavaScript también puede obtener un token de acceso para llamar a Microsoft Graph API o a cualquier API web. Para ilustrar este tema, consulte el apartado en el que se explica el [funcionamiento del ejemplo](#how-the-sample-works).

## <a name="prerequisites"></a>Prerrequisitos

* Una suscripción a Azure: [cree una de forma gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
* [Node.js](https://nodejs.org/en/download/)
* [Visual Studio Code](https://code.visualstudio.com/download) (para editar archivos de proyecto)


> [!div renderon="docs"]
> ## <a name="register-and-download-your-quickstart-application"></a>Registro y descarga de la aplicación de inicio rápido
> Para iniciar la aplicación de inicio rápido, use cualquiera de las siguientes opciones.
>
> ### <a name="option-1-express-register-and-auto-configure-your-app-and-then-download-your-code-sample"></a>Opción 1 (Rápido): registrar y configurar de modo automático la aplicación y, a continuación, descargar el código de ejemplo
>
> 1. Inicie sesión en [Azure Portal](https://portal.azure.com).
> 1. Si la cuenta le proporciona acceso a más de un inquilino, seleccione la cuenta en la esquina superior derecha y establezca la sesión del portal en el inquilino de Active Directory (Azure AD) que desee utilizar.
> 1. Seleccione [App registrations](https://portal.azure.com/#blade/Microsoft_AAD_RegisteredApps/ApplicationsListBlade/quickStartType/JavascriptSpaQuickstartPage/sourceType/docs) (Registros de aplicaciones).
> 1. Escriba un nombre para la aplicación.
> 1. En **Supported account types** (Tipos de cuenta compatibles), seleccione **Accounts in any organizational directory and personal Microsoft accounts** (Cuentas en cualquier directorio de organización y cuentas personales de Microsoft).
> 1. Seleccione **Registrar**.
> 1. Vaya al panel de inicio rápido y siga las instrucciones para descargar y configurar automáticamente su nueva aplicación.
>
> ### <a name="option-2-manual-register-and-manually-configure-your-application-and-code-sample"></a>Opción 2 (Manual): registrar y configurar manualmente la aplicación y el código de ejemplo
>
> #### <a name="step-1-register-your-application"></a>Paso 1: Registrar su aplicación
>
> 1. Inicie sesión en [Azure Portal](https://portal.azure.com).
>
> 1. Si la cuenta proporciona acceso a más de un inquilino, seleccione la cuenta en la esquina superior derecha y establezca la sesión del portal en el inquilino de Azure AD que desee utilizar.
> 1. Seleccione [App registrations](https://go.microsoft.com/fwlink/?linkid=2083908) (Registros de aplicaciones).
> 1. Seleccione **Nuevo registro**.
> 1. Cuando se abra la página **Registrar una aplicación**, escriba el nombre de su aplicación.
> 1. En **Supported account types** (Tipos de cuenta compatibles), seleccione **Accounts in any organizational directory and personal Microsoft accounts** (Cuentas en cualquier directorio de organización y cuentas personales de Microsoft).
> 1. Seleccione **Registrar**. En la página de **información general** de la aplicación, anote el valor del **Identificador de aplicación (cliente)** para su uso posterior.
> 1. En el panel izquierdo de la aplicación registrada, seleccione **Autenticación**.
> 1. En **Configuraciones de plataforma**, seleccione **Agregar una plataforma**. Se abre un panel a la izquierda. Allí, seleccione la región **Aplicaciones de página única**.
> 1. También en el panel de la izquierda, establezca el valor **URI de redireccionamiento** en `http://localhost:3000/`. 
> 1. Seleccione **Configurar**.

> [!div class="sxs-lookup" renderon="portal"]
> #### <a name="step-1-configure-your-application-in-the-azure-portal"></a>Paso 1: Configuración de la aplicación en Azure Portal
> Para que el código de ejemplo de este inicio rápido funcione, tiene que agregar un valor de `redirectUri` de `http://localhost:3000/`.
> > [!div renderon="portal" id="makechanges" class="nextstepaction"]
> > [Realizar estos cambios por mí]()
>
> > [!div id="appconfigured" class="alert alert-info"]
> > ![Ya configurada](media/quickstart-v2-javascript/green-check.png) La aplicación está configurada con estos atributos.

#### <a name="step-2-download-the-project"></a>Paso 2: Descarga del proyecto

> [!div renderon="docs"]
> Para ejecutar el proyecto con un servidor web con Node.js. [descargue los archivos principales del proyecto](https://github.com/Azure-Samples/ms-identity-javascript-v2/archive/quickstart.zip).

> [!div renderon="portal" class="sxs-lookup"]
> Ejecutar el proyecto con un servidor Web mediante Node.js

> [!div renderon="portal" id="autoupdate" class="nextstepaction" class="sxs-lookup"]
> [Descargar el código de ejemplo](https://github.com/Azure-Samples/ms-identity-javascript-v2/archive/quickstart.zip)

> [!div renderon="docs"]
> #### <a name="step-3-configure-your-javascript-app"></a>Paso 3: Configuración de la aplicación de JavaScript
>
> En la carpeta *app*, edite *authConfig.js* y establezca los valores `clientID`, `authority` y `redirectUri` que se encuentran en `msalConfig`.
>
> ```javascript
>
>  // Config object to be passed to Msal on creation
>  const msalConfig = {
>    auth: {
>      clientId: "Enter_the_Application_Id_Here",
>      authority: "Enter_the_Cloud_Instance_Id_HereEnter_the_Tenant_Info_Here",
>      redirectUri: "Enter_the_Redirect_Uri_Here",
>    },
>    cache: {
>      cacheLocation: "sessionStorage", // This configures where your cache will be stored
>      storeAuthStateInCookie: false, // Set this to "true" if you are having issues on IE11 or Edge
>    }
>  };
>
>```

> [!div renderon="portal" class="sxs-lookup"]
> > [!NOTE]
> > :::no-loc text="Enter_the_Supported_Account_Info_Here":::

> [!div renderon="docs"]
>
> Donde:
> - *\<Enter_the_Application_Id_here>* es el **identificador de aplicación (cliente)** de la aplicación que ha registrado.
> - *\<Enter_the_Cloud_Instance_Id_Here >* es la instancia de la nube de Azure. En el caso de la nube de Azure principal o global, simplemente escriba *https://login.microsoftonline.com/* . Para nubes **nacionales** (por ejemplo, China), consulte [Nubes nacionales](https://docs.microsoft.com/azure/active-directory/develop/authentication-national-cloud).
> - *\<Enter_the_Tenant_info_here >* está establecido en una de las opciones siguientes:
>    - Si la aplicación admite *solo las cuentas de este directorio organizativo*, reemplace este valor por el **identificador de inquilino** o el **nombre de inquilino** (por ejemplo, *contoso.microsoft.com*).
>    - Si la aplicación admite *cuentas en cualquier directorio organizativo*, reemplace este valor por **organizaciones**.
>    - Si la aplicación admite *cuentas en cualquier directorio organizativo y cuentas Microsoft personales*, reemplace este valor por **común**. Para restringir la compatibilidad a *Personal Microsoft accounts only* (Solo cuentas Microsoft personales), reemplace este valor por **consumidores**.
> - *\<Enter_the_Redirect_Uri_Here>* es `http://localhost:3000`
> > [!TIP]
> > Para encontrar los valores de **Identificador de aplicación (cliente)** , **Identificador de directorio (inquilino)** y **Tipos de cuenta admitidos**, vaya a la página **Información general** del registro de aplicaciones en Azure Portal.
>
> [!div class="sxs-lookup" renderon="portal"]
> #### <a name="step-3-your-app-is-configured-and-ready-to-run"></a>Paso 3: La aplicación está configurada y lista para ejecutarse
> Hemos configurado el proyecto con los valores de las propiedades de su aplicación.

> [!div renderon="docs"]
>
> Después, en la misma carpeta, edite el archivo *graphConfig.js* para establecer `graphMeEndpoint` y `graphMailEndpoint` para el objeto `apiConfig`.
> ```javascript
>   // Add here the endpoints for MS Graph API services you would like to use.
>   const graphConfig = {
>     graphMeEndpoint: "Enter_the_Graph_Endpoint_Herev1.0/me",
>     graphMailEndpoint: "Enter_the_Graph_Endpoint_Herev1.0/me/messages"
>   };
>
>   // Add here scopes for access token to be used at MS Graph API endpoints.
>   const tokenRequest = {
>       scopes: ["Mail.Read"]
>   };
> ```
>

> [!div renderon="docs"]
>
> *\<EspecifiqueAquíElPuntoDeConexiónDelGráfico>* es el punto de conexión en el que se realizarán las llamadas API. Como servicio principal o global de Microsoft Graph API, escriba `https://graph.microsoft.com`. Para más información, consulte [Implementaciones de nube nacionales](https://docs.microsoft.com/graph/deployments).
>
> #### <a name="step-4-run-the-project"></a>Paso 4: Ejecución del proyecto

Ejecute el proyecto con un servidor web mediante [Node.js](https://nodejs.org/en/download/):

1. Para iniciar el servidor, ejecute los siguientes comandos desde el directorio del proyecto:
    ```bash
    npm install
    npm start
    ```
1. Vaya a `http://localhost:3000/`.

1. Seleccione **Iniciar sesión** para iniciar el proceso de inicio de sesión y, luego, llame a Microsoft Graph API.

    La primera vez que inicie sesión, se le pedirá que dé su consentimiento para permitir que la aplicación acceda a su perfil e inicie sesión automáticamente. Después de que haya iniciado sesión correctamente, la información del perfil de usuario debe mostrarse en la página.

## <a name="more-information"></a>Más información

### <a name="how-the-sample-works"></a>Funcionamiento del ejemplo

![Funcionamiento del SPA de ejemplo de JavaScript: 1. El SPA realiza el inicio de sesión. 2. El SPA adquiere un token de identificador de la plataforma de identidad de Microsoft. 3. El SPA llama al token de adquisición. 4. La plataforma de identidad de Microsoft devuelve un token de acceso al SPA. 5. El SPA realiza una solicitud GET de HTTP con el token de acceso a Microsoft Graph API. 6. Graph API devuelve una respuesta HTTP al SPA.](media/quickstart-v2-javascript/javascriptspa-intro.svg)

### <a name="msaljs"></a>msal.js

La biblioteca MSAL.js inicia la sesión de los usuarios y solicita los tokens que se usan para acceder a una API protegida por la plataforma de identidad de Microsoft. El archivo *index.html* del ejemplo contiene una referencia a la biblioteca:

```html
<script type="text/javascript" src="https://alcdn.msauth.net/browser/2.0.0-beta.0/js/msal-browser.js" integrity=
"sha384-r7Qxfs6PYHyfoBR6zG62DGzptfLBxnREThAlcJyEfzJ4dq5rqExc1Xj3TPFE/9TH" crossorigin="anonymous"></script>
```
> [!TIP]
> Puede reemplazar la versión anterior con la versión más reciente en las [versiones MSAL.js](https://github.com/AzureAD/microsoft-authentication-library-for-js/releases).

Si tiene Node.js instalado, también puede descargar la versión más reciente mediante el administrador de paquetes de Node.js (npm):

```batch
npm install @azure/msal-browser
```

## <a name="next-steps"></a>Pasos siguientes

El [repositorio de GitHub de MSAL.js](https://github.com/AzureAD/microsoft-authentication-library-for-js) contiene documentación adicional de la biblioteca y una sección de preguntas frecuentes, y ofrece además soporte técnico para los problemas.

Para obtener una guía paso a paso más detallada sobre la compilación de la aplicación para este inicio rápido, consulte:

> [!div class="nextstepaction"]
> [Tutorial para iniciar sesión y llamar a MS Graph](https://docs.microsoft.com/azure/active-directory/develop/tutorial-v2-javascript-auth-code)

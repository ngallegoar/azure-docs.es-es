---
title: 'Tutorial de aplicaciones de página única de JavaScript 2.0: Plataforma de identidad de Microsoft | Azure'
description: Se describe cómo las aplicaciones SPA de JavaScript pueden usar el flujo de código de autorización para llamar a una API que requiere tokens de acceso mediante el punto de conexión de Azure Active Directory v2.0
services: active-directory
documentationcenter: dev-center-name
author: hahamil
manager: CelesteDG
ROBOTS: NOINDEX
ms.service: active-directory
ms.subservice: develop
ms.topic: tutorial
ms.workload: identity
ms.date: 04/22/2020
ms.author: hahamil
ms.custom: aaddev, identityplatformtop40
ms.openlocfilehash: ee156c8118a67061d0a000867ee64fe1f3ebd18c
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/28/2020
ms.locfileid: "82181887"
---
# <a name="sign-in-users-and-call-the-microsoft-graph-api-from-a-javascript-single-page-application-spa---msaljs-20"></a>Inicio de sesión de usuarios y llamada a Microsoft Graph API desde una aplicación de página única (SPA) de JavaScript: MSAL.js 2.0

> [!IMPORTANT]
> Esta funcionalidad actualmente está en su versión preliminar. Las versiones preliminares están a su disposición con la condición de que acepte los [términos de uso adicionales](https://azure.microsoft.com/support/legal/preview-supplemental-terms/). Es posible que algunos de los aspectos de esta característica cambien antes de ofrecer disponibilidad general (GA).

En este tutorial se utiliza una versión de MSAL.js que usa el flujo de código de autorización de OAuth 2.0 con PKCE. Para obtener más información sobre este protocolo y conocer las diferencias entre el flujo implícito y el flujo del código de autorización, consulte la [documentación](https://docs.microsoft.com/azure/active-directory/develop/v2-oauth2-auth-code-flow). Si busca un tutorial que use el flujo implícito, consulte el [tutorial de MSAL.js v1](https://docs.microsoft.com/azure/active-directory/develop/tutorial-v2-javascript-spa).

Esta versión de MSAL.js mejora la biblioteca msal-core actual y usa el flujo de código de autorización en el explorador. La mayoría de las características disponibles en la anterior biblioteca están disponibles en esta versión, pero en ambos casos hay matices en el flujo de autenticación. Esta versión **no** admite el flujo implícito.

En esta guía se muestra cómo una aplicación de una sola página (SPA) de JavaScript puede:
- Iniciar sesión en cuentas personales, profesionales y educativas
- Adquisición de un token de acceso
- Llamar a Microsoft Graph API o a otras API que requieran tokens de acceso del *punto de conexión de la Plataforma de identidad de Microsoft*.

## <a name="how-the-sample-app-generated-by-this-guide-works"></a>Funcionamiento de la aplicación de ejemplo generada por esta guía

![Muestra cómo funciona la aplicación de ejemplo generada por este tutorial](media/active-directory-develop-guidedsetup-javascriptspa-introduction/javascriptspa-intro.svg)

### <a name="more-information"></a>Más información

La aplicación de ejemplo que se crea con esta guía permite que una aplicación de página única de JavaScript haga consultas a Microsoft Graph API o a una API web que acepte tokens de un punto de conexión de la Plataforma de identidad de Microsoft. En este escenario, después de que un usuario inicia sesión, se solicita un token de acceso y se agrega a las solicitudes HTTP mediante el encabezado de autorización. La adquisición y la renovación de los tokens se realizan por medio de la biblioteca de autenticación de Microsoft (MSAL).

### <a name="libraries"></a>Bibliotecas

Esta guía utiliza la siguiente biblioteca:

|Biblioteca|Descripción|
|---|---|
|[msal.js](https://github.com/AzureAD/microsoft-authentication-library-for-js/tree/dev/lib/msal-browser)|Biblioteca de autenticación de Microsoft para el paquete del explorador de JavaScript|

## <a name="set-up-your-web-server-or-project"></a>Configuración de un proyecto o servidor web

¿Prefiere descargar este proyecto de ejemplo en su lugar? Para ejecutar el proyecto con un servidor web local, como Node.js, clone los archivos del proyecto:

`git clone https://github.com/Azure-Samples/ms-identity-javascript-v2`

Para configurar el ejemplo de código antes de ejecutarlo, vaya al [paso de configuración](#register-your-application).

## <a name="prerequisites"></a>Prerrequisitos

* Para ejecutar este tutorial, necesita un servidor web local, como [Node.js](https://nodejs.org/en/download/) o [.NET Core](https://www.microsoft.com/net/core).

* Si usa Node.js para ejecutar el proyecto, instale un entorno de desarrollo integrado (IDE), como [Visual Studio Code](https://code.visualstudio.com/download), para editar los archivos del proyecto.

* Las instrucciones de este tutorial se basan en Node.js.

## <a name="create-your-project"></a>Creación del proyecto

Asegúrese de que tiene instalado [Node.js](https://nodejs.org/en/download/) y, a continuación, cree una carpeta para hospedar la aplicación. A continuación, implemente un servidor web [Express](https://expressjs.com/) pequeño para atender el archivo `index.html`.

1. En primer lugar, vaya a la carpeta del proyecto en el terminal y, a continuación, ejecute los siguientes comandos de NPM.
    ```console
    npm init -y
    npm install @azure/msal-Browser
    npm install express
    ```
2. Después, cree un archivo .js llamado *server.js* y agregue el código siguiente:

   ```JavaScript
   const express = require('express');
   const morgan = require('morgan');
   const path = require('path');
   const argv = require('yargs')
      .usage('Usage: $0 -p [PORT]')
      .alias('p', 'port')
      .describe('port', '(Optional) Port Number - default is 3000')
      .strict()
      .argv;

   const DEFAULT_PORT = 3000;

   //initialize express.
   const app = express();

   // Initialize variables.
   let port = DEFAULT_PORT; // -p {PORT} || 3000;
   if (argv.p) {
      port = argv.p;
   }

   // Configure morgan module to log all requests.
   app.use(morgan('dev'));

   // Set the front-end folder to serve public assets.
   app.use("/lib", express.static(path.join(__dirname, "../../lib/msal-browser/lib")));

   // Setup app folders
   app.use(express.static('app'));

   // Set up a route for index.html.
   app.get('*', function (req, res) {
      res.sendFile(path.join(__dirname + '/index.html'));
   });

   // Start the server.
   app.listen(port);
   console.log(`Listening on port ${port}...`);
    ```

Ahora tiene un servidor simple para dar servicio a la SPA. La estructura de carpetas deseada al final de este tutorial es la siguiente:

![representación de texto de la estructura de carpetas de la SPA deseada](./media/tutorial-v2-javascript-spa/single-page-application-folder-structure.png)

## <a name="create-the-spa-ui"></a>Creación de la interfaz de usuario de SPA

1. Cree un archivo *index.html* para la aplicación de página única de JavaScript en la carpeta *app*. Este archivo implementa una interfaz de usuario creada con **Bootstrap 4 Framework** e importa archivos de script para la configuración, la autenticación y las llamadas a la API.

   En el archivo *index.html*, agregue el código siguiente:

   ```html
   <!DOCTYPE html>
   <html lang="en">
      <head>
         <meta charset="UTF-8">
         <meta name="viewport" content="width=device-width, initial-scale=1.0, shrink-to-fit=no">
         <title>Quickstart | MSAL.JS Vanilla JavaScript SPA</title>

         <!-- IE support: add promises polyfill before msal.js  -->
         <script type="text/javascript" src="//cdn.jsdelivr.net/npm/bluebird@3.7.2/js/browser/bluebird.min.js"></script>
         <script type="text/javascript" src="https://alcdn.msauth.net/browser/2.0.0-beta.0/js/msal-browser.js"></script>

         <!-- adding Bootstrap 4 for UI components  -->
         <link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.4.1/css/bootstrap.min.css" integrity="sha384-Vkoo8x4CGsO3+Hhxv8T/Q5PaXtkKtu6ug5TOeNV6gBiFeWPGFN9MuhOf23Q9Ifjh" crossorigin="anonymous">
         <link rel="SHORTCUT ICON" href="https://c.s-microsoft.com/favicon.ico?v2" type="image/x-icon">
      </head>
      <body>
         <nav class="navbar navbar-expand-lg navbar-dark bg-primary">
            <a class="navbar-brand" href="/">MS Identity Platform</a>
            <div class="btn-group ml-auto dropleft">
               <button type="button" id="SignIn" class="btn btn-secondary" onclick="signIn()">
                  Sign In
               </button>
            </div>
         </nav>
         <br>
         <h5 class="card-header text-center">Vanilla JavaScript SPA calling MS Graph API with MSAL.JS</h5>
         <br>
         <div class="row" style="margin:auto" >
         <div id="card-div" class="col-md-3" style="display:none">
         <div class="card text-center">
            <div class="card-body">
            <h5 class="card-title" id="WelcomeMessage">Please sign-in to see your profile and read your mails</h5>
            <div id="profile-div"></div>
            <br>
            <br>
            <button class="btn btn-primary" id="seeProfile" onclick="seeProfile()">See Profile</button>
            <br>
            <br>
            <button class="btn btn-primary" id="readMail" onclick="readMail()">Read Mails</button>
            </div>
         </div>
         </div>
         <br>
         <br>
            <div class="col-md-4">
            <div class="list-group" id="list-tab" role="tablist">
            </div>
            </div>
            <div class="col-md-5">
             <div class="tab-content" id="nav-tabContent">
            </div>
            </div>
         </div>
         <br>
         <br>

         <!-- importing bootstrap.js and supporting js libraries -->
         <script src="https://code.jquery.com/jquery-3.4.1.slim.min.js" integrity="sha384-J6qa4849blE2+poT4WnyKhv5vZF5SrPo0iEjwBvKU7imGFAV0wwj1yYfoRSJoZ+n" crossorigin="anonymous"></script>
         <script src="https://cdn.jsdelivr.net/npm/popper.js@1.16.0/dist/umd/popper.min.js" integrity="sha384-Q6E9RHvbIyZFJoft+2mJbHaEWldlvI9IOYy5n3zV9zzTtmI3UksdQRVvoxMfooAo" crossorigin="anonymous"></script>
         <script src="https://stackpath.bootstrapcdn.com/bootstrap/4.4.1/js/bootstrap.min.js" integrity="sha384-wfSDF2E50Y2D1uUdj0O3uMBJnjuUD4Ih7YwaYd1iqfktj0Uod8GCExl3Og8ifwB6" crossorigin="anonymous"></script>

         <!-- importing app scripts (load order is important) -->
         <script type="text/javascript" src="./authConfig.js"></script>
         <script type="text/javascript" src="./graphConfig.js"></script>
         <script type="text/javascript" src="./ui.js"></script>

         <!-- <script type="text/javascript" src="./authRedirect.js"></script>   -->
         <!-- uncomment the above line and comment the line below if you would like to use the redirect flow -->
         <script type="text/javascript" src="./authPopup.js"></script>
         <script type="text/javascript" src="./graph.js"></script>
      </body>
     </html>
     ```

   > [!TIP]
   > Puede reemplazar la versión de MSAL.js del script anterior con la versión más reciente en la sección de [versiones de MSAL.js](https://github.com/AzureAD/microsoft-authentication-library-for-js/releases).


2. Ahora, cree un archivo .js llamado *ui.js*, que tendrá acceso a los elementos del DOM y los actualizará, y agregue el código siguiente:

   ```JavaScript
   // Select DOM elements to work with
   const welcomeDiv = document.getElementById("welcomeMessage");
   const signInButton = document.getElementById("signIn");
   const signOutButton = document.getElementById('signOut');
   const cardDiv = document.getElementById("card-div");
   const mailButton = document.getElementById("readMail");
   const profileButton = document.getElementById("seeProfile");
   const profileDiv = document.getElementById("profile-div");

   function showWelcomeMessage(account) {
     // Reconfiguring DOM elements
     cardDiv.classList.remove('d-none');
     welcomeDiv.innerHTML = `Welcome ${account.name}`;
     signInButton.classList.add('d-none');
     signOutButton.classList.remove('d-none');
   }

   function updateUI(data, endpoint) {
     console.log('Graph API responded at: ' + new Date().toString());

     if (endpoint === graphConfig.graphMeEndpoint) {
       const title = document.createElement('p');
       title.innerHTML = "<strong>Title: </strong>" + data.jobTitle;
       const email = document.createElement('p');
       email.innerHTML = "<strong>Mail: </strong>" + data.mail;
       const phone = document.createElement('p');
       phone.innerHTML = "<strong>Phone: </strong>" + data.businessPhones[0];
       const address = document.createElement('p');
       address.innerHTML = "<strong>Location: </strong>" + data.officeLocation;
       profileDiv.appendChild(title);
       profileDiv.appendChild(email);
       profileDiv.appendChild(phone);
       profileDiv.appendChild(address);

     } else if (endpoint === graphConfig.graphMailEndpoint) {
         if (data.value.length < 1) {
           alert("Your mailbox is empty!")
         } else {
           const tabList = document.getElementById("list-tab");
           tabList.innerHTML = ''; // clear tabList at each readMail call
           const tabContent = document.getElementById("nav-tabContent");

           data.value.map((d, i) => {
             // Keeping it simple
             if (i < 10) {
               const listItem = document.createElement("a");
               listItem.setAttribute("class", "list-group-item list-group-item-action")
               listItem.setAttribute("id", "list" + i + "list")
               listItem.setAttribute("data-toggle", "list")
               listItem.setAttribute("href", "#list" + i)
               listItem.setAttribute("role", "tab")
               listItem.setAttribute("aria-controls", i)
               listItem.innerHTML = d.subject;
               tabList.appendChild(listItem)

               const contentItem = document.createElement("div");
               contentItem.setAttribute("class", "tab-pane fade")
               contentItem.setAttribute("id", "list" + i)
               contentItem.setAttribute("role", "tabpanel")
               contentItem.setAttribute("aria-labelledby", "list" + i + "list")
               contentItem.innerHTML = "<strong> from: " + d.from.emailAddress.address + "</strong><br><br>" + d.bodyPreview + "...";
               tabContent.appendChild(contentItem);
             }
           });
         }
     }
   }
   ```

## <a name="register-your-application"></a>Registrar su aplicación

Siga las instrucciones para [registrar una nueva aplicación de página única](https://docs.microsoft.com/zure/active-directory/develop/scenario-spa-app-registration).

#### <a name="set-a-redirect-url-for-nodejs"></a>Configuración de una dirección URL de redireccionamiento para Node.js

Para Node.js, puede establecer el puerto del servidor web en el archivo *server.js*. En este tutorial se usa el puerto 3000, pero puede usar cualquier otro disponible.

Para configurar una URL de redireccionamiento en la información de registro de la aplicación, vuelva al panel **Registro de aplicación** y realice una de las acciones siguientes para registrar una nueva **SPA**:

- Establezca *`http://localhost:3000/`* como la **dirección URL de redireccionamiento**.
- Si usa un puerto TCP personalizado, use *`http://localhost:<port>/`* (donde *\<port>* es el número de puerto TCP personalizado).

### <a name="configure-your-javascript-spa"></a>Configuración de JavaScript SPA

Cree un nuevo archivo .js llamado *authConfig.js*, que contendrá los parámetros de configuración para la autenticación, y agregue el código siguiente:

```javascript
  const msalConfig = {
    auth: {
      clientId: "Enter_the_Application_Id_Here",
      authority: "Enter_the_Cloud_Instance_Id_HereEnter_the_Tenant_Info_Here",
      redirectUri: "Enter_the_Redirect_Uri_Here",
    },
    cache: {
      cacheLocation: "sessionStorage", // This configures where your cache will be stored
      storeAuthStateInCookie: false, // Set this to "true" if you are having issues on IE11 or Edge
    }
  };

  // Add here scopes for id token to be used at MS Identity Platform endpoints.
  const loginRequest = {
   scopes: ["openid", "profile", "User.Read"]
  };

   // Add here scopes for access token to be used at MS Graph API endpoints.
  const tokenRequest = {
   scopes: ["User.Read", "Mail.Read"]
  };

```

 Donde:
 - *\<Enter_the_Application_Id_here>* es el **identificador de aplicación (cliente)** de la aplicación que ha registrado.
 - *\<Enter_the_Cloud_Instance_Id_Here >* es la instancia de la nube de Azure. En el caso de la nube de Azure principal o global, simplemente escriba *https://login.microsoftonline.com* . Para nubes **nacionales** (por ejemplo, China), consulte [Nubes nacionales](https://docs.microsoft.com/azure/active-directory/develop/authentication-national-cloud).
 - *\<Enter_the_Tenant_info_here >* está establecido en una de las opciones siguientes:
   - Si la aplicación admite *solo las cuentas de este directorio organizativo*, reemplace este valor por el **identificador de inquilino** o el **nombre de inquilino** (por ejemplo, *contoso.microsoft.com*).
   - Si la aplicación admite *cuentas en cualquier directorio organizativo*, reemplace este valor por **organizaciones**.
   - Si la aplicación admite *cuentas en cualquier directorio organizativo y cuentas Microsoft personales*, reemplace este valor por **común**. Para restringir la compatibilidad a *Personal Microsoft accounts only* (Solo cuentas Microsoft personales), reemplace este valor por **consumidores**.
- *\Enter_the_Redirect_Uri_Here>* es el puerto registrado en el portal ( *`http://localhost:3000/`* )


Cree un nuevo archivo .js llamado `graphConfig.js`, que contendrá los parámetros de configuración que llaman a Microsoft Graph API, y agregue el código siguiente:
```javascript
// Add here the endpoints for MS Graph API services you would like to use.
const graphConfig = {
    graphMeEndpoint: "Enter_the_Graph_Endpoint_Herev1.0/me",
    graphMailEndpoint: "Enter_the_Graph_Endpoint_Herev1.0/me/messages"
};
```
- *\<Enter_the_Graph_Endpoint_Here >* es la instancia de MS Graph API. En el caso del punto de conexión global de MS Graph API, basta con reemplazar esta cadena por `https://graph.microsoft.com`. En el caso de las implementaciones de nube nacional, consulte la [Documentación de Graph API](https://docs.microsoft.com/graph/deployments).

## <a name="use-the-microsoft-authentication-library-msal-to-sign-in-the-user"></a>Uso de la biblioteca de autenticación de Microsoft (MSAL) para iniciar la sesión del usuario

### <a name="popup"></a>PopUp
Cree un nuevo archivo .js llamado `authPopup.js`, que contendrá la lógica de la autenticación y de la adquisición del token para el control popUp del inicio de sesión, y agregue el código siguiente:

   ```JavaScript
  // Create the main myMSALObj instance
// configuration parameters are located at authConfig.js
const myMSALObj = new msal.PublicClientApplication(msalConfig);

function signIn() {
    myMSALObj.loginPopup(loginRequest)
        .then(loginResponse => {
            console.log('id_token acquired at: ' + new Date().toString());

            if (myMSALObj.getAccount()) {
                showWelcomeMessage(myMSALObj.getAccount());
            }
        }).catch(error => {
            console.error(error);
        });
}

function signOut() {
    myMSALObj.logout();
}

function getTokenPopup(request) {
    return myMSALObj.acquireTokenSilent(request)
        .catch(error => {
            console.warn(error);
            console.warn("silent token acquisition fails. acquiring token using popup");

            // fallback to interaction when silent call fails
            return myMSALObj.acquireTokenPopup(request)
                .then(tokenResponse => {
                    return tokenResponse;
                }).catch(error => {
                    console.error(error);
                });
        });
}

function seeProfile() {
    if (myMSALObj.getAccount()) {
        getTokenPopup(loginRequest)
            .then(response => {
                callMSGraph(graphConfig.graphMeEndpoint, response.accessToken, updateUI);
                profileButton.classList.add('d-none');
                mailButton.classList.remove('d-none');
            }).catch(error => {
                console.error(error);
            });
    }
}

function readMail() {
    if (myMSALObj.getAccount()) {
        getTokenPopup(tokenRequest)
            .then(response => {
                callMSGraph(graphConfig.graphMailEndpoint, response.accessToken, updateUI);
            }).catch(error => {
                console.error(error);
            });
    }
}
   ```
### <a name="redirect"></a>Redirect
Cree un nuevo archivo .js llamado `authRedirect.js`, que contendrá la lógica de la autenticación y de la adquisición del token para el control redirect del inicio de sesión, y agregue el código siguiente:

```javascript
// Create the main myMSALObj instance
// configuration parameters are located at authConfig.js
const myMSALObj = new msal.PublicClientApplication(msalConfig);

let accessToken;

// Register Callbacks for Redirect flow
myMSALObj.handleRedirectCallback(authRedirectCallBack);

function authRedirectCallBack(error, response) {
    if (error) {
        console.error(error);
    } else {
        if (myMSALObj.getAccount()) {
            console.log('id_token acquired at: ' + new Date().toString());
            showWelcomeMessage(myMSALObj.getAccount());
            getTokenRedirect(loginRequest);
        } else if (response.tokenType === "Bearer") {
            console.log('access_token acquired at: ' + new Date().toString());
        } else {
            console.log("token type is:" + response.tokenType);
        }
    }
}

// Redirect: once login is successful and redirects with tokens, call Graph API
if (myMSALObj.getAccount()) {
    showWelcomeMessage(myMSALObj.getAccount());
}

function signIn() {
    myMSALObj.loginRedirect(loginRequest);
}

function signOut() {
    myMSALObj.logout();
}

// This function can be removed if you do not need to support IE
function getTokenRedirect(request) {
    return myMSALObj.acquireTokenSilent(request)
        .then((response) => {
            console.log(response);
            if (response.accessToken) {
                console.log('access_token acquired at: ' + new Date().toString());
                accessToken = response.accessToken;

                callMSGraph(graphConfig.graphMeEndpoint, response.accessToken, updateUI);
                profileButton.style.display = 'none';
                mailButton.style.display = 'initial';
            }
        })
        .catch(error => {
            console.warn("silent token acquisition fails. acquiring token using redirect");
            // fallback to interaction when silent call fails
            return myMSALObj.acquireTokenRedirect(request);
        });
}

function seeProfile() {
    getTokenRedirect(loginRequest);
}

function readMail() {
    getTokenRedirect(tokenRequest);
}
```

### <a name="more-information"></a>Más información

Cuando un usuario selecciona el botón **Iniciar sesión** por primera vez, el método `signIn` llama a `loginPopup` para iniciar la sesión del usuario. Este método hace que se abra una ventana emergente con el *punto de conexión de la Plataforma de identidad de Microsoft* para pedir y validar las credenciales del usuario. Después de un inicio de sesión correcto, *msal.js* inicia el [flujo de código de autorización](https://docs.microsoft.com/azure/active-directory/develop/v2-oauth2-auth-code-flow).

En este momento, se envía un código de autorización protegido por PKCE al punto de conexión del token protegido con CORS y se intercambia por tokens. Se reciben un token de identificador, un token de acceso y un token de actualización; *msal.js* los procesa y la información contenida en los tokens se almacena en caché.

El token de identificador contiene información básica sobre el usuario, como su nombre para mostrar. Si piensa usar los datos proporcionados por este token, antes debe validarse en el servidor back-end, para asegurarse de que el token se emitió para un usuario válido en la aplicación. El token de actualización tiene una duración limitada y expira después de 24 horas. Este token se puede usar para adquirir nuevos tokens de acceso de forma silenciosa.

La instancia de SPA generada por esta guía llama a `acquireTokenSilent` o a `acquireTokenPopup` para adquirir un *token de acceso* que se usa para consultar la información del perfil de usuario a Microsoft Graph API. Si necesita obtener un ejemplo que valide el token de identificador, consulte la aplicación de ejemplo [active-directory-javascript-singlepageapp-dotnet-webapi-v2](https://github.com/Azure-Samples/active-directory-javascript-singlepageapp-dotnet-webapi-v2) en GitHub. En el ejemplo se usa una API web de ASP.NET para la validación de tokens.

#### <a name="get-a-user-token-interactively"></a>Obtención de un token de usuario interactivamente

Después del inicio de sesión inicial, no se debería pedir a los usuarios que se volviesen a autenticar cada vez que tengan que solicitar un token para acceder a un recurso. Para evitar estas solicitudes de reautenticación, use `acquireTokenSilent`. Sin embargo, puede haber situaciones en las que sea necesario hacer que los usuarios interactúen con el punto de conexión de la Plataforma de identidad de Microsoft. Por ejemplo:

- Los usuarios deben volver a escribir las credenciales porque la contraseña expiró.
- La aplicación solicita acceso a un recurso para el que se necesita el consentimiento del usuario.
- Se requiere la autenticación en dos fases.

La llamada a `acquireTokenPopup` abre una ventana emergente (o `acquireTokenRedirect` redirige a los usuarios al punto de conexión de la Plataforma de identidad de Microsoft). En esa ventana, los usuarios tienen que interactuar confirmando sus credenciales, dándole el consentimiento al recurso requerido o completando la autenticación en dos fases.

#### <a name="get-a-user-token-silently"></a>Obtención de un token de usuario en silencio

El método `acquireTokenSilent` controla la renovación y las adquisiciones de tokens sin la interacción del usuario. Después de ejecutar `loginPopup` (o `loginRedirect`) por primera vez, `acquireTokenSilent` es el método que se usa habitualmente para obtener los tokens que se utilizan para acceder a los recursos protegidos en las llamadas posteriores. (Las llamadas para solicitar o renovar tokens se realizan en modo silencioso). `acquireTokenSilent` puede dar error en algunos casos. Por ejemplo, la contraseña del usuario puede haber expirado. La aplicación puede abordar esta excepción de dos maneras:

1. Puede realizar una llamada de inmediato a `acquireTokenPopup` para desencadenar un mensaje de inicio de sesión de usuario. Este patrón se da comúnmente en aplicaciones en línea en las que no hay ningún contenido no autenticado en la aplicación disponible para el usuario. El ejemplo generado por esta instalación guiada usa este patrón.

1. Puede indicar visualmente al usuario que se requiere un inicio de sesión interactivo, de manera que el usuario pueda seleccionar el momento adecuado para iniciar sesión, o bien puede reintentar ejecutar el método `acquireTokenSilent` en un momento posterior. Esta técnica se suele usar cuando el usuario puede utilizar otra funcionalidad de la aplicación sin ser interrumpido. Por ejemplo, podría haber contenido no autenticado disponible en la aplicación. En esta situación, el usuario puede decidir cuándo desea iniciar sesión para acceder al recurso protegido o para actualizar la información obsoleta.

> [!NOTE]
> Esta guía de inicio rápido usa los métodos `loginPopup` y `acquireTokenPopup` de forma predeterminada. Si usa Internet Explorer como explorador, se recomienda usar los métodos `loginRedirect` y `acquireTokenRedirect`, debido a un [problema conocido](https://github.com/AzureAD/microsoft-authentication-library-for-js/wiki/Known-issues-on-IE-and-Edge-Browser#issues) relacionado con la forma en la que Internet Explorer controla las ventanas emergentes. Si desea ver cómo lograr el mismo resultado mediante métodos de redireccionamiento, consulte [*authRedirect.js*](https://github.com/Azure-Samples/active-directory-javascript-graphapi-v2/blob/quickstart/JavaScriptSPA/authRedirect.js).

## <a name="call-the-microsoft-graph-api-by-using-the-token-you-just-acquired"></a>Llamada a Microsoft Graph API con el token que acaba de obtener


 A continuación, cree un archivo .js llamado *graph.js*, que realizará una llamada REST a Microsoft Graph API, y agregue el código siguiente:

   ```javascript

// Helper function to call MS Graph API endpoint
// using authorization bearer token scheme
function callMSGraph(endpoint, token, callback) {
    const headers = new Headers();
    const bearer = `Bearer ${token}`;

    headers.append("Authorization", bearer);

    const options = {
        method: "GET",
        headers: headers
    };

    console.log('request made to Graph API at: ' + new Date().toString());

    fetch(endpoint, options)
        .then(response => response.json())
        .then(response => callback(response, endpoint))
        .catch(error => console.log(error));
}
   ```

### <a name="more-information-about-making-a-rest-call-against-a-protected-api"></a>Más información acerca de cómo realizar una llamada de REST a una API protegida

En esta aplicación de ejemplo que se crea en esta guía, el método `callMSGraph()` se usa para realizar una solicitud HTTP `GET` a un recurso protegido que requiere un token. A continuación, el método devuelve el contenido al autor de la llamada. Este método agrega el token adquirido al *encabezado de autorización HTTP*. En este ejemplo de aplicación creada en esta guía, el recurso es el punto de conexión *me* de Microsoft Graph API, que muestra información del perfil del usuario.

## <a name="test-your-code"></a>Prueba del código

1. En Node.js, inicie el servidor web mediante la ejecución de los siguientes comandos desde la carpeta de la aplicación:

   ```bash
   npm install
   npm start
   ```
1. En el explorador, escriba **http://localhost:3000** o **http://localhost:{port}** , donde *port* es el puerto en el que escucha el servidor web. Verá el contenido del archivo *index.html* y el botón **Iniciar sesión**.

## <a name="test-your-application"></a>Prueba de la aplicación

Cuando el explorador haya cargado el archivo *index.html*, seleccione **Iniciar sesión**. Se le pedirá que inicie sesión con el punto de conexión de la plataforma de identidad de Microsoft:

![La ventana de inicio de sesión en la cuenta de SPA de JavaScript](media/active-directory-develop-guidedsetup-javascriptspa-test/javascriptspascreenshot1.png)

### <a name="provide-consent-for-application-access"></a>Consentimiento para el acceso a la aplicación

La primera vez que inicie sesión en la aplicación, se le pedirá que le conceda acceso a su perfil e inicie su sesión:

![La ventana "Permisos solicitados"](media/active-directory-develop-guidedsetup-javascriptspa-test/javascriptspaconsent.png)

### <a name="view-application-results"></a>Visualización de los resultados de la aplicación

Una vez iniciada la sesión, la información del perfil de usuario se devuelve en la respuesta de Microsoft Graph API que se muestra en la página:

![Resultados de la llamada a Microsoft Graph API](media/active-directory-develop-guidedsetup-javascriptspa-test/javascriptsparesults.png)

### <a name="more-information-about-scopes-and-delegated-permissions"></a>Más información sobre los ámbitos y permisos delegados

Microsoft Graph API requiere el ámbito *user.read* para leer el perfil del usuario. De forma predeterminada, este ámbito se agrega automáticamente en todas las aplicaciones que se registran en Azure Portal. Otras API de Microsoft Graph, así como las API personalizadas para el servidor back-end, pueden requerir ámbitos adicionales. Por ejemplo, Microsoft Graph API requiere el ámbito *Mail.Read* para mostrar los correos electrónicos del usuario.

> [!NOTE]
> Es posible que se pida al usuario algún consentimiento adicional a medida que agrega ámbitos.

Si una API de back-end no requiere un ámbito (no se recomienda), puede usar *clientId* como ámbito en las llamadas de adquisición de tokens.

[!INCLUDE [Help and support](../../../includes/active-directory-develop-help-support-include.md)]

## <a name="next-steps"></a>Pasos siguientes

El [repositorio de GitHub de MSAL.js](https://github.com/AzureAD/microsoft-authentication-library-for-js) contiene documentación adicional de la biblioteca y una sección de preguntas frecuentes, además de ofrecer soporte técnico para los problemas.

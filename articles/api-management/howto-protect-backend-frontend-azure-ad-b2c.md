---
title: Protección del back-end de SPA con OAuth 2.0 mediante Azure Active Directory B2C y Azure API Management.
description: Proteja una API con OAuth 2.0 mediante Azure Active Directory B2C, Azure API Management e Easy Auth para llamarla desde una aplicación de página única de JavaScript.
services: api-management, azure-ad-b2c, app-service
documentationcenter: ''
author: WillEastbury
manager: alberts
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/20/2020
ms.author: wieastbu
ms.custom: fasttrack-new
ms.openlocfilehash: 55acea360de11c5fcc699d65daf92cf24dfd691d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "79475483"
---
# <a name="protect-spa-backend-with-oauth-20-azure-active-directory-b2c-and-azure-api-management"></a>Protección del back-end de SPA con OAuth 2.0, Azure Active Directory B2C y Azure API Management

En este escenario se muestra cómo configurar la instancia de Azure API Management para proteger una API.
Vamos a utilizar el protocolo OAuth 2.0 con Azure AD B2C junto con API Management para proteger un back-end de Azure Functions mediante Easy Auth.

## <a name="aims"></a>Objetivos
Vamos a ver cómo se puede usar API Management en un escenario simplificado con Azure Functions y Azure AD B2C. Creará una aplicación JavaScript (JS) que llama a una API que inicie la sesión de los usuarios con Azure AD B2C. A continuación, usará las características de la directiva validate-jwt de API Management para proteger la API de back-end.

Para una defensa minuciosa, usará Easy Auth para validar el token de nuevo en la API de back-end.

## <a name="prerequisites"></a>Prerrequisitos
Para seguir los pasos de este artículo, debe tener:
* Una cuenta de almacenamiento de Azure (StorageV2) de uso general V2 para hospedar la aplicación de página única JS de front-end
* Una instancia de Azure API Management 
* Una aplicación de Azure Functions vacía (que ejecute el entorno de tiempo de ejecución de .NET Core V2 en un plan de consumo de Windows) para hospedar la API llamada
* Un inquilino de Azure AD B2C vinculado a una suscripción 

Aunque en la práctica usaría recursos en la misma región en cargas de trabajo de producción, para este artículo de procedimientos, la región de implementación no importa.

## <a name="overview"></a>Información general
A continuación se muestra una ilustración de los componentes en uso y el flujo entre ellos una vez completado este proceso.
![Componentes en uso y flujo](../api-management/media/howto-protect-backend-frontend-azure-ad-b2c/image-arch.png "Componentes en uso y flujo")

Aquí se muestra una introducción rápida de los pasos:

1. Cree las aplicaciones de llamada de Azure AD B2C (front-end, API Management) y de API con ámbitos, y concédales acceso de API
1. Cree las directivas de registro o inicio de sesión para permitir que los usuarios inicien sesión con Azure AD B2C 
1. Configure API Management con los nuevos identificadores y claves de cliente de Azure AD B2C para habilitar la autorización de usuario de OAuth2 en la consola del desarrollador
1. Compile Function API
1. Configure Function API para habilitar Easy Auth con el nuevo identificador y las claves de cliente de Azure AD B2C y realice el bloqueo para la IP virtual de APIM 
1. Compile la definición de API en API Management
1. Configure Oauth2 para la configuración de la API de API Management
1. Configure la directiva**CORS** y agregue la **validate-jwt** para validar el token de OAuth para todas las solicitudes entrantes
1. Compile la aplicación que realiza la llamada para que consuma la API
1. Cargue el ejemplo de aplicación JS de página única
1. Configure aplicación JS cliente de ejemplo con las nuevas claves y el nuevo identificador de cliente de Azure AD B2C 
1. Pruebe la aplicación cliente

## <a name="configure-azure-ad-b2c"></a>Configuración de Azure AD B2C 
Abra la hoja Azure AD B2C en el portal y realice los pasos siguientes.
1. Seleccione la pestaña **Aplicaciones**. 
1. Haga clic en el botón "Agregar" y cree tres aplicaciones para los siguientes fines:
   * Cliente front-end.
   * API de función de back-end.
   * [Opcional] Portal para desarrolladores de API Management (a menos que ejecute Azure API Management en el nivel de consumo, escenario sobre el que se habla más adelante).
1. Defina los valores de Aplicación web o API web para las tres aplicaciones y establezca "Permitir flujo implícito" en Sí solo en el cliente front-end.
1. Ahora establezca el URI del identificador de aplicación, que sea único y pertinente para el servicio que se va a crear.
1. Por el momento, use los marcadores de posición para las direcciones URL de respuesta, como https://localhost, las actualizaremos más adelante.
1. Haga clic en "Crear" y repita los pasos 2-5 para cada una de las tres aplicaciones anteriores. Registre el URI de AppID, el nombre y el identificador de aplicación para su uso posterior para las tres aplicaciones.
1. Abra la aplicación Portal para desarrolladores de API Management desde la lista de aplicaciones, seleccione la pestaña *Claves* (situada en General) y haga clic en "Generar clave" para generar una clave de autenticación.
1. Tras hacer clic en Guardar, registre la clave en un lugar seguro para su uso posterior. Tenga en cuenta que este lugar será el ÚNICO donde podrá ver y copiar la clave.
1. Ahora, seleccione la pestaña *Ámbitos publicados* (en Acceso de API).
1. Cree y asígnele un nombre a un ámbito para la API de función, registre el ámbito y el valor completo del ámbito y haga clic en "Guardar".
   > [!NOTE]
   > Los ámbitos de Azure AD B2C son permisos dentro de la API a los que otras aplicaciones pueden solicitar acceso mediante la hoja de acceso a API de sus aplicaciones.
1. Abra las otras dos aplicaciones y mire en la pestaña *Acceso de API*.
1. Concédales acceso al ámbito de la API de back-end y al predeterminado que ya estaba allí ("Access the user's profile" [Acceso al perfil del usuario]).
1. Genere una clave para cada una; para ello, seleccione la pestaña *Claves* en "General", de este modo podrá generar las claves de autenticación y registrarlas en un lugar seguro para más adelante.

## <a name="create-a-sign-up-or-sign-in-user-flow"></a>Creación de un flujo de usuario "Registrarse e iniciar sesión"
1. Vuelva a la raíz (o "Información general") de la hoja Azure AD B2C. 
1. Seleccione "Flujos de usuario (directivas)" y elija "Nuevo flujo de usuario".
1. Seleccione el tipo de flujo de usuario "Registrarse e iniciar sesión".
1. Asígnele un nombre a la directiva y regístrelo para más adelante.
1. A continuación, en "Proveedores de identidades", active "Registro de id. de usuario" (puede aparecer como "Email sign up" [Registro con correo electrónico]) y haga clic en Aceptar. 
1. En "User Attributes and claims" (Notificaciones y atributos de usuario), haga clic en "Mostrar más...", elija las opciones de notificaciones que desee que los usuarios escriban y reciban de vuelta en el token. Compruebe como mínimo "Nombre para mostrar" y "Dirección de correo electrónico" para recopilar y devolver, y haga clic en "Aceptar" y en "Crear".
1. Seleccione la directiva que creó en la lista y haga clic en el botón "Ejecutar flujo de usuario".
1. Con esta acción se abrirá la hoja Ejecutar flujo de usuario. Seleccione la aplicación de front-end y registre la dirección del dominio b2clogin.com que se muestra en la lista desplegable "Seleccionar un dominio".
1. Haga clic en el vínculo de la parte superior para abrir "Well-known OpenId configuration endpoint" (Punto de conexión de configuración de OpenID conocido) y registre los valores de authorization_endpoint y token_endpoint, así como el valor del vínculo en sí como punto de conexión de configuración de OpenID conocido.

   > [!NOTE]
   > Las directivas de B2C permiten exponer los puntos de conexión de inicio de sesión de Azure AD B2C para poder capturar diferentes componentes de datos e iniciar la sesión de los usuarios de diferentes maneras. En este caso hemos configurado un punto de conexión de registro o de inicio de sesión, que expuso un punto de conexión de configuración conocido, en concreto, la directiva creada se identificó en la dirección URL mediante el parámetro p=.
   > 
   > Una vez hecho esto tendrá una plataforma de identidad B2C funcional para que los usuarios inicien sesión en varias aplicaciones. 
   > Si lo desea, haga clic en el botón "Ejecutar flujo de usuario" aquí (para pasar por el proceso de registro o de inicio de sesión) y hacerse una idea de lo que sería en la práctica, pero se producirá un error en el paso de redireccionamiento al final porque la aplicación no se ha implementado todavía.

## <a name="build-the-function-api"></a>Compilación de la API de función
1. Vuelva a cambiar al inquilino de Azure AD estándar en Azure Portal para poder configurar de nuevo los elementos de la suscripción. 
1. Vaya a la hoja Aplicaciones de funciones de Azure Portal, abra la aplicación de función vacía y cree una función en el portal "Webhook + API" con el inicio rápido.
1. Pegue el código de ejemplo siguiente en Run.csx sobre el código existente que aparece.

   ```csharp
   
   using System.Net;
   using Microsoft.AspNetCore.Mvc;
   using Microsoft.Extensions.Primitives;
   
   public static async Task<IActionResult> Run(HttpRequest req, ILogger log)
   {
      log.LogInformation("C# HTTP trigger function processed a request.");
      
      return (ActionResult)new OkObjectResult($"Hello World, time and date are {DateTime.Now.ToString()}");
   }
   
   ```

   > [!NOTE]
   > El código de la función de script de C# que acaba de pegar simplemente registra una línea en los registros de función y devuelve el texto "Hola mundo" con algunos datos dinámicos (fecha y hora).

3. Seleccione "Integrar" en la hoja de la izquierda y "Editor avanzado" en la esquina superior derecha del panel.
4. Pegue el código de ejemplo siguiente sobre el JSON existente.

   ```json
   {
      "bindings": [
       {
        "authLevel": "function",
        "name": "req",
        "type": "httpTrigger",
        "direction": "in",
        "methods": [
           "get"
        ],
        "route": "hello"
       },
       {
         "name": "$return",
         "type": "http",
         "direction": "out"
       }
     ]
   }
   ```

5. Vuelva a la pestaña HttpTrigger1, haga clic en "Obtener la dirección URL de la función" y copie la dirección URL que aparece.

   > [!NOTE]
   > Los enlaces que acaba de crear simplemente indican a las funciones que respondan a solicitudes HTTP GET anónimas a la dirección URL que acaba de copiar. (https://yourfunctionappname.azurewebsites.net/api/hello?code=secretkey) Ahora tenemos una API HTTPS sin servidor escalable capaz de devolver una carga muy sencilla.
   > Ahora puede probar la llamada a esta API desde un explorador web mediante la dirección URL anterior. Por otra parte, también puede quitar la parte del secreto ?code=secret de la dirección URL y demostrar que Azure Functions devuelve un error 401.

## <a name="configure-and-secure-the-function-api"></a>Configuración y protección de la API de función
1. Es necesario configurar dos áreas adicionales en la aplicación de funciones (restricciones de autenticación y de red).
1. En primer lugar, vamos a configurar la autenticación/autorización, por lo que haremos clic en el nombre de la aplicación de funciones (junto al icono &lt;Z&gt; de Functions) para mostrar la página de información general.
1. A continuación, seleccione la pestaña "Platform features" (Características de la plataforma) y "Autenticación/Autorización".
1. Active la característica de autenticación de App Service.
1. En "Proveedores de autenticación", elija "Azure Active Directory" y "Avanzado" en el conmutador del modo de administración.
1. Pegue el identificador de aplicación de la API de Backend Function que aparece en Azure AD B2C en el cuadro "Id. de cliente".
1. Pegue el punto de conexión de configuración OpenID de la directiva de registro o inicio de sesión en el cuadro URL del emisor (hemos registrado esta configuración anteriormente).
1. Seleccione Aceptar.
1. Establezca la acción que desea realizar cuando la solicitud no se autentique en el cuadro desplegable "Iniciar sesión con Azure Active Directory" y haga clic en "Guardar".

   > [!NOTE]
   > Ahora, la API de Function está implementada, por lo que debe generar respuestas 401 si no se proporciona la clave correcta y devolver datos cuando se presente una solicitud correcta.
   > Ha agregado una medida de seguridad de defensa en profundidad adicional en EasyAuth configurando la opción "Login With Azure AD" (Inicio de sesión con Azure AD) para administrar las solicitudes no autenticadas. Tenga en cuenta que esto cambiará el comportamiento de las solicitudes no autorizadas entre la aplicación BackEnd Function y la SPA Frontend, ya que EasyAuth emitirá un redireccionamiento 302 a AAD en lugar de una respuesta 401 no autorizada. Corregiremos este comportamiento más adelante con API Management.
   > Sin embargo, aún no hemos aplicado la seguridad IP. Si tiene una clave válida y un token de OAuth2, cualquiera puede realizar la llamada desde cualquier lugar (lo ideal sería exigir que todas las solicitudes lleguen a través de API Management).
   > Si usa el nivel de consumo de API Management, no podrá realizar este bloqueo por IP virtual, ya que no hay ninguna dirección IP estática dedicada para ese nivel. Deberá utilizar el método que bloquea las llamadas API con la clave de función secreta compartida, por lo que no se podrán realizar los pasos 11-13.

1. Cierre la hoja "Autenticación/Autorización". 
1. Seleccione "Redes" y "Restricciones de acceso".
1. A continuación, bloquee las direcciones IP de la aplicación de funciones permitidas a la IP virtual de la instancia de API Management. Esta IP virtual se muestra en la sección de información general de API Management del portal.
1. Si desea seguir interactuando con el portal de funciones y llevar a cabo los pasos opcionales que se indican a continuación, primero debe agregar su propia dirección IP pública o intervalo CIDR.
1. Cuando haya una entrada de permiso en la lista, Azure agrega una regla de denegación implícita para bloquear todas las demás direcciones. 

Deberá agregar bloques de direcciones con formato CIDR al panel restricciones de IP. Si tiene que agregar una sola dirección, como la IP virtual de API Management, debe hacerlo con el formato xx.xx.xx.xx.

   > [!NOTE]
   > Ahora la API de función no debería estar accesible por llamada desde otro origen distinto de API Management o su dirección.
   
## <a name="import-the-function-app-definition"></a>Importación de la definición de la aplicación de funciones
1. Abra la hoja *API Management* y después *la instancia*.
1. Seleccione la hoja API en la sección API Management de la instancia.
1. En el panel "Add a New API" (Agregar una nueva API), elija "Function App" y seleccione "Completo" en la parte superior de la ventana emergente.
1. Haga clic en Examinar, elija la aplicación de funciones que hospede la API y haga clic en Seleccionar.
1. Proporciónele un nombre a la API y una descripción para uso interno de API Management y agréguelos al producto "Unlimited" (Ilimitado).
1. Asegúrese de registrar la dirección URL de base para su uso posterior y haga clic en Crear.

## <a name="configure-oauth2-for-api-management"></a>Configuración de Oauth2 para API Management

1. Después, seleccione la hoja OAuth 2.0 en la pestaña Seguridad y haga clic en "Agregar".
1. Proporcione valores para *Nombre para mostrar* y *Descripción* para el punto de conexión de OAuth agregado (estos valores se mostrarán en el siguiente paso como punto de conexión de Oauth2).
1. Puede escribir cualquier valor en la dirección URL de la página de registro del cliente, ya que este valor no se usará.
1. Active el tipo de concesión *Implicit Auth* (Autenticación implícita) y desactive el tipo de concesión Código de autorización.
1. Vaya a los campos del punto de conexión *Autorización* y *Token*, y escriba los valores que capturó en el documento XML de configuración conocido anterior.
1. Desplácese hacia abajo y rellene un *parámetro de cuerpo adicional* denominado "Recurso" con el identificador de cliente de la API de Backend Function del registro de la aplicación de Azure AD B2C.
1. Seleccione "Credenciales de cliente", establezca el identificador de cliente en el identificador de la aplicación de consola de desarrollador (omita este paso si usa el modelo de consumo de API Management).
1. Establezca el secreto de cliente en la clave que registró anteriormente. Omita este paso si usa el modelo de consumo de API Management.
1. Por último, registre el valor de redirect_urila de la concesión de código de autenticación de API Management para su uso posterior.

## <a name="set-up-oauth2-for-your-api"></a>Configuración de Oauth2 para la API
1. La API aparecerá en el lado izquierdo del portal en la sección "Todas las API"; haga clic en ella para abrirla.
1. Seleccione la pestaña "Configuración".
1. Para actualizar la configuración, seleccione "OAuth 2.0" en el botón de radio de autorización de usuario.
1. Seleccione el servidor OAuth que definió anteriormente.
1. Active la casilla "Override scope" (Invalidar ámbito) y escriba el ámbito que registró antes para la llamada API de back-end.

   > [!NOTE]
   > Ahora tenemos una instancia de API Management que sabe cómo obtener tokens de acceso de Azure AD B2C para autorizar solicitudes y conoce la configuración para Azure Active Directory B2C de Oauth2.

## <a name="set-up-the-cors-and-validate-jwt-policies"></a>Configuración de las directivas **CORS** y **validate-jwt**

> Se deben seguir las siguientes secciones independientemente del nivel de APIM que se use. 

1. Vuelva a la pestaña de diseño y elija "Todas las API". Haga clic en el botón de vista de código para mostrar el editor de directivas.
1. Edite la sección de entrada y pegue el código XML siguiente para que sea similar al siguiente.

   ```xml
   <inbound>
      <validate-jwt header-name="Authorization" failed-validation-httpcode="401" failed-validation-error-message="Unauthorized. Access token is missing or invalid.">
         <openid-config url="https://tenant.b2clogin.com/tenant.onmicrosoft.com/v2.0/.well-known/openid-configuration?p=B2C_1_MyDefaultPolicy" />
         <required-claims>
            <claim name="aud">
               <value>your-backend-api-application-client-id</value>
            </claim>
         </required-claims>
      </validate-jwt>
      <cors>
         <allowed-origins>
            <origin>*</origin>
         </allowed-origins>
         <allowed-methods>
           <method>GET</method>
         </allowed-methods>
         <allowed-headers>
            <header>*</header>
         </allowed-headers>
         <expose-headers>
           <header>*</header>
         </expose-headers>
       </cors>
   </inbound>
   ```
1. Edite la dirección URL de openid-config para que coincida con el punto de conexión de Azure AD B2C conocido de la directiva de registro o de inicio de sesión.
1. Edite el valor de la notificación para que coincida con el identificador de aplicación válido, también conocido como identificador de cliente para la aplicación de API de back-end, y guárdelo.

   > [!NOTE]
   > Ahora API Management podrá responder a las solicitudes de origen cruzado a las aplicaciones JS de página única y realizará la limitación, la limitación de velocidad y la validación previa del token de autenticación JWT que se pasa ANTES de reenviar la solicitud a la API de función.

   > [!NOTE]
   > La sección siguiente es opcional y no es aplicable al nivel **Consumo**, que no es compatible con el portal para desarrolladores.
   > Si no va a usar el portal para desarrolladores o no puede usarlo, ya que usa el nivel Consumo, omita este paso y vaya directamente a ["Compilación de la aplicación JavaScript de página única para consumir la API"](#build-the-javascript-spa-to-consume-the-api).

## <a name="optional-configure-the-developer-portal"></a>[Opcional] Configuración del portal para desarrolladores

1. Abra la hoja Azure AD B2C y vaya al registro de la aplicación del portal para desarrolladores.
1. Establezca la entrada "URL de respuesta" en la que anotó cuando configuró redirect_uri de la concesión de código de autenticación en API Management anteriormente.

   Ahora que la autorización de usuario OAuth 2.0 está habilitada en `Echo API`, la consola del desarrollador obtiene un token de acceso para el usuario antes de llamar a la API.

1. Vaya a cualquier operación de `Echo API` en el portal para desarrolladores y seleccione **Probar** para ir a la consola del desarrollador.
1. Tenga en cuenta el nuevo elemento de la sección **Autorización** correspondiente al servidor de autorización que acaba de agregar.
1. Seleccione **Código de autorización** de la lista desplegable de autorizaciones y se le pedirá que inicie sesión en el inquilino de Azure AD. Si ya ha iniciado sesión con la cuenta, es posible que no se le solicite nada.
1. Tras iniciar sesión correctamente, un encabezado `Authorization: Bearer` se agregará a la solicitud con un token de acceso de Azure AD B2C cifrado en Base64. 
1. Seleccione **Enviar** y podrá llamar a la API correctamente.

   > [!NOTE]
   > Ahora API Management es capaz de adquirir tokens para el portal para desarrolladores con el fin de probar la API. Además conoce su definición y presenta la página de prueba adecuada en el portal para desarrolladores.

1. En la hoja de información general del portal de API Management, haga clic en "Portal para desarrolladores" para iniciar sesión como administrador de la API.
1. Aquí, usted y otros consumidores seleccionados de la API pueden probarla y llamarla desde una consola.
1. Seleccione "Productos", elija "Unlimited" (Ilimitado) y la API que creada anteriormente, y haga clic en "PROBAR".
1. Muestre la clave de suscripción de API y cópiela en lugar seguro junto con la dirección URL de la solicitud que necesitará más adelante.
1. Seleccione también Implícito en la lista desplegable autenticación de OAuth y es posible que tenga que autenticarse aquí en un elemento emergente.
1. Haga clic en "Enviar" y, si todo está bien, la aplicación de funciones debería responder con un mensaje de saludo mediante API Management, con un mensaje del código de estado 200 OK y JSON.

   > [!NOTE]
   > Enhorabuena, ahora tiene Azure AD B2C, API Management y Azure Functions trabajando juntos para publicar, proteger y consumir una API. Es posible que haya observado que la API está protegida dos veces con este método, una con el encabezado Ocp-Subscription-Key de API Management y otra con la autorización: Bearer de JWT.
   > Sería correcto; como este ejemplo es una aplicación JavaScript de página única, la clave de API Management solo se usa para las llamadas de facturación y de limitación de velocidad.
   > La autenticación y la autorización reales se administran mediante Azure AD B2C y se encapsulan en JWT, que se valida dos veces, una desde API Management y, a continuación, desde Azure Functions.

## <a name="build-the-javascript-spa-to-consume-the-api"></a>Compilación de la aplicación JavaScript de página única para consumir la API
1. Abra la hoja Cuentas de almacenamiento en Azure Portal. 
1. Seleccione la cuenta que ha creado y la hoja "Sitio web estático" en la sección Configuración (si no ve la opción "Sitio web estático", compruebe que creó una cuenta V2).
1. Establezca la característica de hospedaje web estático en "Habilitado" y dele a documento de índice el nombre "index.html". Después, haga clic en "Guardar".
1. Anote el contenido del punto de conexión principal, ya que esta ubicación es donde se hospedará el sitio de front-end. 

   > [!NOTE]
   > Puede usar Azure Blob Storage + reescritura de la red CDN o Azure App Service, pero la característica de hospedaje de sitios web estáticos de Blob Storage proporciona un contenedor predeterminado que sirve contenido/html/JS/css web estático de Azure Storage e infiere una página predeterminada para que no tengamos que hacer nada.

## <a name="upload-the-js-spa-sample"></a>Carga del ejemplo de aplicación JS de página única
1. Todavía en la hoja de la cuenta de almacenamiento, seleccione la hoja "Blobs" en la sección Blob service y haga clic en el contenedor $web que aparece en el panel derecho.
1. Guarde el código siguiente en un archivo de la máquina como index.html y cargue este archivo en el contenedor $web.

   ```html
   <!doctype html>
   <html lang="en">
   <head>
        <meta charset="utf-8">
        <meta http-equiv="X-UA-Compatible" content="IE=edge">
        <meta name="viewport" content="width=device-width, initial-scale=1">
        <title>Sample JS SPA</title>
        <link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/4.0.0/css/bootstrap.min.css" integrity="sha384-Gn5384xqQ1aoWXA+058RXPxPg6fy4IWvTNh0E263XmFcJlSAwiGgFAW/dAiS6JXm" crossorigin="anonymous">
   </head>
   <body>
        <div class="container-fluid">
            <div class="row">
                <div class="col-md-12">
                    <nav class="navbar navbar-expand-lg navbar-light bg-light navbar-dark bg-dark">
                        <a class="navbar-brand" href="#">Sample Code</a>
                        <ul class="navbar-nav ml-md-auto">
                            <li class="nav-item dropdown">
                                <a class="btn btn-large btn-success" onClick="login()">Sign In</a>
                            </li>
                        </ul>
                    </nav>
                </div>
            </div>
            <div class="row">
                <div class="col-md-12">
                    <div class="jumbotron">
                        <h2>
                    <div id="message">Hello, world!</div>
                    </h2>
                        <p>
                            <a class="btn btn-primary btn-large" onClick="GetAPIData()">Call API</a>
                        </p>
                    </div>
                </div>
            </div>
        </div>
        <script src="https://code.jquery.com/jquery-3.2.1.min.js"></script>
        <script src="https://cdnjs.cloudflare.com/ajax/libs/popper.js/1.12.9/umd/popper.min.js" integrity="sha384-ApNbgh9B+Y1QKtv3Rn7W3mgPxhU9K/ScQsAP7hUibX39j7fakFPskvXusvfa0b4Q" crossorigin="anonymous"></script>
        <script src="https://maxcdn.bootstrapcdn.com/bootstrap/4.0.0/js/bootstrap.min.js" integrity="sha384-JZR6Spejh4U02d8jOt6vLEHfe/JQGiRRSQQxSfFWpi1MquVdAyjUar5+76PVCmYl" crossorigin="anonymous"></script>
        <script src="https://secure.aadcdn.microsoftonline-p.com/lib/1.0.0/js/msal.js"></script>
        <script lang="javascript">
            var applicationConfig = {
                clientID: "clientidgoeshere",
                authority: "https://tenant.b2clogin.com/tfp/tenant/policy",
                b2cScopes: ["https://tenant/app/scope"],
                webApi: 'http://functionurl',
                subKey: 'apimkeygoeshere'
            };
            var msalConfig = {
                auth: {
                        clientId: applicationConfig.clientID, 
                        authority:  applicationConfig.authority,
                        validateAuthority: false
                },
                cache: {
                        cacheLocation: "localStorage",
                        storeAuthStateInCookie: true
                }
            };
            var clientApplication = new Msal.UserAgentApplication(msalConfig);
            function login() {
                var loginRequest = {
                    scopes: applicationConfig.b2cScopes
                };
                clientApplication.loginPopup(loginRequest).then(function (loginResponse) {
                    var tokenRequest = {
                        scopes: applicationConfig.b2cScopes
                    };
                    clientApplication.acquireTokenSilent(tokenRequest).then(function (tokenResponse) {
                        document.getElementById("signinbtn").innerHTML = "Logged in as: " + clientApplication.account.name;
                        document.getElementById("callapibtn").hidden = false
                        }).catch(function (error) {
                            clientApplication.acquireTokenPopup(tokenRequest).then(function (tokenResponse) {
                                }).catch (function (error) {
                                    console.log("Error acquiring the popup:\n" + error);
                                });
                        })
                    }).catch (function (error) {
                        console.log("Error during login:\n" + error);
                });
            }
            function GetAPIData() {
                var tokenRequest = {
                    scopes: applicationConfig.b2cScopes
                }
                clientApplication.acquireTokenSilent(tokenRequest).then(function (tokenResponse) {
                    callApiWithAccessToken(tokenResponse.accessToken);
                    }).catch(function (error) {
                        clientApplication.acquireTokenPopup(tokenRequest).then(function (tokenResponse) {
                            callApiWithAccessToken(tokenResponse.accessToken);
                            
                        }).catch(function (error) {
                            console.log("Error acquiring the access token to call the Web api:\n" + error);
                        });
                    })
            }
            function callApiWithAccessToken(token)
            {
                console.log("calling "  + applicationConfig.webApi +  " with " + token);
                    // Make the api call here
                $.ajax({
                    type: "get",
                    headers: {'Authorization': 'Bearer ' + token, 'Ocp-Apim-Subscription-Key': applicationConfig.subKey},                   url: applicationConfig.webApi
                }
                ).done(function (body) {
                    document.getElementById("message").innerHTML = "The API Said " + body;
                });
            }
        </script>
    </body>
   </html>
   
   ```

1. Vaya al punto de conexión principal del sitio web estático que almacenó en la última sección.

   > [!NOTE]
   > Enhorabuena, acaba de implementar una aplicación JavaScript de página única en Azure Storage. Como no hemos configurado la aplicación JS con sus claves para la API ni con los detalles de Azure AD B2C todavía, en este momento la página no funcionará al abrirla.

## <a name="configure-the-js-spa-for-azure-ad-b2c"></a>Configuración de la aplicación JS de página única para Azure AD B2C
1. Ahora sabemos dónde está todo: podemos configurar la aplicación de página única con la dirección de API de API Management adecuada y los identificadores de aplicación y de cliente de Azure AD B2C correctos.
1. Vuelva a la hoja de almacenamiento de Azure Portal, haga clic en index.html y elija "Editar blob". 
1. Actualice los detalles de autenticación para que coincidan con la aplicación de front-end que registró anteriormente en B2C, teniendo en cuenta que los valores "b2cScopes" son para el back-end de la API.
1. La clave de API web y la dirección URL de la API se encuentran en el panel de prueba de API Management para la operación de API.
1. Cree una clave de suscripción de APIM; para ello, vaya a la hoja API Management de API Management, seleccione "Suscripciones" y haga clic en "Agregar suscripción"; finalmente, guarde el registro. Al hacer clic en los puntos suspensivos (...) junto a la fila creada, podrá mostrar las claves para poder copiar la clave principal.
1. Debe tener un aspecto similar al código siguiente:  

   ```javascript
   var applicationConfig =
      clientID: "{aadb2c-clientid-goeshere}",
      authority: "https://{tenant}.b2clogin.com/{tenant}/{policy}",
      b2cScopes: ["https://{tenant}/{app}/{scope}"], 
      webApi: 'http://{apim-url-for-your-function}',
      subKey: '{apim-subscription-key-goes-here}'
   };
   ```

1. Haga clic en Guardar

## <a name="set-the-redirect-uris-for-the-azure-ad-b2c-frontend-app"></a>Establecimiento de los URI de redireccionamiento para la aplicación de front-end de Azure AD B2C
1. Abra la hoja Azure AD B2C y vaya al registro de la aplicación JavaScript de front-end.
1. Establezca la dirección URL de redireccionamiento en la que anotó al configurar el punto de conexión principal del sitio web estático.

   > [!NOTE] 
   > Esta configuración hará que un cliente de la aplicación de front-end reciba un token de acceso con las notificaciones adecuadas de Azure AD B2C.
   > La aplicación de página única podrá agregarlo como token de portador al encabezado HTTPS en la llamada a la API de back-end.
   > API Management validará previamente el token y limitará la velocidad de las llamadas al punto de conexión de la clave del suscriptor antes de pasar la solicitud a la API de función de Azure receptora.
   > La aplicación de página única representará la respuesta en el explorador.

   > *Enhorabuena, ha configurado Azure AD B2C, Azure API Management, Azure Functions y la autorización de Azure App Service para que trabajen en perfecta armonía.*

   > [!NOTE]
   > Ahora tenemos una aplicación sencilla con una API segura sencilla, vamos a probarla.

## <a name="test-the-client-application"></a>Prueba de la aplicación cliente
1. Abra la dirección URL de la aplicación de ejemplo que anotó de la cuenta de almacenamiento que creó anteriormente.
1. Haga clic en "Iniciar sesión" en la esquina superior derecha, lo que hará que aparezca su perfil de registro o de inicio de sesión de Azure AD B2C emergente.
1. Después del inicio de sesión, la sección "Ha iniciado sesión como" de la pantalla se rellenará con los datos de JWT.
1. Ahora haga clic en "Call Web API" (Llamar a API web) y la página debería actualizarse con los valores devueltos desde la API protegida.

## <a name="and-were-done"></a>Y hemos terminado
Los pasos anteriores se pueden adaptar y editar para permitir gran cantidad de usos diferentes de Azure AD B2C con API Management.

## <a name="next-steps"></a>Pasos siguientes
* Obtenga más información sobre [Escenarios de autenticación para Azure AD](../active-directory/develop/authentication-scenarios.md).
* Consulte más [vídeos](https://azure.microsoft.com/documentation/videos/index/?services=api-management) sobre la administración de API.
* Para conocer otras formas de proteger el servicio back-end, consulte [Autenticación de certificado mutua](api-management-howto-mutual-certificates.md).
* [Creación de una instancia del servicio de API Management](get-started-create-service-instance.md).
* [Administración de su primera API en Administración de API de Azure](import-and-publish.md).

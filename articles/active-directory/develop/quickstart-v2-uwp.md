---
title: 'Inicio rápido: Inicio de sesión de usuarios y llamada a Microsoft Graph en una aplicación de la Plataforma universal de Windows | Azure'
titleSuffix: Microsoft identity platform
description: En este inicio rápido, obtendrá información sobre cómo una aplicación de la plataforma universal de Windows (UWP) puede obtener un token de acceso y llamar a una API protegida por la plataforma de identidad de Microsoft.
services: active-directory
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: quickstart
ms.workload: identity
ms.date: 10/07/2020
ms.author: jmprieur
ms.custom: aaddev, identityplatformtop40, scenarios:getting-started, languages:UWP
ms.openlocfilehash: 28d912153b52580727e0fb5086e0a7ae55e8b545
ms.sourcegitcommit: 04fb3a2b272d4bbc43de5b4dbceda9d4c9701310
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/12/2020
ms.locfileid: "94560934"
---
# <a name="quickstart-call-the-microsoft-graph-api-from-a-universal-windows-platform-uwp-application"></a>Inicio rápido: Llamar a Microsoft Graph API desde una aplicación de la Plataforma universal de Windows (UWP)

En este inicio rápido descargará y ejecutará un código de ejemplo que muestra cómo una aplicación de Plataforma universal de Windows puede realizar el inicio de sesión de usuarios y obtener un token de acceso para llamar a Microsoft Graph API. 

Para ilustrar este tema, consulte el apartado en el que se explica el [funcionamiento del ejemplo](#how-the-sample-works).

> [!div renderon="docs"]
> ## <a name="prerequisites"></a>Prerrequisitos
>
> * Una cuenta de Azure con una suscripción activa. [Cree una cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
> * [Visual Studio 2019](https://visualstudio.microsoft.com/vs/)
>
> ## <a name="register-and-download-your-quickstart-app"></a>Registro y descarga de la aplicación de inicio rápido
> [!div renderon="docs" class="sxs-lookup"]
> Tiene dos opciones para comenzar con la aplicación de inicio rápido:
> * [Rápido] [Opción 1: registrar y configurar de modo automático la aplicación y, a continuación, descargar el código de ejemplo](#option-1-register-and-auto-configure-your-app-and-then-download-your-code-sample)
> * [Manual] [Opción 2: registrar y configurar manualmente la aplicación y el código de ejemplo](#option-2-register-and-manually-configure-your-application-and-code-sample)
>
> ### <a name="option-1-register-and-auto-configure-your-app-and-then-download-your-code-sample"></a>Opción 1: registrar y configurar de modo automático la aplicación y, a continuación, descargar el código de ejemplo
>
> 1. Vaya al nuevo panel de [Azure Portal: Registros de aplicaciones](https://portal.azure.com/#blade/Microsoft_AAD_RegisteredApps/applicationsListBlade/quickStartType/UwpQuickstartPage/sourceType/docs).
> 1. Escriba un nombre para la aplicación y haga clic en **Registrar**.
> 1. Siga las instrucciones para descargar y configurar automáticamente la nueva aplicación en un solo clic.
>
> ### <a name="option-2-register-and-manually-configure-your-application-and-code-sample"></a>Opción 2: registrar y configurar manualmente la aplicación y el código de ejemplo
> [!div renderon="docs"]
> #### <a name="step-1-register-your-application"></a>Paso 1: Registrar su aplicación
> Para registrar la aplicación y agregar la información de registro de la aplicación a la solución, siga estos pasos:
> 1. Inicie sesión en [Azure Portal](https://portal.azure.com) con una cuenta personal, profesional o educativa de Microsoft.
> 1. Si la cuenta proporciona acceso a más de un inquilino, haga clic en la cuenta en la esquina superior derecha y establezca la sesión del portal en el inquilino de Azure AD deseado.
> 1. Vaya a la página [Registros de aplicaciones](https://aka.ms/MobileAppReg) de la plataforma de identidad de Microsoft para desarrolladores.
> 1. Seleccione **Nuevo registro**.
> 1. En **Registrar una aplicación**, escriba la información de registro de la aplicación:
>      - En la sección **Nombre**, escriba un nombre significativo para la aplicación, que se mostrará a los usuarios de la aplicación, por ejemplo, `UWP-App-calling-MsGraph`.
>      - En **Tipos de cuenta admitidos**, seleccione **Cuentas en cualquier directorio de organización y cuentas personales de Microsoft (por ejemplo, Skype, Xbox o Outlook.com)** .
> 1. Seleccione **Registrar** para crear la aplicación y anote el valor de **Id. del cliente de aplicación**, lo usará en un paso posterior.
> 1. En **Administrar**, seleccione **Autenticación**.
> 1. Selecciones **Agregar una plataforma** > **Aplicaciones móviles y de escritorio**.
> 1. En **URI de redirección**, seleccione `https://login.microsoftonline.com/common/oauth2/nativeclient`.
> 1. Seleccione **Configurar**.

> [!div renderon="portal" class="sxs-lookup"]
> #### <a name="step-1-configure-the-application"></a>Paso 1: Configuración de la aplicación
> Para que el código de ejemplo de este inicio rápido funcione, es preciso agregar un identificador URI de redirección como **https://login.microsoftonline.com/common/oauth2/nativeclient** .
> > [!div renderon="portal" id="makechanges" class="nextstepaction"]
> > [Hacer este cambio por mí]()
>
> > [!div id="appconfigured" class="alert alert-info"]
> > ![Ya configurada](media/quickstart-v2-uwp/green-check.png) La aplicación está configurada con estos atributos.

#### <a name="step-2-download-the-visual-studio-project"></a>Paso 2: Descargue el proyecto de Visual Studio

> [!div renderon="docs"]
> [Descargue el proyecto de Visual Studio](https://github.com/Azure-Samples/active-directory-dotnet-native-uwp-v2/archive/msal3x.zip)

> [!div class="sxs-lookup" renderon="portal"]
> Ejecute el proyecto con Visual Studio 2019.
> [!div class="sxs-lookup" renderon="portal" id="autoupdate" class="nextstepaction"]
> [Descargar el código de ejemplo](https://github.com/Azure-Samples/active-directory-dotnet-native-uwp-v2/archive/msal3x.zip)

> [!div class="sxs-lookup" renderon="portal"]
> #### <a name="step-3-your-app-is-configured-and-ready-to-run"></a>Paso 3: La aplicación está configurada y lista para ejecutarse
> Hemos configurado el proyecto con los valores de las propiedades de su aplicación y está preparado para ejecutarse.

> [!div class="sxs-lookup" renderon="portal"]
> > [!NOTE]
> > `Enter_the_Supported_Account_Info_Here`

> [!div renderon="docs"]
> #### <a name="step-3-configure-the-visual-studio-project"></a>Paso 3: Configuración del proyecto de Visual Studio
>
> 1. Extraiga el archivo .zip en una carpeta local cerca de la raíz de la unidad. Por ejemplo, en **C:\Azure-Samples**.
> 1. Abra el proyecto en Visual Studio. Si se le solicita, instale la carga de trabajo de **desarrollo de la Plataforma universal de Windows** y los componentes del SDK.
> 1. En *MainPage.Xaml.cs*, cambie el valor de la variable `ClientId` por el valor de **Id. de aplicación (cliente)** de la aplicación registrada anteriormente.
>
>    ```csharp
>    private const string ClientId = "Enter_the_Application_Id_here";
>    ```
>
>    El valor de **Id. de aplicación (cliente)** se encuentra en el panel **Información general** de Azure Portal (**Azure Active Directory** > **Registros de aplicaciones** >  *{Registro de la aplicación}* ).
> 1. Cree y seleccione un certificado de prueba autofirmado para el paquete:
>     1. En el **Explorador de soluciones**, haga doble clic en archivo *Package.appxmanifest*.
>     1. Seleccione **Empaquetado** > **Elegir certificado...**  > **Crear...**
>     1. Escriba una contraseña y seleccione **Aceptar**.
>     1. Seleccione **Seleccionar de archivo...** y el archivo *Native_UWP_V2_TemporaryKey.pfx* que acaba de crear; después, seleccione **Aceptar**.
>     1. Cierre el archivo *Package.appxmanifest* (seleccione **Aceptar** si se le pide que lo guarde).
>     1. En el **Explorador de soluciones**, haga clic con el botón derecho en el proyecto **Native_UWP_V2** y seleccione **Propiedades**.
>     1. Seleccione **Signing** (Firma) y el archivo .pfx creado en el menú desplegable **Elija un archivo de nombre de clave seguro**.

#### <a name="step-4-run-the-application"></a>Paso 4: Ejecución de la aplicación

Para ejecutar la aplicación de ejemplo en la máquina local:

1. En la barra de herramientas de Visual Studio, elija la plataforma correcta (probablemente **x64** o **x86**, no ARM). El dispositivo de destino debería cambiar de *Dispositivo* a *Máquina local*.
1. Seleccione **Depurar** > **Iniciar sin depurar**.
    
    Si se le pide, es posible que deba habilitar **Developer Mode** (Modo de desarrollador) antes de habilitar **Iniciar sin depurar** de nuevo para iniciar la aplicación.

Cuando aparezca la ventana de la aplicación, podrá seleccionar el botón **Call Microsoft Graph API** (Llamar a Microsoft Graph API), escribir sus credenciales y dar su consentimiento a los permisos solicitados por la aplicación. Si esto se realiza correctamente, la aplicación muestra algunos datos del token y otros obtenidos de la llamada a Microsoft Graph API.

## <a name="how-the-sample-works"></a>Funcionamiento del ejemplo

![Muestra cómo funciona la aplicación de ejemplo generada por este inicio rápido.](media/quickstart-v2-uwp/uwp-intro.svg)

### <a name="msalnet"></a>MSAL.NET

MSAL ([Microsoft.Identity.Client](https://www.nuget.org/packages/Microsoft.Identity.Client)) es la biblioteca utilizada para iniciar sesión con usuarios y solicitar tokens de seguridad. Los tokens de seguridad se utilizan para acceder a una API protegida por la Plataforma de identidad de Microsoft para desarrolladores. Puede instalar MSAL mediante la ejecución del siguiente comando en la *Consola del Administrador de paquetes* de Visual Studio:

```powershell
Install-Package Microsoft.Identity.Client
```

### <a name="msal-initialization"></a>Inicialización de MSAL

Puede agregar la referencia de MSAL con el código siguiente:

```csharp
using Microsoft.Identity.Client;
```

A continuación, se inicializa MSAL con el código siguiente:

```csharp
public static IPublicClientApplication PublicClientApp;
PublicClientApp = PublicClientApplicationBuilder.Create(ClientId)
                                                .WithRedirectUri("https://login.microsoftonline.com/common/oauth2/nativeclient")
                                                    .Build();
```

El valor de `ClientId` es el de **Id. del cliente de aplicación** de la aplicación que ha registrado en Azure Portal. Puede encontrar este valor en la página **Información general** de la aplicación en Azure Portal.

### <a name="requesting-tokens"></a>Solicitud de tokens

MSAL tiene dos métodos para adquirir tokens en una aplicación de UWP: `AcquireTokenInteractive` y `AcquireTokenSilent`.

#### <a name="get-a-user-token-interactively"></a>Obtención de un token de usuario interactivamente

En algunas situaciones, es necesario forzar a los usuarios a interactuar con el punto de conexión de la plataforma de identidad de Microsoft mediante una ventana emergente para validar sus credenciales o dar su consentimiento. Estos son algunos ejemplos:

- La primera vez que los usuarios inician sesión en la aplicación
- Cuando los usuarios deben volver a escribir sus credenciales porque la contraseña expiró
- Cuando la aplicación solicita acceso a un recurso para el cual el usuario necesita dar su consentimiento
- Cuando se requiere la autenticación en dos fases

```csharp
authResult = await App.PublicClientApp.AcquireTokenInteractive(scopes)
                      .ExecuteAsync();
```

El parámetro `scopes` contiene los ámbitos que se solicitan, como `{ "user.read" }` para Microsoft Graph o `{ "api://<Application ID>/access_as_user" }` para las API web personalizadas.

#### <a name="get-a-user-token-silently"></a>Obtención de un token de usuario en silencio

Use el método `AcquireTokenSilent` para obtener tokens que permiten obtener acceso a recursos protegidos después del método `AcquireTokenInteractive` inicial. No es recomendable pedirle al usuario que valide sus credenciales cada vez que necesite obtener acceso a un recurso. La mayor parte del tiempo, se espera que la renovación y adquisición de tokens se produzca sin ninguna interacción por parte del usuario.

```csharp
var accounts = await App.PublicClientApp.GetAccountsAsync();
var firstAccount = accounts.FirstOrDefault();
authResult = await App.PublicClientApp.AcquireTokenSilent(scopes, firstAccount)
                                      .ExecuteAsync();
```

* `scopes` contiene los ámbitos que se solicitan, como `{ "user.read" }` para Microsoft Graph o `{ "api://<Application ID>/access_as_user" }` para las API web personalizadas.
* `firstAccount` especifica la primera cuenta de usuario en la memoria caché (MSAL admite varios usuarios en una sola aplicación).

[!INCLUDE [Help and support](../../../includes/active-directory-develop-help-support-include.md)]

## <a name="next-steps"></a>Pasos siguientes

Visite el tutorial de escritorio de Windows para acceder a una guía completa paso a paso sobre la creación de aplicaciones y nuevas características, que incluye una explicación completa de esta guía de inicio rápido.

> [!div class="nextstepaction"]
> [UWP: Tutorial de Graph API de llamada](tutorial-v2-windows-uwp.md)

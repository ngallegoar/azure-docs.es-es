---
title: 'Tutorial: Concesión de acceso a una API web de ASP.NET Core desde una aplicación de página única'
titleSuffix: Azure AD B2C
description: En este tutorial, aprenderá a usar Active Directory B2C para proteger una API web de .NET Core e invocarla desde una aplicación de una sola página de Node.js.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.author: mimart
ms.date: 07/24/2019
ms.custom: mvc
ms.topic: tutorial
ms.service: active-directory
ms.subservice: B2C
ms.openlocfilehash: f6f9ff7bb0d504ecc163f6ce1f87477b1ea9c2d1
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/24/2020
ms.locfileid: "78186156"
---
# <a name="tutorial-grant-access-to-an-aspnet-core-web-api-from-a-single-page-application-using-azure-active-directory-b2c"></a>Tutorial: Concesión de acceso a una API web de ASP.NET Core desde una aplicación de página única mediante Azure Active Directory B2C

En este tutorial se muestra cómo llamar a un recurso de API web ASP.NET Core protegido mediante Azure Active Directory B2C (Azure AD B2C) desde una aplicación de página única.

En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Incorporar una aplicación de API web
> * Configurar los ámbitos para una API web
> * Conceder permisos a la API web
> * Configurar el ejemplo para que use la aplicación

## <a name="prerequisites"></a>Prerrequisitos

* Complete los pasos y requisitos previos en [Tutorial: Habilitación de la autenticación en una aplicación de página única mediante Azure Active Directory B2C](tutorial-single-page-app.md).
* Visual Studio 2019 o una versión posterior, o Visual Studio Code
* .NET Core 2.2 o una versión posterior
* Node.js

## <a name="add-a-web-api-application"></a>Incorporar una aplicación de API web

[!INCLUDE [active-directory-b2c-appreg-webapi](../../includes/active-directory-b2c-appreg-webapi.md)]

## <a name="configure-scopes"></a>Configuración de ámbitos

Los ámbitos proporcionan una manera de controlar el acceso a los recursos protegidos. La API web utiliza los ámbitos para implementar el control de acceso basado en el ámbito. Por ejemplo, algunos usuarios pueden tener tanto acceso de lectura como de escritura, mientras que otros usuarios pueden tener permisos de solo lectura. En este tutorial, definirá los permisos de lectura y escritura para la API web.

[!INCLUDE [active-directory-b2c-scopes](../../includes/active-directory-b2c-scopes.md)]

Anote el valor indicado bajo **ÁMBITOS** correspondiente al ámbito `demo.read`; lo usará en un paso posterior al configurar la aplicación de página única. El valor de ámbito completo es similar a `https://contosob2c.onmicrosoft.com/api/demo.read`.

## <a name="grant-permissions"></a>Concesión de permisos

Para llamar a una API web protegida desde otra aplicación, deberá conceder permisos de la API web a la aplicación.

En el tutorial de requisitos previos, ha creado una aplicación web denominada *webapp1*. En este tutorial, configurará esa aplicación para que llame a la API web que creó en un apartado anterior, *webapi1*.

[!INCLUDE [active-directory-b2c-permissions-api](../../includes/active-directory-b2c-permissions-api.md)]

La aplicación web de página única está registrada para llamar a la API web protegida. Un usuario se autentica con Azure AD B2C para utilizar la aplicación de página única. La aplicación de página única obtiene una concesión de autorización de Azure AD B2C para acceder a la API web protegida.

## <a name="configure-the-sample"></a>Configuración del ejemplo

Ahora que se ha registrado la API web y tiene ámbitos definidos, debe configurar el código de la API web para usar el inquilino de Azure AD B2C. En este tutorial, configurará una aplicación web de ejemplo con .NET Core que se descarga desde GitHub.

[Descargue un \*archivo zip](https://github.com/Azure-Samples/active-directory-b2c-dotnetcore-webapi/archive/master.zip) o clone el proyecto de API web de ejemplo desde GitHub.

```console
git clone https://github.com/Azure-Samples/active-directory-b2c-dotnetcore-webapi.git
```

### <a name="configure-the-web-api"></a>Configuración de la API web

1. Abra el archivo <em>B2C-WebApi/**appsettings.json**</em> en Visual Studio o Visual Studio Code.
1. Modifique el bloque `AzureAdB2C` para que refleje el nombre del inquilino, el identificador de la aplicación de API web, el nombre de la directiva de registro o de inicio de sesión y los ámbitos que definió anteriormente. El bloque debe ser similar al siguiente ejemplo (con los valores `Tenant` y `ClientId` adecuados):

    ```json
    "AzureAdB2C": {
      "Tenant": "<your-tenant-name>.onmicrosoft.com",
      "ClientId": "<webapi-application-ID>",
      "Policy": "B2C_1_signupsignin1",

      "ScopeRead": "demo.read",
      "ScopeWrite": "demo.write"
    },
    ```

#### <a name="enable-cors"></a>Habilitación de CORS

Para permitir que la aplicación de página única llame a la API web de ASP.NET Core, debe habilitar [CORS](https://docs.microsoft.com/aspnet/core/security/cors) en la API web.

1. En *Startup.cs*, agregue CORS al método `ConfigureServices()`.

    ```csharp
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddCors();
    ```

1. También en el método `ConfigureServices()`, establezca el valor `jwtOptions.Authority` en el siguiente URI de emisor de token.

    Reemplace `<your-tenant-name>` por el nombre del inquilino de B2C.

    ```csharp
    jwtOptions.Authority = $"https://<your-tenant-name>.b2clogin.com/{Configuration["AzureAdB2C:Tenant"]}/{Configuration["AzureAdB2C:Policy"]}/v2.0";
    ```

1. En el método `Configure()`, configure CORS.

    ```csharp
    public void Configure(IApplicationBuilder app, IHostingEnvironment env, ILoggerFactory loggerFactory)
    {
        app.UseCors(builder =>
            builder.WithOrigins("http://localhost:6420").AllowAnyHeader().AllowAnyMethod());
    ```

1. (Solo Visual Studio) En el Explorador de soluciones, en **Propiedades**, abra el archivo *launchSettings.json* y busque el bloque `iisExpress`.
1. (Solo Visual Studio) Actualice el valor `applicationURL` con el número de puerto que especificó al registrar la aplicación *webapi1* en un paso anterior. Por ejemplo:

    ```json
    "iisExpress": {
      "applicationUrl": "http://localhost:5000/",
      "sslPort": 0
    }
    ```

### <a name="configure-the-single-page-application"></a>Configuración de la aplicación de página única

La aplicación de página única (SPA) del [tutorial anterior](tutorial-single-page-app.md) de la serie usa Azure AD B2C para el registro y el inicio de sesión y llama a la API web de ASP.NET Core protegida por el inquilino de demostración *frabrikamb2c*.

En esta sección actualizará la aplicación de página única para que llame a la API web de ASP.net Core protegida por *su* inquilino de Azure AD B2C que ejecuta en la máquina local.

Para cambiar la configuración en la aplicación de página única:

1. Abra el archivo *index.html* del proyecto [active-directory-b2c-javascript-msal-singlepageapp][github-js-spa] que descargó o clonó en el tutorial anterior.
1. Configure el ejemplo con el URI para el ámbito *demo.read* que creó anteriormente y la dirección URL de la API web.
    1. En la definición de `appConfig`, reemplace el valor `b2cScopes` por el URI completo del ámbito (el valor **ÁMBITO** que anotó anteriormente).
    1. Cambie el valor `webApi` por el URI de redirección que agregó al registrar la API web en un paso anterior.

    La definición de `appConfig` debe ser similar al siguiente bloque de código (con el nombre del inquilino en el lugar de `<your-tenant-name>`):

    ```javascript
    // The current application coordinates were pre-registered in a B2C tenant.
    var appConfig = {
      b2cScopes: ["https://<your-tenant-name>.onmicrosoft.com/api/demo.read"],
      webApi: "http://localhost:5000/"
    };
    ```

## <a name="run-the-spa-and-web-api"></a>Ejecución de la aplicación de página única y la API web

Por último, ejecute la API web de ASP.NET Core y la aplicación de página única de Node.js en la máquina local. A continuación, inicie sesión en la aplicación de página única y presione un botón para iniciar una solicitud a la API protegida.

Aunque ambas aplicaciones se ejecutan localmente en este tutorial, usan Azure AD B2C para el registro y el inicio de sesión seguros y para conceder acceso a la API web protegida.

### <a name="run-the-aspnet-core-web-api"></a>Ejecución de la API web de ASP.NET Core

En Visual Studio, presione **F5** para compilar y depurar la solución *B2C-WebAPI.sln*. Cuando se inicia el proyecto, se muestra una página web en el explorador predeterminado que anuncia que la API web está disponible para las solicitudes.

Si prefiere usar la CLI de `dotnet` en lugar de Visual Studio:

1. Abra una ventana de consola y cambie al directorio que contiene el archivo *\*.csproj*. Por ejemplo:

    `cd active-directory-b2c-dotnetcore-webapi/B2C-WebApi`

1. Compile y ejecute la API web mediante la ejecución de `dotnet run`.

    Cuando la API esté en funcionamiento, debería ver una salida similar a la siguiente (para el tutorial, puede omitir cualquier advertencia `NETSDK1059` sin problema):

    ```console
    $ dotnet run
    Hosting environment: Production
    Content root path: /home/user/active-directory-b2c-dotnetcore-webapi/B2C-WebApi
    Now listening on: http://localhost:5000
    Application started. Press Ctrl+C to shut down.
    ```

### <a name="run-the-single-page-app"></a>Ejecución de la aplicación de una sola página

1. Abra una ventana de consola y cambie al directorio que contiene el ejemplo de Node.js. Por ejemplo:

    `cd active-directory-b2c-javascript-msal-singlepageapp`

1. Ejecute los comandos siguientes:

    ```console
    npm install && npm update
    node server.js
    ```

    La ventana de la consola muestra el número de puerto donde se hospeda la aplicación.

    ```console
    Listening on port 6420...
    ```

1. Vaya a `http://localhost:6420` en el explorador para ver la aplicación.
1. Inicie sesión con la dirección de correo electrónico y la contraseña que usó en el [tutorial anterior](tutorial-single-page-app.md). Tras iniciar sesión correctamente, debería ver el mensaje `User 'Your Username' logged-in`.
1. Seleccione el botón **Call Web API** (Llamar a API web). La aplicación de página única obtiene una concesión de autorización de Azure AD B2C y accede a la API web protegida para mostrar el contenido de su página de índice:

    ```Output
    Web APi returned:
    "<html>\r\n<head>\r\n  <title>Azure AD B2C API Sample</title>\r\n ...
    ```

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, ha aprendido a:

> [!div class="checklist"]
> * Incorporar una aplicación de API web
> * Configurar los ámbitos para una API web
> * Conceder permisos a la API web
> * Configurar el ejemplo para que use la aplicación

Ahora que ha visto una aplicación de página única solicitar un recurso de una API web protegida, entremos en detalles sobre cómo interactúan estos tipos de aplicaciones entre sí y con Azure AD B2C.

> [!div class="nextstepaction"]
> [Tipos de aplicaciones que se pueden usar en Active Directory B2C >](application-types.md)

<!-- Links - EXTERNAL -->
[github-js-spa]: https://github.com/Azure-Samples/active-directory-b2c-javascript-msal-singlepageapp

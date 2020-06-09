---
title: 'Tutorial: Concesión de acceso a una API web de Node.js desde una aplicación de escritorio'
description: Tutorial acerca de cómo usar Active Directory B2C para proteger una API web de Node.js y llamarla desde una aplicación de escritorio de .NET.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.author: mimart
ms.date: 10/12/2019
ms.custom: mvc
ms.topic: tutorial
ms.service: active-directory
ms.subservice: B2C
ms.openlocfilehash: 8ebfbeeb4533f21bc0fa10a5fee7b88ef069c262
ms.sourcegitcommit: d118ad4fb2b66c759b70d4d8a18e6368760da3ad
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/02/2020
ms.locfileid: "84298861"
---
# <a name="tutorial-grant-access-to-a-nodejs-web-api-from-a-desktop-app-using-azure-active-directory-b2c"></a>Tutorial: Concesión de acceso a una API web de Node.js desde una aplicación de escritorio mediante Azure Active Directory B2C

En este tutorial se muestra cómo llamar a una API web de Node.js protegida por Azure Active Directory B2C (Azure AD B2C) desde una aplicación de escritorio de Windows Presentation Foundation (WPF), también protegida por Azure AD B2C.

En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Incorporar una aplicación de API web
> * Configurar los ámbitos para una API web
> * Conceder permisos a la API web
> * Actualizar el ejemplo para que use la aplicación

## <a name="prerequisites"></a>Requisitos previos

Complete los pasos y requisitos previos en [Tutorial: Autenticación de usuarios en una aplicación cliente de escritorio nativa](tutorial-desktop-app.md).

## <a name="add-a-web-api-application"></a>Incorporar una aplicación de API web

[!INCLUDE [active-directory-b2c-appreg-webapi](../../includes/active-directory-b2c-appreg-webapi.md)]

## <a name="configure-scopes"></a>Configuración de ámbitos

Los ámbitos proporcionan una manera de controlar el acceso a los recursos protegidos. La API web utiliza los ámbitos para implementar el control de acceso basado en el ámbito. Por ejemplo, algunos usuarios pueden tener tanto acceso de lectura como de escritura, mientras que otros usuarios pueden tener permisos de solo lectura. En este tutorial, definirá los permisos de lectura y escritura para la API web.

[!INCLUDE [active-directory-b2c-scopes](../../includes/active-directory-b2c-scopes.md)]

Anote el valor indicado bajo **ÁMBITOS** correspondiente al ámbito `demo.read`; lo usará en un paso posterior al configurar la aplicación de escritorio. El valor de ámbito completo es similar a `https://contosob2c.onmicrosoft.com/api/demo.read`.

## <a name="grant-permissions"></a>Concesión de permisos

Para llamar a una API web protegida desde una aplicación cliente nativa, debe conceder a la aplicación cliente nativa registrada permisos para la API web que registró en Azure AD B2C.

En el tutorial de requisitos previos, registró una aplicación cliente nativa denominada *nativeapp1*. En los pasos siguientes se configura el registro de la aplicación nativa con los ámbitos de API expuestos para *webapi1* en la sección anterior. Esto permite que la aplicación de escritorio obtenga un token de acceso de Azure AD B2C que la API web puede usar para comprobar y proporcionar acceso de ámbito a sus recursos. Más adelante en el tutorial configurará y ejecutará los códigos de ejemplo de la aplicación de escritorio y de la API web.

Para registrar una aplicación en su inquilino de Azure AD B2C, puede usar la nueva experiencia unificada **Registros de aplicaciones**, o bien la experiencia anterior **Aplicaciones (heredado)** . [Más información acerca de la nueva experiencia](https://aka.ms/b2cappregtraining).

#### <a name="app-registrations"></a>[Registros de aplicaciones](#tab/app-reg-ga/)

1. Seleccione **Registros de aplicaciones** y, después, la aplicación cliente nativa que debería tener acceso a la API. Por ejemplo, *nativeapp1*.
1. En **Administrar**, seleccione **Permisos de API**.
1. En **Permisos configurados**, seleccione **Agregar un permiso**.
1. Seleccione la pestaña **Mis API**.
1. Seleccione la API a la que la aplicación cliente nativa debe tener acceso. Por ejemplo, *webapi1*.
1. En **Permiso**, expanda **demo** y, a continuación, seleccione los ámbitos que definió anteriormente. Por ejemplo, *demo.read* y *demo.write*.
1. Seleccione **Agregar permisos**. Como se indicó, espere unos minutos antes de continuar con el paso siguiente.
1. Seleccione **Conceder consentimiento de administrador para (el nombre de inquilino)** .
1. Seleccione la cuenta de administrador que tiene actualmente la sesión iniciada o inicie sesión con una cuenta en el inquilino de Azure AD B2C que tenga asignado al menos el rol *Administrador de aplicaciones en la nube*.
1. Seleccione **Aceptar**.
1. Seleccione **Actualizar** y, a continuación, compruebe que aparece "Concedido para..." en **Estado** para ambos ámbitos. Los permisos pueden tardar unos minutos en propagarse.

#### <a name="applications-legacy"></a>[Aplicaciones (heredado)](#tab/applications-legacy/)

1. Seleccione **Aplicaciones (heredado)** y, a continuación, *nativeapp1*.
1. Seleccione **Acceso de API** y, a continuación, seleccione **Agregar**.
1. En el menú desplegable **Seleccionar API**, seleccione *webapi1*.
1. En la lista desplegable **Seleccionar ámbitos**, seleccione los ámbitos que ha definido anteriormente. Por ejemplo, *demo.read* y *demo.write*.
1. Seleccione **Aceptar**.

* * *

Un usuario se autentica con Azure AD B2C para usar la aplicación web de escritorio WPF. La aplicación de escritorio obtiene una concesión de autorización de Azure AD B2C para acceder a la API web protegida.

## <a name="configure-the-samples"></a>Configuración de los ejemplos

Ahora que la API web está registrada y que ha configurado los ámbitos y los permisos, deberá configurar la aplicación de escritorio y la API web de ejemplo para usar el inquilino de Azure AD B2C.

### <a name="update-the-desktop-application"></a>Actualización de la aplicación de escritorio

En un requisito previo de este artículo, modificó una [aplicación de escritorio de WPF](https://github.com/Azure-Samples/active-directory-b2c-dotnet-desktop) para habilitar el inicio de sesión con un flujo de usuario en el inquilino de Azure AD B2C. En esta sección, actualizará la misma aplicación para hacer referencia a la API web que registró anteriormente, *webapi1*.

1. Abra la solución **active-directory-b2c-wpf** (`active-directory-b2c-wpf.sln`) en Visual Studio.
1. En el proyecto **active-directory-b2c-wpf**, abra el archivo *App.xaml.cs* y busque las siguientes definiciones de variable.
    1. Reemplace el valor de la variable `ApiScopes` por el valor que anotó anteriormente al definir el ámbito **demo.read**.
    1. Reemplace el valor de la variable `ApiEndpoint` por el **URI de redirección** que anotó anteriormente al registrar la API web (por ejemplo, *webapi1*) en el inquilino.

    Este es un ejemplo:

    ```csharp
    public static string[] ApiScopes = { "https://contosob2c.onmicrosoft.com/api/demo.read" };
    public static string ApiEndpoint = "http://localhost:5000";
    ```

### <a name="get-and-update-the-nodejs-api-sample"></a>Obtención y actualización de la API de Node.js de ejemplo

A continuación, obtenga en GitHub el código de ejemplo de la API web de Node.js y configúrelo para usar la API web que registró en el inquilino de Azure AD B2C.

[Descargue un archivo zip](https://github.com/Azure-Samples/active-directory-b2c-javascript-nodejs-webapi/archive/master.zip) o clone la aplicación web de ejemplo desde GitHub.

```console
git clone https://github.com/Azure-Samples/active-directory-b2c-javascript-nodejs-webapi.git
```

El ejemplo de API web de Node.js usa la biblioteca de Passport.js para permitir que Azure AD B2C proteja las llamadas a la API.

1. Abra el archivo `index.js` .
1. Actualice estas definiciones de variables con los siguientes valores. Cambie `<web-API-application-ID>` por el **Id. de aplicación (cliente)** de la API web que registró anteriormente (*webapi1*). Cambie `<your-b2c-tenant>` por el nombre del inquilino de Azure AD B2C.

    ```nodejs
    var clientID = "<web-API-application-ID>";
    var b2cDomainHost = "<your-b2c-tenant>.b2clogin.com";
    var tenantIdGuid = "<your-b2c-tenant>.onmicrosoft.com";
    var policyName = "B2C_1_signupsignin1";
    ```
1. Dado que está ejecutando la API localmente, actualice la ruta de acceso en la ruta del método GET a `/` en lugar de la ubicación de la aplicación de demostración `/hello`:

    ```nodejs
    app.get("/",
    ```

## <a name="run-the-samples"></a>Ejecución de las muestras

### <a name="run-the-nodejs-web-api"></a>Ejecución de la API web de Node.js

1. Inicie un símbolo del sistema de Node.js.
2. Cambie al directorio que contiene el ejemplo de Node.js. Por ejemplo, `cd c:\active-directory-b2c-javascript-nodejs-webapi`.
3. Ejecute los comandos siguientes:
    ```console
    npm install && npm update
    ```
    ```console
    node index.js
    ```

### <a name="run-the-desktop-application"></a>Ejecución de la aplicación de escritorio

1. Abra la solución **active-directory-b2c-wpf** en Visual Studio.
2. Presione **F5** para ejecutar la aplicación de escritorio.
3. Inicie sesión con la dirección de correo electrónico y la contraseña que usó en el tutorial [Authenticate users with Azure Active Directory B2C in a desktop app tutorial](tutorial-desktop-app.md) (Autenticación de los usuarios con Azure Active Directory B2C en una aplicación de escritorio).
4. Seleccione el botón **Llamada a API**.

La aplicación de escritorio realiza una solicitud a la API web que se ejecuta localmente y, tras la comprobación de que existe un token de acceso válido, indica el nombre para mostrar del usuario con sesión iniciada.

![Nombre para mostrar indicado en el panel superior de la aplicación de escritorio de WPF](./media/tutorial-desktop-app-webapi/desktop-app-01-post-api-call.png)

La aplicación de escritorio, protegida por Azure AD B2C, llama a la API web que se ejecuta localmente y que también está protegida por Azure AD B2C.

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, ha aprendido a:

> [!div class="checklist"]
> * Incorporar una aplicación de API web
> * Configurar los ámbitos para una API web
> * Conceder permisos a la API web
> * Actualizar el ejemplo para que use la aplicación

> [!div class="nextstepaction"]
> [Tutorial: Adición de proveedores de identidades a las aplicaciones en Azure Active Directory B2C](tutorial-add-identity-providers.md)

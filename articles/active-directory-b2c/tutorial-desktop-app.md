---
title: 'Tutorial: Autenticación de usuarios en una aplicación cliente nativa'
titleSuffix: Azure AD B2C
description: Tutorial acerca de cómo usar Azure Active Directory B2C para proporcionar inicio de sesión de usuario en una aplicación de escritorio de .NET.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.author: mimart
ms.date: 10/12/2019
ms.custom: mvc
ms.topic: tutorial
ms.service: active-directory
ms.subservice: B2C
ms.openlocfilehash: 06d27c3a3daa4702653a2063d0ac70fd094e2d74
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/24/2020
ms.locfileid: "78186206"
---
# <a name="tutorial-authenticate-users-in-a-native-desktop-client-using-azure-active-directory-b2c"></a>Tutorial: Autenticación de usuarios en una aplicación cliente de escritorio nativa mediante Azure Active Directory B2C

En este tutorial se muestra cómo usar Azure Active Directory B2C (Azure AD B2C) para registrar e iniciar sesión con usuarios en una aplicación de escritorio de Windows Presentation Foundation (WPF). Azure AD B2C permite que las aplicaciones puedan autenticarse en cuentas de las redes sociales, cuentas de empresa y cuentas de Azure Active Directory mediante protocolos estándar abiertos.

En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Agregar la aplicación cliente nativa
> * Configurar el ejemplo para que use la aplicación
> * Registrarse mediante el flujo de usuario

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Prerrequisitos

- [Cree flujos de usuario](tutorial-create-user-flows.md) para habilitar las experiencias de usuario en la aplicación.
- [Instale Visual Studio 2019](https://www.visualstudio.com/downloads/) con las cargas de trabajo de **desarrollo de escritorio de .NET** y de **desarrollo web y de ASP.NET**.

## <a name="add-the-native-client-application"></a>Agregar la aplicación cliente nativa

[!INCLUDE [active-directory-b2c-appreg-native](../../includes/active-directory-b2c-appreg-native.md)]

Anote el **Id. de aplicación (cliente)** para usarlo en un paso posterior.

## <a name="configure-the-sample"></a>Configuración del ejemplo

En este tutorial, va a configurar un ejemplo que puede descargar desde GitHub. La aplicación de escritorio WPF de ejemplo muestra el registro y el inicio de sesión, y puede llamar a una API web protegida en Azure AD B2C. [Descargue un archivo zip](https://github.com/Azure-Samples/active-directory-b2c-dotnet-desktop/archive/msalv3.zip), [examine el repositorio](https://github.com/Azure-Samples/active-directory-b2c-dotnet-desktop) o clone el ejemplo de GitHub.

```
git clone https://github.com/Azure-Samples/active-directory-b2c-dotnet-desktop.git
```

Para actualizar la aplicación de modo que funcione con su inquilino de Azure AD B2C e invoque sus flujos de usuario en lugar de los incluidos en el inquilino de demostración predeterminado:

1. Abra la solución **active-directory-b2c-wpf** (`active-directory-b2c-wpf.sln`) en Visual Studio.
2. En el proyecto **active-directory-b2c-wpf**, abra el archivo *App.xaml.cs* y busque las siguientes definiciones de variable. Reemplace `{your-tenant-name}` por el nombre del inquilino de Azure AD B2C y `{application-ID}` por el identificador de la aplicación que anotó anteriormente.

    ```csharp
    private static readonly string Tenant = "{your-tenant-name}.onmicrosoft.com";
    private static readonly string AzureAdB2CHostname = "{your-tenant-name}.b2clogin.com";
    private static readonly string ClientId = "{application-ID}";
    ```

3. Actualice las variables de nombre de directiva con los nombres de los flujos de usuario que creó como parte de los requisitos previos. Por ejemplo:

    ```csharp
    public static string PolicySignUpSignIn = "B2C_1_signupsignin1";
    public static string PolicyEditProfile = "B2C_1_profileediting1";
    public static string PolicyResetPassword = "B2C_1_passwordreset1";
    ```

## <a name="run-the-sample"></a>Ejecución del ejemplo

Presione **F5** para compilar y ejecutar el ejemplo.

### <a name="sign-up-using-an-email-address"></a>Registro con una dirección de correo electrónico

1. Seleccione **Iniciar sesión** para registrarse como usuario. Esto usa el flujo de usuario **B2C_1_signupsignin1**.
2. Azure AD B2C presenta una página de inicio de sesión con un vínculo **Registrarse ahora**. Como no tiene aún una cuenta, seleccione el vínculo **Registrarse ahora**.
3. El flujo de trabajo del registro presenta una página para recopilar y verificar la identidad del usuario con una dirección de correo electrónico. El flujo de trabajo de registro también recopila la contraseña del usuario y los atributos requeridos definidos en el flujo de usuario.

    Utilice una dirección de correo electrónico válida y valídela mediante el código de verificación. Establezca una contraseña. Especifique valores para los atributos solicitados.

    ![Página de registro que se muestra como parte del flujo de trabajo de inicio de sesión o registro](./media/tutorial-desktop-app/azure-ad-b2c-sign-up-workflow.png)

4. Seleccione **Crear** para crear una cuenta local en el inquilino de Azure AD B2C.

Ahora el usuario puede utilizar su dirección de correo electrónico para iniciar sesión y usar la aplicación de escritorio. Después de un registro o un inicio de sesión correctos, en el panel inferior de la aplicación de WPF se muestran los detalles del token.

![Detalles de token mostrados en el panel inferior de la aplicación de escritorio de WPF](./media/tutorial-desktop-app/desktop-app-01-post-signin.png)

Si selecciona el botón **Llamada a API**, se muestra un **mensaje de error**. Este error se debe a que, en su estado actual, la aplicación está intentando acceder a una API protegida por el inquilino de demostración, `fabrikamb2c.onmicrosoft.com`. Como el token de acceso solamente es válido para el inquilino de Azure AD B2C, la llamada a la API no tiene autorización.

Continúe con el siguiente tutorial para registrar una API web protegida en su propio inquilino y habilitar la funcionalidad de **Llamada a API**.

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, ha aprendido a:

> [!div class="checklist"]
> * Agregar la aplicación cliente nativa
> * Configurar el ejemplo para que use la aplicación
> * Registrarse mediante el flujo de usuario

A continuación, para habilitar la funcionalidad del botón **Llamada a API**, conceda a la aplicación de escritorio de WPF acceso a una API web registrada en su propio inquilino de Azure AD B2C:

> [!div class="nextstepaction"]
> [Tutorial: Concesión de acceso a una API web de Node.js desde una aplicación de escritorio >](tutorial-desktop-app-webapi.md)

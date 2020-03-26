---
title: 'Tutorial: Habilitación de la autenticación en una aplicación de página única'
titleSuffix: Azure AD B2C
description: En este tutorial, aprenderá a usar Azure Active Directory B2C para proporcionar inicio de sesión de usuario a una aplicación de página única (SPA).
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.author: mimart
ms.date: 10/14/2019
ms.custom: mvc, seo-javascript-september2019
ms.topic: tutorial
ms.service: active-directory
ms.subservice: B2C
ms.openlocfilehash: 435800d9c6bfd9131d50681a9808f9836104fac0
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/24/2020
ms.locfileid: "78183353"
---
# <a name="tutorial-enable-authentication-in-a-single-page-application-using-azure-active-directory-b2c-azure-ad-b2c"></a>Tutorial: Habilitación de la autenticación en una aplicación de página única mediante Azure Active Directory B2C (Azure AD B2C)

En este tutorial se muestra cómo usar Azure Active Directory B2C (Azure AD B2C) para que los usuarios se registren e inicien sesión en una aplicación de página única (SPA). Azure AD B2C permite que las aplicaciones puedan autenticarse en cuentas de las redes sociales, cuentas de empresa y cuentas de Azure Active Directory mediante protocolos estándar abiertos.

En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Actualizar la aplicación en Azure AD B2C
> * Configurar el ejemplo para que use la aplicación
> * Registrarse mediante el flujo de usuario

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Prerrequisitos

Antes de continuar con los pasos en este tutorial, necesita tener los siguientes recursos de Azure AD B2C:

* [Inquilino de Azure AD B2C](tutorial-create-tenant.md)
* [Aplicación registrada](tutorial-register-applications.md) en su inquilino
* [Flujos de usuario creados](tutorial-create-user-flows.md) en su inquilino

También necesita los siguientes elementos en su entorno de desarrollo:

* Editor de código; por ejemplo, [Visual Studio Code](https://code.visualstudio.com/) o [Visual Studio 2019](https://www.visualstudio.com/downloads/).
* [SDK de .NET Core 2.2](https://dotnet.microsoft.com/download) o versiones posteriores
* [Node.js](https://nodejs.org/en/download/)

## <a name="update-the-application"></a>Actualizar la aplicación

En el segundo tutorial que completó como parte de los requisitos previos, registró una aplicación web en Azure AD B2C. Para habilitar la comunicación con el ejemplo en este tutorial, deberá agregar un identificador URI de redirección a la aplicación en Azure AD B2C.

Para actualizar la aplicación, puede usar la experiencia **Aplicaciones** actual o la nueva experiencia **Registros de aplicaciones (versión preliminar)** unificada. [Más información acerca de la nueva experiencia](https://aka.ms/b2cappregintro).

#### <a name="applications"></a>[Aplicaciones](#tab/applications/)

1. Inicie sesión en [Azure Portal](https://portal.azure.com).
1. Asegúrese de usar el directorio que contiene el inquilino de Azure AD B2C. Para ello, seleccione el filtro **Directorio y suscripción** en el menú superior y luego el directorio que contiene el inquilino.
1. Seleccione **Todos los servicios** en la esquina superior izquierda de Azure Portal, y busque y seleccione **Azure AD B2C**.
1. Seleccione **Aplicaciones** y, a continuación, seleccione la aplicación *webapp1*.
1. En **URL de respuesta**, agregue `http://localhost:6420`.
1. Seleccione **Guardar**.
1. En la página de propiedades, registre el **identificador de aplicación**. Dicho identificador se usará en un paso posterior para actualizar el código de la aplicación web de una página.

#### <a name="app-registrations-preview"></a>[Registros de aplicaciones (versión preliminar)](#tab/app-reg-preview/)

1. Inicie sesión en [Azure Portal](https://portal.azure.com).
1. Seleccione el filtro **Directorio y suscripción** en el menú superior y, luego, elija el directorio que contiene el inquilino de Azure AD B2C.
1. En el menú de la izquierda, seleccione **Azure AD B2C**. O bien, seleccione **Todos los servicios** y busque y seleccione **Azure AD B2C**.
1. Seleccione **Registros de aplicaciones (versión preliminar)** , seleccione la pestaña **Aplicaciones propias** y, a continuación, seleccione la aplicación *webapp1*.
1. Seleccione **Autenticación** y, después, seleccione **Probar la nueva experiencia** (si se muestra).
1. En **Web**, seleccione el vínculo **Agregar URI**, escriba `http://localhost:6420` y, a continuación, seleccione **Guardar**.
1. Seleccione **Información general**.
1. Anote el **Id. de aplicación (cliente)** ; lo usará en un paso posterior al actualizar el código en la aplicación web de página única.

* * *

## <a name="get-the-sample-code"></a>Obtención del código de ejemplo

En este tutorial, va a configurar un ejemplo de código que puede descargar desde GitHub. Este ejemplo muestra cómo una aplicación de página única puede usar Azure AD B2C para el registro del usuario, el inicio de sesión y llamar a una API web protegida.

[Descargue un archivo zip](https://github.com/Azure-Samples/active-directory-b2c-javascript-msal-singlepageapp/archive/master.zip) o clone el ejemplo de GitHub.

```
git clone https://github.com/Azure-Samples/active-directory-b2c-javascript-msal-singlepageapp.git
```

## <a name="update-the-sample"></a>Actualización del ejemplo

Ahora que ha obtenido el ejemplo, actualice el código con el nombre del inquilino de Azure AD B2C y el identificador de aplicación que registró en el paso anterior.

1. Abra el archivo `index.html` en el directorio raíz del proyecto.
1. En la definición `msalConfig`, reemplace el valor de **clientId** por el identificador de aplicación que registró en el paso anterior. A continuación, reemplace el valor del identificador URI de **authority** por el nombre del inquilino de Azure AD B2C. Actualice también el URI con el nombre del flujo de registro o inicio de sesión de usuario que creó en uno de los requisitos previos (por ejemplo, *B2C_1_signupsignin1*).

    ```javascript
    var msalConfig = {
        auth: {
            clientId: "00000000-0000-0000-0000-000000000000", //This is your client ID
            authority: "https://fabrikamb2c.b2clogin.com/fabrikamb2c.onmicrosoft.com/b2c_1_susi", //This is your tenant info
            validateAuthority: false
        },
        cache: {
            cacheLocation: "localStorage",
            storeAuthStateInCookie: true
        }
    };
    ```

    El nombre del flujo de usuario que se ha utilizado en este tutorial es **B2C_1_signupsignin1**. Si va a usar otro nombre de flujo de usuario distinto, especifíquelo en el valor `authority`.

## <a name="run-the-sample"></a>Ejecución del ejemplo

1. Abra una ventana de consola y cambie al directorio que contiene el ejemplo. Por ejemplo:

    ```console
    cd active-directory-b2c-javascript-msal-singlepageapp
    ```
1. Ejecute los comandos siguientes:

    ```
    npm install && npm update
    node server.js
    ```

    La ventana de consola muestra el número de puerto del servidor Node.js que se ejecuta localmente:

    ```
    Listening on port 6420...
    ```

1. Vaya a `http://localhost:6420` en el explorador para ver la aplicación.

El ejemplo admite el registro, inicio de sesión, edición de un perfil y restablecimiento de una contraseña. Este tutorial ilustra cómo un usuario se registra mediante una dirección de correo electrónico.

### <a name="sign-up-using-an-email-address"></a>Registro con una dirección de correo electrónico

> [!WARNING]
> Después del registro o el inicio de sesión, es posible que se muestre un [error de permisos insuficientes](#error-insufficient-permissions). Este error está previsto, debido a la implementación actual del código de ejemplo. Este problema se resolverá en una versión futura del código de ejemplo, momento en el que se quitará esta advertencia.

1. Seleccione **Iniciar sesión** para iniciar el flujo de usuario *B2C_1_signupsignin1* que especificó en un paso anterior.
1. Azure AD B2C presenta una página de inicio de sesión con un vínculo de registro. Como no tiene aún una cuenta, seleccione el vínculo **Registrarse ahora**.
1. El flujo de trabajo del registro presenta una página para recopilar y verificar la identidad del usuario con una dirección de correo electrónico. El flujo de trabajo de registro también recopila la contraseña del usuario y los atributos requeridos definidos en el flujo de usuario.

    Utilice una dirección de correo electrónico válida y valídela mediante el código de verificación. Establezca una contraseña. Especifique valores para los atributos solicitados.

    ![Página de registro presentada por el flujo de usuario de inicio de sesión o registro](./media/tutorial-single-page-app/azure-ad-b2c-sign-up-workflow.png)

1. Seleccione **Crear** para crear una cuenta local en el directorio de Azure AD B2C.

Al seleccionar **Crear**, la página de registro se cierra y se muestra la página de inicio de sesión.

Ahora puede usar su dirección de correo electrónico y contraseña para iniciar sesión en la aplicación.

### <a name="error-insufficient-permissions"></a>Error: permisos insuficientes

Después de iniciar sesión, es posible que la aplicación devuelva un error de permisos insuficientes:

```Output
ServerError: AADB2C90205: This application does not have sufficient permissions against this web resource to perform the operation.
Correlation ID: ce15bbcc-0000-0000-0000-494a52e95cd7
Timestamp: 2019-07-20 22:17:27Z
```

Recibirá este error porque la aplicación web está intentando acceder a una API web protegida por el directorio de demostración *fabrikamb2c*. Como el token de acceso solamente es válido para el directorio de Azure AD, la llamada a la API no tiene autorización.

Para corregir este error, continúe en el siguiente tutorial de la serie (consulte [Siguientes pasos](#next-steps)) para crear una API web protegida para el directorio.

## <a name="next-steps"></a>Pasos siguientes

En este artículo, ha aprendido cómo:

> [!div class="checklist"]
> * Actualizar la aplicación en Azure AD B2C
> * Configurar el ejemplo para que use la aplicación
> * Registrarse mediante el flujo de usuario

Pase al siguiente tutorial de la serie para conceder acceso a una API web protegida desde la SPA:

> [!div class="nextstepaction"]
> [Tutorial: Concesión de acceso a una API web de ASP.NET Core desde una aplicación de página única mediante Azure Active Directory B2C >](tutorial-single-page-app-webapi.md)

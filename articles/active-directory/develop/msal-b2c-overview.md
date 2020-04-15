---
title: Uso de MSAL con Azure Active Directory B2C | Azure
titleSuffix: Microsoft identity platform
description: La biblioteca de autenticación de Microsoft para JavaScript (MSAL.js) permite que las aplicaciones trabajen con Azure AD B2C y adquieran tokens para llamar a API web protegidas. Estas API web pueden ser Microsoft Graph, otras API de Microsoft, API web de terceros o su propia API web.
services: active-directory
author: negoe
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 09/16/2019
ms.author: negoe
ms.reviewer: nacanuma
ms.custom: aaddev
ms.openlocfilehash: dc8a330bc09f37f7941534ed7c17d1ffd14d08c5
ms.sourcegitcommit: 2d7910337e66bbf4bd8ad47390c625f13551510b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80875969"
---
# <a name="use-microsoft-authentication-library-for-javascript-to-work-with-azure-active-directory-b2c"></a>Uso de la biblioteca de autenticación de Microsoft para JavaScript para trabajar con Azure Active Directory B2C

La [biblioteca de autenticación de Microsoft para JavaScript (MSAL.js)](https://github.com/AzureAD/microsoft-authentication-library-for-js) permite que los desarrolladores de JavaScript autentiquen a los usuarios con identidades sociales y locales mediante [Azure Active Directory B2C (Azure AD B2C)](https://docs.microsoft.com/azure/active-directory-b2c/). Al usar Azure AD B2C como un servicio de administración de identidades, puede personalizar y controlar la manera en que los clientes se registran, inician sesión y administran sus perfiles al usar las aplicaciones.

Azure AD B2C también permite personalizar la marca y la UI de sus aplicaciones durante el proceso de autenticación para proporcionar una experiencia perfecta a sus clientes.

En este artículo se muestra cómo usar MSAL.js para trabajar con Azure AD B2C y se resumen los puntos clave que debe tener en cuenta. Para información completa y un tutorial, consulte la [documentación de Azure AD B2C](https://docs.microsoft.com/azure/active-directory-b2c/overview).

## <a name="prerequisites"></a>Prerrequisitos

Si todavía no crea su propio [inquilino de Azure AD B2C](https://docs.microsoft.com/azure/active-directory-b2c/tutorial-create-tenant), empiece por crear uno ahora mismo (también puede usar un inquilino de Azure AD B2C existente si ya tiene).

Esta demostración contiene dos partes:

- Cómo proteger una API web.
- Cómo registrar una aplicación de página única para la autenticación y llamar a *esa* API web.

## <a name="nodejs-web-api"></a>API web de Node.js

> [!NOTE]
> En este momento, MSAL.js para Node sigue en desarrollo (consulte la [hoja de ruta](https://github.com/AzureAD/microsoft-authentication-library-for-js/wiki#roadmap)). Mientras tanto, se recomienda usar [passport-azure-ad](https://github.com/AzureAD/passport-azure-ad), una biblioteca de autenticación para Node.js desarrollada y respaldada por Microsoft.

En los pasos siguientes se muestra cómo una **API web** puede usar Azure AD B2C para protegerse y exponer ámbitos seleccionados en una aplicación cliente.

### <a name="step-1-register-your-application"></a>Paso 1: Registrar su aplicación

Para proteger la API web con Azure AD B2C, primero debe registrarla. Consulte [Registrar la aplicación](https://docs.microsoft.com/azure/active-directory-b2c/add-web-application?tabs=applications) para obtener instrucciones detalladas.

### <a name="step-2-download-the-sample-application"></a>Paso 2: Descarga de la aplicación de ejemplo

Descargue el ejemplo como archivo ZIP o clónelo desde GitHub:

```console
git clone https://github.com/Azure-Samples/active-directory-b2c-javascript-nodejs-webapi.git
```

### <a name="step-3-configure-authentication"></a>Paso 3: Configurar la autenticación

1. Abra el archivo `config.js` del ejemplo.

2. Configure el ejemplo con las credenciales de la aplicación que obtuvo anteriormente al registrar la aplicación. Cambie las líneas de código siguiente reemplazando los valores por los nombres de clientID, host, tenantId y nombre de la directiva.

```JavaScript
const clientID = "<Application ID for your Node.js Web API - found on Properties page in Azure portal e.g. 93733604-cc77-4a3c-a604-87084dd55348>";
const b2cDomainHost = "<Domain of your B2C host eg. fabrikamb2c.b2clogin.com>";
const tenantId = "<your-tenant-ID>.onmicrosoft.com"; // Alternatively, you can use your Directory (tenant) ID (GUID)
const policyName = "<Name of your sign in / sign up policy, e.g. B2C_1_signupsignin1>";
```

Para más información, consulte este [ejemplo de API web B2C de Node.js](https://github.com/Azure-Samples/active-directory-b2c-javascript-nodejs-webapi).

---

## <a name="javascript-spa"></a>JavaScript SPA

En los pasos siguientes se muestra cómo una **aplicación de página única** puede usar Azure AD B2C para el registro, el inicio de sesión y las llamadas a una API web protegida.

### <a name="step-1-register-your-application"></a>Paso 1: Registrar su aplicación

Para implementar la autenticación, primero debe registrar la aplicación. Consulte [Registrar la aplicación](https://docs.microsoft.com/azure/active-directory-b2c/tutorial-register-applications) para obtener instrucciones detalladas.

### <a name="step-2-download-the-sample-application"></a>Paso 2: Descarga de la aplicación de ejemplo

Descargue el ejemplo como archivo ZIP o clónelo desde GitHub:

```console
git clone https://github.com/Azure-Samples/active-directory-b2c-javascript-msal-singlepageapp.git
```

### <a name="step-3-configure-authentication"></a>Paso 3: Configurar la autenticación

Hay dos puntos de interés en la configuración de la aplicación:

- Configuración de un punto de conexión de API y los ámbitos expuestos
- Configuración de los parámetros de autenticación y los ámbitos de token

1. Abra el archivo `apiConfig.js` del ejemplo.

2. Configure el ejemplo con los parámetros que obtuvo anteriormente al registrar la API web. Cambie las líneas de código siguiente reemplazando los valores por la dirección de la API web y los ámbitos expuestos.

   ```javascript
    // The current application coordinates were pre-registered in a B2C tenant.
    const apiConfig = {
        b2cScopes: ["https://fabrikamb2c.onmicrosoft.com/helloapi/demo.read"], //API scopes you exposed during api registration
        webApi: "https://fabrikamb2chello.azurewebsites.net/hello" 
    };
   ```

3. Abra el archivo `authConfig.js` del ejemplo.

4. Configure el ejemplo con los parámetros que obtuvo anteriormente al registrar la aplicación de página única. Cambie las líneas de código siguientes reemplazando los valores por su ClientId, los metadatos de autoridad y los ámbitos de solicitud de token.

   ```javascript
    // Config object to be passed to Msal on creation.
    const msalConfig = {
        auth: {
            clientId: "e760cab2-b9a1-4c0d-86fb-ff7084abd902",
            authority: "https://fabrikamb2c.b2clogin.com/fabrikamb2c.onmicrosoft.com/B2C_1_signupsignin1",
            validateAuthority: false
        },
        cache: {
            cacheLocation: "localStorage", // This configures where your cache will be stored
            storeAuthStateInCookie: false // Set this to "true" to save cache in cookies
        }
    };

    // Add here scopes for id token to be used at the MS Identity Platform endpoint
    const loginRequest = {
        scopes: ["openid", "profile"],
    };
   ```

Para más información, consulte este [ejemplo de aplicación de página única de B2C de JavaScript](https://github.com/Azure-Samples/active-directory-b2c-javascript-msal-singlepageapp).

---

## <a name="next-steps"></a>Pasos siguientes

Más información sobre:
- [Flujos de usuario](https://docs.microsoft.com/azure/active-directory-b2c/tutorial-create-user-flows)
- [Directivas personalizadas](https://docs.microsoft.com/azure/active-directory-b2c/custom-policy-get-started)
- [Personalización de la experiencia de usuario](https://docs.microsoft.com/azure/active-directory-b2c/custom-policy-configure-user-input)

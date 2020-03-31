---
title: Uso de MSAL con Azure Active Directory B2C | Azure
titleSuffix: Microsoft identity platform
description: La biblioteca de autenticación de Microsoft (MSAL) permite a las aplicaciones interoperar con Azure AD B2C y adquirir tokens para llamar a API web protegidas. Estas API web pueden ser Microsoft Graph, otras API de Microsoft, API web de terceros o su propia API web.
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
ms.openlocfilehash: e25564e64410701754390024a5bcfd39321343e2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "76696459"
---
# <a name="use-microsoft-authentication-library-to-interoperate-with-azure-active-directory-b2c"></a>Uso de la biblioteca de autenticación de Microsoft para interoperar con Azure Active Directory B2C

La biblioteca de autenticación de Microsoft (MSAL) permite a los desarrolladores de aplicaciones autenticar a los usuarios con identidades sociales y locales mediante [Azure Active Directory B2C (Azure AD B2C)](https://docs.microsoft.com/azure/active-directory-b2c/). Azure AD B2C es un servicio de administración de identidades. Al usarlo, puede personalizar y controlar el modo en que los clientes se suscriben, inician sesión y administran sus perfiles al usar las aplicaciones.

Azure AD B2C también permite personalizar la marca y la UI de sus aplicaciones para proporcionar una experiencia sin interrupciones perfecta a sus clientes.

En este tutorial se muestra cómo usar MSAL para interoperar con Azure AD B2C.

## <a name="prerequisites"></a>Prerrequisitos

Si aún no ha creado su propio [inquilino de Azure AD B2C](https://docs.microsoft.com/azure/active-directory-b2c/tutorial-create-tenant), cree uno ahora. También puede usar un inquilino de Azure AD B2C existente.

## <a name="javascript"></a>JavaScript

En los siguientes pasos se muestra cómo una aplicación de página única puede usar Azure AD B2C para el registro, el inicio de sesión y las llamadas a una API web protegida.

### <a name="step-1-register-your-application"></a>Paso 1: Registrar su aplicación

Para implementar la autenticación, primero debe registrar la aplicación. Consulte [Registrar la aplicación](https://github.com/Azure-Samples/active-directory-b2c-javascript-msal-singlepageapp#step-4-register-your-own-web-application-with-azure-ad-b2c) para obtener instrucciones detalladas.

### <a name="step-2-download-the-sample-application"></a>Paso 2: Descarga de la aplicación de ejemplo

Descargue el ejemplo como archivo ZIP o clónelo desde GitHub:

```
git clone https://github.com/Azure-Samples/active-directory-b2c-javascript-msal-singlepageapp.git
```

### <a name="step-3-configure-authentication"></a>Paso 3: Configurar la autenticación

1. Abra el archivo **index.html** del ejemplo.

1. Configure el ejemplo con el identificador y la clave de cliente que registró anteriormente mientras registraba la aplicación. Cambie las siguientes líneas de código reemplazando los valores por los nombres del directorio y las API:

   ```javascript
   // The current application coordinates were pre-registered in a B2C tenant.

    var appConfig = {
        b2cScopes: ["https://fabrikamb2c.onmicrosoft.com/helloapi/demo.read"],
        webApi: "https://fabrikamb2chello.azurewebsites.net/hello"
    };

    const msalConfig = {
        auth: {
            clientId: "e760cab2-b9a1-4c0d-86fb-ff7084abd902" //This is your client/application ID
            authority: "https://fabrikamb2c.b2clogin.com/fabrikamb2c.onmicrosoft.com/b2c_1_susi", //This is your tenant info
            validateAuthority: false
        },
        cache: {
            cacheLocation: "localStorage",
            storeAuthStateInCookie: true
        }
    };
    // create UserAgentApplication instance
    const myMSALObj = new Msal.UserAgentApplication(msalConfig);

   ```

El nombre del [flujo de usuario](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-reference-policies) de este tutorial es **B2C_1_signupsignin1**. Si va usar otro, defina el valor de **autoridad** con el mismo nombre.

### <a name="step-4-configure-your-application-to-use-b2clogincom"></a>Paso 4: Configurar la aplicación para que use `b2clogin.com`

Puede usar `b2clogin.com` en lugar de `login.microsoftonline.com` como dirección URL de redireccionamiento. Se hace desde la aplicación de Azure AD B2C al configurar un proveedor de identidades para el registro y el inicio de sesión.

El uso de `b2clogin.com` en el contexto de `https://your-tenant-name.b2clogin.com/your-tenant-guid` tiene los efectos siguientes:

- Los servicios de Microsoft consumen menos espacio en el encabezado de la cookie.
- Las direcciones URL ya no incluyen una referencia a Microsoft. Por ejemplo, la aplicación de Azure AD B2C probablemente hace referencia a `login.microsoftonline.com`.

 Para usar `b2clogin.com`, debe actualizar la configuración de la aplicación.  

- Establezca la propiedad **validateAuthority** en `false` para que se puedan producir redireccionamientos mediante `b2clogin.com`.

En el ejemplo siguiente se muestra cómo se puede establecer la propiedad:

```javascript
// The current application coordinates were pre-registered in a B2C directory.

const msalConfig = {
    auth:{
        clientId: "Enter_the_Application_Id_here",
        authority: "https://contoso.b2clogin.com/tfp/contoso.onmicrosoft.com/B2C_1_signupsignin1",
        b2cScopes: ["https://contoso.onmicrosoft.com/demoapi/demo.read"],
        webApi: 'https://contosohello.azurewebsites.net/hello',
        validateAuthority: false;

};
// create UserAgentApplication instance
const myMSALObj = new UserAgentApplication(msalConfig);
```

> [!NOTE]
> La aplicación de Azure AD B2C probablemente se refiere a `login.microsoftonline.com` en varios lugares, tales como las referencias de flujos de usuario y puntos de conexión de token. Asegúrese de que el punto de conexión de autorización, el punto de conexión del token y el emisor se han actualizado para utilizar `your-tenant-name.b2clogin.com`.

Siga [este ejemplo de JavaScript de MSAL](https://github.com/Azure-Samples/active-directory-b2c-javascript-msal-singlepageapp#single-page-application-built-on-msaljs-with-azure-ad-b2c) sobre cómo usar la versión preliminar de MSAL para JavaScript (MSAL.js). El ejemplo obtiene un token de acceso y llama a una API protegida por Azure AD B2C.

## <a name="next-steps"></a>Pasos siguientes

Más información sobre:

- [Directivas personalizadas](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-overview-custom)
- [Personalización de la interfaz de usuario](https://docs.microsoft.com/azure/active-directory-b2c/customize-ui-overview)
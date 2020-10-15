---
title: Configuración del inicio de sesión con una cuenta de Microsoft con directivas personalizadas
titleSuffix: Azure AD B2C
description: Cómo usar las directivas personalizadas para habilitar la cuenta de Microsoft (MSA) como proveedor de identidades mediante el protocolo OpenID Connect (OIDC).
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: how-to
ms.date: 05/12/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: db81f8b60cf4883223f6fc084c19c8da1d07bc9a
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "85388109"
---
# <a name="set-up-sign-in-with-a-microsoft-account-using-custom-policies-in-azure-active-directory-b2c"></a>Configuración del inicio de sesión con una cuenta Microsoft mediante directivas personalizadas en Azure Active Directory B2C

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

En este artículo se muestra cómo habilitar el inicio de sesión para los usuarios desde una cuenta Microsoft mediante [directivas personalizadas](custom-policy-overview.md) en Azure Active Directory B2C (Azure AD B2C).

## <a name="prerequisites"></a>Requisitos previos

- Siga los pasos de [Introducción a las directivas personalizadas en Azure Active Directory B2C](custom-policy-get-started.md).
- Si aún no tiene una cuenta Microsoft, cree una en [https://www.live.com/](https://www.live.com/).

## <a name="register-an-application"></a>Registro de una aplicación

Para habilitar el inicio de sesión de los usuarios con una cuenta de Microsoft, deberá registrar una aplicación dentro del inquilino de Azure AD. El inquilino de Azure AD no es el mismo que el usuario de Azure AD B2C.

1. Inicie sesión en [Azure Portal](https://portal.azure.com).
1. Asegúrese de que usa el directorio que contiene el inquilino de Azure AD. Para ello, seleccione el filtro **Directorio y suscripción** que se encuentra en el menú superior y elija el directorio que contiene el inquilino de Azure AD.
1. Elija **Todos los servicios** en la esquina superior izquierda de Azure Portal, y busque y seleccione **Registros de aplicaciones**.
1. Seleccione **Nuevo registro**.
1. Escriba el **nombre** de la aplicación. Por ejemplo, *MSAapp1*.
1. En **Tipos de cuenta compatibles**, seleccione **Cuentas en cualquier directorio organizativo (cualquier directorio de Azure AD: multiinquilino) y cuentas de Microsoft personales (como Skype o Xbox)** .
1. En **URI de redirección (opcional)** , seleccione **Web** y escriba `https://<tenant-name>.b2clogin.com/<tenant-name>.onmicrosoft.com/oauth2/authresp` en el cuadro de texto. Reemplace `<tenant-name>` por el nombre del inquilino de Azure AD B2C.
1. Seleccione **Registrar**.
1. Registre el **Id. de aplicación (cliente)** que se muestra en la página de información general de la aplicación. Lo va a necesitar cuando configure el proveedor de notificaciones en una sección posterior.
1. Seleccione **Certificates & secrets** (Certificados y secretos)
1. Haga clic en **Nuevo secreto de cliente**.
1. Escriba una **Descripción** del secreto, por ejemplo, *secreto de cliente de la aplicación de MSA* y, luego, haga clic en **Agregar**.
1. Anote la contraseña de la aplicación que se muestra en la columna **Value**. Usará este valor en la sección siguiente.

## <a name="configuring-optional-claims"></a>Configuración de notificaciones opcionales

Si quiere obtener las notificaciones `family_name` y `given_name` de Azure AD, puede configurar notificaciones opcionales para la aplicación en la interfaz de usuario de Azure Portal o el manifiesto de aplicación. Para obtener más información, consulte [Procedimientos: Proporcionar notificaciones opcionales a la aplicación de Azure AD](../active-directory/develop/active-directory-optional-claims.md).

1. Inicie sesión en [Azure Portal](https://portal.azure.com). Busque y seleccione **Azure Active Directory**.
1. En la sección **Administrar**, seleccione **Registros de aplicaciones**.
1. Seleccione en la lista la aplicación para la que desea configurar notificaciones opcionales.
1. En la sección **Administrar**, seleccione **Configuración del token (versión preliminar)** .
1. Seleccione **Agregar notificación opcional**.
1. Seleccione el tipo de token que desea configurar.
1. Seleccione las notificaciones opcionales que va a agregar.
1. Haga clic en **Agregar**.

## <a name="create-a-policy-key"></a>Creación de una clave de directiva

Ahora que creó la aplicación en el inquilino de Azure AD, deberá almacenar el secreto de cliente de esa aplicación en el inquilino de Azure AD B2C.

1. Inicie sesión en [Azure Portal](https://portal.azure.com/).
1. Asegúrese de que usa el directorio que contiene el inquilino de Azure AD B2C. Seleccione el filtro **Directorio y suscripciones** del menú superior y elija el directorio que contiene el inquilino.
1. Elija **Todos los servicios** en la esquina superior izquierda de Azure Portal, y busque y seleccione **Azure AD B2C**.
1. En la página de introducción, seleccione **Identity Experience Framework**.
1. Seleccione **Claves de directiva** y luego **Agregar**.
1. En **Opciones**, elija `Manual`.
1. Escriba un **nombre** para la clave de directiva. Por ejemplo, `MSASecret`. Se agregará el prefijo `B2C_1A_` automáticamente al nombre de la clave.
1. En **Secreto**, escriba el secreto de cliente que anotó en la sección anterior.
1. En **Uso de claves**, seleccione `Signature`.
1. Haga clic en **Crear**.

## <a name="add-a-claims-provider"></a>Incorporación de un proveedor de notificaciones

Para permitir que los usuarios inicien sesión con una cuenta de Microsoft, deberá definir la cuenta como un proveedor de notificaciones con el que Azure AD B2C pueda comunicarse mediante un punto de conexión. El punto de conexión proporciona un conjunto de notificaciones que Azure AD B2C usa para comprobar que un usuario concreto se ha autenticado.

Puede definir Azure AD como proveedor de notificaciones. Para ello, agregue el elemento **ClaimsProvider** en el archivo de extensión de la directiva.

1. Abra el archivo de directiva *TrustFrameworkExtensions.xml*.
1. Busque el elemento **ClaimsProviders**. Si no existe, agréguelo debajo del elemento raíz.
1. Agregue un nuevo elemento **ClaimsProvider** tal como se muestra a continuación:

    ```xml
    <ClaimsProvider>
      <Domain>live.com</Domain>
      <DisplayName>Microsoft Account</DisplayName>
      <TechnicalProfiles>
        <TechnicalProfile Id="MSA-OIDC">
          <DisplayName>Microsoft Account</DisplayName>
          <Protocol Name="OpenIdConnect" />
          <Metadata>
            <Item Key="ProviderName">https://login.live.com</Item>
            <Item Key="METADATA">https://login.live.com/.well-known/openid-configuration</Item>
            <Item Key="response_types">code</Item>
            <Item Key="response_mode">form_post</Item>
            <Item Key="scope">openid profile email</Item>
            <Item Key="HttpBinding">POST</Item>
            <Item Key="UsePolicyInRedirectUri">0</Item>
            <Item Key="client_id">Your Microsoft application client ID</Item>
          </Metadata>
          <CryptographicKeys>
            <Key Id="client_secret" StorageReferenceId="B2C_1A_MSASecret" />
          </CryptographicKeys>
          <OutputClaims>
            <OutputClaim ClaimTypeReferenceId="issuerUserId" PartnerClaimType="oid" />
            <OutputClaim ClaimTypeReferenceId="givenName" PartnerClaimType="given_name" />
            <OutputClaim ClaimTypeReferenceId="surName" PartnerClaimType="family_name" />
            <OutputClaim ClaimTypeReferenceId="displayName" PartnerClaimType="name" />
            <OutputClaim ClaimTypeReferenceId="authenticationSource" DefaultValue="socialIdpAuthentication" />
            <OutputClaim ClaimTypeReferenceId="identityProvider" PartnerClaimType="iss" />
            <OutputClaim ClaimTypeReferenceId="email" />
          </OutputClaims>
          <OutputClaimsTransformations>
            <OutputClaimsTransformation ReferenceId="CreateRandomUPNUserName" />
            <OutputClaimsTransformation ReferenceId="CreateUserPrincipalName" />
            <OutputClaimsTransformation ReferenceId="CreateAlternativeSecurityId" />
            <OutputClaimsTransformation ReferenceId="CreateSubjectClaimFromAlternativeSecurityId" />
          </OutputClaimsTransformations>
          <UseTechnicalProfileForSessionManagement ReferenceId="SM-SocialLogin" />
        </TechnicalProfile>
      </TechnicalProfiles>
    </ClaimsProvider>
    ```

1. Reemplace el valor de **client_id** por el *identificador de la aplicación (cliente)* de la aplicación de Azure AD.
1. Guarde el archivo.

Ya configuró la directiva para que Azure AD B2C sepa cómo comunicarse con su aplicación de cuenta de Microsoft en Azure AD.

### <a name="upload-the-extension-file-for-verification"></a>Carga del archivo de extensión para su comprobación

Antes de continuar, cargue la directiva modificada para confirmar que no tiene ningún problema hasta el momento.

1. Vaya a su inquilino de Azure AD B2C en Azure Portal y seleccione **Identity Experience Framework**.
1. En la página **Directivas personalizadas**, seleccione **Cargar directiva personalizada**.
1. Habilite **Sobrescribir la directiva, si existe**, y busque y seleccione el archivo *TrustFrameworkExtensions.xml*.
1. Haga clic en **Cargar**.

Si no se muestra ningún error en el portal, siga hasta la sección siguiente.

## <a name="register-the-claims-provider"></a>Registro del proveedor de notificaciones

En este momento ya tiene configurado el proveedor de identidades, pero todavía no está disponible en ninguna de las pantallas de registro ni de inicio de sesión. Para que esté disponible, debe crear un duplicado de un recorrido del usuario de plantilla existente y, después, modificarlo para que también tenga el proveedor de identidades de la cuenta de Microsoft.

1. Abra el archivo *TrustFrameworkBase.xml* del paquete de inicio.
1. Busque y copie todo el contenido del elemento **UserJourney** que incluye `Id="SignUpOrSignIn"`.
1. Abra el archivo *TrustFrameworkExtensions.xml* y busque el elemento **UserJourneys**. Si el elemento no existe, agréguelo.
1. Pegue todo el contenido del elemento **UserJourney** que ha copiado como elemento secundario del elemento **UserJourneys**.
1. Cambie el identificador del recorrido del usuario. Por ejemplo, `SignUpSignInMSA`.

### <a name="display-the-button"></a>Visualización del botón

El elemento **ClaimsProviderSelection** es análogo a un botón de proveedor de identidades en una pantalla de registro o de inicio de sesión. Si agrega un elemento **ClaimsProviderSelection** para una cuenta Microsoft, se muestra un botón nuevo cuando un usuario llega a la página.

1. En el archivo *TrustFrameworkExtensions.xml*, busque el elemento **OrchestrationStep** que incluya `Order="1"` en el recorrido de usuario que creó.
1. En **ClaimsProviderSelects**, agregue el siguiente elemento. Establezca un valor adecuado en **TargetClaimsExchangeId**, por ejemplo, `MicrosoftAccountExchange`:

    ```xml
    <ClaimsProviderSelection TargetClaimsExchangeId="MicrosoftAccountExchange" />
    ```

### <a name="link-the-button-to-an-action"></a>Vincular el botón a una acción

Ahora que hay un botón colocado, es preciso vincularlo a una acción. En este caso, la acción es para que Azure AD B2C se comunique con la cuenta de Microsoft para recibir un token.

1. Busque el elemento **OrchestrationStep** que incluye `Order="2"` en el recorrido del usuario.
1. Al agregar el siguiente elemento **ClaimsExchange**, asegúrese de usar el mismo valor para el identificador que usó para **TargetClaimsExchangeId**:

    ```xml
    <ClaimsExchange Id="MicrosoftAccountExchange" TechnicalProfileReferenceId="MSA-OIDC" />
    ```

    Actualice el valor de **TechnicalProfileReferenceId** para que coincida con el valor de `Id` en el elemento **TechnicalProfile** del proveedor de notificaciones que agregó anteriormente. Por ejemplo, `MSA-OIDC`.

1. Guarde el archivo *TrustFrameworkExtensions.xml* y cárguelo de nuevo a fin de verificarlo.

## <a name="create-an-azure-ad-b2c-application"></a>Creación de una aplicación de Azure AD B2C

La comunicación con Azure AD B2C se produce mediante una aplicación que se registra en el inquilino B2C. En esta sección se enumeran los pasos opcionales que puede llevar a cabo para crear una aplicación de prueba, si aún no lo ha hecho.

[!INCLUDE [active-directory-b2c-appreg-idp](../../includes/active-directory-b2c-appreg-idp.md)]

## <a name="update-and-test-the-relying-party-file"></a>Actualización y prueba del archivo del usuario de confianza

Actualice el archivo de usuario de confianza (RP) que inicia el recorrido del usuario que ha creado.

1. Realice una copia del archivo *SignUpOrSignIn.xml* en el directorio de trabajo y cámbiele el nombre. Por ejemplo, cambie su nombre a *SignUpSignInMSA.xml*.
1. Abra el nuevo archivo y actualice el valor del atributo **PolicyId** del elemento **TrustFrameworkPolicy** con un valor único. Por ejemplo, `SignUpSignInMSA`.
1. Actualice el valor de **PublicPolicyUri** con el URI para la directiva. Por ejemplo: `http://contoso.com/B2C_1A_signup_signin_msa`
1. Cambie el valor del atributo **ReferenceId** del elemento **DefaultUserJourney** para que coincida con el identificador del recorrido del usuario que creó anteriormente (SignUpSignInMSA).
1. Guarde los cambios, cargue el archivo y seleccione la nueva directiva en la lista.
1. Asegúrese de que la aplicación de Azure AD B2C que creó en la sección anterior (o completando los requisitos previos, por ejemplo *webapp1* o *testapp1*) esté seleccionada en el campo **Seleccionar aplicación** y haga clic en **Ejecutar ahora** para probarla.
1. Seleccione el botón **Cuenta de Microsoft** e inicie sesión.

    Si la operación de inicio de sesión se realiza correctamente, se le redirigirá a `jwt.ms`, en donde se muestra el token descodificado, similar a lo siguiente:

    ```json
    {
      "typ": "JWT",
      "alg": "RS256",
      "kid": "<key-ID>"
    }.{
      "exp": 1562365200,
      "nbf": 1562361600,
      "ver": "1.0",
      "iss": "https://<tenant-name>.b2clogin.com/10000000-0000-0000-0000-000000000000/v2.0/",
      "sub": "20000000-0000-0000-0000-000000000000",
      "aud": "30000000-0000-0000-0000-000000000000",
      "acr": "b2c_1a_signupsigninmsa",
      "nonce": "defaultNonce",
      "iat": 1562361600,
      "auth_time": 1562361600,
      "idp": "live.com",
      "name": "Azure User",
      "email": "azureuser@contoso.com",
      "tid": "6fc3b573-7b38-4c0c-b627-2e8684f6c575"
    }.[Signature]
    ```

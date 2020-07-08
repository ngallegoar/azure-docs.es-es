---
title: Configuración del inicio de sesión con una cuenta de Azure AD mediante el uso de directivas personalizadas
titleSuffix: Azure AD B2C
description: Configure el inicio de sesión con una cuenta de Azure Active Directory en Azure Active Directory B2C mediante directivas personalizadas.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: how-to
ms.date: 04/20/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 06ba565de8ca24c8c0baa576b74e70035384be09
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85388432"
---
# <a name="set-up-sign-in-with-an-azure-active-directory-account-using-custom-policies-in-azure-active-directory-b2c"></a>Configurar el inicio de sesión con una cuenta de Azure Active Directory mediante directivas personalizadas en Azure Active Directory B2C

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

En este artículo se muestra cómo habilitar el inicio de sesión para los usuarios desde una organización de Azure Active Directory (Azure AD) mediante [directivas personalizadas](custom-policy-overview.md) en Azure Active Directory B2C (Azure AD B2C).

## <a name="prerequisites"></a>Prerrequisitos

Siga los pasos de [Introducción a las directivas personalizadas en Azure Active Directory B2C](custom-policy-get-started.md).


[!INCLUDE [active-directory-b2c-identity-provider-azure-ad](../../includes/active-directory-b2c-identity-provider-azure-ad.md)]

## <a name="create-a-policy-key"></a>Creación de una clave de directiva

Debe almacenar la clave de la aplicación que creó en el inquilino de Azure AD B2C.

1. Asegúrese de que usa el directorio que contiene el inquilino de Azure AD B2C. Seleccione el filtro **Directorio y suscripción** en el menú superior y, luego, elija el directorio que contiene el inquilino de Azure AD B2C.
1. Elija **Todos los servicios** en la esquina superior izquierda de Azure Portal, y busque y seleccione **Azure AD B2C**.
1. En **Directivas**, seleccione **Identity Experience Framework**.
1. Seleccione **Claves de directiva** y luego **Agregar**.
1. En **Opciones**, elija `Manual`.
1. Escriba un **nombre** para la clave de directiva. Por ejemplo, `ContosoAppSecret`.  El prefijo `B2C_1A_` se agrega automáticamente al nombre de la clave cuando se crea, por lo que su referencia en el XML de la siguiente sección es a *B2C_1A_ContosoAppSecret*.
1. En **Secreto**, escriba el secreto de cliente que registró previamente.
1. En **Uso de claves**, seleccione `Signature`.
1. Seleccione **Crear**.

## <a name="add-a-claims-provider"></a>Incorporación de un proveedor de notificaciones

Si quiere que los usuarios inicien sesión con Azure AD, deberá definir Azure AD como proveedor de notificaciones con el que Azure AD B2C se comunique mediante un punto de conexión. El punto de conexión proporciona un conjunto de notificaciones que Azure AD B2C usa para comprobar que un usuario concreto se ha autenticado.

Para definir Azure AD como proveedor de notificaciones, agregue el elemento **ClaimsProvider** al archivo de extensión de la directiva.

1. Abra el archivo *TrustFrameworkExtensions.xml*.
2. Busque el elemento **ClaimsProviders**. Si no existe, agréguelo debajo del elemento raíz.
3. Agregue un nuevo elemento **ClaimsProvider** tal como se muestra a continuación:
    ```xml
    <ClaimsProvider>
      <Domain>Contoso</Domain>
      <DisplayName>Login using Contoso</DisplayName>
      <TechnicalProfiles>
        <TechnicalProfile Id="OIDC-Contoso">
          <DisplayName>Contoso Employee</DisplayName>
          <Description>Login with your Contoso account</Description>
          <Protocol Name="OpenIdConnect"/>
          <Metadata>
            <Item Key="METADATA">https://login.microsoftonline.com/tenant-name.onmicrosoft.com/v2.0/.well-known/openid-configuration</Item>
            <Item Key="client_id">00000000-0000-0000-0000-000000000000</Item>
            <Item Key="response_types">code</Item>
            <Item Key="scope">openid profile</Item>
            <Item Key="response_mode">form_post</Item>
            <Item Key="HttpBinding">POST</Item>
            <Item Key="UsePolicyInRedirectUri">false</Item>
          </Metadata>
          <CryptographicKeys>
            <Key Id="client_secret" StorageReferenceId="B2C_1A_ContosoAppSecret"/>
          </CryptographicKeys>
          <OutputClaims>
            <OutputClaim ClaimTypeReferenceId="issuerUserId" PartnerClaimType="oid"/>
            <OutputClaim ClaimTypeReferenceId="tenantId" PartnerClaimType="tid"/>
            <OutputClaim ClaimTypeReferenceId="givenName" PartnerClaimType="given_name" />
            <OutputClaim ClaimTypeReferenceId="surName" PartnerClaimType="family_name" />
            <OutputClaim ClaimTypeReferenceId="displayName" PartnerClaimType="name" />
            <OutputClaim ClaimTypeReferenceId="authenticationSource" DefaultValue="socialIdpAuthentication" AlwaysUseDefaultValue="true" />
            <OutputClaim ClaimTypeReferenceId="identityProvider" PartnerClaimType="iss" />
          </OutputClaims>
          <OutputClaimsTransformations>
            <OutputClaimsTransformation ReferenceId="CreateRandomUPNUserName"/>
            <OutputClaimsTransformation ReferenceId="CreateUserPrincipalName"/>
            <OutputClaimsTransformation ReferenceId="CreateAlternativeSecurityId"/>
            <OutputClaimsTransformation ReferenceId="CreateSubjectClaimFromAlternativeSecurityId"/>
          </OutputClaimsTransformations>
          <UseTechnicalProfileForSessionManagement ReferenceId="SM-SocialLogin"/>
        </TechnicalProfile>
      </TechnicalProfiles>
    </ClaimsProvider>
    ```

4. En el elemento **ClaimsProvider**, actualice el valor de **Domain** a un valor único que pueda usarse para distinguirlo de otros proveedores de identidades. Por ejemplo, `Contoso`. No incluya un `.com` al final de esta configuración de dominio.
5. En el elemento **ClaimsProvider**, actualice el valor de **DisplayName** a un nombre descriptivo del proveedor de notificaciones. Este valor no se utiliza actualmente.

### <a name="update-the-technical-profile"></a>Actualización del perfil técnico

Para obtener un token del punto de conexión de Azure AD es preciso definir los protocolos que Azure AD B2C debe usar para comunicarse con Azure AD. Esto se realiza dentro del elemento **TechnicalProfile** de **ClaimsProvider**.

1. Actualice el identificador del elemento **TechnicalProfile**. Este identificador se usa para hacer referencia a este perfil técnico desde otras partes de la directiva, por ejemplo: `OIDC-Contoso`.
1. Actualice el valor de **DisplayName**. Este valor se mostrará en el botón de inicio de sesión de la pantalla de inicio de sesión.
1. Actualice el valor de **Description**.
1. Azure AD usa el protocolo OpenID Connect, por lo que debe asegurarse de que el valor de **Protocol** es `OpenIdConnect`.
1. Establezca el valor de **METADATA** como `https://login.microsoftonline.com/tenant-name.onmicrosoft.com/v2.0/.well-known/openid-configuration`, donde `tenant-name` es el nombre del inquilino de Azure AD. Por ejemplo: `https://login.microsoftonline.com/contoso.onmicrosoft.com/v2.0/.well-known/openid-configuration`
1. Establezca **client_id** en el identificador de la aplicación desde el registro de aplicación.
1. En **CryptographicKeys**, actualice el valor de **StorageReferenceId** con el nombre de la clave de directiva que creó anteriormente. Por ejemplo, `B2C_1A_ContosoAppSecret`.

### <a name="upload-the-extension-file-for-verification"></a>Carga del archivo de extensión para su comprobación

Por el momento, ha configurado la directiva para que Azure AD B2C sepa cómo comunicarse con su directorio de Azure AD. Pruebe a cargar el archivo de extensión de la directiva para confirmar que no tiene problemas.

1. En la página **Directivas personalizadas** del inquilino de Azure AD B2C, seleccione **Cargar directiva**.
1. Habilite **Sobrescribir la directiva, si existe**, y busque y seleccione el archivo *TrustFrameworkExtensions.xml*.
1. Haga clic en **Cargar**.

## <a name="register-the-claims-provider"></a>Registro del proveedor de notificaciones

En este momento, el proveedor de identidades ya se ha configurado, pero no está disponible en ninguna de las pantallas de registro o inicio de sesión. Para que esté disponible, debe crear un duplicado de un recorrido del usuario de plantilla existente que modificaremos a continuación para que también tenga el proveedor de identidades de Azure AD.

1. Abra el archivo *TrustFrameworkBase.xml* del paquete de inicio.
1. Busque y copie todo el contenido del elemento **UserJourney** que incluye `Id="SignUpOrSignIn"`.
1. Abra el archivo *TrustFrameworkExtensions.xml* y busque el elemento **UserJourneys**. Si el elemento no existe, agréguelo.
1. Pegue todo el contenido del elemento **UserJourney** que ha copiado como elemento secundario del elemento **UserJourneys**.
1. Cambie el identificador del recorrido del usuario. Por ejemplo, `SignUpSignInContoso`.

### <a name="display-the-button"></a>Visualización del botón

El elemento **ClaimsProviderSelection** es análogo a un botón del proveedor de identidades de una pantalla de registro o inicio de sesión. Si agrega un elemento **ClaimsProviderSelection** a Azure AD, se muestra un nuevo botón cuando un usuario llega a la página.

1. Busque el elemento **OrchestrationStep** que incluya `Order="1"` en el recorrido del usuario que creó en *TrustFrameworkExtensions.xml*.
1. En **ClaimsProviderSelections**, agregue el siguiente elemento. Establezca un valor adecuado en **TargetClaimsExchangeId**, por ejemplo, `ContosoExchange`:

    ```xml
    <ClaimsProviderSelection TargetClaimsExchangeId="ContosoExchange" />
    ```

### <a name="link-the-button-to-an-action"></a>Vincular el botón a una acción

Ahora que hay un botón colocado, es preciso vincularlo a una acción. En este caso, la acción es para que Azure AD B2C se comunique con Azure AD para recibir un token. Para vincular un botón a una acción, vincule el perfil técnico del proveedor de notificaciones de Azure AD:

1. Busque el elemento **OrchestrationStep** que incluye `Order="2"` en el recorrido del usuario.
1. Al agregar el siguiente elemento **ClaimsExchange**, asegúrese de usar el mismo valor para el elemento **Id** que el que usó en **TargetClaimsExchangeId**:

    ```xml
    <ClaimsExchange Id="ContosoExchange" TechnicalProfileReferenceId="OIDC-Contoso" />
    ```

    Actualice el valor de **TechnicalProfileReferenceId** al elemento **Id** del perfil técnico que creó anteriormente. Por ejemplo, `OIDC-Contoso`.

1. Guarde el archivo *TrustFrameworkExtensions.xml* y cárguelo de nuevo a fin de verificarlo.

## <a name="create-an-azure-ad-b2c-application"></a>Creación de una aplicación de Azure AD B2C

La comunicación con Azure AD B2C se produce mediante una aplicación que se registra en el inquilino B2C. En esta sección se enumeran los pasos opcionales que puede llevar a cabo para crear una aplicación de prueba, si aún no lo ha hecho.

[!INCLUDE [active-directory-b2c-appreg-idp](../../includes/active-directory-b2c-appreg-idp.md)]

## <a name="update-and-test-the-relying-party-file"></a>Actualización y prueba del archivo del usuario de confianza

Actualice el archivo de usuario de confianza (RP) que inicia el recorrido del usuario que ha creado.

1. Realice una copia del archivo *SignUpOrSignIn.xml* en el directorio de trabajo y cámbiele el nombre. Por ejemplo, cambie su nombre a *SignUpSignInContoso.xml*.
1. Abra el nuevo archivo y actualice el valor del atributo **PolicyId** del elemento **TrustFrameworkPolicy** con un valor único. Por ejemplo, `SignUpSignInContoso`.
1. Actualice el valor de **PublicPolicyUri** con el URI para la directiva. Por ejemplo, `http://contoso.com/B2C_1A_signup_signin_contoso`.
1. Actualice el valor del atributo **ReferenceId** en **DefaultUserJourney** para que coincida con el identificador del recorrido del usuario que creó anteriormente. Por ejemplo, *SignUpSignInContoso*.
1. Guarde los cambios y cargue el archivo.
1. En **Directivas personalizadas**, seleccione la nueva directiva en la lista.
1. En el menú desplegable **Seleccionar aplicación**, seleccione la aplicación de Azure AD B2C que creó anteriormente. Por ejemplo, *testapp1*.
1. Copie el valor de **Ejecutar punto de conexión ahora** y ábralo en una ventana privada del explorador web, por ejemplo, en modo incógnito en Google Chrome o en una ventana de InPrivate en Microsoft Edge. De esta forma, podrá probar el recorrido completo del usuario sin usar ninguna credencial de Azure AD actualmente almacenada en caché.
1. Seleccione el botón de inicio de sesión de Azure AD, por ejemplo *Empleado de Contoso* y, luego, escriba las credenciales de un usuario de su inquilino de la organización de Azure AD. Se le pedirá que autorice la aplicación y que luego escriba la información de su perfil.

Si el proceso de inicio de sesión se realiza correctamente, el explorador se redirige a `https://jwt.ms`, que muestra el contenido del token devuelto por Azure AD B2C.

## <a name="next-steps"></a>Pasos siguientes

Al trabajar con directivas personalizadas, en ocasiones es posible que necesite información adicional al solucionar problemas de una directiva durante su desarrollo.

Para ayudar a diagnosticar problemas, puede poner temporalmente la directiva en "modo de programador" y recopilar registros con Azure Application Insights. Descubra cómo en [Azure Active Directory B2C: Recopilación de registros](troubleshoot-with-application-insights.md).

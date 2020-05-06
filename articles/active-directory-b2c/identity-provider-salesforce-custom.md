---
title: Configuración del inicio de sesión con un proveedor de Salesforce SAML mediante directivas personalizadas
titleSuffix: Azure AD B2C
description: Configuración del inicio de sesión con un proveedor de Salesforce SAML mediante directivas personalizadas en Azure Active Directory B2C.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 02/27/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 183fe1604cc363a9121d5eef3737751c54e9bdf1
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/28/2020
ms.locfileid: "82229724"
---
# <a name="set-up-sign-in-with-a-salesforce-saml-provider-by-using-custom-policies-in-azure-active-directory-b2c"></a>Configuración del inicio de sesión con un proveedor de Salesforce SAML mediante directivas personalizadas en Azure Active Directory B2C

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

En este artículo se muestra cómo habilitar el inicio de sesión para los usuarios desde una cuenta de organización de Salesforce mediante [directivas personalizadas](custom-policy-overview.md) en Azure Active Directory B2C (Azure AD B2C). Habilite el inicio de sesión mediante la adición de un [perfil técnico de proveedor de identidades SAML](saml-identity-provider-technical-profile.md) a una directiva personalizada.

## <a name="prerequisites"></a>Prerrequisitos

- Siga los pasos de [Introducción a las directivas personalizadas en Azure Active Directory B2C](custom-policy-get-started.md).
- Si aún no lo ha hecho, regístrese en una [cuenta gratuita de Developer Edition](https://developer.salesforce.com/signup). Este artículo usa la [experiencia Salesforce Lightning](https://developer.salesforce.com/page/Lightning_Experience_FAQ).
- [Ha configurado un Mi dominio](https://help.salesforce.com/articleView?id=domain_name_setup.htm&language=en_US&type=0) para su organización de Salesforce.

### <a name="set-up-salesforce-as-an-identity-provider"></a>Configurar Salesforce como proveedor de identidades

1. [Inicie sesión en Salesforce](https://login.salesforce.com/).
2. En el menú de la izquierda, en **Settings** (Configuración), expanda **Identity** (Identidad) y seleccione **Identity Provider** (Proveedor de identidades).
3. Seleccione **Enable Identity Provider** (Habilitar proveedor de identidades).
4. Bajo **Select the certificate** (Seleccionar el certificado), seleccione el certificado que quiere que Salesforce use para comunicarse con Azure AD B2C. Puede usar el certificado predeterminado.
5. Haga clic en **Save**(Guardar).

### <a name="create-a-connected-app-in-salesforce"></a>Crear una aplicación conectada en Salesforce

1. En la página **Identity Provider** (proveedor de identidades), seleccione que **los proveedores de servicios se creen ahora con aplicaciones conectadas. Haga clic aquí.**
2. En **Basic Information**(Información básica), escriba los valores necesarios para la aplicación conectada.
3. En **Web App Settings** (Configuración de aplicación web), active la casilla **Enable SAML** (Habilitar SAML).
4. En el campo **Entity ID** (Id. de entidad), escriba la siguiente dirección URL. Asegúrese de reemplazar el valor de `your-tenant` con el nombre del inquilino de Azure AD B2C.

      ```
      https://your-tenant.b2clogin.com/your-tenant.onmicrosoft.com/B2C_1A_TrustFrameworkBase
      ```

6. En el campo **ACS URL** (Dirección URL de ACS), escriba la siguiente dirección URL. Asegúrese de reemplazar el valor de `your-tenant` con el nombre del inquilino de Azure AD B2C.

      ```
      https://your-tenant.b2clogin.com/your-tenant.onmicrosoft.com/B2C_1A_TrustFrameworkBase/samlp/sso/assertionconsumer
      ```
7. Desplácese hasta la parte inferior de la lista y, después, haga clic en **Guardar**.

### <a name="get-the-metadata-url"></a>Obtener la dirección URL de metadatos

1. En la página de información general de la aplicación conectada, haga clic en **Administrar**.
2. Copie el valor de **punto de conexión de detección de metadatos** y después guárdelo. Lo usará más adelante en este artículo.

### <a name="set-up-salesforce-users-to-federate"></a>Configurar usuarios de Salesforce para la federación

1. En la página **Administrar** de la aplicación conectada, haga clic en **Administrar perfiles**.
2. Seleccione los perfiles (o grupos de usuarios) que quiere que realicen la federación con Azure AD B2C. Como administrador del sistema, active la casilla **Administrador del sistema** para poder realizar la federación con su cuenta de Salesforce.

## <a name="generate-a-signing-certificate"></a>Generación de un certificado de firma

Las solicitudes enviadas a Salesforce deben estar firmadas mediante Azure AD B2C. Para generar un certificado de firma, abra Azure PowerShell y ejecute los comandos siguientes.

> [!NOTE]
> Asegúrese de actualizar el nombre de inquilino y la contraseña en las dos líneas superiores.

```powershell
$tenantName = "<YOUR TENANT NAME>.onmicrosoft.com"
$pwdText = "<YOUR PASSWORD HERE>"

$Cert = New-SelfSignedCertificate -CertStoreLocation Cert:\CurrentUser\My -DnsName "SamlIdp.$tenantName" -Subject "B2C SAML Signing Cert" -HashAlgorithm SHA256 -KeySpec Signature -KeyLength 2048

$pwd = ConvertTo-SecureString -String $pwdText -Force -AsPlainText

Export-PfxCertificate -Cert $Cert -FilePath .\B2CSigningCert.pfx -Password $pwd
```

## <a name="create-a-policy-key"></a>Creación de una clave de directiva

Debe almacenar el certificado que creó en el inquilino de Azure AD B2C.

1. Inicie sesión en [Azure Portal](https://portal.azure.com/).
2. Asegúrese de usar el directorio que contiene el inquilino de Azure AD B2C. Para ello, seleccione el filtro **Directorio y suscripción** en el menú superior y luego el directorio que contiene el inquilino.
3. Elija **Todos los servicios** en la esquina superior izquierda de Azure Portal, y busque y seleccione **Azure AD B2C**.
4. En la página de introducción, seleccione **Identity Experience Framework**.
5. Seleccione **Claves de directiva** y luego **Agregar**.
6. En **Opciones**, elija `Upload`.
7. Escriba un **Nombre** para la directiva. Por ejemplo, SAMLSigningCert. El prefijo `B2C_1A_` se agrega automáticamente al nombre de la clave.
8. Busque el certificado B2CSigningCert.pfx que ha creado y selecciónelo.
9. Escriba la **Contraseña** del certificado.
3. Haga clic en **Crear**.

## <a name="add-a-claims-provider"></a>Incorporación de un proveedor de notificaciones

Si desea que los usuarios inicien sesión con una cuenta de Salesforce, deberá definir la cuenta como un proveedor de notificaciones con el que Azure AD B2C pueda comunicarse mediante un punto de conexión. El punto de conexión proporciona un conjunto de notificaciones que Azure AD B2C usa para comprobar que un usuario concreto se ha autenticado.

Puede definir una cuenta de Salesforce como un proveedor de notificaciones; para ello, agréguela al elemento **ClaimsProvider** en el archivo de extensión de la directiva. Para obtener más información, consulte [Definición de un perfil técnico de proveedor de identidades SAML](saml-identity-provider-technical-profile.md).

1. Abra el archivo *TrustFrameworkExtensions.xml*.
1. Busque el elemento **ClaimsProviders**. Si no existe, agréguelo debajo del elemento raíz.
1. Agregue un nuevo elemento **ClaimsProvider** tal como se muestra a continuación:

    ```XML
    <ClaimsProvider>
      <Domain>salesforce</Domain>
      <DisplayName>Salesforce</DisplayName>
      <TechnicalProfiles>
        <TechnicalProfile Id="salesforce">
          <DisplayName>Salesforce</DisplayName>
          <Description>Login with your Salesforce account</Description>
          <Protocol Name="SAML2"/>
          <Metadata>
            <Item Key="WantsEncryptedAssertions">false</Item>
            <Item Key="WantsSignedAssertions">false</Item>
            <Item Key="PartnerEntity">https://contoso-dev-ed.my.salesforce.com/.well-known/samlidp.xml</Item>
          </Metadata>
          <CryptographicKeys>
            <Key Id="SamlAssertionSigning" StorageReferenceId="B2C_1A_SAMLSigningCert"/>
            <Key Id="SamlMessageSigning" StorageReferenceId="B2C_1A_SAMLSigningCert"/>
          </CryptographicKeys>
          <OutputClaims>
            <OutputClaim ClaimTypeReferenceId="issuerUserId" PartnerClaimType="userId"/>
            <OutputClaim ClaimTypeReferenceId="givenName" PartnerClaimType="given_name"/>
            <OutputClaim ClaimTypeReferenceId="surname" PartnerClaimType="family_name"/>
            <OutputClaim ClaimTypeReferenceId="email" PartnerClaimType="email"/>
            <OutputClaim ClaimTypeReferenceId="displayName" PartnerClaimType="username"/>
            <OutputClaim ClaimTypeReferenceId="authenticationSource" DefaultValue="socialIdpAuthentication"/>
            <OutputClaim ClaimTypeReferenceId="identityProvider" DefaultValue="SAMLIdp" />
          </OutputClaims>
          <OutputClaimsTransformations>
            <OutputClaimsTransformation ReferenceId="CreateRandomUPNUserName"/>
            <OutputClaimsTransformation ReferenceId="CreateUserPrincipalName"/>
            <OutputClaimsTransformation ReferenceId="CreateAlternativeSecurityId"/>
            <OutputClaimsTransformation ReferenceId="CreateSubjectClaimFromAlternativeSecurityId"/>
          </OutputClaimsTransformations>
          <UseTechnicalProfileForSessionManagement ReferenceId="SM-Saml-idp"/>
        </TechnicalProfile>
      </TechnicalProfiles>
    </ClaimsProvider>
    ```

1. Actualice el valor de **PartnerEntity** con la dirección URL de metadatos de Salesforce que ha copiado anteriormente.
1. Actualice el valor de ambas instancias de **StorageReferenceId** con el nombre de la clave de su certificado de firma. Por ejemplo, B2C_1A_SAMLSigningCert.
1. Busque la sección `<ClaimsProviders>` y agregue el siguiente fragmento de código XML. Si la directiva ya contiene el perfil técnico `SM-Saml-idp`, vaya al paso siguiente. Para más información, consulte [Administración de sesión de inicio de sesión único](custom-policy-reference-sso.md).

    ```XML
    <ClaimsProvider>
      <DisplayName>Session Management</DisplayName>
      <TechnicalProfiles>
        <TechnicalProfile Id="SM-Saml-idp">
          <DisplayName>Session Management Provider</DisplayName>
          <Protocol Name="Proprietary" Handler="Web.TPEngine.SSO.SamlSSOSessionProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
          <Metadata>
            <Item Key="IncludeSessionIndex">false</Item>
            <Item Key="RegisterServiceProviders">false</Item>
          </Metadata>
        </TechnicalProfile>
      </TechnicalProfiles>
    </ClaimsProvider>
    ```
1. Guarde el archivo.

### <a name="upload-the-extension-file-for-verification"></a>Carga del archivo de extensión para su comprobación

Por el momento, ha configurado la directiva para que Azure AD B2C sepa cómo comunicarse con su cuenta de Salesforce. Pruebe a cargar el archivo de extensión de la directiva para confirmar que no tiene problemas.

1. En la página **Directivas personalizadas** del inquilino de Azure AD B2C, seleccione **Cargar directiva**.
2. Habilite **Sobrescribir la directiva, si existe**, y busque y seleccione el archivo *TrustFrameworkExtensions.xml*.
3. Haga clic en **Cargar**.

## <a name="register-the-claims-provider"></a>Registro del proveedor de notificaciones

En este punto, el proveedor de identidades ya se ha configurado, pero no está disponible en ninguna de las pantallas de registro o de inicio de sesión. Para que esté disponible, debe crear un duplicado de un recorrido del usuario de plantilla existente y luego modificarlo para que también tenga el proveedor de identidades de Salesforce.

1. Abra el archivo *TrustFrameworkBase.xml* del paquete de inicio.
2. Busque y copie todo el contenido del elemento **UserJourney** que incluye `Id="SignUpOrSignIn"`.
3. Abra el archivo *TrustFrameworkExtensions.xml* y busque el elemento **UserJourneys**. Si el elemento no existe, agréguelo.
4. Pegue todo el contenido del elemento **UserJourney** que ha copiado como elemento secundario del elemento **UserJourneys**.
5. Cambie el identificador del recorrido del usuario. Por ejemplo, `SignUpSignInSalesforce`.

### <a name="display-the-button"></a>Visualización del botón

El elemento **ClaimsProviderSelection** es análogo a un botón de proveedor de identidades en una pantalla de registro o de inicio de sesión. Si agrega un elemento **ClaimsProviderSelection** para una cuenta de LinkedIn, se muestra un nuevo botón cuando un usuario llega a la página.

1. Busque el elemento **OrchestrationStep** que incluye `Order="1"` en el recorrido del usuario que ha creado.
2. En **ClaimsProviderSelects**, agregue el siguiente elemento. Establezca un valor adecuado en **TargetClaimsExchangeId**, por ejemplo, `SalesforceExchange`:

    ```XML
    <ClaimsProviderSelection TargetClaimsExchangeId="SalesforceExchange" />
    ```

### <a name="link-the-button-to-an-action"></a>Vincular el botón a una acción

Ahora que hay un botón colocado, es preciso vincularlo a una acción. En este caso, la acción es para que Azure AD B2C se comunique con la cuenta de Salesforce para recibir un token.

1. Busque el elemento **OrchestrationStep** que incluye `Order="2"` en el recorrido del usuario.
2. Al agregar el siguiente elemento **ClaimsExchange**, asegúrese de usar el mismo valor para el **id.** que usó en **TargetClaimsExchangeId**:

    ```XML
    <ClaimsExchange Id="SalesforceExchange" TechnicalProfileReferenceId="salesforce" />
    ```

    Cambie el valor de **TechnicalProfileReferenceId** al del **id.** del perfil técnico que creó anteriormente. Por ejemplo, `LinkedIn-OAUTH`.

3. Guarde el archivo *TrustFrameworkExtensions.xml* y cárguelo de nuevo a fin de verificarlo.

## <a name="create-an-azure-ad-b2c-application"></a>Creación de una aplicación de Azure AD B2C

La comunicación con Azure AD B2C se produce mediante una aplicación que se registra en el inquilino B2C. En esta sección se enumeran los pasos opcionales que puede llevar a cabo para crear una aplicación de prueba, si aún no lo ha hecho.

[!INCLUDE [active-directory-b2c-appreg-idp](../../includes/active-directory-b2c-appreg-idp.md)]

## <a name="update-and-test-the-relying-party-file"></a>Actualización y prueba del archivo del usuario de confianza

Actualice el archivo de usuario de confianza (RP) que inicia el recorrido del usuario recién creado:

1. Realice una copia del archivo *SignUpOrSignIn.xml* en el directorio de trabajo y cámbiele el nombre. Por ejemplo, cambie su nombre a *.xml*.
2. Abra el nuevo archivo y actualice el valor del atributo **PolicyId** del elemento **TrustFrameworkPolicy** con un valor único. Por ejemplo, `SignUpSignInSalesforce`.
3. Actualice el valor de **PublicPolicyUri** con el URI para la directiva. Por ejemplo: `http://contoso.com/B2C_1A_signup_signin_salesforce`
4. Actualice el valor del atributo **ReferenceId** del elemento **DefaultUserJourney** para que coincida con el identificador del nuevo recorrido del usuario que ha creado (SignUpSignInSalesforce).
5. Guarde los cambios, cargue el archivo y seleccione la nueva directiva en la lista.
6. Asegúrese de que la aplicación de Azure AD B2C que creó está seleccionada en el campo **Seleccionar aplicación** y pruébela; para ello, haga clic en **Ejecutar ahora**.

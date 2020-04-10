---
title: Incorporación de notificaciones y personalización de la entrada del usuario en directivas personalizadas
titleSuffix: Azure AD B2C
description: Aprenda a personalizar la entrada del usuario y a agregar notificaciones en el proceso de registro o inicio de sesión en Azure Active Directory B2C.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 03/17/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 85f2ab6f8c3e5edda027e44eeda13a3279a88321
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "79473683"
---
#  <a name="add-claims-and-customize-user-input-using-custom-policies-in-azure-active-directory-b2c"></a>Adición de notificaciones y personalización de la entrada del usuario mediante directivas personalizadas en Azure Active Directory B2C

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

En este artículo, recopilará un nuevo atributo durante el recorrido de registro en Azure Active Directory B2C (Azure AD B2C). Obtendrá la ciudad de los usuarios, la configurará como una lista desplegable y definirá si es necesario proporcionarla.

> [!NOTE]
> En este ejemplo se usa una notificación "city" integrada. En su lugar, puede elegir uno de los [atributos integrados de Azure AD B2C](user-profile-attributes.md) admitidos o un atributo personalizado. Para usar un atributo personalizado, [habilite los atributos personalizados en la directiva](custom-policy-custom-attributes.md). Para usar otro atributo integrado o personalizado, reemplace "city" por el atributo que prefiera; por ejemplo, el atributo integrado *jobTitle* o uno personalizado como *extension_loyaltyId*.  

Para recopilar los datos iniciales de los usuarios, utilice el recorrido del usuario de registro o inicio de sesión. Las reclamaciones posteriores se pueden recopilar mediante el recorrido de edición de perfil del usuario. Siempre que Azure AD B2C recopila información del usuario de forma directa e interactiva, Identity Experience Framework utiliza su [perfil técnico autoafirmado](self-asserted-technical-profile.md). En este ejemplo:

1. Define una notificación de "ciudad". 
1. Pide al usuario que especifique su ciudad.
1. Conserva la ciudad en el perfil de usuario en el directorio de Azure AD B2C.
1. Lee la notificación de ciudad en el directorio de Azure AD B2C en cada inicio de sesión.
1. Devuelve la ciudad a la aplicación de usuario de confianza después de iniciar sesión o registrarse.  

## <a name="prerequisites"></a>Prerrequisitos

Realice los pasos del artículo [Introducción a las directivas personalizadas](custom-policy-get-started.md). Debe tener una directiva personalizada activa para registrarse e iniciar sesión con cuentas sociales y locales.

## <a name="define-a-claim"></a>Definición de una notificación

Una notificación proporciona un almacenamiento temporal de datos durante la ejecución de una directiva de Azure AD B2C. El [esquema de notificaciones](claimsschema.md) es el lugar en el que se declaran las notificaciones. Los elementos siguientes se usan para definir la notificación:

- **DisplayName**: cadena que define la etiqueta que se muestra al usuario.
- [DataType](claimsschema.md#datatype): tipo de notificación.
- **UserHelpText**: ayuda al usuario a entender los requisitos.
- [UserInputType](claimsschema.md#userinputtype): tipo de control de entrada, como cuadro de texto, botón de selección, lista desplegable o selecciones múltiples.

Abra el archivo de extensiones de la directiva. Por ejemplo, <em>`SocialAndLocalAccounts/`**`TrustFrameworkExtensions.xml`**</em>.

1. Busque el elemento [BuildingBlocks](buildingblocks.md). Si el elemento no existe, agréguelo.
1. Busque el elemento [ClaimsSchema](claimsschema.md). Si el elemento no existe, agréguelo.
1. Agregue la notificación de ciudad al elemento **ClaimsSchema**.  

```xml
<ClaimType Id="city">
  <DisplayName>City where you work</DisplayName>
  <DataType>string</DataType>
  <UserInputType>DropdownSingleSelect</UserInputType>
  <Restriction>
    <Enumeration Text="Bellevue" Value="bellevue" SelectByDefault="false" />
    <Enumeration Text="Redmond" Value="redmond" SelectByDefault="false" />
    <Enumeration Text="Kirkland" Value="kirkland" SelectByDefault="false" />
  </Restriction>
</ClaimType>
```

## <a name="add-a-claim-to-the-user-interface"></a>Incorporación de una notificación a la interfaz de usuario

Los siguientes perfiles técnicos son [autoafirmados](self-asserted-technical-profile.md) y se invocan cuando se espera que un usuario proporcione una entrada:

- **LocalAccountSignUpWithLogonEmail**: flujo de registro de la cuenta local.
- **SelfAsserted-Social**: primer inicio de sesión de un usuario de una cuenta federada.
- **SelfAsserted-ProfileUpdate**: flujo de perfil de edición.

Para recopilar la notificación de ciudad durante el registro, se debe agregar como notificación de salida al perfil técnico `LocalAccountSignUpWithLogonEmail`. Reemplace este perfil técnico en el archivo de extensión. Especifique la lista completa de notificaciones de salida para controlar el orden en que se presentan las notificaciones en la pantalla. Busque el elemento **ClaimsProviders**. Agregue un nuevo elemento ClaimsProviders tal como se muestra a continuación:

```xml
<ClaimsProvider>
  <DisplayName>Local Account</DisplayName>
  <TechnicalProfiles>
    <!--Local account sign-up page-->
    <TechnicalProfile Id="LocalAccountSignUpWithLogonEmail">
      <OutputClaims>
       <OutputClaim ClaimTypeReferenceId="email" PartnerClaimType="Verified.Email" Required="true" />
       <OutputClaim ClaimTypeReferenceId="newPassword" Required="true" />
       <OutputClaim ClaimTypeReferenceId="reenterPassword" Required="true" />
       <OutputClaim ClaimTypeReferenceId="displayName" />
       <OutputClaim ClaimTypeReferenceId="givenName" />
       <OutputClaim ClaimTypeReferenceId="surName" />
       <OutputClaim ClaimTypeReferenceId="city"/>
     </OutputClaims>
   </TechnicalProfile>
  </TechnicalProfiles>
</ClaimsProvider>
<ClaimsProvider>
```

Para recopilar la notificación de ciudad después del inicio de sesión inicial con una cuenta federada, se debe agregar como notificación de salida al perfil técnico `SelfAsserted-Social`. Para que los usuarios de cuentas locales y federadas puedan editar sus datos de perfil posteriormente, agregue la notificación de salida al perfil técnico `SelfAsserted-ProfileUpdate`. Reemplace estos perfiles técnicos en el archivo de extensión. Especifique la lista completa de notificaciones de salida para controlar el orden en que se presentan las notificaciones en la pantalla. Busque el elemento **ClaimsProviders**. Agregue un nuevo elemento ClaimsProviders tal como se muestra a continuación:

```xml
  <DisplayName>Self Asserted</DisplayName>
  <TechnicalProfiles>
    <!--Federated account first-time sign-in page-->
    <TechnicalProfile Id="SelfAsserted-Social">
      <OutputClaims>
        <OutputClaim ClaimTypeReferenceId="displayName"/>
        <OutputClaim ClaimTypeReferenceId="givenName"/>
        <OutputClaim ClaimTypeReferenceId="surname"/>
        <OutputClaim ClaimTypeReferenceId="city"/>
      </OutputClaims>
    </TechnicalProfile>
    <!--Edit profile page-->
    <TechnicalProfile Id="SelfAsserted-ProfileUpdate">
      <OutputClaims>
        <OutputClaim ClaimTypeReferenceId="displayName"/>
        <OutputClaim ClaimTypeReferenceId="givenName" />
        <OutputClaim ClaimTypeReferenceId="surname" />
        <OutputClaim ClaimTypeReferenceId="city"/>
      </OutputClaims>
    </TechnicalProfile>
  </TechnicalProfiles>
</ClaimsProvider>
```

## <a name="read-and-write-a-claim"></a>Lectura y escritura de una notificación

Los siguientes perfiles técnicos son [perfiles técnicos de Active Directory](active-directory-technical-profile.md), que leen y escriben datos en Azure Active Directory.  
Use `PersistedClaims` para escribir datos en el perfil de usuario y `OutputClaims` para leer datos del perfil de usuario en los perfiles técnicos de Active Directory correspondientes.

Reemplace estos perfiles técnicos en el archivo de extensión. Busque el elemento **ClaimsProviders**.  Agregue un nuevo elemento ClaimsProviders tal como se muestra a continuación:

```xml
<ClaimsProvider>
  <DisplayName>Azure Active Directory</DisplayName>
  <TechnicalProfiles>
    <!-- Write data during a local account sign-up flow. -->
    <TechnicalProfile Id="AAD-UserWriteUsingLogonEmail">
      <PersistedClaims>
        <PersistedClaim ClaimTypeReferenceId="city"/>
      </PersistedClaims>
    </TechnicalProfile>
    <!-- Write data during a federated account first-time sign-in flow. -->
    <TechnicalProfile Id="AAD-UserWriteUsingAlternativeSecurityId">
      <PersistedClaims>
        <PersistedClaim ClaimTypeReferenceId="city"/>
      </PersistedClaims>
    </TechnicalProfile>
    <!-- Write data during edit profile flow. -->
    <TechnicalProfile Id="AAD-UserWriteProfileUsingObjectId">
      <PersistedClaims>
        <PersistedClaim ClaimTypeReferenceId="city"/>
      </PersistedClaims>
    </TechnicalProfile>
    <!-- Read data after user authenticates with a local account. -->
    <TechnicalProfile Id="AAD-UserReadUsingEmailAddress">
      <OutputClaims>  
        <OutputClaim ClaimTypeReferenceId="city" />
      </OutputClaims>
    </TechnicalProfile>
    <!-- Read data after user authenticates with a federated account. -->
    <TechnicalProfile Id="AAD-UserReadUsingObjectId">
      <OutputClaims>  
        <OutputClaim ClaimTypeReferenceId="city" />
      </OutputClaims>
    </TechnicalProfile>
  </TechnicalProfiles>
</ClaimsProvider>
```

## <a name="include-a-claim-in-the-token"></a>Incorporación de una notificación en el token 

Para devolver la notificación de ciudad a la aplicación de usuario de confianza, agregue una notificación de salida al archivo <em>`SocialAndLocalAccounts/`**`SignUpOrSignIn.xml`**</em>. La notificación de salida se agregará al token después de un recorrido del usuario correcto y se enviará a la aplicación. Modifique el elemento de perfil técnico en la sección de usuario de confianza para agregar la ciudad como una notificación de salida.
 
```xml
<RelyingParty>
  <DefaultUserJourney ReferenceId="SignUpOrSignIn" />
  <TechnicalProfile Id="PolicyProfile">
    <DisplayName>PolicyProfile</DisplayName>
    <Protocol Name="OpenIdConnect" />
    <OutputClaims>
      <OutputClaim ClaimTypeReferenceId="displayName" />
      <OutputClaim ClaimTypeReferenceId="givenName" />
      <OutputClaim ClaimTypeReferenceId="surname" />
      <OutputClaim ClaimTypeReferenceId="email" />
      <OutputClaim ClaimTypeReferenceId="objectId" PartnerClaimType="sub"/>
      <OutputClaim ClaimTypeReferenceId="identityProvider" />
      <OutputClaim ClaimTypeReferenceId="tenantId" AlwaysUseDefaultValue="true" DefaultValue="{Policy:TenantObjectId}" />
      <OutputClaim ClaimTypeReferenceId="city" DefaultValue="" />
    </OutputClaims>
    <SubjectNamingInfo ClaimType="sub" />
  </TechnicalProfile>
</RelyingParty>
```

## <a name="test-the-custom-policy"></a>Prueba de la directiva personalizada

1. Inicie sesión en [Azure Portal](https://portal.azure.com).
2. Asegúrese de que usa el directorio que contiene el inquilino de Azure AD. Para ello, seleccione el filtro **Directorio y suscripción** que se encuentra en el menú superior y elija el directorio que contiene el inquilino de Azure AD.
3. Elija **Todos los servicios** en la esquina superior izquierda de Azure Portal, y busque y seleccione **Registros de aplicaciones**.
4. Seleccione **Marco de experiencia de identidad**.
5. Seleccione **Cargar directiva personalizada** y cargue los dos archivos de directiva modificados.
2. Seleccione la directiva de registro o inicio de sesión que cargó y haga clic en el botón **Ejecutar ahora**.
3. Debe poder registrarse con una dirección de correo electrónico.

La pantalla de registro debe ser similar a la captura de pantalla siguiente:

![Captura de pantalla de la opción de registro modificada](./media/custom-policy-configure-user-input/signup-with-city-claim-dropdown-example.png)

El token enviado de vuelta a la aplicación contiene la notificación `city`.

```json
{
  "typ": "JWT",
  "alg": "RS256",
  "kid": "X5eXk4xyojNFum1kl2Ytv8dlNP4-c57dO6QGTVBwaNk"
}.{
  "exp": 1583500140,
  "nbf": 1583496540,
  "ver": "1.0",
  "iss": "https://contoso.b2clogin.com/f06c2fe8-709f-4030-85dc-38a4bfd9e82d/v2.0/",
  "aud": "e1d2612f-c2bc-4599-8e7b-d874eaca1ee1",
  "acr": "b2c_1a_signup_signin",
  "nonce": "defaultNonce",
  "iat": 1583496540,
  "auth_time": 1583496540,
  "name": "Emily Smith",
  "email": "joe@outlook.com",
  "given_name": "Emily",
  "family_name": "Smith",
  "city": "Bellevue"
  ...
}
```

## <a name="next-steps"></a>Pasos siguientes

- Más información sobre el elemento [ClaimsSchema](claimsschema.md) en la referencia de IEF.
- Aprenda a [utilizar atributos personalizados en una directiva de edición de perfiles personalizados](custom-policy-custom-attributes.md).

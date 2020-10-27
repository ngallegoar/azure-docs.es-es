---
title: Administración de SSO y personalización de tokens mediante directivas personalizadas
titleSuffix: Azure AD B2C
description: Aprenda cómo administrar SSO y personalizar tokens con directivas personalizadas en Azure Active Directory B2C.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: how-to
ms.date: 10/21/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: e72bd04bb41537546191b8ceb320c0722bd10146
ms.sourcegitcommit: f88074c00f13bcb52eaa5416c61adc1259826ce7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/21/2020
ms.locfileid: "92340298"
---
# <a name="manage-sso-and-token-customization-using-custom-policies-in-azure-active-directory-b2c"></a>Administración de SSO y personalización de tokens con directivas personalizadas en Azure Active Directory B2C

Este artículo proporciona información acerca de cómo puede administrar las configuraciones de inicio de sesión único (SSO), sesión y token mediante [directivas personalizadas](custom-policy-overview.md) en Azure Active Directory B2C (Azure AD B2C).

## <a name="jwt-token-lifetimes-and-claims-configuration"></a>Configuración de las notificaciones y las duraciones del token JWT

Para cambiar la configuración de la duración del token, debe agregar un elemento [ClaimsProviders](claimsproviders.md) en el archivo de usuario de confianza de la directiva que quiere modificar.  **ClaimsProviders** es un elemento secundario del elemento [TrustFrameworkPolicy](trustframeworkpolicy.md).

Inserte el elemento ClaimsProviders entre el elemento BasePolicy y el elemento RelyingParty del archivo del usuario de confianza.

En el parámetro, deberá colocar la información que afecta a la duración del token. El XML es similar al de este ejemplo:

```xml
<ClaimsProviders>
  <ClaimsProvider>
    <DisplayName>Token Issuer</DisplayName>
    <TechnicalProfiles>
      <TechnicalProfile Id="JwtIssuer">
        <Metadata>
          <Item Key="token_lifetime_secs">3600</Item>
          <Item Key="id_token_lifetime_secs">3600</Item>
          <Item Key="refresh_token_lifetime_secs">1209600</Item>
          <Item Key="rolling_refresh_token_lifetime_secs">7776000</Item>
          <Item Key="IssuanceClaimPattern">AuthorityAndTenantGuid</Item>
          <Item Key="AuthenticationContextReferenceClaimPattern">None</Item>
        </Metadata>
      </TechnicalProfile>
    </TechnicalProfiles>
  </ClaimsProvider>
</ClaimsProviders>
```

Los valores siguientes se establecen en el ejemplo anterior:

- **Duración del token de acceso** : el valor se establece con el metadato **token_lifetime_secs** . El valor predeterminado es 3600 segundos (60 minutos).
- **Duración del token de identificador** : el valor se establece con el metadato **token_lifetime_secs** . El valor predeterminado es 3600 segundos (60 minutos).
- **Duración del token de actualización** : el valor se establece con el metadato **refresh_token_lifetime_secs** . El valor predeterminado es 1 209 600 segundos (14 días).
- **Duración de la ventana deslizante del token de actualización** : si quiere establecer este valor para el token de actualización, modifique el valor del metadato **rolling_refresh_token_lifetime_secs** . El valor predeterminado es 7 776 000 segundos (90 días). Si no quiere exigir una duración de ventana deslizante, reemplace esta línea por `<Item Key="allow_infinite_rolling_refresh_token">True</Item>`.
- **Notificación del emisor (iss)** : este valor se establece con el metadato **IssuanceClaimPattern** . Los valores aplicables son `AuthorityAndTenantGuid` y `AuthorityWithTfp`.
- **Configuración de notificación que representa el identificador de directiva** : las opciones para configurar este valor son `TFP` (directiva de plataforma de confianza) y `ACR` (referencia de contexto de autenticación). `TFP` es el valor recomendado. Establezca el valor de **AuthenticationContextReferenceClaimPattern** en `None`.

    En el elemento **ClaimsSchema** , agregue este elemento:

    ```xml
    <ClaimType Id="trustFrameworkPolicy">
      <DisplayName>Trust framework policy name</DisplayName>
      <DataType>string</DataType>
    </ClaimType>
    ```

    En **OutputClaims** , agregue este elemento:

    ```xml
    <OutputClaim ClaimTypeReferenceId="trustFrameworkPolicy" Required="true" DefaultValue="{policy}" />
    ```

    Para ACR, quite **AuthenticationContextReferenceClaimPattern** .

- **Notificación de asunto (sub)** : esta opción se establece de forma predeterminada en ObjectID. Si quiere cambiarla a `Not Supported`, reemplace esta línea:

    ```xml
    <OutputClaim ClaimTypeReferenceId="objectId" PartnerClaimType="sub" />
    ```

    por esta otra:

    ```xml
    <OutputClaim ClaimTypeReferenceId="sub" />
    ```

> [!NOTE]
> Las aplicaciones de página única que usan el flujo de código de autorización con PKCE siempre tienen una duración de token de actualización de 24 horas. [Más información sobre las consecuencias de seguridad de los tokens de actualización en el explorador](../active-directory/develop/reference-third-party-cookies-spas.md#security-implications-of-refresh-tokens-in-the-browser)

## <a name="provide-optional-claims-to-your-app"></a>Proporcionar notificaciones opcionales a la aplicación

Las notificaciones de salida del [perfil técnico de la directiva del usuario de confianza](relyingparty.md#technicalprofile) son valores que se devuelven a una aplicación. La adición de notificaciones de salida enviará la notificación al token después de un recorrido del usuario correcto y se enviará a la aplicación. Modifique el elemento de perfil técnico en la sección del usuario de confianza para agregar las notificaciones deseadas como una notificación de salida.

1. Abra el archivo de la directiva personalizada. Por ejemplo, SignUpOrSignin.xml.
1. Busque el elemento OutputClaims. Agregue el elemento OutputClaim que desea incluir en el token. 
1. Establezca los atributos de la notificaciones de salida. 

En el siguiente ejemplo se agrega la notificación `accountBalance`. La notificación accountBalance se envía a la aplicación como un saldo. 

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
      <!--Add the optional claims here-->
      <OutputClaim ClaimTypeReferenceId="accountBalance" DefaultValue="" PartnerClaimType="balance" />
    </OutputClaims>
    <SubjectNamingInfo ClaimType="sub" />
  </TechnicalProfile>
</RelyingParty>
```

El elemento OutputClaim contiene los atributos siguientes:

  - **ClaimTypeReferenceId** : el identificador de un tipo de notificación que ya se ha definido en la sección [ClaimsSchema](claimsschema.md) del archivo de directiva o del archivo de directiva principal.
  - **PartnerClaimType** : permite cambiar el nombre de la notificación en el token. 
  - **DefaultValue** : un valor predeterminado. También puede establecer el valor predeterminado en un [solucionador de notificaciones](claim-resolver-overview.md), como el identificador de inquilino.
  - **AlwaysUseDefaultValue** : fuerza el uso del valor predeterminado.

## <a name="next-steps"></a>Pasos siguientes

- Más información sobre la [sesión de Azure AD B2C](session-overview.md).
- Obtenga información sobre cómo [configurar el comportamiento de la sesión en las directivas personalizadas](session-behavior-custom-policy.md).
- Referencia: [JwtIssuer](jwt-issuer-technical-profile.md).

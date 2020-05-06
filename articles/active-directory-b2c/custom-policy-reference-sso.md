---
title: Administración de sesiones de inicio de sesión único mediante directivas personalizadas
titleSuffix: Azure AD B2C
description: Obtenga información sobre cómo administrar sesiones SSO mediante directivas personalizadas en Azure AD B2C.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 04/28/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: f2887ab23dd89f1a3e1e3112ce3713ef1139de8e
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/28/2020
ms.locfileid: "82229687"
---
# <a name="single-sign-on-session-management-in-azure-active-directory-b2c"></a>Administración de sesión de inicio de sesión único en Azure Active Directory B2C

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

La administración de [sesiones SSO (inicio de sesión único)](session-overview.md) en Azure Active Directory B2C (Azure AD B2C) permite a los administradores controlar la interacción con el usuario después de que este se haya autenticado. Por ejemplo, el administrador puede controlar si se muestra la selección de proveedores de identidades, o si se tienen que volver a especificar los detalles de la cuenta. En este artículo se describe cómo establecer la configuración de SSO para Azure AD B2C.

La administración de sesiones de SSO utiliza la misma semántica que cualquier otro perfil técnico en directivas personalizadas. Cuando se ejecuta un paso de orquestación, el perfil técnico asociado al paso se consulta para la referencia `UseTechnicalProfileForSessionManagement`. Si existe alguno, se comprueba el proveedor de sesión de SSO al que se hace referencia para ver si el usuario es un participante de la sesión. Si es así, se utiliza el proveedor de sesión de SSO para volver a llenar la sesión. De forma similar, una vez completada la ejecución de un paso de orquestación, se utiliza el proveedor para almacenar información en la sesión si se ha especificado un proveedor de sesión de SSO.

Azure AD B2C ha definido un número de proveedores de sesión de SSO que se pueden usar:

|Proveedor de sesión  |Ámbito  |
|---------|---------|
|[NoopSSOSessionProvider](#noopssosessionprovider)     |  None       |       
|[DefaultSSOSessionProvider](#defaultssosessionprovider)    | Administrador de sesión interno de Azure AD B2C.      |       
|[ExternalLoginSSOSessionProvider](#externalloginssosessionprovider)     | Entre Azure AD B2C y OAuth1, OAuth2 o el proveedor de identidades de OpenId Connect.        |         |
|[OAuthSSOSessionProvider](#oauthssosessionprovider)     | Entre una aplicación de usuario de confianza de OAuth2 u OpenID Connect y Azure AD B2C.        |        
|[SamlSSOSessionProvider](#samlssosessionprovider)     | Entre el proveedor de identidades de Azure AD B2C y SAML. Y entre un proveedor de servicios de SAML (aplicación de usuario de confianza) y Azure AD B2C.  |        




Las clases de administración de SSO se especifican mediante el elemento `<UseTechnicalProfileForSessionManagement ReferenceId="{ID}" />` de un perfil técnico.

## <a name="input-claims"></a>Notificaciones de entrada

El elemento `InputClaims` está vacío o ausente.

## <a name="persisted-claims"></a>Notificaciones persistentes

Las notificaciones que tienen que devolverse a la aplicación o ser usadas por condiciones previas en los pasos posteriores deben almacenarse en la sesión o aumentarse mediante una lectura desde el perfil del usuario en el directorio. El uso de notificaciones persistentes garantiza que los recorridos de autenticación no producirán errores en las notificaciones que faltan. Para agregar notificaciones en la sesión, utilice el elemento `<PersistedClaims>` del perfil técnico. Cuando se utiliza el proveedor para volver a llenar la sesión, las notificaciones persistentes se agregan al contenedor de notificaciones.

## <a name="output-claims"></a>Notificaciones de salida

`<OutputClaims>` se utiliza para recuperar notificaciones de la sesión.

## <a name="session-providers"></a>Proveedores de sesión

### <a name="noopssosessionprovider"></a>NoopSSOSessionProvider

Como el nombre indica, este proveedor no hace nada. Este proveedor se puede utilizar para suprimir el comportamiento de SSO de un perfil técnico específico. El siguiente perfil técnico `SM-Noop` se incluye en el [paquete de inicio de directivas personalizadas](custom-policy-get-started.md#custom-policy-starter-pack).

```XML
<TechnicalProfile Id="SM-Noop">
  <DisplayName>Noop Session Management Provider</DisplayName>
  <Protocol Name="Proprietary" Handler="Web.TPEngine.SSO.NoopSSOSessionProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
</TechnicalProfile>
```

### <a name="defaultssosessionprovider"></a>DefaultSSOSessionProvider

Este proveedor se puede utilizar para almacenar notificaciones en una sesión. Se suele hacer referencia a dicho proveedor en un perfil técnico que se usa para administrar cuentas locales y federadas. El siguiente perfil técnico `SM-AAD` se incluye en el [paquete de inicio de directivas personalizadas](custom-policy-get-started.md#custom-policy-starter-pack).

```XML
<TechnicalProfile Id="SM-AAD">
  <DisplayName>Session Management Provider</DisplayName>
  <Protocol Name="Proprietary" Handler="Web.TPEngine.SSO.DefaultSSOSessionProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
  <PersistedClaims>
    <PersistedClaim ClaimTypeReferenceId="objectId" />
    <PersistedClaim ClaimTypeReferenceId="signInName" />
    <PersistedClaim ClaimTypeReferenceId="authenticationSource" />
    <PersistedClaim ClaimTypeReferenceId="identityProvider" />
    <PersistedClaim ClaimTypeReferenceId="newUser" />
    <PersistedClaim ClaimTypeReferenceId="executed-SelfAsserted-Input" />
  </PersistedClaims>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="objectIdFromSession" DefaultValue="true"/>
  </OutputClaims>
</TechnicalProfile>
```


El siguiente perfil técnico `SM-MFA` se incluye en el [paquete de inicio de directivas personalizadas](custom-policy-get-started.md#custom-policy-starter-pack) `SocialAndLocalAccountsWithMfa`. Este perfil técnico administra la sesión de autenticación multifactor.

```XML
<TechnicalProfile Id="SM-MFA">
  <DisplayName>Session Mananagement Provider</DisplayName>
  <Protocol Name="Proprietary" Handler="Web.TPEngine.SSO.DefaultSSOSessionProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
  <PersistedClaims>
    <PersistedClaim ClaimTypeReferenceId="Verified.strongAuthenticationPhoneNumber" />
  </PersistedClaims>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="isActiveMFASession" DefaultValue="true"/>
  </OutputClaims>
</TechnicalProfile>
```

### <a name="externalloginssosessionprovider"></a>ExternalLoginSSOSessionProvider

Este proveedor se usa para eliminar la pantalla "Elegir proveedor de identidades" y cerrar la sesión de un proveedor de identidades federado. Normalmente se hace referencia al mismo en un perfil técnico configurado para un proveedor de identidades federado, como Facebook o Azure Active Directory. El siguiente perfil técnico `SM-SocialLogin` se incluye en el [paquete de inicio de directivas personalizadas](custom-policy-get-started.md#custom-policy-starter-pack).

```XML
<TechnicalProfile Id="SM-SocialLogin">
  <DisplayName>Session Management Provider</DisplayName>
  <Protocol Name="Proprietary" Handler="Web.TPEngine.SSO.ExternalLoginSSOSessionProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
  <Metadata>
    <Item Key="AlwaysFetchClaimsFromProvider">true</Item>
  </Metadata>
  <PersistedClaims>
    <PersistedClaim ClaimTypeReferenceId="AlternativeSecurityId" />
  </PersistedClaims>
</TechnicalProfile>
```

#### <a name="metadata"></a>Metadatos

| Atributo | Obligatorio | Descripción|
| --- | --- | --- |
| AlwaysFetchClaimsFromProvider | No | No se usa actualmente, se puede omitir. |

### <a name="oauthssosessionprovider"></a>OAuthSSOSessionProvider

Este proveedor se usa para administrar las sesiones de Azure AD B2C entre un usuario de confianza de OAuth2 u OpenID Connect y Azure AD B2C.

```xml
<TechnicalProfile Id="SM-jwt-issuer">
  <DisplayName>Session Management Provider</DisplayName>
  <Protocol Name="Proprietary" Handler="Web.TPEngine.SSO.OAuthSSOSessionProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
</TechnicalProfile>
```

### <a name="samlssosessionprovider"></a>SamlSSOSessionProvider

Este proveedor se utiliza para administrar las sesiones SAML de Azure AD B2C entre una aplicación de usuario de confianza o un proveedor de identidades SAML federado. Cuando se utiliza el proveedor de SSO para almacenar una sesión de proveedor de identidades SAML, `RegisterServiceProviders` se debe establecer en `false`. El `SM-Saml-idp`perfil técnico de del proveedor de identidades SAML[ usa el siguiente perfil técnico de ](saml-identity-provider-technical-profile.md).

```XML
<TechnicalProfile Id="SM-Saml-idp">
  <DisplayName>Session Management Provider</DisplayName>
  <Protocol Name="Proprietary" Handler="Web.TPEngine.SSO.SamlSSOSessionProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
  <Metadata>
    <Item Key="RegisterServiceProviders">false</Item>
  </Metadata>
</TechnicalProfile>
```

Al usar el proveedor para almacenar la sesión SAML de B2C, `RegisterServiceProviders` se debe establecer en `true`. El cierre de sesión de SAML requiere que se completen `SessionIndex` y `NameID`.

El [perfil técnico de emisor de SAML](saml-issuer-technical-profile.md) utiliza el siguiente perfil técnico `SM-Saml-issuer`.

```XML
<TechnicalProfile Id="SM-Saml-issuer">
  <DisplayName>Session Management Provider</DisplayName>
  <Protocol Name="Proprietary" Handler="Web.TPEngine.SSO.SamlSSOSessionProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null"/>
</TechnicalProfile>
```

#### <a name="metadata"></a>Metadatos

| Atributo | Obligatorio | Descripción|
| --- | --- | --- |
| IncludeSessionIndex | No | No se usa actualmente, se puede omitir.|
| RegisterServiceProviders | No | Indica que el proveedor debe registrar todos los proveedores de servicios SAML que han emitido una aserción. Valores posibles: `true` (opción predeterminada) o `false`.|


## <a name="next-steps"></a>Pasos siguientes

- Más información sobre la [sesión de Azure AD B2C](session-overview.md).

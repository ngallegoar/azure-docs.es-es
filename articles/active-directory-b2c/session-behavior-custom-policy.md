---
title: 'Configuración del comportamiento de la sesión mediante directivas personalizadas: Azure Active Directory B2C | Microsoft Docs'
description: Configure el comportamiento de la sesión mediante directivas personalizadas en Azure Active Directory B2C.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: how-to
ms.date: 05/07/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: a2f20a4521efe2806c4bc66e4612b99caf84382a
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85385270"
---
# <a name="configure-session-behavior-using-custom-policies-in-azure-active-directory-b2c"></a>Configuración del comportamiento de la sesión mediante directivas personalizadas en Azure Active Directory B2C

La administración de [sesiones SSO (inicio de sesión único)](session-overview.md) en Azure Active Directory B2C (Azure AD B2C) permite a los administradores controlar la interacción con el usuario después de que este se haya autenticado. Por ejemplo, el administrador puede controlar si se muestra la selección de proveedores de identidades, o si se tienen que volver a especificar los detalles de la cuenta. En este artículo se describe cómo establecer la configuración de SSO para Azure AD B2C.

## <a name="session-behavior-properties"></a>Propiedades de comportamiento de sesión

Puede usar las propiedades siguientes para administrar sesiones de la aplicación web:

- **Vigencia (en minutos) de la sesión de la aplicación web**: la vigencia de la cookie de sesión de Azure AD B2C almacenada en el explorador del usuario tras una autenticación correcta.
    - Valor predeterminado = 86 400 segundos (1440 minutos).
    - Mínimo (inclusivo) = 900 segundos (15 minutos).
    - Máximo (inclusivo) = 86 400 segundos (1440 minutos).
- **Tiempo de expiración de la sesión de la aplicación web**: el [tipo de expiración de sesión](session-overview.md#session-expiry-type), *Rolling* (acumulado) o *Absolute* (absoluto). 
- **Configuración de inicio de sesión único**: el [ámbito de la sesión](session-overview.md#session-scope) del comportamiento de inicio de sesión único (SSO) entre varias aplicaciones y flujos de usuario en el inquilino de Azure AD B2C. 

## <a name="configure-the-properties"></a>Configuración de las propiedades

Para cambiar las configuraciones de comportamiento de sesión y SSO, debe agregar un elemento **UserJourneyBehaviors** dentro del elemento [RelyingParty](relyingparty.md).  El elemento **UserJourneyBehaviors** debe aparecer inmediatamente después de **DefaultUserJourney**. El elemento **UserJourneyBehavors** debería parecerse a este ejemplo:

```xml
<UserJourneyBehaviors>
   <SingleSignOn Scope="Application" />
   <SessionExpiryType>Absolute</SessionExpiryType>
   <SessionExpiryInSeconds>86400</SessionExpiryInSeconds>
</UserJourneyBehaviors>
```

## <a name="single-sign-out"></a>Cierre de sesión único

### <a name="configure-the-applications"></a>Configuración de las aplicaciones

Cuando redirige al usuario al punto de conexión de cierre de sesión de Azure AD B2C (para los protocolos OAuth2 y SAML), Azure AD B2C borra la sesión del usuario del explorador.  Para permitir el [cierre de sesión único](session-overview.md#single-sign-out), establezca la `LogoutUrl` de la aplicación desde Azure Portal:

1. Acceda a [Azure Portal](https://portal.azure.com).
1. Para elegir la instancia de Azure AD B2C, haga clic en su cuenta en la esquina superior derecha de la página.
1. En el menú izquierdo, elija **Azure AD B2C**, seleccione **Registros de aplicaciones** y, después, seleccione la aplicación.
1. Seleccione **Configuración**, **Propiedades** y después busque el cuadro de texto **URL de cierre de sesión**. 

### <a name="configure-the-token-issuer"></a>Configuración del emisor del token 

Para admitir el cierre de sesión único, los perfiles técnicos del emisor de tokens para JWT y SAML deben especificar lo siguiente:

- El nombre del protocolo, como `<Protocol Name="OpenIdConnect" />`.
- La referencia al perfil técnico de la sesión, como `UseTechnicalProfileForSessionManagement ReferenceId="SM-OAuth-issuer" />`.

En el ejemplo siguiente se muestran emisores de tokens de JWT y SAML con el cierre de sesión único:

```xml
<ClaimsProvider>
  <DisplayName>Local Account SignIn</DisplayName>
  <TechnicalProfiles>
    <!-- JWT Token Issuer -->
    <TechnicalProfile Id="JwtIssuer">
      <DisplayName>JWT token Issuer</DisplayName>
      <Protocol Name="OpenIdConnect" />
      <OutputTokenFormat>JWT</OutputTokenFormat>
      ...    
      <UseTechnicalProfileForSessionManagement ReferenceId="SM-OAuth-issuer" />
    </TechnicalProfile>

    <!-- Session management technical profile for OIDC based tokens -->
    <TechnicalProfile Id="SM-OAuth-issuer">
      <DisplayName>Session Management Provider</DisplayName>
      <Protocol Name="Proprietary" Handler="Web.TPEngine.SSO.OAuthSSOSessionProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
    </TechnicalProfile>

    <!--SAML token issuer-->
    <TechnicalProfile Id="Saml2AssertionIssuer">
      <DisplayName>SAML token issuer</DisplayName>
      <Protocol Name="SAML2" />
      <OutputTokenFormat>SAML2</OutputTokenFormat>
      ...
      <UseTechnicalProfileForSessionManagement ReferenceId="SM-Saml-issuer" />
    </TechnicalProfile>

    <!-- Session management technical profile for SAML based tokens -->
    <TechnicalProfile Id="SM-Saml-issuer">
      <DisplayName>Session Management Provider</DisplayName>
      <Protocol Name="Proprietary" Handler="Web.TPEngine.SSO.SamlSSOSessionProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
    </TechnicalProfile>
  </TechnicalProfiles>
</ClaimsProvider>
```

## <a name="next-steps"></a>Pasos siguientes

- Más información sobre la [sesión de Azure AD B2C](session-overview.md).

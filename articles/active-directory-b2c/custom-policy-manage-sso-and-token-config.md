---
title: Administración de SSO y personalización de tokens mediante directivas personalizadas
titleSuffix: Azure AD B2C
description: Aprenda cómo administrar SSO y personalizar tokens con directivas personalizadas en Azure Active Directory B2C.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 10/09/2018
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: ff9f57af92c50c0df6f628113bd9490ca83e1310
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "78189300"
---
# <a name="manage-sso-and-token-customization-using-custom-policies-in-azure-active-directory-b2c"></a>Administración de SSO y personalización de tokens con directivas personalizadas en Azure Active Directory B2C

Este artículo proporciona información acerca de cómo puede administrar las configuraciones de inicio de sesión único (SSO), sesión y token mediante [directivas personalizadas](custom-policy-overview.md) en Azure Active Directory B2C (Azure AD B2C).

## <a name="token-lifetimes-and-claims-configuration"></a>Configuración de notificaciones y duración del token

Para cambiar la configuración de la duración del token, debe agregar un elemento [ClaimsProviders](claimsproviders.md) en el archivo de usuario de confianza de la directiva que quiere modificar.  **ClaimsProviders** es un elemento secundario del elemento [TrustFrameworkPolicy](trustframeworkpolicy.md).

Inserte el elemento ClaimsProviders entre el elemento BasePolicy y el elemento RelyingParty del archivo del usuario de confianza.

En el parámetro, deberá colocar la información que afecta a la duración del token. El XML es similar al de este ejemplo:

```XML
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

- **Duración del token de acceso**: el valor se establece con el metadato **token_lifetime_secs**. El valor predeterminado es 3600 segundos (60 minutos).
- **Duración del token de identificador**: el valor se establece con el metadato **token_lifetime_secs**. El valor predeterminado es 3600 segundos (60 minutos).
- **Duración del token de actualización**: el valor se establece con el metadato **refresh_token_lifetime_secs**. El valor predeterminado es 1 209 600 segundos (14 días).
- **Duración de la ventana deslizante del token de actualización**: si quiere establecer este valor para el token de actualización, modifique el valor del metadato **rolling_refresh_token_lifetime_secs**. El valor predeterminado es 7 776 000 segundos (90 días). Si no quiere exigir una duración de ventana deslizante, reemplace esta línea por `<Item Key="allow_infinite_rolling_refresh_token">True</Item>`.
- **Notificación del emisor (iss)** : este valor se establece con el metadato **IssuanceClaimPattern**. Los valores aplicables son `AuthorityAndTenantGuid` y `AuthorityWithTfp`.
- **Configuración de notificación que representa el identificador de directiva**: las opciones para configurar este valor son `TFP` (directiva de plataforma de confianza) y `ACR` (referencia de contexto de autenticación). `TFP` es el valor recomendado. Establezca el valor de **AuthenticationContextReferenceClaimPattern** en `None`.

    En el elemento **ClaimsSchema**, agregue este elemento:

    ```XML
    <ClaimType Id="trustFrameworkPolicy">
      <DisplayName>Trust framework policy name</DisplayName>
      <DataType>string</DataType>
    </ClaimType>
    ```

    En **OutputClaims**, agregue este elemento:

    ```XML
    <OutputClaim ClaimTypeReferenceId="trustFrameworkPolicy" Required="true" DefaultValue="{policy}" />
    ```

    Para ACR, quite **AuthenticationContextReferenceClaimPattern**.

- **Notificación de asunto (sub)** : esta opción se establece de forma predeterminada en ObjectID. Si quiere cambiarla a `Not Supported`, reemplace esta línea:

    ```XML
    <OutputClaim ClaimTypeReferenceId="objectId" PartnerClaimType="sub" />
    ```

    por esta otra:

    ```XML
    <OutputClaim ClaimTypeReferenceId="sub" />
    ```

## <a name="session-behavior-and-sso"></a>Comportamiento de sesión y SSO

Para cambiar las configuraciones de comportamiento de sesión y SSO, debe agregar un elemento **UserJourneyBehaviors** dentro del elemento [RelyingParty](relyingparty.md).  El elemento **UserJourneyBehaviors** debe aparecer inmediatamente después de **DefaultUserJourney**. El valor del elemento **UserJourneyBehavors** debería parecerse a este ejemplo:

```XML
<UserJourneyBehaviors>
   <SingleSignOn Scope="Application" />
   <SessionExpiryType>Absolute</SessionExpiryType>
   <SessionExpiryInSeconds>86400</SessionExpiryInSeconds>
</UserJourneyBehaviors>
```

Los valores siguientes se configuran en el ejemplo anterior:

- **Single sign on (SSO)** : el inicio de sesión único se configura con **SingleSignOn**. Los valores aplicables son `Tenant`, `Application`, `Policy` y `Suppressed`.
- **Tiempo de espera de la sesión de la aplicación web**: el valor se establece con el elemento **SessionExpiryType**. Los valores aplicables son `Absolute` y `Rolling`.
- **Duración de la sesión de la aplicación web**: el valor se establece con el elemento **SessionExpiryInSeconds**. El valor predeterminado es 86 400 segundos (1440 minutos).

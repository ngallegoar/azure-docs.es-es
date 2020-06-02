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
ms.date: 05/07/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 0b460d5a3c5535b74e349fa46c6a2ad55fc3a8d8
ms.sourcegitcommit: a6d477eb3cb9faebb15ed1bf7334ed0611c72053
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/08/2020
ms.locfileid: "82943644"
---
# <a name="manage-sso-and-token-customization-using-custom-policies-in-azure-active-directory-b2c"></a>Administración de SSO y personalización de tokens con directivas personalizadas en Azure Active Directory B2C

Este artículo proporciona información acerca de cómo puede administrar las configuraciones de inicio de sesión único (SSO), sesión y token mediante [directivas personalizadas](custom-policy-overview.md) en Azure Active Directory B2C (Azure AD B2C).

## <a name="jtw-token-lifetimes-and-claims-configuration"></a>Configuración de notificaciones y duración del token JTW

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

## <a name="next-steps"></a>Pasos siguientes

- Más información sobre la [sesión de Azure AD B2C](session-overview.md).
- Obtenga información sobre cómo [configurar el comportamiento de la sesión en las directivas personalizadas](session-behavior-custom-policy.md).
- Referencia: [JwtIssuer](jwt-issuer-technical-profile.md).
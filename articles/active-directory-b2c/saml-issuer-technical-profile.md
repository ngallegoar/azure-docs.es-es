---
title: Definición de un perfil técnico para un emisor de SAML en una directiva personalizada
titleSuffix: Azure AD B2C
description: Defina un perfil técnico para un emisor de tokens de Lenguaje de marcado de aserción de seguridad (SAML) en una directiva personalizada de Azure Active Directory B2C.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 04/27/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: d709bf02f1cb504121e52f88385d0f6c074b24a0
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85203595"
---
# <a name="define-a-technical-profile-for-a-saml-token-issuer-in-an-azure-active-directory-b2c-custom-policy"></a>Definición de un perfil técnico para un emisor de tokens de SAML en una directiva personalizada de Azure Active Directory B2C

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Azure Active Directory B2C (Azure AD B2C) emite varios tipos de tokens de seguridad a medida que procesa cada flujo de autenticación. Un perfil técnico de un emisor de tokens de SAML emite un token de SAML que se devuelve a la aplicación de usuario de confianza (proveedor de servicios). Este perfil técnico suele ser el último paso de orquestación en el recorrido del usuario.

## <a name="protocol"></a>Protocolo

El atributo **Name** del elemento **Protocol** tiene que establecerse en `None`. Establezca el elemento **OutputTokenFormat** en `SAML2`.

En el ejemplo siguiente se muestra un perfil técnico de `Saml2AssertionIssuer`:

```xml
<TechnicalProfile Id="Saml2AssertionIssuer">
  <DisplayName>Token Issuer</DisplayName>
  <Protocol Name="SAML2"/>
  <OutputTokenFormat>SAML2</OutputTokenFormat>
  <Metadata>
    <Item Key="IssuerUri">https://tenant-name.b2clogin.com/tenant-name.onmicrosoft.com/B2C_1A_signup_signin_SAML</Item>
  </Metadata>
  <CryptographicKeys>
    <Key Id="MetadataSigning" StorageReferenceId="B2C_1A_SamlIdpCert"/>
    <Key Id="SamlMessageSigning" StorageReferenceId="B2C_1A_SamlIdpCert"/>
  </CryptographicKeys>
  <InputClaims/>
  <OutputClaims/>
  <UseTechnicalProfileForSessionManagement ReferenceId="SM-Saml-issuer"/>
</TechnicalProfile>
```

## <a name="input-output-and-persist-claims"></a>Notificaciones de entrada, de salida y persistentes

Los elementos **InputClaims**, **OutputClaims** y **PersistClaims** están vacíos o faltan. Los elementos **InutputClaimsTransformations** y **OutputClaimsTransformations** faltan también.

## <a name="metadata"></a>Metadatos

| Atributo | Obligatorio | Descripción |
| --------- | -------- | ----------- |
| IssuerUri | No | Nombre del emisor que aparece en la respuesta de SAML. El valor debe ser el mismo nombre que el configurado en la aplicación de usuario de confianza. |

## <a name="cryptographic-keys"></a>Claves de cifrado

El elemento CryptographicKeys contiene los siguientes atributos:

| Atributo | Obligatorio | Descripción |
| --------- | -------- | ----------- |
| MetadataSigning | Sí | El certificado X509 (conjunto de claves RSA) que se va a usar para firmar los metadatos SAML. Azure AD B2C usa esta clave para firmar los metadatos. |
| SamlMessageSigning| Sí| Especifique el certificado X509 (conjunto de claves RSA) que se va a usar para firmar los mensajes de SAML. Azure AD B2C usa esta clave para firmar la respuesta `<samlp:Response>` enviada al usuario de confianza.|

## <a name="session-management"></a>Administración de sesiones

Para configurar las sesiones de SAML de Azure AD B2C entre una aplicación de usuario de confianza, el atributo del elemento `UseTechnicalProfileForSessionManagement`, haga referencia a la sesión de inicio de sesión único [SamlSSOSessionProvider](custom-policy-reference-sso.md#samlssosessionprovider).

## <a name="next-steps"></a>Pasos siguientes

Consulte el siguiente artículo para obtener un ejemplo del uso de un perfil técnico del emisor de SAML:

- [Registrar una aplicación SAML en Azure AD B2C](connect-with-saml-service-providers.md)













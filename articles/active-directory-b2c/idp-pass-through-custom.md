---
title: Transmisión de un token de acceso a la aplicación mediante una directiva personalizada
titleSuffix: Azure AD B2C
description: Aprenda cómo puede pasar un token de acceso para proveedores de identidades de OAuth 2.0 como una notificación a la aplicación en Azure Active Directory B2C mediante una directiva personalizada.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: how-to
ms.date: 08/17/2019
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: c434ad6a724ba513caf7923916997600097b43f6
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "85387871"
---
# <a name="pass-an-access-token-through-a-custom-policy-to-your-application-in-azure-active-directory-b2c"></a>Pase de un token de acceso a través de una directiva personalizada a la aplicación en Azure Active Directory B2C

Un [directiva personalizada](custom-policy-get-started.md) en Azure Active Directory B2C (Azure AD B2C) proporciona una oportunidad a los usuarios para registrarse o iniciar sesión con un proveedor de identidades. Cuando esto sucede, Azure AD B2C recibe un [token de acceso](tokens-overview.md) del proveedor de identidades. Azure AD B2C usa ese token para recuperar información sobre el usuario. Agregue un tipo de notificación y envíe dicha notificación a la directiva personalizada para pasar el token a través de las aplicaciones que registre en Azure AD B2C.

Azure AD B2C admite pasar el token de acceso de los proveedores de identidades de [OAuth 2.0](authorization-code-flow.md) y [OpenID Connect](openid-connect.md). Para todos los demás proveedores de identidades, la notificación se devuelve en blanco.

## <a name="prerequisites"></a>Prerrequisitos

* La directiva personalizada se configura con un proveedor de identidades de OAuth 2.0. o de OpenID Connect.

## <a name="add-the-claim-elements"></a>Incorporación de elementos de notificación

1. Abra el archivo *TrustframeworkExtensions.xml* y agregue el siguiente elemento **ClaimType** con un identificador de `identityProviderAccessToken` al elemento **ClaimsSchema**:

    ```xml
    <BuildingBlocks>
      <ClaimsSchema>
        <ClaimType Id="identityProviderAccessToken">
          <DisplayName>Identity Provider Access Token</DisplayName>
          <DataType>string</DataType>
          <AdminHelpText>Stores the access token of the identity provider.</AdminHelpText>
        </ClaimType>
        ...
      </ClaimsSchema>
    </BuildingBlocks>
    ```

2. Agregue el elemento **OutputClaim** al elemento **TechnicalProfile** para cada proveedor de identidades de OAuth 2.0 para el que le gustaría el token de acceso. El ejemplo siguiente muestra el elemento agregado en el perfil técnico de Facebook:

    ```xml
    <ClaimsProvider>
      <DisplayName>Facebook</DisplayName>
      <TechnicalProfiles>
        <TechnicalProfile Id="Facebook-OAUTH">
          <OutputClaims>
            <OutputClaim ClaimTypeReferenceId="identityProviderAccessToken" PartnerClaimType="{oauth2:access_token}" />
          </OutputClaims>
          ...
        </TechnicalProfile>
      </TechnicalProfiles>
    </ClaimsProvider>
    ```

3. Guarde el archivo *TrustFrameworkExtensions.xml*.
4. Abra el archivo de directiva del usuario de confianza, como *SignUpOrSignIn.xml* y agregue el elemento **OutputClaim** a **TechnicalProfile**:

    ```xml
    <RelyingParty>
      <DefaultUserJourney ReferenceId="SignUpOrSignIn" />
      <TechnicalProfile Id="PolicyProfile">
        <OutputClaims>
          <OutputClaim ClaimTypeReferenceId="identityProviderAccessToken" PartnerClaimType="idp_access_token"/>
        </OutputClaims>
        ...
      </TechnicalProfile>
    </RelyingParty>
    ```

5. Guarde el archivo de directiva.

## <a name="test-your-policy"></a>Prueba de la directiva

Al probar las aplicaciones en Azure AD B2C, puede ser útil que el token de Azure AD B2C se devuelva a `https://jwt.ms` para poder revisar las notificaciones en él.

### <a name="upload-the-files"></a>Carga de los archivos

1. Inicie sesión en [Azure Portal](https://portal.azure.com/).
2. Asegúrese de usar el directorio que contiene el inquilino de Azure AD B2C. Para ello, haga clic en el filtro **Directorio y suscripción** del menú superior y seleccione el directorio que contiene el inquilino.
3. Elija **Todos los servicios** en la esquina superior izquierda de Azure Portal, y busque y seleccione **Azure AD B2C**.
4. Seleccione **Marco de experiencia de identidad**.
5. En la página Directivas personalizadas, haga clic en **Cargar directiva**.
6. Seleccione **Sobrescribir la directiva, si existe** y busque y seleccione el archivo *TrustframeworkExtensions.xml*.
7. Seleccione **Cargar**.
8. Repita los pasos 5 a 7 para el archivo del usuario de confianza, como *SignUpOrSignIn.xml*.

### <a name="run-the-policy"></a>Ejecución de la directiva

1. Abra la directiva que ha cambiado. Por ejemplo, *B2C_1A_signup_signin*.
2. En **Aplicación**, seleccione la aplicación que registró anteriormente. Para ver el token en el ejemplo siguiente, **URL de respuesta** debe mostrar `https://jwt.ms`.
3. Seleccione **Ejecutar ahora**.

    Debería ver algo parecido al siguiente ejemplo:

    ![Token descodificado en jwt.ms con el bloque idp_access_token resaltado](./media/idp-pass-through-custom/idp-pass-through-custom-token.PNG)

## <a name="next-steps"></a>Pasos siguientes

Obtenga más información sobre los tokens en la [referencia del token de Azure Active Directory B2C](tokens-overview.md).

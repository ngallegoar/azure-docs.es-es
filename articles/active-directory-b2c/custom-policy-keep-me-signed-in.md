---
title: Mantener la sesión iniciada en Azure Active Directory B2C
description: Aprenda a configurar Mantener la sesión iniciada (KMSI) en Azure Active Directory B2C.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 03/26/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 041fb8d881307b52fb170a11618f930debc522a4
ms.sourcegitcommit: 6397c1774a1358c79138976071989287f4a81a83
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/07/2020
ms.locfileid: "80803167"
---
# <a name="enable-keep-me-signed-in-kmsi-in-azure-active-directory-b2c"></a>Habilitación de Mantener la sesión iniciada (KMSI) en Azure Active Directory B2C

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Puede habilitar la funcionalidad Mantener la sesión iniciada (KMSI) para los usuarios y las aplicaciones nativas que tienen cuentas locales en el directorio de Azure Active Directory B2C (Azure AD B2C). Esta característica concede acceso a los usuarios que vuelven a la aplicación sin pedirles que escriban de nuevo el nombre de usuario y la contraseña. Este acceso se revocará cuando el usuario cierre la sesión.

Los usuarios no deben habilitar esta opción en equipos públicos.

![Ejemplo de página de inicio de sesión de registro que muestra la casilla Mantener la sesión iniciada](./media/custom-policy-keep-me-signed-in/kmsi.PNG)

## <a name="prerequisites"></a>Prerrequisitos

- Un inquilino de Azure AD B2C configurado para permitir el inicio de sesión en la cuenta local. KMSI no es compatible con cuentas de proveedor de identidades externo.
- Realice los pasos del artículo [Introducción a las directivas personalizadas](custom-policy-get-started.md).

## <a name="configure-the-page-identifier"></a>Configuración del identificador de página

Para habilitar KMSI, establezca el elemento de `DataUri` de la definición de contenido en [identificador de página](contentdefinitions.md#datauri) `unifiedssp` y [versión de la página](page-layout.md) *1.1.0* o superior.

1. Abra el archivo de extensión de la directiva. Por ejemplo, <em>`SocialAndLocalAccounts/`**`TrustFrameworkExtensions.xml`**</em>. Este archivo de extensión es uno de los archivos de directivas incluidos en el paquete de inicio de directivas personalizadas, que debería haber obtenido en el requisito previo, [Introducción a las directivas personalizadas](custom-policy-get-started.md).
1. Busque el elemento **BuildingBlocks**. Si el elemento no existe, agréguelo.
1. Agregue el elemento **ContentDefinitions** al elemento **BuildingBlocks** de la directiva.

    La directiva personalizada debería ser similar al fragmento de código siguiente:

    ```xml
    <BuildingBlocks>
      <ContentDefinitions>
        <ContentDefinition Id="api.signuporsignin">
          <DataUri>urn:com:microsoft:aad:b2c:elements:unifiedssp:1.1.0</DataUri>
        </ContentDefinition>
      </ContentDefinitions>
    </BuildingBlocks>
    ```

## <a name="add-the-metadata-to-the-self-asserted-technical-profile"></a>Incorporación de los metadatos al perfil técnico autoafirmado

Para agregar la casilla KMSI a la página de registro e inicio de sesión, establezca los metadatos de `setting.enableRememberMe` en true. Invalide los perfiles técnicos de SelfAsserted-LocalAccountSignin-Email en el archivo de extensión.

1. Busque el elemento ClaimsProviders. Si el elemento no existe, agréguelo.
1. Agregue el siguiente proveedor de notificaciones al elemento ClaimsProviders:

```XML
<ClaimsProvider>
  <DisplayName>Local Account</DisplayName>
  <TechnicalProfiles>
    <TechnicalProfile Id="SelfAsserted-LocalAccountSignin-Email">
      <Metadata>
        <Item Key="setting.enableRememberMe">True</Item>
      </Metadata>
    </TechnicalProfile>
  </TechnicalProfiles>
</ClaimsProvider>
```

1. Guarde el archivo de extensiones.

## <a name="configure-a-relying-party-file"></a>Configuración de un archivo de usuario de confianza

Actualice el archivo de usuario de confianza (RP) que inicia el recorrido del usuario que ha creado.

1. Abra el archivo de la directiva personalizada. Por ejemplo, *SignUpOrSignin.xml*.
1. Si aún no existe, agregue un nodo secundario `<UserJourneyBehaviors>` al nodo `<RelyingParty>`. Tiene que estar situado inmediatamente después de `<DefaultUserJourney ReferenceId="User journey Id" />`, por ejemplo: `<DefaultUserJourney ReferenceId="SignUpOrSignIn" />`.
1. Agregue el siguiente nodo como nodo secundario del elemento `<UserJourneyBehaviors>`.

    ```XML
    <UserJourneyBehaviors>
      <SingleSignOn Scope="Tenant" KeepAliveInDays="30" />
      <SessionExpiryType>Absolute</SessionExpiryType>
      <SessionExpiryInSeconds>1200</SessionExpiryInSeconds>
    </UserJourneyBehaviors>
    ```

    - **SessionExpiryType**: Indica cómo se extiende la sesión el tiempo especificado en `SessionExpiryInSeconds` y `KeepAliveInDays`. El valor `Rolling` (predeterminado) indica que la sesión se extiende cada vez que el usuario realiza la autenticación. El valor `Absolute` indica que el usuario está obligado a autenticarse de nuevo tras el período especificado.

    - **SessionExpiryInSeconds**: Vigencia de las cookies de sesión cuando *Mantener la sesión iniciada* no está habilitado, o si un usuario no selecciona *Mantener la sesión iniciada*. La sesión expira una vez transcurrido `SessionExpiryInSeconds` o se cierra el explorador.

    - **KeepAliveInDays**: Vigencia de las cookies de sesión cuando *Mantener la sesión iniciada* está habilitado y el usuario selecciona *Mantener la sesión iniciada*.  El valor de `KeepAliveInDays` tiene prioridad sobre el valor de `SessionExpiryInSeconds` y determina la hora de expiración de la sesión. Si un usuario cierra el explorador y vuelve a abrirlo, puede iniciar sesión en modo silencioso siempre que se encuentre dentro del período de KeepAliveInDays.

    Para más información, consulte los [comportamientos de recorrido del usuario](relyingparty.md#userjourneybehaviors).

Se recomienda que establezca el valor de SessionExpiryInSeconds para un período breve (1200 segundos), mientras que el valor de KeepAliveInDays se puede establecer en un período relativamente largo (30 días), como se muestra en el ejemplo siguiente:

```XML
<RelyingParty>
  <DefaultUserJourney ReferenceId="SignUpOrSignIn" />
  <UserJourneyBehaviors>
    <SingleSignOn Scope="Tenant" KeepAliveInDays="30" />
    <SessionExpiryType>Absolute</SessionExpiryType>
    <SessionExpiryInSeconds>1200</SessionExpiryInSeconds>
  </UserJourneyBehaviors>
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
    </OutputClaims>
    <SubjectNamingInfo ClaimType="sub" />
  </TechnicalProfile>
</RelyingParty>
```

## <a name="test-your-policy"></a>Prueba de la directiva

1. Guarde los cambios y después cargue el archivo.
1. Para probar la directiva personalizada que cargó, en Azure Portal, vaya a la página de la directiva y seleccione **Ejecutar ahora**.
1. Escriba su **nombre de usuario** y **contraseña**, seleccione **Mantener la sesión iniciada** y haga clic en **iniciar sesión**.
1. Vuelva a Azure Portal. Vaya a la página de la directiva y seleccione **Copiar** para copiar la dirección URL de inicio de sesión.
1. En la barra de direcciones del explorador, quite el parámetro `&prompt=login` de la cadena de consulta, que obliga al usuario a escribir sus credenciales en esa solicitud.
1. En el explorador, haga clic en **Ir**. Ahora, Azure AD B2C emitirá un token de acceso sin pedirle que vuelva a iniciar sesión. 

## <a name="next-steps"></a>Pasos siguientes

Puede encontrar la directiva de ejemplo [aquí](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/tree/master/scenarios/keep%20me%20signed%20in).

---
title: Incorporación de AD FS como proveedor de identidades de SAML mediante directivas personalizadas
titleSuffix: Azure AD B2C
description: Configure AD FS 2016 mediante el protocolo SAML y las directivas personalizadas en Azure Active Directory B2C.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: how-to
ms.date: 10/26/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 8cd761131fba23e89d1f72aed018a3e1dfd27e60
ms.sourcegitcommit: 4cb89d880be26a2a4531fedcc59317471fe729cd
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/27/2020
ms.locfileid: "92668744"
---
# <a name="add-ad-fs-as-a-saml-identity-provider-using-custom-policies-in-azure-active-directory-b2c"></a>Incorporación de AD FS como proveedor de identidades de SAML mediante las directivas personalizadas de Azure Active Directory B2C

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

En este artículo se muestra cómo habilitar el inicio de sesión para una cuenta de usuario de AD FS mediante [directivas personalizadas](custom-policy-overview.md) en Azure Active Directory B2C (Azure AD B2C). Habilite el inicio de sesión mediante la adición de un [perfil técnico de proveedor de identidades SAML](saml-identity-provider-technical-profile.md) a una directiva personalizada.

## <a name="prerequisites"></a>Prerrequisitos

- Siga los pasos de [Introducción a las directivas personalizadas en Azure Active Directory B2C](custom-policy-get-started.md).
- Asegúrese de que tiene acceso a un archivo .pfx del certificado con una clave privada. Puede generar su propio certificado firmado y cargarlo en Azure AD B2C. Azure AD B2C usa este certificado para firmar la solicitud SAML enviada al proveedor de identidades SAML. Para obtener más información sobre cómo generar un certificado, consulte [Generación de un certificado de firma](identity-provider-salesforce-custom.md#generate-a-signing-certificate).
- Para que Azure acepte la contraseña del archivo .pfx, la contraseña debe estar cifrada con la opción TripleDES-SHA1 en la utilidad de exportación del almacén de certificados de Windows en lugar de AES256-SHA256.

## <a name="create-a-policy-key"></a>Creación de una clave de directiva

Debe almacenar el certificado en el inquilino de Azure AD B2C.

1. Inicie sesión en [Azure Portal](https://portal.azure.com/).
2. Asegúrese de que usa el directorio que contiene el inquilino de Azure AD B2C. Seleccione el filtro **Directorio y suscripciones** del menú superior y elija el directorio que contiene el inquilino.
3. Elija **Todos los servicios** en la esquina superior izquierda de Azure Portal, y busque y seleccione **Azure AD B2C**.
4. En la página de introducción, seleccione **Identity Experience Framework**.
5. Seleccione **Claves de directiva** y luego **Agregar**.
6. En **Opciones** , elija `Upload`.
7. Escriba un **nombre** para la clave de directiva. Por ejemplo, `ADFSSamlCert`. Se agregará el prefijo `B2C_1A_` automáticamente al nombre de la clave.
8. Busque el archivo .pfx de certificado con la clave privada y selecciónelo.
9. Haga clic en **Crear**.

## <a name="add-a-claims-provider"></a>Incorporación de un proveedor de notificaciones

Si quiere que los usuarios inicien sesión con una cuenta de AD FS, deberá definirla como proveedor de notificaciones con el que Azure AD B2C pueda comunicarse mediante un punto de conexión. El punto de conexión proporciona un conjunto de notificaciones que Azure AD B2C usa para comprobar que un usuario concreto se ha autenticado.

Puede definir una cuenta de AD FS como proveedor de notificaciones; para ello, agréguela al elemento **ClaimsProvider** en el archivo de extensión de la directiva. Para obtener más información, consulte [Definición de un perfil técnico de proveedor de identidades SAML](saml-identity-provider-technical-profile.md).

1. Abra el archivo *TrustFrameworkExtensions.xml*.
1. Busque el elemento **ClaimsProviders**. Si no existe, agréguelo debajo del elemento raíz.
1. Agregue un nuevo elemento **ClaimsProvider** tal como se muestra a continuación:

    ```xml
    <ClaimsProvider>
      <Domain>contoso.com</Domain>
      <DisplayName>Contoso AD FS</DisplayName>
      <TechnicalProfiles>
        <TechnicalProfile Id="Contoso-SAML2">
          <DisplayName>Contoso AD FS</DisplayName>
          <Description>Login with your AD FS account</Description>
          <Protocol Name="SAML2"/>
          <Metadata>
            <Item Key="WantsEncryptedAssertions">false</Item>
            <Item Key="PartnerEntity">https://your-AD-FS-domain/federationmetadata/2007-06/federationmetadata.xml</Item>
          </Metadata>
          <CryptographicKeys>
            <Key Id="SamlMessageSigning" StorageReferenceId="B2C_1A_SamlCert"/>
          </CryptographicKeys>
          <OutputClaims>
            <OutputClaim ClaimTypeReferenceId="issuerUserId" PartnerClaimType="userPrincipalName" />
            <OutputClaim ClaimTypeReferenceId="givenName" PartnerClaimType="given_name"/>
            <OutputClaim ClaimTypeReferenceId="surname" PartnerClaimType="family_name"/>
            <OutputClaim ClaimTypeReferenceId="email" PartnerClaimType="email"/>
            <OutputClaim ClaimTypeReferenceId="displayName" PartnerClaimType="name"/>
            <OutputClaim ClaimTypeReferenceId="identityProvider" DefaultValue="contoso.com" />
            <OutputClaim ClaimTypeReferenceId="authenticationSource" DefaultValue="socialIdpAuthentication"/>
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

1. Reemplace `your-AD-FS-domain` por el nombre de su dominio de AD FS y reemplace el valor de la notificación de salida **identityProvider** por su DNS (valor arbitrario que indica el dominio).

1. Busque la sección `<ClaimsProviders>` y agregue el siguiente fragmento de código XML. Si la directiva ya contiene el perfil técnico `SM-Saml-idp`, vaya al paso siguiente. Para más información, consulte [Administración de sesión de inicio de sesión único](custom-policy-reference-sso.md).

    ```xml
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

Por el momento, ha configurado la directiva para que Azure AD B2C sepa cómo comunicarse con la cuenta de AD FS. Pruebe a cargar el archivo de extensión de la directiva para confirmar que no tiene problemas.

1. En la página **Directivas personalizadas** del inquilino de Azure AD B2C, seleccione **Cargar directiva**.
2. Habilite **Sobrescribir la directiva, si existe** , y busque y seleccione el archivo *TrustFrameworkExtensions.xml*.
3. Haga clic en **Cargar**.

> [!NOTE]
> La extensión Visual Studio Code B2C usa "socialIdpUserId". También es necesaria una directiva social para AD FS.
>

## <a name="register-the-claims-provider"></a>Registro del proveedor de notificaciones

En este punto, el proveedor de identidades ya se ha configurado, pero no está disponible en ninguna de las pantallas de registro o de inicio de sesión. Para que esté disponible, debe crear un duplicado de un recorrido del usuario de plantilla existente que modificaremos a continuación para que también tenga el proveedor de identidades de AD FS.

1. Abra el archivo *TrustFrameworkBase.xml* del paquete de inicio.
2. Busque y copie todo el contenido del elemento **UserJourney** que incluye `Id="SignUpOrSignIn"`.
3. Abra el archivo *TrustFrameworkExtensions.xml* y busque el elemento **UserJourneys**. Si el elemento no existe, agréguelo.
4. Pegue todo el contenido del elemento **UserJourney** que ha copiado como elemento secundario del elemento **UserJourneys**.
5. Cambie el identificador del recorrido del usuario. Por ejemplo, `SignUpSignInADFS`.

### <a name="display-the-button"></a>Visualización del botón

El elemento **ClaimsProviderSelection** es análogo a un botón de proveedor de identidades en una pantalla de registro o de inicio de sesión. Si agrega un elemento **ClaimsProviderSelection** para una cuenta de AD FS, se muestra un nuevo botón cuando un usuario aterriza en la página.

1. Busque el elemento **OrchestrationStep** que incluye `Order="1"` en el recorrido del usuario que ha creado.
2. En **ClaimsProviderSelections** , agregue el siguiente elemento. Establezca un valor adecuado en **TargetClaimsExchangeId** , por ejemplo, `ContosoExchange`:

    ```xml
    <ClaimsProviderSelection TargetClaimsExchangeId="ContosoExchange" />
    ```

### <a name="link-the-button-to-an-action"></a>Vincular el botón a una acción

Ahora que hay un botón colocado, es preciso vincularlo a una acción. En este caso, la acción es para que Azure AD B2C se comunique con una cuenta de AD FS para recibir un token.

1. Busque el elemento **OrchestrationStep** que incluye `Order="2"` en el recorrido del usuario.
2. Al agregar el siguiente elemento **ClaimsExchange** , asegúrese de usar el mismo valor para el identificador que usó para **TargetClaimsExchangeId** :

    ```xml
    <ClaimsExchange Id="ContosoExchange" TechnicalProfileReferenceId="Contoso-SAML2" />
    ```

    Cambie el valor de **TechnicalProfileReferenceId** para el identificador del perfil técnico que creó anteriormente. Por ejemplo, `Contoso-SAML2`.

3. Guarde el archivo *TrustFrameworkExtensions.xml* y cárguelo de nuevo a fin de verificarlo.


## <a name="configure-an-ad-fs-relying-party-trust"></a>Configuración de una relación de confianza para usuario autenticado de AD FS

Para usar AD FS como proveedor de identidades en Azure AD B2C, debe crear una relación de confianza para usuario autenticado de AD FS con los metadatos de SAML de Azure AD B2C. En el ejemplo siguiente se muestra una dirección URL a los metadatos SAML de un perfil técnico de Azure AD B2C:

```
https://your-tenant-name.b2clogin.com/your-tenant-name.onmicrosoft.com/your-policy/samlp/metadata?idptp=your-technical-profile
```

Reemplace los siguientes valores:

- **your-tenant** por el nombre del inquilino, como inquilino.onmicrosoft.com.
- **your-policy** por el nombre de la directiva. Por ejemplo, B2C_1A_signup_signin_adfs.
- **your-technical-profile** con el nombre de su perfil técnico del proveedor de identidades de SAML. Por ejemplo, Contoso-SAML2.

Abra un explorador y vaya a esta dirección URL. Asegúrese de escribir la dirección URL correcta y que tenga acceso al archivo XML de metadatos. Para agregar una nueva relación de confianza para usuario autenticado mediante el complemento de administración de AD FS (Servicios de federación de Active Directory) y configurar manualmente los valores, realice el procedimiento siguiente en un servidor de federación. Para realizar este procedimiento, se requiere al menos la pertenencia al grupo **Administradores** , o equivalente, en el equipo local.

1. En el Administrador del servidor, seleccione **Herramientas** y luego **Administración de AD FS**.
2. Seleccione **Agregar confianza para usuario de confianza**.
3. En la **página principal** , elija **Claims aware** (Compatible con notificaciones) y haga clic en **Iniciar**.
4. En la página **Seleccionar origen de datos** , seleccione **Import data about the relying party publish online or on a local network** (Importar datos sobre el usuario de confianza para publicar en línea o en una red local), proporcione la dirección URL de metadatos de Azure AD B2C y luego haga clic en **Siguiente**.
5. En la página **Especificar nombre para mostrar** , escriba un nombre en **Nombre para mostrar**. En **Notas** escriba una descripción de esta relación de confianza de usuario y luego haga clic en **Siguiente**.
6. En **Choose Access Control Policy** (Elegir directiva de control de acceso), seleccione una directiva y haga clic en **Siguiente**.
7. En la página **Ready to Add Trust** (Listo para agregar confianza), revise la configuración y luego haga clic en **Siguiente** para guardar la información de la relación de confianza para usuario autenticado.
8. En la página **Finalizar** , haga clic en **Cerrar** ; esta acción muestra automáticamente el cuadro de diálogo **Edit Claim Rules** (Editar reglas de notificación).
9. Seleccione **Agregar regla**.
10. En **Claim rule template** (Plantilla de regla de notificación), seleccione **Send LDAP attributes as claims** (Enviar atributos LDAP como notificaciones).
11. Proporcione un valor en **Claim rule name** (Nombre de la regla de notificación). Para **Attribute store** (Almacén de atributos), seleccione **Select Active Directory** (Seleccionar Active Directory), agregue las siguientes notificaciones y haga clic en **Finalizar** y luego en **Aceptar**.

    | Atributo LDAP | Tipo de notificación saliente |
    | -------------- | ------------------- |
    | User-Principal-Name | userPrincipalName |
    | Surname | family_name |
    | Given-Name | given_name |
    | E-Mail-Address | email |
    | Display-Name | name |

    Tenga en cuenta que estos nombres no se mostrarán en la lista desplegable de tipo de notificaciones salientes. Debe escribirlos manualmente. (La lista desplegable es realmente editable).

12.  Según el tipo de certificado, es posible que tenga que establecer el algoritmo HASH. En la ventana de propiedades de la relación de usuario de confianza (demo de B2C), haga clic en la pestaña **Avanzado** y cambie el valor de **Algoritmo hash seguro** a `SHA-256` y luego haga clic en **Aceptar**.
13. En el Administrador del servidor, seleccione **Herramientas** y luego **Administración de AD FS**.
14. Seleccione la relación de usuario de confianza que creó, seleccione **Actualizar a partir de los metadatos de federación** y luego haga clic en **Actualizar**.

## <a name="create-an-azure-ad-b2c-application"></a>Creación de una aplicación de Azure AD B2C

La comunicación con Azure AD B2C se produce mediante una aplicación que se registra en el inquilino B2C. En esta sección se enumeran los pasos opcionales que puede llevar a cabo para crear una aplicación de prueba, si aún no lo ha hecho.

[!INCLUDE [active-directory-b2c-appreg-idp](../../includes/active-directory-b2c-appreg-idp.md)]

### <a name="update-and-test-the-relying-party-file"></a>Actualización y prueba del archivo del usuario de confianza

Actualice el archivo de usuario de confianza (RP) que inicia el recorrido del usuario que ha creado.

1. Realice una copia del archivo *SignUpOrSignIn.xml* en el directorio de trabajo y cámbiele el nombre. Por ejemplo, cambie su nombre a *SignUpSignInADFS.xml*.
2. Abra el nuevo archivo y actualice el valor del atributo **PolicyId** del elemento **TrustFrameworkPolicy** con un valor único. Por ejemplo, `SignUpSignInADFS`.
3. Actualice el valor de **PublicPolicyUri** con el URI para la directiva. Por ejemplo: `http://contoso.com/B2C_1A_signup_signin_adfs`
4. Cambie el valor del atributo **ReferenceId** del elemento **DefaultUserJourney** para que coincida con el identificador del nuevo recorrido del usuario que ha creado (SignUpSignInADFS).
5. Guarde los cambios, cargue el archivo y seleccione la nueva directiva en la lista.
6. Asegúrese de que la aplicación de Azure AD B2C que creó está seleccionada en el campo **Seleccionar aplicación** y pruébela; para ello, haga clic en **Ejecutar ahora**.

## <a name="troubleshooting-ad-fs-service"></a>Solución de problemas del servicio de AD FS  

AD FS se configura para utilizar el registro de aplicaciones de Windows. Si experimenta problemas al configurar AD FS como proveedor de identidades de SAML mediante directivas personalizadas en Azure AD B2C, puede que quiera comprobar el registro de eventos de AD FS:

1. En la **barra de búsqueda** de Windows, escriba **Visor de eventos** y, después, seleccione la aplicación de escritorio **Visor de eventos**.
1. Para ver el registro de un equipo diferente, haga clic con el botón derecho en **Visor de eventos (local)** . Seleccione **Conectarse a otro equipo** y rellene los campos para completar el cuadro de diálogo **Seleccionar equipo**.
1. En **Visor de eventos** , abra **Registros de aplicaciones y servicios**.
1. Seleccione **AD FS** y, a continuación, seleccione **Administración**. 
1. Para ver más información acerca de un evento, haga doble clic en el suceso.  

### <a name="saml-request-is-not-signed-with-expected-signature-algorithm-event"></a>La solicitud SAML no está firmada con el evento de algoritmo de firma esperado

Este error indica que la solicitud SAML enviada por Azure AD B2C no está firmada con el algoritmo de firma esperado configurado en AD FS. Por ejemplo, la solicitud SAML está firmada con el algoritmo de firma `rsa-sha256`, pero el algoritmo de firma esperado es `rsa-sha1`. Para corregir este problema, asegúrese de que tanto Azure AD B2C como AD FS estén configurados con el mismo algoritmo de firma.

#### <a name="option-1-set-the-signature-algorithm-in-azure-ad-b2c"></a>Opción 1: Establecer el algoritmo de firma en Azure AD B2C  

Puede configurar cómo firmar la solicitud SAML en Azure AD B2C. Los metadatos de [XmlSignatureAlgorithm](saml-identity-provider-technical-profile.md#metadata) controlan el valor del parámetro `SigAlg` (cadena de consulta o parámetro de publicación) en la solicitud SAML. En el ejemplo siguiente se configura Azure AD B2C para usar el algoritmo de firma `rsa-sha256`.

```xml
<Metadata>
  <Item Key="WantsEncryptedAssertions">false</Item>
  <Item Key="PartnerEntity">https://your-AD-FS-domain/federationmetadata/2007-06/federationmetadata.xml</Item>
  <Item Key="XmlSignatureAlgorithm">Sha256</Item>
</Metadata>
```

#### <a name="option-2-set-the-signature-algorithm-in-ad-fs"></a>Opción 2: Establecer el algoritmo de signatura en AD FS 

Como alternativa, puede configurar el algoritmo de firma esperado de la solicitud SAML en AD FS.

1. En el Administrador del servidor, seleccione **Herramientas** y luego **Administración de AD FS**.
1. Seleccione la **Relación de confianza para usuario autenticado** que creó anteriormente.
1. Seleccione **Propiedades** y luego **Avanzado**.
1. Configure el **Algoritmo hash seguro** y seleccione **Aceptar** para guardar los cambios.


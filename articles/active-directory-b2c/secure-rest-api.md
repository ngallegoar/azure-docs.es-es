---
title: Protección de un servicio RESTful en Azure AD B2C
titleSuffix: Azure AD B2C
description: Proteja los intercambios de notificaciones de la API REST en Azure AD B2C.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: how-to
ms.date: 10/15/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: adb9bf48800062d2cc6976a88ec48c1993858dec
ms.sourcegitcommit: 30505c01d43ef71dac08138a960903c2b53f2499
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/15/2020
ms.locfileid: "92089543"
---
# <a name="secure-your-restful-services"></a>Protección de los servicios RESTful 

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Al integrar una API REST en un recorrido del usuario de Azure AD B2C, debe proteger el punto de conexión de la API REST con autenticación. Esto garantiza que solo los servicios que tienen credenciales adecuadas, como Azure AD B2C, pueden realizar llamadas al punto de conexión de la API REST.

Aprenda a integrar una API REST en el recorrido del usuario de Azure AD B2C en los artículos [Validación de entradas del usuario](custom-policy-rest-api-claims-validation.md) y [Adición de intercambios de notificaciones de API de REST a directivas personalizadas](custom-policy-rest-api-claims-exchange.md).

En este artículo se explica cómo proteger la API REST con HTTP básico, Basic, un certificado de cliente o autenticación OAuth2. 

## <a name="prerequisites"></a>Prerrequisitos

Complete los pasos de una de las siguientes guías de procedimientos:

- [Integración de intercambios de notificaciones de API REST en el recorrido del usuario de Azure AD B2C para validar la entrada del usuario](custom-policy-rest-api-claims-validation.md).
- [Adición de intercambios de notificaciones de API de REST a directivas personalizadas](custom-policy-rest-api-claims-exchange.md)

## <a name="http-basic-authentication"></a>Autenticación HTTP básica

La autenticación HTTP básica se define en [RFC 2617](https://tools.ietf.org/html/rfc2617). La autenticación básica funciona de la manera siguiente: Azure AD B2C envía una solicitud HTTP con las credenciales del cliente en el encabezado de autorización. Las credenciales tienen el formato de cadena codificada en Base64 "nombre:contraseña".  

### <a name="add-rest-api-username-and-password-policy-keys"></a>Agregar claves de directiva de nombre de usuario y contraseña de la API REST

Para configurar un perfil técnico de la API REST con autenticación HTTP básica, cree las siguientes claves criptográficas para almacenar el nombre de usuario y la contraseña:

1. Inicie sesión en [Azure Portal](https://portal.azure.com/).
1. Asegúrese de que usa el directorio que contiene el inquilino de Azure AD B2C. Seleccione el filtro **Directorio + suscripción** en el menú superior y elija el directorio de Azure AD B2C.
1. Elija **Todos los servicios** en la esquina superior izquierda de Azure Portal, y busque y seleccione **Azure AD B2C** .
1. En la página de introducción, seleccione **Identity Experience Framework** .
1. Seleccione **Claves de directiva** y, luego, **Agregar** .
1. En **Opciones** , seleccione **Manual** .
1. En **Nombre** , escriba **RestApiUsername** .
    Es posible que se agregue automáticamente el prefijo *B2C_1A_* .
1. En el cuadro **Secreto** , escriba el nombre de usuario de la API REST.
1. En **Uso de la clave** , seleccione **Cifrado** .
1. Seleccione **Crear** .
1. Vuelva a seleccionar **Claves de directiva** .
1. Seleccione **Agregar** .
1. En **Opciones** , seleccione **Manual** .
1. En **Nombre** , escriba **RestApiPassword** .
    Es posible que se agregue automáticamente el prefijo *B2C_1A_* .
1. En el cuadro **Secreto** , escriba la contraseña de la API REST.
1. En **Uso de la clave** , seleccione **Cifrado** .
1. Seleccione **Crear** .

### <a name="configure-your-rest-api-technical-profile-to-use-http-basic-authentication"></a>Configuración del perfil técnico de la API REST para usar la autenticación HTTP básica

Después de crear las claves necesarias, configure los metadatos de perfil técnico de la API REST para que hagan referencia a las credenciales.

1. Abra el archivo de la directiva de extensión (TrustFrameworkExtensions.xml) en el directorio de trabajo.
1. Busque el perfil técnico de la API REST. Por ejemplo, `REST-ValidateProfile` o `REST-GetProfile`.
1. Busque el elemento `<Metadata>`.
1. Cambie el valor de *AuthenticationType* a `Basic`.
1. Cambie el valor de *AllowInsecureAuthInProduction* a `false`.
1. Agregue el siguiente fragmento de código XML inmediatamente después del elemento `</Metadata>` de cierre:
    ```xml
    <CryptographicKeys>
        <Key Id="BasicAuthenticationUsername" StorageReferenceId="B2C_1A_RestApiUsername" />
        <Key Id="BasicAuthenticationPassword" StorageReferenceId="B2C_1A_RestApiPassword" />
    </CryptographicKeys>
    ```

A continuación se incluye un ejemplo de un perfil técnico de RESTful configurado con autenticación HTTP básica:

```xml
<ClaimsProvider>
  <DisplayName>REST APIs</DisplayName>
  <TechnicalProfiles>
    <TechnicalProfile Id="REST-GetProfile">
      <DisplayName>Get user extended profile Azure Function web hook</DisplayName>
      <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.RestfulProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
      <Metadata>
        <Item Key="ServiceUrl">https://your-account.azurewebsites.net/api/GetProfile?code=your-code</Item>
        <Item Key="SendClaimsIn">Body</Item>
        <Item Key="AuthenticationType">Basic</Item>
        <Item Key="AllowInsecureAuthInProduction">false</Item>
      </Metadata>
      <CryptographicKeys>
        <Key Id="BasicAuthenticationUsername" StorageReferenceId="B2C_1A_RestApiUsername" />
        <Key Id="BasicAuthenticationPassword" StorageReferenceId="B2C_1A_RestApiPassword" />
      </CryptographicKeys>
      ...
    </TechnicalProfile>
  </TechnicalProfiles>
</ClaimsProvider>
```

## <a name="https-client-certificate-authentication"></a>Autenticación con certificados de cliente HTTPS

La autenticación con certificados de cliente es una autenticación mutua basada en certificados donde el cliente, Azure AD B2C, proporciona su certificado de cliente al servidor para demostrar su identidad. Esto sucede como parte del protocolo de enlace SSL. Solo pueden acceder al servicio de la API REST los servicios que tienen certificados adecuados, como Azure AD B2C. El certificado de cliente es un certificado digital X.509. En entornos de producción, debe estar firmado por una entidad de certificación.

### <a name="prepare-a-self-signed-certificate-optional"></a>Preparación de un certificado autofirmado (opcional)

Para los entornos que no sean de producción, si aún no tiene ningún certificado, puede usar un certificado autofirmado. En Windows, puede usar el cmdlet [New-SelfSignedCertificate](https://docs.microsoft.com/powershell/module/pkiclient/new-selfsignedcertificate) de PowerShell para generar un certificado.

1. Ejecute este comando de PowerShell para generar un certificado autofirmado. Modifique el argumento `-Subject` según corresponda para su aplicación y el nombre del inquilino de Azure AD B2C. También puede ajustar la fecha de `-NotAfter` para especificar una expiración diferente para el certificado.
    ```powershell
    New-SelfSignedCertificate `
        -KeyExportPolicy Exportable `
        -Subject "CN=yourappname.yourtenant.onmicrosoft.com" `
        -KeyAlgorithm RSA `
        -KeyLength 2048 `
        -KeyUsage DigitalSignature `
        -NotAfter (Get-Date).AddMonths(12) `
        -CertStoreLocation "Cert:\CurrentUser\My"
    ```    
1. Abra **Administrar certificados de usuario** > **Usuario actual** > **Personal** > **Certificados** > *yourappname.yourtenant.onmicrosoft.com* .
1. Seleccione el certificado > **Acción** > **Todas las tareas** > **Exportar** .
1. Seleccione **Sí** > **Siguiente** > **Sí, exportar la clave privada** > **Siguiente** .
1. Acepte los valores predeterminados para **Formato de archivo de exportación** .
1. Proporcionar una contraseña para el certificado.

### <a name="add-a-client-certificate-policy-key"></a>Adición de una clave de directiva de certificado de cliente

1. Inicie sesión en [Azure Portal](https://portal.azure.com/).
1. Asegúrese de que usa el directorio que contiene el inquilino de Azure AD B2C. Seleccione el filtro **Directorio + suscripción** en el menú superior y elija el directorio de Azure AD B2C.
1. Elija **Todos los servicios** en la esquina superior izquierda de Azure Portal, y busque y seleccione **Azure AD B2C** .
1. En la página de introducción, seleccione **Identity Experience Framework** .
1. Seleccione **Claves de directiva** y, luego, **Agregar** .
1. En el cuadro **Opciones** , seleccione **Cargar** .
1. En el cuadro **Nombre** , escriba **RestApiClientCertificate** .
    El prefijo *B2C_1A_* se agrega automáticamente.
1. En el cuadro **Carga de archivos** , seleccione el archivo .pfx del certificado con una clave privada.
1. En el cuadro **Contraseña** , escriba la contraseña del certificado.
1. Seleccione **Crear** .

### <a name="configure-your-rest-api-technical-profile-to-use-client-certificate-authentication"></a>Configuración del perfil técnico de la API REST para usar la autenticación de certificado de cliente

Después de crear la clave necesaria, configure los metadatos de perfil técnico de la API REST para que hagan referencia al certificado de cliente.

1. Abra el archivo de la directiva de extensión (TrustFrameworkExtensions.xml) en el directorio de trabajo.
1. Busque el perfil técnico de la API REST. Por ejemplo, `REST-ValidateProfile` o `REST-GetProfile`.
1. Busque el elemento `<Metadata>`.
1. Cambie el valor de *AuthenticationType* a `ClientCertificate`.
1. Cambie el valor de *AllowInsecureAuthInProduction* a `false`.
1. Agregue el siguiente fragmento de código XML inmediatamente después del elemento `</Metadata>` de cierre:
    ```xml
    <CryptographicKeys>
       <Key Id="ClientCertificate" StorageReferenceId="B2C_1A_RestApiClientCertificate" />
    </CryptographicKeys>
    ```

A continuación se incluye un ejemplo de un perfil técnico de RESTful configurado con un certificado de cliente HTTP:

```xml
<ClaimsProvider>
  <DisplayName>REST APIs</DisplayName>
  <TechnicalProfiles>
    <TechnicalProfile Id="REST-GetProfile">
      <DisplayName>Get user extended profile Azure Function web hook</DisplayName>
      <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.RestfulProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
      <Metadata>
        <Item Key="ServiceUrl">https://your-account.azurewebsites.net/api/GetProfile?code=your-code</Item>
        <Item Key="SendClaimsIn">Body</Item>
        <Item Key="AuthenticationType">ClientCertificate</Item>
        <Item Key="AllowInsecureAuthInProduction">false</Item>
      </Metadata>
      <CryptographicKeys>
        <Key Id="ClientCertificate" StorageReferenceId="B2C_1A_RestApiClientCertificate" />
      </CryptographicKeys>
      ...
    </TechnicalProfile>
  </TechnicalProfiles>
</ClaimsProvider>
```

## <a name="oauth2-bearer-authentication"></a>Autenticación de portador OAuth2 

[!INCLUDE [b2c-public-preview-feature](../../includes/active-directory-b2c-public-preview.md)]

La autenticación de token de portador se define en el [Marco de autorización de OAuth2.0: Bearer Token Usage (RFC 6750)](https://www.rfc-editor.org/rfc/rfc6750.txt) (Marco de autorización de OAuth2.0: uso del token de portador [RFC 6750]). En la autenticación de token de portador, Azure AD B2C envía una solicitud HTTP con un token en el encabezado de autorización.

```http
Authorization: Bearer <token>
```

Un token de portador es una cadena opaca. Puede ser un token de acceso de JWT o cualquier cadena que la API REST espera que Azure AD B2C envíe en el encabezado de autorización. Azure AD B2C admite los tipos siguientes:

- **Token de portador** . Para poder enviar el token de portador en el perfil técnico de RESTful, la directiva debe adquirir primero el token de portador y usarlo en el perfil técnico de RESTful.  
- **Token de portador estático** . Use este enfoque cuando la API REST emita un token de acceso a largo plazo. Para usar un token de portador estático, cree una clave de directiva y haga una referencia desde el perfil técnico de RESTful a la clave de la directiva. 


## <a name="using-oauth2-bearer"></a>Uso del portador de OAuth2  

En los pasos siguientes se muestra cómo usar las credenciales del cliente para obtener un token de portador y pasarlo al encabezado de autorización de las llamadas API REST.  

### <a name="define-a-claim-to-store-the-bearer-token"></a>Definir una notificación para almacenar el token de portador

Una notificación proporciona un almacenamiento temporal de datos durante la ejecución de una directiva de Azure AD B2C. El [esquema de notificaciones](claimsschema.md) es el lugar en el que se declaran las notificaciones. El token de acceso debe almacenarse en una notificación para usarse más tarde. 

1. Abra el archivo de extensiones de la directiva. Por ejemplo, <em>`SocialAndLocalAccounts/`**`TrustFrameworkExtensions.xml`**</em>.
1. Busque el elemento [BuildingBlocks](buildingblocks.md). Si el elemento no existe, agréguelo.
1. Busque el elemento [ClaimsSchema](claimsschema.md). Si el elemento no existe, agréguelo.
1. Agregue las notificaciones siguientes al elemento **ClaimsSchema** .  

```xml
<ClaimType Id="bearerToken">
  <DisplayName>Bearer token</DisplayName>
  <DataType>string</DataType>
</ClaimType>
<ClaimType Id="grant_type">
  <DisplayName>Grant type</DisplayName>
  <DataType>string</DataType>
</ClaimType>
<ClaimType Id="scope">
  <DisplayName>scope</DisplayName>
  <DataType>string</DataType>
</ClaimType>
```

### <a name="acquiring-an-access-token"></a>Obtención un token de acceso 

Puede obtener un token de acceso de varias maneras: obteniéndolo de un [proveedor de identidades federado](idp-pass-through-custom.md), mediante una llamada a una API REST que devuelve un token de acceso, mediante un [flujo de ROPC](https://docs.microsoft.com/azure/active-directory/develop/v2-oauth-ropc) o mediante el [flujo de credenciales del cliente](https://docs.microsoft.com/azure/active-directory/develop/v2-oauth2-client-creds-grant-flow).  

En el ejemplo siguiente se usa un perfil técnico de API REST para realizar una solicitud al punto de conexión del token de Azure AD con las credenciales de cliente pasadas como autenticación HTTP básica. Para configurarlo en Azure AD, consulte [La Plataforma de identidad de Microsoft y el flujo de credenciales de cliente de OAuth 2.0](https://docs.microsoft.com/azure/active-directory/develop/v2-oauth2-client-creds-grant-flow). Es posible que tenga que modificarlo para interactuar con el proveedor de identidades. 

Para ServiceUrl, reemplace el nombre del inquilino con el nombre del inquilino de Azure AD. Consulte las opciones disponibles en la referencia del [perfil técnico de RESTful](restful-technical-profile.md).

```xml
<TechnicalProfile Id="SecureREST-AccessToken">
  <DisplayName></DisplayName>
  <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.RestfulProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
  <Metadata>
    <Item Key="ServiceUrl">https://login.microsoftonline.com/your-tenant-name.onmicrosoft.com/oauth2/v2.0/token</Item>
    <Item Key="AuthenticationType">Basic</Item>
     <Item Key="SendClaimsIn">Form</Item>
  </Metadata>
  <CryptographicKeys>
    <Key Id="BasicAuthenticationUsername" StorageReferenceId="B2C_1A_SecureRESTClientId" />
    <Key Id="BasicAuthenticationPassword" StorageReferenceId="B2C_1A_SecureRESTClientSecret" />
  </CryptographicKeys>
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="grant_type" DefaultValue="client_credentials" />
    <InputClaim ClaimTypeReferenceId="scope" DefaultValue="https://secureb2cfunction.azurewebsites.net/.default" />
  </InputClaims>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="bearerToken" PartnerClaimType="access_token" />
  </OutputClaims>
  <UseTechnicalProfileForSessionManagement ReferenceId="SM-Noop" />
</TechnicalProfile>
```

### <a name="change-the-rest-technical-profile-to-use-bearer-token-authentication"></a>Cambio del perfil técnico de REST para usar la autenticación del token de portador

Para admitir la autenticación de tokens de portador en la directiva personalizada, modifique el perfil técnico de la API REST con lo siguiente:

1. Abra el archivo de directiva de extensión *TrustFrameworkExtensions.xml* desde el directorio de trabajo.
1. Busque el nodo `<TechnicalProfile>` que incluya `Id="REST-API-SignUp"`.
1. Busque el elemento `<Metadata>`.
1. Cambie el valor de *AuthenticationType* a *Bearer* , como se muestra a continuación:
    ```xml
    <Item Key="AuthenticationType">Bearer</Item>
    ```
1. Cambie o agregue *UseClaimAsBearerToken* a *bearerToken* como se indica a continuación. *bearerToken* es el nombre de la notificación de la que se recuperará el token de portador (notificación de salida de `SecureREST-AccessToken`).

    ```xml
    <Item Key="UseClaimAsBearerToken">bearerToken</Item>
    ```
    
1. Asegúrese de agregar la notificación usada anteriormente como notificación de entrada:

    ```xml
    <InputClaim ClaimTypeReferenceId="bearerToken"/>
    ```    

Tras agregar los fragmentos de código anteriores, el perfil técnico debe ser similar al siguiente código XML:

```xml
<ClaimsProvider>
  <DisplayName>REST APIs</DisplayName>
  <TechnicalProfiles>
    <TechnicalProfile Id="REST-GetProfile">
      <DisplayName>Get user extended profile Azure Function web hook</DisplayName>
      <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.RestfulProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
      <Metadata>
        <Item Key="ServiceUrl">https://your-account.azurewebsites.net/api/GetProfile?code=your-code</Item>
        <Item Key="SendClaimsIn">Body</Item>
        <Item Key="AuthenticationType">Bearer</Item>
        <Item Key="UseClaimAsBearerToken">bearerToken</Item>
        <Item Key="AllowInsecureAuthInProduction">false</Item>
      </Metadata>
      <InputClaims>
        <InputClaim ClaimTypeReferenceId="bearerToken"/>
      </InputClaims>
      ...
    </TechnicalProfile>
  </TechnicalProfiles>
</ClaimsProvider>
```

## <a name="using-a-static-oauth2-bearer"></a>Uso de un portador OAuth2 estático 

### <a name="add-the-oauth2-bearer-token-policy-key"></a>Adición de la clave de la directiva de token de portador de OAuth2

Para configurar un perfil técnico de la API REST con un token de portador de OAuth2, obtenga un token de acceso del propietario de la API REST. A continuación, cree la siguiente clave criptográfica para almacenar el token de portador.

1. Inicie sesión en [Azure Portal](https://portal.azure.com/).
1. Asegúrese de que usa el directorio que contiene el inquilino de Azure AD B2C. Seleccione el filtro **Directorio + suscripción** en el menú superior y elija el directorio de Azure AD B2C.
1. Elija **Todos los servicios** en la esquina superior izquierda de Azure Portal, y busque y seleccione **Azure AD B2C** .
1. En la página de introducción, seleccione **Identity Experience Framework** .
1. Seleccione **Claves de directiva** y, luego, **Agregar** .
1. En **Opciones** , elija `Manual`.
1. Escriba un **nombre** para la clave de directiva. Por ejemplo, `RestApiBearerToken`. Se agregará el prefijo `B2C_1A_` automáticamente al nombre de la clave.
1. En **Secreto** , escriba el secreto de cliente que haya registrado previamente.
1. En **Uso de claves** , seleccione `Encryption`.
1. Seleccione **Crear** .

### <a name="configure-your-rest-api-technical-profile-to-use-the-bearer-token-policy-key"></a>Configuración del perfil técnico de la API REST para usar la clave de directiva de token de portador

Después de crear la clave necesaria, configure los metadatos de perfil técnico de la API REST para que hagan referencia al token de portador.

1. Abra el archivo de la directiva de extensión (TrustFrameworkExtensions.xml) en el directorio de trabajo.
1. Busque el perfil técnico de la API REST. Por ejemplo, `REST-ValidateProfile` o `REST-GetProfile`.
1. Busque el elemento `<Metadata>`.
1. Cambie el valor de *AuthenticationType* a `Bearer`.
1. Cambie el valor de *AllowInsecureAuthInProduction* a `false`.
1. Agregue el siguiente fragmento de código XML inmediatamente después del elemento `</Metadata>` de cierre:
    ```xml
    <CryptographicKeys>
       <Key Id="BearerAuthenticationToken" StorageReferenceId="B2C_1A_RestApiBearerToken" />
    </CryptographicKeys>
    ```

A continuación se incluye un ejemplo de un perfil técnico de RESTful configurado con autenticación de token de portador:

```xml
<ClaimsProvider>
  <DisplayName>REST APIs</DisplayName>
  <TechnicalProfiles>
    <TechnicalProfile Id="REST-GetProfile">
      <DisplayName>Get user extended profile Azure Function web hook</DisplayName>
      <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.RestfulProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
      <Metadata>
        <Item Key="ServiceUrl">https://your-account.azurewebsites.net/api/GetProfile?code=your-code</Item>
        <Item Key="SendClaimsIn">Body</Item>
        <Item Key="AuthenticationType">Bearer</Item>
        <Item Key="AllowInsecureAuthInProduction">false</Item>
      </Metadata>
      <CryptographicKeys>
        <Key Id="BearerAuthenticationToken" StorageReferenceId="B2C_1A_RestApiBearerToken" />
      </CryptographicKeys>
      ...
    </TechnicalProfile>
  </TechnicalProfiles>
</ClaimsProvider>
```

## <a name="next-steps"></a>Pasos siguientes

- Obtenga más información sobre el elemento de [perfil técnico de Restful](restful-technical-profile.md) en la referencia de IEF. 

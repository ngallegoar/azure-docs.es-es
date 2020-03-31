---
title: Azure AD B2C (MSAL.NET) | Azure
titleSuffix: Microsoft identity platform
description: Aprenda las consideraciones específicas de uso de Azure AD B2C con la biblioteca de autenticación de Microsoft para .NET (MSAL.NET).
services: active-directory
author: mmacy
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 10/29/2019
ms.author: jeferrie
ms.reviewer: saeeda
ms.custom: aaddev
ms.openlocfilehash: 697b4bc8e3a25085ac6f7d600ea2227dd30a6624
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "79230652"
---
# <a name="use-msalnet-to-sign-in-users-with-social-identities"></a>Use MSAL.NET para que los usuarios inicien sesión con identidades de redes sociales

Puede usar MSAL.NET para que los usuarios inicien sesión con identidades de redes sociales con [Azure Active Directory B2C (Azure AD B2C)](https://aka.ms/aadb2c). Azure AD B2C se basa en la noción de directivas. En MSAL.NET, especificar una directiva se traduce en proporcionar una autoridad.

- Al crear una instancia de la aplicación cliente pública, debe especificar la directiva en la autoridad.
- Cuando desee aplicar una directiva, debe llamar a una invalidación de `AcquireTokenInteractive` que contenga un parámetro `authority`.

Esta página es para MSAL 3.x. Si le interesa MSAL 2.x, consulte [AAD B2C Specifics MSAL 2.x](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/wiki/AAD-B2C-Specifics-MSAL-2.x) (Características de AAD B2C para MSAL 2.x).

## <a name="authority-for-a-azure-ad-b2c-tenant-and-policy"></a>Autoridad para la directiva y el inquilino de Azure AD B2C

La autoridad que se utilizará será `https://{azureADB2CHostname}/tfp/{tenant}/{policyName}`, donde:

- `azureADB2CHostname` es el nombre del inquilino de Azure AD B2C más el host (por ejemplo `{your-tenant-name}.b2clogin.com`).
- `tenant` es el nombre completo del inquilino de Azure AD B2C (por ejemplo, `{your-tenant-name}.onmicrosoft.com`) o el GUID del inquilino. 
- `policyName` es el nombre de la directiva que se aplicará (por ejemplo, "b2c_1_susi", para el registro y el inicio de sesión).

Para obtener más información sobre las autoridades de Azure AD B2C, consulte esta [documentación](/azure/active-directory-b2c/b2clogin).

## <a name="instantiating-the-application"></a>Creación de instancias de la aplicación

Al compilar la aplicación, deberá proporcionar la autoridad.

```csharp
// Azure AD B2C Coordinates
public static string Tenant = "fabrikamb2c.onmicrosoft.com";
public static string AzureADB2CHostname = "fabrikamb2c.b2clogin.com";
public static string ClientID = "90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6";
public static string PolicySignUpSignIn = "b2c_1_susi";
public static string PolicyEditProfile = "b2c_1_edit_profile";
public static string PolicyResetPassword = "b2c_1_reset";

public static string AuthorityBase = $"https://{AzureADB2CHostname}/tfp/{Tenant}/";
public static string Authority = $"{AuthorityBase}{PolicySignUpSignIn}";
public static string AuthorityEditProfile = $"{AuthorityBase}{PolicyEditProfile}";
public static string AuthorityPasswordReset = $"{AuthorityBase}{PolicyResetPassword}";

application = PublicClientApplicationBuilder.Create(ClientID)
               .WithB2CAuthority(Authority)
               .Build();
```

## <a name="acquire-a-token-to-apply-a-policy"></a>Adquisición de un token para aplicar la directiva

Adquirir un token para una API protegida de Azure AD B2C en una aplicación cliente pública requiere usar las invalidaciones con una autoridad:

```csharp
IEnumerable<IAccount> accounts = await application.GetAccountsAsync();
AuthenticationResult ar = await application .AcquireTokenInteractive(scopes)
                                            .WithAccount(GetAccountByPolicy(accounts, policy))
                                            .WithParentActivityOrWindow(ParentActivityOrWindow)
                                            .ExecuteAsync();
```

por:

- `policy` como una de las cadenas anteriores (por ejemplo, `PolicySignUpSignIn`).
- `ParentActivityOrWindow` es necesario para Android (la actividad) y es opcional para otras plataformas que admiten la interfaz de usuario principal, como windows en Windows y UIViewController en iOS. Consulte más información [aquí en el diálogo de la interfaz de usuario](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/wiki/Acquiring-tokens-interactively#withparentactivityorwindow).
- `GetAccountByPolicy(IEnumerable<IAccount>, string)` es un método que encuentra una cuenta para una directiva determinada. Por ejemplo:

  ```csharp
  private IAccount GetAccountByPolicy(IEnumerable<IAccount> accounts, string policy)
  {
   foreach (var account in accounts)
   {
    string userIdentifier = account.HomeAccountId.ObjectId.Split('.')[0];
    if (userIdentifier.EndsWith(policy.ToLower()))
     return account;
   }
   return null;
  }
  ```

Aplicar una directiva o flujo de usuario (por ejemplo, permitir que el usuario final edite su perfil o restablezca su contraseña) se realiza actualmente mediante una llamada a `AcquireTokenInteractive`. En el caso de estas dos directivas, no use el token devuelto ni el resultado de la autenticación.

## <a name="special-case-of-editprofile-and-resetpassword-policies"></a>Las directivas EditProfile y ResetPassword son un caso especial

Cuando desee proporcionar una experiencia donde los usuarios finales inicien sesión con una identidad de redes sociales y editen sus perfiles, desea aplicar la directiva EditProfile de Azure AD B2C. La manera de hacer esto es mediante una llamada a `AcquireTokenInteractive` con la autoridad para esa directiva y una petición establecida en `Prompt.NoPrompt` para evitar que aparezca el cuadro de diálogo de selección de cuenta (ya que el usuario ya había iniciado sesión y tiene una sesión cookie activa).

```csharp
private async void EditProfileButton_Click(object sender, RoutedEventArgs e)
{
 IEnumerable<IAccount> accounts = await app.GetAccountsAsync();
 try
 {
  var authResult = await app.AcquireToken(scopes:App.ApiScopes)
                               .WithAccount(GetUserByPolicy(accounts, App.PolicyEditProfile)),
                               .WithPrompt(Prompt.NoPrompt),
                               .WithB2CAuthority(App.AuthorityEditProfile)
                               .ExecuteAsync();
  DisplayBasicTokenInfo(authResult);
 }
 catch
 {
  . . .
 }
}
```
## <a name="resource-owner-password-credentials-ropc-with-azure-ad-b2c"></a>Credenciales de contraseña de propietario del recurso (ROPC) con Azure AD B2C
Para más detalles sobre el flujo ROPC, consulte esta [documentación](v2-oauth-ropc.md).

Este flujo **no se recomienda** porque no es seguro que la aplicación le pida al usuario la contraseña. Para más información sobre este problema, consulte [este artículo](https://news.microsoft.com/features/whats-solution-growing-problem-passwords-says-microsoft/). 

Al usar el nombre de usuario y contraseña, se está descuidando una serie de cosas:
- Principios de identidad moderna: la contraseña se busca y se reproduce. Dado que tenemos este concepto de secreto compartido que se puede interceptar, es incompatible con el inicio de sesión sin contraseña.
- Los usuarios que necesitan MFA no podrán iniciar sesión (ya que no hay ninguna interacción).
- Los usuarios no podrán realizar el inicio de sesión único.

### <a name="configure-the-ropc-flow-in-azure-ad-b2c"></a>Configuración del flujo ROPC en Azure AD B2C
En el inquilino de Azure AD B2C, cree un nuevo flujo de usuario y seleccione **Iniciar sesión mediante ROPC**. Esto habilitará la directiva ROPC para el inquilino. Consulte [Configuración del flujo de credenciales de contraseña de propietario del recurso en Azure AD B2C](/azure/active-directory-b2c/configure-ropc) para más detalles.

`IPublicClientApplication` contiene un método denominado
```csharp
AcquireTokenByUsernamePassword(
            IEnumerable<string> scopes,
            string username,
            SecureString password)
```

Este método toma como parámetros:
- Los *ámbitos* para los que se solicita un token de acceso.
- Un *nombre de usuario*.
- Una *contraseña* de Securestring para el usuario.

Recuerde que debe usar la autoridad que contiene la directiva ROPC.

### <a name="limitations-of-the-ropc-flow"></a>Limitaciones del flujo ROPC
 - El flujo ROPC **solo funciona para las cuentas locales** (donde se registra con Azure AD B2C mediante correo electrónico o nombre de usuario). Este flujo no funciona si hay federación a cualquier proveedor de identidades admitido por Azure AD B2C (Facebook, Google, etc.).

## <a name="google-auth-and-embedded-webview"></a>Autenticación de Google y vista web insertada

Si es un desarrollador de Azure AD B2C que usa Google como proveedor de identidades, le recomendamos utilizar el explorador del sistema, ya que Google no permite la [autenticación desde vistas web insertadas](https://developers.googleblog.com/2016/08/modernizing-oauth-interactions-in-native-apps.html). Actualmente, `login.microsoftonline.com` es una autoridad de confianza para Google. Esta autoridad funciona con la vista web insertada. Sin embargo, `b2clogin.com` no es una autoridad de confianza para Google, por lo que los usuarios no podrán autenticarse.

Si las cosas cambian, proporcionamos una actualización de la wiki y esta [incidencia](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/issues/688).

## <a name="caching-with-azure-ad-b2c-in-msalnet"></a>Almacenamiento en caché con Azure AD B2C en MSAL.Net 

### <a name="known-issue-with-azure-ad-b2c"></a>Problema conocido con Azure AD B2C

MSAL.Net admite una [memoria caché de tokens](/dotnet/api/microsoft.identity.client.tokencache?view=azure-dotnet). La clave de la caché de tokens se basa en las notificaciones devueltas por el proveedor de identidades. Actualmente MSAL.Net necesita dos notificaciones para generar una clave de caché de tokens:  
- `tid`, que es el identificador del inquilino de Azure AD, y 
- `preferred_username` 

Ambas notificaciones faltan en muchos de los escenarios de Azure AD B2C. 

¿Es el impacto para el cliente que al intentar mostrar el campo de nombre de usuario obtiene "Missing from the token response" (Falta en la respuesta del token) como valor? Si es así, esto se debe a que Azure AD B2C no devuelve un valor en IdToken para preferred_username debido a limitaciones con las cuentas de redes sociales y los proveedores de identidades externos (IDP). Azure AD devuelve un valor para preferred_username porque sabe quién es el usuario, pero para Azure AD B2C, como el usuario puede iniciar sesión con una cuenta local, Facebook, Google, GitHub, etc., no hay un valor coherente que Azure AD B2C pueda usar para preferred_username. Para desbloquear MSAL de la implementación de compatibilidad de la memoria caché con ADAL, decidimos usar "Missing from token response" (Falta en la respuesta del token) al trabajar con las cuentas de Azure AD B2C cuando IdToken devuelve "nothing" para preferred_username. MSAL debe devolver un valor para preferred_username para mantener la compatibilidad de la memoria caché en las bibliotecas.

### <a name="workarounds"></a>Soluciones alternativas

#### <a name="mitigation-for-the-missing-tenant-id"></a>Mitigación para el identificador del inquilino que falta

La solución recomendada es usar la [directiva de caché](#acquire-a-token-to-apply-a-policy)

Como alternativa, puede usar la notificación `tid` si usa las [directivas personalizadas de B2C](https://aka.ms/ief), ya que proporciona la capacidad de devolver notificaciones adicionales a la aplicación. Más información sobre la [transformación de notificaciones](/azure/active-directory-b2c/claims-transformation-technical-profile).

#### <a name="mitigation-for-missing-from-the-token-response"></a>Mitigación de "Missing from token response" (Falta en la respuesta del token)
Una opción es usar la notificación "name" como nombre de usuario preferido. El proceso se menciona este [documento de B2C](../../active-directory-b2c/user-flow-overview.md) -> "En la columna Notificación de devolución, elija las notificaciones que quiere que se devuelvan en los tokens de autorización enviados de vuelta a su aplicación después de una experiencia de edición de perfiles correcta. Por ejemplo, seleccione Nombre para mostrar y Código postal".

## <a name="next-steps"></a>Pasos siguientes 

En el siguiente ejemplo se ofrecen más detalles sobre la adquisición de tokens de forma interactiva con MSAL.NET para aplicaciones de Azure AD B2C.

| Muestra | Plataforma | Descripción|
|------ | -------- | -----------|
|[active-directory-b2c-xamarin-native](https://github.com/Azure-Samples/active-directory-b2c-xamarin-native) | Xamarin iOS, Xamarin Android, UWP | Una aplicación de Xamarin Forms sencilla que muestra cómo usar MSAL.NET para autenticar a los usuarios mediante Azure AD B2C y acceder a una API web con los tokens resultantes.|

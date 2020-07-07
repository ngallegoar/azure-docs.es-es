---
title: Configuración de aplicaciones de API web protegidas | Azure
titleSuffix: Microsoft identity platform
description: Obtenga información sobre cómo compilar una API web protegida y configurar el código de la aplicación.
services: active-directory
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 05/07/2019
ms.author: jmprieur
ms.custom: aaddev
ms.openlocfilehash: 073eca94ad93c69811b02abe2c8649940a394e8e
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "80882478"
---
# <a name="protected-web-api-code-configuration"></a>API web protegida: Configuración del código

Para configurar el código de la API web protegida, debe comprender lo siguiente:

- Qué define las API como protegidas.
- Cómo configurar un token de portador.
- Cómo validar el token.

## <a name="what-defines-aspnet-and-aspnet-core-apis-as-protected"></a>¿Qué define que las API de ASP.NET o ASP.NET Core sean protegidas?

Al igual que las aplicaciones web, las API web de ASP.NET y ASP.NET Core están protegidas porque sus acciones de controlador llevan como prefijo el atributo **[Authorize]** . Solo se puede llamar a las acciones de controlador si se llama a la API con una identidad autorizada.

Tenga en cuenta las preguntas siguientes:

- Solo una aplicación puede llamar a una API web. ¿Cómo conoce la API web la identidad de la aplicación que le llama?
- Si la aplicación llama a la API en nombre de un usuario, ¿cuál es la identidad del usuario?

## <a name="bearer-token"></a>Token de portador

El token de portador que se establece en el encabezado cuando se llama a la aplicación contiene información sobre la identidad de la aplicación. También contiene información sobre el usuario a menos que la aplicación web acepte llamadas de servicio a servicio desde una aplicación de demonio.

Este es un ejemplo de código de C# que muestra a un cliente que llama a la API después de adquirir un token con la biblioteca de autenticación de Microsoft para .NET (MSAL.NET):

```csharp
var scopes = new[] {$"api://.../access_as_user"};
var result = await app.AcquireToken(scopes)
                      .ExecuteAsync();

httpClient = new HttpClient();
httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", result.AccessToken);

// Call the web API.
HttpResponseMessage response = await _httpClient.GetAsync(apiUri);
```

> [!IMPORTANT]
> Una aplicación cliente solicita el token de portador al punto de conexión de la plataforma de Microsoft Identity *para la API web*. La API web es la única aplicación que debe comprobar el token y ver las notificaciones que contiene. Las aplicaciones cliente nunca deben intentar inspeccionar las notificaciones de los tokens.
>
> En el futuro, la API web podría requerir que el token esté cifrado. Este requisito podría impedir el acceso a las aplicaciones cliente que pueden ver los tokens de acceso.

## <a name="jwtbearer-configuration"></a>Configuración de JwtBearer

En esta sección se describe cómo configurar un token de portador.

### <a name="config-file"></a>Archivo config

```Json
{
  "AzureAd": {
    "Instance": "https://login.microsoftonline.com/",
    "ClientId": "[Client_id-of-web-api-eg-2ec40e65-ba09-4853-bcde-bcb60029e596]",
    /*
      You need specify the TenantId only if you want to accept access tokens from a single tenant
     (line-of-business app).
      Otherwise, you can leave them set to common.
      This can be:
      - A GUID (Tenant ID = Directory ID)
      - 'common' (any organization and personal accounts)
      - 'organizations' (any organization)
      - 'consumers' (Microsoft personal accounts)
    */
    "TenantId": "common"
  },
  "Logging": {
    "LogLevel": {
      "Default": "Warning"
    }
  },
  "AllowedHosts": "*"
}
```

### <a name="code-initialization"></a>Inicialización del código

Cuando se llama a una aplicación en una acción de controlador que contiene un atributo **[Authorize]** , ASP.NET y ASP.NET Core extraen el token de acceso del token de portador del encabezado de autorización. Después, el token se reenvía al middleware JwtBearer, que llama a las extensiones de Microsoft IdentityModel para .NET.

En ASP.NET Core, este middleware se inicializa en el archivo Startup.cs.

```csharp
using Microsoft.AspNetCore.Authentication.JwtBearer;
```

El middleware se agrega a la API web mediante esta instrucción:

```csharp
 services.AddAuthentication(AzureADDefaults.JwtBearerAuthenticationScheme)
         .AddAzureADBearer(options => Configuration.Bind("AzureAd", options));
```

 Actualmente, las plantillas de ASP.NET Core crean API web de Azure Active Directory (Azure AD) a las que se conectan usuarios dentro de su organización o de cualquier organización. No inician la sesión de los usuarios con cuentas personales. Sin embargo, puede agregar el código siguiente al archivo Startup.cs para cambiar las plantillas de modo que usen el punto de conexión de la plataforma de identidad de Microsoft:

```csharp
services.Configure<JwtBearerOptions>(AzureADDefaults.JwtBearerAuthenticationScheme, options =>
{
    // This is a Microsoft identity platform web API.
    options.Authority += "/v2.0";

    // The web API accepts as audiences both the Client ID (options.Audience) and api://{ClientID}.
    options.TokenValidationParameters.ValidAudiences = new []
    {
     options.Audience,
     $"api://{options.Audience}"
    };

    // Instead of using the default validation (validating against a single tenant,
    // as we do in line-of-business apps),
    // we inject our own multitenant validation logic (which even accepts both v1 and v2 tokens).
    options.TokenValidationParameters.IssuerValidator = AadIssuerValidator.GetIssuerValidator(options.Authority).Validate;;
});
```

Este fragmento de código se extrae del tutorial incremental de la API web de ASP.NET Core en [Microsoft.Identity.Web/WebApiServiceCollectionExtensions.cs#L50-L63](https://github.com/Azure-Samples/active-directory-dotnet-native-aspnetcore-v2/blob/154282843da2fc2958fad151e2a11e521e358d42/Microsoft.Identity.Web/WebApiServiceCollectionExtensions.cs#L50-L63). El método **AddProtectedWebApi**, que hace más de lo que el fragmento de código muestra, se llama desde Startup.cs.

## <a name="token-validation"></a>Validación de tokens

En el fragmento de código anterior, el middleware JwtBearer, al igual que el middleware OpenID Connect en Web Apps, valida el token en función del valor de `TokenValidationParameters`. El token se descifra según sea necesario, se extraen las notificaciones y se comprueba la firma. Después, el middleware valida el token mediante la comprobación de estos datos:

- Audience: El token está destinado a la API web.
- Sub: se ha emitido para una aplicación que puede llamar a la API web.
- Emisor: lo ha emitido un servicio de token de seguridad (STS) de confianza.
- Expiry: su duración está dentro del intervalo.
- Signature: no se ha alterado.

También puede haber validaciones especiales. Por ejemplo, es posible validar que las claves de firma (cuando se insertan en un token) sean de confianza y que el token no se esté reproduciendo. Por último, algunos protocolos requieren validaciones específicas.

### <a name="validators"></a>Validadores

Los pasos de validación se capturan en los validadores, que se proporcionan en la biblioteca de código abierto [extensiones de Microsoft IdentityModel para .NET](https://github.com/AzureAD/azure-activedirectory-identitymodel-extensions-for-dotnet). Los validadores se definen en el archivo de origen de biblioteca [Microsoft.IdentityModel.Tokens/Validators.cs](https://github.com/AzureAD/azure-activedirectory-identitymodel-extensions-for-dotnet/blob/master/src/Microsoft.IdentityModel.Tokens/Validators.cs).

En esta tabla se describen los validadores:

| Validador | Descripción |
|---------|---------|
| **ValidateAudience** | Garantiza que el token es para la aplicación que valida el token en su nombre. |
| **ValidateIssuer** | Garantiza que el token lo ha emitido un STS de confianza, lo que significa que se trata de un usuario de confianza. |
| **ValidateIssuerSigningKey** | Garantiza que la aplicación que valida el token confía en la clave que se ha usado para firmar el token. Hay un caso especial en el que la clave se inserta en el token. Este caso no es habitual. |
| **ValidateLifetime** | Garantiza que el token ya sea o siga siendo válido. El validador comprueba que la duración del token esté dentro del intervalo especificado por las notificaciones **notBefore** y **expires**. |
| **ValidateSignature** | Garantiza que el token no se ha alterado. |
| **ValidateTokenReplay** | Garantiza que el token no se ha reproducido. Hay un caso especial para algunos protocolos de un solo uso. |

Los validadores se asocian con las propiedades de la clase **TokenValidationParameters**. Las propiedades se inicializan a partir de la configuración de ASP.NET y ASP.NET Core.

En la mayoría de los casos, no es necesario cambiar los parámetros. Las aplicaciones que no son de inquilino único son excepciones. Estas aplicaciones web aceptan usuarios de cualquier organización o de cuentas de Microsoft personales. En este caso, se deben validar los emisores.

## <a name="token-validation-in-azure-functions"></a>Validación de tokens en Azure Functions

También puede validar los tokens de acceso entrantes en Azure Functions. Puede encontrar ejemplos de este tipo de validación en [Microsoft .NET](https://github.com/Azure-Samples/ms-identity-dotnet-webapi-azurefunctions), [NodeJS](https://github.com/Azure-Samples/ms-identity-nodejs-webapi-azurefunctions) y [Python](https://github.com/Azure-Samples/ms-identity-python-webapi-azurefunctions).

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Comprobación de los ámbitos y roles de aplicación en el código](scenario-protected-web-api-verification-scope-app-roles.md)

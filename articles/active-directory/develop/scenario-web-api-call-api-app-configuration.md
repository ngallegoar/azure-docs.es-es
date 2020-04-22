---
title: Configuración de una API web que llama a otras API web | Azure
titleSuffix: Microsoft identity platform
description: Obtenga información sobre cómo compilar una API web que llama a otras API web (configuración del código de la aplicación)
services: active-directory
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 07/16/2019
ms.author: jmprieur
ms.custom: aaddev
ms.openlocfilehash: 38e319efb100d326d55f6f821e7c903306a7c7d0
ms.sourcegitcommit: a53fe6e9e4a4c153e9ac1a93e9335f8cf762c604
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/09/2020
ms.locfileid: "80991014"
---
# <a name="a-web-api-that-calls-web-apis-code-configuration"></a>API web que llama a API web: Configuración del código

Una vez registrada su API web, puede configurar el código de la aplicación.

El código que usa para configurar su API web para que llame a las API web de bajada se basa en el código usado para proteger una API web. Para más información, consulte [API web protegida: configuración de la aplicación](scenario-protected-web-api-app-configuration.md).

# <a name="aspnet-core"></a>[ASP.NET Core](#tab/aspnetcore)

## <a name="code-subscribed-to-ontokenvalidated"></a>Código suscrito a OnTokenValidated

Además de la configuración de código para cualquier API web protegida, debe suscribirse a la validación del token de portador que se recibe cuando se llama a su API:

```csharp
/// <summary>
/// Protects the web API with the Microsoft identity platform, or Azure Active Directory (Azure AD) developer platform
/// This supposes that the configuration files have a section named "AzureAD"
/// </summary>
/// <param name="services">The service collection to which to add authentication</param>
/// <param name="configuration">Configuration</param>
/// <returns></returns>
public static IServiceCollection AddProtectedApiCallsWebApis(this IServiceCollection services,
                                                             IConfiguration configuration,
                                                             IEnumerable<string> scopes)
{
    services.AddTokenAcquisition();
    services.Configure<JwtBearerOptions>(AzureADDefaults.JwtBearerAuthenticationScheme, options =>
    {
        // When an access token for our own web API is validated, we add it
        // to the MSAL.NET cache so that it can be used from the controllers.
        options.Events = new JwtBearerEvents();

        options.Events.OnTokenValidated = async context =>
        {
            context.Success();

            // Adds the token to the cache and handles the incremental consent
            // and claim challenges
            AddAccountToCacheFromJwt(context, scopes);
            await Task.FromResult(0);
        };
    });
    return services;
}
```

## <a name="on-behalf-of-flow"></a>Flujo con derechos delegados

El método AddAccountToCacheFromJwt() debe:

- Crear una instancia de una aplicación cliente confidencial de biblioteca de autenticación de Microsoft (MSAL).
- Llame al método `AcquireTokenOnBehalf` . Esta llamada intercambia el token de portador que ha adquirido el cliente para la API web por un token de portador para el mismo usuario, pero hace que la API llame a una API de bajada.

### <a name="instantiate-a-confidential-client-application"></a>Creación de una instancia de una aplicación cliente confidencial

Este flujo solo está disponible en el flujo de cliente confidencial, por lo que la API web protegida proporciona las credenciales del cliente (certificado o secreto de cliente) para la [clase ConfidentialClientApplicationBuilder](https://docs.microsoft.com/dotnet/api/microsoft.identity.client.confidentialclientapplicationbuilder) mediante los métodos `WithClientSecret` o `WithCertificate`.

![Lista de métodos IConfidentialClientApplication](https://user-images.githubusercontent.com/13203188/55967244-3d8e1d00-5c7a-11e9-8285-a54b05597ec9.png)

```csharp
IConfidentialClientApplication app;

#if !VariationWithCertificateCredentials
app = ConfidentialClientApplicationBuilder.Create(config.ClientId)
           .WithClientSecret(config.ClientSecret)
           .Build();
#else
// Building the client credentials from a certificate
X509Certificate2 certificate = ReadCertificate(config.CertificateName);
app = ConfidentialClientApplicationBuilder.Create(config.ClientId)
    .WithCertificate(certificate)
    .Build();
#endif
```

Por último, en lugar de demostrar su identidad mediante un secreto de cliente o un certificado, las aplicaciones cliente confidenciales pueden demostrar su identidad mediante aserciones de cliente.
Para obtener más información acerca de este escenario avanzado, consulte [Aserciones de cliente confidenciales](msal-net-client-assertions.md).

### <a name="how-to-call-on-behalf-of"></a>Cómo llamar con derechos delegados

La llamada con derechos delegados (OBO) se realiza llamando al [método AcquireTokenOnBehalf](https://docs.microsoft.com/dotnet/api/microsoft.identity.client.acquiretokenonbehalfofparameterbuilder) en la interfaz `IConfidentialClientApplication`.

La clase `UserAssertion` se compila a partir del token de portador recibido por la API web desde sus propios clientes. Hay [dos constructores](https://docs.microsoft.com/dotnet/api/microsoft.identity.client.clientcredential.-ctor?view=azure-dotnet):
* Uno que toma un token de portador JSON Web Token (JWT)
* y otro que toma cualquier tipo de aserción de usuario, otra clase de token de seguridad, cuyo tipo se especifica posteriormente en un parámetro adicional llamado `assertionType`.

![Propiedades y métodos de UserAssertion](https://user-images.githubusercontent.com/13203188/37082180-afc4b708-21e3-11e8-8af8-a6dcbd2dfba8.png)

En la práctica, el flujo con derechos delegados se usa a menudo para adquirir un token para una API de bajada y almacenarlo en la caché de tokens de usuario MSAL.NET. De esta forma, otras partes de la API web pueden llamar posteriormente en las [invalidaciones](https://docs.microsoft.com/dotnet/api/microsoft.identity.client.clientapplicationbase.acquiretokensilent?view=azure-dotnet) de ``AcquireTokenOnSilent`` para llamar a las API de bajada. Esta llamada tiene el efecto de actualizar los tokens, si es necesario.

```csharp
private void AddAccountToCacheFromJwt(IEnumerable<string> scopes, JwtSecurityToken jwtToken, ClaimsPrincipal principal, HttpContext httpContext)
{
    try
    {
        UserAssertion userAssertion;
        IEnumerable<string> requestedScopes;
        if (jwtToken != null)
        {
            userAssertion = new UserAssertion(jwtToken.RawData, "urn:ietf:params:oauth:grant-type:jwt-bearer");
            requestedScopes = scopes ?? jwtToken.Audiences.Select(a => $"{a}/.default");
        }
        else
        {
            throw new ArgumentOutOfRangeException("tokenValidationContext.SecurityToken should be a JWT Token");
        }

        // Create the application
        var application = BuildConfidentialClientApplication(httpContext, principal);

        // .Result to make sure that the cache is filled in before the controller tries to get access tokens
        var result = application.AcquireTokenOnBehalfOf(requestedScopes.Except(scopesRequestedByMsalNet),
                                                        userAssertion)
                                .ExecuteAsync()
                                .GetAwaiter().GetResult();
     }
     catch (MsalException ex)
     {
         Debug.WriteLine(ex.Message);
         throw;
     }
}
```
# <a name="java"></a>[Java](#tab/java)

El flujo "en nombre de" (OBO) se usa para obtener un token para llamar a la API web de nivel inferior. En este flujo, la API web recibe un token de portador con permisos delegados de usuario de la aplicación cliente y, a continuación, intercambia este token por otro token de acceso para llamar a la API web de nivel inferior.

En el código siguiente se usa el valor `SecurityContextHolder` de Spring Security Framework en la API web para obtener el token de portador validado. A continuación, se usa la biblioteca de MSAL Java para obtener un token para la API de nivel inferior mediante la llamada a `acquireToken` con `OnBehalfOfParameters`. MSAL almacena en caché el token para que las siguientes llamadas a la API puedan usar `acquireTokenSilently` para obtener el token almacenado en caché.

```Java
@Component
class MsalAuthHelper {

    @Value("${security.oauth2.client.authority}")
    private String authority;

    @Value("${security.oauth2.client.client-id}")
    private String clientId;

    @Value("${security.oauth2.client.client-secret}")
    private String secret;

    @Autowired
    CacheManager cacheManager;

    private String getAuthToken(){
        String res = null;
        Authentication authentication = SecurityContextHolder.getContext().getAuthentication();

        if(authentication != null){
            res = ((OAuth2AuthenticationDetails) authentication.getDetails()).getTokenValue();
        }
        return res;
    }

    String getOboToken(String scope) throws MalformedURLException {
        String authToken = getAuthToken();

        ConfidentialClientApplication application =
                ConfidentialClientApplication.builder(clientId, ClientCredentialFactory.create(secret))
                        .authority(authority).build();

        String cacheKey = Hashing.sha256()
                .hashString(authToken, StandardCharsets.UTF_8).toString();

        String cachedTokens = cacheManager.getCache("tokens").get(cacheKey, String.class);
        if(cachedTokens != null){
            application.tokenCache().deserialize(cachedTokens);
        }

        IAuthenticationResult auth;
        SilentParameters silentParameters =
                SilentParameters.builder(Collections.singleton(scope))
                        .build();
        auth = application.acquireTokenSilently(silentParameters).join();

        if (auth == null){
            OnBehalfOfParameters parameters =
                    OnBehalfOfParameters.builder(Collections.singleton(scope),
                            new UserAssertion(authToken))
                            .build();

            auth = application.acquireToken(parameters).join();
        }

        cacheManager.getCache("tokens").put(cacheKey, application.tokenCache().serialize());

        return auth.accessToken();
    }
}
```

# <a name="python"></a>[Python](#tab/python)

El flujo "en nombre de" (OBO) se usa para obtener un token para llamar a la API web de nivel inferior. En este flujo, la API web recibe un token de portador con permisos delegados de usuario de la aplicación cliente y, a continuación, intercambia este token por otro token de acceso para llamar a la API web de nivel inferior.

Una API web de Python deberá usar algún middleware para validar el token de portador recibido del cliente. A continuación, la API web podrá obtener el token de acceso para la API de nivel inferior mediante la biblioteca de MSAL para Python al llamar al método [`acquire_token_on_behalf_of`](https://msal-python.readthedocs.io/en/latest/?badge=latest#msal.ConfidentialClientApplication.acquire_token_on_behalf_of). Para obtener un ejemplo del uso de esta API, consulte [Código de prueba para la biblioteca de autenticación de Microsoft para Python en GitHub](https://github.com/AzureAD/microsoft-authentication-library-for-python/blob/1.2.0/tests/test_e2e.py#L429-L472). Consulte también la explicación del [problema 53](https://github.com/AzureAD/microsoft-authentication-library-for-python/issues/53) en ese mismo repositorio para obtener un enfoque que omita la necesidad de una aplicación de nivel intermedio.

---

También puede ver un ejemplo de implementación del flujo con derechos delegados en [Node.js y Azure Functions](https://github.com/Azure-Samples/ms-identity-nodejs-webapi-onbehalfof-azurefunctions/blob/master/MiddleTierAPI/MyHttpTrigger/index.js#L61).

## <a name="protocol"></a>Protocolo

Para más información acerca del protocolo OBO, consulte [Plataforma de identidad de Microsoft y flujo con derechos delegados de OAuth 2.0](https://docs.microsoft.com/azure/active-directory/develop/v2-oauth2-on-behalf-of-flow).

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [API web que llama a las API web: adquisición de un token para la aplicación](scenario-web-api-call-api-acquire-token.md)

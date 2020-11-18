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
ms.date: 09/26/2020
ms.author: jmprieur
ms.custom: aaddev
ms.openlocfilehash: 4a888c3ad771e4a7edbd7110ba584050fe68e810
ms.sourcegitcommit: 6109f1d9f0acd8e5d1c1775bc9aa7c61ca076c45
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2020
ms.locfileid: "94443796"
---
# <a name="a-web-api-that-calls-web-apis-code-configuration"></a>API web que llama a API web: Configuración del código

Una vez registrada su API web, puede configurar el código de la aplicación.

El código que usa para configurar su API web para que llame a las API web de bajada se basa en el código usado para proteger una API web. Para más información, consulte [API web protegida: configuración de la aplicación](scenario-protected-web-api-app-configuration.md).

# <a name="aspnet-core"></a>[ASP.NET Core](#tab/aspnetcore)

## <a name="microsoftidentityweb"></a>Microsoft.Identity.Web

Microsoft recomienda usar el paquete NuGet [Microsoft.Identity.Web](https://www.nuget.org/packages/Microsoft.Identity.Web) al desarrollar API web de nivel inferior de llamada API protegidas por ASP.NET Core. Consulte [API web protegida: Configuración de código | Microsoft.Identity.Web](scenario-protected-web-api-app-configuration.md#microsoftidentityweb) para ver una presentación rápida de la biblioteca en el contexto de una API web.

## <a name="client-secrets-or-client-certificates"></a>Secretos de cliente o certificados de cliente

Dado que la API web ahora llama a una API web de nivel inferior, debe proporcionar un secreto de cliente o un certificado de cliente en el archivo *appsettings.json*. También puede agregar una sección que especifique:

- La dirección URL de la API web de nivel inferior.
- Los ámbitos necesarios para llamar a la API.

En el ejemplo siguiente, la sección `GraphBeta` especifica esta configuración.

```JSON
{
  "AzureAd": {
    "Instance": "https://login.microsoftonline.com/",
    "ClientId": "[Client_id-of-web-api-eg-2ec40e65-ba09-4853-bcde-bcb60029e596]",
    "TenantId": "common"

   // To call an API
   "ClientSecret": "[Copy the client secret added to the app from the Azure portal]",
   "ClientCertificates": [
  ]
 },
 "GraphBeta": {
    "BaseUrl": "https://graph.microsoft.com/beta",
    "Scopes": "user.read"
    }
}
```

En lugar de un secreto de cliente, puede proporcionar un certificado de cliente. En el fragmento de código siguiente se muestra el uso de un certificado almacenado en Azure Key Vault.

```JSON
{
  "AzureAd": {
    "Instance": "https://login.microsoftonline.com/",
    "ClientId": "[Client_id-of-web-api-eg-2ec40e65-ba09-4853-bcde-bcb60029e596]",
    "TenantId": "common"

   // To call an API
   "ClientCertificates": [
      {
        "SourceType": "KeyVault",
        "KeyVaultUrl": "https://msidentitywebsamples.vault.azure.net",
        "KeyVaultCertificateName": "MicrosoftIdentitySamplesCert"
      }
   ]
  },
  "GraphBeta": {
    "BaseUrl": "https://graph.microsoft.com/beta",
    "Scopes": "user.read"
  }
}
```

Microsoft.Identity.Web proporciona varias maneras de describir certificados, tanto mediante configuración como mediante código. Para obtener más información, consulte el [wiki de Microsoft.Identity.Web: uso de certificados](https://github.com/AzureAD/microsoft-identity-web/wiki/Using-certificates) en GitHub.

## <a name="startupcs"></a>Startup.cs

La API web tendrá que adquirir un token para la API de nivel inferior. Para especificarlo, agregue la línea `.EnableTokenAcquisitionToCallDownstreamApi()` después de `.AddMicrosoftIdentityWebApi(Configuration)`. Esta línea expone el servicio `ITokenAcquisition`, que puede usar en las acciones de controlador o páginas. Sin embargo, como verá en los dos puntos siguientes, puede hacer que sea más sencillo. También deberá elegir una implementación de la caché de tokens, por ejemplo `.AddInMemoryTokenCaches()`, en *Startup.cs*:

```csharp
using Microsoft.Identity.Web;

public class Startup
{
  // ...
  public void ConfigureServices(IServiceCollection services)
  {
  // ...
  services.AddAuthentication(JwtBearerDefaults.AuthenticationScheme)
          .AddMicrosoftIdentityWebApi(Configuration, Configuration.GetSection("AzureAd"))
            .EnableTokenAcquisitionToCallDownstreamApi()
            .AddInMemoryTokenCaches();
   // ...
  }
  // ...
}
```

Si no desea adquirir el token por sí mismo, *Microsoft.Identity.Web* proporciona dos mecanismos para llamar a una API web de nivel inferior desde otra API. La opción que elija dependerá de si desea llamar a Microsoft Graph o a otra API.

### <a name="option-1-call-microsoft-graph"></a>Opción 1: Llamada a Microsoft Graph

Si desea llamar a Microsoft Graph, Microsoft.Identity.Web le permite usar directamente `GraphServiceClient` (expuesto por el SDK de Microsoft Graph) en las acciones de la API. Para exponer Microsoft Graph:

1. Agregue el paquete NuGet [Microsoft.Identity.Web.MicrosoftGraph](https://www.nuget.org/packages/Microsoft.Identity.Web.MicrosoftGraph) al proyecto.
1. Agregue `.AddMicrosoftGraph()` después de `.EnableTokenAcquisitionToCallDownstreamApi()` en el archivo *Startup.cs*. `.AddMicrosoftGraph()` tiene varias invalidaciones. Cuando se utiliza la invalidación que toma una sección de la configuración como parámetro, el código se convierte en:

```csharp
using Microsoft.Identity.Web;

public class Startup
{
  // ...
  public void ConfigureServices(IServiceCollection services)
  {
  // ...
  services.AddAuthentication(JwtBearerDefaults.AuthenticationScheme)
          .AddMicrosoftIdentityWebApi(Configuration, Configuration.GetSection("AzureAd"))
            .EnableTokenAcquisitionToCallDownstreamApi()
               .AddMicrosoftGraph(Configuration.GetSection("GraphBeta"))
            .AddInMemoryTokenCaches();
   // ...
  }
  // ...
}
```

### <a name="option-2-call-a-downstream-web-api-other-than-microsoft-graph"></a>Opción 2: Llamada a una API web de nivel inferior que no sea Microsoft Graph

Para llamar a una API de nivel inferior que no sea Microsoft Graph, *Microsoft.Identity.Web* proporciona `.AddDownstreamWebApi()`, que solicita tokens y llama a la API web de nivel inferior.

```csharp
using Microsoft.Identity.Web;

public class Startup
{
  // ...
  public void ConfigureServices(IServiceCollection services)
  {
  // ...
  services.AddAuthentication(JwtBearerDefaults.AuthenticationScheme)
          .AddMicrosoftIdentityWebApi(Configuration, "AzureAd")
            .EnableTokenAcquisitionToCallDownstreamApi()
               .AddDownstreamWebApi("MyApi", Configuration.GetSection("GraphBeta"))
            .AddInMemoryTokenCaches();
   // ...
  }
  // ...
}
```

Al igual que con las aplicaciones web, puede elegir varias implementaciones de la caché de tokens. Para obtener más información, consulte [Microsoft.Identity.Web: Serialización de la caché de tokens](https://aka.ms/ms-id-web/token-cache-serialization) en GitHub.

En la imagen siguiente se muestran las distintas posibilidades de *Microsoft.Identity.Web* y su impacto en el archivo *Startup.cs*:

:::image type="content" source="media/scenarios/microsoft-identity-web-startup-cs.svg" alt-text="Diagrama de bloques que muestra las opciones de configuración del servicio en startup punto c s para llamar a una API web y especificar una implementación de la caché de tokens":::

> [!NOTE]
> Para comprender por completo los ejemplos de código que se indican a continuación, debe estar familiarizado con los [aspectos básicos de ASP.NET Core](/aspnet/core/fundamentals) y, en particular, con la [inserción de dependencias](/aspnet/core/fundamentals/dependency-injection) y las [opciones](/aspnet/core/fundamentals/configuration/options).

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

También puede ver un ejemplo de implementación del flujo con derechos delegados en [Node.js y Azure Functions](https://github.com/Azure-Samples/ms-identity-nodejs-webapi-onbehalfof-azurefunctions/blob/master/Function/MyHttpTrigger/index.js#L61).

## <a name="protocol"></a>Protocolo

Para más información acerca del protocolo OBO, consulte [Plataforma de identidad de Microsoft y flujo con derechos delegados de OAuth 2.0](./v2-oauth2-on-behalf-of-flow.md).

## <a name="next-steps"></a>Pasos siguientes

Avance al siguiente artículo de este escenario, [Obtención de un token para la aplicación](scenario-web-api-call-api-acquire-token.md).

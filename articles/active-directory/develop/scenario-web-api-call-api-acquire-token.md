---
title: Obtención de un token para una API web que llama a otras API web | Azure
titleSuffix: Microsoft identity platform
description: Aprenda a compilar una API web que llama a otras API web que necesitan adquirir un token para la aplicación.
services: active-directory
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 07/15/2020
ms.author: jmprieur
ms.custom: aaddev
ms.openlocfilehash: 56bcc4e4936371b58d78f6de5ce4c2d25fbf614d
ms.sourcegitcommit: 6109f1d9f0acd8e5d1c1775bc9aa7c61ca076c45
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2020
ms.locfileid: "94442810"
---
# <a name="a-web-api-that-calls-web-apis-acquire-a-token-for-the-app"></a>API web que llama a API web: Adquisición de un token para la aplicación

Una vez que ha creado un objeto de aplicación cliente, lo puede usar para adquirir un token y usarlo para llamar a una API web.

## <a name="code-in-the-controller"></a>Código del controlador

# <a name="aspnet-core"></a>[ASP.NET Core](#tab/aspnetcore)

*Microsoft.Identity.Web* agrega métodos de extensión que proporcionan servicios útiles para llamar a Microsoft Graph o a una API web de nivel inferior. Estos métodos se explican en detalle en [Una API web que llama a las API web: llamada a una API](scenario-web-api-call-api-call-api.md). Con estos métodos auxiliares, no es necesario adquirir un token manualmente.

Sin embargo, si desea adquirir manualmente un token, el código siguiente muestra un ejemplo de uso de *Microsoft.Identity.Web* para hacerlo en un controlador de API. Llama a una API de bajada denominada *todolist*.
Para obtener un token para llamar a la API de nivel inferior, se inserta el servicio `ITokenAcquisition` mediante la inserción de dependencias en el constructor del controlador (o el constructor de la página si utiliza Blazor) y se usa en las acciones del controlador, de modo que se obtiene un token para el usuario (`GetAccessTokenForUserAsync`) o para la propia aplicación (`GetAccessTokenForAppAsync`) en caso de un escenario de demonio.

```csharp
[Authorize]
public class MyApiController : Controller
{
    /// <summary>
    /// The web API will accept only tokens 1) for users, 2) that have the `access_as_user` scope for
    /// this API.
    /// </summary>
    static readonly string[] scopeRequiredByApi = new string[] { "access_as_user" };

     static readonly string[] scopesToAccessDownstreamApi = new string[] { "api://MyTodolistService/access_as_user" };

    private readonly ITokenAcquisition _tokenAcquisition;

    public MyApiController(ITokenAcquisition tokenAcquisition)
    {
        _tokenAcquisition = tokenAcquisition;
    }

    public IActionResult Index()
    {
        HttpContext.VerifyUserHasAnyAcceptedScope(scopeRequiredByApi);

        string accessToken = _tokenAcquisition.GetAccessTokenForUserAsync(scopesToAccessDownstreamApi);
        return await callTodoListService(accessToken);
    }
}
```

Para más información sobre el método `callTodoListService`, consulte [Una API web que llama a las API web: llamada a una API](scenario-web-api-call-api-call-api.md).

# <a name="java"></a>[Java](#tab/java)
A continuación, se muestra un ejemplo de código que se llama en las acciones de los controladores de API. Llama a la API de nivel inferior de Microsoft Graph.

```java
@RestController
public class ApiController {

    @Autowired
    MsalAuthHelper msalAuthHelper;

    @RequestMapping("/graphMeApi")
    public String graphMeApi() throws MalformedURLException {

        String oboAccessToken = msalAuthHelper.getOboToken("https://graph.microsoft.com/.default");

        return callMicrosoftGraphMeEndpoint(oboAccessToken);
    }

}
```

# <a name="python"></a>[Python](#tab/python)

Una API web de Python requiere el uso de middleware para validar el token de portador recibido del cliente. A continuación, la API web podrá obtener el token de acceso para una API de nivel inferior mediante la biblioteca de MSAL para Python al llamar al método [`acquire_token_on_behalf_of`](https://msal-python.readthedocs.io/en/latest/?badge=latest#msal.ConfidentialClientApplication.acquire_token_on_behalf_of). Aún no hay disponible ningún ejemplo de este flujo con MSAL Python.

---

## <a name="next-steps"></a>Pasos siguientes

Avance al siguiente artículo de este escenario, [Llamada a una API](scenario-web-api-call-api-call-api.md).

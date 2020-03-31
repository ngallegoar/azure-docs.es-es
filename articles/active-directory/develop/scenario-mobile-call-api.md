---
title: Llamada a una API web desde una aplicación móvil | Azure
titleSuffix: Microsoft identity platform
description: Obtenga información sobre cómo compilar una aplicación móvil que llama a las API web. (Llamada a una API de web).
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/07/2019
ms.author: jmprieur
ms.reviwer: brandwe
ms.custom: aaddev
ms.openlocfilehash: bd848fa6f74f049f97956ef1736ac2b08f3a6148
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "77160158"
---
# <a name="call-a-web-api-from-a-mobile-app"></a>Llamada a una API web desde una aplicación móvil

Después de que la aplicación haya iniciado la sesión de un usuario y recibido los tokens, la Biblioteca de autenticación de Microsoft (MSAL) expone información sobre el usuario, el entorno del usuario y los tokens emitidos. La aplicación puede utilizar estos valores para llamar a una API web o mostrar un mensaje de bienvenida al usuario.

En este artículo, primero veremos el resultado de MSAL. Después, veremos cómo usar un token de acceso de `AuthenticationResult` o `result` para llamar a una API web protegida.

## <a name="msal-result"></a>Resultado de MSAL
MSAL proporciona los valores siguientes: 

- `AccessToken` llama a API web protegidas en una solicitud de portador de HTTP.
- `IdToken` contiene información útil sobre el usuario que ha iniciado sesión. Esta información incluye el nombre del usuario, el inquilino principal y un identificador único para el almacenamiento.
- `ExpiresOn` es el tiempo de expiración del token. MSAL controla la actualización automática de una aplicación.
- `TenantId` es el identificador del inquilino donde el usuario ha iniciado sesión. En el caso de los usuarios invitados en Azure Active Directory (Azure AD) B2B, este valor identifica el inquilino donde el usuario ha iniciado sesión. El valor no identifica el inquilino principal del usuario.  
- `Scopes` indica los ámbitos que se han concedido con el token. Los ámbitos concedidos pueden ser un subconjunto de los ámbitos que ha solicitado.

MSAL también proporciona una abstracción para un valor `Account`. Un valor `Account` representa la cuenta de usuario con sesión iniciada actual:

- `HomeAccountIdentifier` identifica el inquilino principal del usuario.
- `UserName` es el nombre de usuario preferido del usuario. Este valor puede estar vacío para los usuarios de Azure AD B2C.
- `AccountIdentifier` identifica al usuario que ha iniciado sesión. En la mayoría de los casos, este valor es el mismo que el valor `HomeAccountIdentifier`, a menos que el usuario sea un invitado de otro inquilino.

## <a name="call-an-api"></a>Llamar a una API

Una vez que tenga el token de acceso, podrá llamar a una API web. La aplicación usará el token para compilar una solicitud HTTP y después ejecutar la solicitud.

### <a name="android"></a>Android

```Java
        RequestQueue queue = Volley.newRequestQueue(this);
        JSONObject parameters = new JSONObject();

        try {
            parameters.put("key", "value");
        } catch (Exception e) {
            // Error when constructing.
        }
        JsonObjectRequest request = new JsonObjectRequest(Request.Method.GET, MSGRAPH_URL,
                parameters,new Response.Listener<JSONObject>() {
            @Override
            public void onResponse(JSONObject response) {
                // Successfully called Graph. Process data and send to UI.
            }
        }, new Response.ErrorListener() {
            @Override
            public void onErrorResponse(VolleyError error) {
                // Error.
            }
        }) {
            @Override
            public Map<String, String> getHeaders() throws AuthFailureError {
                Map<String, String> headers = new HashMap<>();
                
                // Put access token in HTTP request.
                headers.put("Authorization", "Bearer " + authResult.getAccessToken());
                return headers;
            }
        };

        request.setRetryPolicy(new DefaultRetryPolicy(
                3000,
                DefaultRetryPolicy.DEFAULT_MAX_RETRIES,
                DefaultRetryPolicy.DEFAULT_BACKOFF_MULT));
        queue.add(request);
```

### <a name="msal-for-ios-and-macos"></a>MSAL para iOS y macOS

Los métodos para adquirir tokens devuelven un objeto `MSALResult`. `MSALResult` expone una propiedad `accessToken`. Puede usar `accessToken` para llamar a una API web. Agregue esta propiedad al encabezado de autorización HTTP antes de llamar para acceder a la API web protegida.

```objc
NSMutableURLRequest *urlRequest = [NSMutableURLRequest new];
urlRequest.URL = [NSURL URLWithString:"https://contoso.api.com"];
urlRequest.HTTPMethod = @"GET";
urlRequest.allHTTPHeaderFields = @{ @"Authorization" : [NSString stringWithFormat:@"Bearer %@", accessToken] };
        
NSURLSessionDataTask *task =
[[NSURLSession sharedSession] dataTaskWithRequest:urlRequest
     completionHandler:^(NSData * _Nullable data, NSURLResponse * _Nullable response, NSError * _Nullable error) {}];
[task resume];
```

```swift
let urlRequest = NSMutableURLRequest()
urlRequest.url = URL(string: "https://contoso.api.com")!
urlRequest.httpMethod = "GET"
urlRequest.allHTTPHeaderFields = [ "Authorization" : "Bearer \(accessToken)" ]
     
let task = URLSession.shared.dataTask(with: urlRequest as URLRequest) { (data: Data?, response: URLResponse?, error: Error?) in }
task.resume()
```

### <a name="xamarin"></a>Xamarin

[!INCLUDE [Call web API in .NET](../../../includes/active-directory-develop-scenarios-call-apis-dotnet.md)]

## <a name="make-several-api-requests"></a>Realización de varias solicitudes de API

Si tiene que llamar varias veces a la misma API, o si debe llamar a varias API, tenga en cuenta los temas siguientes al compilar la aplicación:

- **Consentimiento incremental**: La plataforma de identidad de Microsoft permite que las aplicaciones obtengan el consentimiento del usuario cuando se necesitan los permisos, en lugar de todos al principio. Cada vez que la aplicación está lista para llamar a una API, debe solicitar solo los ámbitos que necesita.

- **Acceso condicional**: Cuando realiza varias solicitudes a una API, en determinados escenarios, podría tener requisitos adicionales de acceso condicional. Los requisitos pueden aumentar de este modo si la primera solicitud no tiene ninguna directiva de acceso condicional y la aplicación intenta acceder en modo silencioso a una nueva API que requiere acceso condicional. Para controlar este problema, asegúrese de que detecta los errores de solicitudes silenciosas y prepárese para realizar una solicitud interactiva.  Para obtener más información, consulte [Guía de acceso condicional](../azuread-dev/conditional-access-dev-guide.md).

## <a name="call-several-apis-by-using-incremental-consent-and-conditional-access"></a>Llamada a varias API con consentimiento incremental y acceso condicional

Si necesita llamar a varias API para el mismo usuario, después de haber adquirido un token para el usuario, puede evitar solicitar repetidamente las credenciales del usuario mediante llamadas subsiguientes a `AcquireTokenSilent` para obtener un token:

```csharp
var result = await app.AcquireTokenXX("scopeApi1")
                      .ExecuteAsync();

result = await app.AcquireTokenSilent("scopeApi2")
                  .ExecuteAsync();
```

La interacción es necesaria en los siguientes casos:

- El usuario ha dado su consentimiento para la primera API, pero ahora debe dar su consentimiento para más ámbitos. En este caso, se usa el consentimiento incremental.
- La primera API no requiere autenticación multifactor, pero la siguiente API sí.

```csharp
var result = await app.AcquireTokenXX("scopeApi1")
                      .ExecuteAsync();

try
{
 result = await app.AcquireTokenSilent("scopeApi2")
                  .ExecuteAsync();
}
catch(MsalUiRequiredException ex)
{
 result = await app.AcquireTokenInteractive("scopeApi2")
                  .WithClaims(ex.Claims)
                  .ExecuteAsync();
}
```

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Paso a producción](scenario-mobile-production.md)

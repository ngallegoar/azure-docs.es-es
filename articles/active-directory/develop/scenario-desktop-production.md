---
title: 'Paso a producción de una aplicación de escritorio que llama a las API web: Plataforma de identidad de Microsoft | Azure'
description: Aprenda a pasar a producción una aplicación de escritorio que llama a las API web.
services: active-directory
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 10/30/2019
ms.author: jmprieur
ms.custom: aaddev
ms.openlocfilehash: ea564eb69f102d8e548bf8ae9a626598fa264cd4
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "80882886"
---
# <a name="desktop-app-that-calls-web-apis-move-to-production"></a>Aplicación de escritorio que llama a las API web: Paso a producción

En este artículo aprenderá a pasar a producción la aplicación de escritorio que llama a las API web.

## <a name="handle-errors-in-desktop-applications"></a>Control de errores en aplicaciones de escritorio

En los distintos flujos, ha aprendido a administrar los errores para los flujos silenciosos, tal como se muestra en los fragmentos de código. También ha visto que hay casos donde se necesita una interacción, como en el consentimiento incremental y el acceso condicional.

## <a name="have-the-user-consent-upfront-for-several-resources"></a>Consentimiento del usuario por adelantado para varios recursos

> [!NOTE]
> La plataforma de identidad de Microsoft permite obtener el consentimiento para varios recursos, pero no así Azure Active Directory (Azure AD) B2C. Azure AD B2C solo admite el consentimiento del administrador, no el consentimiento del usuario.

El punto de conexión de la plataforma de identidad de Microsoft (v2.0) no le permitirá obtener un token para varios recursos a la vez. El parámetro `scopes` puede contener ámbitos para un único recurso. Asegúrese de que el usuario da su consentimiento con anterioridad a varios recursos mediante el parámetro `extraScopesToConsent`.

Por ejemplo, podría tener dos recursos, y cada uno de ellos podría tener dos ámbitos:

- `https://mytenant.onmicrosoft.com/customerapi` con los ámbitos `customer.read` y `customer.write`
- `https://mytenant.onmicrosoft.com/vendorapi` con los ámbitos `vendor.read` y `vendor.write`

En este ejemplo, usaría el modificador `.WithAdditionalPromptToConsent`, que tiene el parámetro `extraScopesToConsent`.

Por ejemplo:

### <a name="in-msalnet"></a>En MSAL.NET

```csharp
string[] scopesForCustomerApi = new string[]
{
  "https://mytenant.onmicrosoft.com/customerapi/customer.read",
  "https://mytenant.onmicrosoft.com/customerapi/customer.write"
};
string[] scopesForVendorApi = new string[]
{
 "https://mytenant.onmicrosoft.com/vendorapi/vendor.read",
 "https://mytenant.onmicrosoft.com/vendorapi/vendor.write"
};

var accounts = await app.GetAccountsAsync();
var result = await app.AcquireTokenInteractive(scopesForCustomerApi)
                     .WithAccount(accounts.FirstOrDefault())
                     .WithExtraScopeToConsent(scopesForVendorApi)
                     .ExecuteAsync();
```

### <a name="in-msal-for-ios-and-macos"></a>En MSAL para iOS y macOS

Objective-C:

```objc
NSArray *scopesForCustomerApi = @[@"https://mytenant.onmicrosoft.com/customerapi/customer.read",
                                @"https://mytenant.onmicrosoft.com/customerapi/customer.write"];

NSArray *scopesForVendorApi = @[@"https://mytenant.onmicrosoft.com/vendorapi/vendor.read",
                              @"https://mytenant.onmicrosoft.com/vendorapi/vendor.write"]

MSALInteractiveTokenParameters *interactiveParams = [[MSALInteractiveTokenParameters alloc] initWithScopes:scopesForCustomerApi webviewParameters:[MSALWebviewParameters new]];
interactiveParams.extraScopesToConsent = scopesForVendorApi;
[application acquireTokenWithParameters:interactiveParams completionBlock:^(MSALResult *result, NSError *error) { /* handle result */ }];
```

Swift:

```swift
let scopesForCustomerApi = ["https://mytenant.onmicrosoft.com/customerapi/customer.read",
                            "https://mytenant.onmicrosoft.com/customerapi/customer.write"]

let scopesForVendorApi = ["https://mytenant.onmicrosoft.com/vendorapi/vendor.read",
                          "https://mytenant.onmicrosoft.com/vendorapi/vendor.write"]

let interactiveParameters = MSALInteractiveTokenParameters(scopes: scopesForCustomerApi, webviewParameters: MSALWebviewParameters())
interactiveParameters.extraScopesToConsent = scopesForVendorApi
application.acquireToken(with: interactiveParameters, completionBlock: { (result, error) in /* handle result */ })
```

Con esta llamada obtiene un token de acceso para la primera API web.

Cuando necesite llamar a la segunda API web, llame a la API `AcquireTokenSilent`.

```csharp
AcquireTokenSilent(scopesForVendorApi, accounts.FirstOrDefault()).ExecuteAsync();
```

### <a name="microsoft-personal-account-requires-reconsent-each-time-the-app-runs"></a>La cuenta personal de Microsoft requiere volver a solicitar el consentimiento cada vez que se ejecuta la aplicación

En el caso de los usuarios de cuentas personales de Microsoft, el comportamiento previsto consiste en volver a solicitar el consentimiento en cada llamada de cliente nativo (aplicación de escritorio o móvil) para autorizarla. La identidad del cliente nativo es inherentemente insegura, todo lo contrario a la identidad de una aplicación cliente confidencial, que intercambia un secreto con la plataforma de identidad de Microsoft para demostrar su identidad. La plataforma de identidad de Microsoft eligió mitigar esta inseguridad para servicios del consumidor pidiendo al usuario su consentimiento cada vez que se autoriza la aplicación.

## <a name="next-steps"></a>Pasos siguientes

[!INCLUDE [Move to production common steps](../../../includes/active-directory-develop-scenarios-production.md)]

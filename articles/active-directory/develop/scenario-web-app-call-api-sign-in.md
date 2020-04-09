---
title: 'Eliminación de cuentas de la caché de tokens durante el cierre de sesión: plataforma de identidad de Microsoft | Azure'
description: Obtenga información sobre cómo quitar una cuenta de la memoria caché de tokens en el cierre de sesión
services: active-directory
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 09/30/2019
ms.author: jmprieur
ms.custom: aaddev
ms.openlocfilehash: 934b756329065c466f21fca1480247065bdea28b
ms.sourcegitcommit: d187fe0143d7dbaf8d775150453bd3c188087411
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80881619"
---
# <a name="a-web-app-that-calls-web-apis-remove-accounts-from-the-token-cache-on-global-sign-out"></a>Aplicación web que llama a API web: Eliminación de cuentas de la caché de tokens durante el cierre de sesión global

Ha aprendido cómo agregar el inicio de sesión a la aplicación web en [Aplicación web que inicia sesión de usuarios: Inicio y cierre de sesión](scenario-web-app-sign-user-sign-in.md).

El cierre de sesión es diferente en el caso de una aplicación web que llama a API web. Cuando el usuario cierra la sesión en la aplicación, o en cualquier otra aplicación, debe quitar los tokens asociados a ese usuario de la caché de tokens.

## <a name="intercept-the-callback-after-single-sign-out"></a>Interceptación de la devolución de llamada tras un cierre de sesión único

Para borrar la entrada de la caché de tokens asociada a la cuenta que ha cerrado la sesión, la aplicación puede interceptar el evento posterior a `logout`. Las aplicaciones web almacenan tokens de acceso de cada usuario en una caché de tokens. Al interceptar la devolución de llamada posterior a `logout`, la aplicación web puede quitar el usuario de la caché.

# <a name="aspnet-core"></a>[ASP.NET Core](#tab/aspnetcore)

En ASP.NET Core, el mecanismo de interceptación se ilustra en el método `AddMsal()` de [WebAppServiceCollectionExtensions.cs#L151-L157](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/blob/db7f74fd7e65bab9d21092ac1b98a00803e5ceb2/Microsoft.Identity.Web/WebAppServiceCollectionExtensions.cs#L151-L157).

La dirección URL de cierre de sesión registrada previamente para la aplicación permite implementar el cierre de sesión único. El punto de conexión `logout` de la plataforma de identidad de Microsoft llama a la dirección URL de cierre de sesión. Esta llamada se produce si el cierre de sesión se ha iniciado desde la aplicación web, desde otra aplicación web o desde el explorador. Para más información, consulte [Cierre de sesión único](v2-protocols-oidc.md#single-sign-out).

```csharp
public static class WebAppServiceCollectionExtensions
{
 public static IServiceCollection AddMsal(this IServiceCollection services, IConfiguration configuration, IEnumerable<string> initialScopes, string configSectionName = "AzureAd")
 {
  // Code omitted here

  services.Configure<OpenIdConnectOptions>(AzureADDefaults.OpenIdScheme, options =>
  {
   // Code omitted here

   // Handling the sign-out: Remove the account from MSAL.NET cache.
   options.Events.OnRedirectToIdentityProviderForSignOut = async context =>
   {
    // Remove the account from MSAL.NET token cache.
    var tokenAcquisition = context.HttpContext.RequestServices.GetRequiredService<ITokenAcquisition>();
    await tokenAcquisition.RemoveAccountAsync(context).ConfigureAwait(false);
   };
  });
  return services;
 }
}
```

El código de `RemoveAccountAsync` está disponible en [Microsoft.Identity.Web/TokenAcquisition.cs#L264-L288](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/blob/db7f74fd7e65bab9d21092ac1b98a00803e5ceb2/Microsoft.Identity.Web/TokenAcquisition.cs#L264-L288).

# <a name="aspnet"></a>[ASP.NET](#tab/aspnet)

El ejemplo de ASP.NET no quita las cuentas de la caché durante el cierre de sesión global.

# <a name="java"></a>[Java](#tab/java)

El ejemplo de Java no quita las cuentas de la caché durante el cierre de sesión global.

# <a name="python"></a>[Python](#tab/python)

El ejemplo de Python no quita las cuentas de la caché durante el cierre de sesión global.

---

## <a name="next-steps"></a>Pasos siguientes

# <a name="aspnet-core"></a>[ASP.NET Core](#tab/aspnetcore)

> [!div class="nextstepaction"]
> [Adquisición de un token para la aplicación web](https://docs.microsoft.com/azure/active-directory/develop/scenario-web-app-call-api-acquire-token?tabs=aspnetcore)

# <a name="aspnet"></a>[ASP.NET](#tab/aspnet)

> [!div class="nextstepaction"]
> [Adquisición de un token para la aplicación web](https://docs.microsoft.com/azure/active-directory/develop/scenario-web-app-call-api-acquire-token?tabs=aspnet)

# <a name="java"></a>[Java](#tab/java)

> [!div class="nextstepaction"]
> [Adquisición de un token para la aplicación web](https://docs.microsoft.com/azure/active-directory/develop/scenario-web-app-call-api-acquire-token?tabs=java)

# <a name="python"></a>[Python](#tab/python)

> [!div class="nextstepaction"]
> [Adquisición de un token para la aplicación web](https://docs.microsoft.com/azure/active-directory/develop/scenario-web-app-call-api-acquire-token?tabs=python)

---

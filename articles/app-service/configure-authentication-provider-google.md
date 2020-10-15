---
title: Configuración de la autenticación de Google
description: Aprenda a configurar la autenticación de Google como proveedor de identidades para una aplicación App Service o Azure Functions.
ms.assetid: 2b2f9abf-9120-4aac-ac5b-4a268d9b6e2b
ms.topic: article
ms.date: 09/02/2019
ms.custom:
- seodec18
- fasttrack-edit
ms.openlocfilehash: e8a9fbe6072f3628d755ad3ad5aa5a623fc3ab23
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "80519943"
---
# <a name="configure-your-app-service-or-azure-functions-app-to-use-google-login"></a>Configuración de una aplicación de App Service o Azure Functions para usar el inicio de sesión de Google

[!INCLUDE [app-service-mobile-selector-authentication](../../includes/app-service-mobile-selector-authentication.md)]

En este tema se muestra cómo configurar Azure App Service o Azure Functions para usar Google como proveedor de autenticación.

Para llevar a cabo el procedimiento descrito en este tema, debe tener una cuenta de Google asociada a una dirección de correo electrónico verificada. Para crear una cuenta de Google, vaya a [accounts.google.com](https://go.microsoft.com/fwlink/p/?LinkId=268302).

## <a name="register-your-application-with-google"></a><a name="register"> </a>Registro de la aplicación con Google

1. Siga la documentación de Google sobre el [inicio de sesión de Google para aplicaciones de servidor](https://developers.google.com/identity/sign-in/web/server-side-flow) para crear un identificador de cliente y un secreto de cliente. No es necesario realizar ningún cambio en el código. Use la siguiente información:
    - En **Authorized JavaScript Origins** (Orígenes de JavaScript autorizados), use `https://<app-name>.azurewebsites.net` con el nombre de la aplicación en *\<app-name>* .
    - En **Authorized Redirect URI** (URI de redireccionamiento autorizado), use `https://<app-name>.azurewebsites.net/.auth/login/google/callback`.
1. Copie los valores de identificador y secreto de la aplicación.

    > [!IMPORTANT]
    > El secreto de aplicación es una credencial de seguridad importante. No comparta este secreto con nadie ni lo distribuya en una aplicación cliente.

## <a name="add-google-information-to-your-application"></a><a name="secrets"> </a>Incorporación de información de Google a la aplicación

1. En [Azure Portal], vaya a la aplicación de App Service.
1. Haga clic en **Configuración** > **Autenticación/autorización** y asegúrese de que la opción **Autenticación de App Service** está **Activada**.
1. Seleccione **Google** y pegue los valores de identificador y secreto de la aplicación que obtuvo anteriormente. Habilite los ámbitos que necesite la aplicación.
1. Seleccione **Aceptar**.

   App Service ofrece autenticación pero no restringe el acceso autorizado al contenido del sitio ni a las API. Para obtener más información, consulte [Autorización o denegación de usuarios](app-service-authentication-how-to.md#authorize-or-deny-users).

1. (Opcional) Para restringir el acceso al sitio solo a los usuarios autenticados mediante Google, establezca **Acción necesaria cuando la solicitud no está autenticada** en **Google**. Al establecer esta funcionalidad, la aplicación requiere que se autentiquen todas las solicitudes. También redirige todas las solicitudes no autenticadas a Google para la autenticación.

    > [!CAUTION]
    > Este método de restricción del acceso se aplica a todas las llamadas a la aplicación, lo que puede no interesar en las aplicaciones que tienen una página principal disponible públicamente así como en muchas aplicaciones de página única. Para tales aplicaciones, puede ser preferible **Permitir solicitudes anónimas (ninguna acción)** y que la aplicación inicie manualmente la autenticación. Para más información, consulte [Flujo de autenticación](overview-authentication-authorization.md#authentication-flow).

1. Seleccione **Guardar**.

De este modo ya estará listo para usar Google para realizar la autenticación en la aplicación.

## <a name="next-steps"></a><a name="related-content"> </a>Pasos siguientes

[!INCLUDE [app-service-mobile-related-content-get-started-users](../../includes/app-service-mobile-related-content-get-started-users.md)]

<!-- Anchors. -->

<!-- Images. -->

[0]: ./media/app-service-mobile-how-to-configure-google-authentication/mobile-app-google-redirect.png
[1]: ./media/app-service-mobile-how-to-configure-google-authentication/mobile-app-google-settings.png

<!-- URLs. -->

[Google apis]: https://go.microsoft.com/fwlink/p/?LinkId=268303

[Azure Portal]: https://portal.azure.com/


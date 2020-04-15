---
title: Configuración de la autenticación de Twitter
description: Aprenda a configurar la autenticación de Twitter como proveedor de identidades para una aplicación App Service o Azure Functions.
ms.assetid: c6dc91d7-30f6-448c-9f2d-8e91104cde73
ms.topic: article
ms.date: 02/28/2020
ms.custom:
- seodec18
- fasttrack-edit
ms.openlocfilehash: 11c913b12b4dcb7d2a5ffa532064b347b82904ef
ms.sourcegitcommit: b0ff9c9d760a0426fd1226b909ab943e13ade330
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/01/2020
ms.locfileid: "80519903"
---
# <a name="configure-your-app-service-or-azure-functions-app-to-use-twitter-login"></a>Configuración de una aplicación de App Service o Azure Functions para usar el inicio de sesión de Twitter

[!INCLUDE [app-service-mobile-selector-authentication](../../includes/app-service-mobile-selector-authentication.md)]

En este artículo se muestra cómo configurar Azure App Service o Azure Functions para usar Twitter como proveedor de autenticación.

Para llevar a cabo el procedimiento descrito en este artículo, debe tener una cuenta de Twitter asociada a una dirección de correo electrónico verificada y a un número de teléfono. Para crear una cuenta de Twitter, vaya a [twitter.com].

## <a name="register-your-application-with-twitter"></a><a name="register"> </a>Registrar una aplicación con Twitter

1. Inicie sesión en [Azure Portal] y vaya a la aplicación. Copie el valor de **Dirección URL**. Lo usará para configurar la aplicación de Twitter.
1. Vaya al sitio web para [desarrolladores de Twitter], inicie sesión con las credenciales de la cuenta de Twitter y seleccione **Create an app** (Crear una aplicación).
1. Introduzca el **nombre de la aplicación** y la **descripción de la aplicación** para su nueva aplicación. Pegue la **dirección URL** de la aplicación en el campo **Website URL** (Dirección URL del sitio web). En la sección **Callback URLs** (Direcciones URL de devolución de llamada), escriba la dirección URL HTTPS de la aplicación de App Service y anexe la ruta de acceso`/.auth/login/twitter/callback`. Por ejemplo, `https://contoso.azurewebsites.net/.auth/login/twitter/callback`.
1. En la parte inferior de la página, escriba al menos 100 caracteres en **Tell us how this app will be used** (Infórmenos de cómo se usará esta aplicación) y, después, seleccione **Create** (Crear). Vuelva a hacer clic en **Create** (Crear) en el elemento emergente. Se muestran los detalles de la aplicación.
1. Seleccione la pestaña **Claves y tokens de acceso** .

   Anote estos valores:
   - Clave de API
   - Clave secreta de API

   > [!NOTE]
   > La clave secreta de API es una credencial de seguridad importante. por lo que no debe compartirlo con nadie ni distribuirlo con su aplicación.

## <a name="add-twitter-information-to-your-application"></a><a name="secrets"> </a>Adición de información de Twitter a su aplicación

1. Vaya a la aplicación en [Azure Portal].
1. Haga clic en **Configuración** > **Autenticación/autorización** y asegúrese de que la opción **Autenticación de App Service** está **Activada**.
1. Seleccione **Twitter**.
1. Pegue los valores `API key` y `API secret key` que obtuvo anteriormente.
1. Seleccione **Aceptar**.

   ![Captura de pantalla de la configuración de Twitter para la aplicación móvil][1]

   De forma predeterminada, App Service ofrece autenticación pero no restringe el acceso autorizado al contenido del sitio ni a las API. Debe autorizar a los usuarios en el código de la aplicación.

1. (Opcional) Para restringir el acceso al sitio solo a los usuarios autenticados mediante Twitter, establezca **Acción necesaria cuando la solicitud no está autenticada** en **Twitter**. Al establecer esta funcionalidad, la aplicación requiere que se autentiquen todas las solicitudes. También redirige todas las solicitudes no autenticadas a Twitter para la autenticación.

   > [!CAUTION]
   > Este método de restricción del acceso se aplica a todas las llamadas a la aplicación, lo que puede no interesar en las aplicaciones que tienen una página principal disponible públicamente así como en muchas aplicaciones de página única. Para tales aplicaciones, puede ser preferible **Permitir solicitudes anónimas (ninguna acción)** y que la aplicación inicie manualmente la autenticación. Para más información, consulte [Flujo de autenticación](overview-authentication-authorization.md#authentication-flow).

1. Seleccione **Guardar**.

De este modo ya estará listo para usar Twitter para realizar la autenticación en la aplicación.

## <a name="next-steps"></a><a name="related-content"> </a>Pasos siguientes

[!INCLUDE [app-service-mobile-related-content-get-started-users](../../includes/app-service-mobile-related-content-get-started-users.md)]

<!-- Images. -->

[0]: ./media/app-service-mobile-how-to-configure-twitter-authentication/app-service-twitter-redirect.png
[1]: ./media/app-service-mobile-how-to-configure-twitter-authentication/mobile-app-twitter-settings.png

<!-- URLs. -->

[desarrolladores de Twitter]: https://go.microsoft.com/fwlink/p/?LinkId=268300
[twitter.com]: https://go.microsoft.com/fwlink/p/?LinkID=268287
[Azure Portal]: https://portal.azure.com/
[xamarin]: ../app-services-mobile-app-xamarin-ios-get-started-users.md

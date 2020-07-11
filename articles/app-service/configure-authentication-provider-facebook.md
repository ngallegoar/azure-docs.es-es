---
title: Configuración de la autenticación de Facebook
description: Aprenda a configurar la autenticación de Facebook como proveedor de identidades para una aplicación de App Service o Azure Functions.
ms.assetid: b6b4f062-fcb4-47b3-b75a-ec4cb51a62fd
ms.topic: article
ms.date: 06/06/2019
ms.custom:
- seodec18
- fasttrack-edit
ms.openlocfilehash: b6aad323c0d6fa8f59c9fad203640c477b162503
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "80519958"
---
# <a name="configure-your-app-service-or-azure-functions-app-to-use-facebook-login"></a>Configuración de una aplicación de App Service o Azure Functions para usar el inicio de sesión de Facebook

[!INCLUDE [app-service-mobile-selector-authentication](../../includes/app-service-mobile-selector-authentication.md)]

En este artículo se muestra cómo configurar Azure App Service o Azure Functions para usar Facebook como proveedor de autenticación.

Para llevar a cabo el procedimiento descrito en este artículo, debe tener una cuenta de Facebook asociada a una dirección de correo electrónico verificada y a un número de teléfono móvil. Para crear una cuenta de Facebook, vaya a [facebook.com].

## <a name="register-your-application-with-facebook"></a><a name="register"> </a>Registro de la aplicación con Facebook

1. Vaya al sitio web para [desarrolladores de Facebook] e inicie sesión con las credenciales de su cuenta de Facebook.

   Si no tiene una cuenta de Facebook for Developers, seleccione **Empezar** y siga los pasos de registro.
1. Seleccione **My Apps** > **Add New App** (Mis aplicaciones > Agregar nueva aplicación).
1. En el campo **Display Name** (Nombre para mostrar):
   1. Escriba un nombre único para la aplicación.
   1. Indique el valor de **Contact Email** (Correo electrónico de contacto).
   1. Seleccione **Create App ID** (Crear identificador de aplicación).
   1. Complete la comprobación de seguridad.

   Se abre el panel del desarrollador de la nueva aplicación de Facebook.
1. Haga clic en **Dashboard** > **Facebook Login** > **Set up** > **Web** (Panel > Inicio de sesión de Facebook > Configurar > Web).
1. En el panel de navegación izquierdo, en **Facebook Login** (Inicio de sesión de Facebook), haga clic en **Settings** (Configuración).
1. En el campo **Valid OAuth redirect URIs** (URI de redireccionamiento OAuth válidos), escriba `https://<app-name>.azurewebsites.net/.auth/login/facebook/callback`. No olvide reemplazar `<app-name>` por el nombre de la aplicación de Azure App Service.
1. Seleccione **Save changes** (Guardar los cambios).
1. En el panel izquierdo, seleccione **Settings** > **Basic** (Configuración > Básica). 
1. En el campo **App Secret** (Secreto de aplicación), seleccione **Show** (Mostrar). Copie los valores de **ID de la aplicación** y **Secreto de la aplicación**. Más adelante los usará para configurar la aplicación de App Service en Azure.

   > [!IMPORTANT]
   > El secreto de aplicación es una credencial de seguridad importante, No comparta este secreto con nadie ni lo distribuya en una aplicación cliente.
   >

1. La cuenta de Facebook que utilizó para registrar la aplicación es un administrador de la aplicación. En este momento, solo los administradores pueden iniciar sesión en esta aplicación.

   Para autenticar otras cuentas de Facebook, seleccione **App Review** (Revisión de aplicaciones) y habilite **Make \<your-app-name> public** (Hacer pública) para permitir que el público general acceda a la aplicación mediante la autenticación de Facebook.

## <a name="add-facebook-information-to-your-application"></a><a name="secrets"> </a>Agregar información de Facebook a la aplicación

1. Inicie sesión en [Azure Portal] y vaya a la aplicación App Service.
1. Haga clic en **Configuración** > **Autenticación/autorización** y asegúrese de que la opción **Autenticación de App Service** está **Activada**.
1. Seleccione **Facebook** y pegue los valores de identificador y secreto de la aplicación que obtuvo anteriormente. Habilite los ámbitos que necesite la aplicación.
1. Seleccione **Aceptar**.

   ![Captura de pantalla de la configuración de Facebook para la aplicación móvil][0]

    De forma predeterminada, App Service ofrece autenticación pero no restringe el acceso autorizado al contenido del sitio ni a las API. Tiene que autorizar a los usuarios en el código de la aplicación.
1. (Opcional) Para restringir el acceso solo a los usuarios autenticados mediante Facebook, establezca **Acción necesaria cuando la solicitud no está autenticada** en **Facebook**. Al establecer esta funcionalidad, la aplicación requiere que se autentiquen todas las solicitudes. También redirige todas las solicitudes no autenticadas a Facebook para la autenticación.

   > [!CAUTION]
   > Este método de restricción del acceso se aplica a todas las llamadas a la aplicación, lo que puede no interesar en las aplicaciones que tienen una página principal disponible públicamente así como en muchas aplicaciones de página única. Para tales aplicaciones, puede ser preferible **Permitir solicitudes anónimas (ninguna acción)** y que la aplicación inicie manualmente la autenticación. Para más información, consulte [Flujo de autenticación](overview-authentication-authorization.md#authentication-flow).

1. Seleccione **Guardar**.

De este modo ya estará listo para usar Facebook para realizar la autenticación en la aplicación.

## <a name="next-steps"></a><a name="related-content"> </a>Pasos siguientes

[!INCLUDE [app-service-mobile-related-content-get-started-users](../../includes/app-service-mobile-related-content-get-started-users.md)]

<!-- Images. -->
[0]: ./media/app-service-mobile-how-to-configure-facebook-authentication/mobile-app-facebook-settings.png

<!-- URLs. -->
[Desarrolladores de Facebook]: https://go.microsoft.com/fwlink/p/?LinkId=268286
[facebook.com]: https://go.microsoft.com/fwlink/p/?LinkId=268285
[Get started with authentication]: /en-us/develop/mobile/tutorials/get-started-with-users-dotnet/
[Azure Portal]: https://portal.azure.com/

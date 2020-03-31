---
title: Configuración de la autenticación de Azure AD
description: Aprenda a configurar la autenticación de Azure Active Directory como proveedor de identidades para una aplicación de App Service.
ms.assetid: 6ec6a46c-bce4-47aa-b8a3-e133baef22eb
ms.topic: article
ms.date: 09/03/2019
ms.custom: seodec18, fasttrack-edit
ms.openlocfilehash: fdad1f820d006c39fa135a29a5ec7377c47591f4
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "80046444"
---
# <a name="configure-your-app-service-app-to-use-azure-ad-login"></a>Configuración de la aplicación de App Service para usar el inicio de sesión de Azure AD

[!INCLUDE [app-service-mobile-selector-authentication](../../includes/app-service-mobile-selector-authentication.md)]

En este artículo se muestra cómo configurar Azure App Service para usar Azure Active Directory (Azure AD) como proveedor de autenticación.

Siga estos procedimientos recomendados para configurar la aplicación y la autenticación:

- Asigne a cada aplicación de App Service sus propios permisos y consentimiento.
- Configure cada aplicación de App Service con su propio registro.
- Evite el uso compartido de permisos entre entornos mediante registros de aplicación independientes para ranuras de implementación independientes. Al probar nuevo código, esta práctica puede ayudar a evitar que los problemas afecten a la aplicación de producción.

## <a name="configure-with-express-settings"></a><a name="express"> </a>Configuración rápida

> [!NOTE]
> La opción **Rápida** no está disponible en las nubes de las administraciones públicas. 

1. En [Azure Portal], busque y seleccione **App Services** y luego elija la aplicación.
2. En el panel de navegación izquierdo, seleccione **Autenticación/Autorización** > **Activado**.
3. Seleccione **Azure Active Directory** > **Rápido**.

   Si prefiere elegir un registro de aplicación existente:

   1. Elija **Seleccionar aplicación de AD existente** y haga clic en **Aplicación de Azure AD**.
   2. Elija un registro de aplicación existente y haga clic en **Aceptar**.

3. Seleccione **Aceptar** para registrar la aplicación de App Service en Azure Active Directory. Se crea un nuevo registro de la aplicación.
   
    ![Configuración rápida en Azure Active Directory](./media/configure-authentication-provider-aad/express-settings.png)
   
4. (Opcional) De manera predeterminada, App Service ofrece autenticación pero no restringe el acceso autorizado al contenido del sitio y a las API. Debe autorizar a los usuarios en el código de la aplicación. Para restringir el acceso a la aplicación solo a los usuarios autenticados mediante Azure Active Directory, configure **Acción necesaria cuando la solicitud no está autenticada** en **Iniciar sesión con Azure Active Directory**. Al establecer esta funcionalidad, la aplicación requiere que se autentiquen todas las solicitudes. También redirige todo lo que no está autenticado a Azure Active Directory para la autenticación.

    > [!CAUTION]
    > Este método de restricción del acceso se aplica a todas las llamadas a la aplicación, lo que puede no interesar en las aplicaciones que tienen una página principal disponible públicamente así como en muchas aplicaciones de página única. Para tales aplicaciones, puede ser preferible **Permitir solicitudes anónimas (ninguna acción)** y que la aplicación inicie manualmente el inicio de sesión. Para más información, consulte [Flujo de autenticación](overview-authentication-authorization.md#authentication-flow).
5. Seleccione **Guardar**.

## <a name="configure-with-advanced-settings"></a><a name="advanced"> </a>Configuración avanzada

Puede configurar la aplicación manualmente si quiere usar un registro de aplicaciones de otro inquilino de Azure AD. Para completar esta configuración personalizada:

1. Crear un registro en Azure AD.
2. Proporcionar algunos detalles de registro a App Service.

### <a name="create-an-app-registration-in-azure-ad-for-your-app-service-app"></a><a name="register"> </a>Creación de un registro de aplicaciones en Azure AD para la aplicación App Service

Para configurar la aplicación de App Service, necesitará la siguiente información:

- Id. de cliente
- Id. de inquilino
- Secreto de cliente (opcional)
- URI de Id. de aplicación

Lleve a cabo los siguiente pasos:

1. Inicie sesión en [Azure Portal], busque y seleccione **App Services** y luego elija la aplicación. Anote la **Dirección URL** de la aplicación. La usará para configurar el registro de la aplicación de Azure Active Directory.
1. Haga clic en **Azure Active Directory** > **Registros de aplicaciones** > **Nuevo registro**.
1. En la página **Register an application** (Registrar una aplicación), escriba el **nombre** del registro de aplicaciones.
1. En **URI de redirección**, seleccione **Web** y escriba `<app-url>/.auth/login/aad/callback`. Por ejemplo, `https://contoso.azurewebsites.net/.auth/login/aad/callback`. 
1. Seleccione **Crear**.
1. Una vez creado el registro de la aplicación, copie el **Id. de aplicación (cliente)** y el **Id. de directorio (inquilino)** para usarlos más adelante.
1. Seleccione la **personalización de marca**. En **URL de página principal**, escriba la dirección URL de la aplicación de App Service y seleccione **Guardar**.
1. Seleccione **Expose an API (Exponer una API)**  > **Set (Conjunto)** . Cópiela en la dirección URL de la aplicación de App Service y seleccione **Save** (Guardar).

   > [!NOTE]
   > Este valor es el **URI del identificador de la aplicación** del registro de aplicaciones. Si la aplicación web requiere acceso a una API en la nube, al configurar el recurso de App Service en la nube necesitará el valor de **URI de Id. de aplicación** de la aplicación web. Puede utilizarlo, por ejemplo, si desea que el servicio en la nube conceda acceso explícitamente a la aplicación web.

1. Seleccione **Agregar un ámbito**.
   1. En **Nombre de ámbito**, escriba *user_impersonation*.
   1. En los cuadros de texto, escriba el nombre y la descripción del ámbito de consentimiento que quiere que vean los usuarios en la página de consentimiento. Por ejemplo, escriba *Access my app* (Acceder a mi aplicación). 
   1. Seleccione la opción **Agregar un ámbito**.
1. (Opcional) Para crear un secreto de cliente, seleccione **Certificates & secrets (Certificados y secretos)**  > **New client secret (Nuevo secreto de cliente)**  > **Add (Agregar)** . Copie el valor del secreto del cliente que se muestra en la página. No se volverá a mostrar.
1. (Opcional) Para agregar varios valores en **Direcciones URL de respuesta**, seleccione **Autenticación**.

### <a name="enable-azure-active-directory-in-your-app-service-app"></a><a name="secrets"> </a>Habilitación de Azure Active Directory en la aplicación de App Service

1. En [Azure Portal], busque y seleccione **App Services** y luego elija la aplicación. 
1. En el panel izquierdo, en **Configuración**, seleccione **Autenticación/Autorización** > **Activado**.
1. (Opcional) De forma predeterminada, la autenticación de App Service permite el acceso no autenticado a la aplicación. Para aplicar la autenticación de usuario, establezca la **acción necesaria cuando la solicitud no está autenticada**, en **Log in with Azure Active Directory** (Iniciar sesión con Azure Active Directory).
1. En **Proveedores de autenticación,** seleccione **Azure Active Directory**.
1. En el **modo de administración**, seleccione **Advanced** (Avanzada) y configure la autenticación de App Service de acuerdo con la tabla siguiente:

    |Campo|Descripción|
    |-|-|
    |Id. de cliente| Use el **identificador de la aplicación (cliente)** del registro de aplicaciones. |
    |Id. del emisor| Use `https://login.microsoftonline.com/<tenant-id>` y reemplace *\<tenant-id>* con el **identificador de directorio (inquilino)** del registro de aplicaciones. |
    |Secreto de cliente (opcional)| Use el secreto de cliente que generó en el registro de la aplicación.|
    |Audiencias de token permitidas| Si se trata de una aplicación en la nube o una aplicación de servidor y quiere permitir tokens de autenticación desde una aplicación web, agregue aquí el valor de **URI de Id. de aplicación** de la aplicación web. De forma implícita, el **Id. de cliente** se considera *siempre* que es un público permitido. |

2. Seleccione **Aceptar** y después **Guardar**.

Ahora está preparado para usar Azure Active Directory para realizar la autenticación en la aplicación de App Service.

## <a name="configure-a-native-client-application"></a>Configuración de una aplicación de cliente nativo

Puede registrar clientes nativos para permitir la autenticación con una biblioteca cliente como la **Biblioteca de autenticación de Active Directory**.

1. En [Azure Portal], seleccione **Azure Active Directory** > **Registros de aplicaciones** > **Nuevo registro**.
1. En la página **Register an application** (Registrar una aplicación), escriba el **nombre** del registro de aplicaciones.
1. En **URI de redirección**, seleccione **Cliente público (móvil y escritorio)** , y escriba la dirección URL `<app-url>/.auth/login/aad/callback`. Por ejemplo, `https://contoso.azurewebsites.net/.auth/login/aad/callback`.

    > [!NOTE]
    > Para una aplicación de Windows, use el valor de [SID del paquete](../app-service-mobile/app-service-mobile-dotnet-how-to-use-client-library.md#package-sid) como URI en su lugar.
1. Seleccione **Crear**.
1. Una vez creado el registro de aplicación, copie el valor de **Id. de aplicación (cliente)** .
1. Seleccione **Permisos de API** > **Agregar permiso** > **Mis API**.
1. Seleccione el registro de aplicaciones que creó anteriormente para la aplicación de App Service. Si no ve el registro de aplicación, compruebe que agregó el ámbito **user_impersonation** en [Creación de un registro de aplicaciones en Azure AD para la aplicación App Service](#register).
1. Seleccione **user_impersonation** y después **Agregar permisos**.

Ahora ha configurado una aplicación cliente nativa que puede acceder a la aplicación de App Service.

## <a name="next-steps"></a><a name="related-content"> </a>Pasos siguientes

[!INCLUDE [app-service-mobile-related-content-get-started-users](../../includes/app-service-mobile-related-content-get-started-users.md)]

<!-- URLs. -->

[Azure Portal]: https://portal.azure.com/

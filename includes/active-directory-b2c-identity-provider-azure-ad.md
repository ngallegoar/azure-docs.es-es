---
author: msmimart
ms.service: active-directory-b2c
ms.subservice: B2C
ms.topic: include
ms.date: 04/07/2020
ms.author: mimart
ms.openlocfilehash: fb2ecd9ab63345954356d18164a2afe94a5aae24
ms.sourcegitcommit: a43a59e44c14d349d597c3d2fd2bc779989c71d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/25/2020
ms.locfileid: "96028357"
---
## <a name="register-an-azure-ad-app"></a>Registrar una aplicación de Azure AD

Para habilitar el inicio de sesión para los usuarios de una organización específica de Azure AD, es preciso registrar una aplicación en el inquilino de Azure AD de la organización.

1. Inicie sesión en [Azure Portal](https://portal.azure.com).
1. Asegúrese de que usa el directorio que contiene el inquilino de Azure AD de la organización (por ejemplo, contoso.com). Seleccione el filtro **Directorio y suscripción** del menú superior y, luego, el directorio que contiene el inquilino de Azure AD.
1. Elija **Todos los servicios** en la esquina superior izquierda de Azure Portal, y busque y seleccione **Registros de aplicaciones**.
1. Seleccione **Nuevo registro**.
1. Escriba el **nombre** de la aplicación. Por ejemplo, `Azure AD B2C App`.
1. Acepte la selección predeterminada de **Solo las cuentas de este directorio organizativo** para esta aplicación.
1. Para la **URI de redirección**, acepte el valor de **Web** y escriba la siguiente dirección URL en minúscula, donde `your-B2C-tenant-name` se reemplaza por el nombre del inquilino de Azure AD B2C.

    ```
    https://your-B2C-tenant-name.b2clogin.com/your-B2C-tenant-name.onmicrosoft.com/oauth2/authresp
    ```

    Por ejemplo, `https://fabrikam.b2clogin.com/fabrikam.onmicrosoft.com/oauth2/authresp`.

1. Seleccione **Registrar**. Anote el **Id. de aplicación (cliente)** para usarlo en un paso posterior.
1. Seleccione **Certificados y secretos** y luego seleccione **Nuevo secreto de cliente**.
1. En **Descripción**, escriba una descripción para el secreto, seleccione una fecha de expiración y seleccione **Agregar**. Registre el valor **Value** del secreto para usarlo en un paso posterior.

### <a name="configuring-optional-claims"></a>Configuración de notificaciones opcionales

Si quiere obtener las notificaciones `family_name` y `given_name` de Azure AD, puede configurar notificaciones opcionales para la aplicación en la interfaz de usuario de Azure Portal o el manifiesto de aplicación. Para obtener más información, consulte [Procedimientos: Proporcionar notificaciones opcionales a la aplicación de Azure AD](../articles/active-directory/develop/active-directory-optional-claims.md).

1. Inicie sesión en [Azure Portal](https://portal.azure.com). Busque y seleccione **Azure Active Directory**.
1. En la sección **Administrar**, seleccione **Registros de aplicaciones**.
1. Seleccione en la lista la aplicación para la que desea configurar notificaciones opcionales.
1. En la sección **Administrar**, seleccione **Configuración del token**.
1. Seleccione **Agregar notificación opcional**.
1. En **Tipo de token**, seleccione **ID**.
1. Seleccione las notificaciones opcionales que va a agregar, `family_name` y `given_name`.
1. Haga clic en **Agregar**.
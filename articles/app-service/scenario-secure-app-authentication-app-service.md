---
title: 'Tutorial: Incorporación de la autenticación a una aplicación web en Azure App Service | Azure'
description: En este tutorial aprenderá a habilitar la autenticación y la autorización para una aplicación web que se ejecuta en Azure App Service. También limitará el acceso a la aplicación web solo a los usuarios de su organización.
services: active-directory, app-service-web
author: rwike77
manager: CelesteDG
ms.service: app-service-web
ms.topic: tutorial
ms.workload: identity
ms.date: 11/09/2020
ms.author: ryanwi
ms.reviewer: stsoneff
ms.custom: azureday1
ms.openlocfilehash: a8bd2ef1348692bf57f7e5cb7b6606cfcfd324fe
ms.sourcegitcommit: 80c1056113a9d65b6db69c06ca79fa531b9e3a00
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/09/2020
ms.locfileid: "96905577"
---
# <a name="tutorial-add-authentication-to-your-web-app-running-on-azure-app-service"></a>Tutorial: Incorporación de la autenticación a una aplicación web que se ejecuta en Azure App Service

Aprenda a habilitar la autenticación para la aplicación web que se ejecuta en Azure App Service y a limitar el acceso solo a los usuarios de su organización.

:::image type="content" source="./media/scenario-secure-app-authentication-app-service/web-app-sign-in.svg" alt-text="Diagrama que muestra el inicio de sesión del usuario." border="false":::

App Service proporciona compatibilidad integrada con la autenticación y la autorización, para que pueda proporcionar inicio de sesión a los usuarios y acceso a los datos escribiendo una cantidad mínima de código, o directamente sin código, en la aplicación web. El uso del módulo de autenticación y autorización de App Service no es obligatorio, pero ayuda a simplificar estos procesos en la aplicación. En este artículo se muestra cómo proteger una aplicación web con el módulo de autenticación y autorización de App Service, utilizando Azure Active Directory (Azure AD) como proveedor de identidades.

El módulo de autenticación y autorización se habilita y se configura mediante la configuración de Azure Portal y de la aplicación. No se necesitan SDK, lenguajes específicos o cambios en el código de la aplicación. Se admiten diversos proveedores de identidades, entre los que se incluyen Azure AD, cuenta de Microsoft, Facebook, Google y Twitter. Cuando está habilitado el módulo de autenticación y autorización, cada solicitud HTTP entrante pasa a través de él antes de que el código de aplicación lo controle. Para obtener más información, vea [Autenticación y autorización en Azure App Service](overview-authentication-authorization.md).

En este tutorial, aprenderá a:

> [!div class="checklist"]
>
> * Configurar la autenticación de una aplicación web
> * Limitar el acceso a la aplicación web solo a los usuarios de su organización

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-and-publish-a-web-app-on-app-service"></a>Crear y publicar una aplicación web en App Service

Para este tutorial, necesita una aplicación web implementada en App Service. Puede usar una aplicación web existente, o puede seguir las indicaciones del [inicio rápido de ASP.NET Core](quickstart-dotnetcore.md) para crear y publicar una nueva aplicación web en App Service.

Si va a usar una aplicación web existente o a crear una nueva, tome nota del nombre de esta y del nombre del grupo de recursos en el que está implementada. Necesitará estos nombres en este tutorial. En este tutorial, los nombres de ejemplo de los procedimientos y las capturas de pantalla contienen *SecureWebApp*.

## <a name="configure-authentication-and-authorization"></a>Configuración de la autenticación y la autorización

Ahora tiene una aplicación web que se ejecuta en App Service. A continuación, habilitará la autenticación y la autorización para la aplicación web. En este caso, usa Azure AD como proveedor de identidades. Para más información, consulte [Configuración de una autenticación de App Service o Azure Functions para usar el inicio de sesión de Azure AD](configure-authentication-provider-aad.md).

En el menú de [Azure Portal](https://portal.azure.com), seleccione **Grupos de recursos** o busque y seleccione **Grupos de recursos** desde cualquier página.

En **Grupos de recursos**, busque y seleccione el grupo de recursos. En **Información general**, seleccione la página de administración de la aplicación.

:::image type="content" alt-text="Captura de pantalla que muestra la selección de la página de administración de la aplicación." source="./media/scenario-secure-app-authentication-app-service/select-app-service.png":::

En el menú de la izquierda de la aplicación, seleccione **Autenticación o autorización** y luego seleccione **Activo** para habilitar la autenticación de App Service.

En **Action to take when request is not authenticated** (Acción necesaria cuando la solicitud no está autenticada), seleccione **Log in with Azure Active Directory** (Iniciar sesión con Azure Active Directory).

En **Proveedores de autenticación,** seleccione **Azure Active Directory**. Seleccione **Rápido** y, a continuación, acepte la configuración predeterminada para crear una nueva aplicación de Active Directory. Seleccione **Aceptar**.

:::image type="content" alt-text="Captura de pantalla que muestra la autenticación rápida." source="./media/scenario-secure-app-authentication-app-service/configure-authentication.png":::

En la página **Autenticación o autorización**, seleccione **Guardar**.

Cuando vea la notificación con el mensaje `Successfully saved the Auth Settings for <app-name> App`, actualice la página del portal.

Ahora tiene una aplicación que está protegida por la autenticación y la autorización de App Service.

## <a name="verify-limited-access-to-the-web-app"></a>Comprobación del acceso limitado a la aplicación web

Al habilitar el módulo de autenticación y autorización de App Service, se crea un registro de aplicaciones en el inquilino de Azure AD. El registro de aplicaciones tiene el mismo nombre para mostrar que la aplicación web. Para comprobar la configuración, seleccione **Azure Active Directory** en el menú del portal y **Registros de aplicaciones**. Seleccione el registro de aplicaciones que creó. En la información general, compruebe que la opción **Tipos de cuenta compatibles** está establecida en **Solo mi organización**.

:::image type="content" alt-text="Captura de pantalla que muestra cómo comprobar el acceso." source="./media/scenario-secure-app-authentication-app-service/verify-access.png":::

Para comprobar que el acceso a la aplicación está limitado a los usuarios de la organización, inicie un explorador en el modo de incógnito o privado y vaya a `https://<app-name>.azurewebsites.net`. Se le debería dirigir a una página de inicio de sesión segura, lo que confirma que no se permite el acceso de los usuarios no autenticados al sitio. Inicie sesión como un usuario de la organización para obtener acceso al sitio. También puede iniciar un nueva ventana del explorador e intentar iniciar sesión con una cuenta personal para comprobar que los usuarios ajenos a la organización no tienen acceso.

## <a name="clean-up-resources"></a>Limpieza de recursos

Si ya ha terminado con este tutorial y no necesita la aplicación web ni los recursos asociados, [elimine los recursos que creó](scenario-secure-app-clean-up-resources.md).

## <a name="next-steps"></a>Pasos siguientes

En este tutorial ha aprendido a:

> [!div class="checklist"]
>
> * Configurar la autenticación de una aplicación web
> * Limitar el acceso a la aplicación web solo a los usuarios de su organización

> [!div class="nextstepaction"]
> [Accesos de App Service a Storage](scenario-secure-app-access-storage.md)

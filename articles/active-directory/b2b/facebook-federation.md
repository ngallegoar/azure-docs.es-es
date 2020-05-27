---
title: Incorporación de Facebook como proveedor de identidades - Azure AD
description: Fedérese con Facebook para permitir que los usuarios externos (invitados) inicien sesión en sus aplicaciones de Azure AD con sus propias cuentas de Facebook.
services: active-directory
ms.service: active-directory
ms.subservice: B2B
ms.topic: conceptual
ms.date: 05/19/2020
ms.author: mimart
author: msmimart
manager: celestedg
ms.reviewer: mal
ms.custom: it-pro, seo-update-azuread-jan
ms.collection: M365-identity-device-management
ms.openlocfilehash: a92fbd254f223e2c7eb70a4e86bb7e904294395e
ms.sourcegitcommit: bb0afd0df5563cc53f76a642fd8fc709e366568b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/19/2020
ms.locfileid: "83595084"
---
# <a name="add-facebook-as-an-identity-provider-for-external-identities"></a>Incorporación de Facebook como proveedor de identidades para identidades externas

Puede agregar Facebook a los flujos de usuario del registro de autoservicio (versión preliminar) para que los usuarios puedan iniciar sesión en sus aplicaciones con sus propias cuentas de Facebook. Para que los usuarios puedan iniciar sesión con Facebook, primero deberá [habilitar el registro de autoservicio](self-service-sign-up-user-flow.md) del inquilino. Después de agregar Facebook como proveedor de identidades, configure un flujo de usuario para la aplicación y seleccione Facebook como una de las opciones de inicio de sesión.

## <a name="create-an-app-in-the-facebook-developers-console"></a>Creación de una aplicación en la consola de desarrolladores de Facebook

Para usar una cuenta de Facebook como [proveedor de identidades](identity-providers.md), debe crear una aplicación en la consola de desarrolladores de Facebook. Si aún no tiene una cuenta de Facebook, puede suscribirse en [https://www.facebook.com/](https://www.facebook.com).

> [!NOTE]  
> Use las direcciones URL siguientes en los pasos 9 y 16 que verá a continuación.
> - En la **dirección URL del sitio** escriba `https://login.microsoftonline.com`.
> - En **Valid OAuth redirect URIs** (URI de redireccionamiento OAuth válidos), escriba `https://login.microsoftonline.com/te/<tenant-id>/oauth2/authresp`. Puede encontrar el valor `<tenant-ID>` en la hoja de información general de Azure Active Directory.


1. Inicie sesión en [Facebook for developers](https://developers.facebook.com/) con las credenciales de su cuenta de Facebook.
2. Si aún no lo ha hecho, debe registrarse como desarrollador de Facebook. Para ello, seleccione **Get Started** (Comenzar) en la esquina superior derecha de la página, acepte las directivas de Facebook y complete los pasos de registro.
3. Seleccione **Mis aplicaciones** y después **Crear una aplicación**.
4. Especifique el valor de **Display Name** (Nombre para mostrar) y un valor de **Contact Email** (Correo electrónico de contacto) válido.
5. Seleccione **Create App ID** (Crear identificador de aplicación). Es posible que deba aceptar las políticas de la plataforma Facebook y realizar una comprobación de seguridad en línea.
6. Seleccione **Settings** (Configuración)  > **Basic** (Básica).
7. Elija una **Categoría**, por ejemplo, Negocios y Páginas. Este valor es obligatorio para Facebook, pero no se usa para Azure AD.
8. En la parte inferior de la página, seleccione **Add Platform** (Agregar plataforma) y, después, seleccione **Website** (Sitio web).
9. En la **dirección URL del sitio**, escriba la dirección URL adecuada (indicada anteriormente).
10. En la **dirección URL de la directiva de privacidad**, escriba la dirección URL de la página donde se mantiene la información de privacidad de la aplicación; por ejemplo, `http://www.contoso.com`.
11. Seleccione **Save changes** (Guardar los cambios).
12. En la parte superior de la página, copie el valor de **App ID** (Id. de la aplicación).
13. Seleccione **Show** (Mostrar) y copie el valor de **App Secret** (Secreto de la aplicación). Use ambos para configurar Facebook como proveedor de identidades de su inquilino. El **secreto de la aplicación** es una credencial de seguridad importante.
14. Seleccione el signo más junto a **PRODUCTS** (Productos) y, después, seleccione **Set up** (Configurar) en **Facebook Login** (Inicio de sesión de Facebook).
15. En **Facebook Login** (Inicio de sesión de Facebook), seleccione **Settings** (Configuración).
16. En **Valid OAuth redirect URIs** (URI de redireccionamiento OAuth válidos), escriba la dirección URL adecuada (indicada anteriormente).
17. Seleccione **Save Changes** (Guardar cambios) en la parte inferior de la página.
18. Para que la aplicación de Facebook esté disponible para Azure AD, seleccione el selector de Estado situado en la parte superior derecha de la página y **actívelo** para hacer que la aplicación sea pública y, después, seleccione **Switch Mode** (Modo de conmutador). En este momento el estado debería cambiar de **Desarrollo** a **Activo**.
    
## <a name="configure-a-facebook-account-as-an-identity-provider"></a>Configuración de una cuenta de Facebook como proveedor de identidades

1. Inicie sesión en [Azure Portal](https://portal.azure.com) como administrador global del inquilino de Azure AD.
2. En **Servicios de Azure**, seleccione **Azure Active Directory**.
3. En el menú de la izquierda, seleccione **External Identities**.
4. Seleccione **All identity providers** (Todos los proveedores de identidades) y,a continuación, **Facebook**.
5. En el **id. de cliente**, escriba el **id.de aplicación** de Facebook que creó anteriormente.
6. En el **secreto de cliente**, escriba el **secreto de aplicación** que ha anotado.

   ![Captura de pantalla que muestra la página para agregar un proveedor de identidades de redes sociales.](media/facebook-federation/add-social-identity-provider-page.png)

7. Seleccione **Guardar**.

## <a name="next-steps"></a>Pasos siguientes

- [Invitación para que los usuarios externos colaboren](add-users-administrator.md)
- [Incorporación del registro de autoservicio a una aplicación](self-service-sign-up-user-flow.md)

---
title: Autorización de cuentas de desarrollador con Azure Active Directory B2C
titleSuffix: Azure API Management
description: Aprenda a autorizar a los usuarios para que usen Azure Active Directory B2C en API Management.
services: api-management
documentationcenter: API Management
author: miaojiang
manager: cfowler
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 11/04/2019
ms.author: apimpm
ms.openlocfilehash: 7b586edd7adce8bcea61419005a3ce8cfc814fb3
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "86254952"
---
# <a name="how-to-authorize-developer-accounts-by-using-azure-active-directory-b2c-in-azure-api-management"></a>Procedimiento para autorizar a las cuentas de desarrollador para que usen Azure Active Directory B2C en Azure API Management

## <a name="overview"></a>Información general

Azure Active Directory B2C es una solución de administración de identidades en la nube, destinada a aplicaciones móviles y web orientadas al consumidor. Puede usarlo para administrar el acceso a su portal para desarrolladores. Esta guía le muestra la configuración que se necesita en el servicio de API Management para integrarse con Azure Active Directory B2C. Para más información sobre cómo habilitar el acceso al portal para desarrolladores con Azure Active Directory clásico, consulte [Procedimiento para autorizar a las cuentas de desarrollador para que usen Azure Active Directory].

> [!NOTE]
> Para completar los pasos descritos en esta guía, primero debe tener un inquilino de Azure Active Directory B2C en el que crear una aplicación. Además, debe tener listas las directivas de registro e inicio de sesión. Para más información, consulte [Introducción a Azure Active Directory B2C].

[!INCLUDE [premium-dev-standard.md](../../includes/api-management-availability-premium-dev-standard.md)]

## <a name="authorize-developer-accounts-by-using-azure-active-directory-b2c"></a>Autorización de cuentas de desarrollador con Azure Active Directory B2C

1. Para empezar, inicie sesión en [Azure Portal](https://portal.azure.com) y busque la instancia de API Management.

   > [!NOTE]
   > Si todavía no ha creado una instancia de servicio de API Management, consulte [Creación de una instancia del servicio API Management][Create an API Management service instance] en el [tutorial Introducción a Azure API Management][Get started with Azure API Management].

1. En **Identidades**. Haga clic en **+ Agregar** en la parte superior.

   Aparece el panel **Agregar proveedor de identidades** a la derecha. Elija **Azure Active Directory B2C**.
    
   ![Agregar AAD B2C como proveedor de identidades][api-management-howto-add-b2c-identity-provider]

1. Copie la **Dirección URL de redireccionamiento**.

   ![Dirección URL de redireccionamiento del proveedor de identidades de AAD B2C][api-management-howto-copy-b2c-identity-provider-redirect-url]

1. En una pestaña nueva, acceda al inquilino de Azure Active Directory B2C en Azure Portal y abra la hoja **Aplicaciones**.

   ![Registro de una aplicación nueva 1][api-management-howto-aad-b2c-portal-menu]

1. Haga clic en el botón **Agregar** para crear una nueva aplicación de Azure Active Directory B2C.

   ![Registro de una aplicación nueva 2][api-management-howto-aad-b2c-add-button]

1. En la hoja **Nueva aplicación**, escriba un nombre para la aplicación. Elija **Sí** en **Aplicación web o API Web** y elija **Sí** en **Permitir flujo implícito**. A continuación, pegue la **dirección URL de redireccionamiento** que copió en el paso 3 en el cuadro de texto **Dirección URL de respuesta**.

   ![Registro de una aplicación nueva 3][api-management-howto-aad-b2c-app-details]

1. Si usa el nuevo portal para desarrolladores (en lugar del antiguo), rellene los campos **Nombre dado**, **Apellidos** e **Id. de objeto del usuario** en las notificaciones de la aplicación.

    ![Notificaciones de la aplicación](./media/api-management-howto-aad-b2c/api-management-application-claims.png)

1. Haga clic en el botón **Crear**. Cuando se crea la aplicación, aparece en la hoja **Aplicaciones**. Haga clic en el nombre de la aplicación para ver sus detalles.

   ![Registro de una aplicación nueva 4][api-management-howto-aad-b2c-app-created]

1. En la hoja **Propiedades**, copie el **identificador de aplicación** en el Portapapeles.

   ![Identificador de aplicación 1][api-management-howto-aad-b2c-app-id]

1. Vuelva al panel **Agregar proveedor de identidades** de API Management y pegue el identificador en el cuadro de texto **Id. de cliente**.
    
1.  Vuelva al registro de la aplicación B2C, haga clic en el botón **Claves** y, luego, haga clic en **Generar clave**. Haga clic en **Guardar** para guardar la configuración y mostrar la **clave de la aplicación**. Copie la clave en el Portapapeles.

    ![Clave de aplicación 1][api-management-howto-aad-b2c-app-key]

1.  Vuelva al panel **Agregar proveedor de identidades** de API Management y pegue la clave en el cuadro de texto **Secreto de cliente**.
    
1.  Especifique el nombre de dominio del inquilino de Azure Active Directory B2C en **Inquilino de inicio de sesión**.

1.  El campo **Autoridad** le permite controlar la dirección URL de inicio de sesión de Azure AD B2C que se usará. Establezca el valor en **<nombre_de_su_inquilino_de_b2c>. b2clogin.com**.

1. Especifique la **directiva de registro** y la **directiva de inicio de sesión** en las directivas del inquilino de B2C. Si lo desea, también puede proporcionar la **directiva de edición de perfil** y la **directiva de restablecimiento de contraseña**.

1. Una vez que especifique la configuración deseada, haga clic en **Guardar**.

    Una vez que se guardan los cambios, los desarrolladores podrán crear cuentas nuevas e iniciar sesión en el portal para desarrolladores con Azure Active Directory B2C.

## <a name="developer-portal---add-azure-ad-b2c-account-authentication"></a>Portal para desarrolladores: incorporación de autenticación de cuenta de Azure AD B2C

En el portal para desarrolladores, se puede iniciar sesión con AAD B2C utilizando el widget **Sign-in button: OAuth** (Botón de inicio de sesión: OAuth). El widget ya está incluido en la página de inicio de sesión del contenido predeterminado del portal para desarrolladores.

Aunque se creará automáticamente una nueva cuenta cada vez que un nuevo usuario inicie sesión con AAD B2C, puede considerar la idea de agregar el mismo widget a la página de registro.

El widget **Sign-up form: OAuth** (Formulario de inicio de sesión: OAuth) representa un formulario que se utiliza para registrarse con OAuth.

> [!IMPORTANT]
> Debe [volver a publicar el portal](api-management-howto-developer-portal-customize.md#publish) para que los cambios de AAD surtan efecto.

## <a name="legacy-developer-portal---how-to-sign-up-with-azure-ad-b2c"></a>Portal para desarrolladores heredado: registro con Azure AD B2C

[!INCLUDE [api-management-portal-legacy.md](../../includes/api-management-portal-legacy.md)]

1. Para registrar una cuenta de desarrollador con Azure Active Directory B2C, abra una nueva ventana del explorador y vaya al portal para desarrolladores. Haga clic en el botón **Registrarse**.

   ![Portal para desarrolladores 1][api-management-howto-aad-b2c-dev-portal]

2. Elija registrarse con **Azure Active Directory B2C**.

   ![Portal para desarrolladores 2][api-management-howto-aad-b2c-dev-portal-b2c-button]

3. Se le redirigirá a la directiva de registro que configuró en la sección anterior. Elija registrarse con la dirección de correo electrónico o una de sus cuentas sociales existentes.

   > [!NOTE]
   > Si Azure Active Directory B2C es la única opción habilitada en la pestaña **Identidades** del portal para editores, se le redirigirá a la directiva de registro de forma directa.

   ![Portal para desarrolladores][api-management-howto-aad-b2c-dev-portal-b2c-options]

   Una vez completado el registro, se le redirigirá de nuevo al portal para desarrolladores. Ahora ya inició sesión en el portal para desarrolladores de la instancia del servicio API Management.

    ![Registro completado][api-management-registration-complete]

## <a name="next-steps"></a>Pasos siguientes

*  [Introducción a Azure Active Directory B2C]
*  [Azure Active Directory B2C: Marco de directiva extensible]
*  [Uso de una cuenta Microsoft como proveedor de identidades en Azure Active Directory B2C]
*  [Uso de una cuenta de Google como proveedor de identidades en Azure Active Directory B2C]
*  [Uso de una cuenta de LinkedIn como proveedor de identidades en Azure Active Directory B2C]
*  [Uso de una cuenta de Facebook como proveedor de identidades en Azure Active Directory B2C]



[api-management-howto-add-b2c-identity-provider]: ./media/api-management-howto-aad-b2c/api-management-add-b2c-identity-provider.PNG
[api-management-howto-copy-b2c-identity-provider-redirect-url]: ./media/api-management-howto-aad-b2c/api-management-b2c-identity-provider-redirect-url.PNG
[api-management-howto-aad-b2c-portal-menu]: ./media/api-management-howto-aad-b2c/api-management-b2c-portal-menu.PNG
[api-management-howto-aad-b2c-add-button]: ./media/api-management-howto-aad-b2c/api-management-b2c-add-button.PNG
[api-management-howto-aad-b2c-app-details]: ./media/api-management-howto-aad-b2c/api-management-b2c-app-details.PNG
[api-management-howto-aad-b2c-app-created]: ./media/api-management-howto-aad-b2c/api-management-b2c-app-created.PNG
[api-management-howto-aad-b2c-app-id]: ./media/api-management-howto-aad-b2c/api-management-b2c-app-id.PNG
[api-management-howto-aad-b2c-client-id]: ./media/api-management-howto-aad-b2c/api-management-b2c-client-id.PNG
[api-management-howto-aad-b2c-app-key]: ./media/api-management-howto-aad-b2c/api-management-b2c-app-key.PNG
[api-management-howto-aad-b2c-app-key-saved]: ./media/api-management-howto-aad-b2c/api-management-b2c-app-key-saved.PNG
[api-management-howto-aad-b2c-client-secret]: ./media/api-management-howto-aad-b2c/api-management-b2c-client-secret.PNG
[api-management-howto-aad-b2c-allowed-tenant]: ./media/api-management-howto-aad-b2c/api-management-b2c-allowed-tenant.PNG
[api-management-howto-aad-b2c-policies]: ./media/api-management-howto-aad-b2c/api-management-b2c-policies.PNG
[api-management-howto-aad-b2c-dev-portal]: ./media/api-management-howto-aad-b2c/api-management-b2c-dev-portal.PNG
[api-management-howto-aad-b2c-dev-portal-b2c-button]: ./media/api-management-howto-aad-b2c/api-management-b2c-dev-portal-b2c-button.PNG
[api-management-howto-aad-b2c-dev-portal-b2c-options]: ./media/api-management-howto-aad-b2c/api-management-b2c-dev-portal-b2c-options.PNG
[api-management-complete-registration]: ./media/api-management-howto-aad/api-management-complete-registration.PNG
[api-management-registration-complete]: ./media/api-management-howto-aad/api-management-registration-complete.png

[api-management-security-external-identities]: ./media/api-management-howto-aad/api-management-b2c-security-tab.png
[api-management-security-aad-new]: ./media/api-management-howto-aad/api-management-security-aad-new.png
[api-management-new-aad-application-menu]: ./media/api-management-howto-aad/api-management-new-aad-application-menu.png
[api-management-new-aad-application-1]: ./media/api-management-howto-aad/api-management-new-aad-application-1.png
[api-management-new-aad-application-2]: ./media/api-management-howto-aad/api-management-new-aad-application-2.png
[api-management-new-aad-app-created]: ./media/api-management-howto-aad/api-management-new-aad-app-created.png
[api-management-aad-app-permissions]: ./media/api-management-howto-aad/api-management-aad-app-permissions.png
[api-management-aad-app-client-id]: ./media/api-management-howto-aad/api-management-aad-app-client-id.png
[api-management-client-id]: ./media/api-management-howto-aad/api-management-client-id.png
[api-management-aad-key-before-save]: ./media/api-management-howto-aad/api-management-aad-key-before-save.png
[api-management-aad-key-after-save]: ./media/api-management-howto-aad/api-management-aad-key-after-save.png
[api-management-client-secret]: ./media/api-management-howto-aad/api-management-client-secret.png
[api-management-client-allowed-tenants]: ./media/api-management-howto-aad/api-management-client-allowed-tenants.png
[api-management-client-allowed-tenants-save]: ./media/api-management-howto-aad/api-management-client-allowed-tenants-save.png
[api-management-aad-delegated-permissions]: ./media/api-management-howto-aad/api-management-aad-delegated-permissions.png
[api-management-dev-portal-signin]: ./media/api-management-howto-aad/api-management-dev-portal-signin.png
[api-management-aad-signin]: ./media/api-management-howto-aad/api-management-aad-signin.png
[api-management-aad-app-multi-tenant]: ./media/api-management-howto-aad/api-management-aad-app-multi-tenant.png
[api-management-aad-reply-url]: ./media/api-management-howto-aad/api-management-aad-reply-url.png
[api-management-permissions-form]: ./media/api-management-howto-aad/api-management-permissions-form.png
[api-management-configure-product]: ./media/api-management-howto-aad/api-management-configure-product.png
[api-management-add-groups]: ./media/api-management-howto-aad/api-management-add-groups.png
[api-management-select-group]: ./media/api-management-howto-aad/api-management-select-group.png
[api-management-aad-groups-list]: ./media/api-management-howto-aad/api-management-aad-groups-list.png
[api-management-aad-group-added]: ./media/api-management-howto-aad/api-management-aad-group-added.png
[api-management-groups]: ./media/api-management-howto-aad/api-management-groups.png
[api-management-edit-group]: ./media/api-management-howto-aad/api-management-edit-group.png

[How to add operations to an API]: ./mock-api-responses.md
[How to add and publish a product]: api-management-howto-add-products.md
[Monitoring and analytics]: api-management-monitoring.md
[Add APIs to a product]: api-management-howto-add-products.md#add-apis
[Publish a product]: api-management-howto-add-products.md#publish-product
[Get started with Azure API Management]: get-started-create-service-instance.md
[API Management policy reference]: ./api-management-policies.md
[Caching policies]: ./api-management-policies.md#caching-policies
[Create an API Management service instance]: get-started-create-service-instance.md

[https://oauth.net/2/]: https://oauth.net/2/
[WebApp-GraphAPI-DotNet]: https://github.com/AzureADSamples/WebApp-GraphAPI-DotNet
[Introducción a Azure Active Directory B2C]: ../active-directory-b2c/overview.md
[Procedimiento para autorizar a las cuentas de desarrollador para que usen Azure Active Directory]: ./api-management-howto-aad.md
[Azure Active Directory B2C: Marco de directiva extensible]: ../active-directory-b2c/user-flow-overview.md
[Uso de una cuenta Microsoft como proveedor de identidades en Azure Active Directory B2C]: ../active-directory-b2c/identity-provider-microsoft-account.md
[Uso de una cuenta de Google como proveedor de identidades en Azure Active Directory B2C]: ../active-directory-b2c/identity-provider-google.md
[Uso de una cuenta de Facebook como proveedor de identidades en Azure Active Directory B2C]: ../active-directory-b2c/identity-provider-facebook.md
[Uso de una cuenta de LinkedIn como proveedor de identidades en Azure Active Directory B2C]: ../active-directory-b2c/identity-provider-linkedin.md

[Prerequisites]: #prerequisites
[Configure an OAuth 2.0 authorization server in API Management]: #step1
[Configure an API to use OAuth 2.0 user authorization]: #step2
[Test the OAuth 2.0 user authorization in the Developer Portal]: #step3
[Next steps]: #next-steps

[Log in to the Developer portal using an Azure Active Directory account]: #Log-in-to-the-Developer-portal-using-an-Azure-Active-Directory-account

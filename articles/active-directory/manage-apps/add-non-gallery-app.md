---
title: 'Agregar aplicaciones que no están en la galería: plataforma de identidad de Microsoft | Microsoft Docs'
description: Agregue una aplicación que no esté en la galería a su inquilino de Azure AD
services: active-directory
author: kenwith
manager: celestedg
ms.service: active-directory
ms.subservice: app-mgmt
ms.topic: tutorial
ms.workload: identity
ms.date: 10/24/2019
ms.author: kenwith
ms.reviewer: arvinh,luleon
ms.collection: M365-identity-device-management
ms.openlocfilehash: 5db8aed0a47e7d8d928ef3287010d60efbc5e5da
ms.sourcegitcommit: 3541c9cae8a12bdf457f1383e3557eb85a9b3187
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/09/2020
ms.locfileid: "86200455"
---
# <a name="add-an-unlisted-non-gallery-application-to-your-azure-ad-organization"></a>Agregue una aplicación no enumerada (que no esté en la galería) a su organización de Azure AD

Además de las opciones de la [galería de aplicaciones de Azure AD](https://azure.microsoft.com/documentation/articles/active-directory-saas-tutorial-list/), tiene la opción de agregar una **aplicación que no sea de la galería**. Puede agregar cualquier aplicación que ya exista en su organización o cualquier aplicación de terceros de un proveedor que aún no forme parte de la galería de Azure AD. Dependiendo de su [contrato de licencia](https://azure.microsoft.com/pricing/details/active-directory/), las siguientes funcionalidades están disponibles:

- Integración de autoservicio de cualquier aplicación que admita proveedores de identidades de [Lenguaje de marcado de aserción de seguridad (SAML) 2.0](https://wikipedia.org/wiki/SAML_2.0) (iniciado por el proveedor de servicios o por el proveedor de identidades)
- Integración de autoservicio de cualquier aplicación web que tenga una página de inicio de sesión basada en HTML que use [SSO basado en contraseña](what-is-single-sign-on.md#password-based-sso)
- Conexión de autoservicio de las aplicaciones que usan el protocolo [System for Cross-Domain Identity Management (SCIM) para el aprovisionamiento de usuarios](../app-provisioning/use-scim-to-provision-users-and-groups.md)
- Capacidad para agregar vínculos a cualquier aplicación del [iniciador de aplicaciones de Office 365](https://www.microsoft.com/microsoft-365/blog/2014/10/16/organize-office-365-new-app-launcher-2/) o del [panel de acceso de Azure AD](what-is-single-sign-on.md#linked-sign-on)

En este artículo se describe cómo agregar una aplicación que no esté en la galería a las **aplicaciones empresariales** de Azure Portal sin tener que escribir código. Si por el contrario quiere obtener instrucciones para desarrolladores sobre cómo integrar aplicaciones personalizadas con Azure AD, consulte [Escenarios de autenticación para Azure AD](../develop/authentication-scenarios.md). Cuando desarrolle una aplicación que use un protocolo moderno como [OpenId Connect/OAuth](../develop/active-directory-v2-protocols.md) para autenticar a los usuarios, puede registrarla en la plataforma de identidad de Microsoft mediante la experiencia [Registros de aplicaciones](../develop/quickstart-register-app.md) en Azure Portal.

## <a name="add-a-non-gallery-application"></a>Incorporación de una aplicación ajena a la galería

1. Inicie sesión en el [portal de Azure Active Directory](https://aad.portal.azure.com/) con su cuenta de administrador de la plataforma de identidad de Microsoft.

2. Seleccione **Aplicaciones empresariales** > **Nueva aplicación**.

3. (Opcional pero recomendado) En el cuadro de búsqueda **Examinar la Galería de Azure AD**, especifique el nombre para mostrar de la aplicación. 

4. Seleccione **Cree su propia aplicación**. Se abre la página **Cree su propia aplicación**.

   ![Agregar aplicación](media/add-non-gallery-app/create-your-own-application.png)

5. Empiece a escribir el nombre para mostrar para la nueva aplicación. Si hay aplicaciones de la galería con nombres similares, aparecerán en una lista de resultados de la búsqueda.

   > [!NOTE]
   > Se recomienda usar la versión de la galería de su aplicación siempre que sea posible. Si la aplicación que quiere agregar aparece en los resultados de la búsqueda, selecciónela y omita el resto de este procedimiento.

6. En **¿Cuál es el objetivo de utilizar la aplicación?** elija **Integrar cualquier otra aplicación que no se encuentre en la galería**. Esta opción se utiliza normalmente para las aplicaciones SAML y WS-Fed.

   > [!NOTE]
   > Las otras dos opciones se usan en los escenarios siguientes:
   >* **Configurar Application Proxy para el acceso remoto seguro a una aplicación local** abre la página de configuración de Azure AD Application Proxy y los conectores.
   >* **Registrar una aplicación con la que está trabajando para integrarla con Azure AD** abre la página **Registros de aplicaciones**. Esta opción se usa normalmente para las aplicaciones de OpenID Connect.

7. Seleccione **Crear**. Se abre la página **Información general** de la aplicación.

## <a name="configure-user-sign-in-properties"></a>Configuración de las propiedades de inicio de sesión de usuario

1. Seleccione **Propiedades** para abrir el panel de propiedades para editarlas.

    ![Panel Editar propiedades](media/add-non-gallery-app/edit-properties.png)

2. Establezca las siguientes opciones para determinar cómo los usuarios que están asignados o no asignados a la aplicación pueden iniciar sesión en la aplicación y si un usuario puede ver la aplicación en el panel de acceso.

    - **Enabled for users to sign-in** (Habilitado para que los usuarios inicien sesión) determina si los usuarios asignados a la aplicación pueden iniciar sesión.
    - **Asignación de usuarios necesaria** determina si los usuarios que no están asignados a la aplicación pueden iniciar sesión.
    - **Visible to user** (Visible para el usuario) determina si los usuarios asignados a una aplicación pueden verlo en el panel de acceso y el iniciador de Office 365.

      Comportamiento para los usuarios **asignados**:

       | Propiedad de la aplicación | Propiedad de la aplicación | Propiedad de la aplicación | Experiencia del usuario asignado | Experiencia del usuario asignado |
       |---|---|---|---|---|
       | ¿Habilitado para que los usuarios inicien sesión? | ¿Se requiere la asignación de usuarios? | ¿Es visible para los usuarios? | ¿Pueden los usuarios asignados iniciar sesión? | ¿Pueden ver los usuarios asignados la aplicación?* |
       | sí | sí | sí | sí | sí  |
       | sí | sí | no  | sí | no   |
       | sí | no  | sí | sí | sí  |
       | sí | no  | no  | sí | no   |
       | no  | sí | sí | no  | no   |
       | no  | sí | no  | no  | no   |
       | no  | no  | sí | no  | no   |
       | no  | no  | no  | no  | no   |

      Comportamiento para los usuarios **no asignados**:

       | Propiedad de la aplicación | Propiedad de la aplicación | Propiedad de la aplicación | Experiencia del usuario no asignado | Experiencia del usuario no asignado |
       |---|---|---|---|---|
       | ¿Está habilitado para que los usuarios inicien sesión? | ¿Se requiere la asignación de usuarios? | ¿Es visible para los usuarios? | ¿Pueden iniciar sesión los usuarios no asignados? | ¿Pueden ver la aplicación los usuarios no asignados?* |
       | sí | sí | sí | no  | no   |
       | sí | sí | no  | no  | no   |
       | sí | no  | sí | sí | no   |
       | sí | no  | no  | sí | no   |
       | no  | sí | sí | no  | no   |
       | no  | sí | no  | no  | no   |
       | no  | no  | sí | no  | no   |
       | no  | no  | no  | no  | no   |

     ¿*Puede el usuario ver la aplicación en el panel de acceso y el iniciador de aplicaciones de Office 365?

3. Para usar un logotipo personalizado, cree uno de 215 x 215 píxeles y guárdelo en formato PNG. A continuación, vaya a su logotipo y cárguelo.

    ![Cambio del logotipo](media/add-non-gallery-app/change-logo.png)

4. Cuando haya terminado, seleccione **Guardar**.

## <a name="next-steps"></a>Pasos siguientes

Ahora que ha agregado la aplicación a la organización de Azure AD, [elija el método de inicio de sesión único](what-is-single-sign-on.md#choosing-a-single-sign-on-method) que quiera usar y consulte a continuación el artículo adecuado:

- [Configuración del inicio de sesión único basado en SAML](configure-single-sign-on-non-gallery-applications.md)
- [Configuración del inicio de sesión único con contraseña](configure-password-single-sign-on-non-gallery-applications.md)
- [Configuración del inicio de sesión vinculado](configure-linked-sign-on.md)

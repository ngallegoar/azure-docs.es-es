---
title: 'Inicio rápido: Configuración del inicio de sesión único (SSO) de una aplicación en el inquilino de Azure Active Directory (Azure AD)'
description: En este inicio rápido se describe el proceso de configuración del inicio de sesión único (SSO) de una aplicación en el inquilino de Azure Active Directory (Azure AD).
services: active-directory
author: kenwith
manager: celestedg
ms.service: active-directory
ms.subservice: app-mgmt
ms.topic: quickstart
ms.workload: identity
ms.date: 07/01/2020
ms.author: kenwith
ms.collection: M365-identity-device-management
ms.openlocfilehash: f90edf525a8deb35376d6637090e7b424b2fd658
ms.sourcegitcommit: e132633b9c3a53b3ead101ea2711570e60d67b83
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/07/2020
ms.locfileid: "86038997"
---
# <a name="quickstart-set-up-single-sign-on-sso-for-an-application-in-your-azure-active-directory-azure-ad-tenant"></a>Inicio rápido: Configuración del inicio de sesión único (SSO) de una aplicación en el inquilino de Azure Active Directory (Azure AD)

Comience a usar inicios de sesión de usuario simplificados mediante la configuración del inicio de sesión único de una aplicación que haya agregado a su inquilino de Azure AD. Después de configurar el inicio de sesión único, los usuarios podrán iniciar sesión en una aplicación con sus credenciales de Azure AD. El inicio de sesión único se incluye en la edición gratuita de Azure AD.

## <a name="prerequisites"></a>Requisitos previos

Para configurar el inicio de sesión único de una aplicación que ha agregado a su inquilino de Azure AD, necesita:

- Una cuenta de Azure con una suscripción activa. [Cree una cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
- Uno de los siguientes roles: Administrador global, Administrador de aplicaciones en la nube, Administrador de aplicaciones o Propietario de la entidad de servicio.
- Una aplicación que admita el inicio de sesión único y que se haya configurado previamente y agregado ya a la galería de Azure AD. La mayoría de las aplicaciones pueden usar Azure AD para el inicio de sesión único. Las aplicaciones de la galería de Azure AD se han configurado previamente. Aunque la aplicación no aparezca en la lista, o se trate de una aplicación de desarrollado personalizado, puede usarla con Azure AD. Consulte los tutoriales y otra documentación adicional en la tabla de contenido. Este inicio rápido se centra en las aplicaciones que se han configurado previamente para el inicio de sesión único y que los desarrolladores de aplicaciones han agregado a la galería de Azure AD.
- (Opcional: finalización de [Visualización de sus aplicaciones](view-applications-portal.md)).
- (Opcional: finalización de [Adición de una aplicación](add-application-portal.md)).
- (Opcional: finalización de [Configuración de una aplicación](add-application-portal-configure.md)).


>[!IMPORTANT]
>Para probar los pasos de este inicio rápido, se recomienda usar un entorno que no sea de producción.


## <a name="enable-single-sign-on-for-an-app"></a>Habilitar el inicio de sesión único de una aplicación

Cuando termine de agregar una aplicación al inquilino de Azure AD, se le mostrará inmediatamente la página de información general correspondiente. Si va a configurar una aplicación que ya se ha agregado, examine el primer inicio rápido para ver las aplicaciones que se han agregado a su inquilino. 

Para configurar el inicio de sesión único de una aplicación:

1. En el portal de Azure AD, seleccione **Aplicaciones empresariales** y, a continuación, busque y seleccione la aplicación que desea configurar para el inicio de sesión único.
2. En la sección Administrar, seleccione **Inicio de sesión único** para abrir el panel de propiedades para editarlo.
    :::image type="content" source="media/add-application-portal-setup-sso/configure-sso.png" alt-text="Muestra la página de configuración del inicio de sesión único en el portal de Azure AD":::.
3. Seleccione SAML para abrir la página de configuración del inicio de sesión único. En este ejemplo, la aplicación que se está configurando para el inicio de sesión único es GitHub. Después de configurar GitHub, los usuarios podrán iniciar sesión en GitHub con sus credenciales desde su inquilino de Azure AD.
    :::image type="content" source="media/add-application-portal-setup-sso/github-sso.png" alt-text="Muestra la página de configuración del inicio de sesión único en GitHub":::.
4. El proceso para configurar una aplicación para que use Azure AD para el inicio de sesión único basado en SAML varía en función de la aplicación. Observe que hay un vínculo a la guía de GitHub. Puede encontrar guías para otras aplicaciones en: https://docs.microsoft.com/azure/active-directory/saas-apps/
5. Siga la guía para configurar el inicio de sesión único de la aplicación. Muchas aplicaciones tienen requisitos de suscripción específicos para la funcionalidad de inicio de sesión único. Por ejemplo, GitHub requiere una suscripción Enterprise.
    :::image type="content" source="media/add-application-portal-setup-sso/github-pricing.png" alt-text="Muestra la opción de inicio de sesión único en la suscripción Enterprise de la página de precios de GitHub":::.


## <a name="next-steps"></a>Pasos siguientes

- [Eliminación de una aplicación](delete-application-portal.md)

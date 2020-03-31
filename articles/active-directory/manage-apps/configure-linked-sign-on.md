---
title: 'Inicio de sesión vinculado para aplicaciones de Azure AD: plataforma de identidad de Microsoft'
description: Configure el inicio de sesión vinculado (SSO) en las aplicaciones empresariales de Azure AD de la plataforma de identidad de Microsoft (Azure AD)
services: active-directory
author: msmimart
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.topic: article
ms.workload: identity
ms.date: 05/08/2019
ms.author: mimart
ms.reviewer: arvinh,luleon
ms.collection: M365-identity-device-management
ms.openlocfilehash: dfe4aeb17f482cc9d4126efc6d65d3f7d173536b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "77063550"
---
# <a name="configure-linked-sign-on"></a>Configuración del inicio de sesión vinculado

Al agregar una aplicación web de la galería o que no sea de la galería, una de las opciones de inicio de sesión único disponibles es el [inicio de sesión vinculado](what-is-single-sign-on.md). Seleccione esta opción si desea agregar un vínculo a una aplicación en el panel de acceso de Azure AD o en el portal de Office 365 de la organización. Puede usar este método para agregar vínculos a aplicaciones web personalizadas que actualmente utilizan los Servicios de federación de Active Directory (u otro servicio de federación), en lugar de Azure AD para la autenticación. O bien, puede agregar vínculos profundos a páginas específicas de SharePoint o a otras páginas web que desee que aparezcan en los paneles de acceso de su usuario.

## <a name="before-you-begin"></a>Antes de empezar

Si la aplicación no se ha agregado a su inquilino de Azure AD, consulte [Incorporación de una aplicación de la galería](add-gallery-app.md) o [Incorporación de una aplicación que no es de la galería](add-non-gallery-app.md).

### <a name="open-the-app-and-select-linked-sign-on"></a>Abrir la aplicación y seleccionar inicio de sesión vinculado

1. Inicie sesión en [Azure Portal](https://portal.azure.com) como administrador de aplicaciones en la nube o administrador de aplicaciones para el inquilino de Azure AD.

1. Vaya a **Azure Active Directory** > **Aplicaciones empresariales**. Se mostrará una muestra aleatoria de las aplicaciones en su inquilino de Azure AD. 

1. En el menú **Tipo de aplicación**, seleccione **Todas las aplicaciones** y, después, **Aplicar**.

1. Escriba el nombre de la aplicación en el cuadro de búsqueda y seleccione la aplicación en los resultados.

1. En la sección **Administrar**, seleccione **Inicio de sesión único**. 

1. Seleccione **Vinculado**.

1. Escriba la dirección URL de la aplicación con la que va a realizar la vinculación. Escriba la dirección URL y seleccione **Guardar**. 
 
1. Puede asignar usuarios y grupos a la aplicación, lo que hace que la aplicación aparezca en el [iniciador de aplicaciones de Office 365](https://blogs.office.com/2014/10/16/organize-office-365-new-app-launcher-2/) o en el [panel de acceso de Azure AD](end-user-experiences.md) de dichos usuarios.

1. Seleccione **Guardar**.

## <a name="next-steps"></a>Pasos siguientes

- [Asignación de usuarios y grupos a la aplicación](methods-for-assigning-users-and-groups.md)
- [Configuración del aprovisionamiento automático de cuentas de usuario](../app-provisioning/configure-automatic-user-provisioning-portal.md)

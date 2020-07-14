---
title: 'Inicio rápido: Configuración de las propiedades de una aplicación en el inquilino de Azure Active Directory (Azure AD)'
description: Este inicio rápido usa Azure Portal para configurar una aplicación que se ha registrado con su inquilino de Azure Active Directory (Azure AD).
services: active-directory
author: kenwith
manager: celestedg
ms.service: active-directory
ms.subservice: app-mgmt
ms.topic: quickstart
ms.workload: identity
ms.date: 10/29/2019
ms.author: kenwith
ms.collection: M365-identity-device-management
ms.openlocfilehash: 82a3310f6fc2169a515b4b13d81c88d187bd0f9c
ms.sourcegitcommit: 845a55e6c391c79d2c1585ac1625ea7dc953ea89
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2020
ms.locfileid: "85956159"
---
# <a name="quickstart-configure-properties-for-an-application-in-your-azure-active-directory-azure-ad-tenant"></a>Inicio rápido: Configuración de las propiedades de una aplicación en el inquilino de Azure Active Directory (Azure AD)

En el inicio rápido anterior, agregó una aplicación a su inquilino de Azure AD. Cuando se agrega una aplicación, se permite que el inquilino de Azure AD sepa que es el proveedor de identidades de la aplicación. Ahora configurará algunas de las propiedades de la aplicación.
 
## <a name="prerequisites"></a>Requisitos previos

Para configurar las propiedades de una aplicación en el inquilino de Azure AD, necesita:

- Una cuenta de Azure con una suscripción activa. [Cree una cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
- Uno de los siguientes roles: Administrador global, Administrador de aplicaciones en la nube, Administrador de aplicaciones o Propietario de la entidad de servicio.
- (Opcional: finalización de [Visualización de sus aplicaciones](view-applications-portal.md)).
- (Opcional: finalización de [Adición de una aplicación](add-application-portal.md)).

>[!IMPORTANT]
>Para probar los pasos de este inicio rápido, se recomienda usar un entorno que no sea de producción.

## <a name="configure-app-properties"></a>Configuración de las propiedades de una aplicación

Cuando termine de agregar una aplicación al inquilino de Azure AD, se le mostrará inmediatamente la página de información general correspondiente. Si va a configurar una aplicación que ya se ha agregado, examine el primer inicio rápido para ver las aplicaciones que se han agregado a su inquilino. 

Para editar las propiedades de la aplicación:

1. En el portal de Azure AD, seleccione **Aplicaciones empresariales** y, a continuación, busque y seleccione la aplicación que desea configurar.
2. En la sección Administrar, seleccione **Propiedades** para abrir el panel de propiedades para editarlo.
    ![Muestra la pantalla de propiedades y las propiedades de la aplicación editables](media/add-application-portal/edit-properties.png).
3. Dedique un momento a comprender las opciones disponibles para su configuración.
    - La opción **¿Habilitado para que los usuarios inicien sesión?** determina si los usuarios asignados a la aplicación pueden iniciar sesión.
    - La opción **¿Asignación de usuarios?** determina si los usuarios que no están asignados a la aplicación pueden iniciar sesión.
    - La opción **¿Es visible para los usuarios?** determina si los usuarios asignados a una aplicación pueden verla en el panel de acceso (https://myapps.microsoft.com) y en el iniciador de aplicaciones de Office 365 (el menú de gofres de la parte superior izquierda de un sitio web de Office 365 o Microsoft 365).
4. Use las siguientes tablas para ayudarle a elegir las opciones que más se ajusten a sus necesidades.

   - Comportamiento para los usuarios **asignados**:

       | Propiedad de la aplicación | Propiedad de la aplicación | Propiedad de la aplicación | Experiencia del usuario asignado | Experiencia del usuario asignado |
       |---|---|---|---|---|
       | ¿Está habilitado para que los usuarios inicien sesión? | ¿Se requiere la asignación de usuarios? | ¿Es visible para los usuarios? | ¿Pueden los usuarios asignados iniciar sesión? | ¿Pueden ver los usuarios asignados la aplicación?* |
       | sí | sí | sí | sí | sí  |
       | sí | sí | no  | sí | no   |
       | sí | no  | sí | sí | sí  |
       | sí | no  | no  | sí | no   |
       | no  | sí | sí | no  | no   |
       | no  | sí | no  | no  | no   |
       | no  | no  | sí | no  | no   |
       | no  | no  | no  | no  | no   |

   - Comportamiento para los usuarios **no asignados**:

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

## <a name="use-a-custom-logo"></a>Uso de un logotipo personalizado

Para usar un logotipo personalizado:

1. Cree un logotipo de 215 por 215 píxeles y guárdelo en formato PNG.
2. En el portal de Azure AD, seleccione **Aplicaciones empresariales** y, a continuación, busque y seleccione la aplicación que desea configurar.
3. En la sección Administrar, seleccione **Propiedades** para abrir el panel de propiedades para editarlo. 
4. Seleccione el icono para cargar el logotipo.
5. Cuando haya terminado, seleccione **Guardar**. 
    ![Muestra cómo cambiar el logotipo desde la página Propiedades de la aplicación](media/add-application-portal/change-logo.png).

   > [!NOTE]
   > La miniatura que se muestra en este panel de **Propiedades** no se actualiza inmediatamente. Puede cerrar y volver a abrir las propiedades para ver el icono actualizado.

## <a name="next-steps"></a>Pasos siguientes

Ahora que ha configurado las propiedades de una aplicación puede continuar con la configuración del inicio de sesión único.

- [Configuración del inicio de sesión único](add-application-portal-setup-sso.md)
- [Eliminación de una aplicación](delete-application-portal.md)
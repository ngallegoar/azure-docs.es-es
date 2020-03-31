---
title: Incorporación o eliminación de asignaciones de roles para usuarios externos con RBAC y Azure Portal
description: Aprenda a conceder acceso a recursos de Azure para los usuarios externos a una organización mediante el control de acceso basado en rol (RBAC) de Azure.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.assetid: ''
ms.service: role-based-access-control
ms.devlang: ''
ms.topic: conceptual
ms.tgt_pltfrm: ''
ms.workload: identity
ms.date: 11/25/2019
ms.author: rolyon
ms.reviewer: skwan
ms.custom: it-pro
ms.openlocfilehash: 1c440b85f792ac5bb1336f4d20f930aafc38ad7d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "79225368"
---
# <a name="add-or-remove-role-assignments-for-external-guest-users-using-azure-rbac-and-the-azure-portal"></a>Incorporación o eliminación de asignaciones de roles para usuarios invitados externos con RBAC de Azure y Azure Portal

El [control de acceso basado en rol (RBAC) de Azure](overview.md) permite una mejor administración de la seguridad para organizaciones grandes y para PYMES que trabajan con colaboradores externos, proveedores o autónomos que necesitan tener acceso a recursos específicos de su entorno, pero no necesariamente a toda la infraestructura ni a los ámbitos relacionados con la facturación. Puede usar las funcionalidades de [Azure Active Directory B2B](../active-directory/b2b/what-is-b2b.md) para colaborar con usuarios invitados externos y puede usar RBAC para conceder solo los permisos que los usuarios invitados necesitan en su entorno.

## <a name="prerequisites"></a>Prerrequisitos

Para agregar o quitar asignaciones de roles, debe tener:

- Permisos `Microsoft.Authorization/roleAssignments/write` y `Microsoft.Authorization/roleAssignments/delete`, como [Administrador de acceso de usuarios](built-in-roles.md#user-access-administrator) o [propietario](built-in-roles.md#owner)

## <a name="when-would-you-invite-guest-users"></a>¿Cuándo invitará a los usuarios invitados?

A continuación se describen algunos escenarios de ejemplo en los que podría invitar a usuarios invitados a su organización y concederles permisos:

- Permitir que un proveedor externo por cuenta propia que solo tiene una cuenta de correo electrónico acceda a los recursos de Azure para la realización de un proyecto
- Permitir que un asociado externo administre determinados recursos o una suscripción completa.
- Permitir que los ingenieros del servicio de soporte técnico que no están en su organización (por ejemplo, el soporte técnico de Microsoft) accedan temporalmente a los recursos de Azure para solucionar problemas

## <a name="permission-differences-between-member-users-and-guest-users"></a>Diferencias de permisos entre usuarios miembros y usuarios invitados

Los miembros nativos de un directorio (usuarios miembros) tienen permisos diferentes que los usuarios invitados de otro directorio como invitado de colaboración B2B (usuarios invitados). Por ejemplo, los usuarios miembros puede leer casi toda la información del directorio, mientras que los usuarios invitados tienen permisos de directorio restringidos. Para más información sobre los usuarios miembros y los usuarios invitados, consulte [¿Cuales son los permisos de usuario predeterminados en Azure Active Directory?](../active-directory/fundamentals/users-default-permissions.md).

## <a name="add-a-guest-user-to-your-directory"></a>Adición de un usuario invitado a su directorio

Siga estos pasos para agregar un usuario invitado a su directorio mediante la página de Azure Active Directory.

1. Asegúrese de que la configuración de colaboración externa de la organización se configure de forma que le permita invitar a otros usuarios. Para más información, consulte [Habilitación de la colaboración externa B2B y administración de quién puede invitar a otros usuarios](../active-directory/b2b/delegate-invitations.md).

1. En Azure Portal, haga clic en **Azure Active Directory** > **Usuarios** > **Nuevo usuario invitado**.

    ![Característica Nuevo usuario invitado en Azure Portal](./media/role-assignments-external-users/invite-guest-user.png)

1. Siga los pasos para agregar un nuevo usuario invitado. Para más información, consulte [Incorporación de usuarios de colaboración B2B de Azure Active Directory en Azure Portal](../active-directory/b2b/add-users-administrator.md#add-guest-users-to-the-directory).

Después de agregar un usuario invitado al directorio, puede enviarle un vínculo directo a una aplicación compartida o bien, el propio usuario invitado puede hacer clic en la dirección URL de canje del correo electrónico de invitación.

![Correo electrónico de invitación de usuario invitado](./media/role-assignments-external-users/invite-email.png)

Para que el usuario invitado pueda acceder a su directorio, debe completar el proceso de invitación.

![Revisión de los permisos de la invitación del usuario invitado](./media/role-assignments-external-users/invite-review-permissions.png)

Para más información sobre el proceso de invitación, consulte [Canje de invitación de colaboración B2B de Azure Active Directory](../active-directory/b2b/redemption-experience.md).

## <a name="add-a-role-assignment-for-a-guest-user"></a>Adición de una asignación de roles para un usuario invitado

En RBAC, para conceder acceso es preciso asignar un rol. Para agregar una asignación de roles para un usuario invitado, siga los [mismos pasos](role-assignments-portal.md#add-a-role-assignment) que para un usuario miembro, un grupo, una entidad de servicio o una identidad administrada. Siga estos pasos para agregar una asignación de roles a un usuario invitado en distintos ámbitos.

1. En Azure Portal, haga clic en **Todos los servicios**.

1.  Seleccione el conjunto de recursos al que se aplica el acceso, también conocido como el ámbito. Por ejemplo, puede seleccionar **Grupos de administración**, **Suscripciones**, **Grupos de recursos** o un recurso.

1. Haga clic en el recurso específico.

1. Haga clic en **Control de acceso (IAM).**

    En la siguiente captura de pantalla se muestra un ejemplo de la hoja Control de acceso (IAM) de un grupo de recursos. Si realiza algún cambio en el control de acceso aquí, solo se aplicará al grupo de recursos.

    ![Hoja Control de acceso (IAM) para un grupo de recursos](./media/role-assignments-external-users/access-control-resource-group.png)

1. Haga clic en la pestaña **Asignaciones de roles** para ver todas las asignaciones de roles en este ámbito.

1. Haga clic en **Agregar** > **Agregar asignación de roles** para abrir el panel Agregar asignación de roles.

    Si no tiene permisos para asignar roles, la opción Agregar asignación de roles se deshabilitará.

    ![Menú Agregar](./media/role-assignments-external-users/add-menu.png)

1. En la lista desplegable **Rol**, seleccione un rol como **Colaborador de la máquina virtual**.

1. En la lista **Seleccionar**, seleccione el usuario invitado. Si no ve al usuario en la lista, puede escribir en el cuadro **Seleccionar** para buscar en el directorio por nombres para mostrar, direcciones de correo electrónico o identificadores de objeto.

   ![Panel Agregar asignación de roles](./media/role-assignments-external-users/add-role-assignment.png)

1. Haga clic en **Guardar** para asignar el rol en el ámbito seleccionado.

    ![Asignación del rol Colaborador de la máquina virtual](./media/role-assignments-external-users/access-control-role-assignments.png)

## <a name="add-a-role-assignment-for-a-guest-user-not-yet-in-your-directory"></a>Adición de una asignación de roles para un usuario invitado que todavía no está en el directorio

Para agregar una asignación de roles para un usuario invitado, siga los [mismos pasos](role-assignments-portal.md#add-a-role-assignment) que para un usuario miembro, un grupo, una entidad de servicio o una identidad administrada.

Si el usuario invitado todavía no está en el directorio, puede invitar al usuario directamente desde el panel Agregar asignación de roles.

1. En Azure Portal, haga clic en **Todos los servicios**.

1.  Seleccione el conjunto de recursos al que se aplica el acceso, también conocido como el ámbito. Por ejemplo, puede seleccionar **Grupos de administración**, **Suscripciones**, **Grupos de recursos** o un recurso.

1. Haga clic en el recurso específico.

1. Haga clic en **Control de acceso (IAM).**

1. Haga clic en la pestaña **Asignaciones de roles** para ver todas las asignaciones de roles en este ámbito.

1. Haga clic en **Agregar** > **Agregar asignación de roles** para abrir el panel Agregar asignación de roles.

    ![Menú Agregar](./media/role-assignments-external-users/add-menu.png)

1. En la lista desplegable **Rol**, seleccione un rol como **Colaborador de la máquina virtual**.

1. En la lista **Seleccionar**, escriba la dirección de correo electrónico de la persona a la que desea invitar y seleccione esa persona.

   ![Invitar a usuarios invitados en el panel Agregar asignación de roles](./media/role-assignments-external-users/add-role-assignment-new-guest.png)

1. Haga clic en **Guardar** para agregar el usuario invitado al directorio, asignar el rol y enviar una invitación.

    Transcurridos unos instantes, verá una notificación de la asignación de roles e información sobre la invitación.

    ![Asignación de roles y notificación de usuario invitado](./media/role-assignments-external-users/invited-user-notification.png)

1. Para invitar manualmente al usuario invitado, haga clic con el botón derecho y copie el vínculo de invitación de la notificación. No haga clic en el vínculo de invitación porque eso inicia el proceso de invitación.

    El vínculo de invitación tendrá el siguiente formato:

    `https://invitations.microsoft.com/redeem/...`

1. Envíe el vínculo de invitación al usuario invitado para completar el proceso de invitación.

    Para más información sobre el proceso de invitación, consulte [Canje de invitación de colaboración B2B de Azure Active Directory](../active-directory/b2b/redemption-experience.md).

## <a name="remove-a-guest-user-from-your-directory"></a>Eliminación de un usuario invitado del directorio

Antes de eliminar un usuario invitado de un directorio, primero debe eliminar las asignaciones de roles de dicho usuario invitado. Siga estos pasos para eliminar un usuario invitado de un directorio.

1. Abra **Control de acceso (IAM)** en un ámbito como, por ejemplo, grupo de administración, suscripción, grupo de recursos o recurso, en el que el usuario invitado tenga una asignación de roles.

1. Haga clic en la pestaña **Asignaciones de roles** para ver todas las asignaciones de roles.

1. En la lista de asignaciones de roles, agregue una marca de verificación junto al usuario invitado con la asignación de roles que desea eliminar.

   ![Eliminar asignación de roles](./media/role-assignments-external-users/remove-role-assignment-select.png)

1. Haga clic en **Quitar**.

   ![Mensaje de eliminación de asignación de roles](./media/role-assignments-external-users/remove-role-assignment.png)

1. En el mensaje de eliminación de asignación de roles que aparece, haga clic en **Sí**.

1. En la barra de navegación izquierda, haga clic en **Azure Active Directory** > **Usuarios**.

1. Haga clic en el usuario invitado que desea eliminar.

1. Haga clic en **Eliminar**.

   ![Eliminar usuario invitado](./media/role-assignments-external-users/delete-guest-user.png)

1. En el mensaje de eliminación que aparece, haga clic en **Sí**.

## <a name="troubleshoot"></a>Solución de problemas

### <a name="guest-user-cannot-browse-the-directory"></a>El usuario invitado no puede examinar el directorio

Los usuarios invitados tienen permisos de directorio restringidos. Por ejemplo, los usuarios invitados no pueden examinar el directorio y no pueden buscar grupos o aplicaciones. Para más información, consulte [¿Cuáles son los permisos de usuario predeterminados en Azure Active Directory?](../active-directory/fundamentals/users-default-permissions.md).

![El usuario invitado no puede examinar los usuarios de un directorio](./media/role-assignments-external-users/directory-no-users.png)

Si un usuario invitado necesita privilegios adicionales en el directorio, puede asignar un rol de directorio al usuario invitado. Si realmente desea que un usuario invitado tenga acceso de lectura completo al directorio, puede agregar el usuario invitado al rol [Lectores de directorio](../active-directory/users-groups-roles/directory-assign-admin-roles.md) en Azure AD. Para más información, consulte [Concesión de permisos a los usuarios de organizaciones asociadas en el inquilino de Azure Active Directory](../active-directory/b2b/add-guest-to-role.md).

![Asignar el rol Lectores de directorio](./media/role-assignments-external-users/directory-roles.png)

### <a name="guest-user-cannot-browse-users-groups-or-service-principals-to-assign-roles"></a>El usuario invitado no puede examinar usuarios, grupos o entidades de servicio para asignar roles

Los usuarios invitados tienen permisos de directorio restringidos. Incluso si un usuario invitado es [Propietario](built-in-roles.md#owner) en un ámbito, si intenta agregar una asignación de roles para conceder acceso a otra persona, no podrá examinar la lista de usuarios, grupos o entidades de servicio.

![El usuario invitado no puede examinar las entidades de seguridad para asignar roles](./media/role-assignments-external-users/directory-no-browse.png)

Si el usuario invitado conoce el nombre de inicio de sesión exacto de alguien del directorio, puede conceder acceso. Si realmente desea que un usuario invitado tenga acceso de lectura completo al directorio, puede agregar el usuario invitado al rol [Lectores de directorio](../active-directory/users-groups-roles/directory-assign-admin-roles.md) en Azure AD. Para más información, consulte [Concesión de permisos a los usuarios de organizaciones asociadas en el inquilino de Azure Active Directory](../active-directory/b2b/add-guest-to-role.md).

### <a name="guest-user-cannot-register-applications-or-create-service-principals"></a>El usuario invitado no puede registrar aplicaciones ni crear entidades de servicio

Los usuarios invitados tienen permisos de directorio restringidos. Si un usuario invitado necesita poder registrar aplicaciones o crear entidades de servicio, puede agregar el usuario invitado al rol [Desarrollador de aplicaciones](../active-directory/users-groups-roles/directory-assign-admin-roles.md) en Azure AD. Para más información, consulte [Concesión de permisos a los usuarios de organizaciones asociadas en el inquilino de Azure Active Directory](../active-directory/b2b/add-guest-to-role.md).

![El usuario invitado no puede registrar aplicaciones](./media/role-assignments-external-users/directory-access-denied.png)

### <a name="guest-user-does-not-see-the-new-directory"></a>El usuario invitado no ve el nuevo directorio

Si a un usuario invitado se le ha concedido acceso a un directorio, pero no ve el nuevo directorio enumerado en Azure Portal cuando intenta cambiar en su panel **Directorio y suscripción**, asegúrese de que el usuario invitado ha completado el proceso de invitación. Para más información sobre el proceso de invitación, consulte [Canje de invitación de colaboración B2B de Azure Active Directory](../active-directory/b2b/redemption-experience.md).

### <a name="guest-user-does-not-see-resources"></a>El usuario invitado no ve los recursos

Si a un usuario invitado se le ha concedido acceso a un directorio, pero no ve los recursos a los que se le ha concedido acceso en Azure Portal, asegúrese de que el usuario invitado ha seleccionado el directorio correcto. Un usuario invitado podría tener acceso a varios directorios. Para cambiar de directorio, en la parte superior izquierda, haga clic en **Directorio y suscripción** y, a continuación, haga clic en el directorio adecuado.

![Panel Directorios y suscripciones de Azure Portal](./media/role-assignments-external-users/directory-subscription.png)

## <a name="next-steps"></a>Pasos siguientes

- [Incorporación de usuarios de colaboración B2B de Azure Active Directory en Azure Portal](../active-directory/b2b/add-users-administrator.md)
- [Propiedades de un usuario de colaboración B2B de Azure Active Directory](../active-directory/b2b/user-properties.md)
- [Elementos del correo electrónico de invitación para la colaboración B2B: Azure Active Directory](../active-directory/b2b/invitation-email-elements.md)
- [Adición de un usuario invitado como coadministrador](classic-administrators.md#add-a-guest-user-as-a-co-administrator)
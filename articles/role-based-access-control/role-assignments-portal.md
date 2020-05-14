---
title: 'Incorporación o eliminación de asignaciones de roles de Azure mediante Azure Portal: RBAC de Azure'
description: Aprenda a conceder acceso a recursos de Azure para usuarios, grupos, entidades de servicio e identidades administradas mediante Azure Portal y el control de acceso basado en roles de Azure (RBAC de Azure).
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.assetid: 8078f366-a2c4-4fbb-a44b-fc39fd89df81
ms.service: role-based-access-control
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 01/25/2020
ms.author: rolyon
ms.reviewer: bagovind
ms.openlocfilehash: 88cb1e9893e0ca058e57ce12f00747a59248fb19
ms.sourcegitcommit: 4499035f03e7a8fb40f5cff616eb01753b986278
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/03/2020
ms.locfileid: "82735698"
---
# <a name="add-or-remove-azure-role-assignments-using-the-azure-portal"></a>Incorporación o eliminación de asignaciones de roles de Azure mediante Azure Portal

[!INCLUDE [Azure RBAC definition grant access](../../includes/role-based-access-control-definition-grant.md)] En este artículo se describe cómo asignar roles con Azure Portal.

Si tiene que asignar roles de Administrador en Azure Active Directory, consulte [Ver y asignar roles de administrador en Azure Active Directory](../active-directory/users-groups-roles/directory-manage-roles-portal.md).

## <a name="prerequisites"></a>Prerrequisitos

Para agregar o quitar asignaciones de roles, debe tener:

- Permisos `Microsoft.Authorization/roleAssignments/write` y `Microsoft.Authorization/roleAssignments/delete`, como [Administrador de acceso de usuarios](built-in-roles.md#user-access-administrator) o [propietario](built-in-roles.md#owner)

## <a name="access-control-iam"></a>Control de acceso (IAM)

**Control de acceso (IAM)** es la hoja que se usa para asignar roles y conceder acceso a los recursos de Azure. También se conoce como administración de identidades y acceso y aparece en varias ubicaciones en Azure Portal. A continuación se muestra un ejemplo de la hoja Control de acceso (IAM) de una suscripción.

![Hoja Control de acceso (IAM) para una suscripción](./media/role-assignments-portal/access-control-subscription.png)

Para ser más eficaz con la hoja Control de acceso (IAM), es útil si puede responder las siguientes tres preguntas cuando intente asignar un rol:

1. **¿Quién necesita acceso?**

    Quién se refiere a un usuario, grupo, entidad de servicio o identidad administrada. Esto también se denomina una *entidad de seguridad*.

1. **¿Qué rol necesitan?**

    Los permisos se agrupan en roles. Puede seleccionarlos de una lista de varios [roles integrados](built-in-roles.md) o utilizar sus propios roles personalizados.

1. **¿Dónde necesitan acceso?**

    Dónde se refiere al conjunto de recursos al que se aplica el acceso. Dónde puede ser un grupo de administración, una suscripción, un grupo de recursos o un único recurso, como una cuenta de almacenamiento. Esto se denomina el *ámbito*.

## <a name="add-a-role-assignment"></a>Adición de una asignación de roles

En RBAC de Azure, para conceder acceso a un recurso de Azure, se agrega una asignación de roles. Siga estos pasos para asignar un rol.

1. En Azure Portal, haga clic en **Todos los servicios** y luego seleccione el ámbito al que quiere conceder acceso. Por ejemplo, puede seleccionar **Grupos de administración**, **Suscripciones**, **Grupos de recursos** o un recurso.

1. Haga clic en el recurso específico de ese ámbito.

1. Haga clic en **Control de acceso (IAM).**

1. Haga clic en la pestaña **Asignaciones de roles** para ver todas las asignaciones de roles en este ámbito.

    ![Control de acceso (IAM) y la pestaña Asignaciones de roles](./media/role-assignments-portal/role-assignments.png)

1. Haga clic en **Agregar** > **Agregar asignación de roles**.

   Si no tiene permisos para asignar roles, la opción Agregar asignación de roles se deshabilitará.

   ![Menú Agregar](./media/role-assignments-portal/add-menu.png)

    Se abre el panel Agregar asignación de roles.

   ![Panel Agregar asignación de roles](./media/role-assignments-portal/add-role-assignment.png)

1. En la lista desplegable **Rol**, seleccione un rol como **Colaborador de la máquina virtual**.

1. En la lista **Seleccionar**, seleccione un usuario, grupo, entidad de servicio o identidad administrada. Si no ve la entidad de seguridad en la lista, puede escribir en el cuadro **Seleccionar** para buscar nombres para mostrar, direcciones de correo electrónico e identificadores de objeto en el directorio.

1. Haga clic en **Guardar** para asignar el rol.

   Transcurridos unos instantes, se asigna el rol a la entidad de seguridad en el ámbito seleccionado.

    ![Agregar asignación de roles guardado](./media/role-assignments-portal/add-role-assignment-save.png)

## <a name="assign-a-user-as-an-administrator-of-a-subscription"></a>Asignación de un usuario como administrador de una suscripción

Para que un usuario sea administrador de una suscripción de Azure, asígnele el rol [Propietario](built-in-roles.md#owner) en el ámbito de la suscripción. El rol Propietario da al usuario acceso total a todos los recursos de la suscripción, incluido el derecho a conceder acceso a otros. Estos pasos son los mismos que para la asignación de cualquier otro rol.

1. En Azure Portal, haga clic en **Todos los servicios** y luego en **Suscripciones**.

1. Haga clic en la suscripción a la que quiere conceder acceso.

1. Haga clic en **Control de acceso (IAM).**

1. Haga clic en la pestaña **Asignaciones de roles** para ver todas las asignaciones de roles de esta suscripción.

    ![Control de acceso (IAM) y la pestaña Asignaciones de roles](./media/role-assignments-portal/role-assignments.png)

1. Haga clic en **Agregar** > **Agregar asignación de roles**.

   Si no tiene permisos para asignar roles, la opción Agregar asignación de roles se deshabilitará.

   ![Menú Agregar](./media/role-assignments-portal/add-menu.png)

    Se abre el panel Agregar asignación de roles.

   ![Panel Agregar asignación de roles](./media/role-assignments-portal/add-role-assignment.png)

1. En la lista desplegable **Rol**, seleccione el rol **Propietario**.

1. En la lista **Seleccionar**, seleccione un usuario. Si no ve el usuario en la lista, puede escribir en el cuadro **Seleccionar** para buscar nombres para mostrar y direcciones de correo electrónico en el directorio.

1. Haga clic en **Guardar** para asignar el rol.

   Transcurridos unos instantes, al usuario se le asigna el rol Propietario en el ámbito de la suscripción.

## <a name="remove-a-role-assignment"></a>Eliminación de una asignación de rol

En RBAC de Azure, para quitar el acceso de un recurso de Azure, se quita una asignación de roles. Siga estos pasos para eliminar una asignación de roles.

1. Abra **Control de acceso (IAM)** en un ámbito como, por ejemplo, grupo de administración, suscripción, grupo de recursos o recurso, en donde quiere quitar el acceso.

1. Haga clic en la pestaña **Asignaciones de roles** para ver todas las asignaciones de roles para esta suscripción.

1. En la lista de asignaciones de roles, agregue una marca de verificación a la entidad de seguridad con la asignación de roles que desee quitar.

   ![Mensaje de eliminación de asignación de roles](./media/role-assignments-portal/remove-role-assignment-select.png)

1. Haga clic en **Quitar**.

   ![Mensaje de eliminación de asignación de roles](./media/role-assignments-portal/remove-role-assignment.png)

1. En el mensaje de eliminación de asignación de roles que aparece, haga clic en **Sí**.

    Si ve un mensaje que indica que las asignaciones de roles heredadas no se pueden quitar, significa que intenta quitar una asignación de roles en un ámbito secundario. Debe abrir el control de acceso (IAM) en el ámbito en el que se asignó el rol e intentarlo de nuevo. Una forma rápida de abrir el control de acceso (IAM) en el ámbito correcto es examinar la columna **Ámbito** y hacer clic en el vínculo situado junto a **(Heredado)** .

   ![Mensaje de eliminación de asignación de roles](./media/role-assignments-portal/remove-role-assignment-inherited.png)

## <a name="next-steps"></a>Pasos siguientes

- [Enumeración de asignaciones de roles de Azure mediante Azure Portal](role-assignments-list-portal.md)
- [Tutorial: Concesión de acceso de usuario a los recursos de Azure mediante RBAC y Azure Portal](quickstart-assign-role-user-portal.md)
- [Solución de problemas de Azure RBAC](troubleshooting.md)
- [Organización de los recursos con grupos de administración de Azure](../governance/management-groups/overview.md)

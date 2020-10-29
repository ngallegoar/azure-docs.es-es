---
title: 'Incorporación o eliminación de asignaciones de roles de Azure mediante Azure Portal: RBAC de Azure'
description: Aprenda a conceder acceso a recursos de Azure para usuarios, grupos, entidades de servicio e identidades administradas mediante Azure Portal y el control de acceso basado en roles de Azure (RBAC de Azure).
services: active-directory
author: rolyon
manager: mtillman
ms.service: role-based-access-control
ms.topic: how-to
ms.workload: identity
ms.date: 09/30/2020
ms.author: rolyon
ms.reviewer: bagovind
ms.openlocfilehash: 1aebb80fcba6c21ccce63772195af80b94092bde
ms.sourcegitcommit: 28c5fdc3828316f45f7c20fc4de4b2c05a1c5548
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/22/2020
ms.locfileid: "92368326"
---
# <a name="add-or-remove-azure-role-assignments-using-the-azure-portal"></a>Incorporación o eliminación de asignaciones de roles de Azure mediante Azure Portal

[!INCLUDE [Azure RBAC definition grant access](../../includes/role-based-access-control-definition-grant.md)] En este artículo se describe cómo asignar roles con Azure Portal.

Si tiene que asignar roles de Administrador en Azure Active Directory, consulte [Ver y asignar roles de administrador en Azure Active Directory](../active-directory/roles/manage-roles-portal.md).

## <a name="prerequisites"></a>Requisitos previos

Para agregar o quitar asignaciones de roles, debe tener:

- Permisos `Microsoft.Authorization/roleAssignments/write` y `Microsoft.Authorization/roleAssignments/delete`, como [Administrador de acceso de usuarios](built-in-roles.md#user-access-administrator) o [propietario](built-in-roles.md#owner)

## <a name="access-control-iam"></a>Control de acceso (IAM)

**Control de acceso (IAM)** es la página que se usa normalmente para asignar roles y conceder acceso a los recursos de Azure. También se conoce como administración de identidades y acceso y aparece en varias ubicaciones en Azure Portal. A continuación se muestra un ejemplo de la página Control de acceso (IAM) de una suscripción.

![Página Control de acceso (IAM) para una suscripción](./media/role-assignments-portal/access-control-subscription.png)

Para que esto sea más eficaz con la página control de acceso (IAM), es útil seguir estos pasos cuando quiera asignar un rol.

1. Determine quién necesita acceso. Puede asignar un rol a un usuario, grupo, entidad de servicio o identidad administrada.

1. Busque el rol adecuado. Los permisos se agrupan en roles. Puede seleccionarlos de una lista de varios [roles integrados de Azure](built-in-roles.md) o puede utilizar sus propios roles personalizados.

1. Identifique el ámbito necesario. Azure proporciona cuatro niveles de ámbito: el [grupo de administración](../governance/management-groups/overview.md), la suscripción, el [grupo de recursos](../azure-resource-manager/management/overview.md#resource-groups) y el recurso. Para obtener más información sobre el ámbito, vea [Comprensión del ámbito](scope-overview.md).

1. Siga los pasos de una de las secciones siguientes para asignar un rol.

## <a name="add-a-role-assignment"></a>Adición de una asignación de roles

En RBAC de Azure, para conceder acceso a un recurso de Azure, se agrega una asignación de roles. Siga estos pasos para asignar un rol.

1. En Azure Portal, haga clic en **Todos los servicios** y luego seleccione el ámbito al que quiere conceder acceso. Por ejemplo, puede seleccionar **Grupos de administración** , **Suscripciones** , **Grupos de recursos** o un recurso.

1. Haga clic en el recurso específico de ese ámbito.

1. Haga clic en **Control de acceso (IAM).**

1. Haga clic en la pestaña **Asignaciones de roles** para ver todas las asignaciones de roles en este ámbito.

    ![Control de acceso (IAM) y la pestaña Asignaciones de roles](./media/role-assignments-portal/role-assignments.png)

1. Haga clic en **Agregar** > **Agregar asignación de roles** .

   Si no tiene permisos para asignar roles, la opción Agregar asignación de roles se deshabilitará.

   ![Menú Agregar asignación de roles](./media/shared/add-role-assignment-menu.png)

    Se abre el panel Agregar asignación de roles.

   ![Panel Agregar asignación de roles](./media/role-assignments-portal/add-role-assignment.png)

1. En la lista desplegable **Rol** , seleccione un rol como **Colaborador de la máquina virtual** .

1. En la lista **Seleccionar** , seleccione un usuario, grupo, entidad de servicio o identidad administrada. Si no ve la entidad de seguridad en la lista, puede escribir en el cuadro **Seleccionar** para buscar nombres para mostrar, direcciones de correo electrónico e identificadores de objeto en el directorio.

1. Haga clic en **Guardar** para asignar el rol.

   Transcurridos unos instantes, se asigna el rol a la entidad de seguridad en el ámbito seleccionado.

    ![Agregar asignación de roles guardado](./media/role-assignments-portal/add-role-assignment-save.png)

## <a name="assign-a-user-as-an-administrator-of-a-subscription"></a>Asignación de un usuario como administrador de una suscripción

Para que un usuario sea administrador de una suscripción de Azure, asígnele el rol [Propietario](built-in-roles.md#owner) en el ámbito de la suscripción. El rol Propietario da al usuario acceso total a todos los recursos de la suscripción, incluido el derecho a conceder acceso a otros. Estos pasos son los mismos que para la asignación de cualquier otro rol.

1. En Azure Portal, haga clic en **Todos los servicios** y luego en **Suscripciones** .

1. Haga clic en la suscripción a la que quiere conceder acceso.

1. Haga clic en **Control de acceso (IAM).**

1. Haga clic en la pestaña **Asignaciones de roles** para ver todas las asignaciones de roles de esta suscripción.

    ![Control de acceso (IAM) y la pestaña Asignaciones de roles](./media/role-assignments-portal/role-assignments.png)

1. Haga clic en **Agregar** > **Agregar asignación de roles** .

   Si no tiene permisos para asignar roles, la opción Agregar asignación de roles se deshabilitará.

   ![Menú para agregar la asignación de roles en una suscripción](./media/shared/add-role-assignment-menu.png)

    Se abre el panel Agregar asignación de roles.

   ![Panel para agregar la asignación de roles en una suscripción](./media/role-assignments-portal/add-role-assignment.png)

1. En la lista desplegable **Rol** , seleccione el rol **Propietario** .

1. En la lista **Seleccionar** , seleccione un usuario. Si no ve el usuario en la lista, puede escribir en el cuadro **Seleccionar** para buscar nombres para mostrar y direcciones de correo electrónico en el directorio.

1. Haga clic en **Guardar** para asignar el rol.

   Transcurridos unos instantes, al usuario se le asigna el rol Propietario en el ámbito de la suscripción.

## <a name="add-a-role-assignment-for-a-managed-identity-preview"></a>Adición de una asignación de roles para una identidad administrada (versión preliminar)

Puede agregar asignaciones de roles para una identidad administrada mediante la página **Control de acceso (IAM)** como se describió anteriormente en este artículo. Cuando se usa la página Control de acceso (IAM), se empieza con el ámbito y, a continuación, se selecciona la identidad administrada y el rol. En esta sección se describe una manera alternativa de agregar asignaciones de roles para una identidad administrada. Con estos pasos, empieza con la identidad administrada y luego selecciona el ámbito y el rol.

> [!IMPORTANT]
> La adición de una asignación de roles para una identidad administrada mediante estos pasos alternativos se encuentra actualmente en versión preliminar.
> Esta versión preliminar se ofrece sin Acuerdo de Nivel de Servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas.
> Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

### <a name="system-assigned-managed-identity"></a>Identidad administrada asignada por el sistema

Siga estos pasos para asignar un rol a una identidad administrada asignada por el sistema a partir de la identidad administrada.

1. En Azure Portal, abra una identidad administrada asignada por el sistema.

1. En el menú izquierdo, haga clic en **Identidad** .

    ![Identidad administrada asignada por el sistema](./media/shared/identity-system-assigned.png)

1. En **Permisos** , haga clic en **Asignaciones de roles de Azure** .

    Si los roles ya están asignados a la identidad administrada asignada por el sistema seleccionada, verá la lista de asignaciones de roles. En esta lista se incluyen todas las asignaciones de roles de las que tenga permiso para leer.

    ![Asignaciones de roles para una identidad administrada asignada por el sistema](./media/shared/role-assignments-system-assigned.png)

1. Para cambiar la suscripción, haga clic en la lista **Suscripción** .

1. Haga clic en **Agregar asignación de roles (versión preliminar)** .

1. Use las listas desplegables para seleccionar el conjunto de recursos a los que se aplica la asignación de roles, como **Suscripción** , **Grupo de recursos** o recurso.

    Si no tiene permisos de escritura de asignación de roles para el ámbito seleccionado, se mostrará un mensaje alineado. 

1. En la lista desplegable **Rol** , seleccione un rol como **Colaborador de la máquina virtual** .

   ![Panel para agregar la asignación de roles para la identidad administrada asignada por el sistema](./media/role-assignments-portal/add-role-assignment-with-scope.png)

1. Haga clic en **Guardar** para asignar el rol.

   Transcurridos unos instantes, se asigna el rol a la identidad administrada en el ámbito seleccionado.

### <a name="user-assigned-managed-identity"></a>Identidad administrada asignada por el usuario

Siga estos pasos para asignar un rol a una identidad administrada asignada por el usuario a partir de la identidad administrada.

1. En Azure Portal, abra una identidad administrada asignada por el usuario.

1. En el menú de la izquierda, haga clic en **Asignaciones de roles de Azure** .

    Si los roles ya están asignados a la identidad administrada asignada por el usuario seleccionada, verá la lista de asignaciones de roles. En esta lista se incluyen todas las asignaciones de roles de las que tenga permiso para leer.

    ![Asignaciones de roles para una identidad administrada asignada por el usuario](./media/shared/role-assignments-user-assigned.png)

1. Para cambiar la suscripción, haga clic en la lista **Suscripción** .

1. Haga clic en **Agregar asignación de roles (versión preliminar)** .

1. Use las listas desplegables para seleccionar el conjunto de recursos a los que se aplica la asignación de roles, como **Suscripción** , **Grupo de recursos** o recurso.

    Si no tiene permisos de escritura de asignación de roles para el ámbito seleccionado, se mostrará un mensaje alineado. 

1. En la lista desplegable **Rol** , seleccione un rol como **Colaborador de la máquina virtual** .

   ![Panel para agregar la asignación de roles para la identidad administrada asignada por el usuario](./media/role-assignments-portal/add-role-assignment-with-scope.png)

1. Haga clic en **Guardar** para asignar el rol.

   Transcurridos unos instantes, se asigna el rol a la identidad administrada en el ámbito seleccionado.

## <a name="remove-a-role-assignment"></a>Eliminación de una asignación de rol

En RBAC de Azure, para quitar el acceso de un recurso de Azure, se quita una asignación de roles. Siga estos pasos para eliminar una asignación de roles.

1. Abra **Control de acceso (IAM)** en un ámbito como, por ejemplo, grupo de administración, suscripción, grupo de recursos o recurso, en donde quiere quitar el acceso.

1. Haga clic en la pestaña **Asignaciones de roles** para ver todas las asignaciones de roles para esta suscripción.

1. En la lista de asignaciones de roles, agregue una marca de verificación a la entidad de seguridad con la asignación de roles que desee quitar.

   ![Asignación de roles seleccionada que se va a quitar](./media/role-assignments-portal/remove-role-assignment-select.png)

1. Haga clic en **Quitar** .

   ![Mensaje de eliminación de asignación de roles](./media/role-assignments-portal/remove-role-assignment.png)

1. En el mensaje de eliminación de asignación de roles que aparece, haga clic en **Sí** .

    Si ve un mensaje que indica que las asignaciones de roles heredadas no se pueden quitar, significa que intenta quitar una asignación de roles en un ámbito secundario. Debe abrir el control de acceso (IAM) en el ámbito en el que se asignó el rol e intentarlo de nuevo. Una forma rápida de abrir el control de acceso (IAM) en el ámbito correcto es examinar la columna **Ámbito** y hacer clic en el vínculo situado junto a **(Heredado)** .

   ![Quitar el mensaje de asignación de roles de las asignaciones de roles heredadas](./media/role-assignments-portal/remove-role-assignment-inherited.png)

## <a name="next-steps"></a>Pasos siguientes

- [Enumeración de asignaciones de roles de Azure mediante Azure Portal](role-assignments-list-portal.md)
- [Tutorial: Concesión de acceso de usuario a los recursos de Azure mediante RBAC y Azure Portal](quickstart-assign-role-user-portal.md)
- [Solución de problemas de Azure RBAC](troubleshooting.md)
- [Organización de los recursos con grupos de administración de Azure](../governance/management-groups/overview.md)

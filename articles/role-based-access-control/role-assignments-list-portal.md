---
title: Lista de asignaciones de roles con RBAC de Azure y Azure Portal
description: Aprenda a determinar a qué recursos tienen acceso los usuarios, grupos, entidades de servicio e identidades administradas mediante el control de acceso basado en rol (RBAC) y Azure Portal.
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
ms.date: 03/18/2020
ms.author: rolyon
ms.reviewer: bagovind
ms.openlocfilehash: 03a3d3c7d572d7ec5b8d3ac3d527d0d59e649bc2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "80062239"
---
# <a name="list-role-assignments-using-azure-rbac-and-the-azure-portal"></a>Lista de asignaciones de roles con RBAC de Azure y Azure Portal

[!INCLUDE [Azure RBAC definition list access](../../includes/role-based-access-control-definition-list.md)] En este artículo se describe cómo enumerar las asignaciones de roles mediante Azure Portal.

> [!NOTE]
> Si su organización ha externalizado las funciones de administración a un proveedor de servicios que usa la [administración de recursos delegados de Azure](../lighthouse/concepts/azure-delegated-resource-management.md), las asignaciones de roles autorizadas por ese proveedor de servicios no se mostrarán aquí.

## <a name="list-role-assignments-for-a-user-or-group"></a>Lista de las asignaciones de rol de un usuario o grupo

La forma más fácil de ver los roles asignados a un usuario o grupo de una suscripción es usar el panel **Recursos de Azure**.

1. En Azure Portal, haga clic en **Todos los servicios** y luego seleccione **Usuarios** o **Grupos**.

1. Haga clic en el usuario o grupo del que desea enumerar las asignaciones de roles.

1. Haga clic en **Recursos de Azure**.

    Verá una lista de los roles asignados al usuario o grupo seleccionado en varios ámbitos como los de grupo de administración, suscripción, grupo de recursos o recurso. En esta lista se incluyen todas las asignaciones de roles de las que tenga permiso para leer.

    ![Asignaciones de roles de un usuario](./media/role-assignments-list-portal/azure-resources-user.png)    

1. Para cambiar la suscripción, haga clic en la lista **Suscripciones**.

## <a name="list-owners-of-a-subscription"></a>Enumeración de los propietarios de una suscripción

Los usuarios que se han asignado al rol de [propietario](built-in-roles.md#owner) para una suscripción pueden administrar todo en esta. Siga estos pasos para mostrar los propietarios de una suscripción.

1. En Azure Portal, haga clic en **Todos los servicios** y luego en **Suscripciones**.

1. Haga clic en la suscripción de la que quiera mostrar los propietarios.

1. Haga clic en **Control de acceso (IAM).**

1. Haga clic en la pestaña **Asignaciones de roles** para ver todas las asignaciones de roles para esta suscripción.

1. Desplácese hasta la sección **Propietarios** para ver todos los usuarios a los que se ha asignado el rol de propietario para esta suscripción.

   ![Control de acceso a la suscripción: pestaña Asignaciones de roles](./media/role-assignments-list-portal/access-control-role-assignments-subscription.png)

## <a name="list-role-assignments-at-a-scope"></a>Lista de las asignaciones de roles en un ámbito

1. En Azure Portal, haga clic en **Todos los servicios** y luego seleccione el ámbito. Por ejemplo, puede seleccionar **Grupos de administración**, **Suscripciones**, **Grupos de recursos** o un recurso.

1. Haga clic en el recurso específico.

1. Haga clic en **Control de acceso (IAM).**

1. Haga clic en la pestaña **Asignaciones de roles** para ver todas las asignaciones de roles en este ámbito.

   ![Control de acceso: pestaña Asignaciones de roles](./media/role-assignments-list-portal/access-control-role-assignments.png)

   En la pestaña Asignaciones de roles puede ver quién tiene acceso a este ámbito. Observe que el ámbito de algunos roles es **este recurso**, mientras que el de otros es **(heredado)**  de otro ámbito. El acceso se asigna específicamente a este recurso o se hereda de una asignación en el ámbito principal.

## <a name="list-role-assignments-for-a-user-at-a-scope"></a>Lista de las asignaciones de rol de un usuario en un ámbito

Para mostrar el acceso de un usuario, grupo, entidad de servicio o identidad administra, puede mostrar sus asignaciones de roles. Siga estos pasos para mostrar las asignaciones de roles de un solo usuario, grupo, entidad de servicio o identidad administrada en un ámbito determinado.

1. En Azure Portal, haga clic en **Todos los servicios** y luego seleccione el ámbito. Por ejemplo, puede seleccionar **Grupos de administración**, **Suscripciones**, **Grupos de recursos** o un recurso.

1. Haga clic en el recurso específico.

1. Haga clic en **Control de acceso (IAM).**

1. Haga clic en la pestaña **Comprobar acceso**.

    ![Control de acceso: pestaña Comprobar acceso](./media/role-assignments-list-portal/access-control-check-access.png)

1. En la lista **Buscar**, seleccione el tipo de entidad de seguridad cuyo acceso quiere comprobar.

1. En el cuadro de búsqueda, escriba una cadena para buscar nombres para mostrar, direcciones de correo electrónico o identificadores de objeto en el directorio.

    ![Lista de selección de Comprobar acceso](./media/role-assignments-list-portal/check-access-select.png)

1. Haga clic en la entidad de seguridad para abrir el panel **Asignaciones**.

    ![Panel de asignaciones](./media/role-assignments-list-portal/check-access-assignments.png)

    En este panel puede ver los roles asignados a la entidad de seguridad seleccionada y el ámbito. Si hay asignaciones denegadas en este ámbito o heredadas en este ámbito, se mostrarán.

## <a name="list-role-assignments-for-a-system-assigned-managed-identity"></a>Lista de asignaciones de roles para una identidad administrada asignada por el sistema

1. En Azure Portal, abra una identidad administrada asignada por el sistema.

1. En el menú izquierdo, haga clic en **Identidad**.

    ![Identidad administrada asignada por el sistema](./media/role-assignments-list-portal/identity-system-assigned.png)

1. En **Asignaciones de roles**, haga clic en **Mostrar los roles RBAC de Azure asignados a esta identidad administrada**.

    Verá una lista de los roles asignados a la identidad administrada asignada por el sistema seleccionada en varios ámbitos como los de grupo de administración, suscripción, grupo de recursos o recurso. En esta lista se incluyen todas las asignaciones de roles de las que tenga permiso para leer.

    ![Asignaciones de roles para una identidad administrada asignada por el sistema](./media/role-assignments-list-portal/azure-resources-system-assigned.png)

## <a name="list-role-assignments-for-a-user-assigned-managed-identity"></a>Lista de asignaciones de roles para una identidad administrada asignada por el usuario

1. En Azure Portal, abra una identidad administrada asignada por el usuario.

1. Haga clic en **Recursos de Azure**.

    Verá una lista de los roles asignados a la identidad administrada asignada por el usuario seleccionada en varios ámbitos como los de grupo de administración, suscripción, grupo de recursos o recurso. En esta lista se incluyen todas las asignaciones de roles de las que tenga permiso para leer.

    ![Asignaciones de roles para una identidad administrada asignada por el sistema](./media/role-assignments-list-portal/azure-resources-user-assigned.png)

1. Para cambiar la suscripción, haga clic en la lista **Suscripciones**.

## <a name="list-number-of-role-assignments"></a>Enumeración del número de asignaciones de roles

Puede tener hasta **2000** asignaciones de roles en cada suscripción. Para ayudarle a realizar un seguimiento de este límite, la pestaña **Asignaciones de roles** incluye un gráfico que muestra el número de asignaciones de roles de la suscripción actual.

![Cuadro de control de acceso: número de asignaciones de roles](./media/role-assignments-list-portal/access-control-role-assignments-chart.png)

Si está cerca del número máximo e intenta agregar más asignaciones de roles, verá una advertencia en el panel **Agregar asignación de roles**. Para saber cómo puede reducir el número de asignaciones de roles, vea [Solución de problemas de RBAC de Azure](troubleshooting.md#azure-role-assignments-limit).

![Advertencia de control de acceso - agregar asignación de roles](./media/role-assignments-list-portal/add-role-assignment-warning.png)

## <a name="next-steps"></a>Pasos siguientes

- [Incorporación o eliminación de asignaciones de roles con RBAC de Azure y Azure Portal](role-assignments-portal.md)
- [Solución de problemas del control de acceso basado en rol para recursos de Azure](troubleshooting.md)

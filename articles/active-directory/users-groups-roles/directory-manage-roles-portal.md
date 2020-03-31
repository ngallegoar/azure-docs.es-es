---
title: 'Visualización y asignación de permisos de roles de administrador: Azure AD | Microsoft Docs'
description: Ahora puede ver y administrar los miembros de un rol de administrador de Azure AD en el portal. Para aquellos que administran con frecuencia las asignaciones de roles.
services: active-directory
author: curtand
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: users-groups-roles
ms.topic: article
ms.date: 11/08/2019
ms.author: curtand
ms.reviewer: vincesm
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: b2815b496545ca5f920c00df7b2b5b7efe15c12a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "74900898"
---
# <a name="view-and-assign-administrator-roles-in-azure-active-directory"></a>Visualización y asignación de roles de administrador en Azure Active Directory

Ahora puede ver y administrar todos los miembros de los roles de administrador en el portal de Azure Active Directory. Si administra las asignaciones de roles con frecuencia, probablemente preferirá esta experiencia. Y si alguna vez se ha preguntado "¿qué es lo que realmente hacen estos roles?", puede ver una lista detallada de los permisos para cada uno de los roles de administrador de Azure AD.

## <a name="view-all-roles"></a>Ver todos los roles

Busque y seleccione **Azure Active Directory**. Seleccione **Roles y administradores** para ver la lista de todos los roles disponibles.

Haga clic en el botón de puntos suspensivos a la derecha de cada fila para abrir la descripción detallada del rol.

![lista de roles en el portal de Azure AD](./media/directory-manage-roles-portal/view-roles-in-azure-active-directory.png)

## <a name="view-my-roles"></a>Ver mis roles

También es fácil ver sus propios permisos. Seleccione **su rol** en la página **Roles y administradores** para ver los roles que tiene asignados actualmente.

## <a name="view-assignments-for-a-role"></a>Ver las asignaciones de un rol

Haga clic en un rol para ver los usuarios asignados a dicho rol. Puede hacer clic en **Administrar en PIM** para obtener funcionalidades de administración adicionales. Los administradores de roles con privilegios pueden cambiar las asignaciones "Permanentes" (siempre activas en el rol) a "Elegibles" (en el rol solo cuando se elevan). Si no dispone de PIM, todavía puede hacer clic en **Administrar en PIM** para registrarse y obtener una versión de prueba. Privileged Identity Management requiere un [Plan de licencias de Azure AD Premium P2](../privileged-identity-management/subscription-requirements.md).

![lista de miembros de un rol de administrador](./media/directory-manage-roles-portal/member-list.png)

Si es un administrador global o un administrador de roles con privilegios, puede agregar o quitar miembros, filtrar la lista o seleccionar un miembro fácilmente para ver sus roles asignados activos.

## <a name="view-a-users-role-permissions"></a>Ver permisos de rol de un usuario

Cuando esté viendo los miembros de un rol, haga clic en **Descripción** para ver una lista completa de los permisos otorgados por la asignación de roles. En la página se incluyen vínculos a documentación relevante que le guiarán a través de la administración de los roles de directorio.

![lista de permisos para un rol de administrador](./media/directory-manage-roles-portal/role-description.png)


## <a name="next-steps"></a>Pasos siguientes

* No dude en compartir con nosotros en el [foro de roles administrativos de Azure AD](https://feedback.azure.com/forums/169401-azure-active-directory?category_id=166032).
* Para obtener más información sobre los roles y la asignación de roles de administrador, consulte el artículo [Assign administrator roles](directory-assign-admin-roles.md) (Asignación de roles de administrador).
* Para conocer los permisos predeterminados de usuario, vea una [comparación de los permisos predeterminados de usuario miembro e invitado](../fundamentals/users-default-permissions.md).

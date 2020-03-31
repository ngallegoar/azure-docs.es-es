---
title: Asignación de roles de directorio a usuarios con Azure Active Directory | Microsoft Docs
description: Instrucciones acerca de cómo asignar roles de administrador y de no administrador a usuarios con Azure Active Directory.
services: active-directory
author: msaburnley
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: fundamentals
ms.topic: conceptual
ms.date: 09/06/2018
ms.author: ajburnle
ms.reviewer: jeffsta
ms.custom: it-pro, seodec18
ms.collection: M365-identity-device-management
ms.openlocfilehash: 2df52969ea79e5d1af132aa82c2ec1ceedb92b82
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "75422894"
---
# <a name="assign-administrator-and-non-administrator-roles-to-users-with-azure-active-directory"></a>Asignación de roles de administrador y de no administrador a usuarios con Azure Active Directory
Si un usuario de su organización necesita permiso para administrar recursos de Azure Active Directory (Azure AD), debe asignar al usuario un rol adecuado en Azure AD, en función de las acciones para las que el usuario necesita permisos.

Para más información acerca de los roles disponibles, consulte [Asignación de roles de administrador en Azure Active Directory](../users-groups-roles/directory-assign-admin-roles.md). Para obtener información sobre cómo agregar usuarios, consulte [Incorporación de nuevos usuarios a Azure Active Directory](add-users-azure-active-directory.md).

## <a name="assign-roles"></a>Asignación de roles
Comúnmente, se asignan roles de Azure AD a los usuarios desde la página **Rol de directorio** de un usuario.

También puede asignar roles mediante Privileged Identity Management (PIM). Para obtener más información sobre cómo usar PIM, consulte [Privileged Identity Management](https://docs.microsoft.com/azure/active-directory/privileged-identity-management).

### <a name="to-assign-a-role-to-a-user"></a>Para asignar un rol a un usuario
1. Inicie sesión en [Azure Portal](https://portal.azure.com/) con una cuenta de administrador global para el directorio. 

2. Busque y seleccione **Azure Active Directory**.

      ![Búsqueda de Azure Active Directory en Azure Portal](media/active-directory-users-assign-role-azure-portal/search-azure-active-directory.png)


3. Seleccione **Usuarios**.

4. Busque y seleccione el usuario que obtiene la asignación de roles. Por ejemplo, _Alain Charon_.

      ![Página Todos los usuarios, selección del usuario](media/active-directory-users-assign-role-azure-portal/directory-role-select-user.png)

5. En la página **Alain Charon - Perfil**, seleccione **Roles asignados**.

    Se muestra la página **Alain Charon - Rol de directorio**.

6. Seleccione **Agregar asignación**, seleccione el rol que asignará a Alain (por ejemplo, _Administrador de la aplicación_) y, a continuación, elija **Seleccionar**.

    ![Página Roles asignados, se muestra el rol seleccionado](media/active-directory-users-assign-role-azure-portal/directory-role-select-role.png)

    El rol de administrador de la aplicación se asigna a Alain Charon y se muestra en la página **Alain Charon - Rol de directorio**.

## <a name="remove-a-role-assignment"></a>Eliminación de una asignación de rol
Si necesita quitar la asignación de roles de un usuario, también puede hacerlo desde la página **Alain Charon - Rol de directorio**.

### <a name="to-remove-a-role-assignment-from-a-user"></a>Para quitar una asignación de rol a un usuario

1. Seleccione **Azure Active Directory**, seleccione **Usuarios** y, a continuación, busque y seleccione los usuarios a los que quitará una asignación de roles. Por ejemplo, _Alain Charon_.

2. Seleccione **Roles asignados**, seleccione **Administrador de aplicaciones** y, luego, **Quitar asignación**.

    ![Página Roles asignados, se muestra el rol seleccionado y la opción para quitarlo](media/active-directory-users-assign-role-azure-portal/directory-role-remove-role.png)

    El rol de administrador de la aplicación se quita de Alain Charon y ya no se muestra en la página **Alain Charon - Rol de directorio**.

## <a name="next-steps"></a>Pasos siguientes
- [Adición o eliminación de usuarios](add-users-azure-active-directory.md)

- [Add or change profile information](active-directory-users-profile-azure-portal.md) (Incorporación o modificación de la información del perfil)

- [Adición de usuarios invitados de otro directorio](../b2b/what-is-b2b.md)

O bien, puede realizar otras tareas de administración de usuarios, como asignar delegados, usar directivas y compartir cuentas de usuario. Para obtener más información acerca de otras acciones disponibles, consulte la [documentación de administración de usuarios en Azure Active Directory](../users-groups-roles/index.yml).



---
title: 'Concesión de acceso para administrar PIM: Azure Active Directory | Microsoft Docs'
description: Aprenda a conceder acceso a otros administradores para administrar Azure AD Privileged Identity Management (PIM).
services: active-directory
documentationcenter: ''
author: curtand
manager: daveba
editor: ''
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.subservice: pim
ms.date: 11/08/2019
ms.author: curtand
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: 7424e92f8520d13137b6ac8787523095058a005f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "74022110"
---
# <a name="grant-access-to-other-administrators-to-manage-privileged-identity-management"></a>Concesión de acceso a otros administradores para administrar Privileged Identity Management

El administrador global que habilita Privileged Identity Management (PIM) para una organización, obtiene automáticamente asignaciones de roles y acceso a Privileged Identity Management. Nadie más en la organización de Azure Active Directory (Azure AD) obtiene acceso de escritura de forma predeterminada, incluidos otros administradores globales. Otros administradores globales, administradores de seguridad y lectores de seguridad tienen acceso de solo lectura a Privileged Identity Management. Para conceder acceso a Privileged Identity Management, el primer usuario puede asignar a otros el rol **Administrador de roles con privilegios**.

> [!NOTE]
> La administración de Privileged Identity Management requiere Azure Multi-Factor Authentication. Dado que las cuentas de Microsoft no se pueden registrar en Azure Multi-Factor Authentication, un usuario que inicia sesión con una cuenta de Microsoft no puede acceder a Privileged Identity Management.

Asegúrese de que siempre haya al menos dos usuarios en un rol de administrador de roles con privilegios, por si se diera el caso de que a un usuario se le impida el acceso o su cuenta se elimine.

## <a name="grant-access-to-manage-pim"></a>Concesión de acceso para administrar PIM

1. Inicie sesión en [Azure Portal](https://portal.azure.com/).

1. Abra **Privileged Identity Management** en Azure AD.

1. Seleccione **Roles de Azure AD**.

1. Seleccione **Roles**.

    ![Roles de Azure AD de Privileged Identity Management: roles](./media/pim-how-to-give-access-to-pim/pim-directory-roles-roles.png)

1. Seleccione el rol **Administrador de roles con privilegios** para abrir la página de miembros.

    ![Administrador de roles con privilegios: miembros](./media/pim-how-to-give-access-to-pim/pim-pra-members.png)

1. Seleccione **Agregar miembro** para abrir el panel Agregar miembros administrados.

1. Seleccione **Seleccionar miembros** para abrir el panel de selección de miembros.

    ![Administrador de roles con privilegios: seleccionar miembros](./media/pim-how-to-give-access-to-pim/pim-pra-select-members.png)

1. Seleccione un miembro y, luego, haga clic en **Seleccionar**.

1. Seleccione **Aceptar** para hacer que el miembro sea válido para el rol **Administrador de roles con privilegios**.

    Cuando se asigna un nuevo rol a alguien en Privileged Identity Management, este usuario se configura automáticamente como **Válido** para activar el rol.

1. Para convertir el miembro en permanente, seleccione el usuario en la lista de miembros de administradores de roles con privilegios.

1. Seleccione **Más** y, a continuación, seleccione **Hacer permanente** para que la asignación sea permanente.

    ![Administrador de roles con privilegios: hacer permanente](./media/pim-how-to-give-access-to-pim/pim-pra-make-permanent.png)

1. Envíe al usuario un vínculo para [Empezar a usar Privileged Identity Management](pim-getting-started.md).

## <a name="remove-access-to-manage-pim"></a>Quitar el acceso para administrar PIM

Antes de quitar a alguien del rol de administrador de roles con privilegios, asegúrese siempre de que seguirá habiendo al menos dos usuarios asignados.

1. Inicie sesión en [Azure Portal](https://portal.azure.com/).

1. Abra **Azure AD Privileged Identity Management**.

1. Seleccione **Roles de Azure AD**.

1. Seleccione **Roles**.

1. Seleccione el rol **Administrador de roles con privilegios** para abrir la página de miembros.

1. Seleccione la casilla de verificación junto al usuario que quiere eliminar y, a continuación, seleccione **Eliminar miembro**.

    ![Administrador de rol con privilegios: quitar miembros](./media/pim-how-to-give-access-to-pim/pim-pra-remove-member.png)

1. Cuando se le pida que confirme que desea quitar el miembro del rol, seleccione **Sí**.

## <a name="next-steps"></a>Pasos siguientes

- [Empiece a usar Privileged Identity Management](pim-getting-started.md)

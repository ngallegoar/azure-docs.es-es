---
title: 'Roles que no se pueden administrar en Privileged Identity Management: Azure Active Directory | Microsoft Docs'
description: Describe los roles que no se pueden administrar en Azure AD Privileged Identity Management (PIM).
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
editor: ''
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.subservice: pim
ms.date: 10/23/2019
ms.author: curtand
ms.custom: pim ; H1Hack27Feb2017;oldportal;it-pro;
ms.collection: M365-identity-device-management
ms.openlocfilehash: 6fac7074cf85a585c93ece60be9eea8ffb9a6345
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "72895197"
---
# <a name="roles-you-cant-manage-in-privileged-identity-management"></a>Roles que no se pueden administrar en Privileged Identity Management

Azure AD Privileged Identity Management (PIM) le permite administrar todos los [roles de Azure AD](../users-groups-roles/directory-assign-admin-roles.md) y todos los [roles de recursos de Azure](../../role-based-access-control/built-in-roles.md). Estos roles también incluyen sus roles personalizados asociados a los grupos de administración, suscripciones, grupos de recursos y recursos. Sin embargo, hay algunos roles que no puede administrar. En este artículo se describen los roles que no se pueden administrar en Privileged Identity Management.

## <a name="classic-subscription-administrator-roles"></a>Roles de administrador de suscripciones clásicas

No puede administrar los siguientes roles de administrador de suscripciones clásico en Privileged Identity Management:

- Administrador de cuenta
- Administrador de servicios
- Coadministrador

Para más información sobre los roles de administrador de suscripciones clásico, consulte [Roles de administrador de suscripciones clásico de RBAC de Azure y de administrador de Azure AD](../../role-based-access-control/rbac-and-directory-admin-roles.md).

## <a name="what-about-office-365-admin-roles"></a>¿Qué son los roles de administrador de Office 365?

Los roles de Exchange Online o SharePoint Online, excepto para el Administrador de Exchange y el Administrador de SharePoint, no están representados en Azure AD y, por tanto, no se pueden administrar en Privileged Identity Management. Para más información acerca de estos servicios de Office 365, consulte [Roles de administrador de Office 365](https://docs.microsoft.com/office365/admin/add-users/about-admin-roles).

> [!NOTE]
> El administrador de SharePoint tiene acceso administrativo a SharePoint Online a través del Centro de administración de SharePoint Online y puede realizar casi cualquier tarea en SharePoint Online. Los usuarios aptos pueden experimentar retrasos al usar este rol dentro de SharePoint después de la activación en Privileged Identity Management.

## <a name="next-steps"></a>Pasos siguientes

- [Asignación de roles de Azure AD en Privileged Identity Management](pim-how-to-add-role-to-user.md)
- [Asignación de roles de recursos de Azure en Privileged Identity Management](pim-resource-roles-assign-roles.md)

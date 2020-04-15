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
ms.date: 03/31/2020
ms.author: curtand
ms.custom: pim ; H1Hack27Feb2017;oldportal;it-pro;
ms.collection: M365-identity-device-management
ms.openlocfilehash: b6eaa50f57dd8037ef0ad96b69284f565bd3558f
ms.sourcegitcommit: 515482c6348d5bef78bb5def9b71c01bb469ed80
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/02/2020
ms.locfileid: "80607524"
---
# <a name="roles-you-cant-manage-in-privileged-identity-management"></a>Roles que no se pueden administrar en Privileged Identity Management

Azure AD Privileged Identity Management (PIM) le permite administrar todos los [roles de Azure AD](../users-groups-roles/directory-assign-admin-roles.md) y todos los [roles de Azure](../../role-based-access-control/built-in-roles.md). Los roles de Azure también pueden incluir sus roles personalizados asociados a los grupos de administración, suscripciones, grupos de recursos y recursos. Sin embargo, hay algunos roles que no puede administrar. En este artículo se describen los roles que no se pueden administrar en Privileged Identity Management.

## <a name="classic-subscription-administrator-roles"></a>Roles de administrador de suscripciones clásicas

No puede administrar los siguientes roles de administrador de suscripciones clásico en Privileged Identity Management:

- Administrador de cuenta
- Administrador de servicios
- Coadministrador

Para más información sobre los roles de administrador de suscripciones clásico, consulte [Roles de administrador de suscripciones clásico de RBAC de Azure y de administrador de Azure AD](../../role-based-access-control/rbac-and-directory-admin-roles.md).

## <a name="what-about-office-365-admin-roles"></a>¿Qué son los roles de administrador de Office 365?

Se admiten todos los roles de Office365 en la experiencia de roles de Azure AD y el portal de administradores, como Administrador de Exchange y Administrador de SharePoint, pero no se admiten roles específicos en RBAC de Exchange ni en RBAC de SharePoint. Para más información acerca de estos servicios de Office 365, consulte [Roles de administrador de Office 365](https://docs.microsoft.com/office365/admin/add-users/about-admin-roles).

> [!NOTE]
> Los usuarios que cumplan los requisitos para el rol Administrador de SharePoint, así como los roles que intenten tener acceso al centro de seguridad y cumplimiento de Microsoft, pueden experimentar retrasos de hasta unas horas después de activar su rol. Estamos trabajando con esos equipos para corregir los problemas.

## <a name="next-steps"></a>Pasos siguientes

- [Asignación de roles de Azure AD en Privileged Identity Management](pim-how-to-add-role-to-user.md)
- [Asignación de roles de recursos de Azure en Privileged Identity Management](pim-resource-roles-assign-roles.md)

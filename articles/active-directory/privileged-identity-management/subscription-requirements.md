---
title: 'Requisitos de licencia para usar Privileged Identity Management: Azure Active Directory | Microsoft Docs'
description: Describe los requisitos de licencia necesarios para usar Azure AD Privileged Identity Management (PIM).
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
editor: markwahl-msft
ms.assetid: 34367721-8b42-4fab-a443-a2e55cdbf33d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: how-to
ms.subservice: pim
ms.date: 01/10/2020
ms.author: curtand
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: 42d931206e99516e0320d0cbedd0812389ee41aa
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "84742171"
---
# <a name="license-requirements-to-use-privileged-identity-management"></a>Requisitos de licencia para usar Privileged Identity Management

Para Azure Active Directory (Azure AD) Privileged Identity Management (PIM), un directorio debe tener una licencia válida. Además, deben asignarse licencias a los administradores y usuarios pertinentes. Este artículo describe los requisitos de licencia para usar Privileged Identity Management.

## <a name="valid-licenses"></a>Licencias válidas

[!INCLUDE [Azure AD Premium P2 license](../../../includes/active-directory-p2-license.md)]

## <a name="how-many-licenses-must-you-have"></a>¿Cuántas licencias debe tener?

Asegúrese de que el directorio tiene al menos tantas licencias de Azure AD Premium P2 como empleados que van a realizar las tareas siguientes:

- Los usuarios asignados como aptos a roles de Azure AD mediante PIM
- Los usuarios que pueden aprobar o rechazar solicitudes de activación en PIM
- Los usuarios asignados a un rol de recursos de Azure con asignaciones Just-In-Time o directas (basadas en tiempo)  
- Los usuarios asignados a una revisión de acceso
- Los usuarios que realizan revisiones de acceso

Las licencias de Azure AD Premium P2 **no** son necesarias para las tareas siguientes:

- No se necesitan licencias para los usuarios con los roles Administrador global o Administrador de roles con privilegios que configuran PIM, configuran directivas, reciben alertas y configuran revisiones de acceso.

Para más información sobre las licencias, vea [Asignación o eliminación de licencias mediante el portal de Azure Active Directory](../fundamentals/license-users-groups.md).

## <a name="example-license-scenarios"></a>Escenarios de licencia de ejemplo

Estos son algunos escenarios de licencia de ejemplo que le ayudarán a determinar el número de licencias que debe tener.

| Escenario | Cálculo | Número de licencias |
| --- | --- | --- |
| Woodgrove Bank tiene 10 administradores para diferentes departamentos y 2 administradores globales que configuran y administran PIM. Eso hace cinco administradores válidos. | Cinco licencias para los administradores que sean válidos | 5 |
| Graphic Design Institute tiene 25 administradores, de los cuales 14 se administran a través de PIM. La activación de roles necesita aprobación y hay tres usuarios diferentes en la organización que pueden aprobar las activaciones. | 14 licencias para los roles válidos + 3 aprobadores | 17 |
| Contoso tiene 50 administradores, de los cuales 42 se administran a través de PIM. La activación de roles necesita aprobación y hay cinco usuarios diferentes en la organización que pueden aprobar las activaciones. Contoso también realiza revisiones mensuales de los usuarios asignados a los roles de administrador y los revisores son los administradores de los usuarios de los cuales seis no están en roles de administrador administrados por PIM. | 42 licencias para los roles válidos + 5 aprobadores + 6 revisores | 53 |

## <a name="what-happens-when-a-license-expires"></a>¿Qué sucede cuando expira una licencia?

Si expira una licencia de Azure AD Premium P2, EMS E5 o una licencia de evaluación, las características de Privileged Identity Management ya no estarán disponibles en el directorio:

- Las asignaciones de roles permanentes para roles de Azure AD no se verán afectadas.
- El servicio de Privileged Identity Management en Azure Portal, así como los cmdlets de Graph API y las interfaces de PowerShell de Privileged Identity Management, ya no estarán disponibles para que los usuarios activen roles con privilegios, administren el acceso con privilegios o realicen revisiones de acceso de roles con privilegios.
- Se quitarán las asignaciones de roles elegibles de roles de Azure AD, puesto que los usuarios ya no podrán activar roles con privilegios.
- Las revisiones de acceso en curso de roles de Azure AD finalizarán y las opciones de configuración de Privileged Identity Management se quitarán.
- Privileged Identity Management no volverá a enviar correos electrónicos cuando se produzcan cambios de asignación de rol.

## <a name="next-steps"></a>Pasos siguientes

- [Implementación de Privileged Identity Management](pim-deployment-plan.md)
- [Empiece a usar Privileged Identity Management](pim-getting-started.md)
- [Roles que no se pueden administrar en Privileged Identity Management](pim-roles.md)

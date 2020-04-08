---
title: Enumeración de las asignaciones de denegación para recursos de Azure con la API de REST
description: Aprenda a enumerar las asignaciones de denegación para usuarios, grupos y aplicaciones mediante el control de acceso basado en rol (RBAC) para recursos de Azure y la API REST.
services: active-directory
documentationcenter: na
author: rolyon
manager: mtillman
editor: ''
ms.assetid: ''
ms.service: role-based-access-control
ms.workload: multiple
ms.tgt_pltfrm: rest-api
ms.devlang: na
ms.topic: conceptual
ms.date: 03/19/2020
ms.author: rolyon
ms.reviewer: bagovind
ms.openlocfilehash: 0f648405a3d71bf27c64dacbb3fd78f3e9801137
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "80063031"
---
# <a name="list-deny-assignments-for-azure-resources-using-the-rest-api"></a>Enumeración de las asignaciones de denegación para recursos de Azure mediante la API de REST

Las [asignaciones de denegación](deny-assignments.md) impiden que los usuarios realicen acciones concretas en recursos de Azure, aunque una asignación de roles les conceda acceso. En este artículo se describe cómo enumerar las asignaciones de denegación mediante la API REST.

> [!NOTE]
> No se pueden crear directamente asignaciones de denegación propias. Para obtener información sobre cómo se crean las asignaciones de denegación, vea [Asignaciones de denegación](deny-assignments.md).

## <a name="prerequisites"></a>Prerrequisitos

Para obtener información sobre una asignación de denegación, debe tener lo siguiente:

- El permiso `Microsoft.Authorization/denyAssignments/read`, que se incluye en la mayoría de [roles integrados para los recursos de Azure](built-in-roles.md).

## <a name="list-a-single-deny-assignment"></a>Lista de una sola asignación de denegación

1. Empiece con la solicitud siguiente:

    ```http
    GET https://management.azure.com/{scope}/providers/Microsoft.Authorization/denyAssignments/{deny-assignment-id}?api-version=2018-07-01-preview
    ```

1. En el identificador URI, reemplace *{scope}* por el ámbito cuya lista de asignaciones de denegación quiere obtener.

    > [!div class="mx-tableFixed"]
    > | Ámbito | Tipo |
    > | --- | --- |
    > | `subscriptions/{subscriptionId}` | Subscription |
    > | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1` | Resource group |
    > | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1` | Resource |

1. Reemplace *{deny-assignment-id}* por el identificador de asignación de denegación que desea recuperar.

## <a name="list-multiple-deny-assignments"></a>Lista de varias asignaciones de denegación

1. Comience con una de las siguientes solicitudes:

    ```http
    GET https://management.azure.com/{scope}/providers/Microsoft.Authorization/denyAssignments?api-version=2018-07-01-preview
    ```

    Con parámetros opcionales:

    ```http
    GET https://management.azure.com/{scope}/providers/Microsoft.Authorization/denyAssignments?api-version=2018-07-01-preview&$filter={filter}
    ```

1. En el identificador URI, reemplace *{scope}* por el ámbito cuya lista de asignaciones de denegación quiere obtener.

    > [!div class="mx-tableFixed"]
    > | Ámbito | Tipo |
    > | --- | --- |
    > | `subscriptions/{subscriptionId}` | Subscription |
    > | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1` | Resource group |
    > | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1` | Resource |

1. Reemplace *{filter}* por la condición que quiere aplicar para filtrar la lista de asignaciones de denegación.

    > [!div class="mx-tableFixed"]
    > | Filter | Descripción |
    > | --- | --- |
    > | (sin filtro) | Lista de todas las asignaciones de denegación del ámbito especificado, y también por encima y por debajo. |
    > | `$filter=atScope()` | Lista de las asignaciones de denegación del ámbito especificado y por encima. No incluye las asignaciones de denegación de ámbitos secundarios. |
    > | `$filter=assignedTo('{objectId}')` | Lista de las asignaciones de denegación para el usuario o la entidad de servicio especificados.<br/>Si el usuario es miembro de un grupo que tiene una asignación de denegación, también se muestra esa asignación de denegación. Este filtro es transitivo para los grupos, lo que significa que, si el usuario es miembro de un grupo y ese grupo es miembro de otro grupo que tiene una asignación de denegación, también se muestra esa asignación de denegación.<br/>Este filtro solo acepta un identificador de objeto para un usuario o una entidad de servicio. No se puede pasar un identificador de objeto para un grupo. |
    > | `$filter=atScope()+and+assignedTo('{objectId}')` | Muestra las asignaciones de denegación para el usuario o la entidad de servicio determinados y en el ámbito especificado. |
    > | `$filter=denyAssignmentName+eq+'{deny-assignment-name}'` | Lista de asignaciones de denegación con el nombre especificado. |
    > | `$filter=principalId+eq+'{objectId}'` | Lista de las asignaciones de denegación para el usuario, el grupo o la entidad de servicio determinados. |

## <a name="list-deny-assignments-at-the-root-scope-"></a>Enumeración de las asignaciones de denegación en el ámbito raíz (/)

1. Eleve el privilegio de acceso como se describe en [Elevación de los privilegios de acceso de un administrador global en Azure Active Directory](elevate-access-global-admin.md).

1. Use la siguiente solicitud:

    ```http
    GET https://management.azure.com/providers/Microsoft.Authorization/denyAssignments?api-version=2018-07-01-preview&$filter={filter}
    ```

1. Reemplace *{filter}* por la condición que quiere aplicar para filtrar la lista de asignaciones de denegación. Se requiere un filtro.

    > [!div class="mx-tableFixed"]
    > | Filter | Descripción |
    > | --- | --- |
    > | `$filter=atScope()` | Lista de asignaciones de denegación solo en el ámbito raíz. No incluye las asignaciones de denegación de ámbitos secundarios. |
    > | `$filter=denyAssignmentName+eq+'{deny-assignment-name}'` | Lista de asignaciones de denegación con el nombre especificado. |

1. Elimine los privilegios de acceso elevados.

## <a name="next-steps"></a>Pasos siguientes

- [Descripción de las asignaciones de denegación para recursos de Azure](deny-assignments.md)
- [Elevación de los privilegios de acceso de un administrador global en Azure Active Directory](elevate-access-global-admin.md)
- [Azure REST API Reference](/rest/api/azure/) (Referencia de API de REST en Azure)

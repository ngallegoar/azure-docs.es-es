---
title: Enumeración de las asignaciones de denegación para recursos de Azure con Azure Portal
description: Aprenda cómo enumerar los usuarios, grupos, entidades de servicio e identidades administradas a los que se les ha denegado el acceso a acciones específicas en recursos de Azure en un ámbito determinado mediante Azure Portal.
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
ms.date: 06/10/2019
ms.author: rolyon
ms.reviewer: bagovind
ms.openlocfilehash: 4db76e5c6191457346ca1f95678cf73843334d3b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "77137423"
---
# <a name="list-deny-assignments-for-azure-resources-using-the-azure-portal"></a>Enumeración de las asignaciones de denegación para recursos de Azure mediante Azure Portal

Las [asignaciones de denegación](deny-assignments.md) impiden que los usuarios realicen acciones concretas en recursos de Azure, aunque una asignación de roles les conceda acceso. En este artículo se describe cómo enumerar las asignaciones de denegación mediante Azure Portal.

> [!NOTE]
> No se pueden crear directamente asignaciones de denegación propias. Para obtener información sobre cómo se crean las asignaciones de denegación, vea [Asignaciones de denegación](deny-assignments.md).

## <a name="prerequisites"></a>Prerequisites

Para obtener información sobre una asignación de denegación, debe tener lo siguiente:

- El permiso `Microsoft.Authorization/denyAssignments/read`, que se incluye en la mayoría de [roles integrados para los recursos de Azure](built-in-roles.md).

## <a name="list-deny-assignments"></a>Enumeración de asignaciones de denegación

Siga estos pasos para enumerar las asignaciones de denegación en el ámbito de la suscripción o del grupo de administración.

1. En Azure Portal, haga clic en **Todos los servicios** y, después, en **Grupos de administración** o **Suscripciones**.

1. Haga clic en el grupo de administración o suscripción que quiera enumerar.

1. Haga clic en **Control de acceso (IAM)** .

1. Haga clic en la pestaña **Asignaciones de denegación** (o haga clic en el botón **Ver** en el icono Ver asignaciones de denegación).

    Si hay asignaciones denegadas en este ámbito o heredadas en este ámbito, se mostrarán.

    ![Control de acceso: pestaña Asignaciones de denegación](./media/deny-assignments-portal/access-control-deny-assignments.png)

1. Para mostrar las columnas adicionales, haga clic en **Editar columnas**.

    ![Asignaciones de denegación: columnas](./media/deny-assignments-portal/deny-assignments-columns.png)

    |  |  |
    | --- | --- |
    | **Nombre** | Nombre de la asignación de denegación. |
    | **Tipo de entidad de seguridad** | Usuario, grupo, grupo definido por el sistema o entidad de servicio. |
    | **Se deniega**  | Nombre de la entidad de seguridad que se incluye en la asignación de denegación. |
    | **Id** | Identificador único para la asignación de denegación. |
    | **Entidades de seguridad excluidas** | Si hay entidades de seguridad que se excluyen de la asignación de denegación. |
    | **No se aplica a los elementos secundarios** | Si la asignación de denegación se hereda en los ámbitos secundarios. |
    | **Protegida por el sistema** | Si Azure administra la asignación de denegación. Actualmente, siempre es sí. |
    | **Ámbito** | Grupo de administración, suscripción, grupo de recursos o identificador de recurso. |

1. Agregue una marca de verificación a cualquiera de los elementos habilitados y, a continuación, haga clic en **Aceptar** para mostrar las columnas seleccionadas.

## <a name="list-details-about-a-deny-assignment"></a>Enumeración de detalles sobre una asignación de denegación

Siga estos pasos para enumerar detalles adicionales sobre una asignación de denegación.

1. Abra el panel **Asignaciones de denegación**, tal como se describe en la sección anterior.

1. Haga clic en el nombre de asignación de denegación para abrir la hoja **Usuarios**.

    ![Asignación de denegación: Usuarios](./media/deny-assignments-portal/deny-assignment-users.png)

    La hoja **Usuarios** incluye las dos secciones siguientes.

    |  |  |
    | --- | --- |
    | **La asignación de denegación se aplica a**  | Entidades de seguridad a las que se aplica la asignación de denegación. |
    | **La asignación de denegación excluye** | Entidades de seguridad que se excluyen de la asignación de denegación. |

    **La entidad de seguridad definida por el sistema** representa a todos los usuarios, grupos, entidades de servicio e identidades administradas de un directorio de Azure AD.

1. Para ver una lista de los permisos que se deniegan, haga clic en **Permisos denegados**.

    ![Asignación de denegación: Permisos denegados](./media/deny-assignments-portal/deny-assignment-denied-permissions.png)

    | Tipo de acción | Descripción |
    | --- | --- |
    | **Acciones**  | Operaciones de administración denegadas. |
    | **NotActions** | Operaciones de administración que se excluyen de la operación de administración denegada. |
    | **DataActions**  | Operaciones de datos denegadas. |
    | **NotDataActions** | Operaciones de datos que se excluyen de la operación de datos denegada. |

    Para el ejemplo mostrado en la captura de pantalla anterior, los siguientes son los permisos efectivos:

    - Se deniegan todas las operaciones de almacenamiento en el plano de datos, excepto las operaciones de proceso.

1. Para ver las propiedades de una asignación de denegación, haga clic en **Propiedades**.

    ![Asignación de denegación: Propiedades](./media/deny-assignments-portal/deny-assignment-properties.png)

    En la hoja **Propiedades**, puede ver el nombre, el identificador, la descripción y el ámbito de la asignación de denegación. El modificador **No se aplica a los elementos secundarios** indica si la asignación de denegación se hereda a los ámbitos secundarios. El modificador **Protegida por el sistema** indica si Azure administra esta asignación de denegación. Actualmente, es **Sí** en todos los casos.

## <a name="next-steps"></a>Pasos siguientes

* [Descripción de las asignaciones de denegación para recursos de Azure](deny-assignments.md)
* [Enumeración de las asignaciones de denegación para recursos de Azure mediante Azure PowerShell](deny-assignments-powershell.md)

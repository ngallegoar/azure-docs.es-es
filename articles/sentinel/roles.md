---
title: Permisos de Azure Sentinel | Microsoft Docs
description: En este artículo se explica cómo Azure Sentinel usa el control de acceso basado en roles para asignar permisos a los usuarios e identifica las acciones permitidas para cada rol.
services: sentinel
cloud: na
documentationcenter: na
author: yelevin
manager: angrobe
ms.assetid: ''
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 12/09/2019
ms.author: yelevin
ms.openlocfilehash: 9fa10dd1b278b48eb714affb74af59140c8baa09
ms.sourcegitcommit: 223cea58a527270fe60f5e2235f4146aea27af32
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/01/2020
ms.locfileid: "84259344"
---
# <a name="permissions-in-azure-sentinel"></a>Permisos de Azure Sentinel

Azure Sentinel usa el  [control de acceso basado en rol (RBAC)](../role-based-access-control/role-assignments-portal.md), para proporcionar  [roles integrados](../role-based-access-control/built-in-roles.md)  que se pueden asignar a usuarios, grupos y servicios en Azure.

Con RBAC, puede usar y crear roles dentro del equipo de operaciones de seguridad para conceder el acceso adecuado a Azure Sentinel. Según los roles, puede tener un control pormenorizado sobre lo que los usuarios con acceso a Azure Sentinel pueden ver. Puede asignar roles de RBAC en el área de trabajo de Azure Sentinel directamente o a una suscripción o grupo de recursos al que pertenezca el área de trabajo.

Hay tres roles de Azure Sentinel integrados específicos.  
**Todos los roles integrados de Azure Sentinel conceden acceso de lectura a los datos en el área de trabajo de Azure Sentinel.**
- [Lector de Azure Sentinel](../role-based-access-control/built-in-roles.md#azure-sentinel-reader)
- [Respondedor de Azure Sentinel](../role-based-access-control/built-in-roles.md#azure-sentinel-responder)
- [Colaborador de Azure Sentinel](../role-based-access-control/built-in-roles.md#azure-sentinel-contributor)

Además de los roles de RBAC dedicados de Azure Sentinel, hay roles de RBAC de Azure y Log Analytics que pueden conceder un conjunto más amplio de permisos que incluye el acceso al área de trabajo de Azure Sentinel y a otros recursos:

- **Roles de Azure:** [Propietario](../role-based-access-control/built-in-roles.md#owner), [colaborador](../role-based-access-control/built-in-roles.md#contributor) y [lector](../role-based-access-control/built-in-roles.md#reader). Los roles de Azure conceden acceso a todos los recursos de Azure, incluidas las áreas de trabajo de Log Analytics y los recursos de Azure Sentinel.

-   **Roles de Log Analytics:** [Colaborador de Log Analytics](../role-based-access-control/built-in-roles.md#log-analytics-contributor), [Lector de Log Analytics](../role-based-access-control/built-in-roles.md#log-analytics-reader). Los roles de Log Analytics conceden acceso a todas las áreas de trabajo de Log Analytics. 

> [!NOTE]
> Los roles de Log Analytics también conceden acceso de lectura a todos los recursos de Azure, pero solo asignarán permisos de escritura a recursos de Log Analytics.


Por ejemplo, a un usuario al que se le han asignado los roles de **Lector de Azure Sentinel** y **Colaborador de Azure** (no **Colaborador de Azure Sentinel**), podrá editar datos en Azure Sentinel, aunque solo tenga permisos de **Lector de Sentinel**. Por tanto, si desea conceder permisos para un usuario solo en Azure Sentinel, elimine con cuidado los permisos anteriores de este usuario, asegurándose de que no interrumpe ningún rol de permiso necesario para otro recurso.

> [!NOTE]
>- Azure Sentinel usa cuadernos de estrategias para una respuesta automatizada ante amenazas. Los cuadernos de estrategias emplean instancias de Azure Logic Apps y son recursos independientes de Azure. Es posible que desee asignar miembros específicos del equipo de operaciones de seguridad con la opción de usar Logic Apps para las operaciones de orquestación de seguridad, automatización y respuesta (SOAR). Puede usar el rol de [colaborador de Logic App](../role-based-access-control/built-in-roles.md#logic-app-contributor) o el de [operador de Logic App](../role-based-access-control/built-in-roles.md#logic-app-operator) para asignar permisos explícitos para el uso de cuadernos de estrategias.
>- Para agregar conectores de datos, los roles necesarios para cada conector son por tipo de conector y se enumeran en la página del conector correspondiente. Además, para poder conectarse a cualquier origen de datos, debe tener permiso de escritura en el área de trabajo de Azure Sentinel.



## <a name="roles-and-allowed-actions"></a>Roles y acciones permitidas

En la siguiente tabla se muestran los roles y las acciones permitidas en Azure Sentinel. Una X indica que la acción se permite para ese rol.

| Role | Creación y ejecución de cuadernos de estrategias| Creación y edición de paneles, reglas analíticas y otros recursos de Azure Sentinel | Administración de incidentes (descarte, asignación, etc.) | Visualización de datos, incidentes, paneles y otros recursos de Azure Sentinel |
|--- |---|---|---|---|
| Lector de Azure Sentinel | -- | -- | -- | X |
| Respondedor de Azure Sentinel|--|--| X | X |
| Colaborador de Azure Sentinel | -- | X | X | X |
| Colaborador de Azure Sentinel y Colaborador de Logic Apps | X | X | X | X |


> [!NOTE]
> - Es recomendable que asigne el rol de menos permisos que los usuarios necesiten para realizar sus tareas. Por ejemplo, asigne el rol de colaborador de Azure Sentinel solo a los usuarios que necesiten crear reglas o paneles.
> - Se recomienda establecer permisos para Azure Sentinel en el ámbito del grupo de recursos, de modo que el usuario pueda tener acceso a todas las áreas de trabajo de Azure Sentinel del mismo grupo de recursos.
>
## <a name="building-custom-rbac-roles"></a>Creación de roles personalizados de RBAC

Además de usar roles de RBAC integrados (o en vez de ello), puede crear roles de RBAC personalizados para Azure Sentinel. Los roles de RBAC personalizados para Azure Sentinel se crean de la misma manera que se crean otros roles [personalizados de RBAC de Azure](../role-based-access-control/custom-roles-rest.md#create-a-custom-role) basados en [permisos específicos para los recursos de Azure Sentinel](../role-based-access-control/resource-provider-operations.md#microsoftsecurityinsights) y [Azure Log Analytics](../role-based-access-control/resource-provider-operations.md#microsoftoperationalinsights).

## <a name="advanced-rbac-on-the-data-you-store-in-azure-sentinel"></a>RBAC avanzado en los datos que almacena en Azure Sentinel
  
Puede usar el control de acceso basado en rol avanzado de Log Analytics en los datos del área de trabajo de Azure Sentinel. Esto incluye el control de acceso basado en rol por tipo de datos y el centrado en recursos. Para más información sobre los roles de Log Analytics, consulte  [Administración de los datos de registro y las áreas de trabajo en Azure Monitor](../azure-monitor/platform/manage-access.md#manage-access-using-workspace-permissions).

## <a name="next-steps"></a>Pasos siguientes
En este documento, ha aprendido a trabajar con roles para usuarios de Azure Sentinel y lo que cada rol permite realizar a los usuarios.

* [Blog de Azure Sentinel](https://aka.ms/azuresentinelblog). Encuentre artículos de blog sobre el cumplimiento y la seguridad de Azure.

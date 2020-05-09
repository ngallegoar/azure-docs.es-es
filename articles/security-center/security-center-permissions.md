---
title: Permisos en Azure Security Center | Microsoft Docs
description: En este artículo se explica cómo Azure Security Center usa el control de acceso basado en roles para asignar permisos a los usuarios e identifica las acciones permitidas para cada rol.
services: security-center
cloud: na
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: ''
ms.service: security-center
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 10/28/2018
ms.author: memildin
ms.openlocfilehash: 6e61571400930d4a781d6d67647bd662a7f2d350
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/28/2020
ms.locfileid: "82106226"
---
# <a name="permissions-in-azure-security-center"></a>Permisos en Azure Security Center

Azure Security Center usa el [control de acceso basado en roles (RBAC)](../role-based-access-control/role-assignments-portal.md), que proporciona [roles integrados](../role-based-access-control/built-in-roles.md) que se pueden asignar a usuarios, grupos y servicios de Azure.

Security Center evalúa la configuración de los recursos para identificar problemas de seguridad y vulnerabilidades. En Security Center, solo se muestra información relacionada con un recurso cuando tiene asignado el rol de Propietario, Colaborador o Lector a la suscripción o grupo de recursos al que pertenece un recurso.

Además de estos roles, hay dos roles específicos de Security Center:

* **Lector de seguridad**: un usuario que pertenece a este rol tiene derechos de visualización en Security Center. El usuario puede ver las recomendaciones, las alertas, una directiva de seguridad y los estados de seguridad, pero no puede realizar cambios.
* **Administrador de seguridad**: un usuario que pertenece a este rol tiene los mismos derechos que el lector de seguridad, y puede actualizar la directiva de seguridad y descartar las alertas y las recomendaciones.

> [!NOTE]
> Los roles de seguridad Lector de seguridad y Administrador de seguridad solo tienen acceso a Security Center. Los roles de seguridad descritos no tienen acceso a otras áreas de servicio de Azure, como Storage, Web y móvil o Internet de las cosas.
>
>

## <a name="roles-and-allowed-actions"></a>Roles y acciones permitidas

En la siguiente tabla se muestran los roles y las acciones permitidas en Security Center.

| Role | Editar directivas de seguridad | Aplicar recomendaciones de seguridad en un recurso</br> (incluido con "Quick Fix!" [Corrección rápida]) | Descartar alertas y recomendaciones | Ver alertas y recomendaciones |
|:--- |:---:|:---:|:---:|:---:|
| Propietario de la suscripción | ✔ | ✔ | ✔ | ✔ |
| Colaborador de la suscripción | -- | ✔ | ✔ | ✔ |
| Propietario del grupo de recursos | -- | ✔ | -- | ✔ |
| Colaborador del grupo de recursos | -- | ✔ | -- | ✔ |
| Lector | -- | -- | -- | ✔ |
| Administrador de seguridad | ✔ | -- | ✔ | ✔ |
| Lector de seguridad | -- | -- | -- | ✔ |

> [!NOTE]
> Es recomendable que asigne el rol de menos permisos que los usuarios necesiten para realizar sus tareas. Por ejemplo, asigne el rol Lector a los usuarios que solo necesiten ver información sobre el estado de seguridad de los recursos, pero no llevar a cabo acciones como aplicar recomendaciones o editar directivas.
>
>

## <a name="next-steps"></a>Pasos siguientes
En este artículo se ha explicado cómo Security Center usa RBAC para asignar permisos a los usuarios e identifica las acciones permitidas para cada rol. Ahora que ya está familiarizado con las asignaciones de roles necesarios para supervisar el estado de seguridad de su suscripción, editar directivas de seguridad y aplicar recomendaciones, aprenderá los siguientes conceptos:

- [Establecer directivas de seguridad en Security Center](tutorial-security-policy.md)
- [Administrar recomendaciones de seguridad en Security Center](security-center-recommendations.md)
- [Supervisar el estado de seguridad de los recursos de Azure](security-center-monitoring.md)
- [Responder a alertas de seguridad en Security Center](security-center-managing-and-responding-alerts.md)
- [Supervisar soluciones de seguridad de asociados](security-center-partner-solutions.md)

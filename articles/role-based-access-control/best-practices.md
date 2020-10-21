---
title: Procedimientos recomendados para Azure RBAC
description: Procedimientos recomendados para usar el control de acceso basado en roles de Azure (Azure RBAC).
services: active-directory
author: rolyon
manager: mtillman
ms.service: role-based-access-control
ms.topic: conceptual
ms.workload: identity
ms.date: 09/30/2020
ms.author: rolyon
ms.openlocfilehash: 4cb3e1fe0275c676e2ce54ff9201502fc3595937
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "91595501"
---
# <a name="best-practices-for-azure-rbac"></a>Procedimientos recomendados para Azure RBAC

En este artículo se describen algunos procedimientos recomendados para usar el control de acceso basado en roles de Azure (Azure RBAC). Estos procedimientos recomendados proceden de nuestra experiencia con Azure RBAC y las experiencias de clientes como usted.

## <a name="only-grant-the-access-users-need"></a>Solo se debe conceder el acceso que los usuarios necesitan

Con Azure RBAC, puede repartir las tareas entre el equipo y conceder a los usuarios únicamente el nivel de acceso que necesitan para realizar su trabajo. En lugar de proporcionar a todos los empleados permisos no restringidos en los recursos o la suscripción de Azure, puede permitir solo determinadas acciones en un ámbito concreto.

Al planear la estrategia de control de acceso, es recomendable conceder a los usuarios el privilegio mínimo para que realicen su trabajo. Evite la asignación de roles más amplios en ámbitos más amplios, aunque inicialmente parezca más práctico hacerlo. Al limitar los roles y los ámbitos, se limitan los recursos en peligro si la entidad de seguridad llegara a verse comprometida.

El siguiente diagrama muestra un patrón sugerido para la asignación de RBAC de Azure.

![RBAC de Azure y privilegios mínimos](./media/best-practices/rbac-least-privilege.png)

Para más información sobre cómo agregar asignaciones de roles, consulte [Incorporación o eliminación de asignaciones de roles mediante Azure Portal](role-assignments-portal.md).

## <a name="limit-the-number-of-subscription-owners"></a>Limitación del número de propietarios de suscripciones

Debe tener un máximo de 3 propietarios de suscripción para reducir el riesgo de vulneración por parte de un propietario en peligro. Esta recomendación se puede supervisar en Azure Security Center. Para otras recomendaciones de identidad y acceso en Security Center, consulte [Guía de referencia sobre las recomendaciones de seguridad](../security-center/recommendations-reference.md).

## <a name="use-azure-ad-privileged-identity-management"></a>Utilice Azure AD Privileged Identity Management

Para proteger las cuentas con privilegios de ciberataques malintencionados, puede usar Azure Active Directory Privileged Identity Management (PIM) para reducir el tiempo de exposición de los privilegios y aumentar la visibilidad de su uso a través de alertas e informes. PIM ayuda a proteger las cuentas con privilegios, ya que proporciona acceso con privilegios Just-in-Time a Azure AD y recursos de Azure. El acceso puede estar limitado por el tiempo, después del cual los privilegios se revocan automáticamente. 

Para más información, vea [¿Qué es Azure AD Privileged Identity Management?](../active-directory/privileged-identity-management/pim-configure.md).

## <a name="next-steps"></a>Pasos siguientes

- [Solución de problemas de Azure RBAC](troubleshooting.md)
---
title: Configuración de alertas de seguridad para roles de recursos de Azure en Privileged Identity Management Azure Active Directory | Microsoft Docs
description: Aprenda a configurar alertas de seguridad para los roles de recurso de Azure en Azure AD Privileged Identity Management (PIM).
services: active-directory
documentationcenter: ''
author: curtand
manager: daveba
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.subservice: pim
ms.date: 11/08/2019
ms.author: curtand
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: f0b938dc808d9b02ad4105d85a5b3125135c51d6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "74023085"
---
# <a name="configure-security-alerts-for-azure-resource-roles-in-privileged-identity-management"></a>Configuración de alertas de seguridad para roles de recursos de Azure en Privileged Identity Management

Privileged Identity Management (PIM) genera alertas cuando existen actividades sospechosas o no seguras en su entorno de organización Azure Active Directory (Azure AD). Cuando se desencadena una alerta, se muestra en la página de alertas.

![Recursos de Azure: página de alertas que muestra la alerta, el nivel de riesgo y el recuento](media/pim-resource-roles-configure-alerts/rbac-alerts-page.png)

## <a name="review-alerts"></a>Revisar alertas

Seleccione una alerta para ver un informe que muestre los usuarios o los roles que desencadenó la alerta, junto con la guía de corrección.

![Informe de alertas que muestra el último análisis de tiempo, la descripción, los pasos de mitigación, el tipo, la gravedad, el impacto de seguridad y cómo impedir que vuelva a pasar](media/pim-resource-roles-configure-alerts/rbac-alert-info.png)

## <a name="alerts"></a>Alertas

| Alerta | severity | Desencadenador | Recomendación |
| --- | --- | --- | --- |
| **Hay demasiados propietarios asignados a un recurso** |Media |Demasiados usuarios tienen el rol de propietario. |Revise los usuarios de la lista y vuelva a asignar algunos a roles con menos privilegios. |
| **Hay demasiados propietarios permanentes asignados a un recurso** |Media |Demasiados usuarios están asignados permanentemente a un rol. |Revise los usuarios de la lista y vuelva a asignar algunos para que tengan que solicitar la activación para usar el rol. |
| **Se ha creado un rol duplicado** |Media |Varios roles tienen los mismos criterios. |Use solo uno de estos roles. |

### <a name="severity"></a>severity

- **Alta**: requiere acción inmediata debido a la infracción de una directiva. 
- **Media**: no requiere acción inmediata, pero indica una posible infracción de una directiva.
- **Baja**: no requiere acción inmediata pero sugiere un cambio de directiva preferida.

## <a name="configure-security-alert-settings"></a>Configuración de alertas de seguridad

En la página de alertas, vaya a **Configuración**.

![Página de alertas con la opción Configuración resaltada](media/pim-resource-roles-configure-alerts/rbac-navigate-settings.png)

Personalice la configuración de las diferentes alertas para que encajen con su entorno y con sus objetivos de seguridad.

![Página de configuración de una alerta para habilitar y configurar los valores](media/pim-resource-roles-configure-alerts/rbac-alert-settings.png)

## <a name="next-steps"></a>Pasos siguientes

- [Configuración de las opciones de rol de recursos de Azure en Privileged Identity Management](pim-resource-roles-configure-role-settings.md)

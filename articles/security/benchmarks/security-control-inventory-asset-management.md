---
title: 'Control de seguridad de Azure: inventario y administración de recursos'
description: Administración de recursos e inventario de control de seguridad
author: msmbaldwin
manager: rkarlin
ms.service: security
ms.topic: conceptual
ms.date: 12/30/2019
ms.author: mbaldwin
ms.custom: security-recommendations
ms.openlocfilehash: 2f24012a69a7673426644a8ded6162cd1021938e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "75930057"
---
# <a name="security-control-inventory-and-asset-management"></a>Control de seguridad: Administración de recursos y del inventario

Las recomendaciones de administración de recursos e inventario se centran en solucionar problemas relacionados con la administración activa (inventario, seguimiento y corrección) de todos los recursos de Azure, de forma que solo se concede acceso a los recursos autorizados, y los recursos no autorizados y no administrados se identifican y eliminan.

## <a name="61-use-azure-asset-discovery"></a>6.1: Uso de la detección de recursos de Azure

| Identificador de Azure | Id. de CIS | Responsabilidad |
|--|--|--|
| 6.1 | 1.1, 1.2, 1.3, 1.4, 9.1, 12.1 | Customer |

Use Azure Resource Graph para consultar o detectar todos los recursos (por ejemplo, proceso, almacenamiento, red, puertos y protocolos, etc.) dentro de las suscripciones.  Asegúrese de que tiene los permisos adecuados (lectura) en el inquilino y enumere todas las suscripciones de Azure, así como los recursos de las suscripciones.

Aunque los recursos clásicos de Azure se pueden detectar a través de Resource Graph, se recomienda encarecidamente crear y usar los recursos de Azure Resource Manager que figuran a continuación.

Creación de consultas con Azure Resource Graph:

https://docs.microsoft.com/azure/governance/resource-graph/first-query-portal

Visualización de las suscripciones de Azure:

https://docs.microsoft.com/powershell/module/az.accounts/get-azsubscription?view=azps-3.0.0

Descripción de Azure RBAC:

https://docs.microsoft.com/azure/role-based-access-control/overview

## <a name="62-maintain-asset-metadata"></a>6.2: Mantenimiento de metadatos de recursos

| Identificador de Azure | Id. de CIS | Responsabilidad |
|--|--|--|
| 6.2 | 1.5 | Customer |

Aplique etiquetas a los recursos de Azure que proporcionan metadatos para organizarlos de forma lógica en una taxonomía.

Creación y uso de etiquetas:

https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags

## <a name="63-delete-unauthorized-azure-resources"></a>6.3: Eliminación de recursos de Azure no autorizados

| Identificador de Azure | Id. de CIS | Responsabilidad |
|--|--|--|
| 6.3 | 1.6 | Customer |

Use el etiquetado, los grupos de administración y las suscripciones independientes, si procede, para organizar y realizar un seguimiento de los recursos. Concilie el inventario periódicamente y asegúrese de que los recursos no autorizados se eliminan de la suscripción de manera oportuna.

Creación de suscripciones de Azure adicionales:

https://docs.microsoft.com/azure/billing/billing-create-subscription

Cómo crear grupos de administración:

https://docs.microsoft.com/azure/governance/management-groups/create

Creación y uso de etiquetas:

https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags

## <a name="64-maintain-an-inventory-of-approved-azure-resources-and-software-titles"></a>6.4: Mantenimiento de un inventario de los recursos de Azure aprobados y títulos de software

| Identificador de Azure | Id. de CIS | Responsabilidad |
|--|--|--|
| 6.4 | 2.1 | Customer |

Defina los recursos de Azure y el software aprobados para los recursos de proceso.

## <a name="65-monitor-for-unapproved-azure-resources"></a>6.5: Supervisión de recursos de Azure no aprobados

| Identificador de Azure | Id. de CIS | Responsabilidad |
|--|--|--|
| 6.5 | 2.3, 2.4 | Customer |

Use Azure Policy para establecer restricciones en el tipo de recursos que se pueden crear en sus suscripciones.

Use Azure Resource Graph para consultar o detectar recursos dentro de sus suscripciones. &nbsp;Asegúrese de que todos los recursos de Azure presentes en el entorno estén aprobados.

Configuración y administración de Azure Policy:

https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage

Creación de consultas con Azure Graph:

https://docs.microsoft.com/azure/governance/resource-graph/first-query-portal

## <a name="66-monitor-for-unapproved-software-applications-within-compute-resources"></a>6.6: Supervisión de aplicaciones de software no aprobadas en recursos de proceso

| Identificador de Azure | Id. de CIS | Responsabilidad |
|--|--|--|
| 6.6 | 2.3/2.4 | Customer |

Use el inventario de máquinas virtuales de Azure para automatizar la recopilación de información sobre todo el software en Virtual Machines. El nombre del software, la versión, el publicador y la hora de actualización están disponibles en Azure Portal. Para obtener acceso a la fecha de instalación y otra información, habilite los diagnósticos de nivel de invitado y transfiera los registros de eventos de Windows a un área de trabajo de Log Analytics.

Habilitación del inventario de máquinas virtuales de Azure:

https://docs.microsoft.com/azure/automation/automation-tutorial-installed-software

## <a name="67-remove-unapproved-azure-resources-and-software-applications"></a>6.7: Eliminación de aplicaciones de software y recursos de Azure no aprobadas

| Identificador de Azure | Id. de CIS | Responsabilidad |
|--|--|--|
| 6.7 | 2.5 | Customer |

Use la supervisión de integridad de archivos (Change Tracking) y el inventario de máquinas virtuales de Azure Security Center para identificar todo el software instalado en Virtual Machines. Puede implementar su propio proceso para quitar software no autorizado. También puede usar una solución de terceros para identificar software no aprobado.

Habilitación de la supervisión de la integridad de los archivos:

https://docs.microsoft.com/azure/security-center/security-center-file-integrity-monitoring#using-file-integrity-monitoring

Descripción de Azure Change Tracking:

https://docs.microsoft.com/azure/automation/change-tracking

Habilitación del inventario de máquinas virtuales de Azure:

https://docs.microsoft.com/azure/automation/automation-tutorial-installed-software

## <a name="68-use-only-approved-applications"></a>6.8: Uso exclusivo de aplicaciones aprobadas

| Identificador de Azure | Id. de CIS | Responsabilidad |
|--|--|--|
| 6,8 | 2.6 | Customer |

Use los controles de aplicación adaptables de Azure Security Center para asegurarse de que solo se ejecute software autorizado y de que todo el software no autorizado no se ejecute en Azure Virtual Machines.

Uso de los controles de aplicaciones adaptables de Azure Security Center:

https://docs.microsoft.com/azure/security-center/security-center-adaptive-application

## <a name="69-use-only-approved-azure-services"></a>6.9: Uso exclusivo de servicios de Azure aprobados

| Identificador de Azure | Id. de CIS | Responsabilidad |
|--|--|--|
| 6.9 | 2.6 | Customer |

Use Azure Policy para restringir qué servicios puede aprovisionar en su entorno.

Configuración y administración de Azure Policy:

https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage

Denegación de un tipo de recurso específico con Azure Policy:

https://docs.microsoft.com/azure/governance/policy/samples/not-allowed-resource-types

## <a name="610-implement-approved-application-list"></a>6.10: Implementación de la lista de aplicaciones aprobadas

| Identificador de Azure | Id. de CIS | Responsabilidad |
|--|--|--|
| 6.1 | 2.7 | Customer |

Use los controles de aplicaciones adaptables de Azure Security Center para especificar en qué tipos de archivo puede aplicarse una regla o no.

Implemente una solución de terceros si no cumple el requisito.

Uso de los controles de aplicaciones adaptables de Azure Security Center:

https://docs.microsoft.com/azure/security-center/security-center-adaptive-application

## <a name="611-limit-users-ability-to-interact-with-azure-resource-manager-via-scripts"></a>6.11: Limitación de la capacidad de los usuarios para interactuar con Azure Resource Manager a través de scripts

| Identificador de Azure | Id. de CIS | Responsabilidad |
|--|--|--|
| 6.11 | 2.8 | Customer |

Use el acceso condicional de Azure para limitar la capacidad de los usuarios de interactuar con Azure Resource Manager configurando &quot;Bloquear acceso&quot; en la aplicación &quot;Microsoft Azure Management&quot;.

Configuración del acceso condicional para bloquear el acceso a Azure Resource Manager:

https://docs.microsoft.com/azure/role-based-access-control/conditional-access-azure-management

## <a name="612-limit-users-ability-to-execute-scripts-within-compute-resources"></a>6.12: Limitación de capacidad de los usuarios para ejecutar scripts en recursos de proceso

| Identificador de Azure | Id. de CIS | Responsabilidad |
|--|--|--|
| 6.12 | 2.8 | Customer |

Use configuraciones específicas del sistema operativo o recursos de terceros para limitar la capacidad de los usuarios de ejecutar scripts en los recursos de proceso de Azure.

Por ejemplo, cómo controlar la ejecución de scripts de PowerShell en entornos Windows:

https://docs.microsoft.com/powershell/module/microsoft.powershell.security/set-executionpolicy?view=powershell-6

## <a name="613-physically-or-logically-segregate-high-risk-applications"></a>6.13: Segregación física o lógica de aplicaciones de alto riesgo

| Identificador de Azure | Id. de CIS | Responsabilidad |
|--|--|--|
| 6.13 | 2.9 | Customer |

El software que se requiere para las operaciones empresariales, pero que puede suponer un riesgo mayor para la organización, debe aislarse en su propia máquina virtual o red virtual y estar lo suficientemente protegida con Azure Firewall o un grupo de seguridad de red.

Creación de una red virtual:

https://docs.microsoft.com/azure/virtual-network/quick-create-portal

Creación un NSG con una configuración de seguridad:

https://docs.microsoft.com/azure/virtual-network/tutorial-filter-network-traffic

## <a name="next-steps"></a>Pasos siguientes

Vea el siguiente control de seguridad: [Configuración segura](security-control-secure-configuration.md)

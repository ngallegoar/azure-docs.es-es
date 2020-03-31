---
title: 'Control de seguridad de Azure: protección de datos'
description: Protección de datos con control de seguridad
author: msmbaldwin
manager: rkarlin
ms.service: security
ms.topic: conceptual
ms.date: 12/30/2019
ms.author: mbaldwin
ms.custom: security-recommendations
ms.openlocfilehash: 5482495f87e87e5d05d8adca6b053810a62dcb4e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "75934519"
---
# <a name="security-control-data-protection"></a>Control de seguridad: Protección de datos

Las recomendaciones de protección de datos se centran en solucionar los problemas relacionados con el cifrado, las listas de control de acceso, el control de acceso basado en identidad y el registro de auditoría para el acceso a datos.

## <a name="41-maintain-an-inventory-of-sensitive-information"></a>4.1: Mantenimiento de un inventario de información confidencial

| Identificador de Azure | Id. de CIS | Responsabilidad |
|--|--|--|
| 4,1 | 13.1 | Customer |

Use etiquetas para ayudar a realizar el seguimiento de los recursos de Azure que almacenan o procesan información confidencial.

Cómo crear y usar etiquetas:

https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags

## <a name="42-isolate-systems-storing-or-processing-sensitive-information"></a>4.2: Aislamiento de los sistemas que almacenan o procesan información confidencial

| Identificador de Azure | Id. de CIS | Responsabilidad |
|--|--|--|
| 4,2 | 13.2 | Customer |

Implemente suscripciones y/o grupos de administración independientes para los entornos de desarrollo, prueba y producción. Los recursos deben separarse por red virtual o subred, etiquetarse adecuadamente y estar protegidos por un grupo de seguridad de red o Azure Firewall. Los recursos que almacenan o procesan datos confidenciales deben estar suficientemente aislados. Para las máquinas virtuales que almacenan o procesan datos confidenciales, implemente la directiva y los procedimientos necesarios para desactivarlos cuando no estén en uso.

Cómo crear suscripciones adicionales de Azure:

https://docs.microsoft.com/azure/billing/billing-create-subscription

Cómo crear grupos de administración:

https://docs.microsoft.com/azure/governance/management-groups/create

Cómo crear y usar etiquetas:

https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags

Cómo crear una red virtual:

https://docs.microsoft.com/azure/virtual-network/quick-create-portal

Cómo crear un grupo de seguridad de red con una configuración de seguridad:

https://docs.microsoft.com/azure/virtual-network/tutorial-filter-network-traffic

Cómo implementar Azure Firewall:

https://docs.microsoft.com/azure/firewall/tutorial-firewall-deploy-portal

Cómo configurar los modos de alertar o alertar y denegar con Azure Firewall:

https://docs.microsoft.com/azure/firewall/threat-intel

## <a name="43-monitor-and-block-unauthorized-transfer-of-sensitive-information"></a>4.3: Supervisión y bloqueo de una transferencia no autorizada de información confidencial

| Identificador de Azure | Id. de CIS | Responsabilidad |
|--|--|--|
| 4.3 | 13.3 | Customer |

Implemente una herramienta automatizada en los perímetros de red que supervise la transferencia no autorizada de información confidencial y bloquee dichas transferencias al tiempo que alerta a los profesionales de seguridad de la información.

## <a name="44-encrypt-all-sensitive-information-in-transit"></a>4.4: Cifrado de toda la información confidencial en tránsito

| Identificador de Azure | Id. de CIS | Responsabilidad |
|--|--|--|
| 4.4. | 14.4 | Compartido |

Cifre toda la información confidencial en tránsito. Asegúrese de que los clientes que se conectan a los recursos de Azure pueden negociar TLS 1.2 o superior.

Siga las recomendaciones de Azure Security Center para el cifrado en reposo y el cifrado en tránsito, si procede.

Descripción del cifrado en tránsito con Azure:

https://docs.microsoft.com/azure/security/fundamentals/encryption-overview#encryption-of-data-in-transit

## <a name="45-use-an-active-discovery-tool-to-identify-sensitive-data"></a>4.5: Uso de una herramienta de detección activa para identificar datos confidenciales

| Identificador de Azure | Id. de CIS | Responsabilidad |
|--|--|--|
| 4.5. | 14.5 | Customer |

Si no hay ninguna característica disponible para el servicio específico en Azure, use una herramienta de detección activa de terceros para identificar toda la información confidencial almacenada, procesada o transmitida por los sistemas tecnológicos de la organización, incluidos los del ubicación local o los que se encuentran en un proveedor de servicios remoto, y actualizar el inventario de información confidencial de la organización.

Use Azure Information Protection para identificar información confidencial en documentos de Office 365.

Use Azure SQL Information Protection para ayudar en la clasificación y el etiquetado de la información almacenada en las instancias de Azure SQL Database.

Cómo implementar la detección de datos SQL de Azure:

https://docs.microsoft.com/azure/sql-database/sql-database-data-discovery-and-classification

Cómo implementar Azure Information Protection:

https://docs.microsoft.com/azure/information-protection/deployment-roadmap

## <a name="46-use-azure-rbac-to-control-access-to-resources"></a>4.6: Uso de RBAC de Azure para controlar el acceso a los recursos

| Identificador de Azure | Id. de CIS | Responsabilidad |
|--|--|--|
| 4.6 | 14.6 | Customer |

Use el RBAC de Azure AD para controlar el acceso a los datos y recursos; si no, use métodos de control de acceso específicos del servicio.

Descripción de Azure RBAC:

https://docs.microsoft.com/azure/role-based-access-control/overview

Cómo configurar el RBAC en Azure:

https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal

## <a name="47-use-host-based-data-loss-prevention-to-enforce-access-control"></a>4.7: Uso de la prevención de pérdida de datos basada en host para aplicar el control de acceso

| Identificador de Azure | Id. de CIS | Responsabilidad |
|--|--|--|
| 4,7 | 14.7 | Customer |

Implemente una herramienta de terceros, como una solución de prevención de pérdida de datos basada en host automatizada, para aplicar controles de acceso a los datos incluso cuando se copian datos de un sistema.

## <a name="48-encrypt-sensitive-information-at-rest"></a>4.8: Cifrado de información confidencial en reposo

| Identificador de Azure | Id. de CIS | Responsabilidad |
|--|--|--|
| 4.8 | 14.8 | Customer |

Use el cifrado en reposo en todos los recursos de Azure. Microsoft recomienda permitir que Azure administre las claves de cifrado, pero existe la opción de administrar sus propias claves en algunas instancias. 

Descripción del cifrado en reposo en Azure:

https://docs.microsoft.com/azure/security/fundamentals/encryption-atrest

Cómo configurar las claves de cifrado administradas por el cliente:

https://docs.microsoft.com/azure/storage/common/storage-encryption-keys-portal

## <a name="49-log-and-alert-on-changes-to-critical-azure-resources"></a>4.9: Registro y alerta de cambios en los recursos críticos de Azure

| Identificador de Azure | Id. de CIS | Responsabilidad |
|--|--|--|
| 4,9 | 14.9 | Customer |

Use Azure Monitor con el registro de actividad de Azure para crear alertas para cuando se produzcan cambios en los recursos críticos de Azure.

Cómo crear alertas para eventos de registro de actividad de Azure:

https://docs.microsoft.com/azure/azure-monitor/platform/alerts-activity-log

## <a name="next-steps"></a>Pasos siguientes

Vea el siguiente control de seguridad: [Administración de vulnerabilidades](security-control-vulnerability-management.md)

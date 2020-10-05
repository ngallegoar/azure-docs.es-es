---
title: 'Azure Security Benchmark v2: administración de recursos'
description: Administración de recursos de Azure Security Benchmark v2
author: msmbaldwin
ms.service: security
ms.topic: conceptual
ms.date: 09/13/2020
ms.author: mbaldwin
ms.custom: security-benchmark
ms.openlocfilehash: ba010c0dd3384af895c68d980fcae788c2d6d45a
ms.sourcegitcommit: 94c750edd4d755d6ecee50ac977328098a277479
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/13/2020
ms.locfileid: "90059087"
---
# <a name="security-control-asset-management"></a>Control de seguridad: Administración de recursos

La administración de recursos cubre controles para garantizar la visibilidad y la gobernanza de la seguridad de los recursos de Azure. Esto incluye recomendaciones sobre permisos para el personal de seguridad, acceso de seguridad al inventario de recursos y administración de aprobaciones de servicios y recursos (inventario, seguimiento y corrección).

## <a name="am-1-ensure-security-team-has-visibility-into-risks-for-assets"></a>AM-1: Asegurarse de que el equipo de seguridad tiene visibilidad sobre los riesgos para los recursos

| Identificador de Azure | Identificadores de CIS Controls v7.1 | Identificadores de NIST SP800-53 r4 |
|--|--|--|--|
| AM-1 | 1.1, 1.2, 1.3, 1.4, 9.1, 12.1 | CM-7, CM-8, CM-11, PM-5 |

Asegúrese de que se conceden a los equipos de seguridad permisos de lector de seguridad en el inquilino y las suscripciones de Azure para que puedan supervisar los riesgos de seguridad mediante Azure Security Center. 

En función de la estructura de las responsabilidades del equipo de seguridad, la supervisión de los riesgos de seguridad puede ser responsabilidad de un equipo de seguridad central o de un equipo local. Dicho esto, la información y los riesgos de seguridad siempre se deben agregar de forma centralizada dentro de una organización. 

Los permisos de lector de seguridad se pueden aplicar en general a un inquilino completo (grupo de administración raíz) o a grupos de administración o a suscripciones específicas. 

Nota: Es posible que se requieran permisos adicionales para obtener visibilidad de las cargas de trabajo y los servicios. 

- [Introducción al rol de lector de seguridad](../../role-based-access-control/built-in-roles.md#security-reader)

- [Información general sobre los grupos de administración de Azure](../../governance/management-groups/overview.md)

**Responsabilidad**: Customer

**Partes interesadas de seguridad del cliente**:

- [Infraestructura y seguridad de los puntos de conexión](/azure/cloud-adoption-framework/organize/cloud-security-infrastructure-endpoint)

- [Administración del cumplimiento de la seguridad](/azure/cloud-adoption-framework/organize/cloud-security-compliance-management)

## <a name="am-2-ensure-security-team-has-access-to-asset-inventory-and-metadata"></a>AM-2: Asegurarse de que el equipo de seguridad tiene acceso a los metadatos y al inventario de recursos

| Identificador de Azure | Identificadores de CIS Controls v7.1 | Identificadores de NIST SP800-53 r4 |
|--|--|--|--|
| AM-2 | 1.5 | CM-8, PM-5 |

Asegúrese de que los equipos de seguridad tienen acceso a un inventario de recursos actualizado continuamente en Azure. Los equipos de seguridad suelen necesitar este inventario para evaluar la exposición potencial de su organización a los riesgos emergentes y como una entrada para mejoras de seguridad continuas. 

La característica de inventario de Azure Security Center y Azure Resource Graph pueden consultar y detectar todos los recursos de las suscripciones, incluidos los recursos de red, las aplicaciones y los servicios de Azure.  

Organice de forma lógica los recursos según la taxonomía de su organización mediante etiquetas y otros metadatos en Azure (nombre, descripción y categoría).  

- [Creación de consultas con Azure Resource Graph Explorer](../../governance/resource-graph/first-query-portal.md)

- [Administración del inventario de recursos de Azure Security Center](../../security-center/asset-inventory.md)

- [Para más información sobre el etiquetado de recursos, vea la guía de decisiones de nomenclatura y etiquetado de recursos](https://docs.microsoft.com/azure/cloud-adoption-framework/decision-guides/resource-tagging/?toc=/azure/azure-resource-manager/management/toc.json).

**Responsabilidad**: Customer

**Partes interesadas de seguridad del cliente**:

- [Infraestructura y seguridad de los puntos de conexión](/azure/cloud-adoption-framework/organize/cloud-security-infrastructure-endpoint)

- [Administración del cumplimiento de la seguridad](/azure/cloud-adoption-framework/organize/cloud-security-compliance-management)

## <a name="am-3-use-only-approved-azure-services"></a>AM-3: Uso exclusivo de servicios de Azure aprobados

| Identificador de Azure | Identificadores de CIS Controls v7.1 | Identificadores de NIST SP800-53 r4 |
|--|--|--|--|
| AM-3 | 2.3, 2.4 | CM-7, CM-8 |

Use Azure Policy para auditar y restringir qué servicios pueden aprovisionar los usuarios en su entorno. Use Azure Resource Graph para consultar y detectar recursos dentro de sus suscripciones.  También puede usar Azure Monitor para crear reglas para desencadenar alertas cuando se detecta un servicio no aprobado.

- [Configuración y administración de Azure Policy](../../governance/policy/tutorials/create-and-manage.md)

- [Denegación de un tipo de recurso específico con Azure Policy](/azure/governance/policy/samples/not-allowed-resource-types)

- [Creación de consultas con Azure Resource Graph Explorer](../../governance/resource-graph/first-query-portal.md)

**Responsabilidad**: Customer

**Partes interesadas de seguridad del cliente**:

- [Administración del cumplimiento de la seguridad](/azure/cloud-adoption-framework/organize/cloud-security-compliance-management)

- [Administración de la posición](/azure/cloud-adoption-framework/organize/cloud-security-posture-management)  

## <a name="am-4-ensure-security-of-asset-lifecycle-management"></a>AM-4: Garantizar la seguridad de la administración del ciclo de vida de los recursos

| Identificador de Azure | Identificadores de CIS Controls v7.1 | Identificadores de NIST SP800-53 r4 |
|--|--|--|--|
| AM-4 | 2.3, 2.4, 2.5 | CM-7, CM-8, CM-10, CM-11 |

Establecer o actualizar las directivas de seguridad que abordan los procesos de administración del ciclo de vida de los recursos para realizar modificaciones de gran impacto. Estas modificaciones incluyen cambios en: proveedores de identidades y acceso, confidencialidad de datos, configuración de red y asignación de privilegios administrativos.

Quite los recursos de Azure cuando ya no los necesite.

- [Eliminación de recursos y grupos de recursos de Azure](../../azure-resource-manager/management/delete-resource-group.md)

**Responsabilidad**: Customer

**Partes interesadas de seguridad del cliente**:

- [Infraestructura y seguridad de los puntos de conexión](/azure/cloud-adoption-framework/organize/cloud-security-infrastructure-endpoint)

- [Administración de la posición](/azure/cloud-adoption-framework/organize/cloud-security-posture-management)  

- [Administración del cumplimiento de la seguridad](/azure/cloud-adoption-framework/organize/cloud-security-compliance-management)

## <a name="am-5-limit-users-ability-to-interact-with-azure-resource-manager"></a>AM-5: Limitación de la capacidad de los usuarios para interactuar con Azure Resource Manager

| Identificador de Azure | Identificadores de CIS Controls v7.1 | Identificadores de NIST SP800-53 r4 |
|--|--|--|--|
| AM-5 | 2.9 | AC-3 |

Use el acceso condicional de Azure AD para limitar la capacidad de los usuarios de interactuar con Azure Resource Manager mediante la configuración de la opción "Bloquear acceso" en la aplicación "Administración de Microsoft Azure".

- [Configuración del acceso condicional para bloquear el acceso a Azure Resource Manager](../../role-based-access-control/conditional-access-azure-management.md)

**Responsabilidad**: Customer

**Partes interesadas de seguridad del cliente**:

- [Administración de la posición](/azure/cloud-adoption-framework/organize/cloud-security-posture-management)  

- [Infraestructura y seguridad de los puntos de conexión](/azure/cloud-adoption-framework/organize/cloud-security-infrastructure-endpoint)

## <a name="am-6-use-only-approved-applications-in-compute-resources"></a>AM-6: Uso exclusivo de aplicaciones aprobadas en recursos de proceso

| Identificador de Azure | Identificadores de CIS Controls v7.1 | Identificadores de NIST SP800-53 r4 |
|--|--|--|--|
| AM-6 | 2.6 | AC-3, CM-7, CM-8, CM-10, CM-11 |

Asegúrese de que solo se ejecute software autorizado y de que todo el software no autorizado no se ejecute en Azure Virtual Machines.

Use los controles de aplicaciones adaptables de Azure Security Center (ASC) para detectar y generar una lista de permitidos de la aplicación. También puede usar los controles de aplicaciones adaptables de ASC para asegurarse de que solo se ejecute software autorizado y de que todo el software no autorizado no se ejecute en Azure Virtual Machines.

Use Change Tracking e Inventario de Azure Automation para automatizar la recopilación de información de inventario de las máquinas virtuales Windows y Linux. El nombre del software, la versión, el publicador y la hora de actualización están disponibles en Azure Portal. Para obtener la fecha de instalación de software y otro tipo de información, habilite los diagnósticos de nivel de invitado y dirija los registros de eventos de Windows al área de trabajo de Log Analytics.

Según el tipo de scripts, puede usar configuraciones específicas del sistema operativo o recursos de terceros para limitar la capacidad de los usuarios de ejecutar scripts en los recursos de proceso de Azure. 

También puede usar una solución de terceros para descubrir e identificar software no aprobado.

- [Uso de los controles de aplicaciones adaptables de Azure Security Center](../../security-center/security-center-adaptive-application.md)

- [Información general de Change Tracking e Inventario en Azure Automation](../../automation/change-tracking.md)

- [Control de la ejecución de scripts de PowerShell en entornos Windows](https://docs.microsoft.com/powershell/module/microsoft.powershell.security/set-executionpolicy?view=powershell-6)

**Responsabilidad**: Customer

**Partes interesadas de seguridad del cliente**:

- [Infraestructura y seguridad de los puntos de conexión](/azure/cloud-adoption-framework/organize/cloud-security-infrastructure-endpoint)

- [Administración de la posición](/azure/cloud-adoption-framework/organize/cloud-security-posture-management)  

- [Administración del cumplimiento de la seguridad](/azure/cloud-adoption-framework/organize/cloud-security-compliance-management)


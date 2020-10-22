---
title: 'Azure Security Benchmark V2: acceso con privilegios'
description: 'Azure Security Benchmark V2: acceso con privilegios'
author: msmbaldwin
ms.service: security
ms.topic: conceptual
ms.date: 09/20/2020
ms.author: mbaldwin
ms.custom: security-benchmark
ms.openlocfilehash: 68fdff9444286a7f304c3a3361ad33a02e87a282
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "91758377"
---
# <a name="security-control-v2-privileged-access"></a>Control de seguridad V2: Acceso con privilegios

El acceso con privilegios abarca los controles para proteger el acceso con privilegios a los recursos y al inquilino de Azure. Incluye una serie de controles para proteger el modelo administrativo, las cuentas administrativas y las estaciones de trabajo de acceso con privilegios frente a riesgos deliberados e involuntarios.

## <a name="pa-1-protect-and-limit-highly-privileged-users"></a>PA-1: Protección y limitación de usuarios con privilegios elevados

| Identificador de Azure | Identificadores de CIS Controls v7.1 | Identificadores de NIST SP800-53 r4 |
|--|--|--|--|
| PA-1 | 4.3, 4.8 | AC-2 |

Limite el número de cuentas de usuario con privilegios elevados y proteja dichas cuentas en un nivel elevado. Los roles integrados más críticos de Azure AD son administrador global y administrador de roles con privilegios, ya que los usuarios asignados a estos dos roles pueden delegar roles de administrador. Con estos privilegios, los usuarios pueden leer y modificar todos los recursos de su entorno de Azure de forma directa o indirecta:

- Administrador global/administrador de empresa: los usuarios con este rol tienen acceso a todas las características administrativas de Azure AD, así como a los servicios que utilizan identidades de Azure AD.

- Administrador de roles con privilegios: los usuarios con este rol pueden administrar asignaciones de roles en Azure AD, y dentro de Azure AD Privileged Identity Management (PIM). Además, este rol permite administrar todos los aspectos de PIM y de las unidades administrativas.

Nota: Si usa roles personalizados con determinados permisos con privilegios asignados, es posible que tenga otros roles críticos que deban administrarse. Además, puede que también quiera aplicar controles similares a la cuenta de administrador de recursos empresariales críticos.  

Puede habilitar el acceso con privilegios Just-in-Time (JIT) a los recursos de Azure y Azure AD mediante Azure AD Privileged Identity Management (PIM). JIT concede permisos temporales para realizar tareas con privilegios solo cuando los usuarios lo necesitan. PIM también puede generar alertas de seguridad cuando hay actividades sospechosas o no seguras en la organización de Azure AD.

- [Permisos de rol administrativo en Azure AD](../../active-directory/users-groups-roles/directory-assign-admin-roles.md)

- [Uso de alertas de seguridad de Azure Privileged Identity Management](../../active-directory/privileged-identity-management/pim-how-to-configure-security-alerts.md)

- [Protección del acceso con privilegios para las implementaciones híbridas y en la nube en Azure AD](../../active-directory/users-groups-roles/directory-admin-roles-secure.md)

**Responsabilidad**: Customer

**Partes interesadas de seguridad del cliente** ([Más información](/azure/cloud-adoption-framework/organize/cloud-security#security-functions)):

- [Administración de identidades y claves](/azure/cloud-adoption-framework/organize/cloud-security-identity-keys)

- [Arquitectura de seguridad](/azure/cloud-adoption-framework/organize/cloud-security-architecture)

- [Administración del cumplimiento de la seguridad](/azure/cloud-adoption-framework/organize/cloud-security-compliance-management)

- [Operaciones de seguridad](/azure/cloud-adoption-framework/organize/cloud-security-operations-center)

## <a name="pa-2-restrict-administrative-access-to-business-critical-systems"></a>PA-2: Restricción del acceso administrativo a los sistemas críticos para la empresa

| Identificador de Azure | Identificadores de CIS Controls v7.1 | Identificadores de NIST SP800-53 r4 |
|--|--|--|--|
| PA-2 | 13.2, 2.10 | AC-2, SC-3, SC-7 |

Aísle el acceso a los sistemas críticos para la empresa mediante la restricción de las cuentas a las que se concede acceso con privilegios a las suscripciones y los grupos de administración en los que se encuentran. Asegúrese de restringir también el acceso a los sistemas de administración, identidad y seguridad que tienen acceso administrativo a los recursos críticos para su negocio, como controladores de dominio (DC) de Active Directory, herramientas de seguridad y herramientas de administración del sistema con agentes instalados en sistemas críticos para la empresa. Los atacantes que ponen en peligro estos sistemas de administración y seguridad pueden convertirlos inmediatamente en un arma para poner en peligro los recursos críticos para la empresa. 

Para garantizar un control de acceso coherente, todos los tipos de control de acceso se deben alinear con la estrategia de segmentación de la empresa. 

Asegúrese de asignar cuentas con privilegios independientes que sean distintas de las cuentas de usuario estándar que se usan para las tareas de correo electrónico, exploración y productividad.

- [Modelo de referencia y componentes de Azure](/security/compass/microsoft-security-compass-introduction#azure-components-and-reference-model-2151)

- [Acceso al grupo de administración](../../governance/management-groups/overview.md#management-group-access)

- [Administradores de la suscripción de Azure](../../cost-management-billing/manage/add-change-subscription-administrator.md)

**Responsabilidad**: Customer

**Partes interesadas de seguridad del cliente** ([Más información](/azure/cloud-adoption-framework/organize/cloud-security#security-functions)):

- [Administración de identidades y claves](/azure/cloud-adoption-framework/organize/cloud-security-identity-keys)

- [Administración del cumplimiento de la seguridad](/azure/cloud-adoption-framework/organize/cloud-security-compliance-management)

- [Arquitectura de seguridad](/azure/cloud-adoption-framework/organize/cloud-security-architecture)

## <a name="pa-3-review-and-reconcile-user-access-regularly"></a>PA-3: Revisión y conciliación de manera periódica del acceso de los usuarios

| Identificador de Azure | Identificadores de CIS Controls v7.1 | Identificadores de NIST SP800-53 r4 |
|--|--|--|--|
| PA-3 | 4.1, 16.9, 16.10 | AC-2 |

Revise las cuentas de usuario y la asignación de acceso con regularidad para asegurarse de que las cuentas y su nivel de acceso sean válidos. Puede usar las revisiones de acceso de Azure AD para revisar las pertenencias a grupos, el acceso a las aplicaciones empresariales y las asignaciones de roles. Los informes de Azure AD pueden proporcionar registros para ayudar a detectar cuentas obsoletas. También puede usar Azure AD Privileged Identity Management para crear un flujo de trabajo de informes de revisión de acceso que facilite el proceso de revisión.
Además, se puede configurar Azure Privileged Identity Management para enviar una alerta cuando se cree un número de cuentas de administrador excesivo y para identificar las cuentas de administrador que hayan quedado obsoletas o que no estén configuradas correctamente. 

Nota: Algunos servicios de Azure admiten roles y usuarios locales que no se administran mediante Azure AD. Estos usuarios deberán administrarse por separado.

- [Creación de una revisión de acceso de los roles de recursos de Azure en Privileged Identity Management (PIM)](../../active-directory/privileged-identity-management/pim-resource-roles-start-access-review.md)

- [Procedimiento para usar las revisiones de acceso e identidades de Azure AD](../../active-directory/governance/access-reviews-overview.md)

**Responsabilidad**: Customer

**Partes interesadas de seguridad del cliente** ([Más información](/azure/cloud-adoption-framework/organize/cloud-security#security-functions)):

- [Administración de identidades y claves](/azure/cloud-adoption-framework/organize/cloud-security-identity-keys)

- [Seguridad de las aplicaciones y DevSecOps](/azure/cloud-adoption-framework/organize/cloud-security-application-security-devsecops)

- [Administración del cumplimiento de la seguridad](/azure/cloud-adoption-framework/organize/cloud-security-compliance-management)

## <a name="pa-4-set-up-emergency-access-in-azure-ad"></a>PA-4: Configuración del acceso de emergencia en Azure AD

| Identificador de Azure | Identificadores de CIS Controls v7.1 | Identificadores de NIST SP800-53 r4 |
|--|--|--|--|
| PA-4 | 16 | AC-2, CP-2 |

Para evitar que se le bloquee por accidente el acceso a la organización de Azure AD, configure una cuenta de acceso de emergencia para cuando no se puedan usar cuentas administrativas normales. Las cuentas de acceso de emergencia tienen normalmente privilegios elevados y no se asignan a usuarios específicos. Las cuentas de acceso de emergencia se limitan a situaciones "excepcionales" o de emergencia en las que no se pueden usar las cuentas administrativas normales.
Debe asegurarse de que las credenciales (como contraseña, certificado o tarjeta inteligente) de las cuentas de acceso de emergencia estén protegidas y solo las conozcan aquellas personas que estén autorizadas a usarlas solo en caso de emergencia.

- [Administración de cuentas de acceso de emergencia en Azure AD](../../active-directory/users-groups-roles/directory-emergency-access.md)

**Responsabilidad**: Customer

**Partes interesadas de seguridad del cliente** ([Más información](/azure/cloud-adoption-framework/organize/cloud-security#security-functions)):

- [Administración de identidades y claves](/azure/cloud-adoption-framework/organize/cloud-security-identity-keys)

- [Seguridad de las aplicaciones y DevSecOps](/azure/cloud-adoption-framework/organize/cloud-security-application-security-devsecops)

- [Administración del cumplimiento de la seguridad](/azure/cloud-adoption-framework/organize/cloud-security-compliance-management)

- [Security Operations (SecOps)](/azure/cloud-adoption-framework/organize/cloud-security-operations-center)

## <a name="pa-5-automate-entitlement-management"></a>PA-5: Automatización de la administración de derechos

| Identificador de Azure | Identificadores de CIS Controls v7.1 | Identificadores de NIST SP800-53 r4 |
|--|--|--|--|
| PA-5 | 16 | AC-2, AC-5, PM-10 |

Use las características de administración de derechos de Azure AD para automatizar los flujos de trabajo de solicitud de acceso, como las asignaciones, las revisiones y la expiración del acceso. También se admite la aprobación en dos o varias fases.
- [¿Qué son las revisiones de acceso de Azure AD?](../../active-directory/governance/access-reviews-overview.md) 

- [¿Qué es la administración de derechos de Azure AD?](../../active-directory/governance/entitlement-management-overview.md)

**Responsabilidad**: Customer

**Partes interesadas de seguridad del cliente** ([Más información](/azure/cloud-adoption-framework/organize/cloud-security#security-functions)):

- [Administración de identidades y claves](/azure/cloud-adoption-framework/organize/cloud-security-identity-keys)

- [Seguridad de las aplicaciones y DevSecOps](/azure/cloud-adoption-framework/organize/cloud-security-application-security-devsecops)

- [Administración del cumplimiento de la seguridad](/azure/cloud-adoption-framework/organize/cloud-security-compliance-management)

## <a name="pa-6-use-privileged-access-workstations"></a>PA-6: Uso de estaciones de trabajo con privilegios de acceso

| Identificador de Azure | Identificadores de CIS Controls v7.1 | Identificadores de NIST SP800-53 r4 |
|--|--|--|--|
| PA-6 | 4.6, 11.6, 12.12 | AC-2, SC-3, SC-7 |

Las estaciones de trabajo seguras y aisladas son de una importancia vital para la seguridad de los roles con acceso a información confidencial como administradores, desarrolladores y operadores de servicios críticos. Use estaciones de trabajo de usuario de alta seguridad o Azure Bastion para las tareas administrativas. Use Azure Active Directory, Protección contra amenazas avanzada de Microsoft Defender (ATP) o Microsoft Intune para implementar una estación de trabajo de usuario segura y administrada para las tareas administrativas. Las estaciones de trabajo protegidas se pueden administrar de forma centralizada para aplicar una configuración segura, como autenticación sólida, líneas de base de software y hardware y acceso lógico y de red restringido. 

- [Descripción de las estaciones de trabajo con privilegios de acceso](../../active-directory/devices/concept-azure-managed-workstation.md)

- [Implementación de una estación de trabajo con privilegios de acceso](../../active-directory/devices/howto-azure-managed-workstation.md)

**Responsabilidad**: Customer

**Partes interesadas de seguridad del cliente** ([Más información](/azure/cloud-adoption-framework/organize/cloud-security#security-functions)):

- [Seguridad de las aplicaciones y DevSecOps](/azure/cloud-adoption-framework/organize/cloud-security-application-security-devsecops)

- [Security Operations (SecOps)](/azure/cloud-adoption-framework/organize/cloud-security-operations-center)

- [Administración de identidades y claves](/azure/cloud-adoption-framework/organize/cloud-security-identity-keys)

## <a name="pa-7-follow-just-enough-administration-least-privilege-principle"></a>PA-7: Seguimiento de solo una administración suficiente (principio de privilegios mínimos)

| Identificador de Azure | Identificadores de CIS Controls v7.1 | Identificadores de NIST SP800-53 r4 |
|--|--|--|--|
| PA-7 | 14.6 | AC-2, AC-3, SC-3 |

El control de acceso basado en roles de Azure (RBAC de Azure) permite administrar el acceso a los recursos de Azure a través de las asignaciones de roles. Puede asignar estos roles a usuarios, grupos, entidades de servicio e identidades administradas. Hay roles integrados predefinidos para determinados recursos, y estos roles se pueden inventariar o consultar mediante herramientas, como la CLI de Azure, Azure PowerShell y Azure Portal. Los privilegios que se asignen a los recursos a través de Azure RBAC siempre se deben limitar a los requisitos de los roles. Los privilegios limitados complementan el enfoque Just-in-Time (JIT) de Azure AD Privileged Identity Management (PIM) y esos privilegios deben revisarse periódicamente.
Use los roles integrados para asignar los permisos y crear solo el rol personalizado cuando sea necesario. 

- [¿Qué es el control de acceso basado en rol de Azure (RBAC)?](../../role-based-access-control/overview.md)

- [Configuración de Azure RBAC](../../role-based-access-control/role-assignments-portal.md)

- [Procedimiento para usar las revisiones de acceso e identidades de Azure AD](../../active-directory/governance/access-reviews-overview.md)

**Responsabilidad**: Customer

**Partes interesadas de seguridad del cliente** ([Más información](/azure/cloud-adoption-framework/organize/cloud-security#security-functions)):

- [Seguridad de las aplicaciones y DevSecOps](/azure/cloud-adoption-framework/organize/cloud-security-application-security-devsecops)

- [Administración del cumplimiento de la seguridad](/azure/cloud-adoption-framework/organize/cloud-security-compliance-management) 

- [Administración de la posición](/azure/cloud-adoption-framework/organize/cloud-security-posture-management)    

- [Administración de identidades y claves](/azure/cloud-adoption-framework/organize/cloud-security-identity-keys)

## <a name="pa-8-choose-approval-process-for-microsoft-support"></a>PA-8: Selección del proceso de aprobación para el soporte técnico de Microsoft 

| Identificador de Azure | Identificadores de CIS Controls v7.1 | Identificadores de NIST SP800-53 r4 |
|--|--|--|--|
| PA-8 | 16 | AC-2, AC-3, AC-4 |

En escenarios de soporte técnico en los que Microsoft necesita acceder a los datos de los clientes, la Caja de seguridad del cliente proporciona una funcionalidad para que pueda revisar y aprobar o rechazar explícitamente la solicitud de acceso a tales datos.

- [Descripción de la Caja de seguridad del cliente](../fundamentals/customer-lockbox-overview.md)

**Responsabilidad**: Customer

**Partes interesadas de seguridad del cliente** ([Más información](/azure/cloud-adoption-framework/organize/cloud-security#security-functions)):

- [Seguridad de las aplicaciones y DevSecOps](/azure/cloud-adoption-framework/organize/cloud-security-application-security-devsecops)

- [Administración del cumplimiento de la seguridad](/azure/cloud-adoption-framework/organize/cloud-security-compliance-management) 

- [Administración de identidades y claves](/azure/cloud-adoption-framework/organize/cloud-security-identity-keys)


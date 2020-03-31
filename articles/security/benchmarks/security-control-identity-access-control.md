---
title: 'Control de seguridad de Azure: identidad y Access Control'
description: Identidad y Access Control del control de seguridad
author: msmbaldwin
manager: rkarlin
ms.service: security
ms.topic: conceptual
ms.date: 12/30/2019
ms.author: mbaldwin
ms.custom: security-recommendations
ms.openlocfilehash: 543573610c2ea3ab0bcd89e1b8f4ee5f5a34dbc0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "75934432"
---
# <a name="security-control-identity-and-access-control"></a>Control de seguridad: Identidad y Access Control

Las recomendaciones de administración de identidades y acceso se centran en solucionar problemas relacionados con el control de acceso basado en la identidad, el bloqueo del acceso administrativo, la alerta de eventos relacionados con la identidad, el comportamiento anómalo de la cuenta y el control de acceso basado en rol.

## <a name="31-maintain-an-inventory-of-administrative-accounts"></a>3.1: Mantenga un inventario de cuentas administrativas

| Id. de Azure | Id. de CIS | Responsabilidad |
|--|--|--|
| 3.1 | 4,1 | Customer |

Azure AD tiene roles integrados que se deben asignar explícitamente y son consultables. Use el módulo de PowerShell de Azure AD para realizar consultas ad hoc para detectar cuentas que son miembros de grupos administrativos.

Cómo obtener un rol de directorio en Azure AD con PowerShell:

https://docs.microsoft.com/powershell/module/azuread/get-azureaddirectoryrole?view=azureadps-2.0

Cómo obtener los miembros de un rol de directorio en Azure AD con PowerShell:

https://docs.microsoft.com/powershell/module/azuread/get-azureaddirectoryrolemember?view=azureadps-2.0

## <a name="32-change-default-passwords-where-applicable"></a>3.2: Cambie las contraseñas predeterminadas cuando proceda

| Id. de Azure | Id. de CIS | Responsabilidad |
|--|--|--|
| 3.2 | 4,2 | Customer |

Azure AD no tiene el concepto de contraseñas predeterminadas. Otros recursos de Azure que requieren una contraseña obligan a crear una contraseña con requisitos de complejidad y una longitud mínima, que cambia en función del servicio. Usted es responsable de las aplicaciones de terceros y de los servicios de Marketplace que pueden usar contraseñas predeterminadas.

## <a name="33-use-dedicated-administrative-accounts"></a>3.3: Use cuentas administrativas dedicadas

| Id. de Azure | Id. de CIS | Responsabilidad |
|--|--|--|
| 3.3 | 4.3 | Customer |

Cree procedimientos operativos estándar en torno al uso de cuentas administrativas dedicadas. Use la administración de identidad y acceso de Azure Security Center para supervisar el número de cuentas administrativas.

También puede habilitar el acceso Just-in-Time/Just-Enough usando roles de Azure AD Privileged Identity Management con privilegios para los servicios de Microsoft y Azure Resource Manager. 

Más información: https://docs.microsoft.com/azure/active-directory/privileged-identity-management/

## <a name="34-use-single-sign-on-sso-with-azure-active-directory"></a>3.4: Use el inicio de sesión único (SSO) con Azure Active Directory

| Id. de Azure | Id. de CIS | Responsabilidad |
|--|--|--|
| 3.4 | 4.4. | Customer |

Siempre que sea posible, use el SSO de Azure Active Directory en lugar de configurar credenciales independientes individuales por servicio. Use las recomendaciones de administración de identidades y acceso de Azure Security Center.

Descripción del SSO con Azure AD:

https://docs.microsoft.com/azure/active-directory/manage-apps/what-is-single-sign-on

## <a name="35-use-multi-factor-authentication-for-all-azure-active-directory-based-access"></a>3.5: Use la autenticación multifactor para todo el acceso basado en Azure Active Directory

| Id. de Azure | Id. de CIS | Responsabilidad |
|--|--|--|
| 3,5 | 4.5, 11.5, 12.11, 16.3 | Customer |

Habilite la MFA de Azure AD y siga las recomendaciones de administración de identidades y acceso de Azure Security Center.

Cómo habilitar la MFA en Azure:

https://docs.microsoft.com/azure/active-directory/authentication/howto-mfa-getstarted

Cómo supervisar la identidad y el acceso en Azure Security Center:

https://docs.microsoft.com/azure/security-center/security-center-identity-access

## <a name="36-use-dedicated-machines-privileged-access-workstations-for-all-administrative-tasks"></a>3.6: Use máquinas dedicadas (estaciones de trabajo de acceso con privilegios) para todas las tareas administrativas

| Id. de Azure | Id. de CIS | Responsabilidad |
|--|--|--|
| 3.6 | 4.6, 11.6, 12.12 | Customer |

Utilice PAW (estaciones de trabajo de acceso privilegiado) con la MFA configurada para iniciar la sesión y configurar recursos de Azure.

Más información sobre las estaciones de trabajo con privilegios de acceso:

https://docs.microsoft.com/windows-server/identity/securing-privileged-access/privileged-access-workstations

Cómo habilitar la MFA en Azure:

https://docs.microsoft.com/azure/active-directory/authentication/howto-mfa-getstarted


## <a name="37-log-and-alert-on-suspicious-activity-from-administrative-accounts"></a>3.7: Registre y alerte de la actividad sospechosa desde cuentas administrativas

| Id. de Azure | Id. de CIS | Responsabilidad |
|--|--|--|
| 3.7 | 4.8, 4.9 | Customer |

Use los informes de seguridad de Azure Active Directory para la generación de registros y alertas cuando se producen actividades sospechosas o no seguras en el entorno. Use Azure Security Center para supervisar la actividad de identidad y acceso.

Cómo identificar usuarios de Azure AD marcados por una actividad de riesgo:

https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-user-at-risk

Cómo supervisar la identidad de los usuarios y la actividad de acceso en Azure Security Center:

https://docs.microsoft.com/azure/security-center/security-center-identity-access

## <a name="38-manage-azure-resources-from-only-approved-locations"></a>3.8: Administre los recursos de Azure solo desde ubicaciones aprobadas

| Id. de Azure | Id. de CIS | Responsabilidad |
|--|--|--|
| 3.8 | 11.7 | Customer |

Use ubicaciones con nombre de acceso condicional para permitir el acceso solo desde agrupaciones lógicas específicas de intervalos de direcciones IP o países o regiones.

Cómo configurar ubicaciones con nombre en Azure:

https://docs.microsoft.com/azure/active-directory/reports-monitoring/quickstart-configure-named-locations

## <a name="39-use-azure-active-directory"></a>3.9: Uso de Azure Active Directory

| Id. de Azure | Id. de CIS | Responsabilidad |
|--|--|--|
| 3.9 | 16.1, 16.2, 16.4, 16.5, 16.6 | Customer |

Use Azure Active Directory (AAD) como el sistema central de autenticación y autorización. AAD protege los datos mediante un fuerte cifrado para los datos en reposo y en tránsito. AAD también cifra con sal, convierte en hash y almacena de forma segura las credenciales de los usuarios.

Cómo crear y configurar una instancia de AAD:

https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-access-create-new-tenant

## <a name="310-regularly-review-and-reconcile-user-access"></a>3.10: Revise y concilie regularmente el acceso de los usuarios

| Id. de Azure | Id. de CIS | Responsabilidad |
|--|--|--|
| 3.10 | 16.9, 16.10 | Customer |

Azure AD proporciona registros para ayudar a descubrir cuentas obsoletas. Además, use las revisiones de acceso de identidad de Azure para administrar de forma eficiente las pertenencias a grupos, el acceso a las aplicaciones empresariales y las asignaciones de roles. El acceso de los usuarios se puede revisar de forma periódica para asegurarse de que solo las personas adecuadas tengan acceso continuado. 

Informes de Azure AD:

https://docs.microsoft.com/azure/active-directory/reports-monitoring/

Cómo usar las revisiones de acceso de identidad de Azure:

https://docs.microsoft.com/azure/active-directory/governance/access-reviews-overview

## <a name="311-monitor-attempts-to-access-deactivated-accounts"></a>3.11: Supervise los intentos de acceso a cuentas desactivadas

| Id. de Azure | Id. de CIS | Responsabilidad |
|--|--|--|
| 3,11 | 16.12 | Customer |

Tiene acceso a los orígenes de registro de eventos de actividad de inicio de sesión, auditoría y riesgo de Azure AD, que permiten la integración con cualquier herramienta de SIEM o supervisión.

Puede simplificar este proceso creando una configuración de diagnóstico para las cuentas de usuario de Azure Active Directory y enviando los registros de auditoría y los registros de inicio de sesión a un área de trabajo de Log Analytics. Puede configurar las alertas deseadas en el área de trabajo de Log Analytics.

Cómo integrar los registros de actividad de Azure en Azure Monitor:

https://docs.microsoft.com/azure/active-directory/reports-monitoring/howto-integrate-activity-logs-with-log-analytics

## <a name="312-alert-on-account-login-behavior-deviation"></a>3.12: Alerte de las desviaciones de comportamiento en los inicios de sesión de las cuentas

| Id. de Azure | Id. de CIS | Responsabilidad |
|--|--|--|
| 3,12 | 16.13 | Customer |

Use las características de protección de riesgos e identidad de Azure AD para configurar las respuestas automatizadas a las acciones sospechosas detectadas relacionadas con las identidades de los usuarios. También puede hacer que Azure Sentinel ingiera los datos para investigarlos más.

Cómo ver los inicios de sesión de riesgo de Azure AD:

https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-risky-sign-ins

Cómo configurar y habilitar las directivas de riesgo de protección de la identidad:

https://docs.microsoft.com/azure/active-directory/identity-protection/howto-identity-protection-configure-risk-policies

Cómo incorporar Azure Sentinel:

https://docs.microsoft.com/azure/sentinel/quickstart-onboard

## <a name="313-provide-microsoft-with-access-to-relevant-customer-data-during-support-scenarios"></a>3.13: Proporcione a Microsoft acceso a los datos pertinentes del cliente durante los escenarios de soporte técnico

| Id. de Azure | Id. de CIS | Responsabilidad |
|--|--|--|
| 3.13 | 16 | Customer |

En escenarios de soporte técnico en los que Microsoft necesita acceder a los datos del cliente, la Caja de seguridad del cliente proporciona una interfaz para que pueda revisar y aprobar o rechazar las solicitudes de acceso a los datos del cliente.

Descripción de la Caja de seguridad del cliente:

https://docs.microsoft.com/azure/security/fundamentals/customer-lockbox-overview

## <a name="next-steps"></a>Pasos siguientes

Vea el siguiente control de seguridad: [Protección de datos](security-control-data-protection.md)

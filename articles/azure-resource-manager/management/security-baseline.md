---
title: Línea de base de seguridad de Azure para Azure Resource Manager
description: La línea de base de seguridad de Azure Resource Manager proporciona instrucciones de procedimientos y recursos para implementar las recomendaciones de seguridad especificadas en Azure Security Benchmark.
author: msmbaldwin
ms.service: azure-resource-manager
ms.topic: conceptual
ms.date: 10/30/2020
ms.author: mbaldwin
ms.custom: subject-security-benchmark
ms.openlocfilehash: d841d3b922a5edd0257934566207187ae031c1b5
ms.sourcegitcommit: 5b93010b69895f146b5afd637a42f17d780c165b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/02/2020
ms.locfileid: "96532183"
---
# <a name="azure-security-baseline-for-azure-resource-manager"></a>Línea de base de seguridad de Azure para Azure Resource Manager

Esta línea de base de seguridad aplica la guía de la [versión 1.0 de Azure Security Benchmark](../../security/benchmarks/overview-v1.md) en Microsoft Azure Resource Manager. Azure Security Benchmark proporciona recomendaciones sobre cómo puede proteger sus soluciones de nube en Azure.
El contenido se agrupa por los **controles de seguridad** definidos por Azure Security Benchmark y las instrucciones relacionadas aplicables a Azure Resource Manager. Se han excluido los **controles** no aplicables a Azure Resource Manager.

 Para ver cómo se asigna completamente Azure Resource Manager a Azure Security Benchmark, consulte el [archivo de asignación de línea de base de seguridad de Azure Resource Manager completo](https://github.com/MicrosoftDocs/SecurityBenchmarks/tree/master/Azure%20Offer%20Security%20Baselines).

## <a name="logging-and-monitoring"></a>Registro y supervisión

*Para obtener más información, consulte [Azure Security Benchmark: registro y supervisión](../../security/benchmarks/security-control-logging-monitoring.md).*

### <a name="22-configure-central-security-log-management"></a>2.2: Configuración de la administración central de registros de seguridad

**Guía**: Ingiera registros de actividad de Azure Policy a través de Azure Monitor. En Azure Monitor, use las áreas de trabajo de Log Analytics para realizar consultas y análisis, y utilice las cuentas de Azure Storage para el almacenamiento en el archivo o a largo plazo. Como alternativa, puede habilitar e incorporar datos en Azure Sentinel o en una herramienta SIEM de terceros.

- [Incorporación de Azure Sentinel](../../sentinel/quickstart-onboard.md) 

- [Recopilación de registros y métricas de plataforma con Azure Monitor](../../azure-monitor/platform/diagnostic-settings.md) 

- [Recopilación de registros de host internos de máquina virtual de Azure con Azure Monitor](../../azure-monitor/learn/quick-collect-azurevm.md) 

- [Introducción a Azure Monitor e integración con herramienta SIEM de terceros](https://azure.microsoft.com/blog/use-azure-monitor-to-integrate-with-siem-tools/)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="23-enable-audit-logging-for-azure-resources"></a>2.3: Habilitación del registro de auditoría para recursos de Azure

**Guía**: Azure Resource Manager utiliza los registros de actividad, que están habilitados automáticamente, para incluir el origen del evento, la fecha, el usuario, la marca de tiempo, las direcciones de origen y de destino, y otros elementos útiles.

- [Recopilación de registros y métricas de plataforma con Azure Monitor](../../azure-monitor/platform/diagnostic-settings.md) 

- [Descripción del registro y de los distintos tipos de registro de Azure](../../azure-monitor/platform/platform-logs-overview.md)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="26-monitor-and-review-logs"></a>2.6: Supervisión y registros de revisión

**Guía**: Analice y supervise los registros en busca de comportamientos anómalos y revise los resultados con regularidad. Use Azure Monitor y un área de trabajo de Log Analytics para revisar los registros y realizar consultas en los datos de dichos registros. 

También puede habilitar e incorporar datos en Azure Sentinel o en una herramienta SIEM de terceros. 

- [Incorporación de Azure Sentinel](../../sentinel/quickstart-onboard.md) 

- [Introducción a las consultas de Log Analytics](../../azure-monitor/log-query/log-analytics-tutorial.md) 

- [Procedimiento para realizar consultas personalizadas en Azure Monitor](../../azure-monitor/log-query/get-started-queries.md)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Compartido

### <a name="27-enable-alerts-for-anomalous-activities"></a>2.7: Habilitación de alertas para actividades anómalas

**Guía**: Use Azure Security Center con Log Analytics para la supervisión y alerta de actividades anómalas que se encuentren en los registros de actividad. Como alternativa, puede habilitar e incorporar datos en Azure Sentinel. 

- [Incorporación de Azure Sentinel](../../sentinel/quickstart-onboard.md) 

- [Administración de alertas de seguridad en Azure Security Center](../../security-center/security-center-managing-and-responding-alerts.md) 

- [Alertas sobre datos de registro de Log Analytics](../../azure-monitor/learn/tutorial-response.md)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

## <a name="identity-and-access-control"></a>Control de identidades y acceso

*Para obtener más información, consulte [Azure Security Benchmark: Identidad y control de acceso](../../security/benchmarks/security-control-identity-access-control.md).*

### <a name="31-maintain-an-inventory-of-administrative-accounts"></a>3.1: Mantenga un inventario de cuentas administrativas

**Guía**: El control de acceso basado en roles de Azure (RBAC) permite administrar el acceso a los recursos de Azure a través de las asignaciones de roles. Puede asignar estos roles a usuarios, grupos de entidades de servicio e identidades administradas. Hay roles integrados predefinidos para determinados recursos, y estos roles se pueden inventariar o consultar mediante herramientas, como la CLI de Azure, Azure PowerShell o el Azure Portal.

- [Obtención de un rol de directorio en Azure AD con PowerShell](/powershell/module/azuread/get-azureaddirectoryrole?preserve-view=true&view=azureadps-2.0)

- [Obtención de los miembros de un rol de directorio en Azure AD con PowerShell](/powershell/module/azuread/get-azureaddirectoryrolemember?preserve-view=true&view=azureadps-2.0)

**Supervisión de Azure Security Center**: Sí

**Responsabilidad**: Customer

### <a name="33-use-dedicated-administrative-accounts"></a>3.3: Use cuentas administrativas dedicadas

**Guía**: Cree procedimientos operativos estándar en torno al uso de cuentas administrativas dedicadas. Use la administración de identidad y acceso de Azure Security Center para supervisar el número de cuentas administrativas.

Además, para ayudarlo a realizar un seguimiento de las cuentas administrativas dedicadas, puede seguir las recomendaciones de Azure Security Center o las directivas integradas de Azure, por ejemplo:

- Debe haber más de un propietario asignado a su suscripción
- Las cuentas en desuso con permisos de propietario deben quitarse de la suscripción
- Las cuentas externas con permisos de propietario deben quitarse de la suscripción

También puede habilitar el acceso Just-in-Time mediante Azure AD Privileged Identity Management y Azure Resource Manager. 

- [Más información acerca de Privileged Identity Management](../../active-directory/privileged-identity-management/index.yml)

- [Uso de Azure Policy](../../governance/policy/tutorials/create-and-manage.md)

**Supervisión de Azure Security Center**: Sí

**Responsabilidad**: Customer

### <a name="34-use-single-sign-on-sso-with-azure-active-directory"></a>3.4: Use el inicio de sesión único (SSO) con Azure Active Directory

**Instrucciones**: Siempre que sea posible, use el SSO de Azure Active Directory en lugar de configurar credenciales independientes individuales por servicio. Use las recomendaciones de identidades y acceso de Azure Security Center. 

- [Descripción del SSO con Azure AD](../../active-directory/manage-apps/what-is-single-sign-on.md)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="35-use-multi-factor-authentication-for-all-azure-active-directory-based-access"></a>3.5: Use la autenticación multifactor para todo el acceso basado en Azure Active Directory

**Guía**: Habilite Multi-Factor Authentication (MFA) de Azure Active Directory y siga las recomendaciones de administración de identidad y acceso de Azure Security Center.

- [Procedimiento para habilitar la MFA en Azure](../../active-directory/authentication/howto-mfa-getstarted.md)

- [Supervisión de la identidad y el acceso en Azure Security Center](../../security-center/security-center-identity-access.md)

**Supervisión de Azure Security Center**: Sí

**Responsabilidad**: Customer

### <a name="36-use-dedicated-machines-privileged-access-workstations-for-all-administrative-tasks"></a>3.6: Use máquinas dedicadas (estaciones de trabajo de acceso con privilegios) para todas las tareas administrativas

**Instrucciones**: use una estación de trabajo segura y administrada por Azure (también conocida como una estación de trabajo de acceso con privilegios o PAW) para las tareas administrativas que requieren privilegios elevados.

- [Descripción de las estaciones de trabajo seguras administradas por Azure](../../active-directory/devices/concept-azure-managed-workstation.md)

- [Cómo habilitar MFA de Azure AD](../../active-directory/authentication/howto-mfa-getstarted.md)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="37-log-and-alert-on-suspicious-activities-from-administrative-accounts"></a>3.7: Registro y alerta de actividades sospechosas desde cuentas administrativas

**Guía**: use la característica de supervisión y los informes de seguridad de Azure Active Directory para detectar cuando se producen actividades sospechosas o no seguras en el entorno. Use Azure Security Center para supervisar la actividad de identidad y acceso.

- [Procedimiento para identificar usuarios de Azure AD marcados por una actividad de riesgo](../../active-directory/identity-protection/overview-identity-protection.md)

- [Supervisión de la actividad de identidad y acceso de los usuarios en Azure Security Center](../../security-center/security-center-identity-access.md)

**Supervisión de Azure Security Center**: Sí

**Responsabilidad**: Customer

### <a name="38-manage-azure-resources-only-from-approved-locations"></a>3.8: Administración de los recursos de Azure solo desde ubicaciones aprobadas

**Guía**: use ubicaciones con nombre de Azure AD para permitir el acceso solo desde agrupaciones lógicas específicas de intervalos de direcciones IP o países o regiones.

- [Configuración de ubicaciones con nombre de Azure AD](../../active-directory/reports-monitoring/quickstart-configure-named-locations.md)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="39-use-azure-active-directory"></a>3.9: Uso de Azure Active Directory

**Guía**: use ubicaciones con nombre de Azure AD para permitir el acceso solo desde agrupaciones lógicas específicas de intervalos de direcciones IP o países o regiones.

- [Configuración de ubicaciones con nombre de Azure AD](../../active-directory/reports-monitoring/quickstart-configure-named-locations.md)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="310-regularly-review-and-reconcile-user-access"></a>3.10: Revise y concilie regularmente el acceso de los usuarios

**Guía**: Azure AD proporciona registros para ayudar a descubrir cuentas obsoletas. Además, use las revisiones de acceso e identidades de Azure AD para administrar de forma eficiente las pertenencias a grupos, el acceso a las aplicaciones empresariales y las asignaciones de roles. El acceso de los usuarios se puede revisar de forma periódica para asegurarse de que solo las personas adecuadas tengan acceso continuado. 

- [Descripción de los informes de Azure AD](../../active-directory/reports-monitoring/index.yml)

- [Procedimiento para usar las revisiones de acceso e identidades de Azure AD](../../active-directory/governance/access-reviews-overview.md)

**Supervisión de Azure Security Center**: Sí

**Responsabilidad**: Customer

### <a name="311-monitor-attempts-to-access-deactivated-credentials"></a>3.11: Supervisión de los intentos de acceso a credenciales desactivadas

**Guía**: tiene acceso a los orígenes de registro de eventos de actividad de inicio de sesión, auditoría y riesgo de Azure AD, que permiten la integración con cualquier herramienta SIEM o de supervisión.

Para simplificar este proceso, cree una configuración de diagnóstico para las cuentas de usuario de Azure AD y envíe los registros de auditoría y los registros de inicio de sesión a un área de trabajo de Log Analytics. Puede configurar las alertas deseadas en el área de trabajo de Log Analytics.

- [Integración de los registros de actividad de Azure en Azure Monitor](../../active-directory/reports-monitoring/howto-integrate-activity-logs-with-log-analytics.md)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="312-alert-on-account-login-behavior-deviation"></a>3.12: Alerte de las desviaciones de comportamiento en los inicios de sesión de las cuentas

**Guía**: use las características de Azure Active Directory Identity Protection para configurar las respuestas automatizadas a las acciones sospechosas detectadas relacionadas con las identidades de los usuarios. También puede hacer que Azure Sentinel ingiera los datos para investigarlos más.

- [Visualización de los inicios de sesión de riesgo de Azure AD](../../active-directory/identity-protection/overview-identity-protection.md)

- [Configuración y habilitación de las directivas de riesgo de protección de identidad](../../active-directory/identity-protection/howto-identity-protection-configure-risk-policies.md)

- [Incorporación de Azure Sentinel](../../sentinel/quickstart-onboard.md)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

## <a name="data-protection"></a>Protección de los datos

*Para obtener más información, consulte [Azure Security Benchmark: Protección de datos](../../security/benchmarks/security-control-data-protection.md).*

### <a name="41-maintain-an-inventory-of-sensitive-information"></a>4.1: Mantenimiento de un inventario de información confidencial

**Instrucciones**: use etiquetas para ayudar a realizar el seguimiento de los recursos de Azure que almacenan o procesan información confidencial.

- [Creación y uso de etiquetas](./tag-resources.md)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="46-use-azure-rbac-to-manage-access-to-resources"></a>4.6: Uso de RBAC de Azure para administrar el acceso a los recursos

**Guía**: Use RBAC de Azure AD para controlar el acceso a datos y recursos; para otros fines, use métodos de control de acceso específicos del servicio.

- [Configuración de RBAC en Azure](../../role-based-access-control/role-assignments-portal.md)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="48-encrypt-sensitive-information-at-rest"></a>4.8: Cifrado de información confidencial en reposo

**Guía**: Para el cifrado del lado servidor en reposo, Azure Resource Manager admite las claves administradas por Microsoft.

- [Explicación de la protección de datos en Azure Resource Manager](azure-resource-manager-security-controls.md#data-protection)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="49-log-and-alert-on-changes-to-critical-azure-resources"></a>4.9: Registro y alerta de cambios en los recursos críticos de Azure

**Guía**: Use Azure Monitor con el registro de actividad de Azure para crear alertas para cuando se produzcan cambios en los recursos críticos de Azure.

- [Creación de alertas para los eventos del registro de actividad de Azure](../../azure-monitor/platform/alerts-activity-log.md)

**Supervisión de Azure Security Center**: Sí

**Responsabilidad**: Customer

## <a name="inventory-and-asset-management"></a>Inventario y administración de recursos

*Para obtener más información, consulte [Azure Security Benchmark: Administración de recursos y del inventario](../../security/benchmarks/security-control-inventory-asset-management.md).*

### <a name="61-use-automated-asset-discovery-solution"></a>6.1: Uso de la solución de detección de recursos automatizada

**Guía**: Use Azure Resource Graph para consultar y detectar todos los recursos (por ejemplo, proceso, almacenamiento, red, puertos, protocolos, etc.) en las suscripciones. Asegúrese de que tiene los permisos adecuados (lectura) en el inquilino y enumere todas las suscripciones de Azure, así como los recursos en las suscripciones.

Aunque los recursos clásicos de Azure se pueden detectar a través de Azure Resource Graph Explorer, se recomienda crear y usar los recursos de Azure Resource Manager que figuran a continuación.

- [Creación de consultas con Azure Resource Graph](../../governance/resource-graph/first-query-portal.md)

- [Visualización de las suscripciones de Azure](/powershell/module/az.accounts/get-azsubscription?preserve-view=true&view=azps-4.8.0)

- [Descripción de Azure RBAC](../../role-based-access-control/overview.md)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="62-maintain-asset-metadata"></a>6.2: Mantenimiento de metadatos de recursos

**Instrucciones**: Utilice el nombre, la descripción y la categoría de la directiva para organizar lógicamente los recursos según una taxonomía.

- [Para más información sobre el etiquetado de recursos, consulte la Guía de decisiones de nomenclatura y etiquetado de recursos](/azure/cloud-adoption-framework/decision-guides/resource-tagging/?toc=%2fazure%2fazure-resource-manager%2fmanagement%2ftoc.json).

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="63-delete-unauthorized-azure-resources"></a>6.3: Eliminación de recursos de Azure no autorizados

**Guía**: use el etiquetado, los grupos de administración y las suscripciones independientes, si procede, para organizar y realizar un seguimiento de los recursos de Azure. Concilie el inventario periódicamente y asegúrese de que los recursos no autorizados se eliminan de la suscripción de manera oportuna.

Además, use Azure Policy para establecer restricciones sobre el tipo de recursos que se pueden crear en las suscripciones del cliente con las siguientes definiciones de directiva integradas:

- Tipos de recursos no permitidos
- Tipos de recursos permitidos

A continuación se proporcionan más detalles relacionados:

- [Creación de suscripciones adicionales de Azure](../../cost-management-billing/manage/create-subscription.md)

- [Creación de grupos de administración](../../governance/management-groups/create-management-group-portal.md)

- [Creación y uso de etiquetas](./tag-resources.md)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="64-define-and-maintain-an-inventory-of-approved-azure-resources"></a>6.4: Definición y mantenimiento de un inventario de los recursos de Azure aprobados

**Guía**: Cree un inventario de los recursos de Azure aprobados y el software aprobado para los recursos de proceso según las necesidades de la organización.

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="65-monitor-for-unapproved-azure-resources"></a>6.5: Supervisión de recursos de Azure no aprobados

**Guía**: use Azure Policy para establecer restricciones sobre el tipo de recursos que se pueden crear en las suscripciones.

Use Azure Resource Graph para consultar y detectar recursos dentro de sus suscripciones.  Asegúrese de que todos los recursos de Azure presentes en el entorno estén aprobados.

- [Configuración y administración de Azure Policy](../../governance/policy/tutorials/create-and-manage.md)

- [Creación de consultas con Azure Resource Graph](../../governance/resource-graph/first-query-portal.md)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="69-use-only-approved-azure-services"></a>6.9: Uso exclusivo de servicios de Azure aprobados

**Guía**: Use Azure Policy para aplicar restricciones en el tipo de recursos que se pueden crear en las suscripciones del cliente con las siguientes definiciones de directiva integradas:

- Tipos de recursos no permitidos
- Tipos de recursos permitidos

A continuación se proporcionan más detalles relacionados:

- [Configuración y administración de Azure Policy](../../governance/policy/tutorials/create-and-manage.md)

- [Denegación de un tipo de recurso específico con Azure Policy](../../governance/policy/samples/built-in-policies.md#general)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="611-limit-users-ability-to-interact-with-azure-resource-manager"></a>6.11: Limitación de la capacidad de los usuarios para interactuar con Azure Resource Manager

**Guía**: Use el acceso condicional de Azure AD para limitar la capacidad de los usuarios de interactuar con Azure Resource Manager mediante la configuración de la opción "Bloquear acceso" en la aplicación "Administración de Microsoft Azure".

- [Configuración del acceso condicional para bloquear el acceso a Azure Resource Manager](../../role-based-access-control/conditional-access-azure-management.md)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="612-limit-users-ability-to-execute-scripts-in-compute-resources"></a>6.12: Limitación de la capacidad de los usuarios para ejecutar scripts en los recursos de proceso

**Instrucciones**: No aplicable; esta recomendación está pensada para los recursos de proceso.

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

## <a name="secure-configuration"></a>Configuración segura

*Para obtener más información, consulte [Azure Security Benchmark: Configuración segura](../../security/benchmarks/security-control-secure-configuration.md).*

### <a name="71-establish-secure-configurations-for-all-azure-resources"></a>7.1: Establezca configuraciones seguras para todos los recursos de Azure

**Guía**: use alias de Azure Policy para crear directivas personalizadas con el fin de auditar o aplicar la configuración de los recursos de Azure. También puede usar definiciones de Azure Policy integradas.

Azure Resource Manager tiene la capacidad de exportar la plantilla en notación de objetos JavaScript (JSON), que se debe revisar para asegurarse de que las configuraciones cumplan los requisitos de seguridad de la organización.

También puede usar las recomendaciones de Azure Security Center como línea de base de configuración segura para los recursos de Azure.

- [Visualización de los alias de Azure Policy disponibles](/powershell/module/az.resources/get-azpolicyalias?preserve-view=true&view=azps-4.8.0)

- [Tutorial: Creación y administración de directivas para aplicar el cumplimiento](../../governance/policy/tutorials/create-and-manage.md)

- [Exportación de uno y varios recursos a una plantilla en Azure Portal](../templates/export-template-portal.md)

- [Guía de referencia sobre las recomendaciones de seguridad](../../security-center/recommendations-reference.md)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="73-maintain-secure-azure-resource-configurations"></a>7.3: Mantenga configuraciones de recursos de Azure seguras

**Guía**: Use la directiva de Azure Policy [deny] y [deploy if not exist] para aplicar la configuración segura en los recursos de Azure.  Además, puede usar plantillas de Azure Resource Manager para mantener la configuración de seguridad de los recursos de Azure que requiere su organización. 

Como administrador, también puede que tenga que bloquear una suscripción, un grupo de recursos o un recurso para impedir que otros usuarios de su organización eliminen o modifiquen accidentalmente recursos esenciales. Puede establecer el bloqueo de nivel en CanNotDelete o ReadOnly.

- [Descripción de los efectos de Azure Policy](../../governance/policy/concepts/effects.md)

- [Creación y administración de directivas para aplicar el cumplimiento](../../governance/policy/tutorials/create-and-manage.md)

- [Información general sobre plantillas de Azure Resource Manager](../templates/overview.md)

- [Bloqueo de recursos para impedir cambios inesperados](lock-resources.md)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="75-securely-store-configuration-of-azure-resources"></a>7.5: Almacene de forma segura la configuración de los recursos de Azure

**Guía**: Use Azure DevOps para almacenar y administrar de forma segura el código, como definiciones de Azure Policy personalizadas, plantillas de Azure Resource Manager y scripts de Desired State Configuration. Para acceder a los recursos que administra en Azure DevOps, puede conceder o denegar permisos a usuarios específicos, grupos de seguridad integrados o grupos definidos en Azure Active Directory (Azure AD) si se integran con Azure DevOps, o Active Directory si se integran con TFS.

- [Almacenamiento de código en Azure DevOps](/azure/devops/repos/git/gitworkflow?preserve-view=true&view=azure-devops)

- [Acerca de los permisos y los grupos en Azure DevOps](/azure/devops/organizations/security/about-permissions)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="77-deploy-configuration-management-tools-for-azure-resources"></a>7.7: Implementación de herramientas de administración de configuración para recursos de Azure

**Guía**: defina e implemente configuraciones de seguridad estándar para los recursos de Azure con Azure Policy. Use alias de Azure Policy para crear directivas personalizadas con el fin de auditar o aplicar la configuración de red de los recursos de Azure. También puede usar definiciones de directivas integradas relacionadas con recursos concretos.  Además, puede usar Azure Automation para implementar los cambios de configuración.

- [Configuración y administración de Azure Policy](../../governance/policy/tutorials/create-and-manage.md)

- [Uso de alias](../../governance/policy/concepts/definition-structure.md#aliases)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="79-implement-automated-configuration-monitoring-for-azure-resources"></a>7.9: Implementación de la supervisión de la configuración automatizada para los recursos de Azure

**Guía**: Use las definiciones de Azure Policy integradas y directivas personalizadas con el fin de auditar y aplicar las configuraciones del sistema y enviar alertas sobre estas. Use la directiva de Azure Policy [audit], [deny] y [deploy if not exist] para aplicar automáticamente las configuraciones en los recursos de Azure.

- [Configuración y administración de Azure Policy](../../governance/policy/tutorials/create-and-manage.md)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="713-eliminate-unintended-credential-exposure"></a>7.13: Elimine la exposición de credenciales no intencionada

**Guía**: Utilice los procedimientos recomendados al crear la plantilla de Resource Manager, ya que ayudan a evitar problemas comunes al usar una plantilla de Resource Manager para implementar una solución.

Implemente el escáner de credenciales para identificar las credenciales en el código. El escáner de credenciales también fomenta el traslado de credenciales detectadas a ubicaciones más seguras, como Azure Key Vault. 

- [Implementación de procedimientos recomendados de seguridad de las plantillas](../templates/template-best-practices.md)

- [Configuración del escáner de credenciales](https://secdevtools.azurewebsites.net/helpcredscan.html)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

## <a name="data-recovery"></a>Recuperación de datos

*Para más información, consulte [Azure Security Benchmark: recuperación de datos](../../security/benchmarks/security-control-data-recovery.md).*

### <a name="93-validate-all-backups-including-customer-managed-keys"></a>9.3: Validación de todas las copias de seguridad, incluidas las claves administradas por el cliente

**Guía**: En caso necesario, asegúrese de poder realizar periódicamente la implementación de plantillas de Azure Resource Manager de forma habitual en una suscripción aislada.

- [Implementación de recursos con Azure Portal y plantillas de Resource Manager](../templates/deploy-portal.md)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="94-ensure-protection-of-backups-and-customer-managed-keys"></a>9.4: Garantía de la protección de las copias de seguridad y las claves administradas por el cliente

**Guía**: Use Azure DevOps para almacenar y administrar de forma segura el código, como definiciones de Azure Policy personalizadas, plantillas de Azure Resource Manager y scripts de Desired State Configuration. Para acceder a los recursos que administra en Azure DevOps, puede conceder o denegar permisos a usuarios específicos, grupos de seguridad integrados o grupos definidos en Azure Active Directory (Azure AD) si se integran con Azure DevOps, o Active Directory si se integran con TFS.

- [Almacenamiento de código en Azure DevOps](/azure/devops/repos/git/gitworkflow?preserve-view=true&view=azure-devops)

- [Acerca de los permisos y los grupos en Azure DevOps](/azure/devops/organizations/security/about-permissions)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

## <a name="incident-response"></a>Respuesta a los incidentes

*Para obtener más información, consulte [Azure Security Benchmark: Respuesta a los incidentes](../../security/benchmarks/security-control-incident-response.md).*

### <a name="101-create-an-incident-response-guide"></a>10.1: Creación de una guía de respuesta ante incidentes

**Guía**: desarrolle una guía de respuesta a incidentes para su organización. Asegúrese de que haya planes de respuesta a incidentes escritos que definan todos los roles del personal, así como las fases de administración y gestión de los incidentes, desde la detección hasta la revisión posterior a la incidencia. 

- [Guía para crear su propio proceso de respuesta a incidentes de seguridad](https://msrc-blog.microsoft.com/2019/07/01/inside-the-msrc-building-your-own-security-incident-response-process/)

- [Anatomía de un incidente del Centro de respuestas de seguridad de Microsoft](https://msrc-blog.microsoft.com/2019/06/27/inside-the-msrc-anatomy-of-a-ssirp-incident/)

- [Uso de la guía de control de incidentes de seguridad de equipos de NIST para la creación de su propio plan de respuesta a incidentes](https://csrc.nist.gov/publications/detail/sp/800-61/rev-2/final)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="103-test-security-response-procedures"></a>10.3: Prueba de los procedimientos de respuesta de seguridad

**Instrucciones**: Realice ejercicios para probar las funcionalidades de respuesta a los incidentes de los sistemas periódicamente; así, ayudará a proteger los recursos de Azure. Identifique puntos débiles y brechas y después revise el plan de respuesta según sea necesario.

- [Publicación de NIST: Guía para probar, entrenar y ejecutar programas para planes y funcionalidades de TI](https://csrc.nist.gov/publications/detail/sp/800-84/final)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="105-incorporate-security-alerts-into-your-incident-response-system"></a>10.5: Incorporación de alertas de seguridad en el sistema de respuesta a incidentes

**Instrucciones**: exporte las alertas y recomendaciones de Azure Security Center y mediante la característica de exportación continua para ayudar a identificar riesgos en los recursos de Azure. La exportación continua permite exportar alertas y recomendaciones de forma manual o continua. Puede usar el conector de datos de Azure Security Center para enviar las alertas a Azure Sentinel.

- [Configuración de la exportación continua](../../security-center/continuous-export.md)

- [Transmisión de alertas a Azure Sentinel](../../sentinel/connect-azure-security-center.md)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="106-automate-the-response-to-security-alerts"></a>10.6: Automatización de la respuesta a las alertas de seguridad

**Guía**: use la característica de automatización de flujos de trabajo de Azure Security Center para desencadenar automáticamente las alertas y recomendaciones de seguridad con el fin de proteger los recursos de Azure.

- [Configuración de la automatización de flujos de trabajo en Security Center](../../security-center/workflow-automation.md)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

## <a name="penetration-tests-and-red-team-exercises"></a>Pruebas de penetración y ejercicios del equipo rojo

*Para obtener más información, consulte [Azure Security Benchmark: Pruebas de penetración y ejercicios del equipo rojo](../../security/benchmarks/security-control-penetration-tests-red-team-exercises.md).*

### <a name="111-conduct-regular-penetration-testing-of-your-azure-resources-and-ensure-remediation-of-all-critical-security-findings"></a>11.1: Realice pruebas de penetración periódicas de los recursos de Azure y asegúrese de corregir todos los resultados de seguridad críticos

**Guía**: Siga las reglas de compromiso de la prueba de penetración de Microsoft Cloud para asegurarse de que las pruebas de penetración no infrinjan las directivas de Microsoft. Use la estrategia de Microsoft y la ejecución de las pruebas de penetración del equipo rojo y sitios activos en la infraestructura de nube, los servicios y las aplicaciones administradas por Microsoft.

- [Reglas de interacción de las pruebas de penetración](https://www.microsoft.com/msrc/pentest-rules-of-engagement?rtc=1)

- [Equipo rojo de Microsoft Cloud](https://gallery.technet.microsoft.com/Cloud-Red-Teaming-b837392e)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Compartido

## <a name="next-steps"></a>Pasos siguientes

- Consulte la [prueba comparativa de seguridad de Azure](../../security/benchmarks/overview.md).
- Obtenga más información sobre las [líneas de base de seguridad de Azure](../../security/benchmarks/security-baselines-overview.md).
---
title: Línea de base de seguridad de Azure para Azure Sentinel
description: La línea de base de seguridad de Azure Sentinel proporciona instrucciones de procedimientos y recursos para implementar las recomendaciones de seguridad especificadas en Azure Security Benchmark.
author: msmbaldwin
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.topic: conceptual
ms.date: 09/10/2020
ms.author: mbaldwin
ms.custom: subject-security-benchmark
ms.openlocfilehash: b1fbedf45514455f0e19461e5ee0558bb2914c58
ms.sourcegitcommit: 0dcafc8436a0fe3ba12cb82384d6b69c9a6b9536
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2020
ms.locfileid: "94427724"
---
# <a name="azure-security-baseline-for-azure-sentinel"></a>Línea de base de seguridad de Azure para Azure Sentinel

Esta línea de base de seguridad aplica las instrucciones de [Azure Security Benchmark, versión 1.0](../security/benchmarks/overview.md) a Azure Sentinel. Azure Security Benchmark proporciona recomendaciones sobre cómo puede proteger sus soluciones de nube en Azure. El contenido se agrupa por medio de los **controles de seguridad** definidos por Azure Security Benchmark y las directrices aplicables a Azure Sentinel. Se han excluido los **controles** no aplicables a Azure Sentinel. Para ver cómo Azure Sentinel se asigna por completo a Azure Security Benchmark, consulte el [archivo completo de asignación de línea de base de seguridad de Azure Sentinel](https://github.com/MicrosoftDocs/SecurityBenchmarks/tree/master/Azure%20Offer%20Security%20Baselines).

## <a name="network-security"></a>Seguridad de las redes

*Para obtener más información, consulte [Azure Security Benchmark: Seguridad de redes](../security/benchmarks/security-control-network-security.md).*

### <a name="11-protect-azure-resources-within-virtual-networks"></a>1.1: Protección de los recursos de Azure dentro de las redes virtuales

**Guía** : No se pueden asociar redes virtuales, subredes ni grupos de seguridad de red directamente a Azure Sentinel. Sin embargo, puede habilitar un punto de conexión privado de Azure para el área de trabajo de Log Analytics asociada a Azure Sentinel para limitar la comunicación hacia las redes privadas y desde allí.

**Supervisión de Azure Security Center** : no disponible actualmente

**Responsabilidad** : Customer

### <a name="111-use-automated-tools-to-monitor-network-resource-configurations-and-detect-changes"></a>1.11: Use herramientas automatizadas para supervisar las configuraciones de recursos de red y detectar cambios

**Guía** : Use el registro de actividad de Azure para supervisar las configuraciones de los recursos de red y detectar cambios en ellos relacionados con el área de trabajo de Azure Sentinel. Cree alertas en Azure Monitor que se desencadenarán cuando se produzcan cambios en los recursos críticos.

- [Habilitación de la auditoría en Azure Sentinel](resources.md)

- [Visualización y recuperación de eventos del registro de actividad de Azure](/azure/azure-monitor/platform/activity-log-view)

- [Creación de alertas en Azure Monitor](../azure-monitor/platform/alerts-activity-log.md)

**Supervisión de Azure Security Center** : no disponible actualmente

**Responsabilidad** : Customer

## <a name="logging-and-monitoring"></a>Registro y supervisión

*Para obtener más información, consulte [Azure Security Benchmark: registro y supervisión](../security/benchmarks/security-control-logging-monitoring.md).*

### <a name="22-configure-central-security-log-management"></a>2.2: Configuración de la administración central de registros de seguridad

**Instrucciones** : Los registros de auditoría de Azure Sentinel se mantienen en los registros de actividad de Azure. Puede ver estos datos transmitiéndolos desde el registro de actividades de Azure a Azure Sentinel, donde podrá realizar investigaciones con ellos y analizarlos.

- [Habilitación de la auditoría en Azure Sentinel](resources.md)

**Supervisión de Azure Security Center** : Sí

**Responsabilidad** : Customer

### <a name="23-enable-audit-logging-for-azure-resources"></a>2.3: Habilitación del registro de auditoría para recursos de Azure

**Guía** : Los registros de auditoría de Azure Sentinel se mantienen en los registros de actividad de Azure. Puede ver estos datos transmitiéndolos desde el registro de actividades de Azure a Azure Sentinel, donde podrá realizar investigaciones con ellos y analizarlos.

- [Habilitación de la auditoría en Azure Sentinel](resources.md)

**Supervisión de Azure Security Center** : Sí

**Responsabilidad** : Customer

### <a name="25-configure-security-log-storage-retention"></a>2.5: Configuración de la retención del almacenamiento de registros de seguridad

**Guía** : En Azure Monitor, establezca el período de retención de registros en las áreas de trabajo de Log Analytics asociadas a las áreas de trabajo de Azure Sentinel atendiendo a las normativas de cumplimiento de su organización.

- [Establecimiento de parámetros de retención de registros](../azure-monitor/platform/manage-cost-storage.md#change-the-data-retention-period)

**Supervisión de Azure Security Center** : No aplicable

**Responsabilidad** : Customer

### <a name="26-monitor-and-review-logs"></a>2.6: Supervisión y revisión de registros

**Guía** : Los registros de auditoría de Azure Sentinel se mantienen en los registros de actividad de Azure. Para ver estos datos, transmítalos desde el registro de actividad de Azure a Azure Sentinel, donde podrá investigarlos y analizarlos. Analice y supervise los registros de las instancias del registro de actividad de Azure Sentinel en busca de comportamientos anómalos. Use la sección "Registros" del área de trabajo de Azure Sentinel para realizar consultas o crear alertas basadas en los registros de Sentinel.

- [Habilitación de la auditoría en Azure Sentinel](resources.md)

**Supervisión de Azure Security Center** : No aplicable

**Responsabilidad** : Customer

### <a name="27-enable-alerts-for-anomalous-activities"></a>2.7: Habilitación de alertas para actividades anómalas

**Guía** : Los registros de auditoría de Azure Sentinel se mantienen en los registros de actividad de Azure. Para ver estos datos, transmítalos desde el registro de actividad de Azure a Azure Sentinel, donde podrá investigarlos y analizarlos. Analice y supervise los registros de las instancias del registro de actividad de Azure Sentinel en busca de comportamientos anómalos. Use la sección "Registros" del área de trabajo de Azure Sentinel para realizar consultas o crear alertas basadas en los registros de Sentinel.

- [Habilitación de la auditoría en Azure Sentinel](resources.md)

**Supervisión de Azure Security Center** : Sí

**Responsabilidad** : Customer

## <a name="identity-and-access-control"></a>Control de identidades y acceso

*Para obtener más información, consulte [Azure Security Benchmark: Identidad y control de acceso](../security/benchmarks/security-control-identity-access-control.md).*

### <a name="31-maintain-an-inventory-of-administrative-accounts"></a>3.1: Mantenga un inventario de cuentas administrativas

**Guía** : Mantenga un inventario de las cuentas de usuario que tienen acceso administrativo al plano de control (por ejemplo, Azure Portal) del área de trabajo de Azure Sentinel. 

Puede usar el panel Administración de identidad y acceso (IAM) de Azure Portal para la suscripción a fin de configurar el control de acceso basado en rol de Azure (Azure RBAC). Los roles se aplican a usuarios, grupos, entidades de servicio e identidades administradas en Active Directory.  Azure Sentinel también usa Azure RBAC para proporcionar roles administrativos integrados (por ejemplo, colaborador de Azure Sentinel), que se pueden asignar a usuarios, grupos y servicios de Azure. 

- [Descripción de los roles personalizados](../role-based-access-control/custom-roles.md)

- [Uso de Azure RBAC en Azure Sentinel](roles.md)

- [Procedimiento para configurar Azure RBAC en libros](quickstart-get-visibility.md)

**Supervisión de Azure Security Center** : Sí

**Responsabilidad** : Customer

### <a name="33-use-dedicated-administrative-accounts"></a>3.3: Use cuentas administrativas dedicadas

**Guía** : Cree procedimientos operativos estándar en torno al uso de cuentas administrativas dedicadas. Use la administración de identidad y acceso de Azure Security Center para supervisar el número de cuentas administrativas.

Además, para ayudarle a realizar un seguimiento de las cuentas administrativas dedicadas, puede seguir las recomendaciones de Azure Security Center o las directivas integradas de Azure Policy, por ejemplo:

- Debe haber más de un propietario asignado a su suscripción
- Las cuentas en desuso con permisos de propietario deben quitarse de la suscripción
- Las cuentas externas con permisos de propietario deben quitarse de la suscripción

- [Uso de Azure Security Center para supervisar la identidad y el acceso (versión preliminar)](../security-center/security-center-identity-access.md)

- [Uso de Azure Policy](../governance/policy/tutorials/create-and-manage.md)

**Supervisión de Azure Security Center** : Sí

**Responsabilidad** : Customer

### <a name="34-use-azure-active-directory-single-sign-on-sso"></a>3.4: Uso del inicio de sesión único (SSO) de Azure Active Directory

**Guía** : El acceso al plano de control en el área de trabajo de Azure Sentinel (por ejemplo, Azure Portal) está disponible a través de la API REST y admite inicio de sesión único. Para autenticarse, establezca el encabezado de autorización de las solicitudes en JSON Web Token, que se obtiene de Azure Active Directory (AAD).

- [Uso de la API REST de Azure Log Analytics](/rest/api/loganalytics/)

- [Descripción del SSO con Azure AD](../active-directory/manage-apps/what-is-single-sign-on.md)

**Supervisión de Azure Security Center** : No aplicable

**Responsabilidad** : Customer

### <a name="35-use-multi-factor-authentication-for-all-azure-active-directory-based-access"></a>3.5: Uso de la autenticación multifactor para todo el acceso basado en Azure Active Directory

**Guía** : Habilite Multi-Factor Authentication (MFA) de Azure Active Directory y siga las recomendaciones de administración de identidad y acceso de Azure Security Center.

- [Procedimiento para habilitar la MFA en Azure](../active-directory/authentication/howto-mfa-getstarted.md)

- [Supervisión de la identidad y el acceso en Azure Security Center](../security-center/security-center-identity-access.md)

**Supervisión de Azure Security Center** : Sí

**Responsabilidad** : Customer

### <a name="36-use-secure-azure-managed-workstations-for-administrative-tasks"></a>3.6: Uso de estaciones de trabajo seguras y administradas por Azure para realizar tareas administrativas

**Guía** : use una estación de trabajo de acceso con privilegios (PAW) con Azure Multi-Factor Authentication (MFA) habilitado para iniciar sesión en los recursos relacionados con Azure Sentinel y configurarlos. 

- [Uso de estaciones de trabajo con privilegios de acceso](/windows-server/identity/securing-privileged-access/privileged-access-workstations)

- [Planeamiento de una implementación de Azure Multi-Factor Authentication basada en la nube](../active-directory/authentication/howto-mfa-getstarted.md)

**Supervisión de Azure Security Center** : No aplicable

**Responsabilidad** : Customer

### <a name="37-log-and-alert-on-suspicious-activities-from-administrative-accounts"></a>3.7: Registro y alerta de actividades sospechosas desde cuentas administrativas

**Guía** : Use Privileged Identity Management (PIM) de Azure Active Directory (AD) para la generación de registros y alertas cuando se produzca una actividad sospechosa o insegura en el entorno.

Además, use las detecciones de riesgo de Azure AD para ver alertas e informes sobre el comportamiento de los usuarios de riesgo.

- [Cómo implementar Privileged Identity Management (PIM)](../active-directory/privileged-identity-management/pim-deployment-plan.md)

- [Información sobre las detecciones de riesgo de Azure AD](/azure/active-directory/reports-monitoring/concept-risk-events)

**Supervisión de Azure Security Center** : Sí

**Responsabilidad** : Customer

### <a name="38-manage-azure-resources-from-only-approved-locations"></a>3.8: Administre los recursos de Azure solo desde ubicaciones aprobadas

**Guía** : El cliente debe usar ubicaciones con nombre de acceso condicional para permitir el acceso a Azure Portal solo desde agrupaciones lógicas específicas de intervalos de direcciones IP o países o regiones.

- [Configuración de ubicaciones con nombre en Azure](../active-directory/reports-monitoring/quickstart-configure-named-locations.md)

**Supervisión de Azure Security Center** : No aplicable

**Responsabilidad** : Customer

### <a name="39-use-azure-active-directory"></a>3.9: Uso de Azure Active Directory

**Guía** : use Azure Active Directory (AD) como sistema central de autenticación y autorización para las instancias de Azure Sentinel. Azure AD protege los datos mediante un cifrado seguro para los datos en reposo y en tránsito. Azure AD también cifra con sal, convierte en hash y almacena de forma segura las credenciales de los usuarios.

- [Creación y configuración de un inquilino de Azure AD](../active-directory/fundamentals/active-directory-access-create-new-tenant.md)

**Supervisión de Azure Security Center** : No aplicable

**Responsabilidad** : Customer

### <a name="310-regularly-review-and-reconcile-user-access"></a>3.10: Revise y concilie regularmente el acceso de los usuarios

**Guía** : Azure Active Directory (AD) proporciona registros para ayudar a descubrir cuentas obsoletas. Además, use las revisiones de acceso de identidad de Azure para administrar de forma eficiente las pertenencias a grupos, el acceso a las aplicaciones empresariales y las asignaciones de roles. El acceso de los usuarios se puede revisar de forma periódica para asegurarse de que solo las personas adecuadas tengan acceso continuado. 

- [Descripción de los informes de Azure AD](/azure/active-directory/reports-monitoring/)

- [Procedimiento para usar las revisiones de acceso de identidad de Azure](../active-directory/governance/access-reviews-overview.md)

**Supervisión de Azure Security Center** : No aplicable

**Responsabilidad** : Customer

### <a name="311-monitor-attempts-to-access-deactivated-credentials"></a>3.11: Supervisión de los intentos de acceso a credenciales desactivadas

**Guía** : Use Azure Active Directory (AD) como sistema central de autenticación y autorización para las áreas de trabajo de Azure Sentinel. Azure AD protege los datos mediante un cifrado seguro para los datos en reposo y en tránsito. Azure AD también cifra con sal, convierte en hash y almacena de forma segura las credenciales de los usuarios.

Tiene acceso a los orígenes de registro de eventos de actividad de inicio de sesión, auditoría y riesgo de Azure AD, que permiten la integración en Azure Sentinel o SIEM de terceros.

Para simplificar este proceso, cree una configuración de diagnóstico para las cuentas de usuario de Azure AD y envíe los registros de auditoría y los registros de inicio de sesión a un área de trabajo de Log Analytics. Puede configurar las alertas de registro deseadas en Log Analytics.

- [Integración de los registros de actividad de Azure en Azure Monitor](/azure/active-directory/reports-monitoring/howto-integrate-activity-logs-with-log-analytics)

- [Incorporación de Azure Sentinel](quickstart-onboard.md)

**Supervisión de Azure Security Center** : No aplicable

**Responsabilidad** : Customer

### <a name="312-alert-on-account-sign-in-behavior-deviation"></a>3.12: Alerta sobre las desviaciones de comportamiento en los inicios de sesión de las cuentas

**Guía** : para la desviación del comportamiento de inicio de sesión de cuenta en el plan de control (por ejemplo, Azure Portal), use las características de detección de riesgos y de Azure AD Identity Protection para configurar respuestas automatizadas a las acciones sospechosas detectadas relacionadas con las identidades de los usuarios. También puede hacer que Azure Sentinel ingiera los datos para investigarlos más.

- [Visualización del inicio de sesión de riesgo de Azure AD](/azure/active-directory/reports-monitoring/concept-risky-sign-ins)

- [Configuración y habilitación de las directivas de riesgo de protección de identidad](../active-directory/identity-protection/howto-identity-protection-configure-risk-policies.md)

- [Incorporación de Azure Sentinel](quickstart-onboard.md)

**Supervisión de Azure Security Center** : no disponible actualmente

**Responsabilidad** : Customer

### <a name="313-provide-microsoft-with-access-to-relevant-customer-data-during-support-scenarios"></a>3.13: Proporcione a Microsoft acceso a los datos pertinentes del cliente durante los escenarios de soporte técnico

**Guía** : Actualmente no está disponible; Caja de seguridad del cliente aún no es compatible con el área de trabajo de Azure Sentinel o Log Analytics.

- [Lista de servicios admitidos por la Caja de seguridad del cliente](../security/fundamentals/customer-lockbox-overview.md#supported-services-and-scenarios-in-general-availability)

**Supervisión de Azure Security Center** : no disponible actualmente

**Responsabilidad** : no disponible actualmente

## <a name="data-protection"></a>Protección de los datos

*Para obtener más información, consulte [Azure Security Benchmark: Protección de datos](../security/benchmarks/security-control-data-protection.md).*

### <a name="41-maintain-an-inventory-of-sensitive-information"></a>4.1: Mantenimiento de un inventario de información confidencial

**Instrucciones** : use etiquetas para ayudar a realizar el seguimiento de los recursos de Azure que almacenan o procesan información confidencial.

- [Creación y uso de etiquetas](/azure/azure-resource-manager/resource-group-using-tags)

**Supervisión de Azure Security Center** : No aplicable

**Responsabilidad** : Customer

### <a name="42-isolate-systems-storing-or-processing-sensitive-information"></a>4.2: Aislamiento de los sistemas que almacenan o procesan información confidencial

**Guía** : Implemente suscripciones o grupos de administración independientes para las áreas de trabajo de desarrollo, pruebas y producción de Sentinel.

- [Creación de suscripciones adicionales de Azure](/azure/billing/billing-create-subscription)

- [Creación de grupos de administración](/azure/governance/management-groups/create)

**Supervisión de Azure Security Center** : No aplicable

**Responsabilidad** : Customer

### <a name="44-encrypt-all-sensitive-information-in-transit"></a>4.4: Cifrado de toda la información confidencial en tránsito

**Guía** : Si usa una máquina virtual local o de Azure como reenviador de syslog, tendrá que configurar el demonio de syslog (rsyslog o syslog-ng) para comunicarse en TLS.

- [Conexión de su solución externa con Common Event Format](connect-common-event-format.md)

**Supervisión de Azure Security Center** : no disponible actualmente

**Responsabilidad** : Compartido

### <a name="45-use-an-active-discovery-tool-to-identify-sensitive-data"></a>4.5: Uso de una herramienta de detección activa para identificar datos confidenciales

**Guía** : Las características de identificación, clasificación y prevención de pérdida de datos todavía no están disponibles en Azure. Implemente una solución de terceros, si es necesario, para fines de cumplimiento.

En el caso de la plataforma subyacente administrada por Microsoft, Microsoft trata todo el contenido de los clientes como confidencial y hace grandes esfuerzos para proteger a los clientes contra la pérdida y exposición de sus datos. Para garantizar la seguridad de los datos de los clientes dentro de Azure, Microsoft ha implementado y mantiene un conjunto de controles y funcionalidades eficaces de protección de datos.

- [Descripción de la protección de datos de los clientes en Azure](../security/fundamentals/protection-customer-data.md)

**Supervisión de Azure Security Center** : No aplicable

**Responsabilidad** : Compartido

### <a name="46-use-role-based-access-control-to-control-access-to-resources"></a>4.6: Uso del control de acceso basado en rol para controlar el acceso a los recursos

**Guía** : Puede usar el panel Administración de identidad y acceso (IAM) de Azure Portal para configurar el control de acceso basado en rol de Azure (Azure RBAC). Los roles se aplican a usuarios, grupos, entidades de servicio e identidades administradas en Active Directory. Puede usar roles personalizados o roles integrados de Azure para usuarios y grupos. 

Azure Sentinel usa Azure RBAC para proporcionar roles integrados que se pueden asignar a usuarios, grupos y servicios en Azure. Con Azure RBAC, puede usar y crear roles dentro del equipo de operaciones de seguridad para conceder el acceso adecuado a Azure Sentinel. Según los roles, puede tener un control pormenorizado sobre lo que los usuarios con acceso a Azure Sentinel pueden ver. Puede asignar roles de Azure en el área de trabajo de Azure Sentinel directamente, o a una suscripción o grupo de recursos al que pertenezca el área de trabajo. Hay tres roles integrados específicos de Azure Sentinel.

- Lector de Azure Sentinel
- Respondedor de Azure Sentinel
- Colaborador de Azure Sentinel

Además de los roles de Azure dedicados de Azure Sentinel, están los roles de Azure y los roles de Azure integrados de Log Analytics que pueden conceder un conjunto más amplio de permisos que incluyen el acceso al área de trabajo de Azure Sentinel y a otros recursos:

Los roles de Azure incluyen propietario, colaborador y lector. Los roles de Azure conceden acceso a todos los recursos de Azure, incluidas las áreas de trabajo de Log Analytics y los recursos de Azure Sentinel.

Los roles de Log Analytics incluyen colaborador y lector de Log Analytics. Los roles de Log Analytics conceden acceso a todas las áreas de trabajo de Log Analytics.

Además, cada libro de Sentinel es un recurso de Azure, y se pueden asignar roles a los usuarios para administrar el acceso.

- [Configuración de Azure RBAC](../role-based-access-control/role-assignments-portal.md)

- [Descripción de los roles personalizados](../role-based-access-control/custom-roles.md)

- [Descripción de los roles en Sentinel y Log Analytics](roles.md)

- [Procedimiento para configurar Azure RBAC en libros](quickstart-get-visibility.md)

**Supervisión de Azure Security Center** : No aplicable

**Responsabilidad** : Customer

### <a name="48-encrypt-sensitive-information-at-rest"></a>4.8: Cifrado de información confidencial en reposo

**Guía** : Actualmente, las áreas de trabajo de Azure Sentinel y Log Analytics de Azure Monitor usan claves administradas por Microsoft para cifrar los datos en reposo incluidos. La posibilidad de aportar su propia clave todavía no se admite por completo en Sentinel, pero se hará en breve.

- [Introducción a las claves administradas por el cliente de Azure Monitor](../azure-monitor/platform/customer-managed-keys.md#customer-managed-key-overview)

- [Uso de las claves administradas por el cliente en Azure Sentinel (versión preliminar)](customer-managed-keys.md)

**Supervisión de Azure Security Center** : no disponible actualmente

**Responsabilidad** : Compartido

### <a name="49-log-and-alert-on-changes-to-critical-azure-resources"></a>4.9: Registro y alerta de cambios en los recursos críticos de Azure

**Guía** : Los registros de auditoría de Azure Sentinel se mantienen en los registros de actividad de Azure. Puede ver estos datos transmitiéndolos desde el registro de actividades de Azure a Azure Sentinel, donde podrá realizar investigaciones con ellos y analizarlos. En la sección "Registros" del área de trabajo de Azure Sentinel, puede crear alertas para cuando se produzcan cambios en las áreas de trabajo de Azure Sentinel de producción, así como en otros recursos críticos o relacionados.

- [Habilitación de la auditoría en Azure Sentinel](resources.md)

**Supervisión de Azure Security Center** : Sí

**Responsabilidad** : Customer

## <a name="inventory-and-asset-management"></a>Inventario y administración de recursos

*Para obtener más información, consulte [Azure Security Benchmark: Administración de recursos y del inventario](../security/benchmarks/security-control-inventory-asset-management.md).*

### <a name="61-use-automated-asset-discovery-solution"></a>6.1: Uso de la solución de detección de recursos automatizada

**Guía** : Use Azure Resource Graph para consultar o detectar todos los recursos (por ejemplo, proceso, almacenamiento, red, puertos y protocolos, etc.) dentro de las suscripciones.  Asegúrese de que tiene los permisos adecuados (lectura) en el inquilino y enumere todas las suscripciones de Azure, así como los recursos de las suscripciones.

Aunque los recursos clásicos de Azure se pueden detectar a través de Resource Graph, a partir de ahora es muy recomendable crear y usar los recursos con Azure Resource Manager.

- [Creación de consultas con Azure Resource Graph](../governance/resource-graph/first-query-portal.md)

- [Visualización de las suscripciones de Azure](https://docs.microsoft.com/powershell/module/az.accounts/get-azsubscription?view=azps-3.0.0)

- [Descripción de Azure RBAC](../role-based-access-control/overview.md)

**Supervisión de Azure Security Center** : No aplicable

**Responsabilidad** : Customer

### <a name="62-maintain-asset-metadata"></a>6.2: Mantenimiento de metadatos de recursos

**Instrucciones** : Aplique etiquetas a los recursos de Azure que proporcionan metadatos para organizarlos de forma lógica en una taxonomía.

- [Creación y uso de etiquetas](/azure/azure-resource-manager/resource-group-using-tags)

**Supervisión de Azure Security Center** : No aplicable

**Responsabilidad** : Customer

### <a name="63-delete-unauthorized-azure-resources"></a>6.3: Eliminación de recursos de Azure no autorizados

**Guía** : use el etiquetado, los grupos de administración y las suscripciones independientes, si procede, para organizar y realizar un seguimiento de los recursos de Azure. Concilie el inventario periódicamente y asegúrese de que los recursos no autorizados se eliminan de la suscripción de manera oportuna.

Además, use Azure Policy para establecer restricciones sobre el tipo de recursos que se pueden crear en las suscripciones del cliente con las siguientes definiciones de directiva integradas:

- Tipos de recursos no permitidos
- Tipos de recursos permitidos

- [Creación de suscripciones adicionales de Azure](/azure/billing/billing-create-subscription)

- [Creación de grupos de administración](/azure/governance/management-groups/create)

- [Creación y uso de etiquetas](/azure/azure-resource-manager/resource-group-using-tags)

**Supervisión de Azure Security Center** : No aplicable

**Responsabilidad** : Customer

### <a name="65-monitor-for-unapproved-azure-resources"></a>6.5: Supervisión de recursos de Azure no aprobados

**Guía** : Use Azure Policy para establecer restricciones en el tipo de recursos que se pueden crear en sus suscripciones. 

Use Azure Resource Graph para consultar o detectar recursos dentro de sus suscripciones.  Asegúrese de que todos los recursos de Azure presentes en el entorno estén aprobados.

- [Configuración y administración de Azure Policy](../governance/policy/tutorials/create-and-manage.md)

- [Creación de consultas con Azure Graph](../governance/resource-graph/first-query-portal.md)

**Supervisión de Azure Security Center** : No aplicable

**Responsabilidad** : Customer

### <a name="69-use-only-approved-azure-services"></a>6.9: Uso exclusivo de servicios de Azure aprobados

**Guía** : use Azure Policy para establecer restricciones sobre el tipo de recursos que se pueden crear en las suscripciones del cliente con las siguientes definiciones de directiva integradas:

- Tipos de recursos no permitidos
- Tipos de recursos permitidos

- [Configuración y administración de Azure Policy](../governance/policy/tutorials/create-and-manage.md)

- [Denegación de un tipo de recurso específico con Azure Policy](/azure/governance/policy/samples/not-allowed-resource-types)

**Supervisión de Azure Security Center** : No aplicable

**Responsabilidad** : Customer

### <a name="611-limit-users-ability-to-interact-with-azure-resource-manager"></a>6.11: Limitación de la capacidad de los usuarios para interactuar con Azure Resource Manager

**Instrucciones** : Configure el acceso condicional de Azure para limitar la capacidad de los usuarios de interactuar con Azure Resource Manager configurando "Bloquear acceso" en la aplicación Microsoft Azure Management.

- [Configuración del acceso condicional para bloquear el acceso a Azure Resource Manager](../role-based-access-control/conditional-access-azure-management.md)

**Supervisión de Azure Security Center** : No aplicable

**Responsabilidad** : Customer

## <a name="secure-configuration"></a>Configuración segura

*Para obtener más información, consulte [Azure Security Benchmark: Configuración segura](../security/benchmarks/security-control-secure-configuration.md).*

### <a name="71-establish-secure-configurations-for-all-azure-resources"></a>7.1: Establezca configuraciones seguras para todos los recursos de Azure

**Instrucciones** : Defina e implemente configuraciones de seguridad estándar para el área de trabajo de Log Analytics asociada al área de trabajo de Sentinel con Azure Policy. Use alias de Azure Policy en el espacio de nombres "Microsoft.DataLakeAnalytics" para crear directivas personalizadas a fin de auditar o aplicar la configuración de las áreas de trabajo de Log Analytics.

- [Visualización de los alias de Azure Policy disponibles](https://docs.microsoft.com/powershell/module/az.resources/get-azpolicyalias?view=azps-3.3.0)

- [Configuración y administración de Azure Policy](../governance/policy/tutorials/create-and-manage.md)

**Supervisión de Azure Security Center** : No aplicable

**Responsabilidad** : Customer

### <a name="73-maintain-secure-azure-resource-configurations"></a>7.3: Mantenga configuraciones de recursos de Azure seguras

**Instrucciones** : Use la directiva de Azure [denegar] e [implementar si no existe] para aplicar la configuración segura en los recursos de Azure.

- [Configuración y administración de Azure Policy](../governance/policy/tutorials/create-and-manage.md)

- [Descripción de los efectos de Azure Policy](../governance/policy/concepts/effects.md)

**Supervisión de Azure Security Center** : No aplicable

**Responsabilidad** : Customer

### <a name="75-securely-store-configuration-of-azure-resources"></a>7.5: Almacene de forma segura la configuración de los recursos de Azure

**Instrucciones** : Si usa definiciones de directivas personalizadas de Azure, use Azure DevOps o Azure Repos para almacenar y administrar el código de forma segura.

- [Almacenamiento de código en Azure DevOps](https://docs.microsoft.com/azure/devops/repos/git/gitworkflow?view=azure-devops)

- [Documentación de Azure Repos](https://docs.microsoft.com/azure/devops/repos/index?view=azure-devops)

**Supervisión de Azure Security Center** : No aplicable

**Responsabilidad** : Customer

### <a name="77-deploy-configuration-management-tools-for-azure-resources"></a>7.7: Implementación de herramientas de administración de configuración para recursos de Azure

**Guía** : Use las definiciones de Azure Policy integradas junto con los alias de Azure Policy en el espacio de nombres "Microsoft.OperationalInsights" para crear directivas personalizadas con el fin de auditar y aplicar las configuraciones del sistema y enviar alertas sobre ellas. Además, desarrolle un proceso y una canalización para administrar las excepciones de las directivas.

- [Configuración y administración de Azure Policy](../governance/policy/tutorials/create-and-manage.md)

**Supervisión de Azure Security Center** : No aplicable

**Responsabilidad** : Customer

### <a name="79-implement-automated-configuration-monitoring-for-azure-resources"></a>7.9: Implementación de la supervisión de la configuración automatizada para los recursos de Azure

**Guía** : Use las definiciones de Azure Policy integradas junto con los alias de Azure Policy en el espacio de nombres "Microsoft.OperationalInsights" para crear directivas personalizadas con el fin de auditar y aplicar las configuraciones del sistema y enviar alertas sobre ellas. Use la directiva de Azure [audit], [deny] y [deploy if not exist] para aplicar automáticamente las configuraciones en los recursos de Azure.

- [Configuración y administración de Azure Policy](../governance/policy/tutorials/create-and-manage.md)

**Supervisión de Azure Security Center** : No aplicable

**Responsabilidad** : Customer

### <a name="711-manage-azure-secrets-securely"></a>7.11: Administre los secretos de Azure de forma segura

**Instrucciones** : Azure Sentinel admite la recopilación de registros de varios orígenes mediante diversos conectores. Algunos de estos conectores requieren la instalación con una clave de área de trabajo de Log Analytics. Al configurar estos conectores, use Azure Key Vault para almacenar las claves con el fin de simplificar la administración de secretos y evitar la exposición accidental de credenciales.

- [Conexión de Sentinel a orígenes de datos](connect-data-sources.md)

- [Creación de un almacén de claves](/azure/key-vault/quick-create-portal) 

- [Cómo proporcionar la autenticación de Key Vault con una identidad administrada](/azure/key-vault/managed-identity)

**Supervisión de Azure Security Center** : Sí

**Responsabilidad** : Customer

### <a name="713-eliminate-unintended-credential-exposure"></a>7.13: Elimine la exposición de credenciales no intencionada

**Instrucciones** : Implemente el escáner de credenciales para identificar las credenciales en el código. El escáner de credenciales también fomenta el traslado de credenciales detectadas a ubicaciones más seguras, como Azure Key Vault.

- [Configuración del escáner de credenciales](https://secdevtools.azurewebsites.net/helpcredscan.html)

**Supervisión de Azure Security Center** : No aplicable

**Responsabilidad** : Customer

## <a name="malware-defense"></a>Defensa contra malware

*Para obtener más información, consulte [Azure Security Benchmark: defensa contra malware](../security/benchmarks/security-control-malware-defense.md).*

### <a name="82-pre-scan-files-to-be-uploaded-to-non-compute-azure-resources"></a>8.2: Examine previamente los archivos que se van a cargar en recursos de Azure que no son de proceso

**Guía** : Microsoft Antimalware está habilitado en el host subyacente que admite los servicios de Azure (por ejemplo, Azure Sentinel y Log Analytics), pero no se ejecuta en contenido del cliente. 

Es su responsabilidad realizar un examen previo del contenido que se carga en los recursos de Azure que no son de proceso, como el área de trabajo de Log Analytics. Microsoft no puede acceder a los datos del cliente y, por lo tanto, no puede realizar exámenes antimalware del contenido del cliente en su nombre.

**Supervisión de Azure Security Center** : No aplicable

**Responsabilidad** : Customer

## <a name="data-recovery"></a>Recuperación de datos

*Para obtener más información, consulte [Azure Security Benchmark: recuperación de datos](../security/benchmarks/security-control-data-recovery.md).*

### <a name="94-ensure-protection-of-backups-and-customer-managed-keys"></a>9.4: Garantía de la protección de las copias de seguridad y las claves administradas por el cliente

**Instrucciones** : Habilite la eliminación temporal en Key Vault para proteger las claves contra la eliminación accidental o malintencionada. En caso de que un usuario revoque la clave de cifrado de claves, ya sea mediante su eliminación o la eliminación del acceso a Azure Sentinel, en una hora, Azure Sentinel aplica el cambio y se comporta como si los datos ya no estuvieran disponibles. Desde ese momento se evita cualquier operación realizada que use recursos de almacenamiento persistente, como la ingesta de datos, los cambios de configuración persistentes y la creación de incidentes. Los datos almacenados previamente no se eliminan, pero permanecen inaccesibles. Los datos inaccesibles se rigen por la directiva de retención de datos y se purgan conforme a esa directiva.

La única operación posible una vez revocada o eliminada la clave de cifrado es la eliminación de la cuenta.

Si se restaura el acceso después de la revocación, Azure Sentinel restaura el acceso a los datos en una hora.

- [Habilitación de la eliminación temporal en Key Vault](https://docs.microsoft.com/azure/storage/blobs/storage-blob-soft-delete?tabs=azure-portal)

- [Uso de las claves administradas por el cliente en Azure Sentinel](customer-managed-keys.md)

**Supervisión de Azure Security Center** : Sí

**Responsabilidad** : Customer

## <a name="incident-response"></a>Respuesta a los incidentes

*Para obtener más información, consulte [Azure Security Benchmark: Respuesta a los incidentes](../security/benchmarks/security-control-incident-response.md).*

### <a name="101-create-an-incident-response-guide"></a>10.1: Creación de una guía de respuesta ante incidentes

**Instrucciones** : Cree una guía de respuesta a incidentes para su organización. Asegúrese de que haya planes de respuesta a incidentes escritos que definan todos los roles del personal, así como las fases de administración y gestión de los incidentes, desde la detección hasta la revisión posterior a la incidencia.

- [Configuración de las automatizaciones de flujos de trabajo en Azure Security Center](../security-center/security-center-planning-and-operations-guide.md)

- [Guía para crear su propio proceso de respuesta a incidentes de seguridad](https://msrc-blog.microsoft.com/2019/07/01/inside-the-msrc-building-your-own-security-incident-response-process/)

- [Anatomía de un incidente del Centro de respuestas de seguridad de Microsoft](https://msrc-blog.microsoft.com/2019/07/01/inside-the-msrc-building-your-own-security-incident-response-process/)

- [El cliente también puede usar la guía de control de incidentes de seguridad de equipos de NIST como referencia para la creación de su propio plan de respuesta a incidentes](https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-61r2.pdf)

**Supervisión de Azure Security Center** : No aplicable

**Responsabilidad** : Customer

### <a name="102-create-an-incident-scoring-and-prioritization-procedure"></a>10.2: Creación de un procedimiento de priorización y puntuación de incidentes

**Instrucciones** : Security Center asigna una gravedad a cada alerta para ayudarle a priorizar aquellas que se deben investigar en primer lugar. La gravedad se basa en la confianza que tiene Security Center en la búsqueda o en el análisis utilizados para emitir la alerta, así como en el nivel de confianza de que ha habido un intento malintencionado detrás de la actividad que ha provocado la alerta.

Además, marque claramente las suscripciones (por ejemplo, producción, no producción) y cree un sistema de nomenclatura para identificar y clasificar claramente los recursos de Azure.

**Supervisión de Azure Security Center** : Sí

**Responsabilidad** : Customer

### <a name="103-test-security-response-procedures"></a>10.3: Prueba de los procedimientos de respuesta de seguridad

**Instrucciones** : Realice ejercicios para probar las capacidades de respuesta a los incidentes de los sistemas con regularidad. Identifique puntos débiles y brechas y revise el plan según sea necesario.

- [Consulte la publicación de NIST: Guía para probar, entrenar y ejecutar programas para planes y funcionalidades de TI](https://nvlpubs.nist.gov/nistpubs/Legacy/SP/nistspecialpublication800-84.pdf)

**Supervisión de Azure Security Center** : No aplicable

**Responsabilidad** : Customer

### <a name="104-provide-security-incident-contact-details-and-configure-alert-notifications-for-security-incidents"></a>10.4: Provisión de detalles de contacto de incidentes de seguridad y configuración de notificaciones de alerta para incidentes de seguridad

**Guía** : La información de contacto del incidente de seguridad la utilizará Microsoft para ponerse en contacto con usted si Microsoft Security Response Center (MSRC) detecta que un tercero no autorizado o ilegal ha accedido a los datos del cliente.  Revise los incidentes después del hecho para asegurarse de que se resuelven los problemas.

- [Establecimiento del contacto de seguridad de Azure Security Center](../security-center/security-center-provide-security-contact-details.md)

**Supervisión de Azure Security Center** : Sí

**Responsabilidad** : Customer

### <a name="105-incorporate-security-alerts-into-your-incident-response-system"></a>10.5: Incorporación de alertas de seguridad en el sistema de respuesta a incidentes

**Instrucciones** : Exporte sus alertas y recomendaciones de Azure Security Center mediante la característica de exportación continua. La exportación continua le permite exportar alertas y recomendaciones de forma manual o continua. Puede usar el conector de datos de Azure Security Center para transmitir las alertas a Sentinel.

- [Configuración de la exportación continua](../security-center/continuous-export.md)

- [Transmisión de alertas a Azure Sentinel](connect-azure-security-center.md)

**Supervisión de Azure Security Center** : No aplicable

**Responsabilidad** : Customer

### <a name="106-automate-the-response-to-security-alerts"></a>10.6: Automatización de la respuesta a las alertas de seguridad

**Instrucciones** : use la característica de automatización del flujo de trabajo de Azure Security Center para desencadenar automáticamente respuestas mediante "Logic Apps" en las alertas y recomendaciones de seguridad.

- [Configuración de la automatización de flujo de trabajo y Logic Apps](../security-center/workflow-automation.md)

**Supervisión de Azure Security Center** : No aplicable

**Responsabilidad** : Customer

## <a name="penetration-tests-and-red-team-exercises"></a>Pruebas de penetración y ejercicios del equipo rojo

*Para obtener más información, consulte [Azure Security Benchmark: Pruebas de penetración y ejercicios del equipo rojo](../security/benchmarks/security-control-penetration-tests-red-team-exercises.md).*

### <a name="111-conduct-regular-penetration-testing-of-your-azure-resources-and-ensure-remediation-of-all-critical-security-findings"></a>11.1: Realice pruebas de penetración periódicas de los recursos de Azure y asegúrese de corregir todos los resultados de seguridad críticos

**Guía** : Siga las reglas de compromiso de la prueba de penetración de Microsoft Cloud para asegurarse de que las pruebas de penetración no infrinjan las directivas de Microsoft. Use la estrategia de Microsoft y la ejecución de las pruebas de penetración del equipo rojo y sitios activos en la infraestructura de nube, los servicios y las aplicaciones administradas por Microsoft. 

- [Reglas de interacción de las pruebas de penetración](https://www.microsoft.com/msrc/pentest-rules-of-engagement?rtc=1) 

- [Equipo rojo de Microsoft Cloud](https://gallery.technet.microsoft.com/Cloud-Red-Teaming-b837392e)

**Supervisión de Azure Security Center** : No aplicable

**Responsabilidad** : Compartido

## <a name="next-steps"></a>Pasos siguientes

- Consulte la [prueba comparativa de seguridad de Azure](/azure/security/benchmarks/overview).
- Obtenga más información sobre las [líneas de base de seguridad de Azure](/azure/security/benchmarks/security-baselines-overview).

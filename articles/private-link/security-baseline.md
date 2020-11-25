---
title: Línea base de seguridad de Azure para Azure Private Link
description: La línea base de seguridad de Azure Private Link proporciona una guía de procedimientos y recursos para implementar las recomendaciones de seguridad especificadas en Azure Security Benchmark.
author: msmbaldwin
ms.service: private-link
ms.topic: conceptual
ms.date: 09/08/2020
ms.author: mbaldwin
ms.custom: subject-security-benchmark
ms.openlocfilehash: 636bc3c64dcd15ead36a9a4e969e802d49895dca
ms.sourcegitcommit: c95e2d89a5a3cf5e2983ffcc206f056a7992df7d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/24/2020
ms.locfileid: "95522250"
---
# <a name="azure-security-baseline-for-azure-private-link"></a>Línea base de seguridad de Azure para Azure Private Link

Esta línea base de seguridad aplica la guía de [Azure Security Benchmark](../security/benchmarks/overview.md) a Azure Private Link. Azure Security Benchmark proporciona recomendaciones sobre cómo puede proteger sus soluciones de nube en Azure. El contenido se agrupa por los **controles de seguridad** definidos por Azure Security Benchmark y las directrices aplicables a Azure Private Link. Se han excluido los **controles** no aplicables a Azure Private Link. Para ver cómo Azure Private Link se asigna por completo a Azure Security Benchmark, consulte el [archivo de asignación de base de referencia de seguridad de Azure Virtual Network completo](https://github.com/MicrosoftDocs/SecurityBenchmarks/tree/master/Azure%20Offer%20Security%20Baselines).

## <a name="network-security"></a>Seguridad de las redes

*Para obtener más información, consulte [Azure Security Benchmark: Seguridad de redes](../security/benchmarks/security-control-network-security.md).*

### <a name="111-use-automated-tools-to-monitor-network-resource-configurations-and-detect-changes"></a>1.11: Use herramientas automatizadas para supervisar las configuraciones de recursos de red y detectar cambios

**Guía**: Utilice Azure Activity Log para supervisar las configuraciones de recursos y detectar cambios en los recursos de red relacionados con Azure Private Link. 

Cree alertas en Azure Monitor que se desencadenarán cuando se produzcan cambios en los recursos críticos.

- [Visualización y recuperación de eventos del registro de actividad de Azure](../azure-monitor/platform/activity-log.md#view-the-activity-log)

- [Creación de alertas en Azure Monitor](../azure-monitor/platform/alerts-activity-log.md)

**Supervisión de Azure Security Center**: no disponible actualmente

**Responsabilidad**: Customer

## <a name="logging-and-monitoring"></a>Registro y supervisión

*Para obtener más información, consulte [Azure Security Benchmark: registro y supervisión](../security/benchmarks/security-control-logging-monitoring.md).*

### <a name="22-configure-central-security-log-management"></a>2.2: Configuración de la administración central de registros de seguridad

**Guía**: Ingesta de registros a través de Azure Monitor para agregar datos de seguridad generados por recursos de red, como puntos de conexión de Private Link, redes virtuales y grupos de seguridad de red. 

En Azure Monitor, use áreas de trabajo de Log Analytics para realizar consultas y análisis, y utilice cuentas de Azure Storage para el almacenamiento de archivos a largo plazo.

Además, habilite los datos integrados en Azure Sentinel o en una SIEM de terceros, en función de los requisitos empresariales de la organización.

- [Incorporación de Azure Sentinel](../sentinel/quickstart-onboard.md)

- [Recopilación de registros y métricas de plataforma con Azure Monitor](../azure-monitor/platform/diagnostic-settings.md)

- [Registro y supervisión para Private Link](private-link-overview.md#logging-and-monitoring)

- [Registros de diagnóstico de un grupo de seguridad de red](../virtual-network/virtual-network-nsg-manage-log.md)

- [Introducción a Azure Monitor e integración con herramienta SIEM de terceros](https://azure.microsoft.com/blog/use-azure-monitor-to-integrate-with-siem-tools/)

**Supervisión de Azure Security Center**: Sí

**Responsabilidad**: Customer

### <a name="23-enable-audit-logging-for-azure-resources"></a>2.3: Habilitación del registro de auditoría para recursos de Azure

**Guía**: Habilite los registros de actividad de Azure Monitor, que registran operaciones realizadas en recursos de Private Link (por ejemplo, quién inició una operación, cuándo se produjo, cuál es el estado de esa operación y otra información de auditoría útil). 

- [Recopilación de registros y métricas de plataforma con Azure Monitor](../azure-monitor/platform/diagnostic-settings.md)

- [Visualización y recuperación de eventos del registro de actividad de Azure](../azure-monitor/platform/activity-log.md#view-the-activity-log)

- [Registro y supervisión para Private Link](private-link-overview.md#logging-and-monitoring)

**Supervisión de Azure Security Center**: Sí

**Responsabilidad**: Customer

### <a name="25-configure-security-log-storage-retention"></a>2.5: Configuración de la retención del almacenamiento de registros de seguridad

**Instrucciones**: Para registros relacionados con Private Link, establezca el período de retención del área de trabajo de Log Analytics de acuerdo con la normativa de cumplimiento de su organización dentro de Azure Monitor. Utilice las cuentas de Azure Storage para el almacenamiento de registros a largo plazo o con fines de archivo.

- [Cambio del período de retención de datos en Log Analytics](../azure-monitor/platform/manage-cost-storage.md#change-the-data-retention-period)

- [Configuración de la directiva de retención para los registros de la cuenta de Azure Storage](../storage/common/storage-monitor-storage-account.md#configure-logging)

**Supervisión de Azure Security Center**: no disponible actualmente

**Responsabilidad**: Customer

### <a name="26-monitor-and-review-logs"></a>2.6: Supervisión y revisión de registros

**Guía**: Analice y supervise los registros en busca de comportamientos anómalos y revise los resultados con regularidad. Use el área de trabajo de Log Analytics de Azure Monitor para revisar los registros y realizar consultas en los datos del registro.

Otra opción es habilitar e incorporar datos en Azure Sentinel o en una herramienta de Administración de eventos e información de seguridad (SIEM) de terceros.

- [Incorporación de Azure Sentinel](../sentinel/quickstart-onboard.md)

- [Descripción del área de trabajo de Log Analytics](../azure-monitor/log-query/log-analytics-tutorial.md)

- [Procedimiento para realizar consultas personalizadas en Azure Monitor](../azure-monitor/log-query/get-started-queries.md)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="27-enable-alerts-for-anomalous-activities"></a>2.7: Habilitación de alertas para actividades anómalas

**Guía**: Use Security Center configurado con el área de trabajo de Log Analytics para la supervisión y alertas sobre actividades anómalas que se encuentran en los registros y eventos de seguridad.

Habilite los datos integrados en Azure Sentinel o en una SIEM de terceros, en función de los requisitos empresariales de la organización.

- [Incorporación de Azure Sentinel](../sentinel/quickstart-onboard.md)

- [Administración de alertas de seguridad en Azure Security Center](../security-center/security-center-managing-and-responding-alerts.md)

- [Alertas sobre datos de registro de Log Analytics](../azure-monitor/learn/tutorial-response.md)

**Supervisión de Azure Security Center**: Sí

**Responsabilidad**: Customer

## <a name="identity-and-access-control"></a>Control de identidades y acceso

*Para obtener más información, consulte [Azure Security Benchmark: Identidad y control de acceso](../security/benchmarks/security-control-identity-access-control.md).*

### <a name="31-maintain-an-inventory-of-administrative-accounts"></a>3.1: Mantenga un inventario de cuentas administrativas

**Guía**: Use los roles de administrador integrados de Azure Active Directory (Azure AD) que se pueden asignar explícitamente y se pueden consultar. Ejecute el módulo de PowerShell de Azure AD para realizar consultas ad hoc a fin de detectar cuentas que son miembros de grupos administrativos.

- [Obtención de un rol de directorio en Azure AD con PowerShell](/powershell/module/azuread/get-azureaddirectoryrole)

- [Obtención de los miembros de un rol de directorio en Azure AD con PowerShell](/powershell/module/azuread/get-azureaddirectoryrolemember)

**Supervisión de Azure Security Center**: Sí

**Responsabilidad**: Customer

### <a name="33-use-dedicated-administrative-accounts"></a>3.3: Use cuentas administrativas dedicadas

**Guía**: Cree procedimientos operativos estándar en torno al uso de cuentas administrativas dedicadas. Use la identidad de Security Center y las características de administración de acceso para supervisar el número de cuentas administrativas.

También puede habilitar el acceso Just-in-Time/Just-Enough usando roles de Azure Active Directory (Azure AD) Privileged Identity Management con privilegios para los servicios de Microsoft y Azure Resource Manager.

- [Más información acerca de Privileged Identity Management](../active-directory/privileged-identity-management/index.yml)

**Supervisión de Azure Security Center**: Sí

**Responsabilidad**: Customer

### <a name="34-use-azure-active-directory-single-sign-on-sso"></a>3.4: Uso del inicio de sesión único (SSO) de Azure Active Directory

**Guía**: Siempre que sea posible, use el inicio de sesión único con Azure Active Directory en lugar de configurar credenciales independientes individuales por servicio.

- [Inicio de sesión único en aplicaciones en Azure Active Directory](../active-directory/manage-apps/what-is-single-sign-on.md)

- [Supervisión de la identidad y el acceso en Azure Security Center](../security-center/security-center-identity-access.md)

**Supervisión de Azure Security Center**: no disponible actualmente

**Responsabilidad**: Customer

### <a name="35-use-multi-factor-authentication-for-all-azure-active-directory-based-access"></a>3.5: Uso de la autenticación multifactor para todo el acceso basado en Azure Active Directory

**Guía**: Habilite la autenticación multifactor (MFA) de Azure Active Directory (Azure AD) y siga las recomendaciones de administración de identidades y acceso de Security Center.

- [Procedimiento para habilitar la MFA en Azure](../active-directory/authentication/howto-mfa-getstarted.md)

- [Supervisión de la identidad y el acceso en Azure Security Center](../security-center/security-center-identity-access.md)

**Supervisión de Azure Security Center**: Sí

**Responsabilidad**: Customer

### <a name="36-use-secure-azure-managed-workstations-for-administrative-tasks"></a>3.6: Uso de estaciones de trabajo seguras y administradas por Azure para realizar tareas administrativas

**Guía**: Utilice una estación de acceso con privilegios (PAW) con Multi-Factor Authentication configurada para iniciar sesión en recursos de red de Azure y configurarlos.

- [Más información sobre las estaciones de trabajo con privilegios de acceso](/windows-server/identity/securing-privileged-access/privileged-access-workstations)

- [Procedimiento para habilitar la MFA en Azure](../active-directory/authentication/howto-mfa-getstarted.md)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="37-log-and-alert-on-suspicious-activities-from-administrative-accounts"></a>3.7: Registro y alerta de actividades sospechosas desde cuentas administrativas

**Instrucciones**: Use la característica de detecciones de riesgo de Azure Active Directory (Azure AD) para ver alertas e informes sobre el comportamiento de los usuarios de riesgo. Ingiera alertas de detección de riesgo de Security Center a Azure Monitor y configure alertas o notificaciones personalizadas mediante grupos de acciones.

- [Descripción de las detecciones de riesgo de Azure Security Center (actividad sospechosa)](../active-directory/identity-protection/overview-identity-protection.md)

- [Integración de los registros de actividad de Azure en Azure Monitor](../active-directory/reports-monitoring/howto-integrate-activity-logs-with-log-analytics.md)

- [Configuración de grupos de acciones para alertas y notificaciones personalizadas](../azure-monitor/platform/action-groups.md)

**Supervisión de Azure Security Center**: Sí

**Responsabilidad**: Customer

### <a name="38-manage-azure-resources-from-only-approved-locations"></a>3.8: Administre los recursos de Azure solo desde ubicaciones aprobadas

**Guía**: Use ubicaciones con nombre de acceso condicional para permitir el acceso solo desde agrupaciones lógicas específicas de intervalos de direcciones IP o países o regiones.

- [Configuración de ubicaciones con nombre en Azure](../active-directory/reports-monitoring/quickstart-configure-named-locations.md)

**Supervisión de Azure Security Center**: no disponible actualmente

**Responsabilidad**: Customer

### <a name="39-use-azure-active-directory"></a>3.9: Uso de Azure Active Directory

**Instrucciones**: Use Azure Active Directory (AD) como sistema central de autenticación y autorización. Azure AD protege los datos mediante el cifrado seguro para datos en reposo y en tránsito, y también sales, hashes y almacena de forma segura las credenciales de usuario.  

- [Procedimiento para crear y configurar una instancia de Azure AD](../active-directory-domain-services/tutorial-create-instance.md)

**Supervisión de Azure Security Center**: Sí

**Responsabilidad**: Customer

### <a name="310-regularly-review-and-reconcile-user-access"></a>3.10: Revise y concilie regularmente el acceso de los usuarios

**Guía**: Azure Active Directory proporciona registros para ayudar a descubrir cuentas obsoletas. Además, use las revisiones de acceso de identidad de Azure para administrar de forma eficiente las pertenencias a grupos, el acceso a las aplicaciones empresariales y las asignaciones de roles. El acceso de los usuarios se puede revisar de forma periódica para asegurarse de que solo las personas adecuadas tengan acceso continuado.

- [Descripción de los informes de Azure AD](../active-directory/reports-monitoring/index.yml)

- [Procedimiento para usar las revisiones de acceso de identidad de Azure](../active-directory/governance/access-reviews-overview.md)

**Supervisión de Azure Security Center**: Sí

**Responsabilidad**: Customer

### <a name="311-monitor-attempts-to-access-deactivated-credentials"></a>3.11: Supervisión de los intentos de acceso a credenciales desactivadas

**Guía**: Use los orígenes de registro de actividad de inicio de sesión, auditoría y evento de riesgo de Azure Active Directory (Azure AD) para integrar en cualquier herramienta de SIEM o supervisión.

Para simplificar este proceso, cree una configuración de diagnóstico para las cuentas de usuario de Azure AD y envíe los registros de auditoría y los registros de inicio de sesión a un área de trabajo de Log Analytics. Configure las alertas deseadas en el área de trabajo de Log Analytics.

- [Integración de los registros de actividad de Azure en Azure Monitor](../active-directory/reports-monitoring/howto-integrate-activity-logs-with-log-analytics.md)

**Supervisión de Azure Security Center**: no disponible actualmente

**Responsabilidad**: Customer

### <a name="312-alert-on-account-sign-in-behavior-deviation"></a>3.12: Alerta sobre las desviaciones de comportamiento en los inicios de sesión de las cuentas

**Guía**: Use las características de protección de riesgos e identidad de Azure Active Directory (Azure AD) para configurar las respuestas automatizadas a las acciones sospechosas detectadas relacionadas con las identidades de los usuarios para los recursos de red. 

Ingiera datos en Azure Sentinel para investigarlos más.

- [Visualización de los inicios de sesión de riesgo de Azure AD](../active-directory/identity-protection/overview-identity-protection.md)

- [Configuración y habilitación de las directivas de riesgo de protección de identidad](../active-directory/identity-protection/howto-identity-protection-configure-risk-policies.md)

- [Incorporación de Azure Sentinel](../sentinel/quickstart-onboard.md)

**Supervisión de Azure Security Center**: no disponible actualmente

**Responsabilidad**: Customer

## <a name="data-protection"></a>Protección de los datos

*Para obtener más información, consulte [Azure Security Benchmark: Protección de datos](../security/benchmarks/security-control-data-protection.md).*

### <a name="42-isolate-systems-storing-or-processing-sensitive-information"></a>4.2: Aislamiento de los sistemas que almacenan o procesan información confidencial

**Guía**: Implemente el aislamiento mediante suscripciones independientes y grupos de administración para dominios de seguridad individuales, como el tipo de entorno y el nivel de confidencialidad de los datos. 

Restrinja el nivel de acceso a los recursos de Azure que con sus aplicaciones y entornos empresariales en función de los requisitos empresariales. 

Controle el acceso a los recursos de Azure a través del control de acceso basado en rol de Azure (RBAC de Azure).

- [Creación de suscripciones adicionales de Azure](../cost-management-billing/manage/create-subscription.md)

- [Creación de grupos de administración](../governance/management-groups/create-management-group-portal.md)

- [Creación y uso de etiquetas](../azure-resource-manager/management/tag-resources.md)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="44-encrypt-all-sensitive-information-in-transit"></a>4.4: Cifrado de toda la información confidencial en tránsito

**Instrucciones**: Cifre toda la información confidencial en tránsito. Asegúrese de que los clientes que se conectan a los recursos de Azure en redes virtuales pueden negociar TLS 1.2 o superior. Private Link no interfiere con los protocolos subyacentes. Use los procedimientos recomendados para otras ofertas usadas por los clientes.

Siga las recomendaciones de Security Center para el cifrado en reposo y el cifrado en tránsito, si procede.

- [Descripción del cifrado en tránsito con Azure](../security/fundamentals/encryption-overview.md#encryption-of-data-in-transit)

**Supervisión de Azure Security Center**: Sí

**Responsabilidad**: Compartido

### <a name="46-use-azure-rbac-to-control-access-to-resources"></a>4.6: Uso de RBAC de Azure para controlar el acceso a los recursos

**Guía**: Use el control de acceso basado en rol de Azure (Azure RBAC) para controlar el acceso a los datos y recursos; si no, use métodos de control de acceso específicos del servicio.

- [Familiarícese con una breve descripción y el identificador único de los roles integrados de Azure aquí](../role-based-access-control/built-in-roles.md)

Invoque el comando de PowerShell "Get-AzRoleDefinition" o "az role definition list" para recuperar una lista de roles de su entorno.

Revise las opciones para controlar la exposición de su servicio a través de la configuración "Visibilidad". en Private Link. Esta configuración de visibilidad decide si un consumidor puede conectarse a su servicio. 

Haga que el servicio sea privado para su consumo desde otras redes virtuales (solo permisos de RBAC de Azure). Restrinja la exposición a un conjunto limitado de suscripciones de confianza o haga que sea pública para que todas las suscripciones de Azure puedan solicitar conexiones en el servicio Private Link.

- [Configuración de Azure RBAC](../role-based-access-control/role-assignments-portal.md)

- [Propiedades del servicio Private Link](private-link-service-overview.md)

**Supervisión de Azure Security Center**: no disponible actualmente

**Responsabilidad**: Customer

### <a name="49-log-and-alert-on-changes-to-critical-azure-resources"></a>4.9: Registro y alerta de cambios en los recursos críticos de Azure

**Instrucciones**: Use alertas del registro de actividad de Azure Monitor para crear alertas para cuando se produzcan cambios en los recursos críticos de Azure, como los puntos de conexión y los servicios de Private Link. 

- [Registros de diagnóstico de un grupo de seguridad de red](private-link-overview.md#logging-and-monitoring)

- [Creación de alertas para los eventos del registro de actividad de Azure](../azure-monitor/platform/alerts-activity-log.md)

**Supervisión de Azure Security Center**: no disponible actualmente

**Responsabilidad**: Customer

## <a name="inventory-and-asset-management"></a>Inventario y administración de recursos

*Para obtener más información, consulte [Azure Security Benchmark: Administración de recursos y del inventario](../security/benchmarks/security-control-inventory-asset-management.md).*

### <a name="61-use-automated-asset-discovery-solution"></a>6.1: Uso de la solución de detección de recursos automatizada

**Guía**: Use Azure Resource Graph para consultar y detectar todos los recursos de red, como los puntos de conexión y los servicios de Private Link dentro de las suscripciones. 

Asegúrese de que tiene los permisos adecuados (lectura) en el inquilino y de que puede enumerar todas las suscripciones de Azure, así como los recursos de las suscripciones.

- [Creación de consultas con Azure Graph](../governance/resource-graph/first-query-portal.md)

- [Visualización de las suscripciones de Azure](/powershell/module/az.accounts/get-azsubscription)

- [Descripción de Azure RBAC](../role-based-access-control/overview.md)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="62-maintain-asset-metadata"></a>6.2: Mantenimiento de metadatos de recursos

**Guía**: Aplique etiquetas a los recursos de Azure mediante metadatos para organizarlos de forma lógica en una taxonomía.

- [Creación y uso de etiquetas](../azure-resource-manager/management/tag-resources.md)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="63-delete-unauthorized-azure-resources"></a>6.3: Eliminación de recursos de Azure no autorizados

**Guía**: Use el etiquetado, los grupos de administración y las suscripciones independientes, si procede, para organizar y realizar un seguimiento de los recursos de Private Link y recursos relacionados. 

Concilie el inventario periódicamente y asegúrese de que los recursos no autorizados se eliminan de la suscripción de manera oportuna.

- [Creación de suscripciones adicionales de Azure](../cost-management-billing/manage/create-subscription.md)

- [Creación de grupos de administración](../governance/management-groups/create-management-group-portal.md)

- [Creación y uso de etiquetas](../azure-resource-manager/management/tag-resources.md)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="64-define-and-maintain-inventory-of-approved-azure-resources"></a>6.4: Definición y mantenimiento de un inventario de los recursos de Azure aprobados

**Instrucciones**: Cree un inventario de los recursos de Azure aprobados y el software para los recursos de proceso basados en las necesidades de la organización.

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="65-monitor-for-unapproved-azure-resources"></a>6.5: Supervisión de recursos de Azure no aprobados

**Guía**: Use Azure Policy para aplicar restricciones en el tipo de recursos que se pueden crear en las suscripciones del cliente con las siguientes definiciones de directiva integradas:

- Tipos de recursos no permitidos

- Tipos de recursos permitidos

Además, use Azure Resource Graph para consultar o detectar recursos dentro de sus suscripciones. Esto puede ayudar en entornos de alta seguridad, como aquellos con cuentas de Storage.

- [Configuración y administración de Azure Policy](../governance/policy/tutorials/create-and-manage.md)

- [Creación de consultas con Azure Graph](../governance/resource-graph/first-query-portal.md)

- [Complementos de ejemplo de Azure Policy para Private Link](../governance/policy/samples/built-in-policies.md)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="67-remove-unapproved-azure-resources"></a>6.7: Eliminación de recursos de Azure no aprobados

**Guía**: El cliente puede impedir la creación o el uso de recursos con Azure Policy según las directivas de la empresa del cliente. Puede implementar su propio proceso para quitar recursos no autorizados.

- [Configuración y administración de Azure Policy](../governance/policy/tutorials/create-and-manage.md)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="69-use-only-approved-azure-services"></a>6.9: Uso exclusivo de servicios de Azure aprobados

**Guía**: Use Azure Policy para aplicar restricciones en el tipo de recursos que se pueden crear en las suscripciones del cliente con las siguientes definiciones de directiva integradas:

- Tipos de recursos no permitidos

- Tipos de recursos permitidos

- [Configuración y administración de Azure Policy](../governance/policy/tutorials/create-and-manage.md)

- [Denegación de un tipo de recurso específico con Azure Policy](../governance/policy/samples/index.md)

- [Complementos de ejemplo de Azure Policy para Private Link](../governance/policy/samples/built-in-policies.md)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="611-limit-users-ability-to-interact-with-azure-resource-manager"></a>6.11: Limitación de la capacidad de los usuarios para interactuar con Azure Resource Manager

**Instrucciones**: use el acceso condicional de Azure para limitar la capacidad de los usuarios de interactuar con Azure Resource Manager mediante la configuración de la opción "Bloquear acceso" en la aplicación "Administración de Microsoft Azure".

- [Configuración del acceso condicional para bloquear el acceso a Azure Resource Manager](../role-based-access-control/conditional-access-azure-management.md)

**Supervisión de Azure Security Center**: no disponible actualmente

**Responsabilidad**: Customer

## <a name="secure-configuration"></a>Configuración segura

*Para obtener más información, consulte [Azure Security Benchmark: Configuración segura](../security/benchmarks/security-control-secure-configuration.md).*

### <a name="71-establish-secure-configurations-for-all-azure-resources"></a>7.1: Establezca configuraciones seguras para todos los recursos de Azure

**Guía**: Use alias de Azure Policy para crear directivas personalizadas con el fin de auditar o aplicar la configuración de los recursos de red de Azure junto con las definiciones de Azure Policy integradas.

Azure Resource Manager tiene la capacidad de exportar la plantilla en notación de objetos JavaScript (JSON). Este artefacto debe revisarse para asegurarse de que las configuraciones cumplan o superen los requisitos de seguridad de su organización.

Implemente las recomendaciones de Security Center como un línea base de configuración segura para los recursos de Azure.

- [Visualización de los alias de Azure Policy disponibles](/powershell/module/az.resources/get-azpolicyalias)

- [Tutorial: Creación y administración de directivas para aplicar el cumplimiento](../governance/policy/tutorials/create-and-manage.md)

- [Complementos de ejemplo de Azure Policy para Private Link](../governance/policy/samples/built-in-policies.md)

- [Exportación de uno y varios recursos a una plantilla en Azure Portal](../azure-resource-manager/templates/export-template-portal.md)

- [Guía de referencia sobre las recomendaciones de seguridad](../security-center/recommendations-reference.md)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="73-maintain-secure-azure-resource-configurations"></a>7.3: Mantenga configuraciones de recursos de Azure seguras

**Instrucciones**: Use las plantillas de Azure Resource Manager y Azure Policy para configurar de forma segura los recursos de Azure asociados a Private Link y recursos relacionados.  

Las plantillas de Azure Resource Manager son archivos basados en la notación de objetos JavaScript (JSON) que se usan para implementar los recursos de Azure; cualquier plantilla personalizada deberá almacenarse y mantenerse de forma segura en un repositorio de código. 

Use la directiva de Azure [denegar] e [implementar si no existe] para aplicar la configuración segura en los recursos de Azure.

- [Información sobre la creación de plantillas de Azure Resource Manager](../azure-resource-manager/templates/quickstart-create-templates-use-the-portal.md)

- [Configuración y administración de Azure Policy](../governance/policy/tutorials/create-and-manage.md)

- [Descripción de los efectos de Azure Policy](../governance/policy/concepts/effects.md)

- [Plantillas de ejemplo de Azure Resource Manager para puntos de conexión privados](/azure/templates/microsoft.network/2019-11-01/privateendpoints)

- [Complementos de ejemplo de Azure Policy para Private Link](../governance/policy/samples/built-in-policies.md)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="75-securely-store-configuration-of-azure-resources"></a>7.5: Almacene de forma segura la configuración de los recursos de Azure

**Guía**: Use Azure DevOps para almacenar y administrar de forma segura el código, como directivas de Azure personalizadas, plantillas de Azure Resource Manager y scripts de Desired State Configuration. 

Conceda o deniegue permisos a usuarios específicos, grupos de seguridad integrados o grupos definidos en Azure Active Directory (Azure AD) si se integra con Azure DevOps para su acceso, o en Active Directory si se integra con Team Foundation Server.

- [Almacenamiento de código en Azure DevOps](/azure/devops/repos/git/gitworkflow)

- [Acerca de los permisos y los grupos en Azure DevOps](/azure/devops/organizations/security/about-permissions)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="77-deploy-configuration-management-tools-for-azure-resources"></a>7.7: Implementación de herramientas de administración de configuración para recursos de Azure

**Guía**: defina e implemente configuraciones de seguridad estándar para los recursos de Azure con Azure Policy. 

Use alias de Azure Policy para crear directivas personalizadas con el fin de auditar o aplicar la configuración de red de los recursos de Azure. 

Use también las definiciones de directiva integradas relacionadas con cualquier recurso específico administrado en sus suscripciones.

- [Configuración y administración de Azure Policy](../governance/policy/tutorials/create-and-manage.md)

- [Uso de alias](../governance/policy/concepts/definition-structure.md#aliases)

- [Complementos de ejemplo de Azure Policy para Private Link](../governance/policy/samples/built-in-policies.md)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="79-implement-automated-configuration-monitoring-for-azure-resources"></a>7.9: Implementación de la supervisión de la configuración automatizada para los recursos de Azure

**Instrucciones**: Use Azure Policy para auditar las configuraciones de recursos de Azure. Por ejemplo, Azure Policy se puede usar para detectar recursos, que no están configurados con un punto de conexión privado.

- [Configuración y administración de Azure Policy](../governance/policy/tutorials/create-and-manage.md)

- [Complementos de ejemplo de Azure Policy para Private Link](../governance/policy/samples/built-in-policies.md)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

## <a name="data-recovery"></a>Recuperación de datos

*Para más información, consulte [Azure Security Benchmark: recuperación de datos](../security/benchmarks/security-control-data-recovery.md).*

### <a name="91-ensure-regular-automated-back-ups"></a>9.1: Garantía de copias de seguridad automáticas periódicas

**Guía**: Use Azure Resource Manager para implementar servicios de Private Link, puntos de conexión y los recursos relacionados. Azure Resource Manager proporciona la capacidad de exportar plantillas que se pueden usar como copias de seguridad para restaurar los puntos de conexión de Private Link y los recursos relacionados. 

Use Azure Automation para llamar a la API de exportación de plantillas de Azure Resource Manager de forma periódica.

- [Introducción sobre Azure Resource Manager](../azure-resource-manager/management/overview.md)

- [Plantillas de ejemplo de Azure Resource Manager para puntos de conexión privados](/azure/templates/microsoft.network/2019-11-01/privateendpoints)

- [Exportación de uno y varios recursos a una plantilla en Azure Portal](../azure-resource-manager/templates/export-template-portal.md)

- [Grupos de recursos: Exportar plantilla](/rest/api/resources/resourcegroups/exporttemplate)

- [Introducción a Azure Automation](../automation/automation-intro.md)

**Supervisión de Azure Security Center**: no disponible actualmente

**Responsabilidad**: Customer

### <a name="92-perform-complete-system-backups-and-backup-any-customer-managed-keys"></a>9.2: Copias de seguridad completas del sistema y copia de seguridad de las claves administradas por el cliente

**Guía**: Use Azure Resource Manager para implementar servicios de Private Link, puntos de conexión y los recursos relacionados. Azure Resource Manager proporciona la capacidad de exportar plantillas, que se pueden usar como copias de seguridad para restaurar los puntos de conexión de Private Link y los recursos relacionados.  

Use Azure Automation para llamar a la API de exportación de plantillas de Azure Resource Manager de forma periódica.  

Realice una copia de seguridad de las claves administradas por el cliente en Azure Key Vault.

- [Introducción sobre Azure Resource Manager](../azure-resource-manager/management/overview.md)

- [Plantillas de ejemplo de Azure Resource Manager para puntos de conexión privados](/azure/templates/microsoft.network/2019-11-01/privateendpoints)

- [Exportación de uno y varios recursos a una plantilla en Azure Portal](../azure-resource-manager/templates/export-template-portal.md)

- [Grupos de recursos: Exportar plantilla](/rest/api/resources/resourcegroups/exporttemplate)

- [Introducción a Azure Automation](../automation/automation-intro.md)

- [Creación de una copia de seguridad de las claves del almacén de claves en Azure](/powershell/module/azurerm.keyvault/backup-azurekeyvaultkey)

**Supervisión de Azure Security Center**: no disponible actualmente

**Responsabilidad**: Customer

### <a name="93-validate-all-backups-including-customer-managed-keys"></a>9.3: Validación de todas las copias de seguridad, incluidas las claves administradas por el cliente

**Instrucciones**: Valide la capacidad de realizar periódicamente la implementación de plantillas de Azure Resource Manager de forma habitual en una suscripción aislada según los requisitos de su empresa. 

Además, valide las restauraciones de la copia de seguridad de las claves administradas por el cliente.

- [Implementación de recursos con Azure Portal y plantillas de Resource Manager](../azure-resource-manager/templates/deploy-portal.md)

- [Restauración de las claves del almacén de claves en Azure](/powershell/module/azurerm.keyvault/restore-azurekeyvaultkey)

**Supervisión de Azure Security Center**: no disponible actualmente

**Responsabilidad**: Customer

### <a name="94-ensure-protection-of-backups-and-customer-managed-keys"></a>9.4: Garantía de la protección de las copias de seguridad y las claves administradas por el cliente

**Guía**: Use Azure DevOps para almacenar y administrar de forma segura el código, como las plantillas de Azure Resource Manager. 

Conceda o deniegue permisos a usuarios específicos, grupos de seguridad integrados o grupos definidos en Azure Active Directory (Azure AD) si se integra con Azure DevOps para el acceso a estos recursos, o en Active Directory si se integra con Team Foundation Server.

- [Almacenamiento de código en Azure DevOps](/azure/devops/repos/git/gitworkflow)

- [Acerca de los permisos y los grupos en Azure DevOps](/azure/devops/organizations/security/about-permissions)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

## <a name="incident-response"></a>Respuesta a los incidentes

*Para obtener más información, consulte [Azure Security Benchmark: Respuesta a los incidentes](../security/benchmarks/security-control-incident-response.md).*

### <a name="101-create-an-incident-response-guide"></a>10.1: Creación de una guía de respuesta ante incidentes

**Instrucciones**: Cree una guía de respuesta a incidentes para su organización. 

Garantice planes de respuesta a incidentes escritos que definan todos los roles del personal y las fases de administración y gestión de los incidentes, desde la detección hasta la revisión posterior a la incidencia.

- [Guía para crear su propio proceso de respuesta a incidentes de seguridad](https://msrc-blog.microsoft.com/2019/07/01/inside-the-msrc-building-your-own-security-incident-response-process/)

- [Anatomía de un incidente del Centro de respuestas de seguridad de Microsoft](https://msrc-blog.microsoft.com/2019/06/27/inside-the-msrc-anatomy-of-a-ssirp-incident/)

- [El cliente también puede usar la guía de control de incidentes de seguridad de equipos de NIST como referencia para la creación de su propio plan de respuesta a incidentes](https://csrc.nist.gov/publications/detail/sp/800-61/rev-2/final)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="102-create-an-incident-scoring-and-prioritization-procedure"></a>10.2: Creación de un procedimiento de priorización y puntuación de incidentes

**Guía**: Security Center asigna una gravedad a cada alerta para ayudar a priorizar aquellas que se deben investigar en primer lugar. La gravedad se basa en la confianza que tiene Security Center en la búsqueda o en el análisis utilizados para emitir la alerta, así como en el nivel de confianza de que ha habido un intento malintencionado detrás de la actividad que ha provocado la alerta.

Marque claramente las suscripciones (por ejemplo, producción, no producción) con etiquetas y cree un sistema de nomenclatura para identificar y clasificar claramente los recursos de Azure, especialmente los que procesan datos confidenciales.  

El cliente es el responsable de asignar prioridades a la corrección de las alertas en función de la importancia de los recursos y el entorno de Azure donde se produjo el incidente.

- [Alertas de seguridad en el Centro de seguridad de Azure](../security-center/security-center-alerts-overview.md)

- [Uso de etiquetas para organizar los recursos de Azure](../azure-resource-manager/management/tag-resources.md)

**Supervisión de Azure Security Center**: Sí

**Responsabilidad**: Customer

### <a name="103-test-security-response-procedures"></a>10.3: Prueba de los procedimientos de respuesta de seguridad

**Guía**: Realice ejercicios para probar las funcionalidades de respuesta a los incidentes de los sistemas periódicamente para ayudar a proteger los recursos de Azure. Identifique puntos débiles y brechas y revise el plan según sea necesario.

- [Publicación de NIST: Guía para probar, entrenar y ejecutar programas para planes y funcionalidades de TI](https://csrc.nist.gov/publications/detail/sp/800-84/final)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="104-provide-security-incident-contact-details-and-configure-alert-notifications-for-security-incidents"></a>10.4: Provisión de detalles de contacto de incidentes de seguridad y configuración de notificaciones de alerta para incidentes de seguridad

**Instrucciones**: La información de contacto del incidente de seguridad la utilizará Microsoft para ponerse en contacto con usted si Microsoft Security Response Center (MSRC) detecta que un tercero no autorizado o ilegal ha accedido a los datos. 

Revise los incidentes, después de la aparición, para asegurarse de que los problemas se hayan resuelto.

- [Establecimiento del contacto de seguridad de Azure Security Center](../security-center/security-center-provide-security-contact-details.md)

**Supervisión de Azure Security Center**: Sí

**Responsabilidad**: Customer

### <a name="105-incorporate-security-alerts-into-your-incident-response-system"></a>10.5: Incorporación de alertas de seguridad en el sistema de respuesta a incidentes

**Guía**: Exporte las alertas y recomendaciones de Security Center y mediante la característica de exportación continua para ayudar a identificar riesgos en los recursos de Azure. La exportación continua le permite exportar alertas y recomendaciones de forma manual o continua. 

Además, use el conector de datos de Security Center para enviar las alertas a Azure Sentinel, según sea necesario en las operaciones empresariales.

- [Configuración de la exportación continua](../security-center/continuous-export.md)

- [Transmisión de alertas a Azure Sentinel](../sentinel/connect-azure-security-center.md)

**Supervisión de Azure Security Center**: no disponible actualmente

**Responsabilidad**: Customer

### <a name="106-automate-the-response-to-security-alerts"></a>10.6: Automatización de la respuesta a las alertas de seguridad

**Guía**: Use la característica Automatización de flujo de trabajo de Security Center para desencadenar automáticamente las respuestas con Logic Apps en las alertas y recomendaciones de seguridad para proteger los recursos de Azure.

- [Configuración de la automatización de flujo de trabajo y Logic Apps](../security-center/workflow-automation.md)

**Supervisión de Azure Security Center**: no disponible actualmente

**Responsabilidad**: Customer

## <a name="penetration-tests-and-red-team-exercises"></a>Pruebas de penetración y ejercicios del equipo rojo

*Para obtener más información, consulte [Azure Security Benchmark: Pruebas de penetración y ejercicios del equipo rojo](../security/benchmarks/security-control-penetration-tests-red-team-exercises.md).*

### <a name="111-conduct-regular-penetration-testing-of-your-azure-resources-and-ensure-remediation-of-all-critical-security-findings"></a>11.1: Realice pruebas de penetración periódicas de los recursos de Azure y asegúrese de corregir todos los resultados de seguridad críticos

**Guía**: Siga las reglas de involucración de Microsoft para asegurarse de que las pruebas de penetración no infrinjan las directivas de Microsoft. 

Use la estrategia de Microsoft y la ejecución de pruebas de penetración de sitios activos y ataques simulados en la infraestructura en la nube, los servicios y las aplicaciones administrados por Microsoft.

- [Reglas de interacción de las pruebas de penetración](https://www.microsoft.com/msrc/pentest-rules-of-engagement?rtc=1)

- [Equipo rojo de Microsoft Cloud](https://gallery.technet.microsoft.com/Cloud-Red-Teaming-b837392e)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Compartido

## <a name="next-steps"></a>Pasos siguientes

- Consulte la [prueba comparativa de seguridad de Azure](../security/benchmarks/overview.md).
- Obtenga más información sobre las [líneas de base de seguridad de Azure](../security/benchmarks/security-baselines-overview.md).
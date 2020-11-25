---
title: Base de referencia de seguridad de Azure para Service Bus
description: La base de referencia de seguridad de Service Bus proporciona instrucciones de procedimientos y recursos para implementar las recomendaciones de seguridad especificadas en Azure Security Benchmark.
author: msmbaldwin
ms.service: service-bus-messaging
ms.topic: conceptual
ms.date: 09/25/2020
ms.author: mbaldwin
ms.custom: subject-security-benchmark
ms.openlocfilehash: a6d4e7f4ca7288b36f6801b9ddb362b7e936862d
ms.sourcegitcommit: 0a9df8ec14ab332d939b49f7b72dea217c8b3e1e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/18/2020
ms.locfileid: "94843628"
---
# <a name="azure-security-baseline-for-service-bus"></a>Base de referencia de seguridad de Azure para Service Bus

La base de referencia de seguridad de Azure para Service Bus contiene recomendaciones que le ayudarán a mejorar la posición de seguridad de la implementación. La base de referencia de este servicio se extrae de la [versión 1.0 de Azure Security Benchmark](../security/benchmarks/overview-v1.md), que proporciona recomendaciones sobre cómo puede proteger las soluciones en la nube en Azure con nuestra guía de procedimientos recomendados. Para obtener más información, consulte [Introducción a las líneas de base de seguridad de Azure](../security/benchmarks/security-baselines-overview.md).

Para ver cómo Service Bus se asigna por completo a Azure Security Benchmark, consulte el [archivo de asignación de base de referencia de seguridad de Service Bus completo](https://github.com/MicrosoftDocs/SecurityBenchmarks/tree/master/Azure%20Offer%20Security%20Baselines).

## <a name="network-security"></a>Seguridad de las redes

*Para obtener más información, consulte [Azure Security Benchmark: Seguridad de redes](../security/benchmarks/security-control-network-security.md).*

### <a name="11-protect-azure-resources-within-virtual-networks"></a>1.1: Protección de los recursos de Azure dentro de las redes virtuales 

**Guía**: la integración de Service Bus con el servicio Azure Private Link permite acceder de forma privada y segura a funcionalidades de mensajería de cargas de trabajo tales como máquinas virtuales enlazadas a redes virtuales. Cree una conexión de punto de conexión privado a su espacio de nombres de Service Bus. El punto de conexión privado usa una dirección IP privada de la red virtual para incorporar el servicio de manera eficaz a su red virtual. Todo el tráfico dirigido al servicio se puede enrutar mediante el punto de conexión privado, por lo que no se necesitan puertas de enlace, dispositivos NAT, conexiones de ExpressRoute o VPN, ni direcciones IP públicas.

También puede proteger el espacio de nombres de Azure Service Bus mediante firewalls. Azure Service Bus admite controles de acceso basado en IP para la compatibilidad con el firewall de entrada. Puede establecer reglas de firewall mediante Azure Portal, plantillas de Azure Resource Manager, o a través de la CLI de Azure o Azure PowerShell.

- [Permiso para acceder a los espacios de nombres de Azure Service Bus a través de puntos de conexión privados](private-link-service.md)

- [Permitir el acceso al espacio de nombres de Azure Service Bus desde intervalos o direcciones IP específicas](service-bus-ip-filtering.md)

**Supervisión de Azure Security Center**: Sí

**Responsabilidad**: Customer

### <a name="12-monitor-and-log-the-configuration-and-traffic-of-virtual-networks-subnets-and-network-interfaces"></a>1.2: Supervisión y registro de la configuración y el tráfico de redes virtuales, subredes e interfaces de red

**Guía**: si usa máquinas virtuales de Azure para acceder a las entidades de Service Bus, habilite los registros de flujo de grupos de seguridad de red (NSG) y envíe los registros a una cuenta de almacenamiento para la auditoría de tráfico. También puede enviar registros de flujo de grupo de seguridad de red a un área de trabajo de Log Analytics y usar el Análisis de tráfico para proporcionar información detallada sobre el flujo de tráfico en la nube de Azure. Algunas de las ventajas del Análisis de tráfico son la capacidad de visualizar la actividad de la red e identificar las zonas activas, identificar las amenazas de seguridad, comprender los patrones de flujo de tráfico y detectar configuraciones de red incorrectas. 

Use Azure Security Center y siga las recomendaciones de protección de redes para ayudar a proteger sus recursos de Service Bus en Azure.

- [Habilitación de los registros de flujo de NSG](../network-watcher/network-watcher-nsg-flow-logging-portal.md)

- [Habilitación y uso del Análisis de tráfico](../network-watcher/traffic-analytics.md)

- [Habilitación de Network Watcher](../network-watcher/network-watcher-create.md)

- [Descripción de la seguridad de red proporcionada por Azure Security Center](../security-center/security-center-network-recommendations.md)

**Supervisión de Azure Security Center**: Sí

**Responsabilidad**: Customer

### <a name="14-deny-communications-with-known-malicious-ip-addresses"></a>1.4: Denegación de las comunicaciones con direcciones IP malintencionadas conocidas

**Guía**: habilite el estándar de DDoS Protection en las redes virtuales asociadas a los espacios de nombres de Service Bus para protegerse de los ataques por denegación de servicio distribuidos (DDoS). Use la inteligencia sobre amenazas integrada de Azure Security Center para denegar las comunicaciones con direcciones IP malintencionadas conocidas o no utilizadas.

- [Configuración de la protección contra DDoS](../virtual-network/manage-ddos-protection.md)

- [Inteligencia sobre amenazas integrada de Azure Security Center](../security-center/azure-defender.md)

**Supervisión de Azure Security Center**: Sí

**Responsabilidad**: Customer

### <a name="15-record-network-packets"></a>1.5: Registro de los paquetes de red

**Guía**: si usa máquinas virtuales de Azure para acceder a las entidades de Service Bus, puede usar la captura de paquetes de Network Watcher para investigar actividades anómalas.

- [Habilitación de Network Watcher](../network-watcher/network-watcher-create.md)

**Supervisión de Azure Security Center**: no disponible actualmente

**Responsabilidad**: Customer

### <a name="16-deploy-network-based-intrusion-detectionintrusion-prevention-systems-idsips"></a>1.6: Implementación de sistemas de prevención de intrusiones y detección de intrusiones (IDS/IPS) basados en la red

**Guía**: si usa máquinas virtuales de Azure para acceder a las entidades de Service Bus, seleccione una oferta de Azure Marketplace que admita la funcionalidad de IDS/IPS con capacidades de inspección de carga. Si la detección o prevención de intrusiones basadas en la inspección de la carga no es necesaria en su organización, puede usar la característica de firewall integrada de Azure Service Bus. Se puede restringir el acceso al espacio de nombres de Service Bus para un intervalo limitado de direcciones IP o una dirección IP específica mediante reglas de firewall.

- [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/?term=Firewall)

- [Procedimiento para agregar una regla de firewall en espacios de nombres de Service Bus para una dirección IP especificada](service-bus-ip-filtering.md)

**Supervisión de Azure Security Center**: Sí

**Responsabilidad**: Customer

### <a name="18-minimize-complexity-and-administrative-overhead-of-network-security-rules"></a>1.8: Minimice la complejidad y la sobrecarga administrativa de las reglas de seguridad de red

**Guía**: use etiquetas de servicio de Virtual Network para definir controles de acceso a la red en los grupos de seguridad de red o las instancias de Azure Firewall que filtran el tráfico hacia y desde los recursos de Service Bus. Puede utilizar etiquetas de servicio en lugar de direcciones IP específicas al crear reglas de seguridad. Al especificar el nombre de la etiqueta de servicio (por ejemplo, ServiceBus) en el campo de origen o destino apropiado de una regla, puede permitir o denegar el tráfico en el servicio correspondiente. Microsoft administra los prefijos de direcciones que la etiqueta de servicio incluye y actualiza automáticamente dicha etiqueta a medida que las direcciones cambian. 

- [Descripción y uso de las etiquetas de servicio](../virtual-network/service-tags-overview.md)

**Supervisión de Azure Security Center**: no disponible actualmente

**Responsabilidad**: Customer

### <a name="19-maintain-standard-security-configurations-for-network-devices"></a>1.9: Mantenga las configuraciones de seguridad estándar para dispositivos de red

**Guía**: defina e implemente configuraciones de seguridad estándar para los recursos de red asociados a los espacios de nombres de Azure Service Bus con Azure Policy. Use alias de Azure Policy en los espacios de nombres "Microsoft.ServiceBus" y "Microsoft.Network" para crear directivas personalizadas con el fin de auditar o aplicar la configuración de red de los espacios de nombres de Service Bus. También puede usar definiciones de directivas integradas relacionadas con Azure Service Bus, como:

- Service Bus debe usar un punto de conexión del servicio de red virtual
- Los registros de diagnóstico de Service Bus deben estar habilitados

También puede crear definiciones de directivas personalizadas si las definiciones integradas no se ajustan a las necesidades de su organización.

- [Configuración y administración de Azure Policy](../governance/policy/tutorials/create-and-manage.md)

- [Directiva integrada de Azure para un espacio de nombres de Service Bus](./policy-reference.md#azure-service-bus-messaging)

- [Ejemplos de Azure Policy para redes](../governance/policy/samples/built-in-policies.md#network)

- [Creación de un plano técnico de Azure](../governance/blueprints/create-blueprint-portal.md)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="110-document-traffic-configuration-rules"></a>1.10: Documente las reglas de configuración de tráfico

**Guía**: use etiquetas para redes virtuales y otros recursos relacionados con la seguridad de red y el flujo de tráfico que están asociados a los espacios de nombres de Service Bus. En el caso de las reglas de los grupos de seguridad de las redes individuales, use el campo "Descripción" para especificar la necesidad del negocio, la duración y otra información descriptiva sobre cualquier regla que permita el tráfico hacia o desde una red asociada a los espacios de nombres de Service Bus. 

Use cualquiera de las definiciones de directiva de Azure integradas relacionadas con el etiquetado, como "Requerir etiqueta y su valor" para asegurarse de que todos los recursos se crean con etiquetas y para notificarle los recursos no etiquetados existentes. 

Puede usar Azure PowerShell o la CLI de Azure para buscar o realizar acciones en los recursos en función de sus etiquetas. 

- [Creación y uso de etiquetas](../azure-resource-manager/management/tag-resources.md) 

- [Creación de una red virtual](../virtual-network/quick-create-portal.md) 

- [Creación de un grupo de seguridad de red con una configuración de seguridad](../virtual-network/tutorial-filter-network-traffic.md)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="111-use-automated-tools-to-monitor-network-resource-configurations-and-detect-changes"></a>1.11: Use herramientas automatizadas para supervisar las configuraciones de recursos de red y detectar cambios

**Guía**: use el registro de actividad de Azure para supervisar las configuraciones de los recursos de red y detectar cambios en los recursos de red relacionados con Azure Service Bus. Cree alertas en Azure Monitor que se desencadenarán cuando se produzcan cambios en los recursos de red críticos.

- [Visualización y recuperación de eventos del registro de actividad de Azure](../azure-monitor/platform/activity-log.md#view-the-activity-log)

- [Creación de alertas en Azure Monitor](../azure-monitor/platform/alerts-activity-log.md)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

## <a name="logging-and-monitoring"></a>Registro y supervisión

*Para obtener más información, consulte [Azure Security Benchmark: registro y supervisión](../security/benchmarks/security-control-logging-monitoring.md).*

### <a name="22-configure-central-security-log-management"></a>2.2: Configuración de la administración central de registros de seguridad

**Guía**: realice la ingesta de los registros a través de Azure Monitor para agregar datos de seguridad generados por Service Bus. En Azure Monitor, use áreas de trabajo de Log Analytics para realizar consultas y análisis, y configurar cuentas de Azure Storage para el almacenamiento a largo plazo o el archivado. También puede configurar registros relacionados con Service Bus que se van a enviar a Azure Sentinel o a una herramienta SIEM de terceros.

- [Procedimiento para definir la configuración de diagnóstico en Azure Service Bus](service-bus-diagnostic-logs.md)

- [Descripción del registro de actividad de Azure](../azure-monitor/platform/platform-logs-overview.md)

- [Incorporación de Azure Sentinel](../sentinel/quickstart-onboard.md) 

- [Introducción a Azure Monitor e integración con herramienta SIEM de terceros](https://azure.microsoft.com/blog/use-azure-monitor-to-integrate-with-siem-tools/)

**Supervisión de Azure Security Center**: Sí

**Responsabilidad**: Customer

### <a name="23-enable-audit-logging-for-azure-resources"></a>2.3: Habilitación del registro de auditoría para recursos de Azure

**Guía**: habilite la configuración de diagnóstico para el espacio de nombres de Azure Service Bus. En este momento, Azure Service Bus admite registros de actividad y operativos o de diagnóstico. Los registros de actividad contienen información sobre las operaciones realizadas en un trabajo. Los registros de diagnóstico proporcionan información más completa sobre las operaciones y acciones realizadas en el espacio de nombres mediante la API o a través de clientes de administración con el SDK de lenguaje. En concreto, estos registros capturan el tipo de operación, incluida la creación de colas, los recursos usados y el estado de la operación.

- [Procedimiento para habilitar la configuración de diagnóstico en Azure Service Bus](service-bus-diagnostic-logs.md)

- [Habilitación de la configuración de diagnóstico para el registro de actividad de Azure](../azure-monitor/platform/activity-log.md)

**Supervisión de Azure Security Center**: Sí

**Responsabilidad**: Customer

### <a name="25-configure-security-log-storage-retention"></a>2.5: Configuración de la retención del almacenamiento de registros de seguridad

**Guía**: en Azure Monitor, establezca el período de retención del área de trabajo de Log Analytics de acuerdo con la normativa de cumplimiento de su organización, con el fin de capturar y revisar los incidentes relacionados con Service Bus.

- [Procedimiento para configurar parámetros de retención de registros en áreas de trabajo de Log Analytics](../azure-monitor/platform/manage-cost-storage.md#change-the-data-retention-period)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="26-monitor-and-review-logs"></a>2.6: Supervisión y revisión de registros

**Guía**: analice y supervise los registros en busca de comportamientos anómalos y revise periódicamente los resultados relacionados con las entidades de Service Bus. Use Azure Monitor para revisar los registros y realizar consultas en los datos de los registros relacionados con Service Bus.

- [Más información sobre el área de trabajo de Log Analytics](../azure-monitor/log-query/get-started-portal.md)

- [Procedimiento para realizar consultas personalizadas en Azure Monitor](../azure-monitor/log-query/get-started-queries.md)

- [Incorporación de Azure Sentinel](../sentinel/quickstart-onboard.md)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="27-enable-alerts-for-anomalous-activities"></a>2.7: Habilitación de alertas para actividades anómalas

**Guía**: Use Azure Security Center con el área de trabajo de Log Analytics para la supervisión y alertas sobre actividades anómalas que se encuentran en los registros y eventos de seguridad. Como alternativa, también puede habilitar e incorporar datos a Azure Sentinel.

- [Incorporación de Azure Sentinel](../sentinel/quickstart-onboard.md)

- [Administración de alertas de seguridad en Azure Security Center](../security-center/security-center-managing-and-responding-alerts.md)

- [Alertas sobre datos de registro de Log Analytics](../azure-monitor/learn/tutorial-response.md)

**Supervisión de Azure Security Center**: no disponible actualmente

**Responsabilidad**: Customer

## <a name="identity-and-access-control"></a>Control de identidades y acceso

*Para obtener más información, consulte [Azure Security Benchmark: Identidad y control de acceso](../security/benchmarks/security-control-identity-access-control.md).*

### <a name="31-maintain-an-inventory-of-administrative-accounts"></a>3.1: Mantenga un inventario de cuentas administrativas

**Guía**: El control de acceso basado en roles de Azure (RBAC de Azure) permite administrar el acceso a los recursos de Azure a través de las asignaciones de roles. Puede asignar estos roles a usuarios, grupos de entidades de servicio e identidades administradas. Hay roles integrados predefinidos para Service Bus, que se pueden inventariar o consultar mediante herramientas, como la CLI de Azure, Azure PowerShell o Azure Portal.

- [Roles integrados para Azure Service Bus](authenticate-application.md#azure-built-in-roles-for-azure-service-bus)

- [Obtención de un rol de directorio en Azure AD con PowerShell](/powershell/module/azuread/get-azureaddirectoryrole?view=azureadps-2.0) 

- [Obtención de los miembros de un rol de directorio en Azure AD con PowerShell](/powershell/module/azuread/get-azureaddirectoryrolemember?view=azureadps-2.0)

**Supervisión de Azure Security Center**: Sí

**Responsabilidad**: Customer

### <a name="32-change-default-passwords-where-applicable"></a>3.2: Cambie las contraseñas predeterminadas cuando proceda

**Guía**: el acceso del plano de control a Service Bus se controla mediante Azure Active Directory (Azure AD). Azure AD no tiene el concepto de contraseñas predeterminadas.

El acceso al plano de datos a Service Bus se controla a través de Azure AD mediante identidades administradas, registros de aplicaciones o firmas de acceso compartido. Los clientes que se conectan a su espacio de nombres de Service Bus usan las firmas de acceso compartido, que se pueden volver a generar en cualquier momento.

- [Descripción de las firmas de acceso compartido para Service Bus](service-bus-sas.md)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="33-use-dedicated-administrative-accounts"></a>3.3: Use cuentas administrativas dedicadas

**Guía**: Cree procedimientos operativos estándar en torno al uso de cuentas administrativas dedicadas. Use la administración de identidad y acceso de Azure Security Center para supervisar el número de cuentas administrativas.

Además, para ayudarle a realizar un seguimiento de las cuentas administrativas dedicadas, puede seguir las recomendaciones de Azure Security Center o las directivas integradas de Azure Policy, por ejemplo:

- Debe haber más de un propietario asignado a su suscripción
- Las cuentas en desuso con permisos de propietario deben quitarse de la suscripción
- Las cuentas externas con permisos de propietario deben quitarse de la suscripción

También puede crear definiciones de directivas personalizadas si las definiciones integradas no se ajustan a las necesidades de su organización.

- [Procedimiento para usar Azure Security Center para supervisar la identidad y el acceso](../security-center/security-center-identity-access.md)

- [Uso de Azure Policy](../governance/policy/tutorials/create-and-manage.md)

**Supervisión de Azure Security Center**: Sí

**Responsabilidad**: Customer

### <a name="34-use-azure-active-directory-single-sign-on-sso"></a>3.4: Uso del inicio de sesión único (SSO) de Azure Active Directory

**Guía**: Microsoft Azure proporciona una administración integrada del control de acceso para recursos y aplicaciones basados en Azure Active Directory (Azure AD). Una ventaja clave de usar Azure AD con Azure Service Bus es que ya no es necesario almacenar las credenciales en el código. En su lugar, puede solicitar un token de acceso de OAuth 2.0 desde la Plataforma de identidad de Microsoft. El nombre del recurso para solicitar un token es https://azure.microsoft.com/services/service-bus/. Azure AD autentica la entidad de seguridad (un usuario, grupo o entidad de servicio) ejecutando la aplicación. Si la autenticación se realiza correctamente, Azure AD devuelve un token de acceso a la aplicación, y esta puede entonces usar el token de acceso para autorizar una solicitud de los recursos de Azure Service Bus.

- [Procedimiento para autenticar una aplicación con Azure AD para acceder a recursos de Service Bus](authenticate-application.md)

- [Descripción de SSO con Azure AD](../active-directory/manage-apps/what-is-single-sign-on.md)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="35-use-multi-factor-authentication-for-all-azure-active-directory-based-access"></a>3.5: Uso de la autenticación multifactor para todo el acceso basado en Azure Active Directory

**Guía**: habilite Multi-Factor Authentication (MFA) de Azure Active Directory y siga las recomendaciones de administración de identidades y acceso de Azure Security Center para ayudar a proteger los recursos habilitados para Service Bus.

- [Procedimiento para habilitar la MFA en Azure](../active-directory/authentication/howto-mfa-getstarted.md)

- [Supervisión de la identidad y el acceso en Azure Security Center](../security-center/security-center-identity-access.md)

**Supervisión de Azure Security Center**: Sí

**Responsabilidad**: Customer

### <a name="36-use-secure-azure-managed-workstations-for-administrative-tasks"></a>3.6: Uso de estaciones de trabajo seguras y administradas por Azure para realizar tareas administrativas

**Guía**: use estaciones de trabajo con acceso con privilegios (PAW) con Multi-Factor Authentication (MFA) configurada para iniciar sesión en recursos habilitados para Service Bus y configurarlos.

- [Más información sobre las estaciones de trabajo con privilegios de acceso](/windows-server/identity/securing-privileged-access/privileged-access-workstations)

- [Procedimiento para habilitar la MFA en Azure](../active-directory/authentication/howto-mfa-getstarted.md)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="37-log-and-alert-on-suspicious-activities-from-administrative-accounts"></a>3.7: Registro y alerta de actividades sospechosas desde cuentas administrativas

**Guía**: use la característica de supervisión y los informes de seguridad de Azure Active Directory para detectar cuando se producen actividades sospechosas o no seguras en el entorno. Use Azure Security Center para supervisar la actividad de identidad y acceso.

- [Procedimiento para identificar usuarios de Azure AD marcados por una actividad de riesgo](../active-directory/identity-protection/overview-identity-protection.md)

- [Supervisión de la actividad de identidad y acceso de los usuarios en Azure Security Center](../security-center/security-center-identity-access.md)

**Supervisión de Azure Security Center**: Sí

**Responsabilidad**: Customer

### <a name="38-manage-azure-resources-from-only-approved-locations"></a>3.8: Administre los recursos de Azure solo desde ubicaciones aprobadas

**Guía**: use ubicaciones con nombre de Azure AD para permitir el acceso solo desde agrupaciones lógicas específicas de intervalos de direcciones IP o países o regiones. 

- [Configuración de ubicaciones con nombre de Azure AD](../active-directory/reports-monitoring/quickstart-configure-named-locations.md)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="39-use-azure-active-directory"></a>3.9: Uso de Azure Active Directory

**Guía**: use Azure Active Directory (AD) como sistema central de autenticación y autorización para recursos de Azure, como Service Bus. Esto permite el control de acceso basado en roles (RBAC) para administrar recursos confidenciales.

- [Procedimiento para crear y configurar una instancia de Azure AD](../active-directory/fundamentals/active-directory-access-create-new-tenant.md)

- [Autorización del acceso a recursos de Service Bus mediante Azure Active Directory](authenticate-application.md)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="310-regularly-review-and-reconcile-user-access"></a>3.10: Revise y concilie regularmente el acceso de los usuarios

**Guía**: Azure Active Directory (Azure AD) proporciona registros para ayudarle a descubrir cuentas obsoletas. Además, use las revisiones de acceso de identidad de Azure para administrar de forma eficiente las pertenencias a grupos, el acceso a las aplicaciones empresariales y las asignaciones de roles. El acceso de los usuarios se puede revisar de forma periódica para asegurarse de que solo las personas adecuadas tengan acceso continuado.

Además, rote periódicamente la firma de acceso compartido del espacio de nombres de Service Bus.

- [Descripción de los informes de Azure AD](../active-directory/reports-monitoring/index.yml)

- [Procedimiento para usar las revisiones de acceso de identidad de Azure](../active-directory/governance/access-reviews-overview.md)

- [Descripción de las firmas de acceso compartido del espacio de nombres de Service Bus](service-bus-sas.md)

**Supervisión de Azure Security Center**: Sí

**Responsabilidad**: Customer

### <a name="311-monitor-attempts-to-access-deactivated-credentials"></a>3.11: Supervisión de los intentos de acceso a credenciales desactivadas

**Guía**: tiene acceso a los orígenes del registro de eventos de las actividades de inicio de sesión, auditoría y riesgo de Azure Active Directory (Azure AD), que permiten la integración en Azure Sentinel o en una herramienta SIEM de terceros.

Para simplificar este proceso, cree una configuración de diagnóstico para las cuentas de usuario de Azure AD y envíe los registros de auditoría y los registros de inicio de sesión a un área de trabajo de Log Analytics. Luego, puede configurar en Azure Monitor las alertas de registro que quiera para determinadas acciones que se producen en los registros.

- [Integración de los registros de actividad de Azure en Azure Monitor](../active-directory/reports-monitoring/howto-integrate-activity-logs-with-log-analytics.md)

- [Autorización del acceso a recursos de Service Bus mediante Azure Active Directory](authenticate-application.md)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="312-alert-on-account-sign-in-behavior-deviation"></a>3.12: Alerta de las desviaciones de comportamiento en los inicios de sesión de las cuentas

**Guía**: use las características Identity Protection y de detección de riesgos de Azure Active Directory para configurar respuestas automatizadas a acciones sospechosas detectadas relacionadas con los recursos habilitados para Service Bus. Debe habilitar las respuestas automatizadas a través de Azure Sentinel para implementar las respuestas de seguridad de su organización.

- [Visualización de los inicios de sesión de riesgo de Azure AD](../active-directory/identity-protection/overview-identity-protection.md)

- [Configuración y habilitación de las directivas de riesgo de protección de identidad](../active-directory/identity-protection/howto-identity-protection-configure-risk-policies.md)

- [Incorporación de Azure Sentinel](../sentinel/quickstart-onboard.md)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="313-provide-microsoft-with-access-to-relevant-customer-data-during-support-scenarios"></a>3.13: Proporcione a Microsoft acceso a los datos pertinentes del cliente durante los escenarios de soporte técnico

**Guía**: actualmente no está disponible; la característica Caja de seguridad del cliente todavía no se admite en Service Bus.

- [Lista de servicios admitidos por la Caja de seguridad del cliente](../security/fundamentals/customer-lockbox-overview.md#supported-services-and-scenarios-in-general-availability)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

## <a name="data-protection"></a>Protección de los datos

*Para obtener más información, consulte [Azure Security Benchmark: Protección de datos](../security/benchmarks/security-control-data-protection.md).*

### <a name="41-maintain-an-inventory-of-sensitive-information"></a>4.1: Mantenimiento de un inventario de información confidencial

**Guía**: use etiquetas en los recursos relacionados con las instancias de Service Bus para ayudar a realizar un seguimiento de los recursos de Azure que almacenan o procesan información confidencial.

- [Creación y uso de etiquetas](../azure-resource-manager/management/tag-resources.md)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="42-isolate-systems-storing-or-processing-sensitive-information"></a>4.2: Aislamiento de los sistemas que almacenan o procesan información confidencial

**Guía**: implemente suscripciones o grupos de administración independientes para los entornos de desarrollo, pruebas y producción. Los espacios de nombres de Service Bus deben estar separados por redes virtuales con puntos de conexión privados configurados y etiquetados adecuadamente.

- [Creación de suscripciones adicionales de Azure](../cost-management-billing/manage/create-subscription.md)

- [Creación de grupos de administración](../governance/management-groups/create-management-group-portal.md)

- [Procedimiento para crear y usar etiquetas](../azure-resource-manager/management/tag-resources.md)

- [Creación de una red virtual](../virtual-network/quick-create-portal.md)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="43-monitor-and-block-unauthorized-transfer-of-sensitive-information"></a>4.3: Supervisión y bloqueo de una transferencia no autorizada de información confidencial

**Guía**: si usa máquinas virtuales de Azure para acceder a las entidades de Service Bus, aproveche las redes virtuales, los puntos de conexión privados, el firewall de Service Bus, los grupos de seguridad de red y las etiquetas de servicio para mitigar la posibilidad de que se produzca una filtración de datos.

Microsoft administra la infraestructura subyacente para Azure Service Bus y ha implementado controles estrictos para evitar la pérdida o exposición de los datos de los clientes.

- [Configuración de reglas de firewall de IP para espacios de nombres de Azure Service Bus](service-bus-ip-filtering.md)

- [Permitir el acceso al espacio de nombres de Azure Service Bus desde redes virtuales específicas](private-link-service.md)

- [Permiso para acceder a los espacios de nombres de Azure Service Bus a través de puntos de conexión privados](private-link-service.md)

- [Descripción de los grupos de seguridad de red y las etiquetas de servicio](../virtual-network/network-security-groups-overview.md)

- [Descripción de la protección de datos de los clientes en Azure](../security/fundamentals/protection-customer-data.md)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Compartido

### <a name="44-encrypt-all-sensitive-information-in-transit"></a>4.4: Cifrado de toda la información confidencial en tránsito

**Guía**: Azure Service Bus aplica de forma predeterminada las comunicaciones cifradas con TLS. Actualmente se admiten las versiones de TLS 1.0, 1.1 y 1.2. Sin embargo, TLS 1.0 y 1.1 están a punto de quedar en desuso en todo el sector, por lo que se recomienda TLS 1.2 o una versión posterior siempre que sea posible.

- [Para comprender las características de seguridad de Service Bus, consulte el artículo sobre seguridad de redes](network-security.md).

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Compartido

### <a name="45-use-an-active-discovery-tool-to-identify-sensitive-data"></a>4.5: Uso de una herramienta de detección activa para identificar datos confidenciales

**Guía**: las características de identificación, clasificación y prevención de pérdida de datos todavía no están disponibles para Azure Service Bus. Implemente una solución de terceros, si es necesario, para fines de cumplimiento.

En el caso de la plataforma subyacente administrada por Microsoft, Microsoft trata todo el contenido de los clientes como confidencial y hace grandes esfuerzos para proteger a los clientes contra la pérdida y exposición de sus datos. Para garantizar la seguridad de los datos de los clientes dentro de Azure, Microsoft ha implementado y mantiene un conjunto de controles y funcionalidades eficaces de protección de datos.

- [Descripción de la protección de datos de los clientes en Azure](../security/fundamentals/protection-customer-data.md)

**Supervisión de Azure Security Center**: no disponible actualmente

**Responsabilidad**: Compartido

### <a name="46-use-role-based-access-control-to-control-access-to-resources"></a>4.6: Uso del control de acceso basado en rol para controlar el acceso a los recursos

**Guía**: Azure Service Bus admite el uso de Azure Active Directory (Azure AD) para autorizar solicitudes a entidades de Service Bus. Con Azure AD, puede usar el control de acceso basado en roles (RBAC) de Azure para conceder permisos a una entidad de seguridad, que puede ser un usuario o una entidad de servicio de aplicación.

- [Descripción de RBAC de Azure y los roles disponibles para Azure Service Bus](authenticate-application.md)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="48-encrypt-sensitive-information-at-rest"></a>4.8: Cifrado de información confidencial en reposo

**Guía**: Azure Service Bus admite la opción de cifrado de datos en reposo con claves administradas por Microsoft o claves administradas por el cliente. Esta característica permite crear, rotar, deshabilitar y revocar el acceso a las claves administradas por el cliente que se usan para cifrar datos en reposo de Azure Service Bus.

- [Procedimiento para configurar claves administradas por el cliente para el cifrado de Azure Service Bus](configure-customer-managed-key.md)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="49-log-and-alert-on-changes-to-critical-azure-resources"></a>4.9: Registro y alerta de cambios en los recursos críticos de Azure

**Guía**: use Azure Monitor con el registro de actividad de Azure para crear alertas para cuando se produzcan cambios en las instancias de producción de Azure Service Bus y otros recursos críticos o relacionados.

- [Creación de alertas para los eventos del registro de actividad de Azure](../azure-monitor/platform/alerts-activity-log.md)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

## <a name="inventory-and-asset-management"></a>Inventario y administración de recursos

*Para obtener más información, consulte [Azure Security Benchmark: Administración de recursos y del inventario](../security/benchmarks/security-control-inventory-asset-management.md).*

### <a name="61-use-automated-asset-discovery-solution"></a>6.1: Uso de la solución de detección de recursos automatizada

**Guía**: use Azure Resource Graph para consultar y detectar todos los recursos (incluidos los espacios de nombres de Azure Service Bus) dentro de las suscripciones. Asegúrese de que tiene los permisos adecuados (lectura) en el inquilino y de que puede enumerar todas las suscripciones de Azure, así como los recursos de las suscripciones.

- [Creación de consultas con Azure Resource Graph](../governance/resource-graph/first-query-portal.md)

- [Visualización de las suscripciones de Azure](/powershell/module/az.accounts/get-azsubscription?view=azps-3.0.0)

- [Descripción de Azure RBAC](../role-based-access-control/overview.md)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="62-maintain-asset-metadata"></a>6.2: Mantenimiento de metadatos de recursos

**Instrucciones**: Aplique etiquetas a los recursos de Azure que proporcionan metadatos para organizarlos de forma lógica en una taxonomía.

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="63-delete-unauthorized-azure-resources"></a>6.3: Eliminación de recursos de Azure no autorizados

**Guía**: use el etiquetado, los grupos de administración y las suscripciones independientes, si procede, para organizar los espacios de nombres de Azure Service Bus y los recursos relacionados, así como para realizar un seguimiento. Concilie el inventario periódicamente y asegúrese de que los recursos no autorizados se eliminan de la suscripción de manera oportuna.

- [Creación de suscripciones adicionales de Azure](../cost-management-billing/manage/create-subscription.md)

- [Creación de grupos de administración](../governance/management-groups/create-management-group-portal.md)

- [Creación y uso de etiquetas](../azure-resource-manager/management/tag-resources.md)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="64-define-and-maintain-inventory-of-approved-azure-resources"></a>6.4: Definición y mantenimiento de un inventario de los recursos de Azure aprobados

**Guía**: Cree un inventario de los recursos de Azure aprobados y el software aprobado para los recursos de proceso según las necesidades de la organización.

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="65-monitor-for-unapproved-azure-resources"></a>6.5: Supervisión de recursos de Azure no aprobados

**Guía**: Use Azure Policy para aplicar restricciones en el tipo de recursos que se pueden crear en las suscripciones del cliente con las siguientes definiciones de directiva integradas:

- Tipos de recursos no permitidos

- Tipos de recursos permitidos

Además, use Azure Resource Graph para consultar o detectar recursos dentro de las suscripciones.

- [Configuración y administración de Azure Policy](../governance/policy/tutorials/create-and-manage.md)

- [Creación de consultas con Azure Graph](../governance/resource-graph/first-query-portal.md)

**Supervisión de Azure Security Center**: Sí

**Responsabilidad**: Customer

### <a name="69-use-only-approved-azure-services"></a>6.9: Uso exclusivo de servicios de Azure aprobados

**Guía**: Use Azure Policy para aplicar restricciones en el tipo de recursos que se pueden crear en las suscripciones del cliente con las siguientes definiciones de directiva integradas:

- Tipos de recursos no permitidos
- Tipos de recursos permitidos

También puede crear definiciones de directivas personalizadas si las definiciones integradas no se ajustan a las necesidades de su organización.

- [Configuración y administración de Azure Policy](../governance/policy/tutorials/create-and-manage.md)

- [Denegación de un tipo de recurso específico con Azure Policy](../governance/policy/samples/index.md)

**Supervisión de Azure Security Center**: Sí

**Responsabilidad**: Customer

### <a name="611-limit-users-ability-to-interact-with-azure-resource-manager"></a>6.11: Limitación de la capacidad de los usuarios para interactuar con Azure Resource Manager

**Guía**: Configure el acceso condicional de Azure para limitar la capacidad de los usuarios de interactuar con Azure Resource Manager configurando "Bloquear acceso" en la aplicación Microsoft Azure Management.

- [Configuración del acceso condicional para bloquear el acceso a Azure Resource Manager](../role-based-access-control/conditional-access-azure-management.md)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

## <a name="secure-configuration"></a>Configuración segura

*Para obtener más información, consulte [Azure Security Benchmark: Configuración segura](../security/benchmarks/security-control-secure-configuration.md).*

### <a name="71-establish-secure-configurations-for-all-azure-resources"></a>7.1: Establezca configuraciones seguras para todos los recursos de Azure

**Guía**: defina e implemente configuraciones de seguridad estándar para las implementaciones de Azure Service Bus. También puede usar definiciones de directiva integradas en Azure Service Bus tales como:

- Los registros de diagnóstico de Service Bus deben estar habilitados
- Service Bus debe usar un punto de conexión de servicio de red virtual para limitar el tráfico de red a las redes privadas.

Use alias de Azure Policy en el espacio de nombres "Microsoft.ServiceBus" para crear directivas personalizadas con el fin de auditar o aplicar las configuraciones.

- [Directivas integradas de Azure para Service Bus ](./policy-reference.md)

- [Visualización de los alias de Azure Policy disponibles](/powershell/module/az.resources/get-azpolicyalias?view=azps-3.3.0)

- [Configuración y administración de Azure Policy](../governance/policy/tutorials/create-and-manage.md)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="73-maintain-secure-azure-resource-configurations"></a>7.3: Mantenga configuraciones de recursos de Azure seguras

**Guía**: use las directivas [deny] y [deployifnotexist] de Azure Policy para aplicar la configuración segura en los recursos habilitados para Service Bus.

- [Configuración y administración de Azure Policy](../governance/policy/tutorials/create-and-manage.md)

- [Para más información, consulte el artículo sobre los efectos de Azure Policy](../governance/policy/concepts/effects.md).

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="77-deploy-configuration-management-tools-for-azure-resources"></a>7.7: Implementación de herramientas de administración de configuración para recursos de Azure

**Guía**: use alias de Azure Policy en el espacio de nombres "Microsoft.ServiceBus" para crear directivas personalizadas con el fin de auditar y aplicar las configuraciones del sistema y enviar alertas sobre ellas. Además, desarrolle un proceso y una canalización para administrar las excepciones de las directivas.

- [Configuración y administración de Azure Policy](../governance/policy/tutorials/create-and-manage.md)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="79-implement-automated-configuration-monitoring-for-azure-resources"></a>7.9: Implementación de la supervisión de la configuración automatizada para los recursos de Azure

**Guía**: use alias de Azure Policy en el espacio de nombres "Microsoft.ServiceBus" para crear directivas personalizadas con el fin de auditar y aplicar las configuraciones del sistema y enviar alertas sobre ellas. Use las directivas [audit], [deny] y [deployifnotexist] de Azure Policy para aplicar automáticamente las configuraciones en las implementaciones de Azure Service Bus y los recursos relacionados.

- [Configuración y administración de Azure Policy](../governance/policy/tutorials/create-and-manage.md)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="711-manage-azure-secrets-securely"></a>7.11: Administre los secretos de Azure de forma segura

**Guía**:  en el caso de máquinas virtuales de Azure o aplicaciones web que se ejecuten en Azure App Service y se usen para acceder a las entidades de Service Bus, emplee una instancia de Managed Service Identity junto con Azure Key Vault para simplificar y proteger la administración de firmas de acceso compartido en sus implementaciones de Azure Service Bus. Asegúrese de que la eliminación temporal de Key Vault esté habilitada.

- [Autenticación de una identidad administrada con Azure Active Directory para acceder a recursos de Service Bus](service-bus-managed-service-identity.md)

- [Configuración de claves administradas por el cliente para Service Bus](configure-customer-managed-key.md)

- [Creación de un almacén de claves](../key-vault/general/quick-create-portal.md)

**Supervisión de Azure Security Center**: Sí

**Responsabilidad**: Customer

### <a name="712-manage-identities-securely-and-automatically"></a>7.12: Administre las identidades de forma segura y automática

**Guía**: en el caso de máquinas virtuales de Azure o aplicaciones web que se ejecuten en Azure App Service y se usen para acceder a las entidades de Service Bus, emplee una instancia de Managed Service Identity junto con Azure Key Vault para simplificar y proteger Azure Service Bus. Asegúrese de que la eliminación temporal de Key Vault esté habilitada.

Use identidades administradas para proporcionar servicios de Azure con una identidad administrada automáticamente en Azure Active Directory (Azure AD). Identidades administradas le permite autenticarse en cualquier servicio que admita la autenticación de Azure AD, incluido Azure Key Vault, sin necesidad de credenciales en el código.

- [Autenticación de una identidad administrada con Azure Active Directory para acceder a recursos de Service Bus](service-bus-managed-service-identity.md)

- [Configuración de claves administradas por el cliente para Service Bus](configure-customer-managed-key.md)

**Supervisión de Azure Security Center**: Sí

**Responsabilidad**: Customer

### <a name="713-eliminate-unintended-credential-exposure"></a>7.13: Elimine la exposición de credenciales no intencionada

**Instrucciones**: Implemente el escáner de credenciales para identificar las credenciales en el código. El escáner de credenciales también fomenta el traslado de credenciales detectadas a ubicaciones más seguras, como Azure Key Vault.

- [Configuración del escáner de credenciales](https://secdevtools.azurewebsites.net/helpcredscan.html)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

## <a name="malware-defense"></a>Defensa contra malware

*Para obtener más información, consulte [Azure Security Benchmark: defensa contra malware](../security/benchmarks/security-control-malware-defense.md).*

### <a name="82-pre-scan-files-to-be-uploaded-to-non-compute-azure-resources"></a>8.2: Examine previamente los archivos que se van a cargar en recursos de Azure que no son de proceso

**Guía**: examine previamente el contenido que se carga en recursos de Azure que no son de proceso, como Azure Service Bus, App Service, Data Lake Storage, Blob Storage, Azure Database for PostgreSQL, etc. Microsoft no tiene acceso a los datos de estas instancias.

Microsoft Antimalware está habilitado en el host subyacente que admite los servicios de Azure, pero no se ejecuta en el contenido del cliente.

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Compartido

## <a name="data-recovery"></a>Recuperación de datos

*Para más información, consulte [Azure Security Benchmark: recuperación de datos](../security/benchmarks/security-control-data-recovery.md).*

### <a name="91-ensure-regular-automated-back-ups"></a>9.1: Garantía de copias de seguridad automáticas periódicas

**Guía**: configure la recuperación ante desastres geográfica para Azure Service Bus. Cuando hay regiones de Azure completas o centros de datos (si no se utilizan zonas de disponibilidad) que experimentan un tiempo de inactividad, es crucial que el procesamiento de datos siga funcionando en otra región o centro de datos. De esta forma, la recuperación ante desastres geográfica y la replicación geográfica son características importantes para cualquier empresa. Azure Service Bus admite tanto la recuperación ante desastres con localización geográfica como la replicación geográfica, en el nivel de espacio de nombres.

- [Descripción de la recuperación ante desastres geográfica para Azure Service Bus](service-bus-geo-dr.md)

**Supervisión de Azure Security Center**: no disponible actualmente

**Responsabilidad**: Customer

### <a name="92-perform-complete-system-backups-and-backup-any-customer-managed-keys"></a>9.2: Copias de seguridad completas del sistema y copia de seguridad de las claves administradas por el cliente

**Guía**: Azure Service Bus proporciona cifrado de datos en reposo con Azure Storage Service Encryption (Azure SSE). Service Bus se basa en Azure Storage para almacenar los datos y, de forma predeterminada, todos los datos que se almacenan con Azure Storage se cifran mediante claves administradas por Microsoft. Si usa Azure Key Vault para almacenar claves administradas por el cliente, asegúrese de realizar copias de seguridad automatizadas periódicas de las claves.

Asegúrese de realizar copias de seguridad automatizadas periódicas de sus secretos de Key Vault con el siguiente comando de PowerShell: Backup-AzKeyVaultSecret

- [Procedimiento para configurar claves administradas por el cliente para el cifrado de datos en reposo de Azure Service Bus](configure-customer-managed-key.md)

- [Procedimiento para realizar copias de seguridad de secretos de Key Vault](/powershell/module/azurerm.keyvault/backup-azurekeyvaultsecret)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="93-validate-all-backups-including-customer-managed-keys"></a>9.3: Validación de todas las copias de seguridad, incluidas las claves administradas por el cliente

**Guía**: pruebe la restauración de las claves administradas por el cliente que se usan para cifrar los datos de Service Bus.

- [Procedimiento para configurar claves administradas por el cliente para el cifrado de datos en reposo de Azure Service Bus](configure-customer-managed-key.md)

- [Restauración de las claves del almacén de claves en Azure](/powershell/module/azurerm.keyvault/restore-azurekeyvaultkey?view=azurermps-6.13.0)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="94-ensure-protection-of-backups-and-customer-managed-keys"></a>9.4: Garantía de la protección de las copias de seguridad y las claves administradas por el cliente

**Guía**: Habilite la eliminación temporal en Key Vault para proteger las claves contra la eliminación accidental o malintencionada. Azure Service Bus requiere que las claves administradas por el cliente tengan configuradas las opciones Eliminación temporal y No purgar.

- [Habilitación de la eliminación temporal en Key Vault](../storage/blobs/soft-delete-blob-overview.md?tabs=azure-portal)

- [Configuración de un almacén de claves con claves](../event-hubs/configure-customer-managed-key.md)

**Supervisión de Azure Security Center**: Sí

**Responsabilidad**: Customer

## <a name="incident-response"></a>Respuesta a los incidentes

*Para obtener más información, consulte [Azure Security Benchmark: Respuesta a los incidentes](../security/benchmarks/security-control-incident-response.md).*

### <a name="101-create-an-incident-response-guide"></a>10.1: Creación de una guía de respuesta ante incidentes

**Guía**: desarrolle una guía de respuesta a incidentes para su organización. Asegúrese de que haya planes de respuesta a incidentes escritos que definan todos los roles del personal, así como las fases de administración y gestión de los incidentes, desde la detección hasta la revisión posterior a la incidencia. 

- [Guía para crear su propio proceso de respuesta a incidentes de seguridad](https://msrc-blog.microsoft.com/2019/07/01/inside-the-msrc-building-your-own-security-incident-response-process/) 

- [Anatomía de un incidente del Centro de respuestas de seguridad de Microsoft](https://msrc-blog.microsoft.com/2019/06/27/inside-the-msrc-anatomy-of-a-ssirp-incident/) 

- [Uso de la guía de control de incidentes de seguridad de equipos de NIST para la creación de su propio plan de respuesta a incidentes](https://csrc.nist.gov/publications/detail/sp/800-61/rev-2/final)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="102-create-an-incident-scoring-and-prioritization-procedure"></a>10.2: Creación de un procedimiento de priorización y puntuación de incidentes

**Instrucciones**: Azure Security Center asigna una gravedad a cada alerta para ayudarle a priorizar aquellas que se deben investigar en primer lugar. La gravedad se basa en la confianza que tiene Security Center en la búsqueda o en el análisis utilizados para emitir la alerta, así como en el nivel de confianza de que ha habido un intento malintencionado detrás de la actividad que ha provocado la alerta.

Adicionalmente, marque las suscripciones con etiquetas y cree un sistema de nomenclatura para identificar y clasificar los recursos de Azure, especialmente los que procesan datos confidenciales. Es su responsabilidad asignar prioridades a la corrección de las alertas en función de la importancia de los recursos y el entorno de Azure donde se produjo el incidente. 

- [Alertas de seguridad en el Centro de seguridad de Azure](../security-center/security-center-alerts-overview.md) 

- [Uso de etiquetas para organizar los recursos de Azure](../azure-resource-manager/management/tag-resources.md)

**Supervisión de Azure Security Center**: Sí

**Responsabilidad**: Customer

### <a name="103-test-security-response-procedures"></a>10.3: Prueba de los procedimientos de respuesta de seguridad

**Guía**: Realice ejercicios para probar las capacidades de respuesta a los incidentes de los sistemas con regularidad. Identifique puntos débiles y brechas y revise el plan según sea necesario.

- [Publicación de NIST: Guía para probar, entrenar y ejecutar programas para planes y funcionalidades de TI](https://nvlpubs.nist.gov/nistpubs/Legacy/SP/nistspecialpublication800-84.pdf)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="104-provide-security-incident-contact-details-and-configure-alert-notifications-for-security-incidents"></a>10.4: Provisión de detalles de contacto de incidentes de seguridad y configuración de notificaciones de alerta para incidentes de seguridad

**Instrucciones**: La información de contacto del incidente de seguridad la utilizará Microsoft para ponerse en contacto con usted si Microsoft Security Response Center (MSRC) detecta que un tercero no autorizado o ilegal ha accedido a los datos. Revise los incidentes después del hecho para asegurarse de que se resuelven los problemas. 

- [Establecimiento del contacto de seguridad de Azure Security Center](../security-center/security-center-provide-security-contact-details.md)

**Supervisión de Azure Security Center**: Sí

**Responsabilidad**: Customer

### <a name="105-incorporate-security-alerts-into-your-incident-response-system"></a>10.5: Incorporación de alertas de seguridad en el sistema de respuesta a incidentes

**Instrucciones**: exporte las alertas y recomendaciones de Azure Security Center y mediante la característica de exportación continua para ayudar a identificar riesgos en los recursos de Azure. La exportación continua permite exportar alertas y recomendaciones de forma manual o continua. Puede usar el conector de datos de Azure Security Center para enviar las alertas a Azure Sentinel. 

- [Configuración de la exportación continua](../security-center/continuous-export.md) 

- [Transmisión de alertas a Azure Sentinel](../sentinel/connect-azure-security-center.md)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="106-automate-the-response-to-security-alerts"></a>10.6: Automatización de la respuesta a las alertas de seguridad

**Guía**: 

use la característica de automatización de flujos de trabajo de Azure Security Center para desencadenar automáticamente las alertas y recomendaciones de seguridad con el fin de proteger los recursos de Azure. 

- [Configuración de la automatización de flujos de trabajo en Security Center](../security-center/workflow-automation.md)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

## <a name="penetration-tests-and-red-team-exercises"></a>Pruebas de penetración y ejercicios del equipo rojo

*Para obtener más información, consulte [Azure Security Benchmark: Pruebas de penetración y ejercicios del equipo rojo](../security/benchmarks/security-control-penetration-tests-red-team-exercises.md).*

### <a name="111-conduct-regular-penetration-testing-of-your-azure-resources-and-ensure-remediation-of-all-critical-security-findings"></a>11.1: Realice pruebas de penetración periódicas de los recursos de Azure y asegúrese de corregir todos los resultados de seguridad críticos

**Guía**: Siga las reglas de compromiso de la prueba de penetración de Microsoft Cloud para asegurarse de que las pruebas de penetración no infrinjan las directivas de Microsoft. Use la estrategia de Microsoft y la ejecución de las pruebas de penetración del equipo rojo y sitios activos en la infraestructura de nube, los servicios y las aplicaciones administradas por Microsoft. 

- [Reglas de interacción de las pruebas de penetración](https://www.microsoft.com/msrc/pentest-rules-of-engagement?rtc=1) 

- [Equipo rojo de Microsoft Cloud](https://gallery.technet.microsoft.com/Cloud-Red-Teaming-b837392e)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Compartido

## <a name="next-steps"></a>Pasos siguientes

- Consulte la [prueba comparativa de seguridad de Azure](../security/benchmarks/overview.md).
- Obtenga más información sobre las [líneas de base de seguridad de Azure](../security/benchmarks/security-baselines-overview.md).
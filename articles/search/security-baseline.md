---
title: Base de referencia de Azure Cognitive Search
description: La base de referencia de seguridad de Azure Cognitive Search proporciona instrucciones de procedimientos y recursos para implementar las recomendaciones de seguridad especificadas en Azure Security Benchmark.
author: msmbaldwin
ms.service: search
ms.topic: conceptual
ms.date: 09/25/2020
ms.author: mbaldwin
ms.custom: subject-security-benchmark
ms.openlocfilehash: f4782b923222208bbf759ba8415162621a55e0e1
ms.sourcegitcommit: fb3c846de147cc2e3515cd8219d8c84790e3a442
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/27/2020
ms.locfileid: "92631162"
---
# <a name="azure-security-baseline-for-azure-cognitive-search"></a>Base de referencia de Azure Cognitive Search

Esta base de referencia de seguridad aplica la guía de la [versión 1.0 de Azure Security Benchmark](../security/benchmarks/overview.md) en Azure Cognitive Search. Azure Security Benchmark proporciona recomendaciones sobre cómo puede proteger sus soluciones de nube en Azure. El contenido se agrupa por los **controles de seguridad** definidos por Azure Security Benchmark y las instrucciones relacionadas aplicables a Azure Cognitive Search. Se han excluido los **controles** no aplicables a Azure Cognitive Search o al cliente.

Para ver cómo Azure Cognitive Search se asigna por completo a Azure Security Benchmark, consulte el [archivo de asignación de base de referencia de seguridad de Cognitive Search completo](https://github.com/MicrosoftDocs/SecurityBenchmarks/tree/master/Azure%20Offer%20Security%20Baselines).

## <a name="network-security"></a>Seguridad de las redes

*Para más información, consulte [Azure Security Benchmark: Seguridad de redes](../security/benchmarks/security-control-network-security.md).*

### <a name="11-protect-azure-resources-within-virtual-networks"></a>1.1: Protección de los recursos de Azure dentro de las redes virtuales

**Guía** : asegúrese de que a todas las implementaciones de subred de Microsoft Azure Virtual Network se les haya aplicado un grupo de seguridad de red con reglas para implementar un esquema de acceso "con privilegios mínimos". Solo permita el acceso a los puertos de confianza de la aplicación y a los intervalos de direcciones IP. Siempre que sea factible, implemente Azure Cognitive Search con un punto de conexión privado de Azure para permitir el acceso privado a sus servicios desde su red virtual.

Cognitive Search también admite la funcionalidad de seguridad de red adicional para administrar las listas de control de acceso de la red. Configure el servicio de búsqueda para que solo permita la comunicación con orígenes de confianza, restringiendo el acceso de intervalos específicos de direcciones IP públicas mediante su funcionalidad de firewall.

- [Procedimiento para configurar puntos de conexión privados para Azure Cognitive Search](./service-create-private-endpoint.md)

- [Procedimiento para configurar el firewall de Azure Cognitive Search](./service-configure-firewall.md)

**Supervisión de Azure Security Center** : no disponible actualmente

**Responsabilidad** : Customer

### <a name="12-monitor-and-log-the-configuration-and-traffic-of-virtual-networks-subnets-and-nics"></a>1.2: Supervisión y registro de la configuración y el tráfico de redes virtuales, subredes y NIC

**Guía** : Cognitive Search no se puede implementar directamente en una red virtual. Sin embargo, si la aplicación cliente o los orígenes de datos están en una red virtual, puede supervisar y registrar el tráfico de esos componentes integrados en la red, lo que incluye las solicitudes enviadas a un servicio de búsqueda en la nube. Las recomendaciones estándar incluyen la habilitación de un registro de flujo de grupo de seguridad de red y el envío de registros a Azure Storage o a un área de trabajo de Log Analytics. También puede usar Análisis de tráfico para obtener información detallada sobre los patrones de tráfico.

- [Habilitación de los registros de flujo de grupos de seguridad de red](../network-watcher/network-watcher-nsg-flow-logging-portal.md)

- [Habilitación y uso del Análisis de tráfico](../network-watcher/traffic-analytics.md)

- [Descripción de la seguridad de red proporcionada por Azure Security Center](../security-center/security-center-network-recommendations.md)

**Supervisión de Azure Security Center** : Sí

**Responsabilidad** : Customer

### <a name="13-protect-critical-web-applications"></a>1.3: Proteja las aplicaciones web críticas

**Guía** : no se aplica a Cognitive Search. Esta recomendación se destina a las aplicaciones web que se ejecutan en Azure App Service o en recursos de proceso.

**Supervisión de Azure Security Center** : No aplicable

**Responsabilidad** : No aplicable

### <a name="14-deny-communications-with-known-malicious-ip-addresses"></a>1.4: Deniegue las comunicaciones con direcciones IP malintencionadas conocidas

**Guía** : Cognitive Search no ofrece una característica específica para combatir un ataque de denegación de servicio distribuido, pero puede habilitar DDoS Protection estándar en las redes virtuales asociadas a su servicio Cognitive Search como protección general.

- [Configuración de la protección contra DDoS](../virtual-network/manage-ddos-protection.md)

**Supervisión de Azure Security Center** : no disponible actualmente

**Responsabilidad** : Customer

### <a name="15-record-network-packets"></a>1.5: Registro de los paquetes de red

**Guía** : habilite los registros de flujo de grupos de seguridad de red para los grupos de seguridad de red que protegen Azure Virtual Machines (VM) que se conectarán a su servicio Cognitive Search. Envíe registros a una cuenta de Azure Storage para la auditoría de tráfico. 

Habilite la captura de paquetes de Network Watcher si es necesario para investigar actividades anómalas.

- [Habilitación de los registros de flujo de NSG](../network-watcher/network-watcher-nsg-flow-logging-portal.md)

- [Habilitación de Network Watcher](../network-watcher/network-watcher-create.md)

**Supervisión de Azure Security Center** : no disponible actualmente

**Responsabilidad** : Customer

### <a name="16-deploy-network-based-intrusion-detectionintrusion-prevention-systems-idsips"></a>1.6: Implementación de sistemas de prevención de intrusiones y detección de intrusiones (IDS/IPS) basados en la red

**Guía** : Cognitive Search no admite la detección de intrusiones en la red, pero, como mitigación de intrusiones, puede configurar reglas de firewall para especificar las direcciones IP aceptadas por el servicio Cognitive Search. Configure un punto de conexión privado para mantener el tráfico de búsqueda fuera de la red pública de Internet.

- [Procedimiento para configurar claves administradas por el cliente para el cifrado de datos en reposo](./search-security-manage-encryption-keys.md)

- [Procedimiento para obtener información de la clave administrada por el cliente de mapas de sinónimos e índices](./search-security-get-encryption-keys.md)

**Supervisión de Azure Security Center** : no disponible actualmente

**Responsabilidad** : Customer

### <a name="17-manage-traffic-to-web-applications"></a>1.7: Administre el tráfico a las aplicaciones web

**Guía** : no se aplica a Cognitive Search. Esta recomendación se destina a las aplicaciones web que se ejecutan en Azure App Service o en recursos de proceso.

**Supervisión de Azure Security Center** : No aplicable

**Responsabilidad** : No aplicable

### <a name="18-minimize-complexity-and-administrative-overhead-of-network-security-rules"></a>1.8: Minimice la complejidad y la sobrecarga administrativa de las reglas de seguridad de red

**Guía** : si se beneficia de los indexadores y conjuntos de aptitudes en Cognitive Search, use etiquetas de servicio para representar un intervalo de direcciones IP con permiso para conectarse a recursos externos. 

Permita o deniegue el tráfico a los recursos especificando el nombre de la etiqueta de servicio (por ejemplo, AzureCognitiveSearch) en el campo adecuado de origen o destino de una regla. 

- [Etiquetas de servicio de red virtual](../virtual-network/service-tags-overview.md)

**Supervisión de Azure Security Center** : no disponible actualmente

**Responsabilidad** : Customer

### <a name="19-maintain-standard-security-configurations-for-network-devices"></a>1.9: Mantenga las configuraciones de seguridad estándar para dispositivos de red

**Guía** : Cognitive Search no tiene recursos de red ni depende de ellos por diseño. Las aplicaciones cliente y los orígenes de datos relacionados con la aplicación de búsqueda podrían estar en una red virtual, pero el servicio de búsqueda no se implementa por sí mismo en la red. 

**Supervisión de Azure Security Center** : No aplicable

**Responsabilidad** : No aplicable

### <a name="110-document-traffic-configuration-rules"></a>1.10: Documente las reglas de configuración de tráfico

**Guía** : puede configurar Cognitive Search con un punto de conexión privado de Azure para integrar el servicio de búsqueda con una red virtual.  Use etiquetas de recurso para grupos de seguridad de red y otros recursos relacionados con la seguridad de red y el flujo de tráfico. En el caso de las reglas de grupo de seguridad de red individuales, use el campo "Descripción" para documentar las reglas que permiten con origen y destino en una red. 

Use cualquiera de las definiciones de Azure Policy integradas relacionadas con el etiquetado, como efectos "Requerir etiqueta y su valor", para que todos los recursos se creen con etiquetas y para notificarle los recursos no etiquetados existentes. 

Puede usar Azure PowerShell o la CLI de Azure para buscar o realizar acciones en los recursos en función de sus etiquetas. 

- [Procedimiento para crear un punto de conexión privado en Cognitive Search](./service-create-private-endpoint.md)

- [Creación y uso de etiquetas](../azure-resource-manager/management/tag-resources.md)

- [Creación de una red virtual de Azure](../virtual-network/quick-create-portal.md)

- [Filtrado del tráfico de red con reglas de grupos de seguridad de red](../virtual-network/tutorial-filter-network-traffic.md)

**Supervisión de Azure Security Center** : no disponible actualmente

**Responsabilidad** : Customer

### <a name="111-use-automated-tools-to-monitor-network-resource-configurations-and-detect-changes"></a>1.11: Use herramientas automatizadas para supervisar las configuraciones de recursos de red y detectar cambios

**Guía** : Cognitive Search no tiene ningún componente de red, ni depende de ninguno, por lo que no se pueden supervisar las configuraciones de estos recursos.

**Supervisión de Azure Security Center** : No aplicable

**Responsabilidad** : No aplicable

## <a name="logging-and-monitoring"></a>Registro y supervisión

*Para más información, consulte [Azure Security Benchmark: registro y supervisión](../security/benchmarks/security-control-logging-monitoring.md).*

### <a name="21-use-approved-time-synchronization-sources"></a>2.1: Uso de orígenes de sincronización de hora aprobados

**Guía** : Cognitive Search no admite la configuración de sus propios orígenes de sincronización de hora. El servicio de búsqueda se basa en los orígenes de sincronización de hora de Microsoft y no se expone a los clientes para su configuración.

**Supervisión de Azure Security Center** : No aplicable

**Responsabilidad** : Microsoft

### <a name="22-configure-central-security-log-management"></a>2.2: Configuración de la administración central de registros de seguridad

**Guía** : realice la ingesta de registros relacionados con Cognitive Search a través de Azure Monitor para agregar datos de seguridad generados por dispositivos de punto de conexión, recursos de red y otros sistemas de seguridad. En Azure Monitor, use áreas de trabajo de Log Analytics para realizar consultas y análisis, y use cuentas de Azure Storage para el almacenamiento de archivos a largo plazo.
También puede habilitar e incorporar los datos en Azure Sentinel o en una herramienta SIEM de terceros.

- [Introducción a Azure Monitor e integración con herramienta SIEM de terceros](https://azure.microsoft.com/blog/use-azure-monitor-to-integrate-with-siem-tools/)

- [Recopilación de registros y métricas de plataforma con Azure Monitor](../azure-monitor/platform/diagnostic-settings.md)

- [Incorporación de Azure Sentinel](../sentinel/quickstart-onboard.md)

**Supervisión de Azure Security Center** : no disponible actualmente

**Responsabilidad** : Customer

### <a name="23-enable-audit-logging-for-azure-resources"></a>2.3: Habilitación del registro de auditoría para recursos de Azure

**Guía** : los registros operativos y de diagnóstico proporcionan información sobre las operaciones detalladas de Cognitive Search, y resultan útiles para supervisar el servicio y las cargas de trabajo que tienen acceso al servicio.  Para capturar datos de diagnóstico, habilite el registro especificando dónde se almacena la información de registro.

- [Procedimiento para recopilar y analizar datos de registro en Azure Cognitive Search](./search-monitor-logs.md)

- [Recopilación de registros y métricas de plataforma con Azure Monitor](../azure-monitor/platform/diagnostic-settings.md) 

**Supervisión de Azure Security Center** : Sí

**Responsabilidad** : Customer

### <a name="24-collect-security-logs-from-operating-systems"></a>2.4: Recopilación de registros de seguridad de sistemas operativos

**Guía** : no se aplica a Cognitive Search. esta recomendación está pensada para los recursos de proceso.

**Supervisión de Azure Security Center** : No aplicable

**Responsabilidad** : No aplicable

### <a name="25-configure-security-log-storage-retention"></a>2.5: Configuración de la retención del almacenamiento de registros de seguridad

**Guía** : los datos históricos que se envían a las métricas de diagnóstico se conservan de forma predeterminada en Cognitive Search durante 30 días. Para una retención más larga, asegúrese de habilitar la configuración que especifica una opción de almacenamiento con el fin de conservar las métricas y los eventos registrados.

En Azure Monitor, establezca el período de retención del área de trabajo de Log Analytics de acuerdo con la normativa de cumplimiento de su organización. Use cuentas de Azure Storage para el almacenamiento de archivos y a largo plazo. 

- [Cambio del período de retención de datos en Log Analytics](../azure-monitor/platform/manage-cost-storage.md#change-the-data-retention-period)

- [Configuración de la directiva de retención para los registros de la cuenta de Azure Storage](../storage/common/storage-monitor-storage-account.md#configure-logging)

**Supervisión de Azure Security Center** : Sí

**Responsabilidad** : Customer

### <a name="26-monitor-and-review-logs"></a>2.6: Supervisión y registros de revisión

**Guía** : analice y supervise los registros del servicio Cognitive Search en busca de algún comportamiento anómalo. Use Log Analytics de Azure Monitor para revisar los registros y realizar consultas en los datos de registro. Como alternativa, puede habilitar e incorporar datos en Azure Sentinel o en una herramienta SIEM de terceros.

- [Procedimiento para recopilar y analizar datos de registro en Cognitive Search](./search-monitor-logs.md)

- [Procedimiento para visualizar datos del registro de búsqueda en Power BI](./search-monitor-logs-powerbi.md)

- [Incorporación de Azure Sentinel](../sentinel/quickstart-onboard.md)

- [Información acerca de Log Analytics](../azure-monitor/log-query/get-started-portal.md)

- [Procedimiento para realizar consultas personalizadas en Azure Monitor](../azure-monitor/log-query/get-started-queries.md)

**Supervisión de Azure Security Center** : No aplicable

**Responsabilidad** : Customer

### <a name="27-enable-alerts-for-anomalous-activities"></a>2.7: Habilitación de alertas para actividades anómalas

**Guía** : use Security Center con el área de trabajo de Log Analytics para supervisar y generar alertas sobre actividades anómalas encontradas en los registros y eventos de seguridad. Como alternativa, puede habilitar e incorporar datos a Azure Sentinel.

- [Incorporación de Azure Sentinel](../sentinel/quickstart-onboard.md)

- [Administración de alertas de seguridad en Azure Security Center](../security-center/security-center-managing-and-responding-alerts.md)

- [Alertas sobre datos de registro de Log Analytics](../azure-monitor/learn/tutorial-response.md)

**Supervisión de Azure Security Center** : no disponible actualmente

**Responsabilidad** : Customer

### <a name="28-centralize-anti-malware-logging"></a>2.8: Centralización del registro antimalware

**Guía** : no se aplica a Cognitive Search. Microsoft administra la solución antimalware en la plataforma subyacente.

**Supervisión de Azure Security Center** : No aplicable

**Responsabilidad** : No aplicable

### <a name="29-enable-dns-query-logging"></a>2.9: Habilitación del registro de consultas DNS

**Guía** : no se aplica a Cognitive Search. No genera ni utiliza registros DNS.

**Supervisión de Azure Security Center** : No aplicable

**Responsabilidad** : No aplicable

### <a name="210-enable-command-line-audit-logging"></a>2.10: Habilitación del registro de auditoría de la línea de comandos

**Guía** : no se aplica a Cognitive Search. La auditoría de línea de comandos no está disponible para Cognitive Search.

**Supervisión de Azure Security Center** : No aplicable

**Responsabilidad** : No aplicable

## <a name="identity-and-access-control"></a>Control de identidades y acceso

*Para obtener más información, consulte [Azure Security Benchmark: Identidad y control de acceso](../security/benchmarks/security-control-identity-access-control.md).*

### <a name="31-maintain-an-inventory-of-administrative-accounts"></a>3.1: Mantenga un inventario de cuentas administrativas

**Guía** : El control de acceso basado en roles de Azure (RBAC de Azure) permite administrar el acceso a los recursos de Azure a través de las asignaciones de roles. Puede asignar estos roles a usuarios, grupos de entidades de servicio e identidades administradas. Hay roles integrados predefinidos para determinados recursos, y estos roles se pueden inventariar o consultar mediante herramientas como la CLI de Azure, Azure PowerShell o el Azure Portal.

Los roles de Cognitive Search están asociados a permisos que admiten tareas de administración de nivel de servicio.  Estos roles no otorgan acceso al punto de conexión de servicio. El acceso a las operaciones en el punto de conexión (tales como administración de índices, rellenado de índices y consultas en los datos de búsqueda) usa claves de API para autenticar la solicitud.

- [Configuración de roles para el acceso administrativo a Azure Cognitive Search](./search-security-rbac.md)

- [Creación y administración de claves de API para un servicio de Azure Cognitive Search](./search-security-api-keys.md)

- [Obtención de un rol de directorio en Azure AD con PowerShell](/powershell/module/azuread/get-azureaddirectoryrole)
- [Obtención de los miembros de un rol de directorio en Azure AD con PowerShell](/powershell/module/azuread/get-azureaddirectoryrolemember)

**Supervisión de Azure Security Center** : no disponible actualmente

**Responsabilidad** : Customer

### <a name="32-change-default-passwords-where-applicable"></a>3.2: Cambie las contraseñas predeterminadas cuando proceda

**Guía** : no se aplica a Cognitive Search. No tiene un concepto de contraseñas predeterminadas.

**Supervisión de Azure Security Center** : No aplicable

**Responsabilidad** : No aplicable

### <a name="33-use-dedicated-administrative-accounts"></a>3.3: Use cuentas administrativas dedicadas

**Guía** : Cognitive Search no tiene el concepto de que las cuentas de administrador de nivel local o de Azure Active Directory (Azure AD) se puedan usar para administrar índices y operaciones. 

Use los roles integrados de Azure AD, que deben asignarse explícitamente, en el caso de las operaciones de administración. Invoque el módulo de PowerShell de Azure AD para realizar consultas ad hoc a fin de detectar cuentas que son miembros de grupos administrativos.

- [Procedimiento para usar roles para el acceso administrativo en Cognitive Search](./search-security-rbac.md)

- [Obtención de un rol de directorio en Azure AD con PowerShell](/powershell/module/azuread/get-azureaddirectoryrole)

**Supervisión de Azure Security Center** : No aplicable

**Responsabilidad** : Customer

### <a name="34-use-single-sign-on-sso-with-azure-active-directory"></a>3.4: Use el inicio de sesión único (SSO) con Azure Active Directory

**Guía** : use la autenticación de SSO con Azure Active Directory (Azure AD) para acceder a la información del servicio de búsqueda sobre las operaciones de administración admitidas a través de Azure Resource Manager. 

Establezca un proceso para reducir el número de identidades y credenciales habilitando el inicio de sesión único del servicio con las identidades preexistentes de la organización.

- [Descripción del SSO con Azure AD](../active-directory/manage-apps/what-is-single-sign-on.md)

**Supervisión de Azure Security Center** : No aplicable

**Responsabilidad** : Customer

### <a name="35-use-multi-factor-authentication-for-all-azure-active-directory-based-access"></a>3.5: Use la autenticación multifactor para todo el acceso basado en Azure Active Directory

**Guía** : habilite la característica Multi-Factor Authentication (MFA) de Azure Active Directory (Azure AD) y siga las recomendaciones de identidad y acceso de Security Center.

- [Procedimiento para habilitar la MFA en Azure](../active-directory/authentication/howto-mfa-getstarted.md) 

- [Supervisión de la identidad y el acceso en Azure Security Center](../security-center/security-center-identity-access.md) 

**Supervisión de Azure Security Center** : No aplicable

**Responsabilidad** : Customer

### <a name="36-use-dedicated-machines-privileged-access-workstations-for-all-administrative-tasks"></a>3.6: Use máquinas dedicadas (estaciones de trabajo de acceso con privilegios) para todas las tareas administrativas

**Guía** : utilice una estación de acceso con privilegios (PAW) configurada con Multi-Factor Authentication (MFA) para iniciar sesión en los recursos de Azure y acceder a ellos.

- [Descripción de las estaciones de trabajo seguras administradas por Azure](../active-directory/devices/concept-azure-managed-workstation.md)
 

- [Cómo habilitar MFA de Azure AD](../active-directory/authentication/howto-mfa-getstarted.md)
 

**Supervisión de Azure Security Center** : No aplicable

**Responsabilidad** : Customer

### <a name="37-log-and-alert-on-suspicious-activities-from-administrative-accounts"></a>3.7: Registro y alerta de actividades sospechosas desde cuentas administrativas

**Guía** : use los informes de seguridad y la supervisión de Azure Active Directory (Azure AD) para detectar cuándo se producen actividades sospechosas o no seguras en el entorno. Use Azure Security Center para supervisar la actividad de identidad y acceso.

- [Procedimiento para identificar usuarios de Azure AD marcados por una actividad de riesgo](../active-directory/identity-protection/overview-identity-protection.md)

- [Supervisión de la actividad de identidad y acceso de los usuarios en Azure Security Center](../security-center/security-center-identity-access.md)

**Supervisión de Azure Security Center** : Sí

**Responsabilidad** : Customer

### <a name="38-manage-azure-resources-only-from-approved-locations"></a>3.8: Administración de los recursos de Azure solo desde ubicaciones aprobadas

**Guía** : no se aplica a Cognitive Search. No admite el uso de la ubicación aprobada como condición de acceso.

**Supervisión de Azure Security Center** : No aplicable

**Responsabilidad** : No aplicable

### <a name="39-use-azure-active-directory"></a>3.9: Uso de Azure Active Directory

**Guía** : use Azure Active Directory (Azure AD) como sistema central de autenticación y autorización para las tareas de administración del nivel de servicio en Azure Cognitive Search. Las identidades de Azure AD no conceden acceso al punto de conexión del servicio de búsqueda.  El acceso a operaciones tales como la administración de índices, el rellenado de índices y las consultas en los datos de búsqueda están disponibles a través de claves de API.

- [Procedimiento para crear y configurar una instancia de Azure AD](../active-directory/fundamentals/active-directory-access-create-new-tenant.md)

- [Creación y administración de claves de API para un servicio de Azure Cognitive Search](./search-security-api-keys.md)

**Supervisión de Azure Security Center** : no disponible actualmente

**Responsabilidad** : Customer

### <a name="310-regularly-review-and-reconcile-user-access"></a>3.10: Revise y concilie regularmente el acceso de los usuarios

**Instrucciones** : Azure Active Directory (Azure AD) proporciona registros para ayudar a descubrir cuentas obsoletas. Use las revisiones de acceso e identidades de Azure AD para administrar de forma eficiente las pertenencias a grupos, el acceso a las aplicaciones empresariales y las asignaciones de roles. El acceso de los usuarios se puede revisar de forma periódica para asegurarse de que solo las personas adecuadas tengan acceso continuado. 

Revise los registros de diagnóstico de Cognitive Search en busca de actividades tales como la administración de índices, el rellenado de índices y las consultas en el punto de conexión del servicio de búsqueda.

- [Descripción de los informes de Azure AD](../active-directory/reports-monitoring/index.yml)

- [Procedimiento para usar las revisiones de acceso e identidades de Azure AD](../active-directory/governance/access-reviews-overview.md)

- [Supervisión de operaciones y actividades de Azure Cognitive Search](./search-monitor-usage.md)

**Supervisión de Azure Security Center** : no disponible actualmente

**Responsabilidad** : Customer

### <a name="311-monitor-attempts-to-access-deactivated-credentials"></a>3.11: Supervisión de los intentos de acceso a credenciales desactivadas

**Guía** : el acceso a los orígenes del registro de eventos de actividades de inicio de sesión, auditoría y riesgo de Azure Active Directory (Azure AD) permite la integración en cualquier herramienta SIEM o de supervisión.

Para simplificar este proceso, cree una configuración de diagnóstico para las cuentas de usuario de Azure AD, y envíe los registros de auditoría y los registros de inicio de sesión a un área de trabajo de Log Analytics. Configure las alertas que quiera en el área de trabajo de Log Analytics.

- [Integración de los registros de actividad de Azure en Azure Monitor](../active-directory/reports-monitoring/howto-integrate-activity-logs-with-log-analytics.md) 

**Supervisión de Azure Security Center** : no disponible actualmente

**Responsabilidad** : Customer

### <a name="312-alert-on-account-login-behavior-deviation"></a>3.12: Alerte de las desviaciones de comportamiento en los inicios de sesión de las cuentas

**Instrucciones** : use las características de protección de identidad de Azure Active Directory (Azure AD) para configurar respuestas automatizadas a las acciones sospechosas que se detecten relacionadas con las identidades de los usuarios. Realice la ingesta de datos en Azure Sentinel para investigarlos más, según sea necesario.

- [Visualización de los inicios de sesión de riesgo de Azure AD](../active-directory/identity-protection/overview-identity-protection.md) 

- [Configuración y habilitación de las directivas de riesgo de protección de identidad](../active-directory/identity-protection/howto-identity-protection-configure-risk-policies.md) 

- [Incorporación de Azure Sentinel](../sentinel/quickstart-onboard.md) 

**Supervisión de Azure Security Center** : no disponible actualmente

**Responsabilidad** : Customer

### <a name="313-provide-microsoft-with-access-to-relevant-customer-data-during-support-scenarios"></a>3.13: Proporcione a Microsoft acceso a los datos pertinentes del cliente durante los escenarios de soporte técnico

**Guía** : no se aplica a Cognitive Search. La característica Caja de seguridad del cliente no admite Cognitive Search.

**Supervisión de Azure Security Center** : No aplicable

**Responsabilidad** : No aplicable

## <a name="data-protection"></a>Protección de los datos

*Para obtener más información, consulte [Azure Security Benchmark: Protección de datos](../security/benchmarks/security-control-data-protection.md).*

### <a name="41-maintain-an-inventory-of-sensitive-information"></a>4.1: Mantenimiento de un inventario de información confidencial

**Instrucciones** : use etiquetas para ayudar a realizar el seguimiento de los recursos de Azure que almacenan o procesan información confidencial.

**Supervisión de Azure Security Center** : no disponible actualmente

**Responsabilidad** : Customer

### <a name="42-isolate-systems-storing-or-processing-sensitive-information"></a>4.2: Aislamiento de los sistemas que almacenan o procesan información confidencial

**Instrucciones** : Implemente suscripciones y/o grupos de administración independientes para los entornos de desarrollo, prueba y producción. Los recursos deben separarse por red virtual o subred, etiquetarse adecuadamente y protegerse en un grupo de seguridad de red o Azure Firewall. Los recursos que almacenan o procesan datos confidenciales deben estar aislados. Use Private Link para configurar un punto de conexión privado en Cognitive Search.

- [Creación de suscripciones adicionales de Azure](../cost-management-billing/manage/create-subscription.md) 

- [Creación y uso de etiquetas](../azure-resource-manager/management/tag-resources.md)

- [Procedimiento para crear un punto de conexión privado en Cognitive Search](./service-create-private-endpoint.md)

**Supervisión de Azure Security Center** : no disponible actualmente

**Responsabilidad** : Customer

### <a name="43-monitor-and-block-unauthorized-transfer-of-sensitive-information"></a>4.3: Supervisión y bloqueo de una transferencia no autorizada de información confidencial

**Guía** : Use una solución de terceros de Azure Marketplace en los perímetros de la red para supervisar la transferencia no autorizada de información confidencial y bloquear dichas transferencias al tiempo que alerta a los profesionales de seguridad de la información.

Microsoft administra la plataforma subyacente y trata todo el contenido de los clientes como confidencial, y protege a los clientes contra la pérdida y exposición de los datos. Para garantizar la seguridad de los datos de los clientes dentro de Azure, Microsoft ha implementado y mantiene un conjunto de controles y funcionalidades eficaces de protección de datos.

- [Descripción de la protección de datos de los clientes en Azure](../security/fundamentals/protection-customer-data.md) 

**Supervisión de Azure Security Center** : No aplicable

**Responsabilidad** : Customer

### <a name="44-encrypt-all-sensitive-information-in-transit"></a>4.4: Cifrado de toda la información confidencial en tránsito

**Guía** : Cognitive Search cifra los datos en tránsito con Seguridad de la capa de transporte 1.2 y aplica siempre el cifrado (SSL/TLS) para todas las conexiones. Así se garantiza que todos los datos se cifran "en tránsito" entre el cliente y el servicio.

- [Descripción del cifrado en tránsito con Azure](../security/fundamentals/encryption-overview.md#encryption-of-data-in-transit) 

**Supervisión de Azure Security Center** : No aplicable

**Responsabilidad** : Microsoft

### <a name="45-use-an-active-discovery-tool-to-identify-sensitive-data"></a>4.5: Uso de una herramienta de detección activa para identificar datos confidenciales

**Guía** : las características de identificación, clasificación y prevención de pérdida de datos no están disponibles aún en Cognitive Search. Implemente una solución de terceros, si es necesario, para fines de cumplimiento. 

Microsoft administra la plataforma subyacente y trata todo el contenido de los clientes como confidencial, y protege a los clientes contra la pérdida y exposición de los datos. Para garantizar la seguridad de los datos de los clientes dentro de Azure, Microsoft ha implementado y mantiene un conjunto de controles y funcionalidades eficaces de protección de datos.

- [Descripción de la protección de datos de los clientes en Azure](../security/fundamentals/protection-customer-data.md)

**Supervisión de Azure Security Center** : No aplicable

**Responsabilidad** : Customer

### <a name="46-use-azure-rbac-to-manage-access-to-resources"></a>4.6: Uso de RBAC de Azure para administrar el acceso a los recursos

**Guía** : en cuanto a la administración de servicios, use el control de acceso basado en roles de Azure (RBAC de Azure) para administrar el acceso a las claves y a la configuración. En el caso de las operaciones de contenido, como la indexación y las consultas, Cognitive Search usa claves en lugar de un modelo de control de acceso basado en identidades. Use RBAC de Azure para controlar el acceso a las claves.
- [Configuración de RBAC en Azure](../role-based-access-control/role-assignments-portal.md) 

 
- [Procedimiento para configurar roles para el acceso administrativo a Cognitive Search](./search-security-rbac.md)

**Supervisión de Azure Security Center** : No aplicable

**Responsabilidad** : Customer

### <a name="47-use-host-based-data-loss-prevention-to-enforce-access-control"></a>4.7: Uso de la prevención de pérdida de datos basada en host para aplicar el control de acceso

**Guía** : no se aplica a Cognitive Search. Esta guía se destina a los recursos de proceso. 

Microsoft administra la infraestructura subyacente de Cognitive Search y ha implementado controles estrictos para evitar la pérdida o exposición de los datos de los clientes.

- [Descripción de la protección de datos de los clientes en Azure](../security/fundamentals/protection-customer-data.md)

**Supervisión de Azure Security Center** : No aplicable

**Responsabilidad** : Microsoft

### <a name="48-encrypt-sensitive-information-at-rest"></a>4.8: Cifrado de información confidencial en reposo

**Guía** : Cognitive Search cifra automáticamente el contenido indexado en reposo con claves administradas por Microsoft. Si se necesita más protección, puede complementar el cifrado predeterminado con un segundo nivel de cifrado mediante las claves que se crean y administran en Azure Key Vault.

- [Configuración de claves administradas por el cliente para el cifrado de datos en Azure Cognitive Search](./search-security-manage-encryption-keys.md)

- [Descripción del cifrado en reposo en Azure](../security/fundamentals/encryption-atrest.md)

**Supervisión de Azure Security Center** : no disponible actualmente

**Responsabilidad** : Compartido

### <a name="49-log-and-alert-on-changes-to-critical-azure-resources"></a>4.9: Registro y alerta de cambios en los recursos críticos de Azure

**Guía** : use Azure Monitor con el registro de actividad de Azure a fin de crear alertas cuando se produzcan cambios en las instancias de producción de Cognitive Search y otros recursos críticos o relacionados.

- [Creación de alertas para los eventos del registro de actividad de Azure](../azure-monitor/platform/alerts-activity-log.md)

- [Procedimiento para crear alertas sobre actividades de Cognitive Search](./search-monitor-logs.md)

**Supervisión de Azure Security Center** : Sí

**Responsabilidad** : Customer

## <a name="vulnerability-management"></a>Administración de vulnerabilidades

*Para obtener más información, consulte [Azure Security Benchmark: Administración de vulnerabilidades](../security/benchmarks/security-control-vulnerability-management.md).*

### <a name="51-run-automated-vulnerability-scanning-tools"></a>5.1: Ejecute herramientas de análisis de vulnerabilidades automatizado

**Guía** : actualmente no está disponible para Cognitive Search.  En el caso de los clústeres que almacenan el contenido del servicio de búsqueda, Microsoft es responsable de la administración de vulnerabilidades de esos clústeres.

**Supervisión de Azure Security Center** : no disponible actualmente

**Responsabilidad** : Microsoft

### <a name="52-deploy-automated-operating-system-patch-management-solution"></a>5.2: Implemente una solución de administración de revisiones de sistema operativo automatizada

**Guía** : no se aplica a Cognitive Search. esta recomendación está pensada para los recursos de proceso.

**Supervisión de Azure Security Center** : No aplicable

**Responsabilidad** : No aplicable

### <a name="53-deploy-an-automated-patch-management-solution-for-third-party-software-titles"></a>5.3: Implementación de una solución de administración de revisiones automatizada de títulos de software de terceros

**Guía** : no se aplica a Cognitive Search. esta recomendación está pensada para los recursos de proceso.

**Supervisión de Azure Security Center** : No aplicable

**Responsabilidad** : No aplicable

### <a name="54-compare-back-to-back-vulnerability-scans"></a>5.4: Compare los exámenes de vulnerabilidades opuestos

**Guía** : no se aplica a Cognitive Search. Microsoft realiza la administración de vulnerabilidades en los sistemas subyacentes que admiten servicios de Cognitive Search.

**Supervisión de Azure Security Center** : No aplicable

**Responsabilidad** : Microsoft

### <a name="55-use-a-risk-rating-process-to-prioritize-the-remediation-of-discovered-vulnerabilities"></a>5.5: Use un proceso de clasificación de riesgos para priorizar la corrección de las vulnerabilidades detectadas

**Guía** : no se aplica a Cognitive Search. No tiene ningún sistema de puntuación o clasificación de riesgo estándar para los resultados del examen de vulnerabilidades.

**Supervisión de Azure Security Center** : No aplicable

**Responsabilidad** : No aplicable

## <a name="inventory-and-asset-management"></a>Inventario y administración de recursos

*Para obtener más información, consulte [Azure Security Benchmark: Administración de recursos y del inventario](../security/benchmarks/security-control-inventory-asset-management.md).*

### <a name="61-use-automated-asset-discovery-solution"></a>6.1: Uso de la solución de detección de recursos automatizada

**Guía** : use Azure Resource Graph para consultar y detectar todos los recursos (por ejemplo, proceso, almacenamiento, red, puertos y protocolos, etc.) en las suscripciones.  

Asegúrese de que tiene los permisos adecuados (lectura) en el inquilino y enumere todas las suscripciones de Azure, así como los recursos en las suscripciones.  

- [Creación de consultas con Azure Resource Graph Explorer](../governance/resource-graph/first-query-portal.md) 

- [Visualización de las suscripciones de Azure](/powershell/module/az.accounts/get-azsubscription) 

- [Descripción de Azure RBAC](../role-based-access-control/overview.md)

**Supervisión de Azure Security Center** : No aplicable

**Responsabilidad** : Customer

### <a name="62-maintain-asset-metadata"></a>6.2: Mantenimiento de metadatos de recursos

**Guía** : aplique etiquetas a los recursos de Azure con metadatos para organizarlos de forma lógica en una taxonomía.

- [Creación y uso de etiquetas](../azure-resource-manager/management/tag-resources.md)

**Supervisión de Azure Security Center** : No aplicable

**Responsabilidad** : Customer

### <a name="63-delete-unauthorized-azure-resources"></a>6.3: Eliminación de recursos de Azure no autorizados

**Guía** : Use el etiquetado, los grupos de administración y las suscripciones independientes, si procede, para organizar y realizar un seguimiento de los recursos. Concilie el inventario periódicamente y asegúrese de que los recursos no autorizados se eliminan de la suscripción de manera oportuna.
- [Creación de suscripciones adicionales de Azure](../cost-management-billing/manage/create-subscription.md) 

- [Creación de grupos de administración](../governance/management-groups/create-management-group-portal.md) 

- [Creación y uso de etiquetas](../azure-resource-manager/management/tag-resources.md) 

**Supervisión de Azure Security Center** : No aplicable

**Responsabilidad** : Customer

### <a name="64-define-and-maintain-an-inventory-of-approved-azure-resources"></a>6.4: Definición y mantenimiento de un inventario de los recursos de Azure aprobados

**Guía** : defina una lista de recursos de Azure aprobados relacionados con la indexación y el procesamiento de conjuntos de aptitudes en Cognitive Search.

**Supervisión de Azure Security Center** : No aplicable

**Responsabilidad** : Customer

### <a name="65-monitor-for-unapproved-azure-resources"></a>6.5: Supervisión de recursos de Azure no aprobados

**Guía** : se recomienda definir un inventario de recursos de Azure que se hayan aprobado para su uso según las directivas y los estándares anteriores de la organización; luego, supervise los recursos de Azure no aprobados con Azure Policy o Azure Resource Graph.

- [Configuración y administración de Azure Policy](../governance/policy/tutorials/create-and-manage.md) 

- [Creación de consultas con Azure Graph](../governance/resource-graph/first-query-portal.md) 

**Supervisión de Azure Security Center** : No aplicable

**Responsabilidad** : Customer

### <a name="66-monitor-for-unapproved-software-applications-within-compute-resources"></a>6.6: Supervisión de aplicaciones de software no aprobadas en recursos de proceso

**Guía** : no se aplica a Cognitive Search. Esta guía se destina a los recursos de proceso.

Se recomienda tener un inventario de las aplicaciones de software que se han considerado aprobadas según las directivas de la organización y los estándares de seguridad, además de supervisar los títulos de software no aprobados instalados en los recursos de proceso de Azure.

**Supervisión de Azure Security Center** : No aplicable

**Responsabilidad** : No aplicable

### <a name="67-remove-unapproved-azure-resources-and-software-applications"></a>6.7: Eliminación de aplicaciones de software y recursos de Azure no aprobadas

**Guía** : no se aplica a Cognitive Search. esta recomendación está pensada para los recursos de proceso.

**Supervisión de Azure Security Center** : No aplicable

**Responsabilidad** : No aplicable

### <a name="68-use-only-approved-applications"></a>6.8: Uso exclusivo de aplicaciones aprobadas

**Guía** : no se aplica a Cognitive Search. No expone ningún recurso de proceso ni permite la instalación de aplicaciones de software en ninguno de sus recursos.

**Supervisión de Azure Security Center** : No aplicable

**Responsabilidad** : No aplicable

### <a name="69-use-only-approved-azure-services"></a>6.9: Uso exclusivo de servicios de Azure aprobados

**Guía** : use Azure Policy para establecer restricciones en el tipo de recursos que se pueden crear en las suscripciones del cliente con las siguientes definiciones de directiva integradas:

- Tipos de recursos no permitidos
- Tipos de recursos permitidos

Use Azure Resource Graph para consultar o detectar recursos dentro de las suscripciones. Asegúrese de que todos los recursos de Azure presentes en el entorno estén aprobados.

- [Configuración y administración de Azure Policy](../governance/policy/tutorials/create-and-manage.md) 

- [Denegación de un tipo de recurso específico con Azure Policy](../governance/policy/samples/index.md) 

**Supervisión de Azure Security Center** : no disponible actualmente

**Responsabilidad** : Customer

### <a name="610-maintain-an-inventory-of-approved-software-titles"></a>6.10: Mantenimiento de un inventario de títulos de software aprobados

**Guía** : no se aplica a Cognitive Search. Esta recomendación se destina a las aplicaciones que se ejecutan en recursos de proceso.

**Supervisión de Azure Security Center** : No aplicable

**Responsabilidad** : No aplicable

### <a name="611-limit-users-ability-to-interact-with-azure-resource-manager"></a>6.11: Limitación de la capacidad de los usuarios para interactuar con Azure Resource Manager

**Guía** : en cuanto a la administración de servicios, use el acceso condicional de Azure para limitar la capacidad de los usuarios de interactuar con Azure Resource Manager mediante la configuración de la opción "Bloquear acceso" en la aplicación "Administración de Microsoft Azure". 

Controle el acceso a las claves utilizadas para autenticar las solicitudes de todas las demás operaciones, especialmente las relacionadas con el contenido en Cognitive Search.

**Supervisión de Azure Security Center** : no disponible actualmente

**Responsabilidad** : Customer

### <a name="612-limit-users-ability-to-execute-scripts-in-compute-resources"></a>6.12: Limitación de la capacidad de los usuarios para ejecutar scripts en los recursos de proceso

**Guía** : no se aplica a Cognitive Search. esta recomendación está pensada para los recursos de proceso.

**Supervisión de Azure Security Center** : No aplicable

**Responsabilidad** : No aplicable

### <a name="613-physically-or-logically-segregate-high-risk-applications"></a>6.13: Segregación física o lógica de aplicaciones de alto riesgo

**Guía** : no se aplica a Cognitive Search. Esta recomendación se destina a las aplicaciones web que se ejecutan en Azure App Service o en recursos de proceso.

**Supervisión de Azure Security Center** : No aplicable

**Responsabilidad** : No aplicable

## <a name="secure-configuration"></a>Configuración segura

*Para obtener más información, consulte [Azure Security Benchmark: Configuración segura](../security/benchmarks/security-control-secure-configuration.md).*

### <a name="71-establish-secure-configurations-for-all-azure-resources"></a>7.1: Establezca configuraciones seguras para todos los recursos de Azure

**Guía** : use alias de Azure Policy en el espacio de nombres "Microsoft.Search" para crear directivas personalizadas con el fin de auditar o aplicar la configuración de los recursos de Azure Cognitive Search. También puede usar definiciones de Azure Policy integradas para servicios de Cognitive Search, como:

- Habilitación del registro de auditoría para recursos de Azure

Azure Resource Manager tiene la capacidad de exportar la plantilla en notación de objetos JavaScript (JSON), que se debe revisar para asegurarse de que las configuraciones cumplan los requisitos de seguridad de la organización. 

También puede usar las recomendaciones de Azure Security Center como línea de base de configuración segura para los recursos de Azure. 

- [Controles de Cumplimiento normativo de Azure Policy para Azure Cognitive Search](./security-controls-policy.md)

- [Visualización de los alias de Azure Policy disponibles](/powershell/module/az.resources/get-azpolicyalias)

**Supervisión de Azure Security Center** : no disponible actualmente

**Responsabilidad** : Customer

### <a name="72-establish-secure-operating-system-configurations"></a>7.2: Establezca configuraciones del sistema operativo seguras

**Guía** : no se aplica a Cognitive Search. esta recomendación está pensada para los recursos de proceso.

**Supervisión de Azure Security Center** : No aplicable

**Responsabilidad** : No aplicable

### <a name="73-maintain-secure-azure-resource-configurations"></a>7.3: Mantenga configuraciones de recursos de Azure seguras

**Guía** : use los efectos [deny] y [deployifnotexist] de Azure Policy para aplicar la configuración segura en los recursos del servicio Cognitive Search. 

Se pueden usar plantillas de Azure Resource Manager para mantener la configuración de seguridad de los recursos de Azure que requiere su organización. 

- [Descripción de los efectos de Azure Policy](../governance/policy/concepts/effects.md)

- [Controles de Cumplimiento normativo de Azure Policy para Azure Cognitive Search](./security-controls-policy.md)

- [Creación y administración de directivas para aplicar el cumplimiento](../governance/policy/tutorials/create-and-manage.md)

- [Información general sobre plantillas de Azure Resource Manager](../azure-resource-manager/templates/overview.md)

**Supervisión de Azure Security Center** : No aplicable

**Responsabilidad** : Customer

### <a name="74-maintain-secure-operating-system-configurations"></a>7.4: Mantenga configuraciones del sistema operativo seguras

**Guía** : no se aplica a Cognitive Search. esta recomendación está pensada para los recursos de proceso.

**Supervisión de Azure Security Center** : No aplicable

**Responsabilidad** : No aplicable

### <a name="75-securely-store-configuration-of-azure-resources"></a>7.5: Almacene de forma segura la configuración de los recursos de Azure

**Guía** : Si usa definiciones de personalizadas de Azure Policy, use Azure DevOps o Azure Repos para almacenar y administrar el código de forma segura.

- [Almacenamiento de código en Azure DevOps](/azure/devops/repos/git/gitworkflow)

- [Documentación de Azure Repos](/azure/devops/repos/index)

**Supervisión de Azure Security Center** : No aplicable

**Responsabilidad** : Customer

### <a name="76-securely-store-custom-operating-system-images"></a>7.6: Almacene imágenes de sistema operativo personalizadas de forma segura

**Guía** : no se aplica a Cognitive Search. esta recomendación está pensada para los recursos de proceso.

**Supervisión de Azure Security Center** : No aplicable

**Responsabilidad** : No aplicable

### <a name="77-deploy-configuration-management-tools-for-azure-resources"></a>7.7: Implementación de herramientas de administración de configuración para recursos de Azure

**Guía** : defina e implemente configuraciones de seguridad estándar para los recursos del servicio Cognitive Search con Azure Policy. 

Use alias para crear directivas personalizadas con el fin de auditar o aplicar las configuraciones de red. También puede usar definiciones de directivas integradas relacionadas con recursos concretos. 

Además, puede usar Azure Automation para implementar cambios de configuración y administrar excepciones de directivas. 

- [Controles de Cumplimiento normativo de Azure Policy para Azure Cognitive Search](./security-controls-policy.md)

- [Configuración y administración de Azure Policy](../governance/policy/tutorials/create-and-manage.md)

**Supervisión de Azure Security Center** : No aplicable

**Responsabilidad** : Customer

### <a name="78-deploy-configuration-management-tools-for-operating-systems"></a>7.8: Implementación de herramientas de administración de la configuración para sistemas operativos

**Guía** : no se aplica a Cognitive Search. esta recomendación está pensada para los recursos de proceso.

**Supervisión de Azure Security Center** : No aplicable

**Responsabilidad** : No aplicable

### <a name="79-implement-automated-configuration-monitoring-for-azure-resources"></a>7.9: Implementación de la supervisión de la configuración automatizada para los recursos de Azure

**Guía** : use Security Center para realizar análisis de base de referencia de los recursos del servicio Cognitive Search.  Además, use Azure Policy para alertar y auditar las configuraciones de recursos. 

- [Corrección de recomendaciones en Azure Security Center](../security-center/security-center-remediate-recommendations.md)

- [Controles de Cumplimiento normativo de Azure Policy para Azure Cognitive Search](./security-controls-policy.md)

**Supervisión de Azure Security Center** : Sí

**Responsabilidad** : Customer

### <a name="710-implement-automated-configuration-monitoring-for-operating-systems"></a>7.10: Implemente la supervisión de configuración automatizada para sistemas operativos

**Guía** : no se aplica a Cognitive Search. esta recomendación está pensada para los recursos de proceso.

**Supervisión de Azure Security Center** : No aplicable

**Responsabilidad** : No aplicable

### <a name="711-manage-azure-secrets-securely"></a>7.11: Administre los secretos de Azure de forma segura

**Guía** : use identidades administradas de Azure junto con Azure Key Vault para simplificar la administración de secretos de las aplicaciones en la nube.
- [Uso de identidades administradas para recursos de Azure](../azure-app-configuration/howto-integrate-azure-managed-service-identity.md) 
- [Creación de un almacén de claves](../key-vault/secrets/quick-create-portal.md) 

- [Cómo proporcionar la autenticación de Key Vault con una identidad administrada](../key-vault/general/assign-access-policy-portal.md) 

**Supervisión de Azure Security Center** : Sí

**Responsabilidad** : Customer

### <a name="712-manage-identities-securely-and-automatically"></a>7.12: Administre las identidades de forma segura y automática

**Guía** : use una identidad administrada de Azure para conceder a Cognitive Search acceso a otros servicios de Azure, como Key Vault y orígenes de datos de indexador, mediante una identidad administrada automáticamente en Azure Active Directory (Azure AD). Las identidades administradas le permiten autenticarse en cualquier servicio que admita la autenticación de Azure AD, incluido Azure Key Vault, sin necesidad de credenciales en el código. 

- [Configuración de una conexión de indexador a un origen de datos mediante una identidad administrada](./search-howto-managed-identities-data-sources.md)

- [Configuración de claves administradas por el cliente para el cifrado de datos mediante una identidad administrada](./search-security-manage-encryption-keys.md)

**Supervisión de Azure Security Center** : no disponible actualmente

**Responsabilidad** : Customer

### <a name="713-eliminate-unintended-credential-exposure"></a>7.13: Elimine la exposición de credenciales no intencionada

**Guía** : no se aplica a Cognitive Search. No hospeda código y no tiene credenciales que identificar.

**Supervisión de Azure Security Center** : No aplicable

**Responsabilidad** : No aplicable

## <a name="malware-defense"></a>Defensa contra malware

*Para obtener más información, consulte [Azure Security Benchmark: defensa contra malware](../security/benchmarks/security-control-malware-defense.md).*

### <a name="81-use-centrally-managed-antimalware-software"></a>8.1: Uso de software antimalware administrado centralmente

**Guía** : no se aplica a Cognitive Search. esta recomendación está pensada para los recursos de proceso.

Microsoft Antimalware está habilitado en el host subyacente que admite los servicios de Azure (por ejemplo, Azure Cognitive Search), pero no se ejecuta en el contenido del cliente.

**Supervisión de Azure Security Center** : No aplicable

**Responsabilidad** : No aplicable

### <a name="82-pre-scan-files-to-be-uploaded-to-non-compute-azure-resources"></a>8.2: Examine previamente los archivos que se van a cargar en recursos de Azure que no son de proceso

**Guía** : examine previamente el contenido que se carga en recursos de Azure que no sean de proceso, como Cognitive Search, Blob Storage, Azure SQL Database, etc. 

Es su responsabilidad realizar un análisis previo del contenido que se carga en recursos de Azure que no son de proceso. Microsoft no puede acceder a los datos del cliente y, por lo tanto, no puede realizar exámenes antimalware del contenido del cliente en su nombre.

**Supervisión de Azure Security Center** : No aplicable

**Responsabilidad** : Customer

### <a name="83-ensure-antimalware-software-and-signatures-are-updated"></a>8.3: Garantía de la actualización del software y las firmas antimalware

**Guía** : no se aplica a Cognitive Search. No permite que se instalen soluciones antimalware en sus recursos. En la plataforma subyacente, Microsoft controla la actualización de cualquier software antimalware y las firmas. 

En el caso de los recursos de proceso que son propiedad de su organización y se usan en la solución de búsqueda, siga las recomendaciones de Proceso y aplicaciones de Security Center para asegurarse de que todos los puntos de conexión estén actualizados con las firmas más recientes. Para Linux, use una solución antimalware de terceros.

**Supervisión de Azure Security Center** : No aplicable

**Responsabilidad** : Compartido

## <a name="data-recovery"></a>Recuperación de datos

*Para obtener más información, consulte [Azure Security Benchmark: recuperación de datos](../security/benchmarks/security-control-data-recovery.md).*

### <a name="91-ensure-regular-automated-back-ups"></a>9.1: Garantía de copias de seguridad automáticas periódicas

**Guía** : no se puede realizar una copia de seguridad del contenido almacenado en un servicio de búsqueda a través de Azure Backup o cualquier otro mecanismo integrado, pero se puede volver a generar un índice a partir del código fuente de la aplicación y de los orígenes de datos principales, o bien crear una herramienta personalizada para recuperar y almacenar el contenido indexado.

- [Ejemplo de copia de seguridad y restauración de índice de GitHub](https://github.com/Azure-Samples/azure-search-dotnet-samples/tree/master/index-backup-restore)

**Supervisión de Azure Security Center** : No aplicable

**Responsabilidad** : Customer

### <a name="92-perform-complete-system-backups-and-backup-any-customer-managed-keys"></a>9.2: Copias de seguridad completas del sistema y copia de seguridad de las claves administradas por el cliente

**Guía** : Cognitive Search no admite actualmente la copia de seguridad automatizada de los datos en un servicio de búsqueda y debe realizarse una copia de seguridad a través de un proceso manual.  También puede hacer una copia de seguridad de las claves administradas por el cliente en Azure Key Vault. 

- [Copia de seguridad y restauración de un índice de Azure Cognitive Search](/samples/azure-samples/azure-search-dotnet-samples/azure-search-backup-restore-index/)

- [Copias de seguridad de las claves de Key Vault en Azure](/powershell/module/azurerm.keyvault/backup-azurekeyvaultkey)

**Supervisión de Azure Security Center** : No aplicable

**Responsabilidad** : Customer

### <a name="93-validate-all-backups-including-customer-managed-keys"></a>9.3: Validación de todas las copias de seguridad, incluidas las claves administradas por el cliente

**Guía** : Cognitive Search no admite actualmente la copia de seguridad automatizada de los datos en un servicio de búsqueda; la copia de seguridad y la restauración deben efectuarse por medio de un proceso manual.  Realice periódicamente la restauración de datos del contenido del que haya realizado una copia de seguridad manualmente para garantizar la integridad de un extremo a otro del proceso de copia de seguridad.

- [Copia de seguridad y restauración de un índice de Azure Cognitive Search](/samples/azure-samples/azure-search-dotnet-samples/azure-search-backup-restore-index/)

- [Restauración de las claves de Key Vault en Azure](/powershell/module/azurerm.keyvault/restore-azurekeyvaultkey)

**Supervisión de Azure Security Center** : No aplicable

**Responsabilidad** : Customer

### <a name="94-ensure-protection-of-backups-and-customer-managed-keys"></a>9.4: Garantía de la protección de las copias de seguridad y las claves administradas por el cliente

**Guía** : Cognitive Search no admite actualmente la copia de seguridad automatizada de los datos en un servicio de búsqueda; la copia de seguridad debe efectuarse por medio de un proceso manual.  También puede hacer una copia de seguridad de las claves administradas por el cliente en Azure Key Vault. 

Habilite la eliminación temporal y la protección de purga en Key Vault para proteger las claves contra la eliminación accidental o malintencionada. Si se usa Azure Storage para almacenar las copias de seguridad anuales, habilite la eliminación temporal para guardar y recuperar los datos cuando se eliminen blobs o instantáneas de blobs. 

- [Copia de seguridad y restauración de un índice de Azure Cognitive Search](/samples/azure-samples/azure-search-dotnet-samples/azure-search-backup-restore-index/)

- [Procedimiento para habilitar la eliminación temporal y la protección de purga en Key Vault](../storage/blobs/soft-delete-blob-overview.md?tabs=azure-portal)

- [Eliminación temporal para Azure Blob Storage](../storage/blobs/soft-delete-blob-overview.md?tabs=azure-portal)

**Supervisión de Azure Security Center** : no disponible actualmente

**Responsabilidad** : Customer

## <a name="incident-response"></a>Respuesta a los incidentes

*Para obtener más información, consulte [Azure Security Benchmark: Respuesta a los incidentes](../security/benchmarks/security-control-incident-response.md).*

### <a name="101-create-an-incident-response-guide"></a>10.1: Creación de una guía de respuesta ante incidentes

**Guía** : desarrolle una guía de respuesta a incidentes para su organización. Asegúrese de que haya planes de respuesta a incidentes escritos que definan todos los roles del personal, así como las fases de administración y gestión de los incidentes, desde la detección hasta la revisión posterior a la incidencia.

- [Guía para crear su propio proceso de respuesta a incidentes de seguridad](https://msrc-blog.microsoft.com/2019/07/01/inside-the-msrc-building-your-own-security-incident-response-process/)

- [Anatomía de un incidente del Centro de respuestas de seguridad de Microsoft](https://msrc-blog.microsoft.com/2019/07/01/inside-the-msrc-building-your-own-security-incident-response-process/)

- [El cliente también puede usar la guía de control de incidentes de seguridad de equipos de NIST como referencia para la creación de su propio plan de respuesta a incidentes](https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-61r2.pdf)

**Supervisión de Azure Security Center** : No aplicable

**Responsabilidad** : Customer

### <a name="102-create-an-incident-scoring-and-prioritization-procedure"></a>10.2: Creación de un procedimiento de priorización y puntuación de incidentes

**Instrucciones** : Security Center asigna una gravedad a cada alerta para ayudarle a priorizar aquellas que se deben investigar en primer lugar. La gravedad se basa en la confianza que tiene Security Center en la búsqueda o en el análisis utilizados para emitir la alerta, así como en el nivel de confianza de que ha habido un intento malintencionado detrás de la actividad que ha provocado la alerta.

Adicionalmente, marque las suscripciones con etiquetas y cree un sistema de nomenclatura para identificar y clasificar los recursos de Azure, especialmente los que procesan datos confidenciales. Es su responsabilidad asignar prioridades a la corrección de las alertas en función de la importancia de los recursos y el entorno de Azure donde se produjo el incidente.

- [Uso de etiquetas para organizar los recursos de Azure](../azure-resource-manager/management/tag-resources.md)

- [Alertas de seguridad en el Centro de seguridad de Azure](../security-center/security-center-alerts-overview.md)

**Supervisión de Azure Security Center** : Sí

**Responsabilidad** : Customer

### <a name="103-test-security-response-procedures"></a>10.3: Prueba de los procedimientos de respuesta de seguridad

**Instrucciones** : Realice ejercicios para probar las capacidades de respuesta a los incidentes de los sistemas con regularidad. Identifique puntos débiles y brechas y revise el plan según sea necesario.

- [Consulte la publicación de NIST: "Guía para probar, entrenar y ejecutar programas para planes y funcionalidades de TI"](https://nvlpubs.nist.gov/nistpubs/Legacy/SP/nistspecialpublication800-84.pdf).

**Supervisión de Azure Security Center** : No aplicable

**Responsabilidad** : Customer

### <a name="104-provide-security-incident-contact-details-and-configure-alert-notifications-for-security-incidents"></a>10.4: Provisión de detalles de contacto de incidentes de seguridad y configuración de notificaciones de alerta para incidentes de seguridad

**Instrucciones** : La información de contacto del incidente de seguridad la utilizará Microsoft para ponerse en contacto con usted si Microsoft Security Response Center (MSRC) detecta que un tercero no autorizado o ilegal ha accedido a los datos. Revise los incidentes después del hecho para asegurarse de que se resuelven los problemas.

- [Establecimiento del contacto de seguridad de Azure Security Center](../security-center/security-center-provide-security-contact-details.md)

**Supervisión de Azure Security Center** : Sí

**Responsabilidad** : Customer

### <a name="105-incorporate-security-alerts-into-your-incident-response-system"></a>10.5: Incorporación de alertas de seguridad en el sistema de respuesta a incidentes

**Guía** : exporte sus alertas y recomendaciones de Security Center mediante la característica de exportación continua. La exportación continua permite exportar alertas y recomendaciones de forma manual o continua. Puede usar el conector de datos de Security Center para transmitir las alertas a Azure Sentinel.

- [Configuración de la exportación continua](../security-center/continuous-export.md)

- [Transmisión de alertas a Azure Sentinel](../sentinel/connect-azure-security-center.md)

**Supervisión de Azure Security Center** : No aplicable

**Responsabilidad** : Customer

### <a name="106-automate-the-response-to-security-alerts"></a>10.6: Automatización de la respuesta a las alertas de seguridad

**Instrucciones** : use la característica de automatización del flujo de trabajo de Azure Security Center para desencadenar automáticamente respuestas mediante "Logic Apps" en las alertas y recomendaciones de seguridad.

- [Configuración de la automatización de flujo de trabajo y Logic Apps](../security-center/workflow-automation.md)

**Supervisión de Azure Security Center** : no disponible actualmente

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

- Consulte la [prueba comparativa de seguridad de Azure](../security/benchmarks/overview.md).
- Obtenga más información sobre las [líneas de base de seguridad de Azure](../security/benchmarks/security-baselines-overview.md).
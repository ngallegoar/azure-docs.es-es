---
title: Línea de referencia de seguridad de Azure para Azure Databricks
description: Línea de referencia de seguridad de Azure para Azure Databricks
author: msmbaldwin
ms.service: security
ms.topic: conceptual
ms.date: 05/04/2020
ms.author: mbaldwin
ms.custom: security-benchmark
ms.openlocfilehash: 7d417f412592d87dec751b9d98a26bf786c719e5
ms.sourcegitcommit: e0330ef620103256d39ca1426f09dd5bb39cd075
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/05/2020
ms.locfileid: "82793920"
---
# <a name="azure-security-baseline-for-azure-databricks"></a>Línea de referencia de seguridad de Azure para Azure Databricks

La línea de referencia de seguridad de Azure para Azure Databricks contiene recomendaciones que le ayudarán a mejorar la situación de seguridad de la implementación.

La base de referencia de este servicio se extrae de la [versión 1.0 de Azure Security Benchmark](https://docs.microsoft.com/azure/security/benchmarks/overview), que proporciona recomendaciones sobre cómo puede proteger las soluciones en la nube en Azure con nuestra guía de procedimientos recomendados.

Para más información, consulte [Introducción a la línea de base de seguridad de Azure](https://docs.microsoft.com/azure/security/benchmarks/security-baselines-overview).

## <a name="network-security"></a>Seguridad de las redes

*Para obtener más información, consulte [Control de seguridad: seguridad de redes](https://docs.microsoft.com/azure/security/benchmarks/security-control-network-security).*

### <a name="11-protect-resources-using-network-security-groups-or-azure-firewall-on-your-virtual-network"></a>1.1: Proteja los recursos mediante grupos de seguridad de red o Azure Firewall en su red virtual

**Guía**: implemente Azure Databricks en su propia red virtual de Azure Virtual Network (VNet). La implementación predeterminada de Azure Databricks es un servicio totalmente administrado en Azure: todos los recursos del plano de datos, incluida una red virtual con la que están asociados todos los clústeres, se implementan en un grupo de recursos bloqueados. Sin embargo, si necesita personalizar la red, puede implementar los recursos del plano de datos de Azure Databricks en su propia red virtual (también llamado inyección VNet), lo que le permitirá implementar configuraciones de red personalizadas. Puede aplicar su propio grupo de seguridad de red (NSG) con reglas personalizadas a restricciones de tráfico de salida específicas.

Además, puede configurar reglas de NSG para indicar las restricciones de tráfico de salida en la subred en la que se implementará la instancia de Azure Databricks. Azure Firewall se puede configurar para las áreas de trabajo insertadas en la red virtual.

* [Implementación de Azure Databricks en su propia red virtual](https://docs.microsoft.com/azure/databricks/administration-guide/cloud-configurations/azure/vnet-inject)

* [Administración de grupos de seguridad de red](https://docs.microsoft.com/azure/virtual-network/manage-network-security-group)

**Supervisión de Azure Security Center**: no disponible actualmente

**Responsabilidad**: Customer

### <a name="12-monitor-and-log-the-configuration-and-traffic-of-vnets-subnets-and-nics"></a>1.2: Supervisión y registro de la configuración y el tráfico de redes virtuales, subredes y NIC

**Guía**: implemente Azure Databricks en su propia red virtual de Azure Virtual Network (VNet). No se crea automáticamente un grupo de seguridad de red (NSG). Debe crear un NSG de línea de base que tenga solo las reglas predeterminadas de Azure. Al implementar un área de trabajo, se agregan las reglas que requiere Databricks. También puede empezar con un NSG vacío, y las reglas adecuadas se agregarán automáticamente. Habilite los registros de flujo de NSG y envíe registros a una cuenta de almacenamiento para las auditorías de tráfico. También puede enviar registros de flujo a un área de trabajo de Log Analytics y usar el Análisis de tráfico para proporcionar información detallada sobre el flujo de tráfico en la nube de Azure. Algunas de las ventajas del Análisis de tráfico son la capacidad de visualizar la actividad de la red e identificar las zonas activas, identificar las amenazas de seguridad, comprender los patrones de flujo de tráfico y detectar configuraciones de red incorrectas.

* [Implementación de Azure Databricks en su propia red virtual](https://docs.microsoft.com/azure/databricks/administration-guide/cloud-configurations/azure/vnet-inject)

* [Habilitación de los registros de flujo de NSG](https://docs.microsoft.com/azure/network-watcher/network-watcher-nsg-flow-logging-portal)

* [Habilitación y uso del Análisis de tráfico](https://docs.microsoft.com/azure/network-watcher/traffic-analytics)

* [Habilitación de Network Watcher](https://docs.microsoft.com/azure/network-watcher/network-watcher-create)

**Supervisión de Azure Security Center**: Sí

**Responsabilidad**: Customer

### <a name="13-protect-critical-web-applications"></a>1.3: Proteja las aplicaciones web críticas

**Instrucciones**: No aplicable; esta recomendación está pensada para las aplicaciones web que se ejecutan en Azure App Service o en recursos de proceso.

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: No aplicable

### <a name="14-deny-communications-with-known-malicious-ip-addresses"></a>1.4: Deniegue las comunicaciones con direcciones IP malintencionadas conocidas

**Guía**: habilite Azure DDoS Protection estándar en las redes virtuales de Azure asociadas a las instancias de Azure Databricks para protegerse de los ataques por denegación de servicio distribuidos. Use la inteligencia sobre amenazas integrada de Azure Security Center para denegar las comunicaciones con direcciones IP públicas malintencionadas conocidas o no usadas.

* [Administración de Azure DDoS Protection estándar mediante Azure Portal](https://docs.microsoft.com/azure/virtual-network/manage-ddos-protection)

* [Descripción de la protección contra amenazas para la capa de red de Azure en Azure Security Center](https://docs.microsoft.com/azure/security-center/threat-protection#threat-protection-for-azure-network-layer-)

**Supervisión de Azure Security Center**: Sí

**Responsabilidad**: Customer

### <a name="15-record-network-packets-and-flow-logs"></a>1.5: Registre los paquetes de red y registros de flujo

**Guía**: implemente Azure Databricks en su propia red virtual de Azure Virtual Network (VNet). No se crea automáticamente un grupo de seguridad de red (NSG). Debe crear un NSG de línea de base que tenga solo las reglas predeterminadas de Azure. Al implementar un área de trabajo, se agregan las reglas que requiere Databricks. Habilite los registros de flujo de NSG y envíe registros a una cuenta de almacenamiento para la auditoría del tráfico. También puede enviar registros de flujo a un área de trabajo de Log Analytics y usar el Análisis de tráfico para proporcionar información detallada sobre el flujo de tráfico en la nube de Azure. Algunas de las ventajas del Análisis de tráfico son la capacidad de visualizar la actividad de la red e identificar las zonas activas, identificar las amenazas de seguridad, comprender los patrones de flujo de tráfico y detectar configuraciones de red incorrectas.

* [Implementación de Azure Databricks en su propia red virtual](https://docs.microsoft.com/azure/databricks/administration-guide/cloud-configurations/azure/vnet-inject)

* [Habilitación de los registros de flujo de NSG](https://docs.microsoft.com/azure/network-watcher/network-watcher-nsg-flow-logging-portal)

* [Habilitación y uso del Análisis de tráfico](https://docs.microsoft.com/azure/network-watcher/traffic-analytics)

* [Habilitación de Network Watcher](https://docs.microsoft.com/azure/network-watcher/network-watcher-create)

**Supervisión de Azure Security Center**: Sí

**Responsabilidad**: Customer

### <a name="16-deploy-network-based-intrusion-detectionintrusion-prevention-systems-idsips"></a>1.6: Implemente sistemas de prevención de intrusiones y detección de intrusiones (IDS/IPS) basados en la red

**Guía**: implemente una aplicación virtual de red (NVA) compatible con IDP/IP desde Azure Marketplace para crear un área de trabajo integrada con la red virtual, en la que todos los clústeres de Azure Databricks tengan una sola dirección IP de salida. Dicha dirección IP única se puede usar como una capa de seguridad adicional con otros servicios y aplicaciones de Azure que permiten el acceso basado en direcciones IP específicas. Puede usar un firewall de Azure u otras herramientas de terceros, como las aplicaciones virtuales de red, para administrar el tráfico de entrada y salida.

Si la detección y/o la prevención de intrusiones basadas en la inspección de carga no es un requisito, se puede usar Azure Firewall con la inteligencia sobre amenazas. El filtrado basado en inteligencia sobre amenazas de Azure Firewall puede alertar y denegar el tráfico desde y hacia los dominios y las direcciones IP malintencionados conocidos. La direcciones IP y los dominios proceden de la fuente Inteligencia sobre amenazas de Microsoft.

* [Asignación de una única dirección IP pública para áreas de trabajo insertadas en la red virtual con Azure Firewall](https://docs.microsoft.com/azure/databricks/kb/cloud/azure-vnet-single-ip)

* [Creación de una aplicación virtual de red](https://docs.microsoft.com/azure/virtual-network/tutorial-create-route-table-portal)

* [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/?term=Firewall)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="17-manage-traffic-to-web-applications"></a>1.7: Administre el tráfico a las aplicaciones web

**Instrucciones**: No aplicable; esta recomendación está pensada para las aplicaciones web que se ejecutan en Azure App Service o en recursos de proceso.

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: No aplicable

### <a name="18-minimize-complexity-and-administrative-overhead-of-network-security-rules"></a>1.8: Minimice la complejidad y la sobrecarga administrativa de las reglas de seguridad de red

**Guía**: use etiquetas de servicio de red virtual para definir controles de acceso de red en los grupos de seguridad de red que están conectados a las subredes con las que está asociada la instancia de Azure Databricks. Puede utilizar etiquetas de servicio en lugar de direcciones IP específicas al crear reglas de seguridad. Al especificar el nombre de la etiqueta de servicio (por ejemplo, ApiManagement) en el campo de origen o destino apropiado de una regla, puede permitir o denegar el tráfico para el servicio correspondiente. Microsoft administra los prefijos de direcciones que la etiqueta de servicio incluye y actualiza automáticamente dicha etiqueta a medida que las direcciones cambian. Las etiquetas de servicio no son compatibles con las áreas de trabajo de red virtual no insertadas.

* [Descripción de las etiquetas de servicio](https://docs.microsoft.com/azure/virtual-network/service-tags-overview)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="19-maintain-standard-security-configurations-for-network-devices"></a>1.9: Mantenga las configuraciones de seguridad estándar para dispositivos de red

**Guía**: defina e implemente configuraciones de seguridad de red para las instancias de Azure Databricks con Azure Policy. Puede usar alias de Azure Policy en el espacio de nombres "Microsoft.Databricks" para especificar definiciones de directivas personalizadas. Las directivas no se aplicarán a los recursos dentro del grupo de recursos administrado.

También puede usar Azure Blueprints para simplificar las implementaciones de Azure a gran escala mediante el empaquetado de artefactos clave de entorno, como las plantillas de Azure Resource Manager, el control de acceso basado en rol (RBAC) y las directivas, en una única definición de plano técnico. Aplique fácilmente el plano técnico a nuevas suscripciones y entornos, y ajuste el control y la administración mediante el control de versiones.

* [Configuración y administración de Azure Policy](https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage)

* [Descripción de los alias de Azure Policy y la estructura de definiciones](https://docs.microsoft.com/azure/governance/policy/concepts/definition-structure)

* [Creación de un plano técnico de Azure](https://docs.microsoft.com/azure/governance/blueprints/create-blueprint-portal)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="110-document-traffic-configuration-rules"></a>1.10: Documente las reglas de configuración de tráfico

**Guía**: use etiquetas para NSG y otros recursos relacionados con la seguridad de red y el flujo de tráfico que están asociados a su instancia de Azure Databricks. En el caso de las reglas de NSG individuales, use el campo "Descripción" para especificar las necesidades empresariales o la duración (etc.) de las reglas que permiten que entre o salga el tráfico en una red.

* [Creación y uso de etiquetas](httphttps://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="111-use-automated-tools-to-monitor-network-resource-configurations-and-detect-changes"></a>1.11: Use herramientas automatizadas para supervisar las configuraciones de recursos de red y detectar cambios

**Guía**: use el registro de actividad de Azure para supervisar las configuraciones de los recursos de red y detectar cambios en los recursos de red relacionados con las instancias de Azure Databricks. Cree alertas en Azure Monitor que se desencadenarán cuando se produzcan cambios en los recursos de red críticos.

* [Visualización y recuperación de eventos del registro de actividad de Azure](https://docs.microsoft.com/azure/azure-monitor/platform/activity-log-view)

* [Creación de alertas en Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-activity-log)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

## <a name="logging-and-monitoring"></a>Registro y supervisión

*Para obtener más información, consulte [Control de seguridad: Registro y supervisión](https://docs.microsoft.com/azure/security/benchmarks/security-control-logging-monitoring).*

### <a name="21-use-approved-time-synchronization-sources"></a>2.1: Uso de orígenes de sincronización de hora aprobados

**Guía**: Microsoft mantiene los orígenes de hora de los recursos de Azure; sin embargo, tiene la opción de administrar la configuración de la sincronización de hora de los recursos de proceso. Dado que Azure Databricks es un servicio PaaS, no tiene acceso directo a las máquinas virtuales de un clúster de Azure Databricks y no puede establecer la configuración de sincronización de hora.

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Microsoft

### <a name="22-configure-central-security-log-management"></a>2.2: Configuración de la administración central de registros de seguridad

**Guía**: ingiera registros a través de Azure Monitor para agregar datos de seguridad generados por Azure Databricks. Dentro de Azure Monitor, podrá consultar el área de trabajo de Log Analytics que está configurada para recibir los registros de diagnóstico y de Databricks. Use cuentas de Azure Storage para almacenar registros a largo plazo o archivarlos, o use los centros de eventos para exportar los datos a otros sistemas. Como alternativa, puede habilitar e incorporar datos en Azure Sentinel o en una herramienta SIEM de terceros.

Nota: Los registros de diagnóstico de Azure Databricks requieren el plan Azure Databricks Premium

* [Configuración del diagnóstico](https://docs.microsoft.com/azure/azure-monitor/platform/diagnostic-settings#create-diagnostic-settings-in-azure-portal)

* [Incorporación de Azure Sentinel](https://docs.microsoft.com/azure/sentinel/quickstart-onboard)

* [Introducción a Azure Monitor e integración con herramienta SIEM de terceros](https://azure.microsoft.com/blog/use-azure-monitor-to-integrate-with-siem-tools/)

**Supervisión de Azure Security Center**: Sí

**Responsabilidad**: Customer

### <a name="23-enable-audit-logging-for-azure-resources"></a>2.3: Habilitación del registro de auditoría para recursos de Azure

**Guía**: Habilite la configuración de diagnóstico del registro de actividad de Azure y envíela a un área de trabajo de Log Analytics, un centro de eventos de Azure o una cuenta de almacenamiento de Azure para su archivo. Con los datos del registro de actividad de Azure, puede responder a las preguntas "qué, quién y cuándo" de las operaciones de escritura (PUT, POST, DELETE) llevadas a cabo en el nivel del plano de control para los recursos de Azure.

En el caso del registro de auditoría, Azure Databricks proporciona registros de diagnóstico completos de un extremo a otro de las actividades que realizan los usuarios de Azure Databricks, lo que permite a la empresa supervisar los patrones de uso detallados de Azure Databricks.

Nota: Los registros de diagnóstico de Azure Databricks requieren el plan Azure Databricks Premium

* [Habilitación de la configuración de diagnóstico para el registro de actividad de Azure](https://docs.microsoft.com/azure/azure-monitor/platform/diagnostic-settings-legacy))

* [Habilitación de la configuración de diagnóstico para Azure Databricks](https://docs.microsoft.com/azure/databricks/administration-guide/account-settings/azure-diagnostic-logs)

**Supervisión de Azure Security Center**: no disponible actualmente

**Responsabilidad**: Customer

### <a name="24-collect-security-logs-from-operating-systems"></a>2.4: Recopilación de registros de seguridad de sistemas operativos

**Guía**: Azure Databricks proporciona registros de diagnóstico completos de un extremo a otro de las actividades que realizan los usuarios de Azure Databricks, lo que permite a la empresa supervisar los patrones de uso detallados de Azure Databricks.

Nota: Los registros de diagnóstico de Azure Databricks requieren el plan Azure Databricks Premium. El registro de seguridad del sistema operativo no está disponible.

* [Habilitación de la configuración de diagnóstico para el registro de actividad de Azure](https://docs.microsoft.com/azure/azure-monitor/platform/diagnostic-settings-legacy)

* [Habilitación de la configuración de diagnóstico para Azure Databricks](https://docs.microsoft.com/azure/databricks/administration-guide/account-settings/azure-diagnostic-logs)

**Supervisión de Azure Security Center**: no disponible actualmente

**Responsabilidad**: Customer

### <a name="25-configure-security-log-storage-retention"></a>2.5: Configuración de la retención del almacenamiento de registros de seguridad

**Guía**: habilite la configuración de diagnóstico para Azure Databricks. Si decide almacenar registros en un área de trabajo de Log Analytics, establezca el período de retención de dicha área de trabajo de acuerdo con la normativa de cumplimiento de su organización. Use cuentas de Azure Storage para el almacenamiento de archivos a largo plazo. Se realiza un seguimiento de las actividades relacionadas con la seguridad en los registros de auditoría de Databricks.

Nota: Los registros de diagnóstico de Azure Databricks requieren el plan Azure Databricks Premium

* [Habilitación de la configuración de diagnóstico en Azure Databricks](https://docs.microsoft.com/azure/databricks/administration-guide/account-settings/azure-diagnostic-logs)

* [Configuración de parámetros de retención de registros para áreas de trabajo de Log Analytics](https://docs.microsoft.com/azure/azure-monitor/platform/manage-cost-storage#change-the-data-retention-period)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="26-monitor-and-review-logs"></a>2.6: Supervisión y registros de revisión

**Guía**: habilite la configuración de diagnóstico para Azure Databricks y envíe registros a un área de trabajo de Log Analytics. Use el área de trabajo de Log Analytics para analizar y supervisar los registros de Azure Databricks en busca de un comportamiento anómalo y revise periódicamente los resultados.

Como alternativa, puede habilitar e incorporar datos en Azure Sentinel o en una herramienta SIEM de terceros.

Nota: Los registros de diagnóstico de Azure Databricks requieren el plan Azure Databricks Premium

* [Habilitación de la configuración de diagnóstico en Azure Databricks](https://docs.microsoft.com/azure/databricks/administration-guide/account-settings/azure-diagnostic-logs)

* [Consulta de los registros de Azure Databricks enviados al área de trabajo de Log Analytics](https://docs.microsoft.com/azure/databricks/administration-guide/account-settings/azure-diagnostic-logs#analyze-diagnostic-logs)

* [Incorporación de Azure Sentinel](https://docs.microsoft.com/azure/sentinel/quickstart-onboard)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="27-enable-alerts-for-anomalous-activity"></a>2.7: Habilitación de alertas para actividades anómalas

**Guía**: configure el diagnóstico para su instancia de Azure Databricks y envíe registros al área de trabajo de Log Analytics. En el área de trabajo Log Analytics, configure las alertas que se activarán cuando se cumpla un conjunto predefinido de condiciones.

Como alternativa, puede habilitar e incorporar datos en Azure Sentinel o en una herramienta SIEM de terceros.

Nota: Los registros de diagnóstico de Azure Databricks requieren el plan Azure Databricks Premium

* [Envío de los registros de Azure Databricks al área de trabajo de Log Analytics](https://docs.microsoft.com/azure/databricks/administration-guide/account-settings/azure-diagnostic-logs#configure-diagnostic-log-delivery))

* [Configuración de las alertas en el área de trabajo de Log Analytics](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-log)

**Supervisión de Azure Security Center**: no disponible actualmente

**Responsabilidad**: Customer

### <a name="28-centralize-anti-malware-logging"></a>2.8: Centralización del registro antimalware

**Guía**: No aplicable.

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: No aplicable

### <a name="29-enable-dns-query-logging"></a>2.9: Habilitación del registro de consultas de DNS

**Guía**: no aplicable; Azure Databricks no procesa ni genera registros relacionados con DNS a los que el usuario pueda acceder.

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: No aplicable

### <a name="210-enable-command-line-audit-logging"></a>2.10: Habilitación del registro de auditoría de la línea de comandos

**Guía**: No aplicable; esta guía está pensada para recursos de proceso.

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: No aplicable

## <a name="identity-and-access-control"></a>Control de identidades y acceso

*Para obtener más información, consulte [Control de seguridad: Identidad y control de acceso](https://docs.microsoft.com/azure/security/benchmarks/security-control-identity-access-control).*

### <a name="31-maintain-an-inventory-of-administrative-accounts"></a>3.1: Mantenga un inventario de cuentas administrativas

**Guía**: puede usar la API de SCIM de Azure Databricks para administrar a los usuarios de un área de trabajo de Azure Databricks y conceder privilegios administrativos a usuarios designados. También puede administrarlos a través de la interfaz de usuario de Azure Databricks.

* [Uso de las API de SCIM](https://docs.microsoft.com/azure/databricks/dev-tools/api/latest/scim/)

* [Adición y eliminación de usuarios en Azure Databricks](https://docs.microsoft.com/azure/databricks/administration-guide/users-groups/users)

**Supervisión de Azure Security Center**: Sí

**Responsabilidad**: Customer

### <a name="32-change-default-passwords-where-applicable"></a>3.2: Cambie las contraseñas predeterminadas cuando proceda

**Guía**: Azure Databricks usa Azure Active Directory (AD) para proporcionar acceso a Azure Portal, así como a la consola de administración de Azure Databricks. Sin embargo, Azure AD no incluye el concepto de contraseñas predeterminadas; el usuario es responsable de cambiar o no permitir las contraseñas predeterminadas para las aplicaciones personalizadas o de terceros.

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="33-use-dedicated-administrative-accounts"></a>3.3: Use cuentas administrativas dedicadas

**Guía**: puede usar las API de SCIM de Azure Databricks para agregar usuarios con privilegios de administrador en una instancia de Azure Databricks. También puede administrarlos a través de la interfaz de usuario de Azure Databricks.

* [Uso de las API de SCIM](https://docs.microsoft.com/azure/databricks/dev-tools/api/latest/scim/)

* [Adición y eliminación de usuarios en Azure Databricks](https://docs.microsoft.com/azure/databricks/administration-guide/users-groups/users)

**Supervisión de Azure Security Center**: Sí

**Responsabilidad**: Customer

### <a name="34-use-single-sign-on-sso-with-azure-active-directory"></a>3.4: Use el inicio de sesión único (SSO) con Azure Active Directory

**Guía**: Azure Databricks se configura automáticamente para usar el inicio de sesión único de Azure Active Directory para la autenticación de los usuarios. Los usuarios ajenos a la organización deben completar el proceso de invitación y se deben agregar al inquilino de Active Directory antes de que puedan iniciar sesión en Azure Databricks mediante el inicio de sesión único. Puede implementar SCIM para automatizar el aprovisionamiento y desaprovisionamiento de usuarios desde las áreas de trabajo.

* [Uso de las API de SCIM](https://docs.microsoft.com/azure/databricks/dev-tools/api/latest/scim/)

* [Descripción del inicio de sesión único para Azure Databricks](https://docs.microsoft.com/azure/databricks/administration-guide/users-groups/single-sign-on/)

* [Invitación de usuarios al inquilino de Azure AD](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-external-users)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Microsoft

### <a name="35-use-multi-factor-authentication-for-all-azure-active-directory-based-access"></a>3.5: Use la autenticación multifactor para cualquier acceso basado en Azure Active Directory.

**Guía**: Habilite la MFA de Azure AD y siga las recomendaciones de administración de identidades y acceso de Azure Security Center.

* [Procedimiento para habilitar la MFA en Azure](https://docs.microsoft.com/azure/active-directory/authentication/howto-mfa-getstarted)

* [Supervisión de la identidad y el acceso en Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-identity-access)

**Supervisión de Azure Security Center**: Sí

**Responsabilidad**: Customer

### <a name="36-use-dedicated-machines-privileged-access-workstations-for-all-administrative-tasks"></a>3.6: Use máquinas dedicadas (estaciones de trabajo de acceso con privilegios) para todas las tareas administrativas

**Guía**: Utilice PAW (estaciones de trabajo de acceso privilegiado) con la MFA configurada para iniciar la sesión y configurar recursos de Azure.

* [Más información sobre las estaciones de trabajo con privilegios de acceso](https://docs.microsoft.com/windows-server/identity/securing-privileged-access/privileged-access-workstations)

* [Procedimiento para habilitar la MFA en Azure](https://docs.microsoft.com/azure/active-directory/authentication/howto-mfa-getstarted)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="37-log-and-alert-on-suspicious-activity-from-administrative-accounts"></a>3.7: Registre y alerte de la actividad sospechosa desde cuentas administrativas

**Guía**: use los informes de seguridad de Azure Active Directory para la generación de registros y alertas cuando se producen actividades sospechosas o no seguras en el entorno. Use Azure Security Center para supervisar la actividad de identidad y acceso. Además, puede aprovechar los registros de diagnóstico de Azure Databricks.

* [Procedimiento para identificar usuarios de Azure AD marcados por una actividad de riesgo](https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-user-at-risk)

* [Supervisión de la actividad de identidad y acceso de los usuarios en Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-identity-access)

**Supervisión de Azure Security Center**: Sí

**Responsabilidad**: Customer

### <a name="38-manage-azure-resources-from-only-approved-locations"></a>3.8: Administre los recursos de Azure solo desde ubicaciones aprobadas

**Guía**: Use ubicaciones con nombre de acceso condicional para permitir el acceso solo desde agrupaciones lógicas específicas de intervalos de direcciones IP o países o regiones.

* [Configuración de ubicaciones con nombre en Azure](https://docs.microsoft.com/azure/active-directory/reports-monitoring/quickstart-configure-named-locations)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="39-use-azure-active-directory"></a>3.9: Uso de Azure Active Directory

**Guía**: Azure Databricks se configura automáticamente para usar el inicio de sesión único de Azure Active Directory para la autenticación de los usuarios. Los usuarios ajenos a la organización deben completar el proceso de invitación y se deben agregar al inquilino de Active Directory antes de que puedan iniciar sesión en Azure Databricks mediante el inicio de sesión único.

* [Descripción del inicio de sesión único para Azure Databricks](https://docs.microsoft.com/azure/databricks/administration-guide/users-groups/single-sign-on/)

* [Invitación de usuarios al inquilino de Azure AD](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-external-users)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Microsoft

### <a name="310-regularly-review-and-reconcile-user-access"></a>3.10: Revise y concilie regularmente el acceso de los usuarios

**Guía**: Azure AD proporciona registros para ayudar a descubrir cuentas obsoletas. Además, use las revisiones de acceso de identidad de Azure para administrar de forma eficiente las pertenencias a grupos, el acceso a las aplicaciones empresariales y las asignaciones de roles. El acceso de los usuarios se puede revisar de forma periódica para asegurarse de que solo las personas adecuadas tengan acceso continuado. También puede implementar las API de SCIM y los registros de diagnóstico de Azure Databricks para revisar el acceso de los usuarios. Además, puede usar las API de SCIM y los registros de diagnóstico de Azure Databricks para revisar el acceso de los usuarios.

Asimismo, revise y administre periódicamente el acceso de los usuarios desde la consola de administración de Azure Databricks.

* [Informes de Azure AD](https://docs.microsoft.com/azure/active-directory/reports-monitoring/)

* [Procedimiento para usar las revisiones de acceso de identidad de Azure](https://docs.microsoft.com/azure/active-directory/governance/access-reviews-overview)

* [Administración del acceso de usuario dentro de la consola de administración de Azure Databricks](https://docs.microsoft.com/azure/databricks/administration-guide/users-groups/users)

**Supervisión de Azure Security Center**: Sí

**Responsabilidad**: Customer

### <a name="311-monitor-attempts-to-access-deactivated-accounts"></a>3.11: Supervise los intentos de acceso a cuentas desactivadas

**Guía**: Tiene acceso a los orígenes de registro de eventos de actividad de inicio de sesión, auditoría y riesgo de Azure AD, que permiten la integración con cualquier herramienta de SIEM o supervisión. Además, puede usar los registros de diagnóstico de Azure Databricks para revisar la actividad de acceso de los usuarios.

Puede simplificar este proceso creando una configuración de diagnóstico para las cuentas de usuario de Azure Active Directory y enviando los registros de auditoría y los registros de inicio de sesión a un área de trabajo de Log Analytics. Puede configurar las alertas deseadas en el área de trabajo de Log Analytics.

* [Uso de las API de SCIM](https://docs.microsoft.com/azure/databricks/dev-tools/api/latest/scim/)

* [Integración de los registros de actividad de Azure en Azure Monitor](https://docs.microsoft.com/azure/active-directory/reports-monitoring/howto-integrate-activity-logs-with-log-analytics)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="312-alert-on-account-login-behavior-deviation"></a>3.12: Alerte de las desviaciones de comportamiento en los inicios de sesión de las cuentas

**Guía**: Use las características de protección de riesgos e identidad de Azure AD para configurar las respuestas automatizadas a las acciones sospechosas detectadas relacionadas con las identidades de los usuarios. También puede hacer que Azure Sentinel ingiera los datos para investigarlos más. Además, puede usar los registros de diagnóstico de Azure Databricks para revisar la actividad de acceso de los usuarios.

* [Visualización de los inicios de sesión de riesgo de Azure AD](https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-risky-sign-ins)

* [Configuración y habilitación de las directivas de riesgo de protección de identidad](https://docs.microsoft.com/azure/active-directory/identity-protection/howto-identity-protection-configure-risk-policies)

* [Incorporación de Azure Sentinel](https://docs.microsoft.com/azure/sentinel/quickstart-onboard)

**Supervisión de Azure Security Center**: no disponible actualmente

**Responsabilidad**: Customer

### <a name="313-provide-microsoft-with-access-to-relevant-customer-data-during-support-scenarios"></a>3.13: Proporcione a Microsoft acceso a los datos pertinentes del cliente durante los escenarios de soporte técnico

**Guía**: cuando haya incidencias de soporte técnico abiertas, los ingenieros del servicio de atención al cliente y soporte técnico solicitarán consentimiento para acceder a los datos de clientes pertinentes.

**Supervisión de Azure Security Center**: no disponible actualmente

**Responsabilidad**: no disponible actualmente

## <a name="data-protection"></a>Protección de los datos

*Para obtener más información, consulte [Control de seguridad: Protección de datos](https://docs.microsoft.com/azure/security/benchmarks/security-control-data-protection).*

### <a name="41-maintain-an-inventory-of-sensitive-information"></a>4.1: Mantenimiento de un inventario de información confidencial

**Guía**: use etiquetas para ayudar a realizar el seguimiento de las instancias de Azure Databricks que procesan información confidencial.

* [Creación y uso de etiquetas](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="42-isolate-systems-storing-or-processing-sensitive-information"></a>4.2: Aislamiento de los sistemas que almacenan o procesan información confidencial

**Instrucciones**: Implemente suscripciones y/o grupos de administración independientes para los entornos de desarrollo, prueba y producción. La implementación predeterminada de Azure Databricks es un servicio totalmente administrado que se implementa dentro de una red virtual propia. Si necesita personalizar la red, puede implementar Azure Databricks en su red virtual. El procedimiento recomendado consiste en crear áreas de trabajo de Azure Databricks independientes para diferentes equipos o departamentos empresariales.

* [Implementación de Azure Databricks en su propia red virtual](https://docs.microsoft.com/azure/databricks/administration-guide/cloud-configurations/azure/vnet-inject)

* [Creación de suscripciones adicionales de Azure](https://docs.microsoft.com/azure/billing/billing-create-subscription)

* [Creación de grupos de administración](https://docs.microsoft.com/azure/governance/management-groups/create)

* [Creación de una red virtual](https://docs.microsoft.com/azure/virtual-network/quick-create-portal)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="43-monitor-and-block-unauthorized-transfer-of-sensitive-information"></a>4.3: Supervisión y bloqueo de transferencias no autorizadas de información confidencial

**Guía**: imite la arquitectura de protección contra filtraciones datos de Databricks para mitigar la posibilidad de que se produzcan filtraciones de este tipo.

* [Protección contra la filtración de datos con Azure Databricks](https://databricks.com/blog/2020/03/27/data-exfiltration-protection-with-azure-databricks.html)

Microsoft administra la infraestructura subyacente para Azure Databricks y ha implementado controles estrictos para evitar la pérdida o exposición de los datos de los clientes.

* [Descripción de la protección de datos de los clientes en Azure](https://docs.microsoft.com/azure/security/fundamentals/protection-customer-data)

**Supervisión de Azure Security Center**: no disponible actualmente

**Responsabilidad**: no disponible actualmente

### <a name="44-encrypt-all-sensitive-information-in-transit"></a>4.4: Cifrado de toda la información confidencial en tránsito

**Guía**: Microsoft negociará TLS 1.2 de forma predeterminada al administrar la instancia de Azure Databricks a través de Azure Portal o la consola de Azure Databricks. La aplicación web de Databricks es compatible con TLS 1.3.

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Microsoft

### <a name="45-use-an-active-discovery-tool-to-identify-sensitive-data"></a>4.5: Uso de una herramienta de detección activa para identificar datos confidenciales

**Guía**: actualmente no disponible; las características de identificación, clasificación y prevención de pérdida de datos no están disponibles en este momento en Azure Databricks. Etiquete las instancias de Azure Databricks y los recursos relacionados que podrían procesar información confidencial como tal e implemente una solución de terceros, si es necesario, para fines de cumplimiento.

La plataforma de Databricks solo realiza el procesamiento, y todos los datos se almacenan en otros servicios de datos de Azure. En el caso de la plataforma subyacente administrada por Microsoft, Microsoft trata todo el contenido de los clientes como confidencial y hace grandes esfuerzos para proteger a los clientes contra la pérdida y exposición de sus datos. Para garantizar la seguridad de los datos de los clientes dentro de Azure, Microsoft ha implementado y mantiene un conjunto de controles y funcionalidades eficaces de protección de datos.

* [Descripción de la protección de datos de los clientes en Azure](https://docs.microsoft.com/azure/security/fundamentals/protection-customer-data)

**Supervisión de Azure Security Center**: no disponible actualmente

**Responsabilidad**: no disponible actualmente

### <a name="46-use-azure-rbac-to-control-access-to-resources"></a>4.6: Uso de RBAC de Azure para controlar el acceso a los recursos

**Guía**: En Azure Databricks, puede utilizar las listas de control de acceso (ACL) para configurar el permiso para acceder a tablas de datos, clústeres, grupos, trabajos y objetos del área de trabajo, como cuadernos, experimentos y carpetas. Todos los usuarios administradores pueden administrar las listas de control de acceso, al igual que los usuarios que tienen permisos delegados para administrar las listas de control de acceso. Puede usar RBAC de Azure para configurar permisos en el área de trabajo de Azure Databricks.

Nota: El control de acceso a tablas, clústeres, grupos, trabajos y áreas de trabajo solo está disponible en el plan Azure Databricks Premium.

* [Administración del control de acceso en Azure Databricks](https://docs.microsoft.com/azure/databricks/administration-guide/access-control/)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="47-use-host-based-data-loss-prevention-to-enforce-access-control"></a>4.7: Uso de la prevención de pérdida de datos basada en host para aplicar el control de acceso

**Guía**: No aplicable; esta recomendación está pensada para los recursos de proceso.

Microsoft administra la infraestructura subyacente para Azure Databricks y ha implementado controles estrictos para evitar la pérdida o exposición de los datos de los clientes.

* [Descripción de la protección de datos de los clientes en Azure](https://docs.microsoft.com/azure/security/fundamentals/protection-customer-data)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: No aplicable

### <a name="48-encrypt-sensitive-information-at-rest"></a>4.8: Cifrado de información confidencial en reposo

**Guía**: un área de trabajo Azure Databricks consta de un plano de administración que se hospeda en una red virtual administrada de Azure Databricks y de un plano de datos que se implementa en una red virtual administrada por el cliente. El plano de control almacena los cuadernos de todos los usuarios, así como los resultados de cuaderno asociados, en una base de datos. De forma predeterminada, todos los cuadernos y resultados se cifran en reposo con una clave de cifrado diferente.

El sistema de archivos de Databricks (DBFS) es un sistema de archivos distribuido montado en un área de trabajo de Azure Databricks y está disponible en los clústeres de Azure Databricks. DBFS se implementa como una cuenta de almacenamiento en el grupo de recursos administrados en su área de trabajo de Azure Databricks. De manera predeterminada, la cuenta de almacenamiento se cifran con claves administradas por Microsoft. Los datos se almacenan en los servicios de datos de Azure de su propiedad, y tiene la opción de cifrarlos. No se recomienda el uso de DBFS para almacenar los datos de producción.

Nota: Estas características no están disponibles para todas las suscripciones a Azure Databricks. Póngase en contacto con el representante de la cuenta de Microsoft o Databricks para solicitar el acceso.

* [Habilitación de claves administradas por el cliente para los cuadernos de Azure Databricks](https://docs.microsoft.com/azure/databricks/security/customer-managed-key-notebook)

* [Habilitación de claves administradas por el cliente para el sistema de archivos de Azure Databricks](https://docs.microsoft.com/azure/databricks/security/customer-managed-keys-dbfs)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Compartido

### <a name="49-log-and-alert-on-changes-to-critical-azure-resources"></a>4.9: Registro y alerta de cambios en los recursos críticos de Azure

**Guía**: use Azure Monitor con el registro de actividad de Azure para crear alertas para cuando se produzcan cambios en las áreas de trabajo de Azure Databricks críticas.

* [Creación de alertas para los eventos del registro de actividad de Azure](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-activity-log)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

## <a name="vulnerability-management"></a>Administración de vulnerabilidades

*Para obtener más información, consulte [Control de seguridad: Administración de vulnerabilidades](https://docs.microsoft.com/azure/security/benchmarks/security-control-vulnerability-management).*

### <a name="51-run-automated-vulnerability-scanning-tools"></a>5.1: Ejecute herramientas de análisis de vulnerabilidades automatizado

**Instrucciones**: implemente una solución de administración de vulnerabilidades de terceros.

Si tiene una suscripción a una plataforma de administración de vulnerabilidades, puede usar scripts de inicialización de Azure Databricks para instalar agentes de evaluación de vulnerabilidades en los nodos del clúster de Azure Databricks y administrar los nodos mediante el portal correspondiente. Tenga en cuenta que todas las soluciones de terceros funcionan de forma distinta.

* [Instalación manual del agente de Rapid7](https://insightagent.help.rapid7.com/docs/install)

* [Instalación manual del agente de Qualys](https://www.qualys.com/docs/qualys-cloud-agent-linux-install-guide.pdf)

* [Scripts de inicialización de Azure Databricks](https://docs.microsoft.com/azure/databricks/clusters/init-scripts)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="52-deploy-automated-operating-system-patch-management-solution"></a>5.2: Implemente una solución de administración de revisiones de sistema operativo automatizada

**Guía**: Microsoft mantiene las imágenes base del nodo de clúster de Azure Databricks; sin embargo, es responsabilidad del usuario asegurarse de que los nodos del clúster permanezcan revisados. Para agregar una actualización de mantenimiento a un clúster en ejecución existente, debe reiniciarlo.

* [Descripción de las actualizaciones de mantenimiento del entorno de ejecución para Azure Databricks](https://docs.microsoft.com/azure/databricks/release-notes/runtime/maintenance-updates)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="53-deploy-automated-third-party-software-patch-management-solution"></a>5.3: Implementación de una solución automatizada de administración de revisiones de software de terceros

**Guía**: Microsoft mantiene las imágenes base del nodo de clúster de Azure Databricks; sin embargo, es responsabilidad del usuario asegurarse de que todas las aplicaciones de terceros que instale permanezcan revisadas.

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="54-compare-back-to-back-vulnerability-scans"></a>5.4: Compare los exámenes de vulnerabilidades opuestos

**Guía**: implemente una solución de administración de vulnerabilidades de terceros que tenga la capacidad de comparar exámenes de vulnerabilidades a lo largo del tiempo. Si tiene una suscripción a una administración de vulnerabilidades, puede usar el portal de ese proveedor para ver y comparar exámenes de vulnerabilidades opuestos. Tenga en cuenta que todas las soluciones de terceros funcionan de forma distinta.

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="55-use-a-risk-rating-process-to-prioritize-the-remediation-of-discovered-vulnerabilities"></a>5.5: Use un proceso de clasificación de riesgos para priorizar la corrección de vulnerabilidades detectadas.

**Guía**: use un programa de puntuación de riesgos común (por ejemplo, Common Vulnerability Scoring System) o la clasificación de riesgos predeterminada proporcionada por su herramienta de examen de terceros.

**Supervisión de Azure Security Center**: N/D

**Responsabilidad**: Customer

## <a name="inventory-and-asset-management"></a>Inventario y administración de recursos

*Para obtener más información, consulte [Control de seguridad: Administración de recursos y del inventario](https://docs.microsoft.com/azure/security/benchmarks/security-control-inventory-asset-management).*

### <a name="61-use-azure-asset-discovery"></a>6.1: Uso de la detección de recursos de Azure

**Guía**: Use Azure Resource Graph para consultar o detectar todos los recursos (por ejemplo, proceso, almacenamiento, red, puertos y protocolos, etc.) dentro de las suscripciones. Asegúrese de que los permisos adecuados (lectura) existen en el inquilino y enumere todas las suscripciones a Azure, así como los recursos de las suscripciones.

Aunque los recursos clásicos de Azure se pueden detectar a través de Resource Graph, se recomienda encarecidamente crear y usar los recursos de Azure Resource Manager que figuran a continuación.

* [Creación de consultas con Azure Resource Graph](https://docs.microsoft.com/azure/governance/resource-graph/first-query-portal)

* [Visualización de las suscripciones de Azure](https://docs.microsoft.com/powershell/module/az.accounts/get-azsubscription?view=azps-3.0.0)

* [Descripción del RBAC de Azure](https://docs.microsoft.com/azure/role-based-access-control/overview)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="62-maintain-asset-metadata"></a>6.2: Mantenimiento de metadatos de recursos

**Instrucciones**: Aplique etiquetas a los recursos de Azure que proporcionan metadatos para organizarlos de forma lógica en una taxonomía.

* [Creación y uso de etiquetas](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="63-delete-unauthorized-azure-resources"></a>6.3: Eliminación de recursos de Azure no autorizados

**Guía**: use el etiquetado, los grupos de administración y las suscripciones independientes, si procede, para organizar y realizar un seguimiento de los recursos de Azure. Concilie el inventario periódicamente y asegúrese de que los recursos no autorizados se eliminan de la suscripción de manera oportuna.

Además, use Azure Policy para establecer restricciones sobre el tipo de recursos que se pueden crear en las suscripciones del cliente con las siguientes definiciones de directiva integradas:
- Tipos de recursos no permitidos
- Tipos de recursos permitidos

* [Creación de suscripciones adicionales de Azure](https://docs.microsoft.com/azure/billing/billing-create-subscription)

* [Creación de grupos de administración](https://docs.microsoft.com/azure/governance/management-groups/create)

* [Creación y uso de etiquetas](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="64-maintain-inventory-of-approved-azure-resources-and-software-titles"></a>6.4: Mantenimiento de un inventario de los recursos de Azure y títulos de software aprobados

**Guía**: Defina los recursos de Azure y el software aprobados para los recursos de proceso.

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="65-monitor-for-unapproved-azure-resources"></a>6.5: Supervisión de recursos de Azure no aprobados

**Instrucciones**: use Azure Policy para establecer restricciones en el tipo de recursos que se pueden crear en las suscripciones del cliente con las siguientes definiciones de directiva integradas:
- Tipos de recursos no permitidos
- Tipos de recursos permitidos

Use Azure Resource Graph para consultar o detectar recursos dentro de sus suscripciones. Asegúrese de que todos los recursos de Azure presentes en el entorno estén aprobados.

* [Configuración y administración de Azure Policy](https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage)

* [Creación de consultas con Azure Graph](https://docs.microsoft.com/azure/governance/resource-graph/first-query-portal)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="66-monitor-for-unapproved-software-applications-within-compute-resources"></a>6.6: Supervisión de aplicaciones de software no aprobadas en recursos de proceso

**Guía**: no aplicable; Azure Databricks es un servicio PaaS, por lo que los clientes no tienen acceso directo a las máquinas virtuales del clúster de Azure Databricks.

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="67-remove-unapproved-azure-resources-and-software-applications"></a>6.7: Eliminación de aplicaciones de software y recursos de Azure no aprobadas

**Guía**: use Azure Resource Graph para consultar o detectar todos los recursos (por ejemplo, proceso, almacenamiento, red, puertos y protocolos, etc.), incluidos los clústeres de Azure Databricks, dentro de las suscripciones. Quite los recursos de Azure no aprobados que detecte. En el caso de los nodos del clúster de Azure Databricks, implemente una solución de terceros para quitar software no aprobado o alertas sobre este.

* [Creación de consultas con Azure Graph](https://docs.microsoft.com/azure/governance/resource-graph/first-query-portal)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="68-use-only-approved-applications"></a>6.8: Uso exclusivo de aplicaciones aprobadas

**Guía**: no aplicable; Azure Databricks es un servicio PaaS, por lo que el usuario no tiene acceso directo a las máquinas virtuales del clúster de Azure Databricks.

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="69-use-only-approved-azure-services"></a>6.9: Uso exclusivo de servicios de Azure aprobados

**Guía**: use Azure Policy para establecer restricciones sobre el tipo de recursos que se pueden crear en las suscripciones del cliente con las siguientes definiciones de directiva integradas:
- Tipos de recursos no permitidos
- Tipos de recursos permitidos

* [Configuración y administración de Azure Policy](https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage)

* [Denegación de un tipo de recurso específico con Azure Policy](https://docs.microsoft.com/azure/governance/policy/samples/not-allowed-resource-types)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="610-implement-approved-application-list"></a>6.10: Implementación de la lista de aplicaciones aprobadas

**Guía**: no aplicable; Azure Databricks es un servicio PaaS, por lo que el usuario no tiene acceso directo a las máquinas virtuales del clúster de Azure Databricks.

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="611-divlimit-users-ability-to-interact-with-azure-resource-manager-via-scriptsdiv"></a>6.11: <div>Limitación de la capacidad de los usuarios para interactuar con Azure Resource Manager a través de scripts</div>

**Guía**: use el acceso condicional de Azure para limitar la capacidad de los usuarios de interactuar con Azure Resource Manager mediante la configuración de la opción "Bloquear acceso" en la aplicación "Administración de Microsoft Azure".

* [Configuración del acceso condicional para bloquear el acceso a Azure Resource Manager](https://docs.microsoft.com/azure/role-based-access-control/conditional-access-azure-management)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="612-limit-users-ability-to-execute-scripts-within-compute-resources"></a>6.12: Limitación de la capacidad de los usuarios para ejecutar scripts en los recursos de proceso

**Guía**: no aplicable; no se aplica a Azure Databricks, ya que los usuarios (que no son administradores) del clúster no necesitan acceso a los nodos individuales para ejecutar trabajos. El administrador del clúster tiene acceso raíz a todos los nodos del clúster de forma predeterminada.

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: No aplicable

### <a name="613-physically-or-logically-segregate-high-risk-applications"></a>6.13: Segregación física o lógica de aplicaciones de alto riesgo

**Guía**: no aplicable; la prueba comparativa está pensada para Azure App Service o recursos de proceso que hospedan aplicaciones web.

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: No aplicable

## <a name="secure-configuration"></a>Configuración segura

*Para obtener más información, consulte [Control de seguridad: Configuración segura](https://docs.microsoft.com/azure/security/benchmarks/security-control-secure-configuration).*

### <a name="71-establish-secure-configurations-for-all-azure-resources"></a>7.1: Establezca configuraciones seguras para todos los recursos de Azure

**Guía**: defina e implemente configuraciones de seguridad estándar para las instancias de Azure Databricks con Azure Policy. Use alias de Azure Policy en el espacio de nombres "Microsoft.Databricks" para crear directivas personalizadas a fin de auditar o aplicar la configuración de las instancias de Azure Databricks. Tenga en cuenta que las directivas aplicadas no funcionan en el grupo de recursos administrados de Databricks.

* [Visualización de los alias de Azure Policy disponibles](https://docs.microsoft.com/powershell/module/az.resources/get-azpolicyalias?view=azps-3.3.0)

* [Configuración y administración de Azure Policy](https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="72-establish-secure-operating-system-configurations"></a>7.2: Establezca configuraciones del sistema operativo seguras

**Guía**: no aplicable; Azure Databricks es un servicio PaaS, por lo que el usuario no tiene acceso directo a las máquinas virtuales del clúster de Azure Databricks.

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="73-maintain-secure-azure-resource-configurations"></a>7.3: Mantenga configuraciones de recursos de Azure seguras

**Guía**: defina e implemente configuraciones de seguridad estándar para las instancias de Azure Databricks con Azure Policy. Use alias de Azure Policy en el espacio de nombres "Microsoft.Databricks" para crear directivas personalizadas a fin de auditar o aplicar la configuración de las instancias de Azure Databricks. Use la directiva de Azure [denegar] e [implementar si no existe] para aplicar la configuración segura en los recursos de Azure.

* [Configuración y administración de Azure Policy](https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage)

* [Descripción de los efectos de Azure Policy](https://docs.microsoft.com/azure/governance/policy/concepts/effects)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="74-maintain-secure-operating-system-configurations"></a>7.4: Mantenga configuraciones del sistema operativo seguras

**Guía**: Microsoft administra y mantiene las imágenes del sistema operativo de Azure Databricks. Usted es responsable de implementar la configuración de estado en el nivel de sistema operativo.

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Compartido

### <a name="75-securely-store-configuration-of-azure-resources"></a>7.5: Almacene de forma segura la configuración de los recursos de Azure

**Instrucciones**: Si usa definiciones de directivas personalizadas de Azure, use Azure DevOps o Azure Repos para almacenar y administrar el código de forma segura.

* [Almacenamiento de código en Azure DevOps](https://docs.microsoft.com/azure/devops/repos/git/gitworkflow?view=azure-devops)

* [Documentación de Azure Repos](https://docs.microsoft.com/azure/devops/repos/index?view=azure-devops)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="76-securely-store-custom-operating-system-images"></a>7.6: Almacene imágenes de sistema operativo personalizadas de forma segura

**Guía**: si usa imágenes personalizadas para los nodos de clúster de Azure Databricks, inserte la imagen base personalizada en un registro de Docker, como Azure Container Registry (ACR).

* [Personalización de contenedores con Databricks Container Services](https://docs.microsoft.com/azure/databricks/clusters/custom-containers)

* [Descripción de Azure Container Registry](https://docs.microsoft.com/azure/container-registry/container-registry-intro)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="77-deploy-system-configuration-management-tools"></a>7.7: Implemente herramientas de administración de configuración del sistema

**Guía**: defina e implemente configuraciones de seguridad estándar para las instancias de Azure Databricks con Azure Policy. Use alias de Azure Policy en el espacio de nombres "Microsoft.Databricks" para crear directivas personalizadas a fin de auditar o aplicar la configuración de las instancias de Azure Databricks.

* [Configuración y administración de Azure Policy](https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage)

* [Visualización de los alias de Azure Policy disponibles](https://docs.microsoft.com/powershell/module/az.resources/get-azpolicyalias?view=azps-3.3.0)

* [Configuración y administración de Azure Policy](https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="78-deploy-system-configuration-management-tools-for-operating-systems"></a>7.8: Implemente herramientas de administración de configuración del sistema para sistemas operativos

**Guía**: no aplicable; Azure Databricks es un servicio PaaS, por lo que el usuario no tiene acceso directo a las máquinas virtuales del clúster de Azure Databricks.

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="79-implement-automated-configuration-monitoring-for-azure-services"></a>7.9: Implementación de la supervisión de configuración automatizada para los servicios de Azure

**Guía**: defina e implemente configuraciones de seguridad estándar para las instancias de Azure Databricks con Azure Policy. Use alias de Azure Policy en el espacio de nombres "Microsoft.Databricks" para crear directivas personalizadas a fin de auditar o aplicar la configuración de las instancias de Azure Databricks.

* [Visualización de los alias de Azure Policy disponibles](https://docs.microsoft.com/powershell/module/az.resources/get-azpolicyalias?view=azps-3.3.0)

* [Configuración y administración de Azure Policy](https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="710-implement-automated-configuration-monitoring-for-operating-systems"></a>7.10: Implemente la supervisión de configuración automatizada para sistemas operativos

**Guía**: implemente una solución de terceros para supervisar el estado de los sistemas operativos del nodo de clúster de Azure Databricks.

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="711-manage-azure-secrets-securely"></a>7.11: Administre los secretos de Azure de forma segura

**Guía**: use Azure Key Vault con un ámbito de secreto de Azure Databricks para administrar y usar secretos de forma segura.

* [Uso de Azure Key Vault con Azure Databricks](https://docs.microsoft.com/azure/azure-databricks/store-secrets-azure-key-vault)

**Supervisión de Azure Security Center**: Sí

**Responsabilidad**: Customer

### <a name="712-manage-identities-securely-and-automatically"></a>7.12: Administre las identidades de forma segura y automática

**Guía**: actualmente no está disponible; las identidades administradas no están disponibles actualmente para Azure Databricks.

* [Servicios que admiten identidades administradas para recursos de Azure](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/services-support-managed-identities)

**Supervisión de Azure Security Center**: no disponible actualmente

**Responsabilidad**: no disponible actualmente

### <a name="713-eliminate-unintended-credential-exposure"></a>7.13: Elimine la exposición de credenciales no intencionada

**Instrucciones**: Implemente el escáner de credenciales para identificar las credenciales en el código. El escáner de credenciales también fomenta el traslado de credenciales detectadas a ubicaciones más seguras, como Azure Key Vault.

* [Configuración del escáner de credenciales](https://secdevtools.azurewebsites.net/helpcredscan.html)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

## <a name="malware-defense"></a>Defensa contra malware

*Para obtener más información, consulte [Control de seguridad: Defensa contra malware](https://docs.microsoft.com/azure/security/benchmarks/security-control-malware-defense).*

### <a name="81-use-centrally-managed-anti-malware-software"></a>8.1: Use software antimalware administrado centralmente

**Guía**: si tiene una suscripción a una plataforma de administración de vulnerabilidades, puede usar scripts de inicialización de Azure Databricks para instalar agentes de evaluación de vulnerabilidades en los nodos del clúster de Azure Databricks y administrar los nodos mediante el portal correspondiente. Tenga en cuenta que todas las soluciones de terceros funcionan de forma distinta.

* [Instalación manual del agente de Rapid7](https://insightagent.help.rapid7.com/docs/install)

* [Instalación manual del agente de Qualys](https://www.qualys.com/docs/qualys-cloud-agent-linux-install-guide.pdf)

* [Scripts de inicialización de Azure Databricks](https://docs.microsoft.com/azure/databricks/clusters/init-scripts)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="82-pre-scan-files-to-be-uploaded-to-non-compute-azure-resources"></a>8.2: Examine previamente los archivos que se van a cargar en recursos de Azure que no son de proceso

**Guía**: Microsoft Antimalware está habilitado en el host subyacente que admite los servicios de Azure (por ejemplo, Azure App Service), pero no se ejecuta en el contenido.

Examine previamente los archivos que se van a cargar en los nodos de clúster de Azure Databricks o en los recursos relacionados.

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="83-ensure-anti-malware-software-and-signatures-are-updated"></a>8.3: Asegúrese de que se han actualizado el software y las firmas antimalware

**Guía**: si tiene una suscripción a una plataforma de administración de vulnerabilidades, puede usar scripts de inicialización de Azure Databricks para instalar agentes de evaluación de vulnerabilidades en los nodos del clúster de Azure Databricks y administrar los nodos mediante el portal correspondiente. Tenga en cuenta que todas las soluciones de terceros funcionan de forma distinta.

* [Instalación manual del agente de Rapid7](https://insightagent.help.rapid7.com/docs/install)

* [Instalación manual del agente de Qualys](https://www.qualys.com/docs/qualys-cloud-agent-linux-install-guide.pdf)

* [Scripts de inicialización de Azure Databricks](https://docs.microsoft.com/azure/databricks/clusters/init-scripts)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

## <a name="data-recovery"></a>Recuperación de datos

*Para obtener más información, consulte [Control de seguridad: Recuperación de datos](https://docs.microsoft.com/azure/security/benchmarks/security-control-data-recovery).*

### <a name="91-ensure-regular-automated-back-ups"></a>9.1: Garantía de copias de seguridad automáticas periódicas

**Guía**: en el caso de los orígenes de datos de Azure Databricks, asegúrese de que ha configurado un nivel adecuado de redundancia de datos para su caso de uso. Por ejemplo, si va a usar una cuenta de Azure Storage para el almacén de datos de Azure Databricks, elija la opción de redundancia adecuada (LRS,ZRS, GRS, RA-GRS).

* [Orígenes de datos de Azure Databricks](https://docs.microsoft.com/azure/azure-databricks/databricks-connect-to-data-sources#data-sources-for-azure-databricks)

* [Recuperación ante desastres regional para clústeres de Azure Databricks](https://docs.microsoft.com/azure/azure-databricks/howto-regional-disaster-recovery)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="92-perform-complete-system-backups-and-backup-any-customer-managed-keys"></a>9.2: Realización de copias de seguridad completas del sistema y copia de seguridad de las claves administradas por el cliente

**Guía**: realice una copia de seguridad de las claves administradas por el cliente relacionadas con sus implementaciones de Azure Databricks dentro de Azure Key Vault. También puede usar la API REST y la CLI para crear una copia de seguridad diaria de las configuraciones de Databricks. También puede usar la API REST o la CLI para crear una copia de seguridad diaria de las configuraciones de Databricks.

* [Creación de una copia de seguridad de las claves del almacén de claves en Azure](https://docs.microsoft.com/powershell/module/azurerm.keyvault/backup-azurekeyvaultkey?view=azurermps-6.13.0)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="93-validate-all-backups-including-customer-managed-keys"></a>9.3: Validación de todas las copias de seguridad, incluidas las claves administradas por el cliente

**Guía**: pruebe a restaurar las copias de seguridad de las claves administradas por el cliente relacionadas con sus implementaciones de Azure Databricks.

* [Restauración de las claves del almacén de claves en Azure](https://docs.microsoft.com/powershell/module/azurerm.keyvault/restore-azurekeyvaultkey?view=azurermps-6.13.0)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="94-ensure-protection-of-backups-and-customer-managed-keys"></a>9.4: Garantía de la protección de las copias de seguridad y las claves administradas del cliente

**Guía**: asegúrese de habilitar la eliminación temporal en Key Vault para proteger las claves contra la eliminación accidental o malintencionada.

* [Habilitación de la eliminación temporal en Key Vault](https://docs.microsoft.com/azure/storage/blobs/storage-blob-soft-delete?tabs=azure-portal)

**Supervisión de Azure Security Center**: Sí

**Responsabilidad**: Customer

## <a name="incident-response"></a>Respuesta a los incidentes

*Para obtener más información, consulte [Control de seguridad: Respuesta a los incidentes](https://docs.microsoft.com/azure/security/benchmarks/security-control-incident-response).*

### <a name="101-create-an-incident-response-guide"></a>10.1: Creación de una guía de respuesta ante incidentes

**Instrucciones**: Cree una guía de respuesta a incidentes para su organización. Asegúrese de que haya planes de respuesta a incidentes escritos que definan todos los roles del personal, así como las fases de administración y gestión de los incidentes, desde la detección hasta la revisión posterior a la incidencia.

* [Configuración de las automatizaciones de flujos de trabajo en Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-planning-and-operations-guide)

* [Guía para crear su propio proceso de respuesta a incidentes de seguridad](https://msrc-blog.microsoft.com/2019/07/01/inside-the-msrc-building-your-own-security-incident-response-process/)

* [Anatomía de un incidente del Centro de respuestas de seguridad de Microsoft](https://msrc-blog.microsoft.com/2019/07/01/inside-the-msrc-building-your-own-security-incident-response-process/)

* [También puede usar la guía de control de incidentes de seguridad de equipos de NIST como referencia para la creación de su propio plan de respuesta a incidentes](https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-61r2.pdf)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="102-create-an-incident-scoring-and-prioritization-procedure"></a>10.2: Creación de un procedimiento de priorización y puntuación de incidentes

**Instrucciones**: Security Center asigna una gravedad a cada alerta para ayudarle a priorizar aquellas que se deben investigar en primer lugar. La gravedad se basa en la confianza que tiene Security Center en la búsqueda o en el análisis utilizados para emitir la alerta, así como en el nivel de confianza de que ha habido un intento malintencionado detrás de la actividad que ha provocado la alerta.

Además, marque claramente las suscripciones (por ejemplo, producción, no producción) y cree un sistema de nomenclatura para identificar y clasificar claramente los recursos de Azure.

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="103-test-security-response-procedures"></a>10.3: Prueba de los procedimientos de respuesta de seguridad

**Instrucciones**: Realice ejercicios para probar las capacidades de respuesta a los incidentes de los sistemas con regularidad. Identifique puntos débiles y brechas y revise el plan según sea necesario.

* [Consulte la publicación de NIST: Guía para probar, entrenar y ejecutar programas para planes y funcionalidades de TI](https://nvlpubs.nist.gov/nistpubs/Legacy/SP/nistspecialpublication800-84.pdf)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="104-provide-security-incident-contact-details-and-configure-alert-notifications-for-security-incidents"></a>10.4: Provisión de detalles de contacto de incidentes de seguridad y configuración de notificaciones de alerta para incidentes de seguridad

**Guía**: Microsoft usará la información de contacto del incidente de seguridad para ponerse en contacto con usted si Microsoft Security Response Center (MSRC) detecta que un tercero no autorizado o ilegal ha accedido a los datos del cliente. Revise los incidentes después del hecho para asegurarse de que se resuelven los problemas.

* [Establecimiento del contacto de seguridad de Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-provide-security-contact-details)

**Supervisión de Azure Security Center**: Sí

**Responsabilidad**: Customer

### <a name="105-incorporate-security-alerts-into-your-incident-response-system"></a>10.5: Incorporación de alertas de seguridad en el sistema de respuesta a incidentes

**Instrucciones**: Exporte sus alertas y recomendaciones de Azure Security Center mediante la característica de exportación continua. La exportación continua le permite exportar alertas y recomendaciones de forma manual o continua. Puede usar el conector de datos de Azure Security Center para transmitir las alertas a Sentinel.

* [Configuración de la exportación continua](https://docs.microsoft.com/azure/security-center/continuous-export)

* [Transmisión de alertas a Azure Sentinel](https://docs.microsoft.com/azure/sentinel/connect-azure-security-center)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="106-automate-the-response-to-security-alerts"></a>10.6: Automatización de la respuesta a las alertas de seguridad

**Instrucciones**: use la característica de automatización del flujo de trabajo de Azure Security Center para desencadenar automáticamente respuestas mediante "Logic Apps" en las alertas y recomendaciones de seguridad.

* [Configuración de la automatización de flujo de trabajo y Logic Apps](https://docs.microsoft.com/azure/security-center/workflow-automation)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

## <a name="penetration-tests-and-red-team-exercises"></a>Pruebas de penetración y ejercicios del equipo rojo

*Para obtener más información, consulte [Control de seguridad: Pruebas de penetración y ejercicios del equipo rojo](https://docs.microsoft.com/azure/security/benchmarks/security-control-penetration-tests-red-team-exercises).*

### <a name="111-conduct-regular-penetration-testing-of-your-azure-resources-and-ensure-remediation-of-all-critical-security-findings-within-60-days"></a>11.1: Realice pruebas de penetración periódicas de los recursos de Azure y asegúrese de corregir todos los resultados de seguridad críticos en un plazo de 60 días

**Guía**: *[Siga las reglas de afiliación de Microsoft para asegurarse de que las pruebas de penetración no infrinjan las directivas de Microsoft](https://www.microsoft.com/msrc/pentest-rules-of-engagement?rtc=1)

* [Puede encontrar más información sobre la estrategia y ejecución de Microsoft en las pruebas de penetración del equipo rojo y los sitios activos en la infraestructura de nube, los servicios y las aplicaciones administradas por Microsoft aquí.](https://gallery.technet.microsoft.com/Cloud-Red-Teaming-b837392e)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Compartido

## <a name="next-steps"></a>Pasos siguientes

- Consulte la [prueba comparativa de seguridad de Azure](https://docs.microsoft.com/azure/security/benchmarks/overview).
- Obtenga más información sobre las [líneas de base de seguridad de Azure](https://docs.microsoft.com/azure/security/benchmarks/security-baselines-overview).

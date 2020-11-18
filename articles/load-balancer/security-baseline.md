---
title: Línea de base de seguridad de Azure para Azure Load Balancer
description: La línea de base de seguridad de Azure Load Balancer proporciona una guía de procedimientos y recursos para implementar las recomendaciones de seguridad especificadas en Azure Security Benchmark.
author: msmbaldwin
ms.service: load-balancer
ms.topic: conceptual
ms.date: 09/28/2020
ms.author: mbaldwin
ms.custom: subject-security-benchmark
ms.openlocfilehash: 07dd369ceeefee85f4d9180a0b9b33a0d4c8bbdb
ms.sourcegitcommit: e2dc549424fb2c10fcbb92b499b960677d67a8dd
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/17/2020
ms.locfileid: "94698213"
---
# <a name="azure-security-baseline-for-azure-load-balancer"></a>Línea de base de seguridad de Azure para Azure Load Balancer

La base de referencia de seguridad de Azure para Microsoft Azure Load Balancer contiene recomendaciones que lo ayudarán a mejorar la estrategia de seguridad de la implementación. La base de referencia de este servicio se extrae de la [versión 1.0 de Azure Security Benchmark](../security/benchmarks/overview.md), que proporciona recomendaciones sobre cómo puede proteger las soluciones en la nube en Azure con nuestra guía de procedimientos recomendados. Para obtener más información, consulte [Introducción a las líneas de base de seguridad de Azure](../security/benchmarks/security-baselines-overview.md).

## <a name="network-security"></a>Seguridad de las redes

*Para obtener más información, consulte [Azure Security Benchmark: Seguridad de redes](../security/benchmarks/security-control-network-security.md).*

### <a name="11-protect-azure-resources-within-virtual-networks"></a>1.1: Protección de los recursos de Azure dentro de las redes virtuales

**Guía**: Use instancias de Azure Load Balancer internas para permitir solo el tráfico a los recursos de back-end desde determinadas redes virtuales o redes virtuales emparejadas sin exposición a Internet. Implemente una instancia de Load Balancer externa con la traducción de direcciones de red de origen (SNAT) para enmascarar las direcciones IP de los recursos de back-end para la protección frente a la exposición directa a Internet.

Azure ofrece dos tipos de instancias de Load Balancer: Standard y Basic. Use Standard Load Balancer para todas las cargas de trabajo de producción. Implementar grupos de seguridad de red y permita solo el acceso a los intervalos de direcciones IP y puertos de confianza de la aplicación. En los casos en los que no hay ningún grupo de seguridad de red asignado a la subred de back-end o NIC de las máquinas virtuales de back-end, no se permitirá el tráfico en estos recursos del equilibrador de carga. Con Standard Load Balancer, proporcione reglas de salida para definir la NAT de salida con un grupo de seguridad de red. Revise estas reglas de salida para ajustar el comportamiento de las conexiones salientes. 

Se recomienda usar una instancia de Standard Load Balancer para las cargas de trabajo de producción y, normalmente, Basic Load Balancer solo se usa para pruebas, ya que el tipo básico está abierto para las conexiones desde Internet de manera predeterminada y no requiere grupos de seguridad de red para su funcionamiento. 

- [Conexiones salientes en Azure](load-balancer-outbound-connections.md)

- [Actualización de Azure Load Balancer público](./upgrade-basic-standard.md)

**Supervisión de Azure Security Center**: Sí

**Responsabilidad**: Customer

### <a name="12-monitor-and-log-the-configuration-and-traffic-of-virtual-networks-subnets-and-nics"></a>1.2: Supervisión y registro de la configuración y el tráfico de redes virtuales, subredes y NIC

**Guía**: Load Balancer es un servicio de paso a través, ya que se basa en las reglas de grupos de seguridad de red que se aplican a los recursos de back-end y las reglas de salida configuradas para controlar el acceso a Internet.

Revise las reglas de salida configuradas para Standard Load Balancer a través de la hoja de reglas de salida de Load Balancer y la hoja de reglas de equilibrio de carga donde puede tener habilitadas reglas de salida implícitas.

Supervise el número de conexiones salientes para realizar un seguimiento de la frecuencia con la que los recursos llegan a Internet. 

Use Security Center y siga las recomendaciones de protección de redes para ayudar a proteger sus recursos de red de Azure.

Siga las recomendaciones de seguridad para los recursos de back-end y habilite los registros de flujo de los grupos de seguridad de red y envíe los registros a una cuenta de Azure Storage para su auditoría.

Envíe también los registros de flujo a un área de trabajo de Log Analytics y luego use Análisis de tráfico para proporcionar información detallada sobre los patrones de tráfico en la nube de Azure. Algunas de las ventajas del Análisis de tráfico son la capacidad de visualizar la actividad de la red, identificar las zonas activas y las amenazas de seguridad, comprender los patrones de flujo de tráfico y detectar configuraciones de red incorrectas.

- [Habilitación de los registros de flujo de grupos de seguridad de red](../network-watcher/network-watcher-nsg-flow-logging-portal.md)

- [Habilitación y uso del Análisis de tráfico](../network-watcher/traffic-analytics.md)

- [Descripción de la seguridad de red proporcionada por Azure Security Center](../security-center/security-center-network-recommendations.md)

- [Comprobación de las estadísticas de conexión saliente](./load-balancer-standard-diagnostics.md#how-do-i-check-my-outbound-connection-statistics)

**Supervisión de Azure Security Center**: Sí

**Responsabilidad**: Customer

### <a name="13-protect-critical-web-applications"></a>1.3: Proteja las aplicaciones web críticas

**Guía**: Defina explícitamente la conectividad a Internet y las direcciones IP de origen válidas a través de reglas de salida y grupos de seguridad de red con la Load Balancer para usar la inteligencia sobre amenazas de Microsoft para proteger las aplicaciones web.

- [Integración de Azure Firewall](../firewall/integrate-lb.md)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="14-deny-communications-with-known-malicious-ip-addresses"></a>1.4: Deniegue las comunicaciones con direcciones IP malintencionadas conocidas

**Guía**: Habilite la protección contra denegación de servicio distribuida (DDoS) estándar de Azure en las Azure Virtual Network para protegerse frente a ataques DDoS. 

Implemente Azure Firewall en cada uno de los límites de red de la organización con el filtrado basado en inteligencia de amenazas habilitado y configurado para alertar y denegar el tráfico de red malintencionado.

 

Utilice la protección contra amenazas de Security Center para detectar comunicaciones con direcciones IP malintencionadas conocidas. 

Standard Load Balancer está diseñado para ser seguro de manera predeterminada y parte de una instancia de Virtual Network privada y aislada. Se cierra en los flujos de entrada, a menos que los grupos de seguridad de red lo abran para permitir explícitamente el tráfico permitido y para no permitir direcciones IP malintencionadas conocidas. A menos que haya un grupo de seguridad de red en una subred o NIC del recurso de máquina virtual detrás de Load Balancer, no se permite que el tráfico llegue a este recurso. 

Implemente Azure Firewall en cada uno de los límites de red de la organización con el filtrado basado en inteligencia de amenazas habilitado y configurado para alertar y denegar el tráfico de red malintencionado para evitar ataques de direcciones IP malintencionadas. 

Implemente la guía para integrar Azure Firewall con Load Balancer.

Utilice las características de protección contra amenazas de Security Center para detectar comunicaciones con direcciones IP malintencionadas conocidas. 

Security Center (nivel estándar) proporciona acceso a máquinas virtuales Just-in-Time y configura las direcciones IP de origen permitidas para que permitan el acceso solo desde intervalos o direcciones IP que se hayan aprobado.
 

Use la característica de protección de redes adaptables de Security Center para recomendar las configuraciones de los grupos de seguridad de red que limitan los puertos y las direcciones IP de origen según el tráfico real y la inteligencia sobre amenazas.
 

- [Administración de Azure DDoS Protection estándar mediante Azure Portal](../ddos-protection/manage-ddos-protection.md)

- [Filtrado basado en inteligencia sobre amenazas de Azure Firewall](../firewall/threat-intel.md)

- [Protección contra amenazas en Azure Security Center](../security-center/azure-defender.md)

- [Protección de los puertos de administración con acceso Just-in-Time](../security-center/security-center-just-in-time.md)

- [Protección de red adaptable en Azure Security Center](../security-center/security-center-adaptive-network-hardening.md)

- [Integración de Azure Firewall con Load Balancer](../firewall/overview.md)

**Supervisión de Azure Security Center**: Sí

**Responsabilidad**: Customer

### <a name="15-record-network-packets"></a>1.5: Registro de los paquetes de red

**Guía**: Habilite la captura de paquetes de Network Watcher para investigar las actividades anómalas.

- [Creación de una instancia de Network Watcher](../network-watcher/network-watcher-create.md)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="16-deploy-network-based-intrusion-detectionintrusion-prevention-systems-idsips"></a>1.6: Implementación de sistemas de prevención de intrusiones y detección de intrusiones (IDS/IPS) basados en la red

**Guía**: Implemente una oferta de Azure Marketplace que admita la funcionalidad de IDS/IPS con funcionalidades de inspección de carga para el entorno de Load Balancer. 

Si la inspección de la carga no es un requisito, use la inteligencia sobre amenazas de Azure Firewall. El filtrado basado en inteligencia sobre amenazas de Azure Firewall se usa para alertar o bloquear el tráfico desde y hacia dominios y direcciones IP malintencionados conocidos. La direcciones IP y los dominios proceden de la fuente Inteligencia sobre amenazas de Microsoft.

Implemente la solución de firewall que prefiera en cada uno de los límites de red de su organización para detectar o bloquear el tráfico malintencionado.

- [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/?term=Firewall)

- [Implementación de Azure Firewall](../firewall/tutorial-firewall-deploy-portal.md)

- [Configuración de alertas con Azure Firewall](../firewall/threat-intel.md)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="17-manage-traffic-to-web-applications"></a>1.7: Administre el tráfico a las aplicaciones web

**Guía**: Defina explícitamente la conectividad a Internet y las direcciones IP de origen válidas a través de reglas de salida y grupos de seguridad de red con Load Balancer para usar las características de inteligencia sobre amenazas de Microsoft para proteger las aplicaciones web.

- [Integración de Azure Firewall](../firewall/integrate-lb.md)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="18-minimize-complexity-and-administrative-overhead-of-network-security-rules"></a>1.8: Minimice la complejidad y la sobrecarga administrativa de las reglas de seguridad de red

**Guía**: Use etiquetas de servicio en lugar de direcciones IP específicas al crear reglas de seguridad. Especifique el nombre de la etiqueta de servicio en el campo de origen o destino de una regla para permitir o denegar el tráfico para el servicio correspondiente. 

Microsoft administra los prefijos de direcciones que la etiqueta de servicio incluye y actualiza automáticamente dicha etiqueta a medida que las direcciones cambian. 

De forma predeterminada, todos los grupos de seguridad de red incluyen la etiqueta de servicio AzureLoadBalancer para permitir el tráfico de sondeo de estado. 

Consulte la documentación de Azure de todas las etiquetas de servicio disponibles que se pueden usar en reglas de grupos de seguridad de red.

- [Etiquetas de servicio disponibles](../virtual-network/service-tags-overview.md#available-service-tags)

**Supervisión de Azure Security Center**: Sí

**Responsabilidad**: Customer

### <a name="19-maintain-standard-security-configurations-for-network-devices"></a>1.9: Mantenga las configuraciones de seguridad estándar para dispositivos de red

**Guía**: Defina e implemente configuraciones de seguridad estándar para los recursos de red con Azure Policy.

Use Azure Blueprints para simplificar las implementaciones de Azure a gran escala mediante el empaquetado de artefactos de entorno clave, como plantillas de Azure Resource Manager, controles de Azure RBAC y directivas, en una única definición de plano técnico. 

Aplique el plano técnico a nuevas suscripciones y ajuste el control y la administración a través del control de versiones.

- [Configuración y administración de Azure Policy](../governance/policy/tutorials/create-and-manage.md)

- [Ejemplos de Azure Policy para redes](../governance/policy/samples/built-in-policies.md#network)

- [Creación de un plano técnico de Azure](../governance/blueprints/create-blueprint-portal.md)

**Supervisión de Azure Security Center**: Sí

**Responsabilidad**: Customer

### <a name="110-document-traffic-configuration-rules"></a>1.10: Documente las reglas de configuración de tráfico

**Guía**: Use etiquetas de recurso para grupos de seguridad de red y otros recursos relacionados con la seguridad de red y el flujo de tráfico. 

Use el campo "Descripción" para documentar las reglas que permiten el tráfico con origen y destino en una red para las reglas de grupo de seguridad de red individuales.

Implemente cualquiera de las definiciones de Azure Policy integradas relacionadas con el etiquetado, como "Requerir etiqueta y su valor", que garantice que todos los recursos se creen con etiquetas y para notificar los recursos no etiquetados existentes.

Use Azure PowerShell o la CLI de Azure para buscar o realizar acciones en los recursos en función de sus etiquetas.

- [Creación y uso de etiquetas](../azure-resource-manager/management/tag-resources.md)

- [Creación de una red virtual de Azure](../virtual-network/quick-create-portal.md)

- [Filtrado del tráfico de red con reglas de grupos de seguridad de red](../virtual-network/tutorial-filter-network-traffic.md)

**Supervisión de Azure Security Center**: Sí

**Responsabilidad**: Customer

### <a name="111-use-automated-tools-to-monitor-network-resource-configurations-and-detect-changes"></a>1.11: Use herramientas automatizadas para supervisar las configuraciones de recursos de red y detectar cambios

**Guía**: Use el registro de actividad de Azure para supervisar las configuraciones de recursos y detectar cambios en los recursos de Azure. 

Cree alertas en Azure Monitor para recibir una notificación cuando se produzcan cambios en recursos críticos.

- [Visualización y recuperación de eventos del registro de actividad de Azure](../azure-monitor/platform/activity-log.md#view-the-activity-log)

- [Creación de alertas en Azure Monitor](../azure-monitor/platform/alerts-activity-log.md)

**Supervisión de Azure Security Center**: Sí

**Responsabilidad**: Customer

## <a name="logging-and-monitoring"></a>Registro y supervisión

*Para obtener más información, consulte [Azure Security Benchmark: registro y supervisión](../security/benchmarks/security-control-logging-monitoring.md).*

### <a name="22-configure-central-security-log-management"></a>2.2: Configuración de la administración central de registros de seguridad

**Guía**: Revise los cambios en las reglas de salida y los grupos de seguridad de red de Load Balancer mediante la visualización del registro de actividad en las suscripciones. 

Vea los registros de host internos para asegurarse de que los recursos de back-end son seguros.

Exporte estos registros a Log Analytics u otra plataforma de almacenamiento. En Azure Monitor, use áreas de trabajo de Log Analytics para realizar consultas y análisis, y use cuentas de Azure Storage para el almacenamiento de archivos a largo plazo.

Habilite los datos integrados en Azure Sentinel o en una SIEM de terceros, en función de los requisitos empresariales de la organización.

- [Incorporación de Azure Sentinel](../sentinel/quickstart-onboard.md)

- [Recopilación de registros y métricas de plataforma con Azure Monitor](../azure-monitor/platform/diagnostic-settings.md)

- [Recopilación de registros de host internos de máquina virtual de Azure con Azure Monitor](../azure-monitor/learn/quick-collect-azurevm.md)

- [Introducción a Azure Monitor e integración con herramienta SIEM de terceros](https://azure.microsoft.com/blog/use-azure-monitor-to-integrate-with-siem-tools/)

- [Registros de actividad de plataforma](../azure-monitor/platform/activity-log.md)

**Supervisión de Azure Security Center**: Sí

**Responsabilidad**: Customer

### <a name="23-enable-audit-logging-for-azure-resources"></a>2.3: Habilitación del registro de auditoría para recursos de Azure

**Guía**: Revise la información de auditoría y el registro del plano de administración y control capturados con registros de actividad para Basic Load Balancer. Esta configuración de captura está habilitada de manera predeterminada. 

Use los registros de actividad para supervisar las acciones en los recursos para ver todas las actividades y su estado. 

Determine qué operaciones se efectuaron en los recursos de la suscripción con los registros de actividad: 

- quién inició la operación
- cuándo tuvo lugar la operación
- el estado de la operación

- los valores de otras propiedades que podrían ayudarle en la investigación de la operación

puede recuperar información de los registros de actividad mediante Azure PowerShell, la interfaz de la línea de comandos (CLI) de Azure, la API REST de Azure o Azure Portal. 

Implemente funcionalidades de diagnóstico multidimensionales para configuraciones de Standard Load Balancer con Azure Monitor.  Entre estas métricas disponibles para la seguridad se incluye información sobre las conexiones de traducción de direcciones de red (SNAT) de origen y puertos. También hay disponibles métricas adicionales en los paquetes SYN (sincronizar) y los contadores de paquetes. 

Recupere métricas multidimensionales mediante programación a través de las API y escríbalas en una cuenta de almacenamiento mediante la opción "Todas las métricas".

Use el paquete de contenido de los registros de auditoría de Azure con Microsoft Power BI para analizar los datos con los paneles preconfigurados o para personalizar las vistas en función de sus necesidades.

Transmita los registros a un centro de eventos o a un área de trabajo de Log Analytics. También se pueden extraer desde Azure Blob Storage y visualizarse en distintas herramientas, como Excel y Power BI. 

Habilite los datos integrados en Azure Sentinel o en una SIEM de terceros, en función de sus requisitos empresariales.

- [Revise este artículo con instrucciones paso a paso para cada método detalladas en las operaciones de auditoría con Resource Manager](../azure-resource-manager/management/view-activity-logs.md).

- [Registros de Azure Monitor para el equilibrador de carga básica público](./load-balancer-monitor-log.md)

- [Visualización de registros de actividad para supervisar acciones sobre recursos](../azure-resource-manager/management/view-activity-logs.md)

- [Recuperación de las métricas multidimensionales mediante programación con API](./load-balancer-standard-diagnostics.md#retrieve-multi-dimensional-metrics-programmatically-via-apis)

- [Introducción a Azure Monitor e integración con herramienta SIEM de terceros](https://azure.microsoft.com/blog/use-azure-monitor-to-integrate-with-siem-tools)

**Supervisión de Azure Security Center**: Sí

**Responsabilidad**: Customer

### <a name="25-configure-security-log-storage-retention"></a>2.5: Configuración de la retención del almacenamiento de registros de seguridad

**Guía**: El registro de actividad está habilitado de manera predeterminada y se conserva durante 90 días en el almacén de registros de eventos de Azure. Establezca el período de retención del área de trabajo de Log Analytics de acuerdo con la normativa de cumplimiento de su organización en Azure Monitor. Use cuentas de Azure Storage para el almacenamiento de archivos y a largo plazo.

- [Artículo de visualización de registros de actividad para supervisar acciones sobre recursos](../azure-resource-manager/management/view-activity-logs.md)

- [Cambio del período de retención de datos en Log Analytics](../azure-monitor/platform/manage-cost-storage.md#change-the-data-retention-period)

- [Configuración de la directiva de retención para los registros de la cuenta de Azure Storage](../storage/common/storage-monitor-storage-account.md#configure-logging)

**Supervisión de Azure Security Center**: Sí

**Responsabilidad**: Customer

### <a name="26-monitor-and-review-logs"></a>2.6: Supervisión y registros de revisión

**Guía**: Supervise, administre y solucione los problemas de los recursos de Standard Load Balancer mediante la página Load Balancer en Azure Portal y la página Resource Health en Azure Monitor. Las métricas disponibles para la seguridad incluyen información sobre las conexiones de traducción de direcciones de red (SNAT) de origen y puertos. También hay disponibles métricas adicionales en los paquetes SYN (sincronizar) y los contadores de paquetes. 

Use Azure Monitor para revisar el estado de sondeo de estado del punto de conexión con métricas multidimensionales para instancias de Load Balancer externas e internas y Standard Load Balancer. Recopile estas métricas mediante programación a través de las API y escríbalas en una cuenta de almacenamiento mediante la opción "Todas las métricas".

Los registros de Azure Monitor no están disponible para las instancias internas de Basic Load Balancer. 

Use Azure Monitor para ver el estado de sondeo de estado resumido por grupo de back-end para las instancias externas de Basic Load Balancer, como el número de instancias del grupo de back-end que no reciben solicitudes de Load Balancer debido a errores de sondeo de estado. 

Implemente el registro con Azure Operational Insights para proporcionar estadísticas del estado de mantenimiento del equilibrador de carga. 

Use los registros de actividad para ver alertas y supervisar las acciones de los recursos y su estado en las suscripciones de Azure. Los registros de actividad están habilitados de manera predeterminada y se pueden ver en Azure Portal. 

Use Microsoft Power BI con el paquete de contenido de registros de auditoría de Azure y analice los datos con los paneles preconfigurados. Personalice las vistas para que se adapten a sus requisitos en función de las necesidades de la empresa. 

Transmita los registros a un centro de eventos o a un área de trabajo de Log Analytics. También se pueden extraer desde Azure Blob Storage y visualizarse en distintas herramientas, como Excel y Power BI. Puede habilitar e incorporar datos en Azure Sentinel o en una herramienta SIEM de terceros.

- [Sondeos de estado de Load Balancer](./load-balancer-custom-probe-overview.md)

- [API de REST de Azure Monitor](/rest/api/monitor)

- [Recuperación de métricas a través de la API REST](/rest/api/monitor/metrics/list)

- [Diagnóstico de Standard Load Balancer con métricas, alertas y estado de los recursos](./load-balancer-standard-diagnostics.md)

- [Registros de Azure Monitor para el equilibrador de carga básica público](./load-balancer-monitor-log.md)

- [Visualización de las métricas del equilibrador de carga en Azure Portal](./load-balancer-standard-diagnostics.md#view-your-load-balancer-metrics-in-the-azure-portal)

**Supervisión de Azure Security Center**: Sí

**Responsabilidad**: Customer

### <a name="27-enable-alerts-for-anomalous-activities"></a>2.7: Habilitación de alertas para actividades anómalas

**Guía**: Use Security Center con el área de trabajo de Log Analytics para la supervisión y alertas sobre actividades anómalas relacionadas con Load Balancer en los registros y eventos de seguridad.

Habilite e incorpore datos en Azure Sentinel o en una herramienta SIEM de terceros.

- [Incorporación de Azure Sentinel](../sentinel/quickstart-onboard.md)

- [Administración de alertas de seguridad en Azure Security Center](../security-center/security-center-managing-and-responding-alerts.md)

- [Alertas sobre datos de registro de Log Analytics](../azure-monitor/learn/tutorial-response.md)

**Supervisión de Azure Security Center**: Sí

**Responsabilidad**: Customer

### <a name="28-centralize-anti-malware-logging"></a>2.8: Centralización del registro antimalware

**Guía**: No se aplica a Azure Load Balancer. esta recomendación está pensada para los recursos de proceso.

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="29-enable-dns-query-logging"></a>2.9: Habilitación del registro de consultas DNS

**Guía**: No es aplicable porque Azure Load Balancer es un servicio de red principal que no realiza consultas de DNS.

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="210-enable-command-line-audit-logging"></a>2.10: Habilitación del registro de auditoría de la línea de comandos

**Guía**: No es aplicable a Azure Load Balancer, ya que esta recomendación se aplica a los recursos de proceso.

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

## <a name="identity-and-access-control"></a>Control de identidades y acceso

*Para obtener más información, consulte [Azure Security Benchmark: Identidad y control de acceso](../security/benchmarks/security-control-identity-access-control.md).*

### <a name="31-maintain-an-inventory-of-administrative-accounts"></a>3.1: Mantenga un inventario de cuentas administrativas

**Guía**: El control de acceso basado en roles de Azure (Azure RBAC) permite administrar el acceso a los recursos de Azure, como Load Balancer, a través de las asignaciones de roles. Asigne estos roles a usuarios, grupos de entidades de servicio e identidades administradas. 

Inventaríe los roles integrados y predefinidos para determinados recursos con herramientas como la CLI de Azure, Azure PowerShell o Azure Portal.

- [Obtención de un rol de directorio en Azure AD con PowerShell](/powershell/module/azuread/get-azureaddirectoryrole?view=azureadps-2.0)

- [Obtención de los miembros de un rol de directorio en Azure AD con PowerShell](/powershell/module/azuread/get-azureaddirectoryrolemember?view=azureadps-2.0)

**Supervisión de Azure Security Center**: Sí

**Responsabilidad**: Customer

## <a name="data-protection"></a>Protección de los datos

*Para obtener más información, consulte [Azure Security Benchmark: Protección de datos](../security/benchmarks/security-control-data-protection.md).*

### <a name="46-use-azure-rbac-to-manage-access-to-resources"></a>4.6: Uso de RBAC de Azure para administrar el acceso a los recursos

**Guía**: Use Azure RBAC para controlar el acceso a los recursos de Load Balancer.

- [Configuración de Azure RBAC](../role-based-access-control/role-assignments-portal.md)

**Supervisión de Azure Security Center**: Sí

**Responsabilidad**: Customer

### <a name="47-use-host-based-data-loss-prevention-to-enforce-access-control"></a>4.7: Uso de la prevención de pérdida de datos basada en host para aplicar el control de acceso

**Guía**: Load Balancer es un servicio de paso a través que no almacena los datos de los clientes. Forma parte de la plataforma subyacente administrada por Microsoft. 

Microsoft trata todo el contenido de los clientes como confidencial y hace grandes esfuerzos para proteger a los clientes contra la pérdida y exposición de sus datos. 

Para garantizar la seguridad de los datos de los clientes en Azure, Microsoft ha implementado y mantiene un conjunto de controles y funcionalidades eficaces de protección de datos. 

- [Descripción de la protección de datos de los clientes en Azure](../security/fundamentals/protection-customer-data.md)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Compartido

### <a name="49-log-and-alert-on-changes-to-critical-azure-resources"></a>4.9: Registro y alerta de cambios en los recursos críticos de Azure

**Guía**: Use Azure Monitor con el registro de actividad de Azure para crear alertas para cuando se produzcan cambios en los recursos críticos de Azure, como instancias de Load Balancer usadas para importantes cargas de trabajo de producción.

- [Creación de alertas para los eventos del registro de actividad de Azure](../azure-monitor/platform/alerts-activity-log.md)

**Supervisión de Azure Security Center**: Sí

**Responsabilidad**: Customer

## <a name="inventory-and-asset-management"></a>Inventario y administración de recursos

*Para obtener más información, consulte [Azure Security Benchmark: Administración de recursos y del inventario](../security/benchmarks/security-control-inventory-asset-management.md).*

### <a name="61-use-automated-asset-discovery-solution"></a>6.1: Uso de la solución de detección de recursos automatizada

**Guía**: Use Azure Resource Graph para consultar y detectar todos los recursos (por ejemplo, proceso, almacenamiento, red, puertos, protocolos, etc.) en las suscripciones. Se recomienda Azure Resource Manager para crear y usar los recursos actuales. 

Asegúrese de que tiene los permisos adecuados (lectura) en el inquilino y enumere todas las suscripciones de Azure, así como los recursos en las suscripciones.

- [Creación de consultas con Azure Resource Graph Explorer](../governance/resource-graph/first-query-portal.md)

- [Visualización de las suscripciones de Azure](/powershell/module/az.accounts/get-azsubscription?view=azps-3.0.0)

- [Descripción de Azure RBAC](../role-based-access-control/overview.md)

**Supervisión de Azure Security Center**: Sí

**Responsabilidad**: Customer

### <a name="62-maintain-asset-metadata"></a>6.2: Mantenimiento de metadatos de recursos

**Guía**: Aplique etiquetas a los recursos de Azure con metadatos para organizarlos de forma lógica según una taxonomía.

- [Creación y uso de etiquetas](../azure-resource-manager/management/tag-resources.md)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="63-delete-unauthorized-azure-resources"></a>6.3: Eliminación de recursos de Azure no autorizados

**Guía**: Use el etiquetado, los grupos de administración y las suscripciones independientes, si procede, para organizar y realizar un seguimiento de los recursos. 

Concilie el inventario periódicamente y asegúrese de que los recursos no autorizados se eliminan de la suscripción de manera oportuna.

- [Creación de suscripciones adicionales de Azure](../cost-management-billing/manage/create-subscription.md)

- [Creación de grupos de administración](../governance/management-groups/create-management-group-portal.md)

- [Creación y uso de etiquetas](../azure-resource-manager/management/tag-resources.md)

**Supervisión de Azure Security Center**: Sí

**Responsabilidad**: Customer

### <a name="64-define-and-maintain-an-inventory-of-approved-azure-resources"></a>6.4: Definición y mantenimiento de un inventario de los recursos de Azure aprobados

**Guía**: Cree una lista de recursos de Azure aprobados según sus necesidades organizativas, que puede aprovechar como mecanismo de lista de permitidos. Esto permitirá que la organización incorpore todos los servicios de Azure disponibles recientemente una vez que se hayan revisado y aprobado formalmente según los procesos de evaluación de seguridad típicos de la organización.

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="65-monitor-for-unapproved-azure-resources"></a>6.5: Supervisión de recursos de Azure no aprobados

**Guía**: Use Azure Policy para establecer restricciones en el tipo de recursos que se pueden crear en sus suscripciones.

Consulte y detecte recursos con Azure Resource Graph dentro de sus suscripciones. 

Asegúrese de que todos los recursos de Azure presentes en el entorno estén aprobados.

- [Configuración y administración de Azure Policy](../governance/policy/tutorials/create-and-manage.md)

- [Creación de consultas con Azure Resource Graph Explorer](../governance/resource-graph/first-query-portal.md)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="611-limit-users-ability-to-interact-with-azure-resource-manager"></a>6.11: Limitación de la capacidad de los usuarios para interactuar con Azure Resource Manager

**Guía**: Use el acceso condicional de Azure AD para limitar la capacidad de los usuarios de interactuar con Azure Resource Manager mediante la configuración de la opción "Bloquear acceso" en la aplicación "Administración de Microsoft Azure".

- [Configuración del acceso condicional para bloquear el acceso a Azure Resource Manager](../role-based-access-control/conditional-access-azure-management.md)

**Supervisión de Azure Security Center**: Sí

**Responsabilidad**: Customer

### <a name="613-physically-or-logically-segregate-high-risk-applications"></a>6.13: Segregación física o lógica de aplicaciones de alto riesgo

**Guía**: El software que se requiere para las operaciones empresariales, pero que puede suponer un riesgo mayor para la organización, debe aislarse en su propia máquina virtual o red virtual y estar lo suficientemente protegido con Azure Firewall o un grupo de seguridad de red.

- [Creación de una red virtual](../virtual-network/quick-create-portal.md)

- [Creación de un grupo de seguridad de red con una configuración de seguridad](../virtual-network/tutorial-filter-network-traffic.md)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

## <a name="secure-configuration"></a>Configuración segura

*Para obtener más información, consulte [Azure Security Benchmark: Configuración segura](../security/benchmarks/security-control-secure-configuration.md).*

### <a name="71-establish-secure-configurations-for-all-azure-resources"></a>7.1: Establezca configuraciones seguras para todos los recursos de Azure

**Guía**: use alias de Azure Policy para crear directivas personalizadas con el fin de auditar o aplicar la configuración de los recursos de Azure. Use definiciones de Azure Policy integradas.

Azure Resource Manager tiene la capacidad de exportar la plantilla en notación de objetos JavaScript (JSON), que se debe revisar para asegurarse de que las configuraciones cumplan los requisitos de seguridad de la organización.

Exporte plantillas de Azure Resource Manager en formatos de notación de objetos JavaScript (JSON) y revíselos periódicamente para asegurarse de que las configuraciones satisfacen los requisitos de seguridad de la organización. 

Implemente las recomendaciones de Security Center como un línea base de configuración segura para los recursos de Azure. 

- [Visualización de los alias de Azure Policy disponibles](/powershell/module/az.resources/get-azpolicyalias?view=azps-3.3.0)

- [Tutorial: Creación y administración de directivas para aplicar el cumplimiento](../governance/policy/tutorials/create-and-manage.md)

- [Exportación de uno y varios recursos a una plantilla en Azure Portal](../azure-resource-manager/templates/export-template-portal.md)

- [Guía de referencia sobre las recomendaciones de seguridad](../security-center/recommendations-reference.md)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="73-maintain-secure-azure-resource-configurations"></a>7.3: Mantenga configuraciones de recursos de Azure seguras

**Guía**: Utilice las directivas [deny] y [deploy if not exist] de Azure Policy para aplicar una configuración segura en los recursos de Azure.  Además, puede usar plantillas de Azure Resource Manager para mantener la configuración de seguridad de los recursos de Azure que requiere su organización. 

- [Descripción de los efectos de Azure Policy](../governance/policy/concepts/effects.md)

- [Creación y administración de directivas para aplicar el cumplimiento](../governance/policy/tutorials/create-and-manage.md)

- [Información general sobre plantillas de Azure Resource Manager](../azure-resource-manager/templates/overview.md)

**Supervisión de Azure Security Center**: Sí

**Responsabilidad**: Customer

### <a name="75-securely-store-configuration-of-azure-resources"></a>7.5: Almacene de forma segura la configuración de los recursos de Azure

**Guía**: Use Azure DevOps para almacenar y administrar de forma segura el código, como definiciones de Azure Policy personalizadas, plantillas de Azure Resource Manager y scripts de Desired State Configuration. 

Conceda o deniegue permisos a usuarios específicos, grupos de seguridad integrados o grupos definidos en Azure Active Directory (Azure AD) si se integra con Azure DevOps, o en Active Directory si se integra con TFS.

- [Almacenamiento de código en Azure DevOps](/azure/devops/repos/git/gitworkflow?view=azure-devops)

- [Acerca de los permisos y los grupos en Azure DevOps](/azure/devops/organizations/security/about-permissions)

**Supervisión de Azure Security Center**: Sí

**Responsabilidad**: Customer

### <a name="77-deploy-configuration-management-tools-for-azure-resources"></a>7.7: Implementación de herramientas de administración de configuración para recursos de Azure

**Guía**: defina e implemente configuraciones de seguridad estándar para los recursos de Azure con Azure Policy.  Use alias de Azure Policy para crear directivas personalizadas con el fin de auditar o aplicar la configuración de red de los recursos de Azure. Implemente definiciones de directivas integradas relacionadas con recursos concretos de Azure Load Balancer.  Además, puede usar Azure Automation para implementar los cambios de configuración en los recursos de Azure Load Balancer.

- [Configuración y administración de Azure Policy](../governance/policy/tutorials/create-and-manage.md)

- [Uso de alias](../governance/policy/concepts/definition-structure.md#aliases)

**Supervisión de Azure Security Center**: Sí

**Responsabilidad**: Customer

### <a name="79-implement-automated-configuration-monitoring-for-azure-resources"></a>7.9: Implementación de la supervisión de la configuración automatizada para los recursos de Azure

**Guía**: Use Security Center para realizar análisis de línea de base de los recursos de Azure y Azure Policy para alertas y auditorías de configuraciones de recursos.

- [Corrección de recomendaciones en Azure Security Center](../security-center/security-center-remediate-recommendations.md)

**Supervisión de Azure Security Center**: Sí

**Responsabilidad**: Customer

## <a name="incident-response"></a>Respuesta a los incidentes

*Para obtener más información, consulte [Azure Security Benchmark: Respuesta a los incidentes](../security/benchmarks/security-control-incident-response.md).*

### <a name="102-create-an-incident-scoring-and-prioritization-procedure"></a>10.2: Creación de un procedimiento de priorización y puntuación de incidentes

**Instrucciones**: Security Center asigna una gravedad a cada alerta para ayudarle a priorizar aquellas que se deben investigar en primer lugar. 

La gravedad se basa en la confianza que tiene Security Center en la búsqueda o en el análisis utilizados para emitir la alerta, así como en el nivel de confianza de que ha habido un intento malintencionado detrás de la actividad que ha provocado la alerta.

Marque las suscripciones con etiquetas y cree un sistema de nomenclatura para identificar y clasificar los recursos de Azure, especialmente los que procesan datos confidenciales.  

Es su responsabilidad asignar prioridades a la corrección de las alertas en función de la importancia de los recursos y el entorno de Azure donde se produjo el incidente.

- [Alertas de seguridad en el Centro de seguridad de Azure](../security-center/security-center-alerts-overview.md)

- [Uso de etiquetas para organizar los recursos de Azure](../azure-resource-manager/management/tag-resources.md)

**Supervisión de Azure Security Center**: Sí

**Responsabilidad**: Customer

### <a name="105-incorporate-security-alerts-into-your-incident-response-system"></a>10.5: Incorporación de alertas de seguridad en el sistema de respuesta a incidentes

**Guía**: Exporte las alertas y recomendaciones de Security Center mediante la característica de exportación continua para identificar riesgos en los recursos de Azure. 

El uso de la exportación continua en Security Center permite exportar alertas y recomendaciones de forma manual o continua. 

Utilice el conector de datos de Security Center para enviar las alertas a Azure Sentinel.

- [Configuración de la exportación continua](../security-center/continuous-export.md)

- [Transmisión de alertas a Azure Sentinel](../sentinel/connect-azure-security-center.md)

**Supervisión de Azure Security Center**: Sí

**Responsabilidad**: Customer

### <a name="106-automate-the-response-to-security-alerts"></a>10.6: Automatización de la respuesta a las alertas de seguridad

**Guía**: Use la característica de automatización de flujos de trabajo de Security Center para desencadenar automáticamente las alertas y recomendaciones de seguridad con el fin de proteger los recursos de Azure.

- [Configuración de la automatización de flujos de trabajo en Security Center](../security-center/workflow-automation.md)

**Supervisión de Azure Security Center**: Sí

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
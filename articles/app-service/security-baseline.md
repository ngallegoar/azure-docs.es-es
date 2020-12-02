---
title: Base de referencia de seguridad de Azure para App Service
description: La base de referencia de seguridad de App Service proporciona instrucciones sobre procedimientos y recursos para implementar las recomendaciones de seguridad especificadas en Azure Security Benchmark.
author: msmbaldwin
ms.service: app-service
ms.topic: conceptual
ms.date: 11/17/2020
ms.author: mbaldwin
ms.custom: subject-security-benchmark
ms.openlocfilehash: 691bb538ea0f46e26c318a68e9f66a9a986eb5d4
ms.sourcegitcommit: ab94795f9b8443eef47abae5bc6848bb9d8d8d01
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/27/2020
ms.locfileid: "96301836"
---
# <a name="azure-security-baseline-for-app-service"></a>Base de referencia de seguridad de Azure para App Service

La base de referencia de seguridad de Azure para App Service contiene recomendaciones que le ayudarán a mejorar la posición de seguridad de la implementación. La base de referencia de este servicio se extrae de la [versión 1.0 de Azure Security Benchmark](../security/benchmarks/overview-v1.md), que proporciona recomendaciones sobre cómo puede proteger las soluciones en la nube en Azure con nuestra guía de procedimientos recomendados. El contenido se agrupa por medio de los **controles de seguridad** definidos por Azure Security Benchmark y las directrices relacionadas aplicables a App Service. Se han excluido los **controles** no aplicables a App Service.

Para ver cómo App Service se asigna por completo a Azure Security Benchmark, consulte el [archivo completo de asignación de base de referencia de seguridad de App Service](https://github.com/MicrosoftDocs/SecurityBenchmarks/tree/master/Azure%20Offer%20Security%20Baselines).

## <a name="network-security"></a>Seguridad de las redes

*Para obtener más información, consulte [Azure Security Benchmark: Seguridad de redes](../security/benchmarks/security-control-network-security.md).*

### <a name="11-protect-azure-resources-within-virtual-networks"></a>1.1: Protección de los recursos de Azure dentro de las redes virtuales

**Guía**: Cuando se usa App Service en el plan de tarifa aislado, también llamado App Service Environment (ASE), puede implementarlo directamente en una subred de la red virtual de Azure. Use grupos de seguridad de red para proteger Azure App Service Environment mediante el bloqueo del tráfico entrante y saliente de los recursos de la red virtual, o para restringir el acceso a las aplicaciones de App Service Environment.

De forma predeterminada, los grupos de seguridad de red incluyen una regla de denegación implícita con la prioridad más baja, que requiere que se agreguen explícitamente reglas de permiso. Agregue reglas de permiso para el grupo de seguridad de red según una estrategia de redes con menos privilegios. No se puede acceder directamente a las máquinas virtuales subyacentes que se usan para hospedar la instancia de App Service Environment porque se encuentran en una suscripción administrada por Microsoft.

Para proteger una instancia de App Service Environment, enrute el tráfico a través de una instancia de Azure Application Gateway habilitada para Web Application Firewall (WAF). Use puntos de conexión de servicio junto con Application Gateway para proteger el tráfico de publicación de entrada a la aplicación.  

En la instancia de App Service multiinquilino (una aplicación que no está en un nivel aislado), use grupos de seguridad de red para bloquear el tráfico que sale de la aplicación. Permita que las aplicaciones accedan a los recursos de una red virtual, o mediante esta, con la característica Integración de Virtual Network. Esta característica también se puede usar para bloquear el tráfico saliente a las direcciones públicas desde la aplicación. Integración de Virtual Network no se puede usar para proporcionar acceso de entrada a una aplicación.  

Proteja el tráfico entrante a la aplicación con:
- Restricciones de acceso: una serie de reglas de permiso o denegación que controlan el acceso de entrada.
- Puntos de conexión de servicio: pueden denegar el tráfico entrante desde fuera de las redes virtuales o subredes especificadas.
- Puntos de conexión privados: exponen la aplicación a la instancia de Virtual Network con una dirección IP privada. Con los puntos de conexión privados habilitados en la aplicación, ya no es accesible desde Internet.

Al usar la característica Integración de Virtual Network con redes virtuales de la misma región, utilice grupos de seguridad de red y tablas de rutas con rutas definidas por el usuario. Las rutas definidas por el usuario se pueden colocar en la subred de integración para enviar el tráfico saliente según lo previsto.  

Considere implementar una instancia de Azure Firewall para crear, aplicar y registrar directivas de conectividad de red y aplicaciones de forma centralizada en las suscripciones y redes virtuales. Azure Firewall usa una dirección IP pública estática para los recursos de red virtual, lo que permite que los firewalls externos identifiquen el tráfico procedente de la red virtual. 

- [Bloqueo de una instancia de App Service Environment](environment/firewall-integration.md)

- [Protección de las 10 primeras vulnerabilidades del proyecto de seguridad de aplicaciones web (OWASP)](https://owasp.org/www-project-top-ten/)

- [Grupos de seguridad de red](../virtual-network/network-security-groups-overview.md)

- [Integración de su aplicación con una instancia de Azure Virtual Network](web-sites-integrate-with-vnet.md)

- [Consideraciones de red para un entorno de App Service Environment](environment/network-info.md)

- [Creación de una instancia externa de App Service Environment](environment/create-external-ase.md)

- [Creación de un ASE interno](environment/create-ilb-ase.md)

**Supervisión de Azure Security Center**: no disponible actualmente

**Responsabilidad**: Customer

### <a name="12-monitor-and-log-the-configuration-and-traffic-of-virtual-networks-subnets-and-network-interfaces"></a>1.2: Supervisión y registro de la configuración y el tráfico de redes virtuales, subredes e interfaces de red

**Guía**: Implemente recomendaciones de protección de la red de Azure Security Center para proteger las configuraciones y los recursos de red relacionados con sus aplicaciones y API de App Service.

Use Azure Firewall para enviar el tráfico y crear, aplicar y registrar directivas de conectividad de red y aplicaciones de forma centralizada en las suscripciones y redes virtuales. Azure Firewall usa una dirección IP pública estática para los recursos de red virtual, lo que permite que los firewalls externos identifiquen el tráfico procedente de la red virtual. El servicio Azure Firewall está también totalmente integrado con el registro y análisis de Azure Monitor.

- [Información general de Azure Firewall](../firewall/overview.md)

- [Descripción de la seguridad de red proporcionada por Azure Security Center](../security-center/security-center-network-recommendations.md)

- [Habilitación de la supervisión y la protección de App Service](/azure/security-center/defender-for-app-service-intro)

**Supervisión de Azure Security Center**: Sí

**Responsabilidad**: Customer

### <a name="13-protect-critical-web-applications"></a>1.3: Proteja las aplicaciones web críticas

**Guía**: Proteja una aplicación accesible desde Internet de una instancia de App Service Environment (ASE) de la manera siguiente:
- Implemente una instancia de Web Application Firewall (WAF) con Azure Application Gateway delante de una aplicación orientada a Internet.
- Use restricciones de acceso para proteger el tráfico entrante a Application Gateway.
- Proteja la aplicación con Azure Active Directory (Azure AD) para garantizar la autenticación.
- Establezca la versión mínima de TLS en 1.2.
- Establezca la aplicación en solo HTTPS.

Conduzca todo el tráfico saliente de la aplicación a través de un dispositivo de Azure Firewall y supervise los registros. 

Para proteger una aplicación accesible desde Internet en App Service multiinquilino (es decir, no en el nivel aislado), siga estos pasos:
- Implemente un dispositivo habilitado para Web Application Firewall delante de una aplicación.
- Use restricciones de acceso o puntos de conexión de servicio para proteger el tráfico entrante al dispositivo de Web Application Firewall (WAF).
- Proteja la aplicación con Azure AD para garantizar la autenticación.
- Establezca la versión mínima de TLS en 1.2.
- Establezca la aplicación en solo HTTPS.
- Use Integración de Virtual Network y el parámetro de aplicación WEBSITE_VIRTUAL NETWORK_ROUTE_ALL para que todo el tráfico saliente esté sujeto a los grupos de seguridad de red y las rutas definidas por el usuario de la subred de integración.

De forma similar a como hizo con la aplicación de Application Service Environment, conduzca todo el tráfico saliente de la aplicación a través de un dispositivo de Azure Firewall y supervise los registros de dicha aplicación.

Además, revise y siga las recomendaciones del documento Bloqueo de una instancia de App Service Environment.

- [Bloqueo de una instancia de App Service Environment](environment/firewall-integration.md)

- [Azure Web Application Firewall en Azure Application Gateway](../web-application-firewall/ag/ag-overview.md)

- [Restricciones de acceso de Azure App Service](app-service-ip-restrictions.md)

- [Seguimiento de alertas WAF y supervisión fácil de las tendencias con Azure Monitor ](../azure-monitor/overview.md)

**Supervisión de Azure Security Center**: Sí

**Responsabilidad**: Customer

### <a name="14-deny-communications-with-known-malicious-ip-addresses"></a>1.4: Denegación de las comunicaciones con direcciones IP malintencionadas conocidas

**Guía**: Proteja la instancia de App Service Environment como se describe en el documento Bloqueo de una instancia de App Service Environment. Use la funcionalidad integrada de inteligencia sobre amenazas de Azure Security Center para denegar las comunicaciones con direcciones IP públicas malintencionadas conocidas o no utilizadas. Use restricciones de acceso para proteger el tráfico entrante a Application Gateway. 

Proteja App Service multiinquilino (una aplicación que no está en un nivel aislado) con un punto de conexión público accesible desde Internet. Este punto de conexión permite el tráfico solo desde una subred específica dentro de la red virtual y bloquea el resto. Use restricciones de acceso para configurar listas de control de acceso de red (restricciones de IP) a fin de bloquear el tráfico entrante permitido.

Defina la prioridad en la lista ordenada de permitidos o denegados para administrar el acceso de red a la aplicación. La lista puede incluir direcciones IP o subredes de Virtual Network. Cuando contiene una o varias entradas, existe una regla implícita "denegar todo" al final de esta. Esta funcionalidad sirve para todas las cargas de trabajo hospedadas en App Service, lo que incluye Web Apps, API Apps, aplicaciones de Linux, aplicaciones de contenedor de Linux e instancias de Functions. 

Use puntos de conexión de servicio para restringir el acceso a la aplicación web desde una instancia de Azure Virtual Network. Limite el acceso a una instancia de App Service multiinquilino (una aplicación que no está en un nivel aislado) desde subredes seleccionadas con puntos de conexión de servicio. 

- [Restricciones de IP estáticas de Azure App Service](app-service-ip-restrictions.md)

- [Azure Web Application Firewall en Azure Application Gateway](../web-application-firewall/ag/ag-overview.md)

- [Configuración de un firewall de aplicaciones web (WAF) para App Service Environment](environment/app-service-app-service-environment-web-application-firewall.md)

- [Protección del ASE como se describe en Bloqueo de una instancia de App Service Environment](environment/firewall-integration.md)

**Supervisión de Azure Security Center**: Sí

**Responsabilidad**: Customer

### <a name="15-record-network-packets"></a>1.5: Registro de los paquetes de red

**Guía**: Supervise las solicitudes y respuestas que se envían a aplicaciones de App Service, y desde allí, con Security Center. Los ataques contra una aplicación web se pueden supervisar mediante una instancia de Application Gateway en tiempo real que tenga habilitado Web Application Firewall con el registro integrado de Azure Monitor para realizar un seguimiento de las alertas de este servicio de firewall y supervisar fácilmente las tendencias.

- [Firewall de aplicaciones web de Azure en Azure Application Gateway](../web-application-firewall/ag/ag-overview.md)

**Supervisión de Azure Security Center**: Sí

**Responsabilidad**: Customer

### <a name="17-manage-traffic-to-web-applications"></a>1.7: Administre el tráfico a las aplicaciones web

**Guía**: Administre el tráfico de una aplicación en una instancia de App Service Environment:

- Proteja la instancia de App Service Environment como se describe en Bloqueo de una instancia de App Service Environment.
- Implemente una instancia de Application Gateway que tenga una instancia de Azure Web Application Firewall delante de las aplicaciones accesibles desde Internet.
- Configure la aplicación para que solo sea accesible a través de HTTPS.

Administre el tráfico de una aplicación accesible desde Internet en App Service multiinquilino (que no está en el nivel aislado): 

- Implemente una instancia de Application Gateway que tenga habilitado Azure Web Application Firewall delante de las aplicaciones accesibles desde Internet.
- Use restricciones de acceso o puntos de conexión de servicio para proteger el tráfico que entra en Web Application Firewall. La funcionalidad de restricciones de acceso se aplica a todas las cargas de trabajo hospedadas en App Service, lo que incluye Web Apps, API Apps, aplicaciones de Linux, aplicaciones de contenedor de Linux e instancias de Functions.

- Configure la aplicación para que solo sea accesible a través de HTTPS.
- Limite el acceso a la aplicación de App Service con restricciones de IP estática para que solo se reciba tráfico de la dirección VIP en una puerta de enlace de aplicaciones como única dirección con acceso.

Para más información, revise los vínculos a los que se hace referencia.

- [Restricciones de IP estáticas de Azure App Service](app-service-ip-restrictions.md)

- [Azure Web Application Firewall en Azure Application Gateway](../web-application-firewall/ag/ag-overview.md)

- [Configuración de TLS de un extremo a otro con Application Gateway mediante el portal](../application-gateway/end-to-end-ssl-portal.md)

- [Proteja el ASE tal y como se describe en Bloqueo de una instancia de App Service](/azure/app-service/environment/firewall-integration)

**Supervisión de Azure Security Center**: Sí

**Responsabilidad**: Customer

### <a name="18-minimize-complexity-and-administrative-overhead-of-network-security-rules"></a>1.8: Minimice la complejidad y la sobrecarga administrativa de las reglas de seguridad de red

**Guía**: App Service tiene varios puntos de conexión que se usan para administrar el servicio. Estas direcciones de punto de conexión también se incluyen en la etiqueta de servicio IP AppServiceManagement. La etiqueta AppServiceManagement solo se usa con una instancia de App Service Environment para permitir este tráfico. 

Al especificar el nombre de la etiqueta de servicio en el campo de origen o destino apropiado de una regla, puede permitir o denegar el tráfico del servicio correspondiente. En la etiqueta de servicio IP de App Service se realiza un seguimiento de las direcciones de entrada de App Service. No hay ninguna etiqueta de servicio IP que contiene las direcciones de salida usadas por App Service.

Microsoft administra los prefijos de direcciones que la etiqueta de servicio incluye y actualiza automáticamente dicha etiqueta a medida que las direcciones cambian.

- [Etiquetas de servicio de red virtual](../virtual-network/service-tags-overview.md)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="19-maintain-standard-security-configurations-for-network-devices"></a>1.9: Mantenga las configuraciones de seguridad estándar para dispositivos de red

**Guía**: Defina e implemente configuraciones de seguridad estándar para la configuración de red relacionada con las aplicaciones de App Service. 

Mantenga las configuraciones de seguridad usando alias de Azure Policy en los espacios de nombres "Microsoft.Web" y "Microsoft.Network". Cree directivas personalizadas con el fin de auditar o aplicar la configuración de red de las aplicaciones de App Service. 

Use definiciones de directivas integradas en App Service, por ejemplo:
- La aplicación debe usar un punto de conexión de servicio de red virtual.
- La aplicación solo debe ser accesible a través de HTTPS.
- Establezca la versión de TLS mínima en la versión actual.

Para más información, revise los vínculos a los que se hace referencia.

- [Configuración y administración de Azure Policy](../governance/policy/tutorials/create-and-manage.md)

- [Configuración de TLS de un extremo a otro con Application Gateway mediante el portal](../application-gateway/end-to-end-ssl-portal.md)

- [Proteja el ASE tal y como se describe en Bloqueo de una instancia de App Service](/azure/app-service/environment/firewall-integration)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="110-document-traffic-configuration-rules"></a>1.10: Documente las reglas de configuración de tráfico

**Guía**: Use etiquetas para los grupos de seguridad de red y otros recursos relacionados, incluido el flujo de tráfico en App Service.

En el caso de grupos de seguridad de red individuales, utilice el campo "Descripción" para especificar la necesidad del negocio, la duración, etc. de cualquier regla que permita el tráfico hacia una red o desde allí.

Aplique cualquiera de las definiciones integradas de Azure Policy relacionadas con los efectos de etiquetado, como "Requerir etiqueta y su valor", para que todos los recursos se creen con etiquetas y para que se le notifiquen los recursos no etiquetados existentes. Use Azure PowerShell o la CLI de Azure para buscar o realizar acciones en los recursos en función de sus etiquetas.

- [Creación y uso de etiquetas](../azure-resource-manager/management/tag-resources.md)

- [Restricciones de acceso de Azure App Service](/azure/app-service/app-service-ip-restrictions)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="111-use-automated-tools-to-monitor-network-resource-configurations-and-detect-changes"></a>1.11: Use herramientas automatizadas para supervisar las configuraciones de recursos de red y detectar cambios

**Guía**: Use el registro de actividad de Azure para supervisar las configuraciones de los recursos de red y detectar cambios en la configuración de red y en los recursos relacionados con App Service. 

Aplique una de las diversas definiciones integradas de Azure Policy para App Service, como una directiva que audita el uso del servicio de punto de conexión de red virtual en las aplicaciones. Cree alertas en Azure Monitor que se desencadenen cuando se produzcan cambios en la configuraciones o recursos de red críticos. 

Revise las alertas y recomendaciones de seguridad detalladas en Security Center, en el portal o mediante herramientas de programación. Exporte esta información o envíela a otras herramientas de supervisión de su entorno. Existen herramientas para exportar alertas y recomendaciones de forma manual o continua. Con estas herramientas puede realizar lo siguiente:
 
- Realizar una exportación continua al área de trabajo de Log Analytics
- Exportación continua a Azure Event Hubs (para integraciones con SIEM de terceros)
- Exportar a un archivo CSV (una vez)

Se recomienda crear un proceso con herramientas automatizadas para supervisar las configuraciones de recursos de red y detectar rápidamente los cambios.

- [Visualización y recuperación de eventos del registro de actividad de Azure](../azure-monitor/platform/activity-log.md#view-the-activity-log)

- [Creación de alertas en Azure Monitor](../azure-monitor/platform/alerts-activity-log.md)

- [Exportar alertas y recomendaciones de seguridad (versión preliminar)](../security-center/continuous-export.md)

**Supervisión de Azure Security Center**: no disponible actualmente

**Responsabilidad**: Customer

## <a name="logging-and-monitoring"></a>Registro y supervisión

*Para obtener más información, consulte [Azure Security Benchmark: registro y supervisión](../security/benchmarks/security-control-logging-monitoring.md).*

### <a name="22-configure-central-security-log-management"></a>2.2: Configuración de la administración central de registros de seguridad

**Guía**: Integre la instancia de App Service Environment (ASE) con Azure Monitor para enviar registros a Azure Storage, Azure Event Hubs o Log Analytics. Habilite la configuración de diagnóstico del registro de actividad de Azure para el registro de auditoría del plano de control. Las alertas de seguridad de Security Center se publican en el registro de actividad de Azure. Audite los datos del registro de actividad de Azure para responder a las preguntas "qué, quién y cuándo" de las operaciones de escritura (PUT, POST, DELETE) llevadas a cabo en el nivel del plano de control para los recursos de Azure App Service y otros recursos de Azure. Guarde las consultas para usarlas en el futuro, ancle los resultados de la consulta a los paneles de Azure y cree alertas de registro. Además, use la API REST de acceso a datos de Application Insights para acceder a la telemetría mediante programación.

Use Microsoft Azure Sentinel, un sistema de administración de eventos de información de seguridad (SIEM) escalable y nativo de nube, disponible para conectarse a varios orígenes de datos y conectores, según sus requisitos empresariales. También puede habilitar un sistema de administración de eventos de información de seguridad (SIEM) de terceros, como Barracuda en Azure Marketplace, e incorporarle datos.

- [Registro de la actividad de ASE](environment/using-an-ase.md#logging)

- [Procedimiento para habilitar la configuración de diagnóstico en Azure App Service](troubleshoot-diagnostic-logs.md)

- [Habilitación de Application Insights](../azure-monitor/app/app-insights-overview.md)

- [Exportación de datos de telemetría desde Application Insights](../azure-monitor/app/export-telemetry.md)

**Supervisión de Azure Security Center**: Sí

**Responsabilidad**: Customer

### <a name="23-enable-audit-logging-for-azure-resources"></a>2.3: Habilitación del registro de auditoría para recursos de Azure

**Guía**: Habilite la configuración de diagnóstico del registro de actividad de Azure para el registro de auditoría del plano de control de App Service. Envíe los registros a un área de trabajo de Log Analytics, un centro de eventos de Azure o una cuenta de Azure Storage.
Las preguntas "qué, quién y cuándo" de las operaciones de escritura (PUT, POST, DELETE) llevadas a cabo en el nivel del plano de control se pueden determinar mediante los datos de registro de actividad de Azure de los recursos de App Service y de otros recursos de Azure.

Además, Azure Key Vault proporciona administración de secretos centralizada con directivas de acceso e historial de auditorías. 

- [Habilitación de la configuración de diagnóstico para el registro de actividad de Azure](../azure-monitor/platform/activity-log.md)

- [Procedimiento para habilitar la configuración de diagnóstico en Azure App Service](troubleshoot-diagnostic-logs.md)

- [Operaciones de Resource Manager](../role-based-access-control/resource-provider-operations.md)

**Supervisión de Azure Security Center**: Sí

**Responsabilidad**: Customer

### <a name="25-configure-security-log-storage-retention"></a>2.5: Configuración de la retención del almacenamiento de registros de seguridad

**Guía**: En Azure Monitor, establezca el período de retención de registros de las áreas de trabajo de Log Analytics asociadas a los recursos de App Service, según las normativas de cumplimiento de la organización.
- [Establecimiento de parámetros de retención de registros](../azure-monitor/platform/manage-cost-storage.md#change-the-data-retention-period)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="26-monitor-and-review-logs"></a>2.6: Supervisión y revisión de registros

**Guía**: Revise la configuración de diagnóstico del registro de actividad de Azure de los recursos de App Service con los registros que se envían a un área de trabajo de Log Analytics. Realice consultas en Log Analytics para buscar términos, identificar tendencias, analizar patrones y proporcionar otra información detallada basada en los datos recopilados.

Use Application Insights con las aplicaciones de App Service y para recopilar datos de registro, rendimiento y error. Vea los datos de telemetría recopilados por Application Insights en Azure Portal.

Si ha implementado una instancia de Web Application Firewall (WAF), puede supervisar los ataques contra las aplicaciones de App Service mediante un registro en tiempo real de dicho firewall. El registro se integra con Azure Monitor para hacer un seguimiento de las alertas de Web Application Firewall y supervisar con facilidad las tendencias.

Use Azure Sentinel, un sistema de administración de eventos de información de seguridad (SIEM) escalable y nativo de nube, para la integración con diversos orígenes de datos y conectores, según los requisitos. Opcionalmente, habilite e incorpore los datos a una solución de administración de eventos de información de seguridad de terceros de Azure Marketplace.

- [Habilitación de la configuración de diagnóstico para el registro de actividad de Azure](../azure-monitor/platform/activity-log.md)

- [Habilitación de Application Insights](../azure-monitor/app/app-insights-overview.md)

- [Integración de la instancia de App Service Environment con Azure Application Gateway](environment/integrate-with-application-gateway.md)

- [Incorporación de Azure Sentinel](../sentinel/quickstart-onboard.md)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="27-enable-alerts-for-anomalous-activities"></a>2.7: Habilitación de alertas para actividades anómalas

**Guía**: Configure Security Center en su suscripción de Azure y revise las alertas generadas. Use Azure Monitor para obtener los datos de registro de actividad en un centro de eventos, donde se pueden leer mediante una solución de administración de eventos de información de seguridad (SIEM), como Azure Sentinel. 

Supervise los ataques contra sus aplicaciones de App Service mediante un registro de Web Application Firewall en tiempo real con una instancia implementada de Azure Web Application Firewall (WAF). El registro se integra con Azure Monitor para hacer un seguimiento de las alertas de Web Application Firewall (WAF) y supervisar con facilidad las tendencias.

- [Integración de la instancia de App Service Environment con Azure Application Gateway](environment/integrate-with-application-gateway.md)

- [Exportar alertas y recomendaciones de seguridad (versión preliminar)](../security-center/continuous-export.md)

**Supervisión de Azure Security Center**: Sí

**Responsabilidad**: Customer

## <a name="identity-and-access-control"></a>Control de identidades y acceso

*Para obtener más información, consulte [Azure Security Benchmark: Identidad y control de acceso](../security/benchmarks/security-control-identity-access-control.md).*

### <a name="31-maintain-an-inventory-of-administrative-accounts"></a>3.1: Mantenga un inventario de cuentas administrativas

**Guía**: Azure Active Directory (Azure AD) tiene roles integrados que se deben asignar explícitamente y que se pueden consultar. Use el módulo de PowerShell de Azure AD para realizar consultas ad hoc para detectar cuentas que son miembros de grupos administrativos.

- [Obtención de los miembros de un rol de directorio en Azure AD con PowerShell](/powershell/module/azuread/get-azureaddirectoryrolemember?amp;preserve-view=true&view=azureadps-2.0)

- [Cómo usar identidades administradas para App Service y Azure Functions](overview-managed-identity.md?context=azure%2Factive-directory%2Fmanaged-identities-azure-resources%2Fcontext%2Fmsi-context&amp;tabs=dotnet)

- [Incorporación o eliminación de asignaciones de roles de Azure con Azure Portal](../role-based-access-control/role-assignments-portal.md)

**Supervisión de Azure Security Center**: Sí

**Responsabilidad**: Customer

### <a name="32-change-default-passwords-where-applicable"></a>3.2: Cambie las contraseñas predeterminadas cuando proceda

**Guía**: Azure Active Directory (Azure AD) no tiene el concepto de contraseñas predeterminadas. Proporciona acceso de plano de control a App Service.

Por lo general, evite implementar contraseñas predeterminadas para el acceso de los usuarios al compilar sus propias aplicaciones. Use uno de los proveedores de identidades disponibles de forma predeterminada en App Service, como Azure AD, cuenta de Microsoft, Facebook, Google o Twitter.

Deshabilite el acceso anónimo, a menos que sea necesario. 

- [Proveedores de identidades disponibles de forma predeterminada en Azure App Service](overview-authentication-authorization.md#identity-providers)

- [Autenticación y autorización en Azure App Service y Azure Functions](overview-authentication-authorization.md)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="33-use-dedicated-administrative-accounts"></a>3.3: Use cuentas administrativas dedicadas

**Guía**: Cree procedimientos operativos estándar en torno al uso de cuentas administrativas dedicadas. Use las características de Administración de identidades y acceso de Security Center para supervisar el número de cuentas administrativas. 

Use las recomendaciones de Security Center o las directivas integradas de Azure, como:

- Debe haber más de un propietario asignado a la suscripción. 
- Las cuentas en desuso con permisos de propietario deben quitarse de la suscripción
- Las cuentas externas con permisos de propietario deben quitarse de la suscripción

Cree un proceso para supervisar las configuraciones de los recursos de red y detectar los cambios en las cuentas administrativas.

- [Procedimiento para usar Azure Security Center para supervisar la identidad y el acceso](../security-center/security-center-identity-access.md)

- [Uso de Azure Policy](../governance/policy/tutorials/create-and-manage.md)

- [Más información sobre cómo conceder a los usuarios acceso a las aplicaciones](../role-based-access-control/overview.md)

**Supervisión de Azure Security Center**: Sí

**Responsabilidad**: Customer

### <a name="34-use-azure-active-directory-single-sign-on-sso"></a>3.4: Uso del inicio de sesión único (SSO) de Azure Active Directory

**Guía**: Autentique App Service mediante Azure Active Directory (Azure AD). Azure AD proporciona un servicio OAuth 2.0 para el proveedor de identidades y permite el acceso autorizado a aplicaciones web y móviles. 

Las aplicaciones de App Service usan la identidad federada, en la cual un proveedor de identidades de terceros administra automáticamente las identidades de usuario y el flujo de autenticación. De forma predeterminada, están disponibles estos proveedores de identidades:

- Azure AD
- Cuenta Microsoft

- Facebook

- Google

- Twitter

Cuando habilita la autenticación y autorización con uno de estos proveedores, su punto de conexión de inicio de sesión está disponible para la autenticación de los usuarios y para la validación de tokens de autenticación del proveedor.

- [Autenticación y autorización en Azure App Service](overview-authentication-authorization.md#identity-providers)

- [Más información sobre autenticación y autorización en Azure App Service](overview-authentication-authorization.md)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="35-use-multi-factor-authentication-for-all-azure-active-directory-based-access"></a>3.5: Uso de la autenticación multifactor para todo el acceso basado en Azure Active Directory

**Guía**: Habilite la característica Multi-Factor Authentication de Azure Active Directory (Azure AD) y siga las recomendaciones de administración de identidades y acceso de Security Center.

Implemente la autenticación multifactor para Azure AD. Los administradores deben asegurarse de que las cuentas de suscripción del portal están protegidas. La suscripción es vulnerable a los ataques porque administra los recursos creados. 

- [MFA de seguridad de Azure](/previous-versions/azure/security/develop/secure-aad-app)

- [Procedimiento para habilitar la MFA en Azure](../active-directory/authentication/howto-mfa-getstarted.md)

- [Supervisión de la identidad y el acceso en Azure Security Center](../security-center/security-center-identity-access.md)

**Supervisión de Azure Security Center**: Sí

**Responsabilidad**: Customer

### <a name="36-use-secure-azure-managed-workstations-for-administrative-tasks"></a>3.6: Uso de estaciones de trabajo seguras y administradas por Azure para realizar tareas administrativas

**Guía**: Utilice estaciones de trabajo de acceso con privilegios (PAW) que tengan configurada la autenticación multifactor para iniciar sesión en los recursos de Azure y configurarlos.

- [Más información sobre las estaciones de trabajo con privilegios de acceso](/windows-server/identity/securing-privileged-access/privileged-access-workstations)

- [Procedimiento para habilitar la MFA en Azure](../active-directory/authentication/howto-mfa-getstarted.md)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="37-log-and-alert-on-suspicious-activities-from-administrative-accounts"></a>3.7: Registro y alerta de actividades sospechosas desde cuentas administrativas

**Guía**: Utilice Privileged Identity Management (PIM) de Azure Active Directory (Azure AD) para la generación de registros y alertas cuando se produzca una actividad sospechosa o insegura en el entorno.

Además, use las detecciones de riesgo de Azure AD para ver alertas e informes sobre el comportamiento de los usuarios de riesgo.

La protección contra amenazas de Security Center proporciona defensas completas en su entorno, lo que incluye protección contra amenazas de los recursos de proceso de Azure, como máquinas Windows, máquinas Linux, App Service y contenedores de Azure.

- [Cómo implementar Privileged Identity Management (PIM)](../active-directory/privileged-identity-management/pim-deployment-plan.md)

- [Información sobre las detecciones de riesgo de Azure AD](../active-directory/identity-protection/overview-identity-protection.md)

- [Protección contra amenazas de los recursos de proceso de Azure](../security-center/azure-defender.md)

**Supervisión de Azure Security Center**: Sí

**Responsabilidad**: Customer

### <a name="38-manage-azure-resources-from-only-approved-locations"></a>3.8: Administre los recursos de Azure solo desde ubicaciones aprobadas

**Guía**: Use ubicaciones con nombre de acceso condicional para permitir el acceso a Azure Portal solo desde agrupaciones lógicas específicas de intervalos de direcciones IP o países o regiones.

- [Configuración de ubicaciones con nombre en Azure](../active-directory/reports-monitoring/quickstart-configure-named-locations.md)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="39-use-azure-active-directory"></a>3.9: Uso de Azure Active Directory

**Guía**: Use Azure Active Directory (Azure AD) como sistema central de autenticación y autorización para sus aplicaciones de App Service. Azure AD protege los datos mediante el cifrado seguro para datos en reposo y en tránsito, y también sales, hashes y almacena de forma segura las credenciales de usuario.

- [Configuración de las aplicaciones de Azure App Service para usar el inicio de sesión de Azure AD](configure-authentication-provider-aad.md)

- [Procedimiento para crear y configurar una instancia de Azure AD](../active-directory/fundamentals/active-directory-access-create-new-tenant.md)

**Supervisión de Azure Security Center**: Sí

**Responsabilidad**: Customer

### <a name="310-regularly-review-and-reconcile-user-access"></a>3.10: Revise y concilie regularmente el acceso de los usuarios

**Guía**: Detecte cuentas obsoletas con los registros proporcionados por Azure Active Directory (Azure AD). Use revisiones de acceso de identidad de Azure para administrar de forma eficiente las pertenencias a grupos, el acceso a las aplicaciones empresariales y las asignaciones de roles. Revise el acceso de los usuarios periódicamente para asegurarse de que solo los usuarios previstos tengan acceso continuo. 

- [Descripción de los informes de Azure AD](../active-directory/reports-monitoring/index.yml)

- [Procedimiento para usar las revisiones de acceso de identidad de Azure](../active-directory/governance/access-reviews-overview.md)

**Supervisión de Azure Security Center**: Sí

**Responsabilidad**: Customer

### <a name="311-monitor-attempts-to-access-deactivated-credentials"></a>3.11: Supervisión de los intentos de acceso a credenciales desactivadas

**Guía**: Use Azure Active Directory (Azure AD) como sistema central de autenticación y autorización para sus aplicaciones de App Service. Azure AD protege los datos mediante su cifrado seguro en reposo y en tránsito, y también cifra con sal las credenciales de usuario, les aplica hash y las almacena de forma segura.

El acceso a los orígenes del registro de eventos de riesgo, auditoría y actividad de inicio de sesión de Azure AD le permite la integración con Azure Sentinel o con una solución de administración de eventos e información de seguridad (SIEM) de terceros. Simplifique el proceso mediante la creación de una configuración de diagnóstico para las cuentas de usuario de Azure AD y el envío de los registros de auditoría e inicio de sesión a un área de trabajo de Log Analytics. Se pueden configurar las alertas de registro deseadas en Log Analytics.

- [Configuración de las aplicaciones de Azure App Service para usar el inicio de sesión de Azure AD](configure-authentication-provider-aad.md)

- [Integración de los registros de actividad de Azure en Azure Monitor](../active-directory/reports-monitoring/howto-integrate-activity-logs-with-log-analytics.md)

- [Incorporación de Azure Sentinel](../sentinel/quickstart-onboard.md)

**Supervisión de Azure Security Center**: no disponible actualmente

**Responsabilidad**: Customer

### <a name="312-alert-on-account-sign-in-behavior-deviation"></a>3.12: Alerta de las desviaciones de comportamiento en los inicios de sesión de las cuentas

**Guía**: Use Azure Active Directory (Azure AD) como sistema central de autenticación y autorización para sus aplicaciones de App Service. 

Use Azure AD Identity Protection para configurar respuestas automatizadas a acciones sospechosas detectadas relacionadas con las identidades de los usuarios, como la desviación del comportamiento de inicio de sesión de la cuenta en el plano de control con Azure Portal. También puede hacer que Azure Sentinel ingiera los datos para investigarlos más. 

- [Configuración de una aplicación de App Service para usar el inicio de sesión de Azure AD](configure-authentication-provider-aad.md)

- [Visualización de los inicios de sesión de riesgo de Azure AD](../active-directory/identity-protection/overview-identity-protection.md)

- [Configuración y habilitación de las directivas de riesgo de protección de identidad](../active-directory/identity-protection/howto-identity-protection-configure-risk-policies.md)

**Supervisión de Azure Security Center**: no disponible actualmente

**Responsabilidad**: Customer

### <a name="313-provide-microsoft-with-access-to-relevant-customer-data-during-support-scenarios"></a>3.13: Proporcione a Microsoft acceso a los datos pertinentes del cliente durante los escenarios de soporte técnico

**Guía**: No disponible para App Service. La Caja de seguridad del cliente no se admite en Azure App Service.

- [Lista de servicios admitidos por la Caja de seguridad del cliente](../security/fundamentals/customer-lockbox-overview.md#supported-services-and-scenarios-in-general-availability)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

## <a name="data-protection"></a>Protección de los datos

*Para obtener más información, consulte [Azure Security Benchmark: Protección de datos](../security/benchmarks/security-control-data-protection.md).*

### <a name="41-maintain-an-inventory-of-sensitive-information"></a>4.1: Mantenimiento de un inventario de información confidencial

**Guía**: Use etiquetas para ayudar a realizar el seguimiento de los recursos de Azure que almacenan o procesan información confidencial.

- [Creación y uso de etiquetas](../azure-resource-manager/management/tag-resources.md)

**Supervisión de Azure Security Center**: no disponible actualmente

**Responsabilidad**: Customer

### <a name="42-isolate-systems-storing-or-processing-sensitive-information"></a>4.2: Aislamiento de los sistemas que almacenan o procesan información confidencial

**Guía**: En una instancia de App Service Environment, implemente suscripciones o grupos de administración distintos, para entornos de desarrollo, pruebas y producción. Del mismo modo, las aplicaciones que procesan información confidencial se pueden aislar de otras aplicaciones. Implemente la aplicación de App Service en una red virtual. Use grupos de seguridad de red y subredes para un mejor aislamiento de la aplicación. 

En una instancia de App Service Environment (ASE), hay dos tipos de implementación. Ambos permiten aislar el tráfico en función de sus necesidades empresariales.

- App Service Environment externo: expone las aplicaciones hospedadas en App Service Environment en una dirección IP accesible desde Internet.

-  App Service Environment del equilibrador de carga interno (ILB): expone las aplicaciones hospedadas en App Service Environment en una dirección IP dentro de la red virtual. El punto de conexión interno es un equilibrador de carga interno (ILB), y esta es la razón por la que se denomina ASE de ILB. 

En el caso de App Service multiinquilino (una aplicación que no está en el nivel aislado), use Integración de Virtual Network para el acceso de la aplicación a los recursos de la red virtual.  Use el acceso privado a sitios para que una aplicación sea accesible solo desde una red privada; por ejemplo, desde dentro de una red virtual de Azure. Integración de Virtual Network solo se usa para realizar llamadas salientes de la aplicación a la red virtual. La característica Integración de Virtual Network se comporta de forma distinta con redes virtuales de la misma región que con redes virtuales de otras regiones. 
 
- [Consideraciones de red para un entorno de App Service Environment](environment/network-info.md)

- [Creación de una instancia externa de App Service Environment](environment/create-external-ase.md)

- [Creación de un ASE interno](environment/create-ilb-ase.md)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="43-monitor-and-block-unauthorized-transfer-of-sensitive-information"></a>4.3: Supervisión y bloqueo de una transferencia no autorizada de información confidencial

**Guía**: Aunque las características de identificación, clasificación y prevención de pérdida de datos todavía no están disponibles para App Service, puede reducir el riesgo de filtración de datos de la red virtual mediante la eliminación de todas las reglas en las que el destino usa una "etiqueta" para los servicios de Internet o de Azure. 

Microsoft administra la infraestructura subyacente de App Service y ha implementado controles estrictos para evitar la pérdida o exposición de sus datos.

- [Descripción de la protección de datos de los clientes en Azure](../security/fundamentals/protection-customer-data.md)

**Supervisión de Azure Security Center**: no disponible actualmente

**Responsabilidad**: Compartido

### <a name="44-encrypt-all-sensitive-information-in-transit"></a>4.4: Cifrado de toda la información confidencial en tránsito

**Guía**: Use la versión mínima predeterminada de TLS 1.2, configurada en las opciones de TLS/SSL, para cifrar toda la información en tránsito. Asegúrese también de que todas las solicitudes de conexión HTTP se redirigen a HTTPS.

- [Cifrado en tránsito para aplicaciones web de Azure App Service](security-recommendations.md)

**Supervisión de Azure Security Center**: Sí

**Responsabilidad**: Customer

### <a name="45-use-an-active-discovery-tool-to-identify-sensitive-data"></a>4.5: Uso de una herramienta de detección activa para identificar datos confidenciales

**Guía**: No disponible actualmente. Las características de identificación, clasificación y prevención de pérdida de datos todavía no están disponibles para App Service. 

Etiquete las aplicaciones de App Service que puedan estar procesando información confidencial. Si es necesario con fines de cumplimiento, implemente una solución de terceros.

Microsoft administra la plataforma subyacente, trata todos los datos de los clientes como confidenciales y hace grandes esfuerzos para proteger a los clientes contra la pérdida y exposición de sus datos. Para garantizar la seguridad de los datos de los clientes dentro de Azure, Microsoft ha implementado y mantiene un conjunto de controles y funcionalidades eficaces de protección de datos.

- [Descripción de la protección de datos de los clientes en Azure](../security/fundamentals/protection-customer-data.md)

**Supervisión de Azure Security Center**: no disponible actualmente

**Responsabilidad**: Compartido

### <a name="46-use-role-based-access-control-to-control-access-to-resources"></a>4.6: Uso del control de acceso basado en rol para controlar el acceso a los recursos

**Guía**: Use el control de acceso basado en rol (RBAC) de Azure Active Directory (Azure AD) para controlar el acceso al plano de control de App Service en Azure Portal.

- [Configuración de RBAC en Azure](../role-based-access-control/role-assignments-portal.md)

**Supervisión de Azure Security Center**: no disponible actualmente

**Responsabilidad**: Customer

### <a name="48-encrypt-sensitive-information-at-rest"></a>4.8: Cifrado de información confidencial en reposo

**Guía**: El contenido de un sitio web en una aplicación de App Service, como es el caso de los archivos, se almacena en Azure Storage, que cifra automáticamente el contenido en reposo. Elija almacenar los secretos de aplicación en Key Vault y recuperarlos en tiempo de ejecución.

Los secretos suministrados por el cliente se cifran en reposo mientras se almacenan en las bases de datos de configuración de App Service.

Tenga en cuenta que, aunque los sitios web pueden usar opcionalmente los discos conectados localmente como almacenamiento temporal (por ejemplo, D:\local y% TMP%), no se cifran en reposo.

- [Controles de protección de datos para Azure App Service]()

- [Cifrado en reposo de Azure Storage](../storage/common/storage-service-encryption.md)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="49-log-and-alert-on-changes-to-critical-azure-resources"></a>4.9: Registro y alerta de cambios en los recursos críticos de Azure

**Guía**: Use Azure Monitor con el registro de actividad de Azure para crear alertas cuando se produzcan cambios en las aplicaciones de producción de App Service y otros recursos críticos o relacionados.

- [Creación de alertas para los eventos del registro de actividad de Azure](../azure-monitor/platform/alerts-activity-log.md)

**Supervisión de Azure Security Center**: no disponible actualmente

**Responsabilidad**: Customer

## <a name="vulnerability-management"></a>Administración de vulnerabilidades

*Para obtener más información, consulte [Azure Security Benchmark: Administración de vulnerabilidades](../security/benchmarks/security-control-vulnerability-management.md).*

### <a name="51-run-automated-vulnerability-scanning-tools"></a>5.1: Ejecute herramientas de análisis de vulnerabilidades automatizado

**Guía**: Adopte una práctica de DevSecOps para comprobar que las aplicaciones de App Service sean seguras y permanezcan protegidas a lo largo de su ciclo de vida. DevSecOps involucra al equipo de seguridad de la organización y sus funcionalidades en las prácticas de DevOps, lo que convierte la seguridad en una responsabilidad de todos los usuarios del equipo.

Revise y siga las recomendaciones de Security Center para proteger las aplicaciones de App Service.

- [Incorporación de un mecanismo continuo para validar la seguridad en la canalización de CI/CD](/azure/devops/migrate/security-validation-cicd-pipeline?amp;preserve-view=true&view=azure-devops)

- [Implementación de las recomendaciones de evaluación de vulnerabilidades de Azure Security Center](../security-center/deploy-vulnerability-assessment-vm.md)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="55-use-a-risk-rating-process-to-prioritize-the-remediation-of-discovered-vulnerabilities"></a>5.5: Use un proceso de clasificación de riesgos para priorizar la corrección de las vulnerabilidades detectadas

**Guía**: Microsoft realiza la administración de vulnerabilidades en los sistemas subyacentes que admiten App Service. Sin embargo, puede usar la gravedad de las recomendaciones de Security Center, así como la puntuación segura, para medir el riesgo dentro de su entorno. La puntuación segura se basa en el número de recomendaciones de Security Center que ha mitigado. Para dar prioridad a las recomendaciones que deben resolverse en primer lugar, tenga en cuenta la gravedad de cada una.

- [Guía de referencia sobre las recomendaciones de seguridad](../security-center/recommendations-reference.md)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Compartido

## <a name="inventory-and-asset-management"></a>Inventario y administración de recursos

*Para obtener más información, consulte [Azure Security Benchmark: Administración de recursos y del inventario](../security/benchmarks/security-control-inventory-asset-management.md).*

### <a name="61-use-automated-asset-discovery-solution"></a>6.1: Uso de la solución de detección de recursos automatizada

**Guía**: Utilice Azure Resource Graph para consultar o detectar todos los recursos (por ejemplo, proceso, almacenamiento, red, puertos, protocolos, etc.) dentro de las suscripciones. Asegúrese de que se apliquen los permisos adecuados al inquilino y de que pueda enumerar todas las suscripciones de Azure, así como los recursos que contienen.

Aunque los recursos clásicos de Azure se pueden detectar a través de Resource Graph, se recomienda encarecidamente crear y usar los recursos de Azure Resource Manager que figuran a continuación.

- [Creación de consultas con Azure Resource Graph](../governance/resource-graph/first-query-portal.md)

- [Visualización de las suscripciones de Azure](/powershell/module/az.accounts/get-azsubscription?amp;preserve-view=true&view=azps-4.8.0)

- [Descripción de Azure RBAC](../role-based-access-control/overview.md)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="62-maintain-asset-metadata"></a>6.2: Mantenimiento de metadatos de recursos

**Guía**: Aplique etiquetas a los recursos de Azure mediante metadatos para organizarlos de forma lógica en una taxonomía.

- [Creación y uso de etiquetas](../azure-resource-manager/management/tag-resources.md)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="63-delete-unauthorized-azure-resources"></a>6.3: Eliminación de recursos de Azure no autorizados

**Guía**: Use el etiquetado, los grupos de administración y distintas suscripciones, si procede, para organizar los recursos de Azure y realizar su seguimiento. Concilie el inventario periódicamente y asegúrese de que, como parte de este proceso, los recursos no autorizados se eliminen de la suscripción de manera oportuna.

Elija Azure Policy para aplicar restricciones sobre el tipo de recursos que se pueden crear en las suscripciones del cliente con las siguientes definiciones de directivas integradas:

- Tipos de recursos no permitidos
- Tipos de recursos permitidos

Para más información, revise los vínculos a los que se hace referencia.

- [Creación de suscripciones adicionales de Azure](../cost-management-billing/manage/create-subscription.md)

- [Creación de grupos de administración](../governance/management-groups/create-management-group-portal.md)

- [Creación y uso de etiquetas](../azure-resource-manager/management/tag-resources.md)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="64-define-and-maintain-inventory-of-approved-azure-resources"></a>6.4: Definición y mantenimiento de un inventario de los recursos de Azure aprobados

**Guía**: Cree un inventario de los recursos de Azure aprobados y del software aprobado relativos a los recursos de proceso en función de las necesidades de la organización.

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="65-monitor-for-unapproved-azure-resources"></a>6.5: Supervisión de recursos de Azure no aprobados

**Guía**: use Azure Policy para establecer restricciones sobre el tipo de recursos que se pueden crear en las suscripciones.

Use Azure Resource Graph para consultar o detectar recursos dentro de sus suscripciones.  Asegúrese de que todos los recursos de Azure presentes en el entorno estén aprobados. 

- [Configuración y administración de Azure Policy](../governance/policy/tutorials/create-and-manage.md) 

- [Creación de consultas con Azure Graph](../governance/resource-graph/first-query-portal.md)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="66-monitor-for-unapproved-software-applications-within-compute-resources"></a>6.6: Supervisión de aplicaciones de software no aprobadas en recursos de proceso

**Guía**: Use Azure Resource Graph para consultar o detectar recursos dentro de las suscripciones y asegurarse de que los recursos de Azure detectados se aprueban en función de las directivas de la organización.

Use WebJobs en App Service para supervisar las aplicaciones de software no aprobadas que se implementan en los recursos de proceso. Use WebJobs para ejecutar un programa o script en la misma instancia que una aplicación web, una aplicación de API o una aplicación móvil. Defina configuraciones y la supervisión de WebJobs con registros. En la página Detalles de ejecución del WebJob, seleccione Alternar salida para ver el texto del contenido del registro. Tenga en cuenta que WebJobs no es compatible aún con App Service en Linux.

- [Ejecución de tareas en segundo plano con WebJobs en Azure App Service](webjobs-create.md)

- [Tutorial: Creación y administración de directivas para aplicar el cumplimiento](../governance/policy/tutorials/create-and-manage.md)

- [Inicio rápido: Ejecución de la primera consulta de Resource Graph mediante Azure Resource Graph Explorer](../governance/resource-graph/first-query-portal.md)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="67-remove-unapproved-azure-resources-and-software-applications"></a>6.7: Eliminación de aplicaciones de software y recursos de Azure no aprobadas

**Guía**: Asegúrese de que todos los recursos de Azure presentes en el entorno estén aprobados. use Azure Policy para establecer restricciones sobre el tipo de recursos que se pueden crear en las suscripciones. Quite todas las aplicaciones de software implementadas que no se hayan aprobado según las directivas de la organización.

- [Ejecución de tareas en segundo plano con WebJobs en Azure App Service](webjobs-create.md)

- [Tutorial: Creación y administración de directivas para aplicar el cumplimiento](../governance/policy/tutorials/create-and-manage.md)

- [Inicio rápido: Ejecución de la primera consulta de Resource Graph mediante Azure Resource Graph Explorer](../governance/resource-graph/first-query-portal.md)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="68-use-only-approved-applications"></a>6.8: Uso exclusivo de aplicaciones aprobadas

**Guía**: Asegúrese de que todos los recursos de Azure presentes en el entorno estén aprobados. use Azure Policy para establecer restricciones sobre el tipo de recursos que se pueden crear en las suscripciones. Quite todas las aplicaciones de software implementadas que no se hayan aprobado según las directivas de la organización. 

- [Ejecución de tareas en segundo plano con WebJobs en Azure App Service](webjobs-create.md)

- [Tutorial: Creación y administración de directivas para aplicar el cumplimiento](../governance/policy/tutorials/create-and-manage.md)

- [Inicio rápido: Ejecución de la primera consulta de Resource Graph mediante Azure Resource Graph Explorer](../governance/resource-graph/first-query-portal.md)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="69-use-only-approved-azure-services"></a>6.9: Uso exclusivo de servicios de Azure aprobados

**Guía**: Cree un proceso para revisar los servicios de Azure no autorizados de forma periódica para que solo se usen servicios de Azure autorizados en las suscripciones.

Use Azure Resource Graph, dentro de este proceso, para consultar o detectar recursos dentro de sus suscripciones. Asegúrese de que todos los recursos de Azure presentes en el entorno estén aprobados.

Configure Azure Policy para establecer restricciones sobre el tipo de recursos que se pueden crear en las suscripciones con las siguientes definiciones de directivas integradas:

- Tipos de recursos no permitidos

- Tipos de recursos permitidos

Use WebJobs en App Service para supervisar las aplicaciones de software no aprobadas implementadas en los recursos de proceso. Use WebJobs para ejecutar un programa o script en la misma instancia que una aplicación web, una aplicación de API o una aplicación móvil. Defina configuraciones y la supervisión de WebJobs con registros. En la página Detalles de ejecución del WebJob, seleccione Alternar salida para ver el texto del contenido del registro. Tenga en cuenta que WebJobs no es compatible aún con App Service en Linux.

- [Configuración y administración de Azure Policy](../governance/policy/tutorials/create-and-manage.md)

- [Denegación de un tipo de recurso específico con Azure Policy](../governance/policy/samples/built-in-policies.md#general)

- [Ejecución de tareas en segundo plano con WebJobs en Azure App Service](webjobs-create.md)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="610-maintain-an-inventory-of-approved-software-titles"></a>6.10: Mantenimiento de un inventario de títulos de software aprobados

**Guía**: Implemente un proceso para inventariar y revisar los títulos de software de sus suscripciones de forma periódica para que solo se usan servicios de Azure autorizados en ellas.

Use Azure Resource Graph, dentro de este proceso, para consultar o detectar recursos dentro de sus suscripciones. Asegúrese de que todos los recursos de Azure detectados en el entorno estén aprobados.

Configure Azure Policy para aplicar restricciones sobre el tipo de recursos que se pueden crear en las suscripciones del cliente con las siguientes definiciones de directivas integradas:

- Tipos de recursos no permitidos

- Tipos de recursos permitidos

Del mismo modo, use WebJobs en App Service para inventariar las aplicaciones de software no aprobadas implementadas en los recursos del equipo. Defina su configuración y la supervisión con los registros. En la página Detalles de ejecución del WebJob, seleccione Alternar salida para ver el texto del contenido del registro. Tenga en cuenta que WebJobs no es compatible aún con App Service en Linux.

- [Configuración y administración de Azure Policy](../governance/policy/tutorials/create-and-manage.md)

- [Denegación de un tipo de recurso específico con Azure Policy](../governance/policy/samples/built-in-policies.md#general)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="611-limit-users-ability-to-interact-with-azure-resource-manager"></a>6.11: Limitación de la capacidad de los usuarios para interactuar con Azure Resource Manager

**Guía**: Configure el acceso condicional de Azure para limitar la capacidad de los usuarios de interactuar con Azure Resource Manager configurando "Bloquear acceso" en la aplicación "Microsoft Azure Management".

- [Configuración del acceso condicional para bloquear el acceso a Azure Resource Manager](../role-based-access-control/conditional-access-azure-management.md)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="612-limit-users-ability-to-execute-scripts-within-compute-resources"></a>6.12: Limitación de capacidad de los usuarios para ejecutar scripts en recursos de proceso

**Guía**: WebJobs en App Service permite a los clientes ejecutar un programa o script en la misma instancia que una aplicación web, una aplicación de API o una aplicación móvil. Es responsabilidad suya definir la configuración para restringir o limitar los scripts que no están permitidos por la organización. App Service no proporciona un mecanismo para limitar la ejecución de scripts de forma nativa. Tenga en cuenta que WebJobs no es compatible aún con App Service en Linux.

- [Ejecución de tareas en segundo plano con WebJobs en Azure App Service](webjobs-create.md)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="613-physically-or-logically-segregate-high-risk-applications"></a>6.13: Segregación física o lógica de aplicaciones de alto riesgo

**Guía**: Implemente suscripciones o grupos de administración distintos para proporcionar aislamiento a las aplicaciones de App Service de alto riesgo. Implemente una aplicación de mayor riesgo en su propia instancia de Virtual Network, ya que la seguridad perimetral en App Service se logra mediante el uso de redes virtuales. App Service Environment es una implementación de App Service en una subred de la instancia de Azure Virtual Network. 

Hay dos tipos de instancias de Application Service Environment: Application Service Environment externo y Application Service Environment de ILB (equilibrador de carga interno). Elija la arquitectura más adecuada según sus requisitos.

- [Consideraciones de red para un entorno de App Service Environment](environment/network-info.md)

- [Creación de una instancia externa de App Service Environment](environment/create-external-ase.md)

- [Creación y uso de un entorno de una instancia de Azure App Service Environment de Load Balancer](environment/create-ilb-ase.md)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

## <a name="secure-configuration"></a>Configuración segura

*Para obtener más información, consulte [Azure Security Benchmark: Configuración segura](../security/benchmarks/security-control-secure-configuration.md).*

### <a name="71-establish-secure-configurations-for-all-azure-resources"></a>7.1: Establezca configuraciones seguras para todos los recursos de Azure

**Guía**: Defina e implemente configuraciones de seguridad estándar para las aplicaciones de App Service implementadas con Azure Policy.

Utilice los alias de Azure Policy en el espacio de nombres "Microsoft.Web" para crear directivas personalizadas que permitan auditar o aplicar la configuración de su instancia de App Service Web Apps.

Aplique definiciones de directivas integradas, por ejemplo:
- App Service debe usar un punto de conexión del servicio de red virtual
- Las aplicaciones web solo deben ser accesibles a través de HTTPS.

- Uso de la versión más reciente de TLS en las aplicaciones

Se recomienda documentar el proceso para aplicar las definiciones de directivas integradas para su uso estándar.   

- [Visualización de los alias de Azure Policy disponibles](/powershell/module/az.resources/get-azpolicyalias?amp;preserve-view=true&view=azps-4.8.0)

- [Configuración y administración de Azure Policy](../governance/policy/tutorials/create-and-manage.md)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="73-maintain-secure-azure-resource-configurations"></a>7.3: Mantenga configuraciones de recursos de Azure seguras

**Guía**: Use los efectos [deny] y [deploy if not exist] de Azure Policy para aplicar una configuración segura en las aplicaciones de Azure App Service.

- [Configuración y administración de Azure Policy](../governance/policy/tutorials/create-and-manage.md)

- [Descripción de los efectos de Azure Policy](../governance/policy/concepts/effects.md)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="75-securely-store-configuration-of-azure-resources"></a>7.5: Almacene de forma segura la configuración de los recursos de Azure

**Guía**: Elija Azure DevOps o Azure Repos para almacenar y administrar el código de forma segura al usar definiciones de Azure Policy personalizadas.

Use su canalización de integración continua (CI) y entrega continua (CD) existente para implementar una configuración segura conocida.

- [Almacenamiento de código en Azure DevOps](/azure/devops/repos/git/gitworkflow?amp;preserve-view=true&view=azure-devops)

- [Documentación de Azure Repos](/azure/devops/repos/?amp;preserve-view=true&view=azure-devops)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="77-deploy-configuration-management-tools-for-azure-resources"></a>7.7: Implementación de herramientas de administración de configuración para recursos de Azure

**Guía**: Use las definiciones integradas en Azure Policy junto con los alias de esta misma aplicación en el espacio de nombres "Microsoft.Web" para crear directivas personalizadas que permitan auditar y aplicar las configuraciones, así como enviar alertas sobre ellas. Desarrolle un proceso y una canalización para administrar las excepciones de las directivas.

- [Configuración y administración de Azure Policy](../governance/policy/tutorials/create-and-manage.md)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="79-implement-automated-configuration-monitoring-for-azure-resources"></a>7.9: Implementación de la supervisión de la configuración automatizada para los recursos de Azure

**Guía**: Use las definiciones integradas en Azure Policy junto con los alias de esta misma aplicación en el espacio de nombres "Microsoft.Web" para crear directivas personalizadas que permitan auditar y aplicar las configuraciones, así como enviar alertas sobre ellas. 

Use los efectos de Azure Policy [audit], [deny] y [deploy if not exist] para aplicar automáticamente las configuraciones a los recursos de Azure.

- [Configuración y administración de Azure Policy](../governance/policy/tutorials/create-and-manage.md)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="711-manage-azure-secrets-securely"></a>7.11: Administre los secretos de Azure de forma segura

**Guía**: Use identidades administradas para proporcionar a las aplicaciones de App Service una identidad administrada automáticamente en Azure Active Directory (Azure AD). Las identidades administradas permiten que las aplicaciones se autentiquen en cualquier servicio que admita la autenticación de Azure AD, como Key Vault, sin necesidad de credenciales en el código. Asegúrese de que la eliminación temporal está habilitada en Azure Key Vault.

- [Habilitación de la eliminación temporal en Azure Key Vault](../key-vault/general/key-vault-recovery.md)

- [Uso de identidades administradas para App Service](overview-managed-identity.md)

- [Cómo proporcionar la autenticación de Key Vault con una identidad administrada](../key-vault/general/assign-access-policy-portal.md)

**Supervisión de Azure Security Center**: Sí

**Responsabilidad**: Customer

### <a name="712-manage-identities-securely-and-automatically"></a>7.12: Administre las identidades de forma segura y automática

**Guía**: Use identidades administradas para proporcionar a las aplicaciones implementadas de App Service una identidad administrada automáticamente en Azure Active Directory (Azure AD). Las identidades administradas le permiten autenticarse en cualquier servicio que admita la autenticación de Azure AD, como Key Vault, sin necesidad de credenciales en el código.

- [Uso de identidades administradas para App Service](overview-managed-identity.md)

- [Cómo proporcionar la autenticación de Key Vault con una identidad administrada](../key-vault/general/assign-access-policy-portal.md)

**Supervisión de Azure Security Center**: Sí

**Responsabilidad**: Customer

### <a name="713-eliminate-unintended-credential-exposure"></a>7.13: Elimine la exposición de credenciales no intencionada

**Instrucciones**: Implemente el escáner de credenciales para identificar las credenciales en el código. El escáner de credenciales también fomenta el traslado de credenciales detectadas a ubicaciones más seguras, como Azure Key Vault.

- [Configuración del escáner de credenciales](https://secdevtools.azurewebsites.net/helpcredscan.html)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

## <a name="data-recovery"></a>Recuperación de datos

*Para más información, consulte [Azure Security Benchmark: recuperación de datos](../security/benchmarks/security-control-data-recovery.md).*

### <a name="91-ensure-regular-automated-back-ups"></a>9.1: Garantía de copias de seguridad automáticas periódicas

**Guía**: La característica Copia de seguridad y restauración de App Service le permite crear fácilmente copias de seguridad de la aplicación manualmente o según una programación. Puede configurar las copias de seguridad de modo que se conserven durante un período de tiempo indefinido. Puede restaurar la aplicación a una instantánea de un estado anterior sobrescribiendo la aplicación existente o restaurando en otra aplicación.

App Service puede hacer una copia de seguridad de la siguiente información en una cuenta de almacenamiento y un contenedor de Azure, que se han configurado para que los utilice la aplicación:
- Configuración de la aplicación
- Contenido del archivo
- Base de datos conectada a la aplicación

Asegúrese de tiene lugar copias de seguridad normales y automatizadas con una frecuencia definida en las directivas de la organización.

- [Funcionalidad de copia de seguridad de Azure App Service](manage-backup.md)

- [Claves administradas por el cliente para el cifrado de Azure Storage](../storage/common/customer-managed-keys-overview.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="92-perform-complete-system-backups-and-backup-any-customer-managed-keys"></a>9.2: Realización de copias de seguridad completas del sistema y copias de seguridad de las claves administradas por el cliente

**Guía**: Use la característica de copia de seguridad y restauración de App Service para realizar copias de seguridad de las aplicaciones. Las características de copia de seguridad requieren una cuenta de Azure Storage para almacenar la información de copia de seguridad de la aplicación.

- Azure Storage proporciona cifrado en reposo; use las claves proporcionadas por el sistema o sus propias claves administradas por el cliente. Aquí es donde se almacenan los datos de la aplicación cuando no se ejecutan en una aplicación web en Azure.
- La ejecución desde un paquete de implementación es una característica de implementación de App Service. Permite implementar el contenido del sitio desde una cuenta de Azure Storage mediante una dirección URL de Firma de acceso compartido (SAS).

- Las referencias Key Vault son una característica de seguridad de App Service. Permite importar secretos en tiempo de ejecución como opciones de configuración de la aplicación. Úsela para cifrar la dirección URL de SAS de su cuenta de Azure Storage.

Hay más información disponible en los vínculos mencionados.

- [Realizar una copia de seguridad de la aplicación en Azure](manage-backup.md)

- [Restauración de una aplicación que se ejecuta en Azure App Service](web-sites-restore.md)

- [Descripción del cifrado en reposo en Azure](../security/fundamentals/encryption-atrest.md#encryption-at-rest-in-microsoft-cloud-services) 

- [Tabla de modelo de cifrado y administración de claves](../security/fundamentals/encryption-atrest.md)

- [Cifrado en reposo con claves administradas por el cliente](configure-encrypt-at-rest-using-cmk.md)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="93-validate-all-backups-including-customer-managed-keys"></a>9.3: Validación de todas las copias de seguridad, incluidas las claves administradas por el cliente

**Guía**: Pruebe periódicamente el proceso de restauración de las copias de seguridad de las aplicaciones de App Service.

- [Realizar una copia de seguridad de la aplicación en Azure](manage-backup.md)

- [Restauración de una aplicación web de Azure App Service](web-sites-restore.md)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="94-ensure-protection-of-backups-and-customer-managed-keys"></a>9.4: Garantía de la protección de las copias de seguridad y las claves administradas por el cliente

**Guía**: Las copias de seguridad de App Service se almacenan en una cuenta de Azure Storage. Los datos de Azure Storage se cifran y descifran de forma transparente mediante el cifrado AES de 256 bits, uno de los cifrados de bloques más sólidos que hay disponibles, y son compatibles con FIPS 140-2. El cifrado de Azure Storage es similar al cifrado de BitLocker en Windows.

El cifrado de Azure Storage está habilitado para todas las cuentas de almacenamiento, incluidas las cuentas de almacenamiento clásicas y las de Resource Manager. El cifrado de Azure Storage no se puede deshabilitar. Como los datos están protegidos de forma predeterminada, no es necesario modificar el código o las aplicaciones para aprovechar el cifrado de Azure Storage.

De manera predeterminada, los datos de una cuenta de almacenamiento se cifran con claves administradas por Microsoft. Puede confiar en las claves administradas por Microsoft para el cifrado de los datos, o puede administrar el cifrado con sus propias claves. Asegúrese de que la eliminación temporal está habilitada en Azure Key Vault.

- [Cifrado de Azure Storage para datos en reposo](../storage/common/storage-service-encryption.md)

- [Habilitación de la eliminación temporal en Azure Key Vault](../key-vault/general/key-vault-recovery.md)

**Supervisión de Azure Security Center**: Sí

**Responsabilidad**: Customer

## <a name="incident-response"></a>Respuesta a los incidentes

*Para obtener más información, consulte [Azure Security Benchmark: Respuesta a los incidentes](../security/benchmarks/security-control-incident-response.md).*

### <a name="101-create-an-incident-response-guide"></a>10.1: Creación de una guía de respuesta ante incidentes

**Instrucciones**: Cree una guía de respuesta a incidentes para su organización. Asegúrese de que haya planes de respuesta a incidentes escritos que definan todos los roles del personal, así como las fases de administración y gestión de los incidentes, desde la detección hasta la revisión posterior a la incidencia.

- [Configuración de las automatizaciones de flujos de trabajo en Azure Security Center](../security-center/security-center-planning-and-operations-guide.md)

- [Guía para crear su propio proceso de respuesta a incidentes de seguridad](https://msrc-blog.microsoft.com/2019/07/01/inside-the-msrc-building-your-own-security-incident-response-process/)

- [Anatomía de un incidente del Centro de respuestas de seguridad de Microsoft](https://msrc-blog.microsoft.com/2019/07/01/inside-the-msrc-building-your-own-security-incident-response-process/)

- [El cliente también puede usar la guía de control de incidentes de seguridad de equipos de NIST como referencia para la creación de su propio plan de respuesta a incidentes](https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-61r2.pdf)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="102-create-an-incident-scoring-and-prioritization-procedure"></a>10.2: Creación de un procedimiento de priorización y puntuación de incidentes

**Instrucciones**: Security Center asigna una gravedad a cada alerta para ayudarle a priorizar aquellas que se deben investigar en primer lugar. La gravedad se basa en la confianza que tiene Security Center en la búsqueda o en el análisis utilizados para emitir la alerta, así como en el nivel de confianza de que ha habido un intento malintencionado detrás de la actividad que ha provocado la alerta.

Además, marque las suscripciones de forma clara (por ejemplo, producción o no producción) y cree un sistema de nomenclatura para identificar y clasificar claramente los recursos de Azure.

**Supervisión de Azure Security Center**: Sí

**Responsabilidad**: Customer

### <a name="103-test-security-response-procedures"></a>10.3: Prueba de los procedimientos de respuesta de seguridad

**Guía**: Realice ejercicios para probar de forma periódica las capacidades de respuesta del sistema a los incidentes. Identifique puntos débiles y brechas y revise el plan según sea necesario.

- [Consulte la publicación de NIST: "Guía para probar, entrenar y ejecutar programas para planes y funcionalidades de TI"](https://nvlpubs.nist.gov/nistpubs/Legacy/SP/nistspecialpublication800-84.pdf).

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="104-provide-security-incident-contact-details-and-configure-alert-notifications-for-security-incidents"></a>10.4: Provisión de detalles de contacto de incidentes de seguridad y configuración de notificaciones de alerta para incidentes de seguridad

**Guía**: La información de contacto del incidente de seguridad la utilizará Microsoft para ponerse en contacto con usted si Microsoft Security Response Center (MSRC) detecta que un tercero no autorizado o ilegal ha accedido a los datos del cliente.  Revise los incidentes después del hecho para asegurarse de que se resuelven los problemas.

- [Establecimiento del contacto de seguridad de Azure Security Center](../security-center/security-center-provide-security-contact-details.md)

**Supervisión de Azure Security Center**: Sí

**Responsabilidad**: Customer

### <a name="105-incorporate-security-alerts-into-your-incident-response-system"></a>10.5: Incorporación de alertas de seguridad en el sistema de respuesta a incidentes

**Guía**: exporte sus alertas y recomendaciones de Security Center mediante la característica de exportación continua. La exportación continua le permite exportar alertas y recomendaciones de forma manual o continua. Use el conector de datos de Security Center para transmitir las alertas a Sentinel, según sea necesario.

- [Configuración de la exportación continua](../security-center/continuous-export.md)

- [Transmisión de alertas a Azure Sentinel](../sentinel/connect-azure-security-center.md)

**Supervisión de Azure Security Center**: Sí

**Responsabilidad**: Customer

### <a name="106-automate-the-response-to-security-alerts"></a>10.6: Automatización de la respuesta a las alertas de seguridad

**Guía**: Utilice la característica de automatización del flujo de trabajo de Security Center para desencadenar automáticamente respuestas mediante "Logic Apps" en las alertas y recomendaciones de seguridad.

- [Configuración de la automatización de flujo de trabajo y Logic Apps](../security-center/workflow-automation.md)

**Supervisión de Azure Security Center**: Sí

**Responsabilidad**: Customer

## <a name="penetration-tests-and-red-team-exercises"></a>Pruebas de penetración y ejercicios del equipo rojo

*Para obtener más información, consulte [Azure Security Benchmark: Pruebas de penetración y ejercicios del equipo rojo](../security/benchmarks/security-control-penetration-tests-red-team-exercises.md).*

### <a name="111-conduct-regular-penetration-testing-of-your-azure-resources-and-ensure-remediation-of-all-critical-security-findings"></a>11.1: Realice pruebas de penetración periódicas de los recursos de Azure y asegúrese de corregir todos los resultados de seguridad críticos

**Guía**: Siga las reglas de compromiso de Microsoft para asegurarse de que las pruebas de penetración no infrinjan las directivas de Microsoft: https://www.microsoft.com/msrc/pentest-rules-of-engagement?rtc=1

Puede encontrar más información sobre la estrategia y puesta en marcha de un equipo rojo de Microsoft, y las pruebas de penetración en sitios activos de la infraestructura en la nube, las aplicaciones y los servicios administrados por Microsoft.

- [Equipo rojo de Microsoft Cloud](https://gallery.technet.microsoft.com/Cloud-Red-Teaming-b837392e)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Compartido

## <a name="next-steps"></a>Pasos siguientes

- Consulte la [prueba comparativa de seguridad de Azure](../security/benchmarks/overview.md).
- Obtenga más información sobre las [líneas de base de seguridad de Azure](../security/benchmarks/security-baselines-overview.md).
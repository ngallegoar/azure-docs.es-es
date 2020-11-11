---
title: Base de referencia de seguridad de Azure para Virtual WAN
description: La base de referencia de seguridad de Virtual WAN proporciona una guía de procedimientos y recursos para implementar las recomendaciones de seguridad especificadas en Azure Security Benchmark.
author: msmbaldwin
ms.service: virtual-wan
ms.topic: conceptual
ms.date: 11/03/2020
ms.author: mbaldwin
ms.custom: subject-security-benchmark
ms.openlocfilehash: 409c8d163b52e56a1eef68a23e4db79956a68722
ms.sourcegitcommit: 96918333d87f4029d4d6af7ac44635c833abb3da
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/04/2020
ms.locfileid: "93328645"
---
# <a name="azure-security-baseline-for-virtual-wan"></a>Base de referencia de seguridad de Azure para Virtual WAN

Esta base de referencia de seguridad aplica la guía de la [versión 2.0 de Azure Security Benchmark](../security/benchmarks/overview.md) en Microsoft Azure Virtual WAN. Azure Security Benchmark proporciona recomendaciones sobre cómo puede proteger sus soluciones de nube en Azure. El contenido se agrupa por medio de los **controles de seguridad** definidos por Azure Security Benchmark y la guía relacionada aplicable a Virtual WAN. Se han excluido los **controles** no aplicables a Virtual WAN.

Para ver cómo Virtual WAN se asigna por completo a Azure Security Benchmark, consulte el [archivo de asignación de base de referencia de seguridad de Virtual WAN completo](https://github.com/MicrosoftDocs/SecurityBenchmarks/tree/master/Azure%20Offer%20Security%20Baselines).

## <a name="network-security"></a>Seguridad de redes

*Para más información, consulte [Azure Security Benchmark: seguridad de red](/azure/security/benchmarks/security-controls-v2-network-security).*

### <a name="ns-1-implement-security-for-internal-traffic"></a>NS-1: implementación de la seguridad para el tráfico interno

**Guía** : Azure Virtual WAN no admite la implementación directamente en una red virtual. Sin embargo, todavía puede aplicar reglas de grupo de seguridad de red en recursos de radio (spoke), usar protecciones de Azure Firewall o crear tablas de rutas personalizadas para impedir o permitir el tráfico privado.

- [Escenarios de enrutamiento personalizados](scenario-any-to-any.md)

**Supervisión de Azure Security Center** : no disponible actualmente

**Responsabilidad** : Customer

### <a name="ns-2-connect-private-networks-together"></a>NS-2: Conexión conjunta de redes privadas 

**Guía** : Use Azure ExpressRoute o la red privada virtual (VPN) de Azure para crear conexiones privadas entre centros de datos de Azure y una infraestructura local en un entorno de coubicación. Las conexiones ExpressRoute no utilizan la Internet pública y ofrecen más confiabilidad, velocidad, seguridad y una menor latencia que las conexiones a Internet habituales. 

Para las conexiones de punto a sitio y de sitio a sitio, conecte dispositivos o redes locales a una red virtual mediante cualquier combinación de estas opciones de VPN y ExpressRoute. Para conectar entre sí dos o más redes virtuales en Azure, use el emparejamiento de red virtual. El tráfico entre redes virtuales emparejadas es privado y se mantiene en la red troncal de Azure.

- [ExpressRoute en Virtual WAN](virtual-wan-expressroute-portal.md)

- [Introducción sobre la VPN de sitio a sitio](virtual-wan-site-to-site-portal.md)

- [Introducción sobre la VPN de punto a sitio](virtual-wan-point-to-site-portal.md)

- [Private Link](howto-private-link.md)

**Supervisión de Azure Security Center** : no disponible actualmente

**Responsabilidad** : Compartido

### <a name="ns-3-establish-private-network-access-to-azure-services"></a>NS-3: establecimiento del acceso de red privada a los servicios de Azure

**Guía** : Use Azure Private Link para habilitar el acceso privado a Azure Virtual WAN desde sus redes virtuales sin usar Internet. El acceso privado es otra medida de defensa exhaustiva a la seguridad para la autenticación y el tráfico que ofrecen los servicios de Azure.

- [¿Qué es Azure Private Link?](../private-link/private-link-overview.md)

- [Vínculo privado de Virtual WAN](howto-private-link.md)

**Supervisión de Azure Security Center** : no disponible actualmente

**Responsabilidad** : Customer

### <a name="ns-4-protect-applications-and-services-from-external-network-attacks"></a>NS-4: protección de las aplicaciones y servicios de ataques de redes externas

**Guía** : Proteja los recursos de Azure Virtual WAN de ataques de redes externas, incluidos los ataques de denegación de servicio distribuido (DDoS), los ataques específicos de la aplicación y el tráfico de Internet no solicitado y potencialmente malintencionado. 

Use Azure Firewall para proteger las aplicaciones y los servicios contra el tráfico potencialmente malintencionado de Internet y otras ubicaciones externas. Elija Azure DDoS Protection para sus recursos contra ataques en las redes virtuales de Azure. Use Azure Security Center para detectar riesgos de configuración incorrecta relacionados con los recursos de red.

- [Documentación sobre Azure Firewall](/azure/firewall)

- [Administración de Azure DDoS Protection estándar mediante Azure Portal](/azure/virtual-network/manage-ddos-protection) 

- [Recomendaciones de Azure Security Center](../security-center/recommendations-reference.md#recs-network)

**Supervisión de Azure Security Center** : Sí

**Responsabilidad** : Compartido

### <a name="ns-5-deploy-intrusion-detectionintrusion-prevention-systems-idsips"></a>NS-5: implementación de sistemas de prevención de intrusiones y detección de intrusiones (IDS/IPS)

**Guía** : La mejor manera de implementar sistemas de detección o prevención de intrusiones en Azure Virtual WAN es usar una aplicación virtual de red en las redes de radio (spoke) conectadas al centro de conectividad virtual.  Puede encontrar más información en los escenarios de enrutamiento en los vínculos a los que se hace referencia. 

- [Escenario: enrutamiento del tráfico a través de una aplicación virtual de red](scenario-route-through-nva.md)

**Supervisión de Azure Security Center** : no disponible actualmente

**Responsabilidad** : Customer

### <a name="ns-6-simplify-network-security-rules"></a>NS-6: simplificación de las reglas de seguridad de red

**Guía** : Use etiquetas de servicio de Azure Virtual Network para definir controles de acceso a la red en los grupos de seguridad de red o las instancias de Azure Firewall configuradas para los recursos de Virtual WAN. 

Use etiquetas de servicio en lugar de direcciones IP específicas al crear reglas de seguridad. Al especificar el nombre de la etiqueta de servicio (por ejemplo: {VirtualWAN: Virtual Network}) en el campo adecuado de origen o destino de una regla, puede permitir o denegar el tráfico para el servicio correspondiente. Microsoft administra los prefijos de direcciones que la etiqueta de servicio incluye y actualiza automáticamente dicha etiqueta a medida que las direcciones cambian.

**Supervisión de Azure Security Center** : no disponible actualmente

**Responsabilidad** : Customer

### <a name="ns-7-secure-domain-name-service-dns"></a>NS-7: servicio de nombres de dominio (DNS) seguro

**Guía** : Azure Virtual WAN ofrece servidores DNS personalizados para puertas de enlace de VPN de punto a sitio. 

Siga los procedimientos recomendados de la seguridad de DNS con el fin de mitigar los ataques comunes, como el DNS pendiente, los ataques de amplificación de DNS, "poisoning" y suplantación de DNS, etc.

Cuando se use Azure DNS como servicio DNS autoritativo, asegúrese de que los registros y las zonas DNS estén protegidos contra modificaciones accidentales o malintencionadas mediante el control de acceso basado en roles de Azure (Azure RBAC) y bloqueos de recursos.

- [Introducción a Azure DNS](../dns/dns-overview.md) 

- [Guía de implementación del sistema de nombres de dominio (DNS) seguro](https://csrc.nist.gov/publications/detail/sp/800-81/2/final) 

- [Evitar las entradas DNS pendientes y la adquisición de subdominios](../security/fundamentals/subdomain-takeover.md)

**Supervisión de Azure Security Center** : no disponible actualmente

**Responsabilidad** : Customer

## <a name="identity-management"></a>Administración de identidades

*Para más información, consulte [Azure Security Benchmark: Administración de identidades](/azure/security/benchmarks/security-controls-v2-identity-management).*

### <a name="im-1-standardize-azure-active-directory-as-the-central-identity-and-authentication-system"></a>IM-1: Unificación en Azure Active Directory como sistema central de identidad y autenticación

**Guía** : La VPN de punto a sitio de Azure Virtual WAN se integra en Azure Active Directory (Azure AD), que es el servicio predeterminado de administración de identidades y acceso de Azure. Debe unificar Azure AD para controlar la administración de identidades y accesos de la organización en:

- recursos de Microsoft Cloud, como Azure Portal, Azure Storage, Azure Virtual Machines (Linux y Windows), Azure Key Vault, PaaS y aplicaciones SaaS.
- Los recursos de su organización, como aplicaciones en Azure o los recursos de la red corporativa.

Proteja Azure AD con la máxima prioridad en la práctica de seguridad en la nube de su organización. Evalúe su postura de identidad y seguridad con la característica de puntuación de seguridad de Azure Security Center para medir el grado de coincidencia de la configuración con los procedimientos recomendados. Implemente los procedimientos recomendados de Microsoft para mejorar su postura de seguridad.

Además, Azure AD admite identidades externas, que permiten a los usuarios que no tienen un cuenta de Microsoft iniciar sesión en sus aplicaciones y recursos con su identidad externa. Revise la información sobre el uso de Azure AD en escenarios de VPN de punto a sitio en los vínculos a los que se hace referencia.

- [Creación de un inquilino de Azure Active Directory (AD) para conexiones del protocolo P2S OpenVPN](openvpn-azure-ad-tenant-multi-app.md)

- [Configuración de la autenticación de Azure Active Directory para una VPN de usuario](virtual-wan-point-to-site-azure-ad.md)

- [Inquilinos en Azure Active Directory](../active-directory/develop/single-and-multi-tenant-apps.md) 

- [Procedimiento para crear y configurar una instancia de Azure AD](../active-directory/fundamentals/active-directory-access-create-new-tenant.md) 

- [Uso de proveedores de identidades externos para una aplicación](/azure/active-directory/b2b/identity-providers)

**Supervisión de Azure Security Center** : Sí

**Responsabilidad** : Compartido

### <a name="im-4-use-strong-authentication-controls-for-all-azure-active-directory-based-access"></a>IM-4: Uso de controles con autenticación multifactor sólida para todo el acceso basado en Azure Active Directory

**Guía** : La VPN de punto a sitio de Azure Virtual WAN se integra en Azure Active Directory (Azure AD), que admite controles de autenticación seguros con autenticación multifactor y métodos seguros sin contraseña.

- Autenticación multifactor: habilite la autenticación multifactor de Azure AD y siga las recomendaciones de administración de identidades y acceso de Azure Security Center para conocer los procedimientos recomendados para la configuración de la autenticación multifactor. La autenticación multifactor se puede exigir a todos los usuarios, a usuarios concretos o a nivel de cada usuario en función de los factores de riesgo y las condiciones de inicio de sesión.

- Autenticación sin contraseña: hay disponibles tres opciones de autenticación sin contraseña. Estas incluyen Windows Hello para empresas, la aplicación Microsoft Authenticator y métodos de autenticación locales, como las tarjetas inteligentes.

Para administradores y usuarios con privilegios, asegúrese de que se use el nivel más alto del método de autenticación sólida, seguido de la implementación de la directiva de autenticación sólida adecuada para otros usuarios.

- [Habilitación de MFA en VPN de P2S para Virtual WAN](openvpn-azure-ad-mfa.md)

**Supervisión de Azure Security Center** : Sí

**Responsabilidad** : Compartido

### <a name="im-6-restrict-azure-resource-access-based-on-conditions"></a>IM-6: Restricción del acceso a recursos de Azure en función de las condiciones

**Guía** : La VPN de punto a sitio de Azure Virtual WAN admite el acceso condicional de Azure AD para un control de acceso más granular basado en condiciones definidas por el usuario. Por ejemplo, los inicios de sesión de usuarios de ciertos intervalos IP deben usar la autenticación multifactor para iniciar sesión. También se puede usar la directiva de administración de sesión de autenticación granular para distintos casos de uso.

- [Directivas de acceso condicional habituales](../active-directory/conditional-access/concept-conditional-access-policy-common.md) 

- [Configuración de la administración de las sesiones de autenticación con el acceso condicional](../active-directory/conditional-access/howto-conditional-access-session-lifetime.md)

- [Acceso condicional de VPN de P2S de Virtual WAN](openvpn-azure-ad-mfa.md#conditional)

**Supervisión de Azure Security Center** : no disponible actualmente

**Responsabilidad** : Customer

### <a name="im-7-eliminate-unintended-credential-exposure"></a>IM-7: Elimine la exposición de credenciales no intencionada

**Instrucciones** : Implemente el escáner de credenciales para identificar las credenciales en el código. El escáner de credenciales también fomenta el traslado de credenciales detectadas a ubicaciones más seguras, como Azure Key Vault.

En GitHub, puede usar la característica de escaneo de secretos nativos para identificar credenciales u otro tipo de secretos en el código.

- [Configuración del escáner de credenciales](https://secdevtools.azurewebsites.net/helpcredscan.html) 

- [Escaneo de secretos de GitHub](https://docs.github.com/github/administering-a-repository/about-secret-scanning)

**Supervisión de Azure Security Center** : no disponible actualmente

**Responsabilidad** : Customer

## <a name="privileged-access"></a>Acceso con privilegios

*Para más información, consulte [Azure Security Benchmark: Acceso con privilegios](/azure/security/benchmarks/security-controls-v2-privileged-access).*

### <a name="pa-2-restrict-administrative-access-to-business-critical-systems"></a>PA-2: Restricción del acceso administrativo a los sistemas críticos para la empresa

**Guía** : Azure Virtual WAN usa el control de acceso basado en roles de Azure (Azure RBAC) para aislar el acceso a los sistemas críticos para la empresa mediante la restricción de las cuentas a las que se concede acceso con privilegios a las suscripciones y los grupos de administración en los que se encuentran.

Además, restringe el acceso a los sistemas de administración, identidad y seguridad que tienen acceso administrativo a los recursos críticos para su negocio, como controladores de dominio de Active Directory, herramientas de seguridad y herramientas de administración del sistema con agentes instalados en sistemas críticos para la empresa. Los atacantes que ponen en peligro estos sistemas de administración y seguridad pueden convertirlos inmediatamente en un arma para poner en peligro los recursos críticos para la empresa.

Para garantizar un control de acceso coherente, todos los tipos de control de acceso se deben alinear con la estrategia de segmentación de la empresa.

- [Modelo de referencia y componentes de Azure](/security/compass/microsoft-security-compass-introduction#azure-components-and-reference-model-2151) 

- [Acceso al grupo de administración](../governance/management-groups/overview.md#management-group-access) 

- [Administradores de la suscripción de Azure](../cost-management-billing/manage/add-change-subscription-administrator.md)

**Supervisión de Azure Security Center** : no disponible actualmente

**Responsabilidad** : Customer

### <a name="pa-6-use-privileged-access-workstations"></a>PA-6: Uso de estaciones de trabajo con privilegios de acceso

**Guía** : Las estaciones de trabajo seguras y aisladas son de una importancia vital para la seguridad de los roles con acceso a información confidencial como administradores, desarrolladores y operadores de servicios críticos. Use estaciones de trabajo de usuario de alta seguridad o Azure Bastion para las tareas administrativas. Use Azure Active Directory (Azure AD), Protección contra amenazas avanzada (ATP) de Microsoft Defender o Microsoft Intune para implementar una estación de trabajo de usuario segura y administrada para las tareas administrativas. Las estaciones de trabajo protegidas se pueden administrar de forma centralizada para aplicar una configuración segura, como autenticación sólida, líneas de base de software y hardware y acceso lógico y de red restringido.

- [Descripción de las estaciones de trabajo con privilegios de acceso](../active-directory/devices/concept-azure-managed-workstation.md)

- [Implementación de una estación de trabajo con privilegios de acceso](../active-directory/devices/howto-azure-managed-workstation.md)

**Supervisión de Azure Security Center** : no disponible actualmente

**Responsabilidad** : Customer

## <a name="data-protection"></a>Protección de datos

*Para más información, consulte [Azure Security Benchmark: protección de datos](/azure/security/benchmarks/security-controls-v2-data-protection).*

### <a name="dp-4-encrypt-sensitive-information-in-transit"></a>DP-4: Cifrado de la información confidencial en tránsito

**Guía** : El cifrado es fundamental para el tráfico en redes externas y públicas.

- Use controles de acceso, los datos en tránsito deben protegerse frente a ataques de "fuera de banda" (por ejemplo, captura del tráfico) mediante cifrado para que los atacantes no puedan leer ni modificar los datos fácilmente.

- Para el tráfico HTTP, asegúrese de que los clientes que se conectan a los recursos de Azure puedan negociar TLS v1.2 o superior.

- Para la administración remota, use SSH (para Linux) o RDP/TLS (para Windows) en lugar de un protocolo sin cifrar. Se deben deshabilitar los protocolos y las versiones de SSL, TLS y SSH obsoletos, así como los cifrados débiles.

- En la infraestructura subyacente, Azure proporciona cifrado de datos en tránsito de manera predeterminada para el tráfico de datos entre los centros de datos de Azure.

En general, se proporciona cifrado sobre la red troncal de Microsoft segura, además de oportunidades para cifrar el tráfico en la VPN de sitio a sitio, la VPN de sitio a sitio a través de Azure ExpressRoute y la VPN de usuario de punto a sitio.

- [Descripción del cifrado en tránsito con Azure](../security/fundamentals/encryption-overview.md#encryption-of-data-in-transit)

- [Información sobre la seguridad de TLS](/security/engineering/solving-tls1-problem)

- [Cifrado doble para datos de Azure en tránsito](../security/fundamentals/double-encryption.md#data-in-transit)

**Supervisión de Azure Security Center** : Sí

**Responsabilidad** : Compartido

## <a name="asset-management"></a>Administración de recursos

*Para más información, consulte [Azure Security Benchmark: Administración de recursos](/azure/security/benchmarks/security-controls-v2-asset-management).*

### <a name="am-2-ensure-security-team-has-access-to-asset-inventory-and-metadata"></a>AM-2: Asegurarse de que el equipo de seguridad tiene acceso a los metadatos y al inventario de recursos

**Guía** : Aplique etiquetas a los recursos, grupos de recursos y suscripciones de Azure con el fin de organizarlos de manera lógica en una taxonomía. Cada etiqueta consta de un nombre y un par de valores. Por ejemplo, puede aplicar el nombre "Environment" y el valor "Production" a todos los recursos en producción.

Azure Virtual WAN también admite implementaciones de recursos basadas en Azure Resource Manager y consultas de Azure Resource Graph. 

- [Para más información sobre el etiquetado de recursos, vea la guía de decisiones de nomenclatura y etiquetado de recursos](https://docs.microsoft.com/azure/cloud-adoption-framework/decision-guides/resource-tagging/?toc=/azure/azure-resource-manager/management/toc.json).

- [Creación de consultas con Azure Resource Graph Explorer](../governance/resource-graph/first-query-portal.md) 

- [Administración del inventario de recursos de Azure Security Center](../security-center/asset-inventory.md)

**Supervisión de Azure Security Center** : Sí

**Responsabilidad** : Customer

### <a name="am-3-use-only-approved-azure-services"></a>AM-3: Uso exclusivo de servicios de Azure aprobados

**Guía** : Use Azure Policy para restringir qué servicios se pueden aprovisionar en su entorno. Consulte y detecte recursos con Azure Resource Graph dentro de las suscripciones y use Azure Monitor para crear reglas para desencadenar alertas cuando se detecte un servicio no aprobado.

- [Configuración y administración de Azure Policy](../governance/policy/tutorials/create-and-manage.md)

- [Denegación de un tipo de recurso específico con Azure Policy](../governance/policy/samples/built-in-policies.md#general)

- [Creación de consultas con Azure Resource Graph Explorer](../governance/resource-graph/first-query-portal.md)

**Supervisión de Azure Security Center** : no disponible actualmente

**Responsabilidad** : Customer

### <a name="am-5-limit-users-ability-to-interact-with-azure-resource-manager"></a>AM-5: Limitación de la capacidad de los usuarios para interactuar con Azure Resource Manager

**Guía** : Use el acceso condicional de Azure para limitar la capacidad de los usuarios de interactuar con Azure Resource Manager mediante la configuración de la opción "Bloquear acceso" en la aplicación "Administración de Microsoft Azure".

- [Configuración del acceso condicional para bloquear el acceso a Azure Resource Manager](../role-based-access-control/conditional-access-azure-management.md)

**Supervisión de Azure Security Center** : no disponible actualmente

**Responsabilidad** : Customer

## <a name="logging-and-threat-detection"></a>registro y detección de amenazas

*Para más información, consulte [Azure Security Benchmark: registro y detección de amenazas](/azure/security/benchmarks/security-controls-v2-logging-threat-protection).*

### <a name="lt-2-enable-threat-detection-for-azure-identity-and-access-management"></a>LT-2: Habilitación de la detección de amenazas para la administración de identidades y acceso de Azure

**Guía** : Azure Active Directory (Azure AD) proporciona los siguientes registros de usuario que se pueden ver en los informes de Azure AD o integrados en Azure Monitor, Azure Sentinel, SIEM o herramientas de supervisión para casos de uso de supervisión y análisis más sofisticados. Dichos componentes son:

- Inicios de sesión: El informe de inicios de sesión proporciona información sobre el uso de aplicaciones administradas y actividades de inicio de sesión de usuario.
- Los registros de auditoría proporcionan rastreabilidad mediante los registros de todos los cambios realizados por diversas características de Azure AD. Algunos ejemplos de registros de auditoría incluyen los cambios realizados en cualquier recurso de Azure AD, como agregar o quitar usuarios, aplicaciones, grupos, roles y directivas.
- Inicios de sesión de riesgo: Un inicio de sesión de riesgo es un indicador de un intento de inicio de sesión que puede haber realizado alguien que no es el propietario legítimo de una cuenta de usuario.
- Usuarios marcados en riesgo: un usuario en riesgo es un indicador de una cuenta de usuario que puede haber estado en peligro.

Use Azure Security Center para crear alertas sobre determinadas actividades sospechosas, como un número excesivo de intentos de autenticación incorrectos, incluidas cuentas en desuso en la suscripción. Además de la supervisión básica de la protección de seguridad, use el módulo de protección contra amenazas de Security Center para recopilar alertas de seguridad más detalladas de recursos individuales de proceso de Azure (máquinas virtuales, contenedores, App Service), recursos de datos (base de datos SQL y almacenamiento) y niveles de servicio de Azure. Esta funcionalidad le permite ver las anomalías de las cuentas dentro de los recursos individuales.

- [Informes de actividad de auditoría en Azure Active Directory](../active-directory/reports-monitoring/concept-audit-logs.md) 

- [Habilitación de Azure Identity Protection](../active-directory/identity-protection/overview-identity-protection.md)

**Supervisión de Azure Security Center** : Sí

**Responsabilidad** : Compartido

### <a name="lt-3-enable-logging-for-azure-network-activities"></a>LT-3: Habilitación del registro para las actividades de red de Azure

**Guía** : Use las herramientas de captura de paquetes de VPN para registrar los paquetes de red que viajan entre los recursos de Azure Virtual WAN. Esto podría ayudar en el proceso de depuración relacionado con la red híbrida. Aunque no se puede implementar un grupo de seguridad de red en Virtual WAN, puede implementarlo en los recursos de red virtual de radios (spoke).

Habilite los registros de flujo de grupos de seguridad de red en los grupos de seguridad de red para la auditoría del tráfico.

Habilite la característica Análisis de tráfico de Azure para procesar los registros de flujo que se conservan en la cuenta de almacenamiento y, a continuación, enviarlos a un área de trabajo de Log Analytics. Análisis de tráfico proporciona conclusiones adicionales sobre el flujo del tráfico de las redes de Azure. Algunas de las ventajas del Análisis de tráfico son la capacidad de visualizar la actividad de la red e identificar las zonas activas, identificar las amenazas de seguridad, comprender los patrones de flujo de tráfico y detectar configuraciones de red incorrectas.

- [Habilitación de los registros de flujo de grupos de seguridad de red](../network-watcher/network-watcher-nsg-flow-logging-portal.md) 

- [Habilitación y uso del Análisis de tráfico](../network-watcher/traffic-analytics.md) 

Virtual WAN no produce ni procesa registros de consultas de DNS que deban habilitarse.

**Supervisión de Azure Security Center** : no disponible actualmente

**Responsabilidad** : Customer

### <a name="lt-4-enable-logging-for-azure-resources"></a>LT-4: Habilitación del registro para recursos de Azure

**Guía** : Los registros de actividad de Azure, habilitados automáticamente, contienen todas las operaciones de escritura (PUT, POST, DELETE) para los recursos de Azure Virtual WAN, excepto las operaciones de lectura (GET). Los registros de actividad se pueden usar para encontrar errores durante la solución de problemas o para supervisar cómo un usuario de su organización modificó un recurso. Sin embargo, Virtual WAN no genera registros de recursos de Azure.

- [Recopilación de registros y métricas de plataforma con Azure Monitor](../azure-monitor/platform/diagnostic-settings.md) 
- [Descripción del registro y de los distintos tipos de registro de Azure](../azure-monitor/platform/platform-logs-overview.md)

**Supervisión de Azure Security Center** : Sí

**Responsabilidad** : Compartido

### <a name="lt-5-centralize-security-log-management-and-analysis"></a>LT-5: Centralizar la administración y el análisis de los registros de seguridad

**Guía** : Centralice el almacenamiento y el análisis de los registros para permitir su correlación. Asegúrese de que, para cada origen de registro, asigna un propietario de datos, una guía de acceso y una ubicación de almacenamiento; que determinar qué herramientas se usan para procesar y acceder a los datos, y los requisitos de retención de datos. Asegúrese también de que integra los registros de actividad de Azure en los sistemas de registros centrales. 

Recopile registros mediante Azure Monitor para agregar datos de seguridad generados por dispositivos de punto de conexión, recursos de red y otros sistemas de seguridad. En Azure Monitor, use áreas de trabajo de Log Analytics para realizar consultas y análisis, y use cuentas de almacenamiento de Azure para el almacenamiento de archivos a largo plazo. Además, habilite e incorpore los datos a Azure Sentinel o a un sistema SIEM de terceros.

Muchas organizaciones optan por usar Azure Sentinel para los datos de acceso frecuente y Azure Storage para los datos inactivos que se usan con menos frecuencia.

- [Recopilación de registros y métricas de plataforma con Azure Monitor](../azure-monitor/platform/diagnostic-settings.md) 

- [Incorporación de Azure Sentinel](../sentinel/quickstart-onboard.md)

**Supervisión de Azure Security Center** : no disponible actualmente

**Responsabilidad** : Compartido

## <a name="incident-response"></a>Respuesta a los incidentes

*Para más información, consulte [Azure Security Benchmark: respuesta ante incidentes](/azure/security/benchmarks/security-controls-v2-incident-response).*

### <a name="ir-1-preparation--update-incident-response-process-for-azure"></a>IR-1: Preparación: actualización del proceso de respuesta a incidentes para Azure

**Guía** : Asegúrese de que la organización tenga procesos para responder a incidentes de seguridad, que haya actualizado estos procesos para Azure y que los ejercite regularmente para garantizar su disponibilidad. Asegúrese de que la oferta de servicio esté incluida en el proceso de respuesta a incidentes, según corresponda.

- [Implementación de la seguridad en todo el entorno empresarial](https://aka.ms/AzSec4) 
- [Guía de referencia de respuesta a incidentes](https://aka.ms/IRRG)

**Supervisión de Azure Security Center** : no disponible actualmente

**Responsabilidad** : Customer

### <a name="ir-2-preparation--setup-incident-notification"></a>IR-2: Preparación: notificación de incidentes de configuración

**Guía** : Configure la información de contacto en caso de incidentes de seguridad en Azure Security Center. Microsoft utilizará esta información para ponerse en contacto con usted si el Centro de respuestas de seguridad de Microsoft (MSRC) detecta que un tercero no autorizado o ilegal ha accedido a sus datos. También hay opciones para personalizar la alerta y notificación de incidentes en diferentes servicios de Azure en función de sus necesidades de respuesta a incidentes.

La información de contacto del incidente de seguridad la utilizará Microsoft para ponerse en contacto con usted si Microsoft Security Response Center (MSRC) detecta que un tercero no autorizado o ilegal ha accedido a los datos. Revise los incidentes después del hecho para asegurarse de que se resuelvan los problemas por completo.

- [Establecimiento del contacto de seguridad de Azure Security Center](../security-center/security-center-provide-security-contact-details.md) 

- [Establecimiento del contacto de seguridad de Azure Security Center](../security-center/security-center-provide-security-contact-details.md)

**Supervisión de Azure Security Center** : Sí

**Responsabilidad** : Customer

### <a name="ir-3-detection-and-analysis--create-incidents-based-on-high-quality-alerts"></a>IR-3: Detección y análisis: creación de incidentes en función de alertas de alta calidad

**Instrucciones** : Azure Security Center asigna una gravedad a cada alerta para ayudarle a priorizar aquellas que se deben investigar en primer lugar. La gravedad se basa en la confianza que tiene Security Center en la búsqueda o en el análisis utilizados para emitir la alerta, así como en el nivel de confianza de que ha habido un intento malintencionado detrás de la actividad que ha provocado la alerta.

Además, marque las suscripciones de forma clara (por ejemplo, producción o no producción) y cree un sistema de nomenclatura para identificar y clasificar claramente los recursos de Azure.

- [Alertas de seguridad en el Centro de seguridad de Azure](../security-center/security-center-alerts-overview.md) 

- [Uso de etiquetas para organizar los recursos de Azure](../azure-resource-manager/management/tag-resources.md)

**Supervisión de Azure Security Center** : Sí

**Responsabilidad** : Customer

### <a name="ir-4-detection-and-analysis--investigate-an-incident"></a>IR-4: Detección y análisis: investigación de incidentes

**Instrucciones** : Cree una guía de respuesta a incidentes para su organización. Realice ejercicios para probar las capacidades de respuesta a los incidentes de los sistemas con regularidad. Identifique puntos débiles y brechas y revise el plan según sea necesario.

Asegúrese de que haya planes de respuesta a incidentes escritos que definan todos los roles del personal, así como las fases de administración y gestión de los incidentes, desde la detección hasta la revisión posterior a la incidencia.

- [Configuración de las automatizaciones de flujos de trabajo en Azure Security Center](../security-center/security-center-planning-and-operations-guide.md) 

- [Guía para crear su propio proceso de respuesta a incidentes de seguridad](https://msrc-blog.microsoft.com/2019/07/01/inside-the-msrc-building-your-own-security-incident-response-process) 

- [Anatomía de un incidente del Centro de respuestas de seguridad de Microsoft](https://msrc-blog.microsoft.com/2019/07/01/inside-the-msrc-building-your-own-security-incident-response-process) 

- [Consulte la publicación de NIST: Guía para probar, entrenar y ejecutar programas para planes y funcionalidades de TI](https://nvlpubs.nist.gov/nistpubs/Legacy/SP/nistspecialpublication800-84.pdf)

**Supervisión de Azure Security Center** : no disponible actualmente

**Responsabilidad** : Customer

### <a name="ir-5-detection-and-analysis--prioritize-incidents"></a>IR-5: Detección y análisis: clasificar incidentes

**Instrucciones** : Azure Security Center asigna una gravedad a cada alerta para ayudarle a priorizar aquellas que se deben investigar en primer lugar. La gravedad se basa en la confianza que tiene Security Center en la búsqueda o en el análisis utilizados para emitir la alerta, así como en el nivel de confianza de que ha habido un intento malintencionado detrás de la actividad que ha provocado la alerta.

Además, marque las suscripciones de forma clara (por ejemplo, producción o no producción) y cree un sistema de nomenclatura para identificar y clasificar claramente los recursos de Azure.

- [Alertas de seguridad en el Centro de seguridad de Azure](../security-center/security-center-alerts-overview.md) 

- [Uso de etiquetas para organizar los recursos de Azure](../azure-resource-manager/management/tag-resources.md)

**Supervisión de Azure Security Center** : Sí

**Responsabilidad** : Compartido

### <a name="ir-6-containment-eradication-and-recovery--automate-the-incident-handling"></a>IR-6: Contención, erradicación y recuperación: automatización del control de incidentes

**Guía** : Use la característica de automatización del flujo de trabajo de Azure Security Center para desencadenar automáticamente respuestas mediante " Azure Logic Apps" en las alertas y recomendaciones de seguridad.

- [Configuración de la automatización de flujo de trabajo y Logic Apps](../security-center/workflow-automation.md)

**Supervisión de Azure Security Center** : Sí

**Responsabilidad** : Customer

## <a name="posture-and-vulnerability-management"></a>administración de posturas y vulnerabilidades

*Para más información, consulte [Azure Security Benchmark: administración de posturas y vulnerabilidades](/azure/security/benchmarks/security-controls-v2-vulnerability-management).*

### <a name="pv-8-conduct-regular-attack-simulation"></a>PV-8: realización de una simulaciones de ataques periódicas

**Guía** : Según sus requisitos, realice pruebas de penetración o actividades de equipo rojo en los recursos de Azure y asegúrese de corregir todos los resultados de seguridad críticos.

siga las reglas de compromiso de la prueba de penetración de Microsoft Cloud para asegurarse de que las pruebas de penetración no infrinjan las directivas de Microsoft. Use la estrategia de Microsoft, la ejecución de pruebas de penetración de sitios activos y ataques simulados en la infraestructura en la nube, los servicios y las aplicaciones administrados por Microsoft.

- [Pruebas de penetración en Azure](../security/fundamentals/pen-testing.md)

- [Reglas de interacción de las pruebas de penetración](https://www.microsoft.com/msrc/pentest-rules-of-engagement?rtc=1) 

- [Equipo rojo de Microsoft Cloud](https://gallery.technet.microsoft.com/Cloud-Red-Teaming-b837392e)

**Supervisión de Azure Security Center** : No aplicable

**Responsabilidad** : Customer

## <a name="endpoint-security"></a>seguridad de los puntos de conexión

*Para más información, consulte [Azure Security Benchmark: seguridad de los puntos de conexión](/azure/security/benchmarks/security-controls-v2-endpoint-security).*

### <a name="es-1-use-endpoint-detection-and-response-edr"></a>ES-1: uso de la detección y respuesta de puntos de conexión (EDR)

**Guía** : A los clientes no se les permite explícitamente configurar los valores de detección y respuesta de puntos de conexión. Sin embargo, las máquinas virtuales que se usan en la oferta de Azure Virtual WAN sí usan estas funcionalidades. Obtenga más información sobre estas funcionalidades generales en los vínculos a los que se hace referencia. 

- [Introducción a protección contra amenazas avanzada de Microsoft Defender](/windows/security/threat-protection/microsoft-defender-atp/microsoft-defender-advanced-threat-protection)

- [Servicio ATP de Microsoft Defender para servidores Windows](/windows/security/threat-protection/microsoft-defender-atp/configure-server-endpoints)

- [Servicio ATP de Microsoft Defender para servidores que no son de Windows](/windows/security/threat-protection/microsoft-defender-atp/configure-endpoints-non-windows)

**Supervisión de Azure Security Center** : Sí

**Responsabilidad** : Compartido

## <a name="governance-and-strategy"></a>Gobernanza y estrategia

*Para más información, consulte [Azure Security Benchmark: gobernanza y estrategia](/azure/security/benchmarks/security-controls-v2-governance-strategy).*

### <a name="gs-1-define-asset-management-and-data-protection-strategy"></a>GS-1: Definición de la estrategia de protección de datos y administración de recursos 

**Guía** : Asegúrese de documentar y comunicar una estrategia clara para la protección y supervisión continua de sistemas y datos. Dé prioridad a la detección, evaluación, protección y supervisión de los sistemas y datos críticos para la empresa. 

Esta estrategia debe incluir instrucciones, directivas y estándares documentados para los siguientes elementos: 

- Norma de clasificación de datos de acuerdo con los riesgos empresariales
- Visibilidad en la organización de seguridad de los riesgos y el inventario de recursos 
- Aprobación de la organización de seguridad de los servicios de Azure para su uso 
- Seguridad de los recursos durante su ciclo de vida
- Estrategia de control de acceso necesaria según la clasificación de datos de la organización
- Uso de las funcionalidades de protección de datos nativa de Azure y de terceros
- Requisitos de cifrado de datos para casos de uso en tránsito y en reposo
- Normas criptográficas adecuadas

Revise la información adicional disponible en los vínculos a los que se hace referencia.

- [Recomendación para la arquitectura de seguridad de Azure: almacenamiento, datos y cifrado](https://docs.microsoft.com/azure/architecture/framework/security/storage-data-encryption?toc=/security/compass/toc.json&amp;bc=/security/compass/breadcrumb/toc.json)

- [Aspectos básicos de la seguridad de Azure: seguridad, cifrado y almacenamiento de datos de Azure](../security/fundamentals/encryption-overview.md)

- [Cloud Adoption Framework: procedimientos recomendados de cifrado y seguridad de los datos de Azure](https://docs.microsoft.com/azure/security/fundamentals/data-encryption-best-practices?toc=/azure/cloud-adoption-framework/toc.json&amp;bc=/azure/cloud-adoption-framework/_bread/toc.json)

**Supervisión de Azure Security Center** : no disponible actualmente

**Responsabilidad** : Customer

### <a name="gs-2-define-enterprise-segmentation-strategy"></a>GS-2: Definición de una estrategia de segmentación empresarial 

**Guía** : Establezca en toda la empresa una estrategia para segmentar el acceso a los recursos mediante una combinación de identidad, red, aplicación, suscripción, grupo de administración y otros controles. Equilibre concienzudamente la necesidad de separación de seguridad con la necesidad de habilitar el funcionamiento diario de los sistemas que necesitan comunicarse entre sí y acceder a los datos.

Asegúrese de que la estrategia de segmentación se implemente de forma coherente en todos los tipos de control, como la seguridad de red, los modelos de identidad y acceso, y los modelos de acceso o permisos de las aplicaciones, y los controles de los procesos humanos.

- [Guía de la estrategia de segmentación en Azure (vídeo)](/security/compass/microsoft-security-compass-introduction#azure-components-and-reference-model-2151)

- [Guía de la estrategia de segmentación en Azure (documento)](/security/compass/governance#enterprise-segmentation-strategy)

- [Alineación de la segmentación de red con la estrategia de segmentación empresarial](/security/compass/network-security-containment#align-network-segmentation-with-enterprise-segmentation-strategy)

**Supervisión de Azure Security Center** : no disponible actualmente

**Responsabilidad** : Customer

### <a name="gs-3-define-security-posture-management-strategy"></a>GS-3: Definición de la estrategia de administración de la posición de seguridad

**Guía** : Mida y mitigue continuamente los riesgos de los recursos individuales y el entorno donde se hospedan. Dé prioridad a los recursos de gran valor y a las superficies de ataque muy expuestas, como las aplicaciones publicadas, los puntos de entrada y salida de red, los puntos de conexión de usuario y administrador, etc.

- [Azure Security Benchmark: administración de posturas y vulnerabilidades](/azure/security/benchmarks/security-benchmark-v2-posture-vulnerability-management)

**Supervisión de Azure Security Center** : no disponible actualmente

**Responsabilidad** : Customer

### <a name="gs-4-align-organization-roles-responsibilities-and-accountabilities"></a>GS-4: Alineación de los roles y responsabilidades de la organización

**Guía** : Asegúrese de documentar y comunicar una estrategia clara para los roles y las responsabilidades de la organización de seguridad. Dé prioridad a los esfuerzos en función de una responsabilidad clara en las decisiones de seguridad, proporcione a todos los usuarios formación sobre el modelo de responsabilidad compartida y a los equipos técnicos sobre la tecnología para proteger la nube.

- [Procedimiento recomendado de seguridad de Azure 1. Personas: Formación del equipo sobre el recorrido de seguridad de la nube](https://aka.ms/AzSec1)

- [Procedimiento recomendado de seguridad de Azure 2. Personas: Formación del equipo en tecnología de seguridad de la nube](https://aka.ms/AzSec2)

- [Procedimiento recomendado de seguridad de Azure 3. Proceso: Asignación de responsabilidades por las decisiones de seguridad en la nube](https://aka.ms/AzSec3)

**Supervisión de Azure Security Center** : no disponible actualmente

**Responsabilidad** : Customer

### <a name="gs-5-define-network-security-strategy"></a>GS-5: Definición de la estrategia de seguridad de red

**Guía** : Establezca un enfoque de seguridad de red de Azure como parte de la estrategia de control de acceso de seguridad general de su organización. Esta estrategia debe incluir instrucciones, directivas y estándares documentados para los siguientes elementos: 

- Centralización de la responsabilidad de seguridad y la administración de redes
- Modelo de segmentación de la red virtual alineado con la estrategia de segmentación empresarial
- Estrategia de corrección en diferentes escenarios de amenazas y ataques
- Estrategia de entrada y salida y perimetral de Internet
- Estrategia de interconectividad entre el entorno local y la nube híbrida
- Artefactos de seguridad de red actualizados (por ejemplo, diagramas de red y arquitectura de red de referencia)

Revise la información adicional disponible en los vínculos a los que se hace referencia.

- [Procedimiento recomendado de seguridad de Azure 11. Arquitectura: Establecimiento de una estrategia de seguridad unificada](https://aka.ms/AzSec11)

- [Azure Security Benchmark: seguridad de red](/azure/security/benchmarks/security-benchmark-v2-network-security)

- [Azure Network Security Overview (Información general sobre Azure Network Security)](../security/fundamentals/network-overview.md)

- [Estrategia para la arquitectura de red empresarial](/azure/cloud-adoption-framework/ready/enterprise-scale/architecture)

**Supervisión de Azure Security Center** : no disponible actualmente

**Responsabilidad** : Customer

### <a name="gs-6-define-identity-and-privileged-access-strategy"></a>GS-6: Definición de la estrategia de acceso con privilegios e identidades

**Guía** : Establezca una identidad de Azure y enfoques de acceso con privilegios como parte de la estrategia de control de acceso de seguridad general de su organización. Esta estrategia debe incluir instrucciones, directivas y estándares documentados para los siguientes elementos: 

- Un sistema de identidad y autenticación centralizado y su interconectividad con otros sistemas de identidad internos y externos
- Métodos de autenticación sólida en diferentes casos de uso y condiciones
- Protección de usuarios con privilegios elevados
- Supervisión y control de anomalías en las actividades de los usuarios  
- Proceso de revisión y conciliación del acceso y la identidad de los usuarios

Revise la información adicional disponible en los vínculos a los que se hace referencia.

- [Azure Security Benchmark: administración de identidades](/azure/security/benchmarks/security-benchmark-v2-identity-management)

- [Azure Security Benchmark: acceso con privilegios](/azure/security/benchmarks/security-benchmark-v2-privileged-access)

- [Procedimiento recomendado de seguridad de Azure 11. Arquitectura: Establecimiento de una estrategia de seguridad unificada](https://aka.ms/AzSec11)

- [Información general sobre seguridad de administración de identidades de Azure](../security/fundamentals/identity-management-overview.md)

**Supervisión de Azure Security Center** : no disponible actualmente

**Responsabilidad** : Customer

### <a name="gs-7-define-logging-and-threat-response-strategy"></a>GS-7: Definición de la estrategia de registro y respuesta a amenazas

**Guía** : Establezca una estrategia de registro y respuesta a amenazas para detectar y corregir rápidamente las amenazas mientras cumple los requisitos de cumplimiento. Dé prioridad a ofrecer a los analistas alertas de alta calidad y experiencias fluidas para que puedan centrarse en las amenazas, en lugar de en la integración y los pasos manuales. 

Esta estrategia debe incluir instrucciones, directivas y estándares documentados para los siguientes elementos: 

- Las responsabilidades y el rol de la organización en las operaciones de seguridad (SecOps)
- Un proceso de respuesta a incidentes bien definido que esté alineado con NIST u otro marco del sector.
- Captura y retención de registros para admitir la detección de amenazas, la respuesta ante incidentes y las necesidades de cumplimiento
- Visibilidad centralizada de las amenazas e información de correlación sobre amenazas, mediante SIEM, funcionalidades nativas de Azure y otros orígenes: comunicación y plan de notificaciones con los clientes, proveedores y entidades públicas de interés
- Uso de plataformas nativas de Azure y de terceros para el tratamiento de incidentes, como el registro y la detección de amenazas, los análisis forenses y la corrección y erradicación de ataques
- Procesos para controlar incidentes y actividades posteriores a incidentes, como las lecciones aprendidas y la retención de pruebas

Revise la información adicional disponible en los vínculos a los que se hace referencia.
- [Azure Security Benchmark: registro y detección de amenazas](/azure/security/benchmarks/security-benchmark-v2-logging-threat-detection)

- [Azure Security Benchmark: respuesta a incidentes](/azure/security/benchmarks/security-benchmark-v2-incident-response)

- [Procedimiento recomendado de seguridad de Azure 4. Proceso: Actualización de los procesos de respuesta a incidentes para la nube](https://aka.ms/AzSec4)

- [Guía de decisiones sobre registros e informes en Azure Adoption Framework](/azure/cloud-adoption-framework/decision-guides/logging-and-reporting/)

**Supervisión de Azure Security Center** : no disponible actualmente

**Responsabilidad** : Customer

## <a name="next-steps"></a>Pasos siguientes

- Consulte la [Información general sobre Azure Security Benchmark V2](/azure/security/benchmarks/overview).
- Obtenga más información sobre las [líneas de base de seguridad de Azure](/azure/security/benchmarks/security-baselines-overview).

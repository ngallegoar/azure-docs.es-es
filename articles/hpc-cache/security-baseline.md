---
title: Línea de base de seguridad de Azure para Azure HPC Cache
description: La línea de base de seguridad de Azure HPC Cache proporciona una guía de procedimientos y recursos para implementar las recomendaciones de seguridad especificadas en Azure Security Benchmark.
author: msmbaldwin
ms.service: hpc-cache
ms.topic: conceptual
ms.date: 11/19/2020
ms.author: mbaldwin
ms.custom: subject-security-benchmark
ms.openlocfilehash: 036cab033e5b70a8940a488c07374e9092d52b33
ms.sourcegitcommit: 5b93010b69895f146b5afd637a42f17d780c165b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/02/2020
ms.locfileid: "96533662"
---
# <a name="azure-security-baseline-for-azure-hpc-cache"></a>Línea de base de seguridad de Azure para Azure HPC Cache

Esta línea de base de seguridad se aplica en la guía de la [versión 2.0 de Azure Security Benchmark](../security/benchmarks/overview.md) en Microsoft Azure HPC Cache. Azure Security Benchmark proporciona recomendaciones sobre cómo puede proteger sus soluciones de nube en Azure. El contenido se agrupa por medio de los **controles de seguridad** que define Azure Security Benchmark y las directrices aplicables en Azure HPC Cache. Se han excluido los **controles** no aplicables a Azure HPC Cache.

Para ver cómo se adapta Azure HPC Cache por completo a Azure Security Benchmark, consulte el [archivo completo de asignación de línea de base de seguridad de Azure HPC Cache](https://github.com/MicrosoftDocs/SecurityBenchmarks/tree/master/Azure%20Offer%20Security%20Baselines).

## <a name="network-security"></a>Seguridad de redes

*Para más información, consulte [Azure Security Benchmark: seguridad de red](../security/benchmarks/security-controls-v2-network-security.md).*

### <a name="ns-1-implement-security-for-internal-traffic"></a>NS-1: implementación de la seguridad para el tráfico interno

**Guía**: Al implementar los recursos de Azure HPC Cache, debe crear o usar una red virtual existente. 

Asegúrese de que todas las redes virtuales de Azure siguen un principio de segmentación empresarial que se adapte a los riesgos empresariales. Todos los sistemas que podrían suponer un riesgo mayor para la organización deben aislarse en su propia red virtual y estar lo suficientemente protegidos con Azure Firewall o un grupo de seguridad de red (NSG).

Para poder usar la caché, primero es necesario configurar dos opciones relacionadas con la red: 

- Una subred dedicada para la instancia de Azure HPC Cache

- Compatibilidad con DNS para que la caché pueda acceder al almacenamiento y otros recursos

Azure HPC Cache necesita una subred dedicada con estas cualidades: 

- La subred debe tener al menos 64 direcciones IP disponibles.

- La subred no puede hospedar ninguna otra máquina virtual, ni siquiera para servicios relacionados, como máquinas cliente.

- Si usa varias instancias de Azure HPC Cache, cada una necesita su propia subred.

El procedimiento recomendado es crear una subred para cada caché. Puede crear una red virtual y una subred como parte de la creación de la caché.

Para usar HPC Cache con el almacenamiento de NAS local, debe asegurarse de que determinados puertos de la red local permiten el tráfico sin restricciones de la subred de Azure HPC Cache. 

- [Configuración de puertos para el acceso al almacenamiento de NFS](hpc-cache-prerequisites.md#nfs-storage-requirements)

- [Creación de un grupo de seguridad de red con reglas de seguridad](../virtual-network/tutorial-filter-network-traffic.md)

 

- [Implementación y configuración de Azure Firewall](../firewall/tutorial-firewall-deploy-portal.md)

**Supervisión de Azure Security Center**: Sí

**Responsabilidad**: Customer

### <a name="ns-2-connect-private-networks-together"></a>NS-2: Conexión conjunta de redes privadas

**Guía**: Use Azure ExpressRoute o la red privada virtual (VPN) de Azure para crear conexiones privadas entre centros de datos de Azure y una infraestructura local en un entorno de coubicación. Las conexiones ExpressRoute no usan la Internet pública, y ofrecen más confiabilidad, velocidad, seguridad y una menor latencia que las conexiones a Internet habituales. Para las conexiones de punto a sitio y de sitio a sitio, puede conectar dispositivos o redes locales a una red virtual mediante cualquier combinación de estas opciones de VPN y Azure ExpressRoute. 

Para conectar entre sí dos o más redes virtuales en Azure, use el emparejamiento de redes virtuales. El tráfico entre redes virtuales emparejadas es privado y se mantiene en la red troncal de Azure.

- [Qué son los modelos de conectividad de ExpressRoute](../expressroute/expressroute-connectivity-models.md) 

- [Información general sobre la red privada virtual de Azure](../vpn-gateway/vpn-gateway-about-vpngateways.md) 

- [Interconexión de red virtual](../virtual-network/virtual-network-peering-overview.md)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="ns-3-establish-private-network-access-to-azure-services"></a>NS-3: establecimiento del acceso de red privada a los servicios de Azure

**Guía**: use los puntos de conexión de servicio de Azure Virtual Network para proporcionar un acceso seguro a HPC Cache. Los puntos de conexión de servicio son una ruta optimizada a través de la red troncal de Azure que no pasan por Internet. 

HPC Cache no admite el uso de Azure Private Link para proteger sus puntos de conexión de administración en una red privada. 

El acceso privado es una medida de defensa exhaustiva adicional a la seguridad para la autenticación y el tráfico que ofrecen los servicios de Azure.

- [Introducción a los puntos de conexión de servicio de red virtual](../virtual-network/virtual-network-service-endpoints-overview.md) 

- [Información sobre cómo montar Azure HPC Cache](hpc-cache-mount.md)

**Supervisión de Azure Security Center**: no disponible actualmente

**Responsabilidad**: Customer

### <a name="ns-4-protect-applications-and-services-from-external-network-attacks"></a>NS-4: protección de las aplicaciones y servicios de ataques de redes externas

**Guía**: Proteja los recursos de HPC Cache de ataques de redes externas, incluidos los ataques de denegación de servicio distribuido (DDoS), los ataques específicos de la aplicación y el tráfico de Internet no solicitado y potencialmente malintencionado. 

Azure incluye funcionalidades nativas para esta protección: 

- Use Azure Firewall para proteger las aplicaciones y los servicios contra el tráfico potencialmente malintencionado de Internet y otras ubicaciones externas. 
- Proteja sus recursos de ataques DDoS al habilitar la protección contra DDoS estándar en las redes virtuales de Azure. 
- Use Azure Security Center para detectar riesgos de configuración incorrecta relacionados con los recursos de red.

Azure HPC Cache no está diseñado para ejecutar aplicaciones web y no requiere que se configuren opciones adicionales ni se implementen servicios de red complementarios para protegerlo de ataques de red externos que tienen como destino aplicaciones web.

- [Documentación sobre Azure Firewall](../firewall/index.yml) 

- [Administración de Azure DDoS Protection estándar mediante Azure Portal](../ddos-protection/manage-ddos-protection.md) 

- [Recomendaciones de Azure Security Center](../security-center/recommendations-reference.md#recs-network)

**Supervisión de Azure Security Center**: Sí

**Responsabilidad**: Customer

### <a name="ns-5-deploy-intrusion-detectionintrusion-prevention-systems-idsips"></a>NS-5: implementación de sistemas de prevención de intrusiones y detección de intrusiones (IDS/IPS)

**Guía**: Use Azure Firewall con el filtrado basado en inteligencia sobre amenazas para alertar o bloquear el tráfico desde y hacia dominios y direcciones IP malintencionados y conocidos. La direcciones IP y los dominios proceden de la fuente Inteligencia sobre amenazas de Microsoft. 

Cuando deba realizar la inspección de las cargas, puede implementar desde Azure Marketplace un sistema de prevención y detección de intrusiones (IDS/IPS) de terceros que cuente con funcionalidades de inspección de cargas. Como alternativa, puede elegir usar IDS/IPS basado en host o una solución de detección y respuesta de punto de conexión (EDR) basada en host junto un IDS/IPS basado en redes, o en lugar de este.

Nota: Si tiene un requisito normativo o de otro tipo para el uso de IDS/IPS, asegúrese de que siempre esté optimizado para proporcionar alertas de alta calidad a la solución de SIEM.

- [Implementación de Azure Firewall](../firewall/tutorial-firewall-deploy-portal.md) 

- [Capacidades de IDS de terceros de Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace?search=IDS) 

- [Funcionalidad de detección y respuesta de la protección contra amenazas avanzada de Microsoft Defender](/windows/security/threat-protection/microsoft-defender-atp/overview-endpoint-detection-response)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="ns-6-simplify-network-security-rules"></a>NS-6: simplificación de las reglas de seguridad de red

**Guía**: no es aplicable; esta recomendación está destinada a las ofertas que se pueden implementar en instancias de Azure Virtual Network o que tengan la capacidad de definir agrupaciones de intervalos IP permitidos para conseguir una administración eficaz. Actualmente HPC Cache no admite etiquetas de servicio. 

El procedimiento recomendado es crear una subred para cada caché. Puede crear una red virtual y una subred como parte de la creación de la caché.

**Supervisión de Azure Security Center**: no disponible actualmente

**Responsabilidad**: Customer

### <a name="ns-7-secure-domain-name-service-dns"></a>NS-7: servicio de nombres de dominio (DNS) seguro

**Guía**: Siga los procedimientos recomendados de la seguridad de DNS con el fin de mitigar ataques comunes como el de DNS pendiente, los ataques de amplificación de DNS de tipo "poisoning" y suplantación de DNS, etc.

Azure HPC Cache necesita DNS para obtener acceso a recursos que estén fuera de la red virtual privada de la memoria caché. Si el flujo de trabajo incluye recursos fuera de Azure, debe configurar y proteger su propio servidor DNS además de usar Azure DNS.

- Para obtener acceso a los puntos de conexión de Azure Blob Storage, los equipos cliente basados en Azure u otros recursos de Azure, use Azure DNS.
- Para obtener acceso a un almacenamiento local o conectarse a la memoria caché desde clientes que estén fuera de Azure, debe crear un servidor DNS personalizado que pueda resolver esos nombres de host.
- Si el flujo de trabajo incluye recursos internos y externos, configure el servidor DNS personalizado para que reenvíe las solicitudes de resolución específicas de Azure al servidor de Azure DNS. 

Cuando se usa Azure DNS como servicio DNS autoritativo, asegúrese de que los registros y las zonas DNS están protegidos contra modificaciones accidentales o malintencionadas mediante Azure RBAC y bloqueos de recursos.

Si configura su propio servidor DNS, asegúrese de seguir estas instrucciones de seguridad:

- [Guía de implementación del sistema de nombres de dominio (DNS) seguro](https://csrc.nist.gov/publications/detail/sp/800-81/2/final) 

- [Evitar las entradas DNS pendientes y la adquisición de subdominios](../security/fundamentals/subdomain-takeover.md) 

- [Más información sobre los requisitos de acceso DNS para HPC Cache](hpc-cache-prerequisites.md#dns-access)

- [Introducción a Azure DNS](../dns/dns-overview.md)

**Supervisión de Azure Security Center**: no disponible actualmente

**Responsabilidad**: Customer

## <a name="identity-management"></a>Administración de identidades

*Para más información, consulte [Azure Security Benchmark: Administración de identidades](../security/benchmarks/security-controls-v2-identity-management.md).*

### <a name="im-1-standardize-azure-active-directory-as-the-central-identity-and-authentication-system"></a>IM-1: Unificación en Azure Active Directory como sistema central de identidad y autenticación

**Guía**: Azure HPC Cache no se integra con Azure Active Directory para realizar operaciones internas. Sin embargo, Azure AD puede usarse para autenticar a los usuarios en Azure Portal o la CLI con el fin de crear, ver y administrar las implementaciones de HPC Cache y los componentes relacionados.

Azure Active Directory (Azure AD) es un servicio de administración de identidades y acceso predeterminado de Azure. Debe unificar Azure AD para controlar la administración de identidades y accesos de la organización en:

- Los recursos de Microsoft Cloud, como Azure Portal, Azure Storage, Azure Virtual Machines (Linux y Windows), Azure Key Vault, PaaS y aplicaciones SaaS.

- Los recursos de su organización, como aplicaciones en Azure o los recursos de la red corporativa.

La protección de Azure AD debe tener máxima prioridad en la práctica de seguridad en la nube de su organización. Azure AD proporciona una puntuación de seguridad de la identidad para ayudarle a evaluar la posición de seguridad de las identidades en relación con los procedimientos recomendados de Microsoft. Use la puntuación para medir el grado de coincidencia de la configuración con los procedimientos recomendados y para hacer mejoras en la posición de seguridad.

Nota: Azure AD admite identidades externas que permiten a los usuarios que no tienen una cuenta de Microsoft iniciar sesión en sus aplicaciones y recursos mediante la identidad externa.

- [Inquilinos en Azure Active Directory](../active-directory/develop/single-and-multi-tenant-apps.md) 

- [Procedimiento para crear y configurar una instancia de Azure AD](../active-directory/fundamentals/active-directory-access-create-new-tenant.md) 

- [Uso de proveedores de identidades externos para una aplicación](../active-directory/external-identities/identity-providers.md) 

- [¿Qué es la puntuación de seguridad de la identidad en Azure Active Directory?](../active-directory/fundamentals/identity-secure-score.md)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="im-2-manage-application-identities-securely-and-automatically"></a>IM-2: Administración de identidades de aplicaciones de forma segura y automática

**Guía**: HPC Cache usa identidades administradas de Azure en cuentas que no gestionan usuarios, como los servicios o la automatización. Se recomienda usar la característica de identidades administradas de Azure en lugar de crear una cuenta de usuario con más permisos para acceder a los recursos o ejecutarlos. 

HPC Cache puede autenticarse de forma nativa en los servicios y recursos de Azure que admiten la autenticación de Azure AD a través de reglas de concesión de acceso predefinidas. Esto evita la necesidad de usar credenciales codificadas de forma rígida en los archivos de código fuente o de configuración.

- [Identidades administradas de Azure](../active-directory/managed-identities-azure-resources/overview.md) 

- [Servicios que admiten identidades administradas para recursos de Azure](../active-directory/managed-identities-azure-resources/services-support-managed-identities.md)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="im-3-use-azure-ad-single-sign-on-sso-for-application-access"></a>IM-3: Uso del inicio de sesión único de Azure AD para acceder a las aplicaciones

**Guía**: Azure HPC Cache no se integra con Azure AD para realizar operaciones internas. Sin embargo, Azure AD puede usarse para autenticar a los usuarios en Azure Portal o la CLI con el fin de crear, ver y administrar las implementaciones de HPC Cache y los componentes relacionados.

Azure Active Directory proporciona la opción de administración de identidades y acceso a los recursos de Azure, las aplicaciones en la nube y las aplicaciones locales. Esto incluye no solo las identidades empresariales, como los empleados, sino también las identidades externas, como asociados y proveedores. Esto permite que el inicio de sesión único (SSO) administre y proteja el acceso a los datos y recursos de la organización locales y en la nube. Conecte todos los usuarios, las aplicaciones y los dispositivos a Azure AD para obtener un acceso seguro y sin problemas, y para lograr mayor visibilidad y control.

- [Descripción del inicio de sesión único de aplicaciones con Azure AD](../active-directory/manage-apps/what-is-single-sign-on.md)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="im-4-use-strong-authentication-controls-for-all-azure-active-directory-based-access"></a>IM-4: Uso de controles con autenticación multifactor sólida para todo el acceso basado en Azure Active Directory

**Guía**: Aunque Azure HPC Cache no se integra con Azure AD para realizar operaciones internas, Azure AD se puede usar para autenticar usuarios en Azure Portal o la CLI para así poder crear, ver y administrar las implementaciones de HPC Cache y otros componentes relacionados.  

Azure AD admite controles de autenticación sólida a través de la autenticación multifactor (MFA) y métodos seguros sin contraseña.

- Autenticación multifactor: habilite Azure AD MFA y siga las recomendaciones de administración de identidades y acceso de Azure Security Center para conocer varios procedimientos recomendados en la configuración de la autenticación multifactor. La MFA se puede exigir a todos los usuarios, a usuarios concretos o a nivel de cada usuario en función de los factores de riesgo y las condiciones de inicio de sesión.
- Autenticación sin contraseña: hay disponibles tres opciones de autenticación sin contraseña: Windows Hello para empresas, aplicación Microsoft Authenticator y métodos de autenticación locales, como las tarjetas inteligentes.

En cuanto a administradores y usuarios con privilegios, asegúrese de usar el nivel más alto del método de autenticación sólida. Implemente la directiva de autenticación sólida adecuada para otros usuarios.

Azure HPC Cache también admite la autenticación heredada basada en contraseñas en los sistemas cliente que se conectan a la memoria caché. Estos tipos de conexiones incluyen cuentas solo para la nube (esto es, cuentas de usuario creadas directamente en Azure) que tienen una directiva de contraseñas de línea de base o cuentas híbridas (cuentas de usuario que provienen de instancias de Active Directory locales) que seguirán las directivas de contraseñas locales. 

Cuando se usa la autenticación con contraseña, Azure AD proporciona una funcionalidad de protección de contraseña que impide que los usuarios establezcan contraseñas fáciles de adivinar. Microsoft proporciona una lista global de contraseñas prohibidas que se actualiza en función de la telemetría; asimismo, los clientes pueden ampliarla en función de sus necesidades (por ejemplo, mediante la personalización de marca o referencias culturales entre otros). Esta protección de contraseñas se puede usar para las cuentas híbridas y solo en la nube.

Nota: La autenticación basada solo en las credenciales de contraseña es susceptible a métodos de ataque populares. Para una mayor seguridad, use una autenticación sólida, como MFA y una directiva de contraseñas segura. Si usa aplicaciones de terceros y servicios de Marketplace que tienen contraseñas predeterminadas, establezca una contraseña nueva y segura la primera vez que configure el servicio. 

- [Procedimiento para habilitar la MFA en Azure](../active-directory/authentication/howto-mfa-getstarted.md) 

- [Introducción a las opciones de autenticación sin contraseña de Azure Active Directory](../active-directory/authentication/concept-authentication-passwordless.md)

- [Directiva de contraseñas predeterminada de Azure AD](../active-directory/authentication/concept-sspr-policy.md#password-policies-that-only-apply-to-cloud-user-accounts)

- [Eliminación de contraseñas incorrectas mediante la Protección con contraseña de Azure AD](../active-directory/authentication/concept-password-ban-bad.md)

**Supervisión de Azure Security Center**: Sí

**Responsabilidad**: Customer

### <a name="im-5-monitor-and-alert-on-account-anomalies"></a>IM-5: Supervisión y alerta de anomalías de cuenta

**Guía**: Azure HPC Cache puede usar Azure Active Directory para la administración de usuarios desde Azure Portal.  Igualmente, Azure Active Directory proporciona los siguientes orígenes de datos:

- Inicios de sesión: el informe de los inicios de sesión proporciona información sobre el uso de aplicaciones administradas y las actividades de inicio de sesión del usuario.

- Los registros de auditoría proporcionan rastreabilidad mediante los registros de todos los cambios realizados por diversas características de Azure AD. Algunos ejemplos de registros de auditoría incluyen los cambios realizados en cualquier recurso de Azure AD como agregar o quitar usuarios, aplicaciones, grupos, roles y directivas.

- Inicios de sesión de riesgo: un inicio de sesión de riesgo es un indicador de un intento de inicio de sesión que puede haber realizado alguien que no es el propietario legítimo de una cuenta de usuario.

- Usuarios marcados en riesgo: un usuario en riesgo es un indicador de una cuenta de usuario que puede haber estado en peligro.

Estos orígenes de datos se pueden integrar con Azure Monitor, Azure Sentinel o sistemas SIEM de terceros.

Azure Security Center también puede alertar sobre determinadas actividades sospechosas, como un número excesivo de intentos de autenticación incorrectos y cuentas en desuso en la suscripción.

Azure Advanced Threat Protection (AATP) es una solución de seguridad que puede usar señales de Active Directory para identificar, detectar e investigar amenazas avanzadas, identidades en riesgo y acciones internas malintencionadas.

- [Informes de actividad de auditoría en Azure Active Directory](../active-directory/reports-monitoring/concept-audit-logs.md)

- [Visualización de los inicios de sesión de riesgo de Azure AD](../active-directory/identity-protection/overview-identity-protection.md) 

- [Procedimiento para identificar usuarios de Azure AD marcados por una actividad de riesgo](../active-directory/identity-protection/overview-identity-protection.md) 

- [Supervisión de la actividad de identidad y acceso de los usuarios en Azure Security Center](../security-center/security-center-identity-access.md)

 

- [Alertas del módulo de protección de inteligencia sobre amenazas de Azure Security Center](../security-center/alerts-reference.md) 

- [Integración de los registros de actividad de Azure en Azure Monitor](../active-directory/reports-monitoring/howto-integrate-activity-logs-with-log-analytics.md)

**Supervisión de Azure Security Center**: Sí

**Responsabilidad**: Compartido

### <a name="im-7-eliminate-unintended-credential-exposure"></a>IM-7: Elimine la exposición de credenciales no intencionada

**Guía**: no aplicable; HPC Cache no permite a los clientes implementar datos persistentes en el entorno en ejecución.

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

## <a name="privileged-access"></a>Acceso con privilegios

*Para más información, consulte [Azure Security Benchmark: Acceso con privilegios](../security/benchmarks/security-controls-v2-privileged-access.md).*

### <a name="pa-2-restrict-administrative-access-to-business-critical-systems"></a>PA-2: Restricción del acceso administrativo a los sistemas críticos para la empresa

**Guía**: HPC Cache usa Azure RBAC para aislar el acceso a los sistemas críticos para la empresa mediante la restricción de las cuentas a las que se concede acceso con privilegios a las suscripciones y los grupos de administración en los que se encuentran.

Cree procedimientos operativos estándar en torno al uso de cuentas administrativas dedicadas. Para crear una memoria caché, HPC Cache requiere que los usuarios tengan privilegios suficientes en la suscripción para crear las NIC. Si usa Blob Storage, el colaborador de la cuenta de almacenamiento de roles de Azure y el colaborador de datos de Blob Storage son necesarios para que HPC Cache obtenga acceso al almacenamiento. 

Asegúrese de restringir también el acceso a los sistemas de administración, identidad y seguridad que tienen acceso administrativo a los recursos críticos para su negocio, como controladores de dominio (DC) de Active Directory, herramientas de seguridad y herramientas de administración del sistema con agentes instalados en sistemas críticos para la empresa. Los atacantes que ponen en peligro estos sistemas de administración y seguridad pueden convertirlos inmediatamente en un arma para poner en peligro los recursos críticos para la empresa.

Para garantizar un control de acceso coherente, todos los tipos de control de acceso se deben alinear con la estrategia de segmentación de la empresa.

- [Permisos de RBAC en Azure HPC Cache](hpc-cache-prerequisites.md#permissions)

- [Modelo de referencia y componentes de Azure](/security/compass/microsoft-security-compass-introduction#azure-components-and-reference-model-2151) 

- [Acceso al grupo de administración](../governance/management-groups/overview.md#management-group-access)

- [Administradores de la suscripción de Azure](../cost-management-billing/manage/add-change-subscription-administrator.md)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="pa-3-review-and-reconcile-user-access-regularly"></a>PA-3: Revisión y conciliación de manera periódica del acceso de los usuarios

**Guía**: Revise las cuentas de usuario y la asignación de acceso con regularidad para asegurarse de que las cuentas y sus niveles acceso sean válidos. 

Azure HPC Cache puede usar cuentas de Azure Active Directory (Azure AD) para administrar el acceso de los usuarios a través de Azure Portal y las interfaces relacionadas. Asimismo, Azure AD ofrece revisiones de acceso que le permitirán revisar las pertenencias a grupos, el acceso a las aplicaciones empresariales y las asignaciones de roles. Los informes de Azure AD pueden proporcionar registros para ayudar a detectar cuentas obsoletas. También puede usar Azure AD Privileged Identity Management para crear un flujo de trabajo de informes de revisión de acceso para facilitar el proceso de revisión.

Además, se puede configurar Azure Privileged Identity Management para enviar una alerta cuando se cree un número de cuentas de administrador excesivo y para identificar las cuentas de administrador que hayan quedado obsoletas o que no estén configuradas correctamente.

Nota: Algunos servicios de Azure admiten usuarios y roles locales que no se administran mediante Azure AD. Estos usuarios deberán administrarse por separado.

Al usar destinos de almacenamiento de NFS, tendrá que trabajar con los administradores de red y de firewall para comprobar la configuración de acceso y asegurarse de que Azure HPC Cache podrá comunicarse con los sistemas de almacenamiento NFS.

- [Para obtener una lista de permisos de HPC Cache, lea los requisitos previos de Azure HPC Cache](hpc-cache-prerequisites.md) 

- [Creación de una revisión de acceso de los roles de recursos de Azure en Privileged Identity Management (PIM)](../active-directory/privileged-identity-management/pim-resource-roles-start-access-review.md) 

- [Procedimiento para usar las revisiones de acceso e identidades de Azure AD](../active-directory/governance/access-reviews-overview.md)

**Supervisión de Azure Security Center**: Sí

**Responsabilidad**: Customer

### <a name="pa-4-set-up-emergency-access-in-azure-ad"></a>PA-4: Configuración del acceso de emergencia en Azure AD

**Guía**: HPC Cache puede usar Azure Active Directory para administrar el acceso a los recursos a través de Azure Portal. Para evitar que se le bloquee por accidente el acceso a la organización de Azure AD, configure una cuenta de acceso de emergencia para cuando no se puedan usar cuentas administrativas normales. Las cuentas de acceso de emergencia tienen normalmente privilegios elevados y no se asignan a usuarios específicos. Las cuentas de acceso de emergencia se limitan a situaciones "excepcionales" o de emergencia en las que no se pueden usar las cuentas administrativas normales.

Debe asegurarse de que las credenciales (como contraseña, certificado o tarjeta inteligente) de las cuentas de acceso de emergencia estén protegidas y solo las conozcan aquellas personas que estén autorizadas a usarlas solo en caso de emergencia.

- [Administración de cuentas de acceso de emergencia en Azure AD](../active-directory/roles/security-emergency-access.md)

**Supervisión de Azure Security Center**: no disponible actualmente

**Responsabilidad**: Customer

### <a name="pa-5-automate-entitlement-management"></a>PA-5: Automatización de la administración de derechos 

**Guía**: HPC Cache puede usar Azure Active Directory para administrar el acceso a los recursos de la memoria caché a través de Azure Portal. 

Use las características de administración de derechos de Azure AD para automatizar los flujos de trabajo de solicitud de acceso, como las asignaciones, las revisiones y la expiración del acceso. También se admite la aprobación en dos o varias fases. 

- [¿Qué son las revisiones de acceso de Azure AD?](../active-directory/governance/access-reviews-overview.md) 

- [¿Qué es la administración de derechos de Azure AD?](../active-directory/governance/entitlement-management-overview.md)

**Supervisión de Azure Security Center**: no disponible actualmente

**Responsabilidad**: Customer

### <a name="pa-7-follow-just-enough-administration-least-privilege-principle"></a>PA-7: Seguimiento de solo una administración suficiente (principio de privilegios mínimos) 

**Guía**: HPC Cache se integra en el control de acceso basado en roles de Azure (Azure RBAC) para administrar sus recursos. Azure RBAC le permite administrar el acceso a los recursos de Azure mediante las asignaciones de roles. Puede asignar estos roles a usuarios, grupos de entidades de servicio e identidades administradas. Hay roles integrados predefinidos para determinados recursos, y estos roles se pueden inventariar o consultar mediante herramientas como la CLI de Azure, Azure PowerShell o el Azure Portal. 

Los privilegios que asigne a los recursos a través de Azure RBAC siempre se deben limitar a los requisitos de los roles. Este modelo complementa al enfoque Just-in-Time (JIT) de Azure AD Privileged Identity Management (PIM) y se debe revisar periódicamente.

Use los roles integrados para asignar los permisos y crear solo el rol personalizado cuando sea necesario.

- [¿Qué es el control de acceso basado en rol de Azure (RBAC)?](../role-based-access-control/overview.md) 

- [Configuración de RBAC en Azure](../role-based-access-control/role-assignments-portal.md) 

- [Procedimiento para usar las revisiones de acceso e identidades de Azure AD](../active-directory/governance/access-reviews-overview.md)

**Supervisión de Azure Security Center**: Sí

**Responsabilidad**: Customer

## <a name="data-protection"></a>Protección de datos

*Para más información, consulte [Azure Security Benchmark: protección de datos](../security/benchmarks/security-controls-v2-data-protection.md).*

### <a name="dp-1-discover-classify-and-label-sensitive-data"></a>DP-1: detección, clasificación y etiquetado de datos confidenciales 

**Guía**: HPC Cache administra datos confidenciales, pero no tiene capacidad para detectar, clasificar y etiquetar esos datos.

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Compartido

### <a name="dp-2-protect-sensitive-data"></a>DP-2: Protección de datos confidenciales

**Guía**: Proteja los datos confidenciales mediante la restricción del acceso con el control de acceso basado en rol de Azure (Azure RBAC), los controles de acceso basados en la red y los controles específicos de los servicios de Azure (como el cifrado en SQL y otras bases de datos).

Para garantizar un control de acceso coherente, todos los tipos de control de acceso se deben alinear con la estrategia de segmentación de la empresa. La estrategia de segmentación de la empresa también debe estar informada de la ubicación de los datos y sistemas confidenciales o críticos para la empresa.

En el caso de la plataforma subyacente administrada por Microsoft, Microsoft trata todo el contenido de los clientes como confidencial y protege a los clientes contra la pérdida y exposición de los datos. Para garantizar la seguridad de los datos de los clientes dentro de Azure, Microsoft ha implementado algunos controles y funcionalidades de protección de datos predeterminados.

- [Control de acceso basado en rol (RBAC) de Azure](../role-based-access-control/overview.md) 

- [Descripción de la protección de datos de los clientes en Azure](../security/fundamentals/protection-customer-data.md)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Compartido

### <a name="dp-3-monitor-for-unauthorized-transfer-of-sensitive-data"></a>DP-3: Supervisión de la transferencia no autorizada de datos confidenciales

**Guía**: HPC Cache transmite datos confidenciales, pero no admite la supervisión de la transferencia no autorizada de datos confidenciales.

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Compartido

### <a name="dp-4-encrypt-sensitive-information-in-transit"></a>DP-4: Cifrado de la información confidencial en tránsito

**Guía**: HPC Cache admite el cifrado de datos en tránsito con TLS v1.2 u otra versión posterior.

Aunque esto es opcional en el caso del tráfico de redes privadas, es fundamental para el tráfico de redes externas y públicas. Asegúrese de que los clientes que se conectan a los recursos de Azure puedan negociar TLS v1.2 o superior. Para la administración remota, use SSH (para Linux) o RDP/TLS (para Windows) en lugar de un protocolo sin cifrar. Se deben deshabilitar los protocolos y las versiones de SSL, TLS y SSH obsoletos, así como los cifrados débiles.

De forma predeterminada, Azure proporciona el cifrado de los datos en tránsito entre los centros de datos de Azure.

- [Descripción del cifrado en tránsito con Azure](../security/fundamentals/encryption-overview.md#encryption-of-data-in-transit) 

- [Información sobre la seguridad de TLS](/security/engineering/solving-tls1-problem) 

- [Cifrado doble para datos de Azure en tránsito](../security/fundamentals/double-encryption.md#data-in-transit)

**Supervisión de Azure Security Center**: Sí

**Responsabilidad**: Compartido

### <a name="dp-5-encrypt-sensitive-data-at-rest"></a>DP-5: Cifrado de datos confidenciales en reposo

**Guía**: Para complementar los controles de acceso, los datos en reposo deben protegerse frente a ataques de tipo "fuera de banda" (por ejemplo, el acceso al almacenamiento subyacente) mediante cifrado. Esto ayuda a garantizar que los atacantes no puedan leer ni modificar los datos fácilmente.

Azure proporciona cifrado de datos en reposo de manera predeterminada. En el caso de datos muy confidenciales, tiene opciones para implementar el cifrado adicional en reposo en todos los recursos de Azure donde estén disponibles. Azure administra las claves de cifrado de forma predeterminada, pero también le ofrece opciones para administrar sus propias claves (claves administradas por el cliente) en determinados servicios de Azure.

Todos los datos almacenados en Azure, incluidos los discos de caché, se cifran en reposo mediante claves administradas por Microsoft de forma predeterminada. Solo tiene que personalizar la configuración de Azure HPC Cache si quiere administrar las claves que se usan para cifrar los datos.

Si es necesario, implemente una herramienta de terceros para el cumplimiento de los recursos de proceso, como una solución de prevención de pérdida de datos basada en host automatizada, para aplicar controles de acceso a los datos incluso cuando se copian datos de un sistema.

- [Uso de claves de cifrado administradas por el cliente con Azure HPC Cache](./hpc-cache-create.md?tabs=azure-portal#enable-azure-key-vault-encryption-optional)

- [Descripción del cifrado en reposo en Azure](../security/fundamentals/encryption-atrest.md#encryption-at-rest-in-microsoft-cloud-services) 

- [Configuración de las claves de cifrado administradas por el cliente](../storage/common/customer-managed-keys-configure-key-vault.md) 

- [Tabla de modelo de cifrado y administración de claves](../security/fundamentals/encryption-atrest.md)

 

- [Cifrado doble de datos en reposo en Azure](../security/fundamentals/double-encryption.md#data-at-rest)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Compartido

## <a name="asset-management"></a>Administración de recursos

*Para más información, consulte [Azure Security Benchmark: Administración de recursos](../security/benchmarks/security-controls-v2-asset-management.md).*

### <a name="am-1-ensure-security-team-has-visibility-into-risks-for-assets"></a>AM-1: Asegurarse de que el equipo de seguridad tiene visibilidad sobre los riesgos para los recursos

**Guía**: Asegúrese de que se conceden a los equipos de seguridad permisos de lector de seguridad en el inquilino y las suscripciones de Azure para que puedan supervisar los riesgos de seguridad mediante Azure Security Center. 

En función de la estructura de las responsabilidades del equipo de seguridad, la supervisión de los riesgos de seguridad puede ser responsabilidad de un equipo de seguridad central o de un equipo local. Dicho esto, la información y los riesgos de seguridad siempre se deben agregar de forma centralizada dentro de una organización. 

Los permisos de lector de seguridad se pueden aplicar en general a un inquilino completo (grupo de administración raíz) o a grupos de administración o a suscripciones específicas. 

Nota: Es posible que se requieran permisos adicionales para obtener visibilidad de las cargas de trabajo y los servicios. 

- [Introducción al rol de lector de seguridad](../role-based-access-control/built-in-roles.md#security-reader)

- [Información general sobre los grupos de administración de Azure](../governance/management-groups/overview.md)

**Supervisión de Azure Security Center**: no disponible actualmente

**Responsabilidad**: Customer

### <a name="am-2-ensure-security-team-has-access-to-asset-inventory-and-metadata"></a>AM-2: Asegurarse de que el equipo de seguridad tiene acceso a los metadatos y al inventario de recursos

**Guía**: Azure HPC Cache admite el uso de etiquetas. Aplique etiquetas a los recursos, grupos de recursos y suscripciones de Azure con el fin de organizarlos de manera lógica en una taxonomía. Cada etiqueta consta de un nombre y un par de valores. 

Por ejemplo, puede aplicar el nombre "Environment" y el valor "Production" a todos los recursos en producción. Se pueden agregar etiquetas al crear una memoria caché, así como después de la implementación de la misma. 

Use el inventario de máquinas virtuales de Azure para automatizar la recopilación de información sobre el software en Virtual Machines. El nombre del software, la versión, el publicador y la hora de actualización están disponibles en Azure Portal. Para obtener acceso a la fecha de instalación y otra información, habilite los diagnósticos de nivel de invitado y transfiera los registros de eventos de Windows a un área de trabajo de Log Analytics.
HPC Cache no permite la ejecución de una aplicación o instalación de software en sus recursos. 

- [Creación de consultas con Azure Resource Graph Explorer](../governance/resource-graph/first-query-portal.md) 

- [Administración del inventario de recursos de Azure Security Center](../security-center/asset-inventory.md) 

- [Guía de decisiones de nomenclatura y etiquetado de recursos](/azure/cloud-adoption-framework/decision-guides/resource-tagging/?toc=%2fazure%2fazure-resource-manager%2fmanagement%2ftoc.json) 

- [Habilitación del inventario de máquinas virtuales de Azure](../automation/automation-tutorial-installed-software.md)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="am-3-use-only-approved-azure-services"></a>AM-3: Uso exclusivo de servicios de Azure aprobados

**Guía**: HPC Cache admite implementaciones de Azure Resource Manager. Use Azure Policy para auditar y restringir qué servicios pueden aprovisionar los usuarios en su entorno. Use Azure Resource Graph para consultar y detectar recursos dentro de sus suscripciones. También puede usar Azure Monitor para crear reglas para desencadenar alertas cuando se detecta un servicio no aprobado.

- [Configuración y administración de Azure Policy](../governance/policy/tutorials/create-and-manage.md) 

- [Denegación de un tipo de recurso específico con Azure Policy](../governance/policy/samples/index.md) 

- [Creación de consultas con Azure Resource Graph Explorer](../governance/resource-graph/first-query-portal.md)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="am-4-ensure-security-of-asset-lifecycle-management"></a>AM-4: Garantizar la seguridad de la administración del ciclo de vida de los recursos

**Instrucciones**: No aplicable. Azure HPC Cache no se puede usar para garantizar la seguridad de los recursos en un proceso de administración del ciclo de vida. Es responsabilidad del cliente mantener los atributos y las configuraciones de red de los recursos que se consideren de gran impacto. 

Asimismo, es recomendable que el cliente cree un proceso para capturar los cambios de configuración de los atributos y la red, medir el efecto de tales cambios y crear tareas de corrección, según corresponda.

**Supervisión de Azure Security Center**: no disponible actualmente

**Responsabilidad**: Customer

## <a name="logging-and-threat-detection"></a>registro y detección de amenazas

*Para más información, consulte [Azure Security Benchmark: registro y detección de amenazas](/azure/security/benchmarks/security-controls-v2-logging-threat-detection).*

### <a name="lt-1-enable-threat-detection-for-azure-resources"></a>LT-1: Habilitación de la detección de amenazas para recursos de Azure

**Guía**: Use la funcionalidad de detección de amenazas integrada de Azure Security Center y habilite Azure Defender (conocido formalmente como Azure Advanced Threat Protection) para los recursos de HPC Cache. Azure Defender para HPC Cache proporciona una capa adicional de inteligencia de seguridad que detecta intentos poco habituales y potencialmente peligrosos de acceder a los recursos de la memoria caché o vulnerarlos.

Reenvíe los registros de HPC Cache su SIEM, ya que puede usarse para configurar detecciones de amenazas personalizadas. Asegúrese de que está supervisando distintos tipos de recursos de Azure para detectar posibles amenazas y anomalías. Céntrese en obtener alertas de alta calidad para reducir los falsos positivos que deben revisar los analistas. Las alertas se pueden crear a partir de datos de registro, agentes u otros datos.

- [Protección contra amenazas en Azure Security Center](../security-center/azure-defender.md) 

- [Guía de referencia de alertas de seguridad de Azure Security Center](../security-center/alerts-reference.md) 

- [Creación de reglas de análisis personalizadas para detectar amenazas](../sentinel/tutorial-detect-threats-custom.md) 

- [Inteligencia sobre amenazas cibernéticas con Azure Sentinel](/azure/architecture/example-scenario/data/sentinel-threat-intelligence)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="lt-2-enable-threat-detection-for-azure-identity-and-access-management"></a>LT-2: Habilitación de la detección de amenazas para la administración de identidades y acceso de Azure

**Guía**: Azure AD proporciona los siguientes registros de usuario que se pueden ver en los informes de Azure AD o que están integrados con Azure Monitor, Azure Sentinel u otras herramientas de SIEM o supervisión para casos más sofisticados de uso de supervisión y análisis:
- Inicios de sesión: el informe de inicios de sesión proporciona información sobre el uso de aplicaciones administradas y actividades de inicio de sesión de usuario.

- Los registros de auditoría proporcionan rastreabilidad mediante los registros de todos los cambios realizados por diversas características de Azure AD. Algunos ejemplos de registros de auditoría incluyen los cambios realizados en cualquier recurso de Azure AD, como agregar o quitar usuarios, aplicaciones, grupos, roles y directivas.

- Inicios de sesión de riesgo: un inicio de sesión de riesgo es un indicador de un intento de inicio de sesión que puede haber realizado alguien que no es el propietario legítimo de una cuenta de usuario.

- Usuarios marcados en riesgo: un usuario en riesgo es un indicador de una cuenta de usuario que puede haber estado en peligro.

Azure Security Center también puede alertar sobre determinadas actividades sospechosas, como un número excesivo de intentos de autenticación incorrectos o cuentas en desuso en la suscripción. Además de la supervisión básica de la protección de seguridad, el módulo de protección contra amenazas de Azure Security Center también puede recopilar alertas de seguridad más detalladas de recursos individuales de proceso de Azure (máquinas virtuales, contenedores, App Service), recursos de datos (base de datos SQL y almacenamiento) y niveles de servicio de Azure. Esta funcionalidad le permite ver las anomalías de las cuentas dentro de los recursos individuales.

- [Informes de actividad de auditoría en Azure Active Directory](../active-directory/reports-monitoring/concept-audit-logs.md) 

- [Habilitación de Azure Identity Protection](../active-directory/identity-protection/overview-identity-protection.md) 

- [Protección contra amenazas en Azure Security Center](../security-center/azure-defender.md)

**Supervisión de Azure Security Center**: Sí

**Responsabilidad**: Customer

### <a name="lt-3-enable-logging-for-azure-network-activities"></a>LT-3: Habilitación del registro para las actividades de red de Azure

**Guía**: además de las herramientas de captura de paquetes que normalmente están disponibles, también puede usar puertas de enlace de VPN y la funcionalidad de captura de paquetes para registrar los paquetes de red que viajan entre las redes virtuales.

Implemente un grupo de seguridad de red en la red donde se implementan los recursos de Azure HPC Cache. Habilite los registros de flujo de grupos de seguridad de red en los grupos de seguridad de red para la auditoría del tráfico.

Los registros de flujo se conservan en una cuenta de almacenamiento. Habilite la solución de Análisis de tráfico para procesar y enviar los registros de flujo a un área de trabajo de Log Analytics. Análisis de tráfico proporciona conclusiones adicionales sobre el flujo del tráfico de las redes de Azure. El Análisis de tráfico le permite visualizar la actividad de la red e identificar las zonas activas, identificar las amenazas de seguridad, comprender los patrones de flujo de tráfico y detectar configuraciones de red incorrectas.

La caché necesita DNS para acceder a los recursos que están fuera de su red virtual. En función de los recursos que use, puede que tenga que configurar un servidor DNS personalizado y el reenvío entre ese servidor y los servidores de Azure DNS. 

Implemente una solución de terceros de Azure Marketplace para la solución de registro DNS según las necesidades de su organización.

- [Configuración de captura de paquetes para VPN Gateways](../vpn-gateway/packet-capture.md) 

- [Habilitación de los registros de flujo de grupos de seguridad de red](../network-watcher/network-watcher-nsg-flow-logging-portal.md) 

- [Habilitación y uso del Análisis de tráfico](../network-watcher/traffic-analytics.md) 

- [Descripción de la seguridad de red proporcionada por Azure Security Center](../security-center/security-center-network-recommendations.md) 

- [Obtenga más información acerca de los requisitos previos de DNS](hpc-cache-prerequisites.md)

**Supervisión de Azure Security Center**: Sí

**Responsabilidad**: Customer

### <a name="lt-4-enable-logging-for-azure-resources"></a>LT-4: Habilitación del registro para recursos de Azure

**Guía**: los recursos de Azure HPC Cache crean automáticamente registros de actividad. Estos registros contienen todas las operaciones de escritura (PUT, POST, DELETE), pero no incluyen las operaciones de lectura (GET). Los registros de actividad se pueden usar para encontrar errores al solucionar problemas o para supervisar cómo un usuario de su organización modificó un recurso.

También puede usar Azure Security Center y Azure Policy para habilitar los registros de recursos de Azure para HPC Cache y para recopilar datos de registro. Estos registros pueden ser críticos para la investigación posterior de incidentes de seguridad y la realización de ejercicios forenses.

- [Recopilación de registros y métricas de plataforma con Azure Monitor](../azure-monitor/platform/diagnostic-settings.md) 

- [Descripción del registro y de los distintos tipos de registro de Azure](../azure-monitor/platform/platform-logs-overview.md) 

- [Descripción de la recopilación de datos de Azure Security Center](../security-center/security-center-enable-data-collection.md)

**Supervisión de Azure Security Center**: Sí

**Responsabilidad**: Compartido

### <a name="lt-5-centralize-security-log-management-and-analysis"></a>LT-5: Centralizar la administración y el análisis de los registros de seguridad

**Guía**: Centralice el almacenamiento y el análisis de los registros para permitir su correlación. Asegúrese de que asigna en cada origen de registro un propietario de datos, una guía de acceso, una ubicación de almacenamiento, qué herramientas se van a usar para procesar y acceder a los datos y los requisitos de retención de datos.

Asegúrese también de que integra los registros de actividad de Azure en el registro central. Recopile registros mediante Azure Monitor para agregar datos de seguridad generados por dispositivos de punto de conexión, recursos de red y otros sistemas de seguridad. En Azure Monitor, use áreas de trabajo de Log Analytics para realizar consultas y análisis, y use cuentas de Azure Storage para el almacenamiento de archivos a largo plazo.

Además, habilite e incorpore los datos a Azure Sentinel o a un sistema SIEM de terceros.

Muchas organizaciones optan por usar Azure Sentinel para los datos de acceso frecuente y Azure Storage para los datos inactivos que se usan con menos frecuencia.

- [Recopilación de registros y métricas de plataforma con Azure Monitor](../azure-monitor/platform/diagnostic-settings.md) 

- [Incorporación de Azure Sentinel](../sentinel/quickstart-onboard.md)

**Supervisión de Azure Security Center**: Sí

**Responsabilidad**: Customer

## <a name="incident-response"></a>Respuesta a los incidentes

*Para más información, consulte [Azure Security Benchmark: respuesta ante incidentes](../security/benchmarks/security-controls-v2-incident-response.md).*

### <a name="ir-1-preparation--update-incident-response-process-for-azure"></a>IR-1: Preparación: actualización del proceso de respuesta a incidentes para Azure

**Guía**: Asegúrese de que la organización tenga procesos para responder a incidentes de seguridad, que haya actualizado estos procesos para Azure y que los ejercite regularmente para garantizar su disponibilidad.

- [Implementación de la seguridad en todo el entorno empresarial](/azure/cloud-adoption-framework/security/security-top-10#4-process-update-incident-response-ir-processes-for-cloud)

- [Guía de referencia de respuesta a incidentes](/microsoft-365/downloads/IR-Reference-Guide.pdf)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="ir-2-preparation--set-up-incident-notification"></a>IR-2: Preparación: notificación de incidentes de configuración 

**Guía**: Configure la información de contacto en caso de incidentes de seguridad en Azure Security Center. Microsoft utilizará esta información para ponerse en contacto con usted si el Centro de respuestas de seguridad de Microsoft (MSRC) detecta que un tercero no autorizado o ilegal ha accedido a sus datos. También hay opciones para personalizar la alerta y notificación de incidentes en diferentes servicios de Azure en función de sus necesidades de respuesta a incidentes. 

- [Establecimiento del contacto de seguridad de Azure Security Center](../security-center/security-center-provide-security-contact-details.md)

**Supervisión de Azure Security Center**: Sí

**Responsabilidad**: Customer

### <a name="ir-3-detection-and-analysis--create-incidents-based-on-high-quality-alerts"></a>IR-3: Detección y análisis: creación de incidentes en función de alertas de alta calidad

**Guía**: Asegúrese de que cuenta con un proceso para crear alertas de alta calidad y medir la calidad de las alertas. Esto le permite aprender de incidentes anteriores y clasificar las alertas para los analistas, de modo que no pierdan tiempo con falsos positivos. 

Las alertas de alta calidad se pueden crear en función de la experiencia de pasados incidentes, información validada procedente de la comunidad y herramientas diseñadas para generar y limpiar alertas mediante la fusión y correlación de diversos orígenes de la señal. 

Azure Security Center proporciona alertas de alta calidad en muchos recursos de Azure. Puede usar el conector de datos de ASC para enviar las alertas a Azure Sentinel. Azure Sentinel le permite crear reglas de alerta avanzadas con el fin de generar automáticamente incidentes para investigar. 

Exporte las alertas y recomendaciones de Azure Security Center mediante la característica de exportación para ayudar a identificar riesgos en los recursos de Azure. Esta exportación puede hacerse de forma manual o de modo continuo.

- [Configuración de la exportación](../security-center/continuous-export.md)

- [Transmisión de alertas a Azure Sentinel](../sentinel/connect-azure-security-center.md)

**Supervisión de Azure Security Center**: no disponible actualmente

**Responsabilidad**: Customer

### <a name="ir-4-detection-and-analysis--investigate-an-incident"></a>IR-4: Detección y análisis: investigación de incidentes

**Guía**: Asegúrese de que los analistas pueden consultar y usar diversos orígenes de datos a medida que investigan posibles incidentes, para conseguir una visión global de lo que ha sucedido. Se deben recopilar diversos registros para realizar un seguimiento de las actividades de un posible atacante en la cadena de eliminación para evitar puntos ciegos.  También debe asegurarse de que se capturan detalles y aprendizajes para otros analistas y para futuras referencias históricas.  

Los orígenes de datos para la investigación incluyen los orígenes de registro centralizados que ya se recopilan de los servicios en el ámbito y los sistemas en ejecución, pero también pueden incluir:

- Datos de red: use registros de flujo de grupos de seguridad de red, Azure Network Watcher y Azure Monitor para capturar registros de flujo de red y otra información de análisis. 

- Instantáneas de sistemas en ejecución: 

    - Use la funcionalidad de instantáneas de la máquina virtual de Azure para crear una instantánea del disco del sistema en ejecución. 

    - Use la funcionalidad de volcado de la memoria nativa del sistema operativo para crear una instantánea de la memoria del sistema en ejecución.

    - Use la característica de instantánea de los servicios de Azure o la propia funcionalidad del software para crear instantáneas de los sistemas en ejecución.

Azure Sentinel proporciona amplios análisis de datos en prácticamente cualquier origen de registro y un portal de administración de casos para administrar todo el ciclo de vida de los incidentes. La información de inteligencia durante una investigación puede asociarse a un incidente con fines de seguimiento e informes. 

- [Instantánea del disco de una máquina Windows](../virtual-machines/windows/snapshot-copy-managed-disk.md)

- [Instantánea del disco de una máquina Linux](../virtual-machines/linux/snapshot-copy-managed-disk.md)

- [Recopilación del volcado de memoria e información de diagnóstico del servicio de soporte técnico de Microsoft Azure](https://azure.microsoft.com/support/legal/support-diagnostic-information-collection/) 

- [Investigación de incidentes con Azure Sentinel](../sentinel/tutorial-investigate-cases.md)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="ir-5-detection-and-analysis--prioritize-incidents"></a>IR-5: Detección y análisis: clasificar incidentes

**Guía**: Proporcione contexto a los analistas sobre los incidentes en los que deberán centrarse en primer lugar en función de la gravedad de la alerta y la confidencialidad de los recursos. 

Azure Security Center asigna una gravedad a cada alerta para ayudarle a priorizar aquellas que se deben investigar en primer lugar. La gravedad se basa en la confianza que tiene Security Center en la búsqueda o en el análisis que se usa para emitir la alerta, así como en el nivel de confianza para determinar si ha habido un intento malintencionado detrás de la actividad que ha provocado la alerta.

Adicionalmente, marque los recursos con etiquetas y cree un sistema de nomenclatura para identificar y clasificar los recursos de Azure, especialmente los que procesan datos confidenciales.  Es su responsabilidad asignar prioridades a la corrección de las alertas en función de la importancia de los recursos y el entorno de Azure donde se produjo el incidente.

- [Alertas de seguridad en el Centro de seguridad de Azure](../security-center/security-center-alerts-overview.md)

- [Uso de etiquetas para organizar los recursos de Azure](../azure-resource-manager/management/tag-resources.md)

**Supervisión de Azure Security Center**: no disponible actualmente

**Responsabilidad**: Customer

### <a name="ir-6-containment-eradication-and-recovery--automate-the-incident-handling"></a>IR-6: Contención, erradicación y recuperación: automatización del control de incidentes

**Guía**: Automatice las tareas repetitivas manuales para acelerar el tiempo de respuesta y reducir la carga de los analistas. Las tareas manuales tardan más tiempo en ejecutarse, lo que ralentiza cada incidente y reduce el número de incidentes que un analista puede manejar. Las tareas manuales también aumentan la fatiga del analista, lo que aumenta el riesgo del error humano que produce retrasos y reduce la capacidad de los analistas de centrarse de manera efectiva en tareas complejas. Use las características de automatización de flujo de trabajo de Azure Security Center y Azure Sentinel para desencadenar acciones automáticamente o ejecutar un cuaderno de estrategias para responder a las alertas de seguridad entrantes. El cuaderno de estrategias lleva a cabo acciones, como el envío de notificaciones, la deshabilitación de cuentas y el aislamiento de redes problemáticas. 

- [Configuración de la automatización de flujos de trabajo en Security Center](../security-center/workflow-automation.md)

- [Configuración de respuestas automatizadas frente a amenazas en Azure Security Center](../security-center/tutorial-security-incident.md#triage-security-alerts)

- [Configuración de respuestas automatizadas frente a amenazas en Azure Sentinel](../sentinel/tutorial-respond-threats-playbook.md)

**Supervisión de Azure Security Center**: no disponible actualmente

**Responsabilidad**: Customer

## <a name="posture-and-vulnerability-management"></a>administración de posturas y vulnerabilidades

*Para más información, consulte [Azure Security Benchmark: administración de posturas y vulnerabilidades](/azure/security/benchmarks/security-controls-v2-posture-vulnerability-management).*

### <a name="pv-3-establish-secure-configurations-for-compute-resources"></a>PV-3: establecimiento de configuraciones seguras para los recursos de proceso

**Guía**: use Azure Security Center y Azure Policy para establecer configuraciones seguras en todos los recursos de proceso, incluidas las VM, los contenedores y otros elementos.

- [Cómo supervisar las recomendaciones de Azure Security Center](../security-center/security-center-recommendations.md) 

- [Guía de referencia sobre las recomendaciones de seguridad](../security-center/recommendations-reference.md)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="pv-8-conduct-regular-attack-simulation"></a>PV-8: realización de una simulaciones de ataques periódicas

**Guía**: Según sea necesario, realice pruebas de penetración o actividades de equipo rojo en los recursos de Azure y asegúrese de corregir todos los resultados de seguridad críticos.
siga las reglas de compromiso de la prueba de penetración de Microsoft Cloud para asegurarse de que las pruebas de penetración no infrinjan las directivas de Microsoft. Use la estrategia de Microsoft y la ejecución de las pruebas de penetración del equipo rojo y sitios activos en la infraestructura de nube, los servicios y las aplicaciones administradas por Microsoft.

- [Pruebas de penetración en Azure](../security/fundamentals/pen-testing.md)

- [Reglas de interacción de las pruebas de penetración](https://www.microsoft.com/msrc/pentest-rules-of-engagement?rtc=1) 

- [Equipo rojo de Microsoft Cloud](https://gallery.technet.microsoft.com/Cloud-Red-Teaming-b837392e)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Compartido

## <a name="backup-and-recovery"></a>Copia de seguridad y recuperación

*Para más información, consulte [Azure Security Benchmark: Copia de seguridad y recuperación](../security/benchmarks/security-controls-v2-backup-recovery.md).*

### <a name="br-1-ensure-regular-automated-backups"></a>BR-1: Garantía de copias de seguridad automáticas periódicas

**Guía**: dado que Azure HPC Cache es una solución de almacenamiento en caché y no un sistema de almacenamiento, céntrese en garantizar que se realice regularmente una copia de seguridad de los datos en sus destinos de almacenamiento. Siga los procedimientos estándar para los contenedores de blobs de Azure y para realizar copias de seguridad de los destinos de almacenamiento de forma local. 

Para minimizar las perturbaciones que se puedan producir en caso de una interrupción regional, puede tomar medidas para garantizar el acceso a los datos entre regiones.  

Cada instancia de Azure HPC Cache se ejecuta en una suscripción concreta y en una región. Esto significa que es posible que el flujo de trabajo de la memoria caché se interrumpa si la región sufre una interrupción completa. Para minimizar esta interrupción, la organización debe usar un almacenamiento de back-end que sea accesible desde varias regiones. Este almacenamiento puede ser un sistema NAS local con compatibilidad con DNS adecuada o una instancia de Azure Blob Storage que resida en una región distinta de la caché.

A medida que se realiza el flujo de trabajo en la región primaria, los datos se guardan en el almacenamiento a largo plazo fuera de la región. Si la región de la caché deja de estar disponible, puede crear una instancia de Azure HPC Cache duplicada en una región secundaria, conectarse al mismo almacenamiento y reanudar el trabajo desde la nueva caché.

- [Más información sobre la conmutación por error regional](hpc-region-recovery.md)

**Supervisión de Azure Security Center**: no disponible actualmente

**Responsabilidad**: Customer

### <a name="br-2-encrypt-backup-data"></a>BR-2: Cifrado de los datos de copia de seguridad

**Guía**: Asegúrese de que las copias de seguridad están protegidas frente a ataques. Esto debe incluir el cifrado de las copias de seguridad para proteger frente a la pérdida de confidencialidad.

En el caso de las copias de seguridad locales mediante Azure Backup, se proporciona cifrado en reposo mediante la frase de contraseña que proporcione. En el caso de las copias de seguridad periódicas de servicios de Azure, los datos de copia de seguridad se cifran automáticamente mediante claves administradas por la plataforma de Azure. Puede optar por cifrar la copia de seguridad mediante claves administradas por el cliente. En este caso, asegúrese de que esta clave administrada por el cliente del almacén de claves también esté en el ámbito de la copia de seguridad.

Azure HPC Cache también está protegida por el cifrado del host de la máquina virtual en los discos administrados que contienen los datos almacenados en caché, incluso si agrega una clave de cliente para los discos de caché. La adición de una clave administrada por el cliente para el cifrado doble proporciona un nivel de seguridad adicional a los clientes con necesidades de alta seguridad. Consulte el artículo Cifrado del lado servidor de Azure Disk Storage para más información.

Use el control de acceso basado en roles en Azure Backup, Azure Key Vault u otros recursos para proteger las copias de seguridad y las claves administradas por el cliente. Además, puede habilitar características de seguridad avanzadas para requerir MFA antes de modificar o eliminar copias de seguridad.

- [Cifrado de host de VM](../virtual-machines/disk-encryption.md#encryption-at-host---end-to-end-encryption-for-your-vm-data)

- [Cifrado del lado servidor de Azure Disk Storage](../virtual-machines/disk-encryption.md)

- [Introducción a las características de seguridad de Azure Backup](../backup/security-overview.md) 

- [Cifrado de datos de copia de seguridad mediante claves administradas por el cliente](../backup/encryption-at-rest-with-cmk.md)  

- [Creación de una copia de seguridad de las claves del almacén de claves en Azure](/powershell/module/az.keyvault/backup-azkeyvaultkey?preserve-view=true&view=azps-5.1.0)

**Supervisión de Azure Security Center**: no disponible actualmente

**Responsabilidad**: Customer

### <a name="br-3-validate-all-backups-including-customer-managed-keys"></a>BR-3: Validación de todas las copias de seguridad, incluidas las claves administradas por el cliente

**Guía**: asegúrese de forma periódica de que puede restaurar las claves administradas por el cliente de las que se hizo una copia de seguridad.

- [Restauración de las claves de Key Vault en Azure](/powershell/module/az.keyvault/restore-azkeyvaultkey?preserve-view=true&view=azps-5.1.0)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="br-4-mitigate-risk-of-lost-keys"></a>BR-4: Mitigación del riesgo de pérdida de claves

**Guía**: Asegúrese de aplicar medidas para evitar la pérdida de claves y para recuperarse de ella. Habilite la eliminación temporal y la protección de purga de Azure Key Vault para proteger las claves frente a una eliminación accidental o malintencionada.

- [Procedimiento para habilitar la eliminación temporal y la protección de purga en Key Vault](../storage/blobs/soft-delete-blob-overview.md?tabs=azure-portal)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

## <a name="governance-and-strategy"></a>Gobernanza y estrategia

*Para más información, consulte [Azure Security Benchmark: gobernanza y estrategia](../security/benchmarks/security-controls-v2-governance-strategy.md).*

### <a name="gs-1-define-asset-management-and-data-protection-strategy"></a>GS-1: Definición de la estrategia de protección de datos y administración de recursos 

**Guía**: Asegúrese de documentar y comunicar una estrategia clara para la protección y supervisión continua de sistemas y datos. Dé prioridad a la detección, evaluación, protección y supervisión de los sistemas y datos críticos para la empresa. 

Esta estrategia debe incluir instrucciones, directivas y estándares documentados para los siguientes elementos: 

-   Norma de clasificación de datos de acuerdo con los riesgos empresariales

-   Visibilidad en la organización de seguridad de los riesgos y el inventario de recursos 

-   Aprobación de la organización de seguridad de los servicios de Azure para su uso 

-   Seguridad de los recursos durante su ciclo de vida

-   Estrategia de control de acceso necesaria según la clasificación de datos de la organización

-   Uso de las funcionalidades de protección de datos nativa de Azure y de terceros

-   Requisitos de cifrado de datos para casos de uso en tránsito y en reposo

-   Normas criptográficas adecuadas

Para más información, consulte las siguientes referencias:
- [Recomendación para la arquitectura de seguridad de Azure: almacenamiento, datos y cifrado](/azure/architecture/framework/security/storage-data-encryption?bc=%2fsecurity%2fcompass%2fbreadcrumb%2ftoc.json&toc=%2fsecurity%2fcompass%2ftoc.json)

- [Aspectos básicos de la seguridad de Azure: seguridad, cifrado y almacenamiento de datos de Azure](../security/fundamentals/encryption-overview.md)

- [Cloud Adoption Framework: procedimientos recomendados de cifrado y seguridad de los datos de Azure](../security/fundamentals/data-encryption-best-practices.md?bc=%2fazure%2fcloud-adoption-framework%2f_bread%2ftoc.json&toc=%2fazure%2fcloud-adoption-framework%2ftoc.json)

- [Azure Security Benchmark: administración de recursos](/azure/security/benchmarks/security-controls-v2-asset-management)

- [Azure Security Benchmark: protección de datos](/azure/security/benchmarks/security-controls-v2-data-protection)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="gs-2-define-enterprise-segmentation-strategy"></a>GS-2: Definición de una estrategia de segmentación empresarial 

**Guía**: Establezca en toda la empresa una estrategia para segmentar el acceso a los recursos mediante una combinación de identidad, red, aplicación, suscripción, grupo de administración y otros controles.

Equilibre concienzudamente la necesidad de separación de seguridad con la necesidad de habilitar el funcionamiento diario de los sistemas que necesitan comunicarse entre sí y acceder a los datos.

Asegúrese de que la estrategia de segmentación se implementa de forma coherente en todos los tipos de control, como la seguridad de red, los modelos de identidad y acceso, y los modelos de acceso y permisos de las aplicaciones, y los controles de los procesos humanos.

- [Guía de la estrategia de segmentación en Azure (vídeo)](/security/compass/microsoft-security-compass-introduction#azure-components-and-reference-model-2151)

- [Guía de la estrategia de segmentación en Azure (documento)](/security/compass/governance#enterprise-segmentation-strategy)

- [Alineación de la segmentación de red con la estrategia de segmentación empresarial](/security/compass/network-security-containment#align-network-segmentation-with-enterprise-segmentation-strategy)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="gs-3-define-security-posture-management-strategy"></a>GS-3: Definición de la estrategia de administración de la posición de seguridad

**Guía**: Mida y mitigue continuamente los riesgos de los recursos individuales y el entorno en el que se hospedan. Dé prioridad a los recursos de gran valor y a las superficies de ataque muy expuestas, como las aplicaciones publicadas, los puntos de entrada y salida de red, los puntos de conexión de usuario y administrador, etc.

- [Azure Security Benchmark: administración de posturas y vulnerabilidades](/azure/security/benchmarks/security-controls-v2-posture-vulnerability-management)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="gs-4-align-organization-roles-responsibilities-and-accountabilities"></a>GS-4: Alineación de los roles y responsabilidades de la organización

**Guía**: Asegúrese de documentar y comunicar una estrategia clara para los roles y las responsabilidades de la organización de seguridad. Dé prioridad a ofrecer una responsabilidad clara en las decisiones de seguridad, a proporcionar a todos los usuarios formación sobre el modelo de responsabilidad compartida y a los equipos técnicos sobre la tecnología para proteger la nube.

- [Procedimiento recomendado de seguridad de Azure 1. Personas: Formación del equipo sobre el recorrido de seguridad de la nube](/azure/cloud-adoption-framework/security/security-top-10#1-people-educate-teams-about-the-cloud-security-journey)

- [Procedimiento recomendado de seguridad de Azure 2. Personas: Formación del equipo en tecnología de seguridad de la nube](/azure/cloud-adoption-framework/security/security-top-10#2-people-educate-teams-on-cloud-security-technology)

- [Procedimiento recomendado de seguridad de Azure 3. Proceso: Asignación de responsabilidades por las decisiones de seguridad en la nube](/azure/cloud-adoption-framework/security/security-top-10#4-process-update-incident-response-ir-processes-for-cloud)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="gs-5-define-network-security-strategy"></a>GS-5: Definición de la estrategia de seguridad de red

**Guía**: Establezca un enfoque de seguridad de red de Azure como parte de la estrategia de control de acceso de seguridad general de su organización.  

Esta estrategia debe incluir instrucciones, directivas y estándares documentados para los siguientes elementos: 

-   Centralización de la responsabilidad de seguridad y la administración de redes

-   Modelo de segmentación de la red virtual alineado con la estrategia de segmentación empresarial

-   Estrategia de corrección en diferentes escenarios de amenazas y ataques

-   Estrategia de entrada y salida y perimetral de Internet

-   Estrategia de interconectividad entre el entorno local y la nube híbrida

-   Artefactos de seguridad de red actualizados (por ejemplo, diagramas de red y arquitectura de red de referencia)

Para más información, consulte las siguientes referencias:
- [Procedimiento recomendado de seguridad de Azure 11. Arquitectura: Establecimiento de una estrategia de seguridad unificada](/azure/cloud-adoption-framework/security/security-top-10#11-architecture-establish-a-single-unified-security-strategy)

- [Azure Security Benchmark: seguridad de red](/azure/security/benchmarks/security-controls-v2-network-security)

- [Azure Network Security Overview (Información general sobre Azure Network Security)](../security/fundamentals/network-overview.md)

- [Estrategia para la arquitectura de red empresarial](/azure/cloud-adoption-framework/ready/enterprise-scale/architecture)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="gs-6-define-identity-and-privileged-access-strategy"></a>GS-6: Definición de la estrategia de acceso con privilegios e identidades

**Guía**: Establezca una identidad de Azure y enfoques de acceso con privilegios como parte de la estrategia de control de acceso de seguridad general de su organización.  

Esta estrategia debe incluir instrucciones, directivas y estándares documentados para los siguientes elementos: 

-   Un sistema de identidad y autenticación centralizado y su interconectividad con otros sistemas de identidad internos y externos

-   Métodos de autenticación sólida en diferentes casos de uso y condiciones

-   Protección de usuarios con privilegios elevados

-   Supervisión y control de anomalías en las actividades de los usuarios  

-   Proceso de revisión y conciliación del acceso y la identidad de los usuarios

Para más información, consulte las siguientes referencias:

- [Azure Security Benchmark: administración de identidades](/azure/security/benchmarks/security-controls-v2-identity-management)

- [Azure Security Benchmark: acceso con privilegios](/azure/security/benchmarks/security-controls-v2-privileged-access)

- [Procedimiento recomendado de seguridad de Azure 11. Arquitectura: Establecimiento de una estrategia de seguridad unificada](/azure/cloud-adoption-framework/security/security-top-10#11-architecture-establish-a-single-unified-security-strategy)

- [Información general sobre seguridad de administración de identidades de Azure](../security/fundamentals/identity-management-overview.md)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="gs-7-define-logging-and-threat-response-strategy"></a>GS-7: Definición de la estrategia de registro y respuesta a amenazas

**Guía**: Establezca una estrategia de registro y respuesta a amenazas para detectar y corregir rápidamente las amenazas mientras cumple los requisitos de cumplimiento. Para dar prioridad, proporcione a los analistas alertas de alta calidad y experiencias sin problemas para que puedan centrarse en las amenazas en lugar de los pasos manuales y de integración. 

Esta estrategia debe incluir instrucciones, directivas y estándares documentados para los siguientes elementos: 

-   Las responsabilidades y el rol de la organización en las operaciones de seguridad (SecOps) 

-   Un proceso de respuesta a incidentes bien definido que esté alineado con NIST u otro marco del sector. 

-   Captura y retención de registros para admitir la detección de amenazas, la respuesta ante incidentes y las necesidades de cumplimiento

-   Visibilidad y correlación centralizada de la información sobre amenazas, mediante SIEM, funcionalidades nativas de Azure y otros orígenes 

-   Plan de comunicación y notificación con sus clientes, proveedores y entidades públicas de interés

-   Uso de plataformas nativas de Azure y de terceros para el tratamiento de incidentes, como el registro y la detección de amenazas, los análisis forenses y la corrección y erradicación de ataques

-   Procesos para controlar incidentes y actividades posteriores a incidentes, como las lecciones aprendidas y la retención de pruebas

Para más información, consulte las siguientes referencias:

- [Azure Security Benchmark: registro y detección de amenazas](/azure/security/benchmarks/security-controls-v2-logging-threat-detection)

- [Azure Security Benchmark: respuesta a incidentes](/azure/security/benchmarks/security-controls-v2-incident-response)

- [Procedimiento recomendado de seguridad de Azure 4. Proceso: Actualización de los procesos de respuesta a incidentes para la nube](/azure/cloud-adoption-framework/security/security-top-10#4-process-update-incident-response-ir-processes-for-cloud)

- [Guía de decisiones sobre registros e informes en Azure Adoption Framework](/azure/cloud-adoption-framework/decision-guides/logging-and-reporting/)

- [Escala, administración y supervisión empresarial de Azure](/azure/cloud-adoption-framework/ready/enterprise-scale/management-and-monitoring)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

## <a name="next-steps"></a>Pasos siguientes

- Consulte la [Información general sobre Azure Security Benchmark V2](../security/benchmarks/overview.md).
- Obtenga más información sobre las [líneas de base de seguridad de Azure](../security/benchmarks/security-baselines-overview.md).
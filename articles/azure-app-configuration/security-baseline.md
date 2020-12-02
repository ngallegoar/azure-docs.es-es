---
title: Línea de base de seguridad de Azure para Azure App Configuration
description: La línea de base de seguridad de Azure App Configuration proporciona una guía de procedimientos y recursos para implementar las recomendaciones de seguridad especificadas en Azure Security Benchmark.
author: msmbaldwin
ms.service: azure-app-configuration
ms.topic: conceptual
ms.date: 11/20/2020
ms.author: mbaldwin
ms.custom: subject-security-benchmark
ms.openlocfilehash: a885f027250a915347fea758c7c7978af0f45f49
ms.sourcegitcommit: ab94795f9b8443eef47abae5bc6848bb9d8d8d01
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/27/2020
ms.locfileid: "96302526"
---
# <a name="azure-security-baseline-for-azure-app-configuration"></a>Línea de base de seguridad de Azure para Azure App Configuration

Esta línea de base de seguridad aplica las instrucciones de [Azure Security Benchmark versión 2.0](../security/benchmarks/overview.md) a Azure App Configuration. Azure Security Benchmark proporciona recomendaciones sobre cómo puede proteger sus soluciones de nube en Azure. El contenido se agrupa por medio de los **controles de seguridad** que define Azure Security Benchmark y las directrices aplicables a Azure App Configuration. Se han excluido los **controles** no aplicables a Azure App Configuration.

Para ver cómo Azure App Configuration se adapta por completo a Azure Security Benchmark, consulte el [archivo completo de asignación de línea de base de seguridad de Azure App Configuration](https://github.com/MicrosoftDocs/SecurityBenchmarks/tree/master/Azure%20Offer%20Security%20Baselines).

## <a name="network-security"></a>Seguridad de redes

*Para más información, consulte [Azure Security Benchmark: seguridad de red](../security/benchmarks/security-controls-v2-network-security.md).*

### <a name="ns-1-implement-security-for-internal-traffic"></a>NS-1: implementación de la seguridad para el tráfico interno

**Guía**: Azure App Configuration no implementa ningún recurso directamente en una red virtual. Dado que el servicio no se implementa en una red virtual, no es posible aprovechar ciertas características de red para proteger el tráfico interno del servicio, como los grupos de seguridad de red, las tablas de enrutamiento u otros dispositivos de red, como una instancia de Azure Firewall. Sin embargo, App Configuration permite usar puntos de conexión privados para conectarse de manera segura a Azure App Configuration desde una red virtual.

Use Azure Sentinel para detectar el uso de protocolos heredados poco seguros como SSL/TLSv1, SMBv1, LM/NTLMv1, wDigest, enlaces LDAP sin firmar y códigos débiles de Kerberos.

- [Uso de puntos de conexión privados en Azure App Configuration](concept-private-endpoint.md)

- [Libro de protocolos poco seguros de Azure Sentinel](../sentinel/quickstart-get-visibility.md#use-built-in-workbooks)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Compartido

### <a name="ns-2-connect-private-networks-together"></a>NS-2: Conexión conjunta de redes privadas

**Guía**: Azure App Configuration admite el uso de puntos de conexión privados para enviar datos de manera segura a través de un vínculo privado. Use Azure ExpressRoute o la red privada virtual (VPN) de Azure para crear conexiones privadas entre centros de datos de Azure y una infraestructura local en un entorno de coubicación. Las conexiones ExpressRoute no usan la Internet pública, y ofrecen más confiabilidad, velocidad, seguridad y una menor latencia que las conexiones a Internet habituales. Para las conexiones de punto a sitio y de sitio a sitio, puede conectar dispositivos o redes locales a una red virtual mediante cualquier combinación de estas opciones de VPN y Azure ExpressRoute.

Para conectar entre sí dos o más redes virtuales en Azure, use el emparejamiento de redes virtuales. El tráfico entre redes virtuales emparejadas es privado y se mantiene en la red troncal de Azure.

- [Qué son los modelos de conectividad de ExpressRoute](../expressroute/expressroute-connectivity-models.md)

- [Información general sobre la red privada virtual de Azure](../vpn-gateway/vpn-gateway-about-vpngateways.md)

- [Interconexión de red virtual](../virtual-network/virtual-network-peering-overview.md)

**Supervisión de Azure Security Center**: Sí

**Responsabilidad**: Customer

### <a name="ns-3-establish-private-network-access-to-azure-services"></a>NS-3: establecimiento del acceso de red privada a los servicios de Azure

**Guía**: Use Azure Private Link para habilitar el acceso privado a Azure App Configuration desde sus redes virtuales sin usar Internet.

El acceso privado es una medida de defensa exhaustiva adicional a la seguridad para la autenticación y el tráfico que ofrecen los servicios de Azure.

- [¿Qué es Azure Private Link?](../private-link/private-link-overview.md)

- [Configuración de un vínculo privado en Azure App Configuration](concept-private-endpoint.md)

**Supervisión de Azure Security Center**: Sí

**Responsabilidad**: Customer

### <a name="ns-4-protect-applications-and-services-from-external-network-attacks"></a>NS-4: protección de las aplicaciones y servicios de ataques de redes externas

**Guía**: Al acceder a los valores de configuración a través de la red virtual, proteja los recursos frente a ataques de redes externas, incluidos los ataques de denegación de servicio distribuido (DDoS), los ataques específicos de la aplicación y el tráfico de Internet no solicitado y potencialmente malintencionado. Use Azure Firewall para proteger las aplicaciones y los servicios contra el tráfico potencialmente malintencionado de Internet y otras ubicaciones externas. Proteja sus recursos de ataques DDoS al habilitar la protección contra DDoS estándar en las redes virtuales de Azure. Use Azure Security Center para detectar riesgos de configuración incorrecta relacionados con los recursos de red.

Azure App Configuration no está diseñado para ejecutar aplicaciones web, proporciona la configuración para estas aplicaciones web. No es necesario que defina ninguna configuración adicional ni que implemente ningún servicio de red adicional para protegerla de ataques de red externos dirigidos a aplicaciones web.

- [Documentación sobre Azure Firewall](../firewall/index.yml)

- [Administración de Azure DDoS Protection estándar mediante Azure Portal](../ddos-protection/manage-ddos-protection.md)

- [Recomendaciones de Azure Security Center](../security-center/recommendations-reference.md#recs-network)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="ns-5-deploy-intrusion-detectionintrusion-prevention-systems-idsips"></a>NS-5: implementación de sistemas de prevención de intrusiones y detección de intrusiones (IDS/IPS)

**Guía**: Use Azure Firewall con el filtrado basado en inteligencia sobre amenazas para alertar o bloquear el tráfico desde y hacia dominios y direcciones IP malintencionados conocidos. La direcciones IP y los dominios proceden de la fuente Inteligencia sobre amenazas de Microsoft. Cuando se requiera la inspección de las cargas, puede implementar una solución IDS/IPS de terceros desde Azure Marketplace con funcionalidades de inspección de cargas. Como alternativa, puede elegir usar IDS/IPS basado en host o una solución de detección y respuesta de punto de conexión (EDR) basada en host junto un IDS/IPS basado en redes, o en lugar de este.

Nota: Si tiene un requisito normativo o de otro tipo para el uso de IDS/IPS, asegúrese de que siempre esté optimizado para proporcionar alertas de alta calidad a la solución de SIEM.

- [Implementación de Azure Firewall](../firewall/tutorial-firewall-deploy-portal.md)

- [Azure Marketplace incluye funcionalidades de IDS de terceros](https://azuremarketplace.microsoft.com/marketplace?search=IDS)

- [Funcionalidad de detección y respuesta de la protección contra amenazas avanzada de Microsoft Defender](/windows/security/threat-protection/microsoft-defender-atp/overview-endpoint-detection-response)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="ns-6-simplify-network-security-rules"></a>NS-6: simplificación de las reglas de seguridad de red

**Guía**: Use etiquetas de servicio de Azure Virtual Network para definir controles de acceso a la red en los grupos de seguridad de red o las instancias de Azure Firewall configuradas para los recursos de App Configuration. Puede usar la etiqueta de servicio "AppConfiguration" en lugar de direcciones IP específicas al crear reglas de seguridad para el tráfico saliente en la red de la aplicación. Al especificar el nombre de la etiqueta de servicio en el campo de origen o destino apropiado de una regla, puede permitir o denegar el tráfico para el servicio correspondiente. Microsoft administra los prefijos de direcciones que la etiqueta de servicio incluye y actualiza automáticamente dicha etiqueta a medida que las direcciones cambian.

- [Descripción y uso de etiquetas de servicio](../virtual-network/service-tags-overview.md)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

## <a name="identity-management"></a>Administración de identidades

*Para más información, consulte [Azure Security Benchmark: Administración de identidades](../security/benchmarks/security-controls-v2-identity-management.md).*

### <a name="im-1-standardize-azure-active-directory-as-the-central-identity-and-authentication-system"></a>IM-1: Unificación en Azure Active Directory como sistema central de identidad y autenticación

**Guía**: Azure App Configuration se integra en Azure Active Directory (Azure AD), que es el servicio predeterminado de administración de identidades y acceso de Azure. Debe unificar Azure AD para controlar la administración de identidades y accesos de la organización en:
- Los recursos de Microsoft Cloud, como Azure Portal, Azure Storage, Azure Virtual Machines (Linux y Windows), Azure Key Vault, PaaS y aplicaciones SaaS.
- Los recursos de su organización, como aplicaciones en Azure o los recursos de la red corporativa.

La protección de Azure AD debe tener máxima prioridad en la práctica de seguridad en la nube de su organización. Azure AD proporciona una puntuación de seguridad de la identidad para ayudarle a evaluar la posición de seguridad de las identidades en relación con los procedimientos recomendados de Microsoft. Use la puntuación para medir el grado de coincidencia de la configuración con los procedimientos recomendados y para hacer mejoras en la posición de seguridad.

Azure proporciona los siguientes roles integrados de Azure para autorizar el acceso a los datos de App Configuration con Azure AD y OAuth:

- Propietario de los datos de App Configuration: use este rol para proporcionar acceso de lectura, escritura o eliminación a los datos de App Configuration. Esto no concede acceso al recurso de App Configuration.

- Lector de los datos de App Configuration: use este rol para proporcionar acceso de lectura a los datos de App Configuration. Esto no concede acceso al recurso de App Configuration.

- Colaborador: use este rol para administrar el recurso de App Configuration. Aunque se puede acceder a los datos de App Configuration mediante las claves de acceso, este rol no concede acceso directo a los datos mediante Azure AD.

- Lector: use este rol para proporcionar acceso de lectura al recurso de App Configuration. Esto no concede acceso a las claves de acceso del recurso ni a los datos almacenados en App Configuration.

Para más información, consulte las siguientes referencias:

- [Procedimiento para crear y configurar una instancia de Azure AD](../active-directory/fundamentals/active-directory-access-create-new-tenant.md)

- [¿Qué es la puntuación de seguridad de la identidad en Azure Active Directory?](../active-directory/fundamentals/identity-secure-score.md)

- [Autorización del acceso a Azure App Configuration con Azure AD](concept-enable-rbac.md)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="im-2-manage-application-identities-securely-and-automatically"></a>IM-2: Administración de identidades de aplicaciones de forma segura y automática

**Guía**: Use identidades administradas por Azure para acceder a Azure App Configuration desde cuentas no humanas, como otros servicios de Azure. Para limitar la necesidad de administrar credenciales adicionales, se recomienda usar la característica de identidades administradas de Azure en lugar de crear una cuenta humana con más permisos para acceder a los recursos o ejecutarlos. Azure App Configuration también puede asignarse una identidad administrada a sí mismo para autenticarse de forma nativa en otros servicios o recursos de Azure que admitan la autenticación de Azure AD. Esto puede ser útil para permitir el acceso sencillo de Azure App Configuration a Azure Key Vault cuando se recuperan los secretos. Al usar identidades administradas, la identidad está administrada por la plataforma de Azure y no requiere que aprovisione ni rote los secretos.

Azure App Configuration permite que se concedan a la aplicación dos tipos de identidades:
- Una identidad asignada por el sistema está asociada al recurso de configuración. Se elimina si se elimina el recurso de configuración. Un recurso de configuración solo puede tener una identidad asignada por el sistema.
- Una identidad asignada por el usuario es un recurso de Azure independiente que se puede asignar al recurso de configuración. Un recurso de configuración puede tener varias identidades asignadas por el usuario.

Cuando no se puedan aprovechar las identidades administradas, cree una entidad de servicio con permisos restringidos en el nivel de recursos de Azure App Configuration. Configure estas entidades de servicio con credenciales de certificado y revierta solo a secretos de cliente. En ambos casos, Azure Key Vault se puede usar junto con las identidades administradas de Azure, de modo que el entorno en tiempo de ejecución (por ejemplo, una función de Azure) pueda recuperar la credencial del almacén de claves.

- [Uso de identidades administradas para Azure App Configuration](overview-managed-identity.md)

- [Identidades administradas de Azure](../active-directory/managed-identities-azure-resources/overview.md)

- [Servicios que admiten identidades administradas para recursos de Azure](../active-directory/managed-identities-azure-resources/services-support-managed-identities.md)

- [Uso de identidades administradas para acceder a App Configuration](howto-integrate-azure-managed-service-identity.md)

- [Entidad de servicio de Azure](/powershell/azure/create-azure-service-principal-azureps) 

- [Creación de una entidad de servicio con certificado](../active-directory/develop/howto-authenticate-service-principal-powershell.md) 

- [Uso de Azure Key Vault para el registro de entidades de seguridad](../key-vault/general/authentication.md#app-identity-and-security-principals)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="im-3-use-azure-ad-single-sign-on-sso-for-application-access"></a>IM-3: Uso del inicio de sesión único de Azure AD para acceder a las aplicaciones

**Guía**: Azure App Configuration usa Azure Active Directory (Azure AD) para proporcionar la opción de administración de identidades y acceso a los recursos de Azure, las aplicaciones en la nube y las aplicaciones locales. Esto incluye no solo las identidades empresariales, como los empleados, sino también las identidades externas, como asociados y proveedores. Azure AD permite el inicio de sesión único (SSO) para administrar el servicio de App Configuration a través de Azure Portal mediante las identidades de Active Directory corporativas sincronizadas. Conecte todos los usuarios, las aplicaciones y los dispositivos a Azure AD para obtener un acceso seguro y sin problemas, y para lograr mayor visibilidad y control.

- [Descripción del inicio de sesión único de aplicaciones con Azure AD](../active-directory/manage-apps/what-is-single-sign-on.md)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="im-4-use-strong-authentication-controls-for-all-azure-active-directory-based-access"></a>IM-4: Uso de controles con autenticación multifactor sólida para todo el acceso basado en Azure Active Directory

**Guía**: Azure App Configuration usa Azure Active Directory, que admite controles de autenticación sólida a través de la autenticación multifactor (MFA) y otros métodos seguros sin contraseña.
- Autenticación multifactor: habilite Azure AD MFA y siga las recomendaciones de administración de identidades y acceso de Azure Security Center para conocer varios procedimientos recomendados en la configuración de la autenticación multifactor. La autenticación multifactor se puede exigir a todos los usuarios, a usuarios concretos o a nivel de cada usuario en función de los factores de riesgo y las condiciones de inicio de sesión.
- Autenticación sin contraseña: hay disponibles tres opciones de autenticación sin contraseña: Windows Hello para empresas, aplicación Microsoft Authenticator y métodos de autenticación locales, como las tarjetas inteligentes.

Para administradores y usuarios con privilegios, asegúrese de que se usa el nivel más alto del método de autenticación sólida, seguido de la implementación de la directiva de autenticación sólida adecuada para otros usuarios.

Nota: MFA se puede aplicar en las cuentas de usuario que tienen acceso a App Configuration y lo administran, pero no en las cuentas de servicio mediante programación. Use la autenticación sin contraseña, como identidades administradas, siempre que sea posible y aplique MFA en las cuentas de usuario.

- [Procedimiento para habilitar la MFA en Azure](../active-directory/authentication/howto-mfa-getstarted.md)

- [Introducción a las opciones de autenticación sin contraseña de Azure Active Directory](../active-directory/authentication/concept-authentication-passwordless.md)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="im-5-monitor-and-alert-on-account-anomalies"></a>IM-5: Supervisión y alerta de anomalías de cuenta

**Guía**: Azure App Configuration se integra en Azure Active Directory para proporcionar los siguientes orígenes de datos:

-   Inicios de sesión: el informe de inicios de sesión proporciona información sobre el uso de aplicaciones administradas y actividades de inicio de sesión de usuario.

-   Registros de auditoría: proporcionan rastreabilidad mediante los registros de todos los cambios realizados mediante diversas características de Azure AD. Entre los ejemplos de registros de auditoría de los cambios registrados destacan agregar o eliminar usuarios, aplicaciones, grupos, roles y directivas.

-   Inicios de sesión de riesgo: un inicio de sesión de riesgo es un indicador de un intento de inicio de sesión que puede haber realizado alguien que no es el propietario legítimo de una cuenta de usuario.

-   Usuarios marcados en riesgo: un usuario en riesgo es un indicador de una cuenta de usuario que puede haber estado en peligro.

Estos orígenes de datos se pueden integrar con Azure Monitor, Azure Sentinel o sistemas SIEM de terceros.

Azure Security Center también puede alertar sobre determinadas actividades sospechosas, como un número excesivo de intentos de autenticación incorrectos y cuentas en desuso en la suscripción. 

Azure Advanced Threat Protection (AATP) es una solución de seguridad que puede usar señales de Active Directory locales para identificar, detectar e investigar amenazas avanzadas, identidades en riesgo y acciones internas malintencionadas.

- [Informes de actividad de auditoría en Azure AD](../active-directory/reports-monitoring/concept-audit-logs.md)

- [Visualización de los inicios de sesión de riesgo de Azure AD](../active-directory/identity-protection/overview-identity-protection.md)

- [Procedimiento para identificar usuarios de Azure AD marcados por una actividad de riesgo](../active-directory/identity-protection/overview-identity-protection.md)

- [Supervisión de la actividad de identidad y acceso de los usuarios en Azure Security Center](../security-center/security-center-identity-access.md)

- [Alertas del módulo de protección de inteligencia sobre amenazas de Azure Security Center](../security-center/alerts-reference.md)

- [Integración de los registros de actividad de Azure en Azure Monitor](../active-directory/reports-monitoring/howto-integrate-activity-logs-with-log-analytics.md)

- [Conectar datos de Azure AD Identity Protection](../sentinel/connect-azure-ad-identity-protection.md)

- [Azure Advanced Threat Protection](/azure-advanced-threat-protection/what-is-atp)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="im-6-restrict-azure-resource-access-based-on-conditions"></a>IM-6: Restricción del acceso a recursos de Azure en función de las condiciones

**Guía**: Azure App Configuration admite el acceso condicional de Azure Active Directory (Azure AD) para un control de acceso más granular basado en condiciones definidas por el usuario, por ejemplo, los inicios de sesión de usuario desde determinados intervalos IP tendrán que usar MFA para iniciar sesión. También se puede usar la directiva de administración de sesión de autenticación granular para distintos casos de uso. Estas directivas de acceso condicional solo se aplicarán a las cuentas de usuario que se autentiquen en Azure AD para acceder al servicio de App Configuration y administrarlo, pero no se aplicarán a las entidades de seguridad de servicio ni a las cadenas de conexión que se conectan al recurso de configuración.

- [Introducción al acceso condicional de Azure](../active-directory/conditional-access/overview.md)

- [Directivas de acceso condicional habituales](../active-directory/conditional-access/concept-conditional-access-policy-common.md)

- [Configuración de la administración de las sesiones de autenticación con el acceso condicional](../active-directory/conditional-access/howto-conditional-access-session-lifetime.md)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="im-7-eliminate-unintended-credential-exposure"></a>IM-7: Elimine la exposición de credenciales no intencionada

**Guía**: Azure App Configuration permite a los clientes almacenar configuraciones que podrían contener identidades o secretos. Se recomienda implementar el escáner de credenciales para identificar las credenciales en las configuraciones. El escáner de credenciales también fomenta el traslado de credenciales detectadas a ubicaciones más seguras, como Azure Key Vault.

Use el servicio Azure App Configuration junto con Azure Key Vault. Almacene las credenciales en Key Vault y, a continuación, vincule estas credenciales mediante la creación de una referencia de Key Vault en el recurso de App Configuration. Cuando App Configuration crea estas referencias, almacena los URI de los valores de Key Vault, en lugar de los propios valores. Las aplicaciones pueden conectarse a App Configuration para recuperar las credenciales de Key Vault.

En GitHub, puede usar la característica de escaneo de secretos nativos para identificar credenciales u otro tipo de secretos en el código.

- [Tutorial: Uso de referencias de Key Vault en una aplicación de ASP.NET Core](use-key-vault-references-dotnet-core.md)

- [Configuración del escáner de credenciales](https://secdevtools.azurewebsites.net/helpcredscan.html)

- [Escaneo de secretos de GitHub](https://docs.github.com/github/administering-a-repository/about-secret-scanning)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

## <a name="privileged-access"></a>Acceso con privilegios

*Para más información, consulte [Azure Security Benchmark: Acceso con privilegios](../security/benchmarks/security-controls-v2-privileged-access.md).*

### <a name="pa-1-protect-and-limit-highly-privileged-users"></a>PA-1: Protección y limitación de usuarios con privilegios elevados

**Guía**: Limite el número de cuentas o roles con privilegios elevados y proteja estas cuentas en un nivel elevado, ya que los usuarios con este privilegio pueden leer directa o indirectamente, y modificar todos los recursos de su entorno de Azure.

Puede habilitar el acceso con privilegios Just-in-Time (JIT) a los recursos de Azure y Azure AD mediante Azure AD Privileged Identity Management (PIM). JIT concede permisos temporales para realizar tareas con privilegios solo cuando los usuarios lo necesitan. PIM también puede generar alertas de seguridad cuando hay actividades sospechosas o no seguras en la organización de Azure AD.

Las claves de acceso tienen privilegios elevados y deben rotarse periódicamente como procedimiento recomendado de seguridad. Las claves de acceso contienen cadenas de conexión, que incluyen información de credenciales y se consideran secretos. Estos secretos deben almacenarse en Azure Key Vault y el código debe autenticarse en Key Vault para recuperarlos. Las claves de acceso pueden proporcionar acceso de lectura y escritura o de solo lectura a una aplicación. Asegúrese de emitir el tipo correcto de clave de acceso para evitar el acceso no autorizado. Para que sea más seguro, use la característica de identidades administradas en Azure AD. Esto solo requiere que las aplicaciones tengan la dirección URL del punto de conexión de configuración para acceder a los valores de configuración.

- [Procedimientos recomendados de App Configuration](howto-best-practices.md#app-configuration-bootstrap)

- [Uso de identidades administradas para acceder a App Configuration](howto-integrate-azure-managed-service-identity.md)
- [Permisos de rol administrativo en Azure AD](../active-directory/roles/permissions-reference.md)

- [Uso de alertas de seguridad de Azure Privileged Identity Management](../active-directory/privileged-identity-management/pim-how-to-configure-security-alerts.md)

- [Protección del acceso con privilegios para las implementaciones híbridas y en la nube en Azure AD](../active-directory/roles/security-planning.md)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="pa-2-restrict-administrative-access-to-business-critical-systems"></a>PA-2: Restricción del acceso administrativo a los sistemas críticos para la empresa

**Guía**: Azure App Configuration usa Azure RBAC para aislar el acceso a los sistemas críticos para la empresa mediante la restricción de las cuentas a las que se concede acceso con privilegios. App Configuration admite Azure RBAC en el nivel de recurso. Para agrupar en silos las configuraciones críticas para la empresa de manera segura, almacene esta información en su propio recurso de App Configuration. Dentro de un recurso, el acceso granular también está disponible a través de las claves o cuentas de acceso de solo lectura, así como los etiquetados.

Para garantizar un control de acceso coherente, todos los tipos de control de acceso se deben alinear con la estrategia de segmentación de la empresa.

- [Acceso al grupo de administración](../governance/management-groups/overview.md#management-group-access)

- [Administradores de la suscripción de Azure](../cost-management-billing/manage/add-change-subscription-administrator.md)

- [Integración de RBAC mediante Azure AD con App Configuration](concept-enable-rbac.md)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="pa-3-review-and-reconcile-user-access-regularly"></a>PA-3: Revisión y conciliación de manera periódica del acceso de los usuarios

**Guía**: Azure App Configuration usa cuentas de Azure Active Directory (Azure AD) para administrar sus recursos, revisar las cuentas de usuario y acceder a la asignación con regularidad para asegurarse de que las cuentas y el acceso sean válidos. 

Azure proporciona los siguientes roles integrados de Azure para autorizar el acceso a los datos de App Configuration con Azure AD y OAuth:

- Propietario de los datos de App Configuration: use este rol para proporcionar acceso de lectura, escritura o eliminación a los datos de App Configuration. Esto no concede acceso al recurso de App Configuration.

- Lector de los datos de App Configuration: use este rol para proporcionar acceso de lectura a los datos de App Configuration. Esto no concede acceso al recurso de App Configuration.

Puede usar las revisiones de acceso de Azure AD para revisar las pertenencias a grupos, el acceso a las aplicaciones empresariales y las asignaciones de roles, como los roles de App Configuration anteriores. Los informes de Azure AD pueden proporcionar registros para ayudar a detectar cuentas obsoletas. También puede usar Azure AD Privileged Identity Management para crear un flujo de trabajo de informes de revisión de acceso para facilitar el proceso de revisión.

Nota: Las identidades administradas se sugieren siempre que sea posible para autenticarse en App Configuration desde otro servicio o aplicación. Tendrá que administrar las entidades de servicio o las cadenas de conexión configuradas con acceso a App Configuration por separado cuando se use.

- [Creación de una revisión de acceso de los roles de recursos de Azure en Privileged Identity Management (PIM)](../active-directory/privileged-identity-management/pim-resource-roles-start-access-review.md) 

- [Procedimiento para usar las revisiones de acceso e identidades de Azure AD](/azure/active-directory/governance/access-reviews-overview)

- [Autorización del acceso a Azure App Configuration con Azure AD](concept-enable-rbac.md)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="pa-4-set-up-emergency-access-in-azure-ad"></a>PA-4: Configuración del acceso de emergencia en Azure AD

**Guía**: Azure App Configuration se integra en Azure Active Directory para administrar sus recursos. Para evitar que se le bloquee por accidente el acceso a la organización de Azure AD, configure una cuenta de acceso de emergencia para cuando no se puedan usar cuentas administrativas normales. Las cuentas de acceso de emergencia tienen normalmente privilegios elevados y no se asignan a usuarios específicos. Las cuentas de acceso de emergencia se limitan a situaciones "excepcionales" o de emergencia en las que no se pueden usar las cuentas administrativas normales.

Debe asegurarse de que las credenciales (como contraseña, certificado o tarjeta inteligente) de las cuentas de acceso de emergencia estén protegidas y solo las conozcan aquellas personas que estén autorizadas a usarlas solo en caso de emergencia.

- [Administración de cuentas de acceso de emergencia en Azure AD](../active-directory/roles/security-emergency-access.md)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="pa-5-automate-entitlement-management"></a>PA-5: Automatización de la administración de derechos 

**Guía**: Azure App Configuration se integra en Azure Active Directory para administrar sus recursos. Use las características de administración de derechos de Azure AD para automatizar los flujos de trabajo de solicitud de acceso, como las asignaciones, las revisiones y la expiración del acceso. También se admite la aprobación en dos o varias fases.

- [¿Qué son las revisiones de acceso de Azure AD?](../active-directory/governance/access-reviews-overview.md)

- [¿Qué es la administración de derechos de Azure AD?](../active-directory/governance/entitlement-management-overview.md)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="pa-6-use-privileged-access-workstations"></a>PA-6: Uso de estaciones de trabajo con privilegios de acceso

**Guía**: Las estaciones de trabajo seguras y aisladas son de una importancia vital para la seguridad de los roles con acceso a información confidencial como administradores, desarrolladores y operadores de servicios críticos. Use estaciones de trabajo de usuario de alta seguridad o Azure Bastion para las tareas administrativas relacionadas con App Configuration. Use Azure Active Directory, Protección contra amenazas avanzada de Microsoft Defender (ATP) o Microsoft Intune para implementar una estación de trabajo de usuario segura y administrada para las tareas administrativas. Las estaciones de trabajo protegidas se pueden administrar de forma centralizada para aplicar una configuración segura, como autenticación sólida, líneas de base de software y hardware y acceso lógico y de red restringido.

- [Descripción de las estaciones de trabajo con privilegios de acceso](../active-directory/devices/concept-azure-managed-workstation.md) 

- [Implementación de una estación de trabajo con privilegios de acceso](../active-directory/devices/howto-azure-managed-workstation.md)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="pa-7-follow-just-enough-administration-least-privilege-principle"></a>PA-7: Seguimiento de solo una administración suficiente (principio de privilegios mínimos) 

**Guía**: Azure App Configuration se integra en el control de acceso basado en roles de Azure (Azure RBAC) para administrar sus recursos. Azure RBAC le permite administrar el acceso a los recursos de Azure mediante las asignaciones de roles. Puede asignar estos roles a usuarios, grupos de entidades de servicio e identidades administradas. Hay roles integrados predefinidos para Azure App Configuration, y estos roles se pueden inventariar o consultar mediante herramientas, como la CLI de Azure, Azure PowerShell o Azure Portal. Los privilegios que asigne a los recursos a través de Azure RBAC siempre se deben limitar a los requisitos de los roles. Este modelo complementa el enfoque Just-in-Time (JIT) de Azure AD Privileged Identity Management (PIM) y se debe revisar periódicamente.

Azure proporciona los siguientes roles integrados de Azure para autorizar el acceso a los datos de App Configuration con Azure AD y OAuth:
- Propietario de los datos de App Configuration: use este rol para proporcionar acceso de lectura, escritura o eliminación a los datos de App Configuration. Esto no concede acceso al recurso de App Configuration.
- Lector de los datos de App Configuration: use este rol para proporcionar acceso de lectura a los datos de App Configuration. Esto no concede acceso al recurso de App Configuration.

Use los roles integrados para asignar los permisos, y cree solo los roles personalizados cuando sea necesario. 

App Configuration admite el almacenamiento de la configuración de varias aplicaciones en un recurso de App Configuration. Para limitar el acceso a la información entre aplicaciones, cree un recurso de App Configuration por aplicación y configure Azure RBAC en consecuencia.

- [¿Qué es el control de acceso basado en rol de Azure (RBAC)?](../role-based-access-control/overview.md)

- [Configuración de RBAC en Azure](../role-based-access-control/role-assignments-portal.md)

- [Procedimiento para usar las revisiones de acceso e identidades de Azure AD](../active-directory/governance/access-reviews-overview.md)

- [Autorización del acceso a Azure App Configuration con Azure AD](concept-enable-rbac.md)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="pa-8-choose-approval-process-for-microsoft-support"></a>PA-8: Selección del proceso de aprobación para el soporte técnico de Microsoft  

**Guía**: Implemente un proceso de aprobación organizacional para admitir escenarios en los que Microsoft pueda necesitar acceso a los datos de App Configuration. Caja de seguridad del cliente no está disponible actualmente para escenarios de compatibilidad con App Configuration.

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

## <a name="data-protection"></a>Protección de datos

*Para más información, consulte [Azure Security Benchmark: protección de datos](../security/benchmarks/security-controls-v2-data-protection.md).*

### <a name="dp-1-discover-classify-and-label-sensitive-data"></a>DP-1: Detección, clasificación y etiquetado de datos confidenciales

**Guía**: Detecte, clasifique y etiquete los datos confidenciales para que se puedan diseñar los controles adecuados que garanticen que los sistemas tecnológicos de la organización almacenen, procesen y transmitan la información confidencial de forma segura. El etiquetado de información confidencial se admite para los recursos de App Configuration, pero no para los valores de configuración que contiene. Una vez que una aplicación tiene acceso de lectura o de lectura y escritura a un almacén de configuración, tiene acceso completo a cualquiera de las configuraciones que estén incluidas en ese almacén.

- [Etiquetado de información confidencial mediante Azure Information Protection](/azure/information-protection/what-is-information-protection)

- [Etiquetado de la clasificación de datos en Azure](/azure/cloud-adoption-framework/govern/policy-compliance/data-classification#tagging-data-classification-in-azure)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="dp-2-protect-sensitive-data"></a>DP-2: Protección de datos confidenciales

**Guía**: En el caso de la plataforma subyacente administrada por Microsoft, Microsoft trata todo el contenido de los clientes como confidencial y protege a los clientes contra la pérdida y exposición de los datos. Para garantizar la seguridad de los datos de los clientes dentro de Azure, Microsoft ha implementado algunos controles y funcionalidades de protección de datos predeterminados. Asegúrese de rotar periódicamente las claves de acceso a los recursos de App Configuration. La información sobre credenciales de las cadenas de conexión puede almacenarse en Azure Key Vault, y el código debe autenticarse en Key Vault para recuperarla. Las claves de acceso pueden proporcionar acceso de lectura y escritura o de solo lectura a una aplicación. Asegúrese de emitir el tipo correcto de clave de acceso para evitar el acceso no autorizado. Para que sea más seguro, use la característica de identidades administradas en Azure AD. Esto solo requiere que las aplicaciones tengan la dirección URL del punto de conexión de configuración para acceder a los valores de configuración.

Restrinja el acceso mediante el control de acceso basado en roles de Azure (Azure RBAC):

- Separe los datos confidenciales en su propio recurso de App Configuration y, a continuación, asigne las directivas de RBAC en consecuencia de modo que solo esté habilitado el acceso autorizado. 

- Use controles de acceso basado en redes.

- Especifique controles en los servicios de Azure (como el cifrado en bases de datos SQL y otras) y garantice el control de acceso coherente, todos los tipos de control de acceso deben estar alineados con la estrategia de segmentación de la empresa.

- La estrategia de segmentación de la empresa también debe estar informada de la ubicación de los datos y sistemas confidenciales o críticos para la empresa.

Para más información, consulte las siguientes referencias:

- [Autorización del acceso a Azure App Configuration con Azure Active Directory](concept-enable-rbac.md)

- [Cifrado de datos de App Configuration](faq.md#does-app-configuration-encrypt-my-data)

- [Control de acceso basado en rol (RBAC) de Azure](../role-based-access-control/overview.md) 

- [Descripción de la protección de datos de los clientes en Azure](../security/fundamentals/protection-customer-data.md)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Compartido

### <a name="dp-4-encrypt-sensitive-information-in-transit"></a>DP-4: Cifrado de la información confidencial en tránsito

**Guía**: Para complementar los controles de acceso, los datos en tránsito deben protegerse frente a ataques de "fuera de banda" mediante cifrado. Esto ayuda a garantizar que los atacantes no puedan leer ni modificar los datos fácilmente.

Azure App Configuration usa el cifrado TLS para todas las solicitudes HTTP. La infraestructura de Azure proporciona una capa agregada de cifrado de nivel de red para todas las solicitudes entre los centros de datos de Azure. Para el tráfico HTTP, asegúrese de que los clientes que se conectan a los recursos de App Configuration puedan negociar TLS v1.2 o superior.

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Compartido

### <a name="dp-5-encrypt-sensitive-data-at-rest"></a>DP-5: Cifrado de datos confidenciales en reposo

**Guía**: Para complementar los controles de acceso, los datos en reposo deben protegerse frente a ataques de "fuera de banda" (por ejemplo, el acceso al almacenamiento subyacente) mediante cifrado. Esto ayuda a garantizar que los atacantes no puedan leer ni modificar los datos fácilmente.

Azure proporciona cifrado de datos en reposo de manera predeterminada. En el caso de datos muy confidenciales, tiene opciones para implementar el cifrado adicional en reposo en todos los recursos de Azure donde estén disponibles. Azure administra las claves de cifrado de manera predeterminada, pero también ofrece la opción de administrar sus propias claves (claves administradas por el cliente) para Azure App Configuration.

- [Uso de claves administradas por el cliente para cifrar los datos de Azure App Configuration](concept-customer-managed-keys.md)

- [Descripción del cifrado en reposo en Azure](../security/fundamentals/encryption-atrest.md#encryption-at-rest-in-microsoft-cloud-services) 

- [Tabla de modelo de cifrado y administración de claves](../security/fundamentals/encryption-atrest.md#azure-resource-providers-encryption-model-support)

- [Cifrado doble de datos en reposo en Azure](../security/fundamentals/double-encryption.md#data-at-rest)

**Supervisión de Azure Security Center**: Sí

**Responsabilidad**: Customer

## <a name="asset-management"></a>Administración de recursos

*Para más información, consulte [Azure Security Benchmark: Administración de recursos](/azure/security/benchmarks/security-controls-v2-asset-management).*

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

**Guía**: Asegúrese de que los equipos de seguridad tienen acceso a un inventario de recursos actualizado continuamente en Azure, como Azure App Configuration. Los equipos de seguridad suelen necesitar este inventario para evaluar la exposición potencial de su organización a los riesgos emergentes y como una entrada para mejoras de seguridad continuas. Cree un grupo de Azure Active Directory para que contenga el equipo de seguridad autorizado de su organización y asígneles acceso de lectura a todos los recursos de Azure App Configuration, lo que se puede simplificar mediante una única asignación de roles de nivel alto dentro de la suscripción.

La característica de inventario de Azure Security Center y Azure Resource Graph pueden consultar y detectar todos los recursos de las suscripciones, incluidos los recursos de red, las aplicaciones y los servicios de Azure.

Aplique etiquetas a los recursos, grupos de recursos y suscripciones de Azure con el fin de organizarlos de manera lógica en una taxonomía. Cada etiqueta consta de un nombre y un par de valores. Por ejemplo, puede aplicar el nombre "Environment" y el valor "Production" a todos los recursos en producción.

- [Creación de consultas con Azure Resource Graph Explorer](../governance/resource-graph/first-query-portal.md)

- [Administración del inventario de recursos de Azure Security Center](../security-center/asset-inventory.md)

- [Para más información sobre el etiquetado de recursos, vea la guía de decisiones de nomenclatura y etiquetado de recursos](/azure/cloud-adoption-framework/decision-guides/resource-tagging/?toc=%2fazure%2fazure-resource-manager%2fmanagement%2ftoc.json).

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="am-3-use-only-approved-azure-services"></a>AM-3: Uso exclusivo de servicios de Azure aprobados

**Guía**: Azure App Configuration admite implementaciones basadas en Azure Resource Manager y el cumplimiento de configuraciones mediante Azure Policy. Use Azure Policy para auditar y restringir qué servicios pueden aprovisionar los usuarios en su entorno. Use Azure Resource Graph para consultar y detectar recursos dentro de sus suscripciones. También puede usar Azure Monitor para crear reglas para desencadenar alertas cuando se detecta un servicio no aprobado.

- [Configuración y administración de Azure Policy](../governance/policy/tutorials/create-and-manage.md)

- [Denegación de un tipo de recurso específico con Azure Policy](../governance/policy/samples/built-in-policies.md#general)

- [Creación de consultas con Azure Resource Graph Explorer](../governance/resource-graph/first-query-portal.md)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="am-4-ensure-security-of-asset-lifecycle-management"></a>AM-4: Garantizar la seguridad de la administración del ciclo de vida de los recursos

**Guía**: Establecer o actualizar las directivas de seguridad que abordan los procesos de administración del ciclo de vida de los recursos para realizar modificaciones de gran impacto. Estas modificaciones incluyen cambios, entre otros: proveedores de identidades y acceso, confidencialidad de datos, configuración de red y asignación de privilegios administrativos.

Quite los recursos de Azure cuando ya no los necesite. Asegúrese de que los administradores roten periódicamente las claves de acceso para asegurar que solo los usuarios autenticados tengan acceso a su recurso de configuración.

- [Rotación de las claves de cifrado usadas para App Configuration](concept-customer-managed-keys.md)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="am-5-limit-users-ability-to-interact-with-azure-resource-manager"></a>AM-5: Limitación de la capacidad de los usuarios para interactuar con Azure Resource Manager

**Instrucciones**: Use el acceso condicional de Azure para limitar la capacidad de los usuarios de interactuar con Azure Resource Manager mediante la configuración de la opción "Bloquear acceso" en la aplicación "Administración de Microsoft Azure".

- [Configuración del acceso condicional para bloquear el acceso a Azure Resource Manager](../role-based-access-control/conditional-access-azure-management.md)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

## <a name="logging-and-threat-detection"></a>registro y detección de amenazas

*Para más información, consulte [Azure Security Benchmark: registro y detección de amenazas](/azure/security/benchmarks/security-controls-v2-logging-threat-detection).*

### <a name="lt-2-enable-threat-detection-for-azure-identity-and-access-management"></a>LT-2: Habilitación de la detección de amenazas para la administración de identidades y acceso de Azure

**Guía**: App Configuration se integra en Azure Active Directory (Azure AD). Esto proporciona los siguientes registros de usuario que se pueden ver en los informes de Azure AD o integrarse en Azure Monitor, Azure Sentinel u otras herramientas de SIEM/supervisión para casos de uso de supervisión y análisis más sofisticados:
- Inicios de sesión: el informe de inicios de sesión proporciona información sobre el uso de aplicaciones administradas y actividades de inicio de sesión de usuario.
- Los registros de auditoría proporcionan rastreabilidad mediante los registros de todos los cambios realizados por diversas características de Azure AD. Algunos ejemplos de registros de auditoría incluyen los cambios realizados en cualquier recurso de Azure AD como agregar o quitar usuarios, aplicaciones, grupos, roles y directivas.
- Inicios de sesión de riesgo: un inicio de sesión de riesgo es un indicador de un intento de inicio de sesión que puede haber realizado alguien que no es el propietario legítimo de una cuenta de usuario.
- Usuarios marcados en riesgo: un usuario en riesgo es un indicador de una cuenta de usuario que puede haber estado en peligro.

Azure Security Center también puede alertar sobre determinadas actividades sospechosas, como un número excesivo de intentos de autenticación incorrectos y cuentas en desuso en la suscripción. Además de la supervisión básica de la higiene de seguridad, el módulo Protección contra amenazas de Azure Security Center también puede recopilar alertas de seguridad más detalladas de las capas de servicios de Azure. Esta funcionalidad le permite ver las anomalías de las cuentas dentro de los recursos individuales.

Otro método para acceder al recurso de configuración de App Configuration es usar claves de acceso. Es necesario rotarlas periódicamente para asegurarse de que ningún agente no autorizado acceda a su recurso de configuración. La rotación se puede realizar directamente en el portal en "Claves de acceso".

- [Uso de una identidad administrada para permitir que Azure App Configuration acceda a otros recursos protegidos de Azure AD](overview-managed-identity.md)

- [Uso de identidades administradas con Azure App Configuration](howto-integrate-azure-managed-service-identity.md)

- [Informes de actividad de auditoría en Azure Active Directory](../active-directory/reports-monitoring/concept-audit-logs.md)

- [Habilitación de Azure Identity Protection](../active-directory/identity-protection/overview-identity-protection.md)

- [Autorización del acceso a Azure App Configuration con Azure AD](concept-enable-rbac.md)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="lt-3-enable-logging-for-azure-network-activities"></a>LT-3: Habilitación del registro para las actividades de red de Azure

**Guía**: Azure App Configuration no implementa ningún recurso directamente en una red virtual. Sin embargo, App Configuration permite usar puntos de conexión privados para conectarse de manera segura a Azure App Configuration desde una red virtual. Además, Azure App Configuration no produce ni procesa registros de consultas de DNS que deban habilitarse.

Habilite el registro en los puntos de conexión privados configurados de App Configuration para capturar:
- datos procesados por el punto de conexión privado (ENTRADA/SALIDA)
- datos procesados por el servicio Private Link (ENTRADA/SALIDA)
- Disponibilidad del puerto NAT

Para más información, consulte las siguientes referencias:

- [Supervisión de Azure Private Link](../private-link/private-link-overview.md#logging-and-monitoring)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="lt-4-enable-logging-for-azure-resources"></a>LT-4: Habilitación del registro para recursos de Azure

**Guía**: Los registros de actividad, que están disponibles automáticamente, contienen todas las operaciones de escritura (PUT, POST, DELETE) de los recursos de App Configuration, excepto las operaciones de lectura (GET). Los registros de actividad se pueden usar para encontrar errores al solucionar problemas o para supervisar cómo un usuario de su organización modificó un recurso. En el caso de App Configuration, los registros de actividad solo están disponibles en el plano de control y se exponen mediante Azure Resource Manager (ARM). Actualmente no se admite el registro del plano de datos orientado al cliente para App Configuration. Los registros de recursos de Azure tampoco están disponibles para configuración.

- [Recopilación de registros y métricas de plataforma con Azure Monitor](../azure-monitor/platform/diagnostic-settings.md)

- [Descripción del registro y de los distintos tipos de registro de Azure](../azure-monitor/platform/platform-logs-overview.md)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="lt-5-centralize-security-log-management-and-analysis"></a>LT-5: Centralizar la administración y el análisis de los registros de seguridad

**Guía**: Centralice el almacenamiento y el análisis de los registros para permitir su correlación. Asegúrese de que, para cada origen de registro, asigna un propietario de datos, una guía de acceso y una ubicación de almacenamiento; que determinar qué herramientas se usan para procesar y acceder a los datos, y los requisitos de retención de datos.

Asegúrese también de que integra los registros de actividad de Azure en el registro central. Recopile registros mediante Azure Monitor para agregar datos de seguridad generados por dispositivos de punto de conexión, recursos de red y otros sistemas de seguridad. En Azure Monitor, use áreas de trabajo de Log Analytics para realizar consultas y análisis, y use cuentas de Azure Storage para el almacenamiento de archivos a largo plazo.

Además, habilite e incorpore los datos a Azure Sentinel o a un sistema SIEM de terceros. Muchas organizaciones optan por usar Azure Sentinel para los datos de acceso frecuente y Azure Storage para los datos inactivos que se usan con menos frecuencia.

- [Recopilación de registros y métricas de plataforma con Azure Monitor](../azure-monitor/platform/diagnostic-settings.md) 

- [Incorporación de Azure Sentinel](../sentinel/quickstart-onboard.md)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="lt-6-configure-log-storage-retention"></a>LT-6: Configuración de la retención del almacenamiento de registros

**Guía**: Asegúrese de que todas las cuentas de almacenamiento o las áreas de trabajo de Log Analytics que se usan para almacenar registros de App Configuration tengan el período de retención de registros configurado de acuerdo a la normativa de cumplimiento de la organización. Use cuentas de área de trabajo de Azure Storage, Data Lake o Log Analytics para el almacenamiento de archivo a largo plazo.

En Azure Monitor, puede establecer el período de retención del área de trabajo de Log Analytics de acuerdo con la normativa de cumplimiento de su organización.

- [Configuración del período de retención del área de trabajo de Log Analytics](../azure-monitor/platform/manage-cost-storage.md)

- [Almacenamiento de registros de recursos en una cuenta de Azure Storage](../azure-monitor/platform/resource-logs.md#send-to-azure-storage)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

## <a name="incident-response"></a>Respuesta a los incidentes

*Para más información, consulte [Azure Security Benchmark: respuesta ante incidentes](../security/benchmarks/security-controls-v2-incident-response.md).*

### <a name="ir-1-preparation--update-incident-response-process-for-azure"></a>IR-1: Preparación: actualización del proceso de respuesta a incidentes para Azure

**Guía**: Asegúrese de que la organización tenga procesos para responder a incidentes de seguridad, que haya actualizado estos procesos para Azure y que los ejercite regularmente para garantizar su disponibilidad.

- [Implementación de la seguridad en todo el entorno empresarial](/azure/cloud-adoption-framework/security/security-top-10#4-process-update-incident-response-ir-processes-for-cloud)

- [Guía de referencia de respuesta a incidentes](/microsoft-365/downloads/IR-Reference-Guide.pdf)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="ir-2-preparation--setup-incident-notification"></a>IR-2: Preparación: notificación de incidentes de configuración

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

Azure Security Center asigna una gravedad a cada alerta para ayudarle a priorizar aquellas que se deben investigar en primer lugar. La gravedad se basa en la confianza que tiene Security Center en la búsqueda o en el análisis utilizados para emitir la alerta, así como en el nivel de confianza de que ha habido un intento malintencionado detrás de la actividad que ha provocado la alerta.

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

### <a name="pv-1-establish-secure-configurations-for-azure-services"></a>PV-1: establecimiento de configuraciones seguras para servicios de Azure 

**Guía**: Azure App Configuration admite las directivas específicas del servicio siguientes que están disponibles en Azure Security Center para auditar y aplicar las configuraciones de los recursos de Azure. Esto puede configurarse en iniciativas de Azure Policy o Azure Security Center.
- App Configuration debe usar una clave administrada por el cliente: El uso de claves administradas por el cliente proporciona una mejor protección de los datos al permitirle administrar sus claves de cifrado. Esto suele ser necesario para satisfacer los requisitos de cumplimiento.
- App Configuration debe usar un vínculo privado: Las conexiones de punto de conexión privado permiten a los clientes de una red virtual acceder de forma segura a Azure App Configuration a través de un vínculo privado.

Puede usar Azure Blueprints para automatizar la implementación y configuración de servicios y entornos de aplicaciones, incluidas las plantillas de Resource Manager, los controles de Azure RBAC y las directivas, en una única definición de plano técnico.

- [Información adicional sobre las directivas de App Configuration](../governance/policy/samples/built-in-policies.md#app-configuration)

- [Uso de directivas de seguridad en Azure Security Center](../security-center/tutorial-security-policy.md)

- [Ilustración de la implementación de límites de protección en la zona de aterrizaje de escala empresarial](/azure/cloud-adoption-framework/ready/enterprise-scale/architecture#landing-zone-expanded-definition)

- [Tutorial: Creación y administración de directivas para aplicar el cumplimiento](../governance/policy/tutorials/create-and-manage.md)

- [Azure Blueprints](../governance/blueprints/overview.md)

**Supervisión de Azure Security Center**: Sí

**Responsabilidad**: Compartido

### <a name="pv-2-sustain-secure-configurations-for-azure-services"></a>PV-2: sostenimiento de configuraciones seguras para servicios de Azure

**Guía**: Use Azure Security Center para supervisar la línea base de configuración y aplicar el cumplimiento mediante Azure Policy. Azure Policy para App Configuration incluye: 
- App Configuration debe usar una clave administrada por el cliente: El uso de claves administradas por el cliente proporciona una mejor protección de los datos al permitirle administrar sus claves de cifrado. Esto suele ser necesario para satisfacer los requisitos de cumplimiento.
- App Configuration debe usar un vínculo privado: Las conexiones de punto de conexión privado permiten a los clientes de una red virtual acceder de forma segura a Azure App Configuration a través de un vínculo privado.

- [Descripción de los efectos de Azure Policy](../governance/policy/concepts/effects.md) 

- [Creación y administración de directivas para aplicar el cumplimiento](../governance/policy/tutorials/create-and-manage.md)

**Supervisión de Azure Security Center**: Sí

**Responsabilidad**: Compartido

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
- [Recomendación para la arquitectura de seguridad de Azure: almacenamiento, datos y cifrado](/azure/architecture/framework/security/storage-data-encryption?amp;bc=%2fsecurity%2fcompass%2fbreadcrumb%2ftoc.json&toc=%2fsecurity%2fcompass%2ftoc.json)

- [Aspectos básicos de la seguridad de Azure: seguridad, cifrado y almacenamiento de datos de Azure](../security/fundamentals/encryption-overview.md)

- [Cloud Adoption Framework: procedimientos recomendados de cifrado y seguridad de los datos de Azure](../security/fundamentals/data-encryption-best-practices.md?amp;bc=%2fazure%2fcloud-adoption-framework%2f_bread%2ftoc.json&toc=%2fazure%2fcloud-adoption-framework%2ftoc.json)

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

**Guía**: Establezca una estrategia de registro y respuesta a amenazas para detectar y corregir rápidamente las amenazas mientras cumple los requisitos de cumplimiento. Dé prioridad a ofrecer a los analistas alertas de alta calidad y experiencias fluidas para que puedan centrarse en las amenazas, en lugar de en la integración y los pasos manuales. 

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
---
title: Línea de base de seguridad de Azure para Azure Lighthouse
description: La línea de base de seguridad de Azure Lighthouse proporciona una guía de procedimientos y recursos para implementar las recomendaciones de seguridad especificadas en Azure Security Benchmark.
author: msmbaldwin
ms.service: lighthouse
ms.topic: conceptual
ms.date: 11/19/2020
ms.author: mbaldwin
ms.custom: subject-security-benchmark
ms.openlocfilehash: 51c786d12c372276d1cb007cc5a929ab6a6302f7
ms.sourcegitcommit: cd9754373576d6767c06baccfd500ae88ea733e4
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/20/2020
ms.locfileid: "94974738"
---
# <a name="azure-security-baseline-for-azure-lighthouse"></a>Línea de base de seguridad de Azure para Azure Lighthouse

Esta línea de base de seguridad aplica las instrucciones de [la versión 2.0 de Azure Security Benchmark](../security/benchmarks/overview.md) a Azure Lighthouse. Azure Security Benchmark proporciona recomendaciones sobre cómo puede proteger sus soluciones de nube en Azure. El contenido se agrupa por medio de los **controles de seguridad** que define Azure Security Benchmark y las directrices aplicables a Azure Lighthouse. Se han excluido los **controles** no aplicables a Azure Lighthouse.

Para ver cómo Azure Lighthouse se adapta por completo a Azure Security Benchmark, consulte el [archivo completo de asignación de línea de base de seguridad de Azure Lighthouse](https://github.com/MicrosoftDocs/SecurityBenchmarks/tree/master/Azure%20Offer%20Security%20Baselines).

## <a name="identity-management"></a>Administración de identidades

*Para más información, consulte [Azure Security Benchmark: Administración de identidades](/azure/security/benchmarks/security-controls-v2-identity-management).*

### <a name="im-1-standardize-azure-active-directory-as-the-central-identity-and-authentication-system"></a>IM-1: Unificación en Azure Active Directory como sistema central de identidad y autenticación

**Guía**: Azure Lighthouse usa Azure Active Directory (Azure AD) como un servicio de administración de identidades y acceso predeterminado de Azure. Debe unificar Azure AD para controlar la administración de identidades y accesos de la organización en:
- Los recursos de Microsoft Cloud, como Azure Portal, Azure Storage, Azure Virtual Machines (Linux y Windows), Azure Key Vault, PaaS y aplicaciones SaaS.
- Los recursos de su organización, como aplicaciones en Azure o los recursos de la red corporativa.

Con Azure Lighthouse, los usuarios designados de un inquilino de administración tienen un rol integrado de Azure que les permite acceder a suscripciones o grupos de recursos delegados en el inquilino de un cliente. Actualmente se admiten todos los roles integrados, excepto el rol de Propietario y los roles integrados con el permiso DataActions. El rol administrador de acceso de usuario solo se admite para uso limitado en la asignación de roles a identidades administradas. No se admiten los roles personalizados ni los roles de administrador de suscripciones clásicas.

- [Inquilinos en Azure Active Directory](../active-directory/develop/single-and-multi-tenant-apps.md) 

- [Procedimiento para crear y configurar una instancia de Azure AD](../active-directory/fundamentals/active-directory-access-create-new-tenant.md) 

- [Uso de proveedores de identidades externos para una aplicación](/azure/active-directory/b2b/identity-providers) 

- [¿Qué es la puntuación de seguridad de la identidad en Azure Active Directory?](../active-directory/fundamentals/identity-secure-score.md)

**Supervisión de Azure Security Center**: Sí

**Responsabilidad**: Customer

### <a name="im-2-manage-application-identities-securely-and-automatically"></a>IM-2: Administración de identidades de aplicaciones de forma segura y automática

**Guía**: Las identidades administradas de Azure le permiten autenticarse en los servicios y recursos de Azure que admiten la autenticación de Azure AD. La autenticación se habilita mediante reglas de concesión de acceso predefinidas que permiten evitar las credenciales codificadas de forma rígida en los archivos de código fuente o de configuración. Con Azure Lighthouse, los usuarios con el rol Administrador de acceso de usuario en la suscripción de un cliente pueden crear una identidad administrada en el inquilino de ese cliente. Aunque por lo general este rol no se admite con Azure Lighthouse, se puede usar en este escenario concreto, lo que permite a los usuarios con este permiso asignar uno o varios roles integrados específicos a identidades administradas.

En el caso de los servicios que no admiten identidades administradas, use Azure AD para crear entidades de servicio con permisos restringidos en el nivel de recurso. Azure Lighthouse permite que las entidades de servicio accedan a los recursos del cliente según los roles que se les conceden durante el proceso de incorporación. Se recomienda configurar entidades de servicio con credenciales de certificado y revertir a secretos de cliente. En ambos casos, Azure Key Vault se puede usar junto con las identidades administradas de Azure, de modo que el entorno en tiempo de ejecución (por ejemplo, una función de Azure) pueda recuperar la credencial del almacén de claves.

- [Identidades administradas de Azure](../active-directory/managed-identities-azure-resources/overview.md)

- [Entidad de servicio de Azure](/powershell/azure/create-azure-service-principal-azureps)

- [Uso de Azure Key Vault para el registro de entidades de seguridad](../key-vault/general/authentication.md#authorize-a-security-principal-to-access-key-vault)

- [Creación de un usuario que pueda asignar roles a una identidad administrada en el inquilino de un cliente](how-to/deploy-policy-remediation.md#create-a-user-who-can-assign-roles-to-a-managed-identity-in-the-customer-tenant)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="im-4-use-strong-authentication-controls-for-all-azure-active-directory-based-access"></a>IM-4: Uso de controles con autenticación multifactor sólida para todo el acceso basado en Azure Active Directory

**Guía**: requiera Multi-Factor Authentication (MFA) a todos los usuarios del inquilino administrador, incluidos los usuarios que tendrán acceso a los recursos delegados de cliente. Se recomienda que pida a los clientes que implementen MFA también en sus inquilinos.

- [Procedimiento para habilitar la MFA en Azure](../active-directory/authentication/howto-mfa-getstarted.md)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="im-5-monitor-and-alert-on-account-anomalies"></a>IM-5: Supervisión y alerta de anomalías de cuenta

**Guía**: Azure AD proporciona los siguientes orígenes de datos: 

-   Inicios de sesión: el informe de inicios de sesión proporciona información sobre el uso de aplicaciones administradas y actividades de inicio de sesión de usuario.

-   Registros de auditoría: proporcionan rastreabilidad mediante los registros de todos los cambios realizados mediante diversas características de Azure AD. Entre los ejemplos de registros de auditoría de los cambios registrados destacan agregar o eliminar usuarios, aplicaciones, grupos, roles y directivas.

-   Inicios de sesión de riesgo: un inicio de sesión de riesgo es un indicador de un intento de inicio de sesión que puede haber realizado alguien que no es el propietario legítimo de una cuenta de usuario.

-   Usuarios marcados en riesgo: un usuario en riesgo es un indicador de una cuenta de usuario que puede haber estado en peligro.

Estos orígenes de datos se pueden integrar con Azure Monitor, Azure Sentinel o sistemas SIEM de terceros.

Los proveedores de servicios que usan Azure Lighthouse pueden reenviar los registros de Azure AD a Azure Sentinel y ver o establecer alertas en los distintos inquilinos para supervisar y alertar sobre anomalías en las cuentas.

- [Informes de actividad de auditoría en Azure AD](../active-directory/reports-monitoring/concept-audit-logs.md)

- [Visualización de los inicios de sesión de riesgo de Azure AD](/azure/active-directory/reports-monitoring/concept-risky-sign-ins)

- [Administración de áreas de trabajo de Azure Sentinel a gran escala](how-to/manage-sentinel-workspaces.md)

- [Conexión de datos de Azure AD a Azure Sentinel](../sentinel/connect-azure-active-directory.md)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="im-6-restrict-azure-resource-access-based-on-conditions"></a>IM-6: Restricción del acceso a recursos de Azure en función de las condiciones

**Guía**: Azure Lighthouse no admite una funcionalidad de acceso condicional para los recursos delegados de cliente. En el inquilino de administración, use el acceso condicional de Azure AD para un control de acceso más granular basado en condiciones definidas por el usuario, como requerir inicios de sesión de usuario desde determinados intervalos IP para usar Multi-Factor Authentication (MFA). También se puede usar una administración de sesión de autenticación granular mediante la directiva de acceso condicional de Azure AD para distintos casos de uso. 

Debe requerir MFA a todos los usuarios del inquilino de administración, incluidos los usuarios que tendrán acceso a los recursos delegados de cliente. Se recomienda que pida a los clientes que implementen MFA también en sus inquilinos.

- [Introducción al acceso condicional de Azure](../active-directory/conditional-access/overview.md)

- [Directivas de acceso condicional habituales](../active-directory/conditional-access/concept-conditional-access-policy-common.md)

- [Frecuencia de inicio de sesión de usuario](../active-directory/conditional-access/howto-conditional-access-session-lifetime.md)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

## <a name="privileged-access"></a>Acceso con privilegios

*Para más información, consulte [Azure Security Benchmark: Acceso con privilegios](/azure/security/benchmarks/security-controls-v2-privileged-access).*

### <a name="pa-1-protect-and-limit-highly-privileged-users"></a>PA-1: Protección y limitación de usuarios con privilegios elevados

**Guía**: Limite el número de cuentas de usuario con privilegios elevados y proteja dichas cuentas en un nivel elevado. No es necesaria una cuenta de Administrador global para habilitar y usar Azure Lighthouse.

Para acceder a los datos del registro de actividad en el nivel del inquilino, se tiene que asignar a una cuenta el rol integrado de Azure Lector de supervisión en el ámbito raíz (/). Como el rol Lector de supervisión en el ámbito raíz se trata de un amplio nivel de acceso, se recomienda que asigne este rol a una cuenta de entidad de servicio en lugar de a un usuario individual o a un grupo. Esta asignación la debe realizar un usuario que tenga el rol Administrador global con privilegios de acceso elevados adicionales. Este acceso con privilegios elevados se debe agregar inmediatamente antes de realizar la asignación de roles y luego se debe quitar al completar la asignación.

- [Permisos de rol administrativo en Azure AD](/azure/active-directory/users-groups-roles/directory-assign-admin-roles)

- [Asignación de acceso para supervisar los datos del registro de actividad en el nivel de inquilino](how-to/monitor-delegation-changes.md)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="pa-2-restrict-administrative-access-to-business-critical-systems"></a>PA-2: Restricción del acceso administrativo a los sistemas críticos para la empresa

**Guía**: Azure Lighthouse usa el control de acceso basado en roles de Azure (RBAC de Azure) para aislar el acceso a los sistemas críticos para la empresa mediante la restricción de las cuentas a las que se concede acceso con privilegios a las suscripciones y los grupos de administración en los que están.

Asegúrese de seguir el principio de privilegios mínimos para que los usuarios solo tengan los permisos necesarios para realizar sus tareas específicas.

Asegúrese de restringir también el acceso a los sistemas de administración, identidad y seguridad que tienen acceso administrativo a los recursos críticos para la empresa, como controladores de dominio (DC) de Active Directory, herramientas de seguridad y herramientas de administración del sistema con agentes instalados en sistemas críticos para la empresa. Los atacantes que ponen en peligro estos sistemas de administración y seguridad pueden convertirlos inmediatamente en un arma para poner en peligro los recursos críticos para la empresa.

Para garantizar un control de acceso coherente, todos los tipos de control de acceso se deben alinear con la estrategia de segmentación de la empresa.

- [Modelo de referencia y componentes de Azure](/security/compass/microsoft-security-compass-introduction#azure-components-and-reference-model-2151)

- [Acceso al grupo de administración](../governance/management-groups/overview.md#management-group-access)

- [Administradores de la suscripción de Azure](../cost-management-billing/manage/add-change-subscription-administrator.md)

- [Procedimientos recomendados para definir usuarios y roles para Azure Lighthouse](concepts/tenants-users-roles.md#best-practices-for-defining-users-and-roles)

**Supervisión de Azure Security Center**: Sí

**Responsabilidad**: Customer

### <a name="pa-3-review-and-reconcile-user-access-regularly"></a>PA-3: Revisión y conciliación de manera periódica del acceso de los usuarios

**Guía**: Azure Lighthouse usa cuentas de Azure Active Directory (Azure AD) para administrar sus recursos, revisar las cuentas de usuario y obtener acceso a la asignación con regularidad para asegurarse de que las cuentas y el acceso sean válidos. Puede usar las revisiones de acceso de Azure AD para revisar las pertenencias a grupos, el acceso a las aplicaciones empresariales y las asignaciones de roles. Los informes de Azure AD pueden proporcionar registros para ayudar a detectar cuentas obsoletas. También puede usar Azure AD Privileged Identity Management para crear un flujo de trabajo de informes de revisión de acceso para facilitar el proceso de revisión.

Los clientes pueden revisar el nivel de acceso que se concede a los usuarios en el inquilino de administración a través de Azure Lighthouse en Azure Portal. Pueden quitar este acceso en cualquier momento.

Además, se puede configurar Azure Privileged Identity Management para enviar una alerta cuando se cree un número de cuentas de administrador excesivo y para identificar las cuentas de administrador que hayan quedado obsoletas o que no estén configuradas correctamente.

Tenga en cuenta que algunos servicios de Azure admiten usuarios y roles locales que no se administran mediante Azure AD. Estos usuarios deberán administrarse por separado.

- [Creación de una revisión de acceso de los roles de recursos de Azure en Privileged Identity Management (PIM)](../active-directory/privileged-identity-management/pim-resource-roles-start-access-review.md) 

- [Eliminación del acceso a una delegación](how-to/remove-delegation.md)

- [Procedimiento para usar las revisiones de acceso e identidades de Azure AD](../active-directory/governance/access-reviews-overview.md)

- [Visualización de la página Proveedores de servicios en Azure Portal](how-to/view-manage-service-providers.md)

**Supervisión de Azure Security Center**: Sí

**Responsabilidad**: Customer

### <a name="pa-4-set-up-emergency-access-in-azure-ad"></a>PA-4: Configuración del acceso de emergencia en Azure AD

**Guía**: Azure Lighthouse se integra en Azure Active Directory para administrar sus recursos. Para evitar que se le bloquee por accidente el acceso a la organización de Azure AD, configure una cuenta de acceso de emergencia para cuando no se puedan usar cuentas administrativas normales. Las cuentas de acceso de emergencia tienen normalmente privilegios elevados y no se asignan a usuarios específicos. Las cuentas de acceso de emergencia se limitan a situaciones "excepcionales" o de emergencia en las que no se pueden usar las cuentas administrativas normales.

Debe asegurarse de que las credenciales (como contraseña, certificado o tarjeta inteligente) de las cuentas de acceso de emergencia estén protegidas y solo las conozcan aquellas personas que estén autorizadas a usarlas solo en caso de emergencia.

- [Administración de cuentas de acceso de emergencia en Azure AD](/azure/active-directory/users-groups-roles/directory-emergency-access)

**Supervisión de Azure Security Center**: Sí

**Responsabilidad**: Customer

### <a name="pa-5-automate-entitlement-management"></a>PA-5: Automatización de la administración de derechos 

**Guía**: Azure Lighthouse se integra en Azure Active Directory (Azure AD) para administrar sus recursos. Use las características de administración de derechos de Azure AD para automatizar los flujos de trabajo de solicitud de acceso, como las asignaciones, las revisiones y la expiración del acceso. También se admite la aprobación en dos o varias fases.

- [¿Qué son las revisiones de acceso de Azure AD?](../active-directory/governance/access-reviews-overview.md) 

- [¿Qué es la administración de derechos de Azure AD?](../active-directory/governance/entitlement-management-overview.md)

**Supervisión de Azure Security Center**: no disponible actualmente

**Responsabilidad**: Customer

### <a name="pa-6-use-privileged-access-workstations"></a>PA-6: Uso de estaciones de trabajo con privilegios de acceso

**Guía**: Las estaciones de trabajo seguras y aisladas son de una importancia vital para la seguridad de los roles con acceso a información confidencial como administradores, desarrolladores y operadores de servicios críticos. En función de sus requisitos, puede usar estaciones de trabajo de usuario de alta seguridad o Azure Bastion para realizar tareas de administración administrativas con Azure Lighthouse en entornos de producción. Use Azure Active Directory, Protección contra amenazas avanzada de Microsoft Defender (ATP) o Microsoft Intune para implementar una estación de trabajo de usuario segura y administrada para las tareas administrativas. Las estaciones de trabajo protegidas se pueden administrar de forma centralizada para aplicar una configuración segura, como autenticación sólida, líneas de base de software y hardware y acceso lógico y de red restringido. 

- [Descripción de las estaciones de trabajo con privilegios de acceso](../active-directory/devices/concept-azure-managed-workstation.md)

- [Implementación de una estación de trabajo con privilegios de acceso](../active-directory/devices/howto-azure-managed-workstation.md)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="pa-7-follow-just-enough-administration-least-privilege-principle"></a>PA-7: Seguimiento de solo una administración suficiente (principio de privilegios mínimos) 

**Guía**: Azure Lighthouse se integra en el control de acceso basado en roles de Azure (RBAC de Azure) para administrar sus recursos. Azure RBAC le permite administrar el acceso a los recursos de Azure mediante las asignaciones de roles. Puede asignar estos roles integrados a usuarios, grupos, entidades de servicio e identidades administradas. Hay roles integrados predefinidos para determinados recursos, y estos roles se pueden inventariar o consultar mediante herramientas como la CLI de Azure, Azure PowerShell o el Azure Portal. Los privilegios que asigne a los recursos a través de Azure RBAC siempre se deben limitar a los requisitos de los roles. Este modelo complementa el enfoque Just-in-Time (JIT) de Azure AD Privileged Identity Management (PIM) y se debe revisar periódicamente. Use los roles integrados para asignar los permisos, y cree solo los roles personalizados cuando sea necesario.

Azure Lighthouse permite el acceso a los recursos delegados de cliente mediante los roles integrados de Azure. En la mayoría de los casos, querrá asignar estos roles a un grupo o a una entidad de servicio, en lugar de a muchas cuentas de usuario individuales. Esto le permite agregar o quitar el acceso de usuarios individuales sin tener que actualizar y volver a publicar el plan cuando cambien los requisitos de acceso.

Para delegar los recursos de cliente a un inquilino de administración, se debe hacer una implementación desde una cuenta que no sea de invitado en el inquilino del cliente que tenga el rol integrado Propietario para la suscripción que se va a incorporar (o que contiene los grupos de recursos que se están incorporando).

- [Descripción de inquilinos, usuarios y roles en Azure Lighthouse](concepts/tenants-users-roles.md)

- [Aplicación del principio de privilegios mínimos en Azure Lighthouse](concepts/recommended-security-practices.md#assign-permissions-to-groups-using-the-principle-of-least-privilege)

- [¿Qué es el control de acceso basado en rol de Azure (RBAC)?](../role-based-access-control/overview.md) 

- [Procedimiento para usar las revisiones de acceso e identidades de Azure AD](../active-directory/governance/access-reviews-overview.md)

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

**Guía**: los equipos de seguridad de los clientes pueden revisar los registros de actividad para ver las actividades realizadas por los proveedores de servicios que usan Azure Lighthouse. 

Si un proveedor de servicios quiere permitir que su equipo de seguridad revise los recursos delegados de cliente, las autorizaciones del equipo de seguridad deben incluir el rol integrado Lector.

- [Cómo un cliente puede revisar la actividad del proveedor de servicios](how-to/view-service-provider-activity.md)

- [Cómo se especifican los roles al incorporar un cliente a Azure Lighthouse](how-to/onboard-customer.md#define-roles-and-permissions)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="am-3-use-only-approved-azure-services"></a>AM-3: Uso exclusivo de servicios de Azure aprobados

**Guía**: Use Azure Policy para auditar y restringir qué servicios pueden aprovisionar los usuarios en su entorno. Use Azure Resource Graph para consultar y detectar recursos dentro de sus suscripciones. También puede usar Azure Monitor para crear reglas para desencadenar alertas cuando se detecta un servicio no aprobado.

- [Configuración y administración de Azure Policy](../governance/policy/tutorials/create-and-manage.md) 

- [Denegación de un tipo de recurso específico con Azure Policy](../governance/policy/samples/built-in-policies.md#general) 

- [Creación de consultas con Azure Resource Graph Explorer](../governance/resource-graph/first-query-portal.md)

**Supervisión de Azure Security Center**: Sí

**Responsabilidad**: Customer

### <a name="am-4-ensure-security-of-asset-lifecycle-management"></a>AM-4: Garantizar la seguridad de la administración del ciclo de vida de los recursos

**Guía**: quite el acceso a los recursos que se han delegado a través de Azure Lighthouse cuando ya no sean necesarios, de modo que los proveedores de servicios dejen de tener acceso. El acceso lo puede quitar el cliente o el proveedor de servicios. 

- [Quitar el acceso a una delegación](how-to/remove-delegation.md)

**Supervisión de Azure Security Center**: no disponible actualmente

**Responsabilidad**: Customer

### <a name="am-5-limit-users-ability-to-interact-with-azure-resource-manager"></a>AM-5: Limitación de la capacidad de los usuarios para interactuar con Azure Resource Manager

**Guía**: Azure Lighthouse se integra con Azure Active Directory (Azure AD) para proporcionar funcionalidades de identidad y autenticación. Puede usar el acceso condicional de Azure para limitar la capacidad de los usuarios de interactuar con Azure Resource Manager mediante la configuración de la opción "Bloquear acceso" en la aplicación "Administración de Microsoft Azure".

- [Configuración del acceso condicional para bloquear el acceso a Azure Resource Manager](../role-based-access-control/conditional-access-azure-management.md)

**Supervisión de Azure Security Center**: Sí

**Responsabilidad**: Customer

## <a name="logging-and-threat-detection"></a>registro y detección de amenazas

*Para más información, consulte [Azure Security Benchmark: registro y detección de amenazas](/azure/security/benchmarks/security-controls-v2-logging-threat-protection).*

### <a name="lt-1-enable-threat-detection-for-azure-resources"></a>LT-1: Habilitación de la detección de amenazas para recursos de Azure

**Guía**: a través de Azure Lighthouse, puede supervisar los recursos de Azure de sus clientes para detectar posibles amenazas y anomalías. Céntrese en obtener alertas de alta calidad para reducir los falsos positivos que deben revisar los analistas. Las alertas se pueden crear a partir de datos de registro, agentes u otros datos.

Use la funcionalidad integrada de detección de amenazas de Azure Security Center, que se basa en la supervisión de los datos de telemetría de los servicios de Azure y en el análisis de los registros de servicio. Los datos se recopilan con el agente de Log Analytics, que lee distintas configuraciones relacionadas con la seguridad y diversos registros de eventos del sistema y copia los datos en el área de trabajo para analizarlos. 

Además, use Azure Sentinel para compilar reglas de análisis, que capturan las amenazas que cumplen criterios específicos en el entorno del cliente. Las reglas generan incidentes cuando los criterios coinciden, por lo que puede investigar cada incidente. Azure Sentinel también puede importar inteligencia contra amenazas de terceros para mejorar su capacidad de detección de amenazas. 

- [Protección contra amenazas en Azure Security Center](/azure/security-center/threat-protection)

- [Guía de referencia de alertas de seguridad de Azure Security Center](../security-center/alerts-reference.md)

- [Creación de reglas de análisis personalizadas para detectar amenazas](../sentinel/tutorial-detect-threats-custom.md)

- [Administración de áreas de trabajo de Azure Sentinel a gran escala](how-to/manage-sentinel-workspaces.md)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="lt-2-enable-threat-detection-for-azure-identity-and-access-management"></a>LT-2: Habilitación de la detección de amenazas para la administración de identidades y acceso de Azure

**Guía**: a través de Azure Lighthouse, puede usar Azure Security Center para alertar sobre determinadas actividades sospechosas en los inquilinos del cliente que administra, como un número excesivo de intentos de autenticación incorrectos y cuentas en desuso en la suscripción.

Azure Active Directory (Azure AD) proporciona los siguientes registros de usuario que se pueden ver en los informes de Azure AD o integrados en Azure Monitor, Azure Sentinel, SIEM o herramientas de supervisión para casos de uso de supervisión y análisis más sofisticados:
- Inicio de sesión: el informe de inicio de sesión proporciona información sobre el uso de aplicaciones administradas y actividades de inicio de sesión del usuario.
- Los registros de auditoría proporcionan rastreabilidad mediante los registros de todos los cambios realizados por diversas características de Azure AD. Algunos ejemplos de registros de auditoría incluyen los cambios realizados en cualquier recurso de Azure AD como agregar o quitar usuarios, aplicaciones, grupos, roles y directivas.
- Inicios de sesión de riesgo: un inicio de sesión de riesgo es un indicador de un intento de inicio de sesión que puede haber realizado alguien que no es el propietario legítimo de una cuenta de usuario.
- Usuarios marcados en riesgo: un usuario en riesgo es un indicador de una cuenta de usuario que puede haber estado en peligro.

Azure Security Center también puede alertar sobre determinadas actividades sospechosas, como un número excesivo de intentos de autenticación incorrectos y cuentas en desuso en la suscripción. Además de la supervisión básica de la protección de seguridad, el módulo de protección contra amenazas de Azure Security Center también puede recopilar alertas de seguridad más detalladas de recursos individuales de proceso de Azure (máquinas virtuales, contenedores, App Service), recursos de datos (base de datos SQL y almacenamiento) y niveles de servicio de Azure. Esta funcionalidad le permite ver las anomalías de las cuentas dentro de los recursos individuales. 

- [Servicios y escenarios mejorados con Azure Lighthouse](concepts/cross-tenant-management-experience.md#enhanced-services-and-scenarios)

- [Informes de actividad de auditoría en Azure Active Directory](../active-directory/reports-monitoring/concept-audit-logs.md) 

- [Habilitación de Azure Identity Protection](../active-directory/identity-protection/overview-identity-protection.md) 

- [Protección contra amenazas en Azure Security Center](/azure/security-center/threat-protection)

**Supervisión de Azure Security Center**: Sí

**Responsabilidad**: Customer

### <a name="lt-4-enable-logging-for-azure-resources"></a>LT-4: Habilitación del registro para recursos de Azure

**Guía**: los registros de actividad, que están disponibles automáticamente, contienen todas las operaciones de escritura (PUT, POST, DELETE) de los recursos de Azure Lighthouse, excepto las operaciones de lectura (GET). Los registros de actividad se pueden usar para encontrar errores al solucionar problemas o para supervisar cómo un usuario de su organización modificó un recurso.

Con Azure Lighthouse, puede usar los registros de Azure Monitor de forma escalable en los inquilinos del cliente que está administrando. Cree áreas de trabajo de Log Analytics directamente en los inquilinos del cliente para que los datos del cliente permanezcan en sus inquilinos en lugar de exportarse a los suyos. Esto también permite la supervisión centralizada de los recursos o servicios que admite Log Analytics, lo que le proporciona más flexibilidad respecto a los tipos de datos que supervisa.

Los clientes que tengan suscripciones delegadas para Azure Lighthouse pueden visualizar los datos del registro de actividad de Azure para ver todas las acciones realizadas. Esto proporciona a los clientes una visibilidad completa de las operaciones que los proveedores de servicios realizan, junto con las operaciones hechas por los usuarios en el propio inquilino de Azure Active Directory (Azure AD) del cliente.

- [Recopilación de registros y métricas de plataforma con Azure Monitor](../azure-monitor/platform/diagnostic-settings.md) 

- [Descripción del registro y de los distintos tipos de registro de Azure](../azure-monitor/platform/platform-logs-overview.md)

- [Supervisión de los recursos delegados a escala](how-to/monitor-at-scale.md)

- [Visualización de la actividad del proveedor de servicios](how-to/view-service-provider-activity.md)

**Supervisión de Azure Security Center**: Sí

**Responsabilidad**: Compartido

### <a name="lt-5-centralize-security-log-management-and-analysis"></a>LT-5: Centralizar la administración y el análisis de los registros de seguridad

**Guía**: Centralice el almacenamiento y el análisis de los registros para permitir su correlación. Asegúrese de que, para cada origen de registro, asigna un propietario de datos, una guía de acceso y una ubicación de almacenamiento; que determinar qué herramientas se usan para procesar y acceder a los datos, y los requisitos de retención de datos.

Asegúrese también de que integra los registros de actividad de Azure en el registro central. Recopile registros mediante Azure Monitor para agregar datos de seguridad generados por dispositivos de punto de conexión, recursos de red y otros sistemas de seguridad. En Azure Monitor, use áreas de trabajo de Log Analytics para realizar consultas y análisis, y use cuentas de Azure Storage para el almacenamiento de archivos a largo plazo.

Además, habilite e incorpore los datos a Azure Sentinel o a un sistema SIEM de terceros.

Con Azure Lighthouse, puede usar los registros de Azure Monitor de forma escalable en los inquilinos del cliente que está administrando. Cree áreas de trabajo de Log Analytics directamente en los inquilinos del cliente para que los datos del cliente permanezcan en sus inquilinos en lugar de exportarse a los suyos. Esto también permite la supervisión centralizada de los recursos o servicios que admite Log Analytics, lo que le proporciona más flexibilidad respecto a los tipos de datos que supervisa.

Los clientes que tengan suscripciones delegadas para Azure Lighthouse pueden visualizar los datos del registro de actividad de Azure para ver todas las acciones realizadas. Esto proporciona a los clientes una visibilidad completa de las operaciones que los proveedores de servicios realizan, junto con las operaciones hechas por los usuarios en el propio inquilino de Azure Active Directory (Azure AD) del cliente.

Muchas organizaciones optan por usar Azure Sentinel para los datos de acceso frecuente y Azure Storage para los datos inactivos que se usan con menos frecuencia.

- [Recopilación de registros y métricas de plataforma con Azure Monitor](../azure-monitor/platform/diagnostic-settings.md)

- [Supervisión de los recursos delegados a escala](how-to/monitor-at-scale.md)

- [Cómo los clientes pueden ver la actividad del proveedor de servicios](how-to/view-service-provider-activity.md)

- [Administración de áreas de trabajo de Azure Sentinel a gran escala](how-to/manage-sentinel-workspaces.md)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="lt-6-configure-log-storage-retention"></a>LT-6: Configuración de la retención del almacenamiento de registros

**Guía**: Azure Lighthouse actualmente no genera ningún registro relacionado con la seguridad. Los clientes que quieran ver la actividad del proveedor de servicios pueden configurar la retención de registros según los requisitos empresariales, de cumplimiento y de regulación. 

En Azure Monitor, puede establecer el período de retención del área de trabajo de Log Analytics de acuerdo con la normativa de cumplimiento de su organización. Use cuentas de área de trabajo de Azure Storage, Data Lake o Log Analytics para el almacenamiento de archivo a largo plazo.

- [Cambio del período de retención de datos en Log Analytics](../azure-monitor/platform/manage-cost-storage.md#change-the-data-retention-period)

- [Configuración de la directiva de retención para los registros de la cuenta de Azure Storage](../storage/common/storage-monitor-storage-account.md#configure-logging)

- [Exportación de alertas y recomendaciones de Azure Security Center](../security-center/continuous-export.md): configure esta opción y el cliente no podrá establecer la retención de registros.

- [Cómo un cliente puede revisar los datos del registro de actividad de los proveedores de servicios](how-to/view-service-provider-activity.md)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

## <a name="incident-response"></a>Respuesta a los incidentes

*Para más información, consulte [Azure Security Benchmark: respuesta ante incidentes](/azure/security/benchmarks/security-controls-v2-incident-response).*

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

**Guía**: asegúrese de que cuenta con un proceso para crear alertas de alta calidad y medir la calidad de las alertas. Esto le permite aprender de incidentes anteriores y clasificar las alertas para los analistas, de modo que no pierdan tiempo con falsos positivos. 

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

Azure Security Center asigna una gravedad a cada alerta para ayudarle a priorizar aquellas que se deben investigar en primer lugar. La gravedad se basa en la confianza que tiene Security Center en el hallazgo o en el análisis utilizados para emitir la alerta, así como en el nivel de confianza de que ha habido un intento malintencionado detrás de la actividad que ha provocado la alerta.

Adicionalmente, marque los recursos con etiquetas y cree un sistema de nomenclatura para identificar y clasificar los recursos de Azure, especialmente los que procesan datos confidenciales.  Es su responsabilidad asignar prioridades a la corrección de las alertas en función de la importancia de los recursos y el entorno de Azure donde se produjo el incidente.

- [Alertas de seguridad en el Centro de seguridad de Azure](../security-center/security-center-alerts-overview.md)

- [Uso de etiquetas para organizar los recursos de Azure](/azure/azure-resource-manager/resource-group-using-tags)

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

*Para más información, consulte [Azure Security Benchmark: administración de posturas y vulnerabilidades](/azure/security/benchmarks/security-controls-v2-vulnerability-management).*

### <a name="pv-1-establish-secure-configurations-for-azure-services"></a>PV-1: establecimiento de configuraciones seguras para servicios de Azure 

**Guía**: Azure Lighthouse admite las directivas específicas del servicio siguientes que están disponibles en Azure Security Center para auditar y aplicar las configuraciones de los recursos de Azure. Esto puede configurarse en iniciativas de Azure Policy o Azure Security Center.

- Permitir administrar los identificadores de inquilino que se incorporarán mediante Azure Lighthouse.

- Auditar la delegación de ámbitos a un inquilino de administración.

Puede usar Azure Blueprints para automatizar la implementación y configuración de servicios y entornos de aplicaciones, incluidas las plantillas de Resource Manager, los controles de RBAC de Azure y las directivas, en una única definición de plano técnico.

- [Directivas de Azure Lighthouse](samples/policy-reference.md)

- [Tutorial: Creación y administración de directivas para aplicar el cumplimiento](../governance/policy/tutorials/create-and-manage.md) 

- [Azure Blueprints](../governance/blueprints/overview.md)

**Supervisión de Azure Security Center**: Sí

**Responsabilidad**: Customer

### <a name="pv-2-sustain-secure-configurations-for-azure-services"></a>PV-2: sostenimiento de configuraciones seguras para servicios de Azure

**Guía**: Azure Lighthouse admite las directivas específicas del servicio siguientes que están disponibles en Azure Security Center para auditar y aplicar las configuraciones de los recursos de Azure. Esto puede configurarse en iniciativas de Azure Policy o Azure Security Center.

- [Directivas de Azure Lighthouse](samples/policy-reference.md)

**Supervisión de Azure Security Center**: Sí

**Responsabilidad**: Customer

### <a name="pv-3-establish-secure-configurations-for-compute-resources"></a>PV-3: establecimiento de configuraciones seguras para los recursos de proceso

**Guía**: use Azure Security Center y Azure Policy para establecer configuraciones seguras en todos los recursos de proceso, incluidas las VM, los contenedores y otros elementos.

- [Cómo supervisar las recomendaciones de Azure Security Center](../security-center/security-center-recommendations.md) 

- [Guía de referencia sobre las recomendaciones de seguridad](../security-center/recommendations-reference.md)

**Supervisión de Azure Security Center**: Sí

**Responsabilidad**: Customer

### <a name="pv-8-conduct-regular-attack-simulation"></a>PV-8: realización de una simulaciones de ataques periódicas

**Guía**: Según sea necesario, realice pruebas de penetración o actividades de equipo rojo en los recursos de Azure y asegúrese de corregir todos los resultados de seguridad críticos.
siga las reglas de compromiso de la prueba de penetración de Microsoft Cloud para asegurarse de que las pruebas de penetración no infrinjan las directivas de Microsoft. Use la estrategia de Microsoft y la ejecución de las pruebas de penetración del equipo rojo y sitios activos en la infraestructura de nube, los servicios y las aplicaciones administradas por Microsoft.

- [Pruebas de penetración en Azure](../security/fundamentals/pen-testing.md)

- [Reglas de interacción de las pruebas de penetración](https://www.microsoft.com/msrc/pentest-rules-of-engagement?rtc=1) 

- [Equipo rojo de Microsoft Cloud](https://gallery.technet.microsoft.com/Cloud-Red-Teaming-b837392e)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Compartido

## <a name="governance-and-strategy"></a>Gobernanza y estrategia

*Para más información, consulte [Azure Security Benchmark: gobernanza y estrategia](/azure/security/benchmarks/security-controls-v2-governance-strategy).*

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
- [Recomendación para la arquitectura de seguridad de Azure: almacenamiento, datos y cifrado](https://docs.microsoft.com/azure/architecture/framework/security/storage-data-encryption?toc=/security/compass/toc.json&amp;bc=/security/compass/breadcrumb/toc.json)

- [Aspectos básicos de la seguridad de Azure: seguridad, cifrado y almacenamiento de datos de Azure](../security/fundamentals/encryption-overview.md)

- [Cloud Adoption Framework: procedimientos recomendados de cifrado y seguridad de los datos de Azure](https://docs.microsoft.com/azure/security/fundamentals/data-encryption-best-practices?toc=/azure/cloud-adoption-framework/toc.json&amp;bc=/azure/cloud-adoption-framework/_bread/toc.json)

- [Azure Security Benchmark: administración de recursos](/azure/security/benchmarks/security-benchmark-v2-asset-management)

- [Azure Security Benchmark: protección de datos](/azure/security/benchmarks/security-benchmark-v2-data-protection)

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

- [Azure Security Benchmark: administración de posturas y vulnerabilidades](/azure/security/benchmarks/security-benchmark-v2-posture-vulnerability-management)

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

- [Azure Security Benchmark: seguridad de red](/azure/security/benchmarks/security-benchmark-v2-network-security)

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

- [Azure Security Benchmark: administración de identidades](/azure/security/benchmarks/security-benchmark-v2-identity-management)

- [Azure Security Benchmark: acceso con privilegios](/azure/security/benchmarks/security-benchmark-v2-privileged-access)

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

- [Azure Security Benchmark: registro y detección de amenazas](/azure/security/benchmarks/security-benchmark-v2-logging-threat-detection)

- [Azure Security Benchmark: respuesta a incidentes](/azure/security/benchmarks/security-benchmark-v2-incident-response)

- [Procedimiento recomendado de seguridad de Azure 4. Proceso: Actualización de los procesos de respuesta a incidentes para la nube](/azure/cloud-adoption-framework/security/security-top-10#4-process-update-incident-response-ir-processes-for-cloud)

- [Guía de decisiones sobre registros e informes en Azure Adoption Framework](/azure/cloud-adoption-framework/decision-guides/logging-and-reporting/)

- [Escala, administración y supervisión empresarial de Azure](/azure/cloud-adoption-framework/ready/enterprise-scale/management-and-monitoring)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

## <a name="next-steps"></a>Pasos siguientes

- Consulte la [Información general sobre Azure Security Benchmark V2](/azure/security/benchmarks/overview).
- Obtenga más información sobre las [líneas de base de seguridad de Azure](/azure/security/benchmarks/security-baselines-overview).

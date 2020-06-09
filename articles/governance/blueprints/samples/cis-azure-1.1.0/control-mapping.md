---
title: Ejemplo de plano técnico de CIS Microsoft Azure Foundations Benchmark, controles
description: Asignación de recomendaciones del ejemplo de plano técnico de CIS Microsoft Azure Foundations Benchmark para Azure Policy.
ms.date: 05/12/2020
ms.topic: sample
ms.openlocfilehash: b6029e147af49cfb91078c6228615c32ad2db5fe
ms.sourcegitcommit: 1692e86772217fcd36d34914e4fb4868d145687b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/29/2020
ms.locfileid: "84167237"
---
# <a name="recommendation-mapping-of-the-cis-microsoft-azure-foundations-benchmark-blueprint-sample"></a>Asignación de recomendaciones del ejemplo de plano técnico de CIS Microsoft Azure Foundations Benchmark

En el siguiente artículo se detalla el modo en que el ejemplo de plano técnico de CIS Microsoft Azure Foundations Benchmark para Azure Blueprints sigue las recomendaciones de CIS Microsoft Azure Foundations Benchmark. Para más información sobre las recomendaciones, consulte [Banco de pruebas de fundamentos CIS para Microsoft Azure](https://www.cisecurity.org/benchmark/azure/).

Las siguientes asignaciones son para las recomendaciones de **CIS Microsoft Azure Foundations Benchmark v1.1.0**. Use el panel de navegación de la derecha para ir directamente a una asignación de recomendación específica.
Muchas de las recomendaciones asignadas se implementan con una iniciativa de [Azure Policy](../../../policy/overview.md). Para revisar la iniciativa completa, abra **Policy** en Azure Portal y seleccione la página **Definiciones**. Después, busque y seleccione la iniciativa de directiva integrada con las recomendaciones de la **\[Versión preliminar\] Audit CIS Microsoft Azure Foundations Benchmark v1.1.0 e implemente extensiones de VM específicas para admitir los requisitos de auditoría**.

> [!IMPORTANT]
> Cada control que se muestra a continuación está asociado a una o varias definiciones de [Azure Policy](../../../policy/overview.md). Estas directivas pueden ayudarle a [evaluar el cumplimiento](../../../policy/how-to/get-compliance-data.md) del control; sin embargo, a menudo no hay una coincidencia 1:1 o completa entre un control y una o varias directivas. Como tal, el **cumplimiento** con Azure Policy solo se refiere a las propias directivas; esto no garantiza que sea totalmente compatible con todos los requisitos de un control. Además, el estándar de cumplimiento incluye controles que no se abordan con las definiciones de Azure Policy en este momento. Por lo tanto, el cumplimiento en Azure Policy es solo una vista parcial del estado general de cumplimiento. Las asociaciones entre los controles y las definiciones de Azure Policy para este ejemplo de plano técnico de cumplimiento pueden cambiar con el tiempo. Para ver el historial de cambios, consulte el [historial de confirmación de GitHub](https://github.com/MicrosoftDocs/azure-docs/commits/master/articles/governance/blueprints/samples/cis-azure-1.1.0/control-mapping.md).

## <a name="11-ensure-that-multi-factor-authentication-is-enabled-for-all-privileged-users"></a>1.1 Asegúrese de que la autenticación multifactor esté habilitada para todos los usuarios con privilegios

Este plano técnico asigna las definiciones de [Azure Policy](../../../policy/overview.md) que le ayudarán con la supervisión cuando la autenticación multifactor no esté habilitada en las cuentas con privilegios Azure Active Directory.

- MFA debe estar habilitada en las cuentas con permisos de propietario en la suscripción
- MFA debe estar habilitada en las cuentas con permisos de escritura en la suscripción

## <a name="12-ensure-that-multi-factor-authentication-is-enabled-for-all-non-privileged-users"></a>1.2 Asegúrese de que la autenticación multifactor esté habilitada para todos los usuarios sin privilegios

Este plano técnico asigna una definición de [Azure Policy](../../../policy/overview.md) que le ayudará con la supervisión cuando la autenticación multifactor no esté habilitada en las cuentas sin privilegios Azure Active Directory.

- MFA debe estar habilitada en las cuentas con permisos de lectura en la suscripción

## <a name="13-ensure-that-there-are-no-guest-users"></a>1.3 Asegúrese de que no hay ningún usuario invitado

Este plano técnico asigna definiciones de [Azure Policy](../../../policy/overview.md) que contribuirán con la supervisión de las cuentas de invitado que quizá deban eliminarse.

- Las cuentas externas con permisos de lectura deben quitarse de la suscripción
- Las cuentas externas con permisos de escritura deben quitarse de la suscripción
- Las cuentas externas con permisos de propietario deben quitarse de la suscripción

## <a name="123-ensure-that-no-custom-subscription-owner-roles-are-created"></a>1.23 Asegúrese de que no existe ningún rol de propietario de suscripción personalizado

Este plano técnico asigna definiciones de [Azure Policy](../../../policy/overview.md) que contribuirán con la supervisión de los roles de propietario de suscripción personalizados que quizá deban eliminarse.

- No deben existir roles de propietario de suscripción personalizados.

## <a name="21-ensure-that-standard-pricing-tier-is-selected"></a>2.1 Asegúrese de que el plan de tarifa estándar está seleccionado

Este plano técnico asigna una definición de [Azure Policy](../../../policy/overview.md) que le ayudará con la supervisión de las redes y las máquinas virtuales en las que no se haya habilitado el plan de tarifa estándar de Security Center.

- Es necesario seleccionar el plan de tarifa estándar de Security Center.

## <a name="22-ensure-that-automatic-provisioning-of-monitoring-agent-is-set-to-on"></a>2.2 Asegúrese de que "Aprovisionamiento automático del agente de supervisión" esté establecido en "Activado"

Este plano técnico asigna una definición de [Azure Policy](../../../policy/overview.md) que le ayudará a garantizar que el aprovisionamiento automático del agente de Log Analytics esté activado.

- El aprovisionamiento automático del agente de supervisión de Log Analytics debe estar habilitado en su suscripción

## <a name="23-ensure-asc-default-policy-setting-monitor-system-updates-is-not-disabled"></a>2.3 Asegúrese de que la configuración de la directiva predeterminada de ASC "Supervisar las actualizaciones del sistema" no es "Deshabilitada".

Este plano técnico asigna una definición de [Azure Policy](../../../policy/overview.md) que contribuirá a garantizar que las actualizaciones del sistema se hayan instalado en las máquinas virtuales.

- Se deben instalar actualizaciones del sistema en las máquinas

## <a name="24-ensure-asc-default-policy-setting-monitor-os-vulnerabilities-is-not-disabled"></a>2.4 Asegúrese de que la configuración de la directiva predeterminada de ASC "Supervisar las vulnerabilidades del sistema operativo" no sea "Deshabilitada".

Este plano técnico asigna una definición de [Azure Policy](../../../policy/overview.md) que contribuirá con la supervisión de los puntos vulnerables de la máquina virtual sin solucionar.

- Se deben corregir las vulnerabilidades en la configuración de seguridad en las máquinas

## <a name="25-ensure-asc-default-policy-setting-monitor-endpoint-protection-is-not-disabled"></a>2.5 Asegúrese de que la configuración de la directiva predeterminada de ASC "Supervisar Endpoint Protection" no sea "Deshabilitada".

Este plano técnico asigna una definición de [Azure Policy](../../../policy/overview.md) que contribuirá a garantizar que la protección de los puntos de conexión esté activada en las máquinas virtuales.

- Supervisión de la falta de Endpoint Protection en Azure Security Center

## <a name="26-ensure-asc-default-policy-setting-monitor-disk-encryption-is-not-disabled"></a>2.6 Asegúrese de que la configuración de la directiva predeterminada de ASC "Supervisar el cifrado de disco" no sea "Deshabilitada".

Este plano técnico asigna una definición de [Azure Policy](../../../policy/overview.md) que contribuirá a garantizar que los discos de las máquinas virtuales estén cifrados.

- El cifrado de discos debe aplicarse en máquinas virtuales

## <a name="27-ensure-asc-default-policy-setting-monitor-network-security-groups-is-not-disabled"></a>2.7 Asegúrese de que la configuración de la directiva predeterminada de ASC "Supervisar los grupos de seguridad de red" no sea "Deshabilitado".

Este plano técnico asigna una definición de [Azure Policy](../../../policy/overview.md) que contribuirá a proteger las máquinas virtuales de cara a Internet.

- Las recomendaciones de protección de red adaptable se deben aplicar en las máquinas virtuales accesibles desde Internet

## <a name="29-ensure-asc-default-policy-setting-enable-next-generation-firewallngfw-monitoring-is-not-disabled"></a>2.9 Asegúrese de que la configuración de la directiva predeterminada de ASC "Habilitar la supervisión de firewalls de última generación (NGFW)" no sea "Deshabilitado"

Este plano técnico asigna definiciones de [Azure Policy](../../../policy/overview.md) que contribuirán a proteger las subredes y las máquinas virtuales de amenazas al restringir el acceso. La directiva de Security Center a la que se hace referencia en esta recomendación de CIS Microsoft Azure Foundations Benchmark se ha reemplazado por dos recomendaciones nuevas. Las directivas a las que se hace referencia a continuación abordan las nuevas recomendaciones.

- Las subredes deben estar asociadas con un grupo de seguridad de red.
- Las máquinas virtuales accesibles desde Internet deben estar protegidas con grupos de seguridad de red

## <a name="210-ensure-asc-default-policy-setting-monitor-vulnerability-assessment-is-not-disabled"></a>2.10 Asegúrese de que la configuración de la directiva predeterminada de ASC "Supervisar la evaluación de vulnerabilidades" no sea "Deshabilitada".

Este plano técnico asigna una definición de [Azure Policy](../../../policy/overview.md) que contribuirá a garantizar que se detecten y se solucionen los puntos vulnerables.

- Se deben corregir las vulnerabilidades mediante una solución de evaluación de vulnerabilidades

## <a name="211-ensure-asc-default-policy-setting-monitor-storage-blob-encryption-is-not-disabled"></a>2.11 Asegúrese de que la configuración de la directiva predeterminada de ASC "Supervisar el cifrado de Blob Storage" no sea "Deshabilitado"

El cifrado de Azure Storage está habilitado para todas las cuentas de almacenamiento nuevas y existentes y no se puede deshabilitar. (Se trata de una funcionalidad de Azure predeterminada; no hay asignaciones de directiva).

## <a name="212-ensure-asc-default-policy-setting-monitor-jit-network-access-is-not-disabled"></a>2.12 Asegúrese de que la configuración de la directiva predeterminada de ASC "Supervisar el acceso de red JIT" no sea "Deshabilitada".

Este plano técnico asigna una definición de [Azure Policy](../../../policy/overview.md) que contribuirá a controlar el acceso a las máquinas virtuales.

- El control de acceso de red Just-In-Time se debe aplicar en las máquinas virtuales.

## <a name="213-ensure-asc-default-policy-setting-monitor-adaptive-application-whitelisting-is-not-disabled"></a>2.13 Asegúrese de que la configuración de la directiva predeterminada de ASC "Monitor Adaptive Application Whitelisting" (Supervisar las listas blancas de aplicaciones adaptables) no sea "Deshabilitado".

Este plano técnico asigna una definición de [Azure Policy](../../../policy/overview.md) que contribuirá a garantizar que los controles de aplicaciones adaptables estén habilitados en las máquinas virtuales.

- Deben habilitarse los controles de aplicaciones adaptables en las máquinas virtuales

## <a name="214-ensure-asc-default-policy-setting-monitor-sql-auditing-is-not-disabled"></a>2.14 Asegúrese de que la configuración de la directiva predeterminada de ASC "Supervisar la auditoría de SQL" no sea "Deshabilitado".

Este plano técnico asigna una definición de [Azure Policy](../../../policy/overview.md) que ayudará a garantizar que la auditoría de SQL Server esté habilitada.

- La auditoría de SQL Server debe estar habilitada

## <a name="215-ensure-asc-default-policy-setting-monitor-sql-encryption-is-not-disabled"></a>2.15 Asegúrese de que la configuración de la directiva predeterminada de ASC "Supervisar el cifrado SQL" no sea "Deshabilitada".

Este plano técnico asigna una definición de [Azure Policy](../../../policy/overview.md) que contribuirá a garantizar que el cifrado de datos transparente esté habilitado en las bases de datos SQL.

- Se debe permitir el cifrado de datos transparente en bases de datos SQL

## <a name="216-ensure-that-security-contact-emails-is-set"></a>2.16 Asegúrese de que se ha establecido "Correos electrónicos de contacto de seguridad"

Este plano técnico asigna una definición de [Azure Policy](../../../policy/overview.md) que contribuirá a garantizar que las notificaciones de seguridad estén correctamente establecidas

- Es necesario proporcionar una dirección de correo electrónico de contacto de seguridad para la suscripción

## <a name="217-ensure-that-security-contact-phone-number-is-set"></a>2.17 Asegúrese de que se ha establecido "Número de teléfono"

Este plano técnico asigna una definición de [Azure Policy](../../../policy/overview.md) que contribuirá a garantizar que las notificaciones de seguridad estén correctamente establecidas

- Es necesario proporcionar un número de teléfono de contacto de seguridad para la suscripción

## <a name="218-ensure-that-send-email-notification-for-high-severity-alerts-is-set-to-on"></a>2.18 Asegúrese de que "Enviar notificaciones sobre alertas de gravedad alta por correo electrónico" se ha establecido en "Activado"

Este plano técnico asigna una definición de [Azure Policy](../../../policy/overview.md) que contribuirá a garantizar que las notificaciones de seguridad estén correctamente establecidas

- La opción para enviar notificaciones por correo electrónico para alertas de gravedad alta debe estar habilitada

## <a name="219-ensure-that-send-email-also-to-subscription-owners-is-set-to-on"></a>2.19 Asegúrese de que "Enviar correo electrónico también a los propietarios de la suscripción" esté establecido en "Activado"

Este plano técnico asigna una definición de [Azure Policy](../../../policy/overview.md) que contribuirá a garantizar que las notificaciones de seguridad estén correctamente establecidas

- La opción para enviar notificaciones por correo electrónico al propietario de la suscripción en relación a alertas de gravedad alta debe estar habilitada

## <a name="31-ensure-that-secure-transfer-required-is-set-to-enabled"></a>3.1 Asegúrese de la opción "Se requiere transferencia segura" esté establecida en "Habilitada".

Este plano técnico asigna una definición de [Azure Policy](../../../policy/overview.md) que ayudará a supervisar las cuentas de almacenamiento que permitan las conexiones no seguras.

- Se debe habilitar la transferencia segura a las cuentas de almacenamiento

## <a name="37-ensure-default-network-access-rule-for-storage-accounts-is-set-to-deny"></a>3.7 Asegúrese de que la regla de acceso de red predeterminada para las cuentas de almacenamiento esté configurada para denegar

Este plano técnico asigna una definición de [Azure Policy](../../../policy/overview.md) que contribuirá a supervisar las cuentas de almacenamiento que permitan el acceso sin restricciones.

- Auditar el acceso de red sin restricciones a cuentas de almacenamiento

## <a name="38-ensure-trusted-microsoft-services-is-enabled-for-storage-account-access"></a>3.8 Asegúrese de que "Servicios de Microsoft de confianza" está habilitado para el acceso a la cuenta de almacenamiento

Este plano técnico asigna una definición de [Azure Policy](../../../policy/overview.md) que contribuirá a supervisar las cuentas de almacenamiento que no permitan el acceso desde los servicios de Microsoft de confianza.

- Las cuentas de almacenamiento deben permitir el acceso desde los servicios de Microsoft de confianza

## <a name="41-ensure-that-auditing-is-set-to-on"></a>4.1 Asegúrese de que la opción "Auditando" esté establecida en "Activada"

Este plano técnico asigna una definición de [Azure Policy](../../../policy/overview.md) que ayudará a garantizar que la auditoría de SQL Server esté habilitada. 

- La auditoría de SQL Server debe estar habilitada

## <a name="42-ensure-that-auditactiongroups-in-auditing-policy-for-a-sql-server-is-set-properly"></a>4.2 Asegúrese de que la opción "AuditActionGroups" de la directiva de auditoría para un servidor SQL Server esté configurada correctamente

Este plano técnico asigna una definición de [Azure Policy](../../../policy/overview.md) que ayudará a garantizar que la auditoría de SQL Server esté configurada correctamente.

- La opción de configuración Auditoría de SQL debería tener configurado Action-Groups para capturar las actividades críticas

## <a name="43-ensure-that-auditing-retention-is-greater-than-90-days"></a>4.3 Asegúrese de que la retención de "Auditoría" sea "más de 90 días"

Este plano técnico asigna una definición de [Azure Policy](../../../policy/overview.md) que ayudará a garantizar que los registros de SQL Server se conserven durante al menos 90 días.

- Debe configurar los servidores SQL Server para que realicen una retención de la auditoría durante más de 90 días.

## <a name="44-ensure-that-advanced-data-security-on-a-sql-server-is-set-to-on"></a>4.4 Asegúrese de que "Advanced Data Security" en un servidor SQL Server esté establecido en "Activado".

Este plano técnico asigna una definición de [Azure Policy](../../../policy/overview.md) que contribuirá a garantizar que la seguridad de los datos avanzada esté habilitada en los servidores de instancia administrada de SQL y SQL Database.

- La seguridad avanzada de datos debe estar habilitada en los servidores SQL Server
- La opción Advanced Data Security debe estar habilitada en las instancias administradas de SQL

## <a name="45-ensure-that-threat-detection-types-is-set-to-all"></a>4.5 Asegúrese de que "Tipos de detección de amenazas" esté establecido en "Todo"

Este plano técnico asigna definiciones de [Azure Policy](../../../policy/overview.md) que contribuirán a garantizar que la protección avanzada contra amenazas esté configurada correctamente en los servidores de instancia administrada de SQL y SQL Database.

- Los tipos de Advanced Threat Protection deben estar establecidos en "Todos" en la configuración de Advanced Data Security de SQL Server
- Los tipos de Advanced Threat Protection deben estar establecidos en "Todos" en la configuración de Advanced Data Security de la instancia administrada de SQL Server

## <a name="46-ensure-that-send-alerts-to-is-set"></a>4.6 Asegúrese de que "Enviar alertas a" esté establecido

Este plano técnico asigna una definición de [Azure Policy](../../../policy/overview.md) que contribuirá a garantizar que se habilitan correctamente las notificaciones de seguridad de datos avanzada.

- La configuración de seguridad de datos avanzada para SQL Server debe contener una dirección de correo electrónico para recibir alertas de seguridad
- La configuración de seguridad de datos avanzada para una instancia administrada SQL debe contener una dirección de correo electrónico para recibir alertas de seguridad

## <a name="47-ensure-that-email-service-and-co-administrators-is-enabled"></a>4.7 Asegúrese de que la configuración de "Servicio de correo electrónico y coadministradores" sea "Habilitado"

Este plano técnico asigna una definición de [Azure Policy](../../../policy/overview.md) que contribuirá a garantizar que se habilitan correctamente las notificaciones de seguridad de datos avanzada.

- Las notificaciones por correo electrónico para administradores y propietarios de suscripciones deben estar habilitadas en la configuración de seguridad avanzada de datos de SQL Server
- Las notificaciones a los administradores y a los propietarios de la suscripción deben estar habilitadas en la configuración de seguridad avanzada de datos de la instancia administrada de SQL

## <a name="48-ensure-that-azure-active-directory-admin-is-configured"></a>4.8 Asegúrese de que el administrador de Azure Active Directory esté configurado

Este plano técnico asigna una definición de [Azure Policy](../../../policy/overview.md) que contribuirá a garantizar que se aprovisione un administrador de Azure Active Directory para los servidores SQL Server.

- El administrador de Azure Active Directory debe aprovisionarse para servidores SQL Server

## <a name="49-ensure-that-data-encryption-is-set-to-on-on-a-sql-database"></a>4.9 Asegúrese de que la opción "Cifrado de datos" esté establecida en "Activado" en una base de datos SQL Database

Este plano técnico asigna una definición de [Azure Policy](../../../policy/overview.md) que contribuirá a garantizar que el cifrado de datos transparente esté habilitado en las bases de datos SQL.

- Se debe permitir el cifrado de datos transparente en bases de datos SQL

## <a name="410-ensure-sql-servers-tde-protector-is-encrypted-with-byok-use-your-own-key"></a>4.10 Asegúrese de que el protector de TDE de SQL Server esté cifrado con BYOK (use su propia clave)

Este plano técnico asigna definiciones de [Azure Policy](../../../policy/overview.md) que contribuirán a garantizar que el protector cifrado de datos transparente para los servidores de instancia administrada de SQL y SQL Database esté cifrado con su propia clave.

- El protector de TDE de SQL Server debe estar cifrado con su propia clave
- El protector de TDE de la instancia administrada SQL debe estar cifrado con su propia clave

## <a name="411-ensure-enforce-ssl-connection-is-set-to-enabled-for-mysql-database-server"></a>4.11 Asegúrese de que "Aplicar conexión SSL" esté establecido en "HABILITADO" para el servidor de bases de datos MySQL

Este plano técnico asigna una definición de [Azure Policy](../../../policy/overview.md) que contribuirá a garantizar que los servidores de bases de datos MySQL aplican la conexión TLS/SSL.

- La aplicación de la conexión SSL debe estar habilitada para los servidores de bases de datos MySQL

## <a name="412-ensure-server-parameter-log_checkpoints-is-set-to-on-for-postgresql-database-server"></a>4.12 Asegúrese de que "log_checkpoints" del parámetro del servidor está establecido en "ACTIVADO" para el servidor de bases de datos PostgreSQL

Este plano técnico asigna una definición de [Azure Policy](../../../policy/overview.md) que contribuye a garantizar que los servidores de bases de datos PostgreSQL registran los puntos de control.

- Los puntos de control del registro se deben habilitar para los servidores de base de datos de PostgreSQL.

## <a name="413-ensure-enforce-ssl-connection-is-set-to-enabled-for-postgresql-database-server"></a>4.13 Asegúrese de que "Aplicar conexión SSL" esté establecido en "HABILITADO" para el servidor de bases de datos PostgreSQL

Este plano técnico asigna una definición de [Azure Policy](../../../policy/overview.md) que contribuirá a garantizar que los servidores de bases de datos PostgreSQL aplican la conexión TLS/SSL.

- La aplicación de la conexión SSL debe estar habilitada para los servidores de bases de datos PostgreSQL

## <a name="414-ensure-server-parameter-log_connections-is-set-to-on-for-postgresql-database-server"></a>4.14 Asegúrese de que "log_connections" del parámetro del servidor está establecido en "ACTIVADO" para el servidor de bases de datos PostgreSQL

Este plano técnico asigna una definición de [Azure Policy](../../../policy/overview.md) que contribuye a garantizar que los servidores de bases de datos PostgreSQL registran las conexiones.

- Las conexiones del registro deben estar habilitadas para los servidores de bases de datos de PostgreSQL.

## <a name="415-ensure-server-parameter-log_disconnections-is-set-to-on-for-postgresql-database-server"></a>4.15 Asegúrese de que "log_disconnections" del parámetro del servidor está establecido en "ACTIVADO" para el servidor de bases de datos PostgreSQL

Este plano técnico asigna una definición de [Azure Policy](../../../policy/overview.md) que contribuye a garantizar que los servidores de bases de datos PostgreSQL registran las desconexiones.

- Las desconexiones se deben registrar para los servidores de base de datos de PostgreSQL.

## <a name="416-ensure-server-parameter-log_duration-is-set-to-on-for-postgresql-database-server"></a>4.16 Asegúrese de que "log_duration" del parámetro del servidor está establecido en "ACTIVADO" para el servidor de bases de datos PostgreSQL

Este plano técnico asigna una definición de [Azure Policy](../../../policy/overview.md) que contribuye a garantizar que los servidores de bases de datos PostgreSQL registran la duración de las instrucciones completadas.

- La duración del registro debe estar habilitada para los servidores de bases de datos de PostgreSQL.

## <a name="417-ensure-server-parameter-connection_throttling-is-set-to-on-for-postgresql-database-server"></a>4.17 Asegúrese de que "connection_throttling" del parámetro del servidor está establecido en "ACTIVADO" para el servidor de bases de datos PostgreSQL

Este plano técnico asigna una definición de [Azure Policy](../../../policy/overview.md) que contribuirá a mitigar los ataques por fuerza bruta en los servidores de bases de datos PostgreSQL.

- La limitación de conexiones debe estar habilitada para los servidores de bases de datos PostgreSQL

## <a name="419-ensure-that-azure-active-directory-admin-is-configured"></a>4.19 Asegúrese de que el administrador de Azure Active Directory está configurado

Este plano técnico asigna una definición de [Azure Policy](../../../policy/overview.md) que contribuirá a garantizar que se aprovisione un administrador de Azure Active Directory para los servidores SQL Server. CIS Microsoft Azure Foundations Benchmark incluye esta recomendación; sin embargo, es un duplicado de la [recomendación 4.8](#48-ensure-that-azure-active-directory-admin-is-configured).

- El administrador de Azure Active Directory debe aprovisionarse para servidores SQL Server

## <a name="511-ensure-that-a-log-profile-exists"></a>5.1.1 Asegúrese de que existe un perfil de registro

Este plano técnico asigna una definición de [Azure Policy](../../../policy/overview.md) que contribuirá a garantizar que exista un perfil de registro para todas las suscripciones a Azure. 

- Las suscripciones a Azure deben tener un perfil de registro para el registro de actividad

## <a name="512-ensure-that-activity-log-retention-is-set-365-days-or-greater"></a>5.1.2 Asegúrese de que el período de retención del registro de actividad está establecido en 365 días o más

Este plano técnico asigna una definición de [Azure Policy](../../../policy/overview.md) que contribuirá a garantizar que los registros de actividad se conserven durante un año como mínimo.

- El registro de actividad debe conservarse durante al menos un año

## <a name="513-ensure-audit-profile-captures-all-the-activities"></a>5.1.3 Asegúrese de que el perfil de auditoría captura todas las actividades

Este plano técnico asigna una definición de [Azure Policy](../../../policy/overview.md) que ayudará a garantizar que el perfil de registro esté configurado correctamente.

- El perfil de registro de Azure Monitor debe recopilar los registros de las categorías "write", "delete" y "action"

## <a name="514-ensure-the-log-profile-captures-activity-logs-for-all-regions-including-global"></a>5.1.4 Asegúrese de que el perfil de registro captura los registros de actividad de todas las regiones, incluida la global

Este plano técnico asigna una definición de [Azure Policy](../../../policy/overview.md) que ayudará a garantizar que el perfil de registro esté configurado correctamente.

- Azure Monitor debe recopilar los registros de actividad de todas las regiones

## <a name="516-ensure-the-storage-account-containing-the-container-with-activity-logs-is-encrypted-with-byok-use-your-own-key"></a>5.1.6 Asegúrese de que la cuenta de almacenamiento que contiene el contenedor con los registros de actividad está cifrada con BYOK (use su propia clave)

Este plano técnico asigna una definición de [Azure Policy](../../../policy/overview.md) que contribuirá a garantizar que las cuentas de almacenamiento que contengan registros de actividad se cifren con BYOK.

- La cuenta de almacenamiento que contiene el contenedor con los registros de actividad tiene que estar cifrada con BYOK.

## <a name="517-ensure-that-logging-for-azure-keyvault-is-enabled"></a>5.1.7 Asegúrese de que el registro de Azure KeyVault esté habilitado.

Este plano técnico asigna una definición de [Azure Policy](../../../policy/overview.md) que ayudará a garantizar que los registros de diagnóstico estén habilitados para los almacenes de claves.

- Los registros de diagnóstico en Key Vault deben estar habilitados

## <a name="521-ensure-that-activity-log-alert-exists-for-create-policy-assignment"></a>5.2.1 Asegúrese de que existe una alerta de registro de actividad para la creación de una asignación de directiva

Este plano técnico asigna una definición de [Azure Policy](../../../policy/overview.md) que ayuda a garantizar que existan alertas de registro de actividad específicas.

- Debe existir una alerta de registro de actividad para las operaciones de directiva específicas

## <a name="522-ensure-that-activity-log-alert-exists-for-create-or-update-network-security-group"></a>5.2.2 Asegúrese de que existe una alerta de registro de actividad para crear o actualizar el grupo de seguridad de red

Este plano técnico asigna una definición de [Azure Policy](../../../policy/overview.md) que ayuda a garantizar que existan alertas de registro de actividad específicas.

- Debe existir una alerta de registro de actividad para operaciones administrativas específicas.

## <a name="523-ensure-that-activity-log-alert-exists-for-delete-network-security-group"></a>5.2.3 Asegúrese de que existe una alerta de registro de actividad para eliminar el grupo de seguridad de red

Este plano técnico asigna una definición de [Azure Policy](../../../policy/overview.md) que ayuda a garantizar que existan alertas de registro de actividad específicas.

- Debe existir una alerta de registro de actividad para operaciones administrativas específicas.

## <a name="524-ensure-that-activity-log-alert-exists-for-create-or-update-network-security-group-rule"></a>5.2.4 Asegúrese de que existe una alerta de registro de actividad para la regla Crear o actualizar grupo de seguridad de red

Este plano técnico asigna una definición de [Azure Policy](../../../policy/overview.md) que ayuda a garantizar que existan alertas de registro de actividad específicas.

- Debe existir una alerta de registro de actividad para operaciones administrativas específicas.

## <a name="525-ensure-that-activity-log-alert-exists-for-the-delete-network-security-group-rule"></a>5.2.5 Asegúrese de que existe una alerta de registro de actividad para la regla Eliminar grupo de seguridad de red

Este plano técnico asigna una definición de [Azure Policy](../../../policy/overview.md) que ayuda a garantizar que existan alertas de registro de actividad específicas.

- Debe existir una alerta de registro de actividad para operaciones administrativas específicas.

## <a name="526-ensure-that-activity-log-alert-exists-for-create-or-update-security-solution"></a>5.2.6 Asegúrese de que existe una alerta de registro de actividad para la solución de seguridad de creación o actualización

Este plano técnico asigna una definición de [Azure Policy](../../../policy/overview.md) que ayuda a garantizar que existan alertas de registro de actividad específicas.

- Debe existir una alerta de registro de actividad para operaciones de seguridad específicas

## <a name="527-ensure-that-activity-log-alert-exists-for-delete-security-solution"></a>5.2.7 Asegúrese de que existe una alerta de registro de actividad para la solución de seguridad de eliminación

Este plano técnico asigna una definición de [Azure Policy](../../../policy/overview.md) que ayuda a garantizar que existan alertas de registro de actividad específicas.

- Debe existir una alerta de registro de actividad para operaciones de seguridad específicas

## <a name="528-ensure-that-activity-log-alert-exists-for-create-or-update-or-delete-sql-server-firewall-rule"></a>5.2.8 Asegúrese de que existe una alerta de registro de actividad para la regla de firewall Crear, actualizar o eliminar SQL Server

Este plano técnico asigna una definición de [Azure Policy](../../../policy/overview.md) que ayuda a garantizar que existan alertas de registro de actividad específicas.

- Debe existir una alerta de registro de actividad para operaciones administrativas específicas.

## <a name="529-ensure-that-activity-log-alert-exists-for-update-security-policy"></a>5.2.9 Asegúrese de que existe una alerta de registro de actividad para la actualización de la directiva de seguridad

Este plano técnico asigna una definición de [Azure Policy](../../../policy/overview.md) que ayuda a garantizar que existan alertas de registro de actividad específicas.

- Debe existir una alerta de registro de actividad para operaciones de seguridad específicas

## <a name="61-ensure-that-rdp-access-is-restricted-from-the-internet"></a>6.1 Asegúrese de que el acceso RDP está restringido desde Internet

Este plano técnico asigna una definición de [Azure Policy](../../../policy/overview.md) que ayuda a garantizar que el acceso RDP está restringido.

- El acceso RDP desde Internet debe estar bloqueado.

## <a name="62-ensure-that-ssh-access-is-restricted-from-the-internet"></a>6.2 Asegúrese de que el acceso SSH está restringido desde Internet

Este plano técnico asigna una definición de [Azure Policy](../../../policy/overview.md) que ayuda a garantizar que el acceso SSH está restringido.

- El acceso SSH desde Internet debe estar bloqueado.

## <a name="65-ensure-that-network-watcher-is-enabled"></a>6.5 Asegúrese de que Network Watcher está "Habilitado"

Este plano técnico asigna una definición de [Azure Policy](../../../policy/overview.md) que contribuirá a garantizar que Network Watcher esté habilitado para todas las regiones en las que haya recursos implementados. Esta directiva requiere una matriz de parámetros que especifique todas las regiones aplicables. El valor predeterminado de esta definición de iniciativa de directiva es "eastus".

- Network Watcher debe estar habilitado

## <a name="71-ensure-that-os-disk-are-encrypted"></a>7.1 Asegúrese de que "Disco del SO" esté cifrado

Este plano técnico asigna una definición de [Azure Policy](../../../policy/overview.md) que contribuirá a garantizar que el cifrado de los discos está habilitado en las máquinas virtuales.

- El cifrado de discos debe aplicarse en máquinas virtuales

## <a name="72-ensure-that-data-disks-are-encrypted"></a>7.2 Asegúrese de que la opción "Discos de datos" esté cifrada

Este plano técnico asigna una definición de [Azure Policy](../../../policy/overview.md) que contribuirá a garantizar que el cifrado de los discos está habilitado en las máquinas virtuales.

- El cifrado de discos debe aplicarse en máquinas virtuales

## <a name="73-ensure-that-unattached-disks-are-encrypted"></a>7.3 Asegúrese de que la opción "Unattached disks" (Discos no asignados) esté cifrada

Este plano técnico asigna una definición de [Azure Policy](../../../policy/overview.md) que contribuirá a garantizar que los discos no asignados estén cifrados.

- Los discos sin asignar deben cifrarse

## <a name="74-ensure-that-only-approved-extensions-are-installed"></a>7.4 Asegúrese de que solo están instaladas las extensiones aprobadas

Este plano técnico asigna una definición de [Azure Policy](../../../policy/overview.md) que contribuirá a garantizar que solo se hayan instalado las extensiones de máquina virtual aprobadas. Esta directiva requiere una matriz de parámetros que especifique todas las extensiones de máquina virtual aprobadas. Esta definición de iniciativa de directiva contiene valores predeterminados sugeridos que los clientes deben validar. 

- Solo deben instalarse las extensiones de máquina virtual aprobadas

## <a name="75-ensure-that-the-latest-os-patches-for-all-virtual-machines-are-applied"></a>7.5 Asegúrese de que se aplican las revisiones del sistema operativo más recientes para todas las máquinas virtuales

Este plano técnico asigna una definición de [Azure Policy](../../../policy/overview.md) que contribuirá a garantizar que las actualizaciones del sistema se hayan instalado en las máquinas virtuales.

- Se deben instalar actualizaciones del sistema en las máquinas

## <a name="76-ensure-that-the-endpoint-protection-for-all-virtual-machines-is-installed"></a>7.6 Asegúrese de que Endpoint Protection esté instalado para todas las máquinas virtuales

Este plano técnico asigna una definición de [Azure Policy](../../../policy/overview.md) que contribuirá a garantizar que la protección de los puntos de conexión esté activada en las máquinas virtuales.

- Supervisión de la falta de Endpoint Protection en Azure Security Center

## <a name="84-ensure-the-key-vault-is-recoverable"></a>8.4 Asegúrese de que el almacén de claves se puede recuperar

Este plano técnico asigna una definición de [Azure Policy](../../../policy/overview.md) que contribuirá a garantizar que los objetos del almacén de claves se puedan recuperar en caso de eliminación por error.

- Los objetos del almacén de claves deben poder recuperarse

## <a name="85-enable-role-based-access-control-rbac-within-azure-kubernetes-services"></a>8.5 Habilite el control de acceso basado en rol (RBAC) en Azure Kubernetes Services

Este plano técnico asigna una definición de [Azure Policy](../../../policy/overview.md) que contribuirá a garantizar que se usa el control de acceso basado en rol para los permisos administrados en los clústeres de servicio de Kubernetes

- En Kubernetes Services se debe usar el control de acceso basado en rol (RBAC)

## <a name="91-ensure-app-service-authentication-is-set-on-azure-app-service"></a>9.1 Asegúrese de que la autenticación de App Service está establecida en Azure App Service

Este plano técnico asigna una definición de [Azure Policy](../../../policy/overview.md) que contribuirá a garantizar que las solicitudes a las aplicaciones de App Service se autentican.

- La autenticación debe estar habilitada en la aplicación de la API.
- La autenticación debe estar habilitada en la aplicación de funciones
- La autenticación debe estar habilitada en la aplicación web

## <a name="92-ensure-web-app-redirects-all-http-traffic-to-https-in-azure-app-service"></a>9.2 Asegúrese de que la aplicación web redirige todo el tráfico HTTP a HTTPS en Azure App Service

Este plano técnico asigna una definición de [Azure Policy](../../../policy/overview.md) que contribuirá a garantizar que se puede acceder a las aplicaciones web solo mediante conexiones seguras.

- Acceso a la aplicación web solo a través de HTTPS

## <a name="93-ensure-web-app-is-using-the-latest-version-of-tls-encryption"></a>9.3 Asegurarse de que la aplicación web usa la versión más reciente del cifrado TLS

Este plano técnico asigna definiciones de [Azure Policy](../../../policy/overview.md) que le ayudarán a garantizar que las aplicaciones web utilizan la versión más reciente de TLS.

- La versión más reciente de TLS debe usarse en la aplicación de API.
- La versión más reciente de TLS debe usarse en la aplicación de funciones.
- La versión más reciente de TLS debe usarse en la aplicación web.

## <a name="94-ensure-the-web-app-has-client-certificates-incoming-client-certificates-set-to-on"></a>9.4 Asegúrese de que la aplicación web tenga la opción "Client Certificates (Incoming client certificates)" activada.

Este plano técnico asigna definiciones de [Azure Policy](../../../policy/overview.md) que le ayudarán a asegurarse de que solo aquellos clientes con certificados válidos pueden acceder a una aplicación web.

- Asegúrese de que la aplicación de API tenga la opción "Client Certificates (Incoming client certificates)" activada.
- Asegúrese de que la aplicación de funciones tenga la opción "Client Certificates (Incoming client certificates)" activada.
- Asegúrese de que la aplicación web tenga la opción "Client Certificates (Incoming client certificates)" activada.

## <a name="95-ensure-that-register-with-azure-active-directory-is-enabled-on-app-service"></a>9.5 Asegúrese de que el registro con Azure Active Directory esté habilitado en App Service.

Este plano técnico asigna definiciones de [Azure Policy](../../../policy/overview.md) que le ayudarán a garantizar que las aplicaciones web utilizan una identidad administrada.

- Asegúrese de que el registro con Azure Active Directory esté habilitado en la aplicación de API.
- Asegúrese de que el registro con Azure Active Directory esté habilitado en la aplicación de funciones.
- Asegúrese de que el registro con Azure Active Directory esté habilitado en la aplicación web.

## <a name="96-ensure-that-net-framework-version-is-the-latest-if-used-as-a-part-of-the-web-app"></a>9.6 Asegúrese de que la versión de .NET Framework sea la más reciente si se usa como parte de la aplicación web.

Este plano técnico asigna definiciones de [Azure Policy](../../../policy/overview.md) que le ayudarán a garantizar que las aplicaciones web utilizan la versión más reciente de .NET Framework.

- Asegúrese de que la versión de .NET Framework es la más reciente si se usa como parte de la aplicación de API.
- Asegúrese de que la versión de .NET Framework es la más reciente si se usa como parte de la aplicación de funciones.
- Asegúrese de que la versión de .NET Framework es la más reciente si se usa como parte de la aplicación web.

## <a name="97-ensure-that-php-version-is-the-latest-if-used-to-run-the-web-app"></a>9.7 Asegúrese de que la versión de PHP es la más reciente si se usa para ejecutar la aplicación web.

Este plano técnico asigna definiciones de [Azure Policy](../../../policy/overview.md) que le ayudarán a garantizar que las aplicaciones web utilizan la versión más reciente de PHP.

- Asegúrese de que la versión de PHP es la más reciente si se usa como parte de la aplicación de API.
- Asegúrese de que la versión de PHP es la más reciente si se usa como parte de la aplicación de funciones.
- Asegúrese de que la versión de PHP es la más reciente si se usa como parte de la aplicación web.

## <a name="98-ensure-that-python-version-is-the-latest-if-used-to-run-the-web-app"></a>9.8 Asegúrese de que la versión de Python es la más reciente si se usa para ejecutar la aplicación web.

Este plano técnico asigna definiciones de [Azure Policy](../../../policy/overview.md) que le ayudarán a garantizar que las aplicaciones web utilizan la versión más reciente de Python.

- Asegúrese de que la versión de Python es la más reciente si se usa como parte de la aplicación de API.
- Asegúrese de que la versión de Python" es la más reciente si se usa como parte de la aplicación de funciones.
- Asegúrese de que la versión de Python es la más reciente si se usa como parte de la aplicación web.

## <a name="99-ensure-that-java-version-is-the-latest-if-used-to-run-the-web-app"></a>9.9 Asegúrese de que la versión de Java es la más reciente si se usa para ejecutar la aplicación web.

Este plano técnico asigna definiciones de [Azure Policy](../../../policy/overview.md) que le ayudarán a garantizar que las aplicaciones web utilizan la versión más reciente de Java.

- Asegúrese de que la versión de Java es la más reciente si se usa como parte de la aplicación de API.
- Asegúrese de que la versión de Java es la más reciente si se usa como parte de la aplicación de funciones.
- Asegúrese de que la versión de Java es la más reciente si se usa como parte de la aplicación web.

## <a name="910-ensure-that-http-version-is-the-latest-if-used-to-run-the-web-app"></a>9.10 Asegúrese de que la versión de HTTP es la más reciente si se usa para ejecutar la aplicación web.

Este plano técnico asigna definiciones de [Azure Policy](../../../policy/overview.md) que le ayudarán a garantizar que las aplicaciones web utilizan la versión más reciente de HTTP.

- Asegúrese de que la versión de HTTP es la más reciente si se usa para ejecutar la aplicación de API.
- Asegúrese de que la versión de HTTP es la más reciente si se usa para ejecutar la aplicación de funciones.
- Asegúrese de que la versión de HTTP es la más reciente si se usa para ejecutar la aplicación web.

## <a name="next-steps"></a>Pasos siguientes

Ahora que ha revisado la asignación de control del plano técnico de CIS Microsoft Azure Foundations Benchmark, visite los siguientes artículos para obtener información sobre el plano técnico o visite Azure Policy en Azure Portal para asignar la iniciativa:

> [!div class="nextstepaction"]
> [Plano técnico de CIS Microsoft Azure Foundations Benchmark: Introducción](./index.md)
> [Plano técnico de CIS Microsoft Azure Foundations Benchmark: pasos de implementación](./deploy.md)

Artículos adicionales sobre planos técnicos y cómo utilizarlos:

- Información acerca del [ciclo de vida del plano técnico](../../concepts/lifecycle.md).
- Descubra cómo utilizar [parámetros estáticos y dinámicos](../../concepts/parameters.md).
- Aprenda a personalizar el [orden de secuenciación de planos técnicos](../../concepts/sequencing-order.md).
- Averigüe cómo usar el [bloqueo de recursos de planos técnicos](../../concepts/resource-locking.md).
- Aprenda a [actualizar las asignaciones existentes](../../how-to/update-existing-assignments.md).
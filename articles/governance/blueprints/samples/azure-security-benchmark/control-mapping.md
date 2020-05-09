---
title: Controles de ejemplo de plano técnico de Azure Security Benchmark
description: Asignación de controles de ejemplo de plano técnico de Azure Security Benchmark a Azure Policy.
ms.date: 04/16/2020
ms.topic: sample
ms.openlocfilehash: f80b72f06532adef28bf5e5afd1eb94c2e34ee2d
ms.sourcegitcommit: 366e95d58d5311ca4b62e6d0b2b47549e06a0d6d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82691304"
---
# <a name="control-mapping-of-the-azure-security-benchmark-blueprint-sample"></a>Asignación de controles de ejemplo de plano técnico de Azure Security Benchmark

En el siguiente artículo se detalla cómo el ejemplo de plano técnico de Azure Security Benchmark de Azure Blueprints se asigna a los controles de Azure Security Benchmark. Para más información sobre los controles, consulte [Azure Security Benchmark](../../../../security/benchmarks/overview.md).

Las siguientes asignaciones son para controles de **Azure Security Benchmark**. Use el panel de navegación de la derecha para ir directamente a una asignación de control específica. Los controles asignados se implementan con una iniciativa de [Azure Policy](../../../policy/overview.md). Para revisar la iniciativa completa, abra **Policy** en Azure Portal y seleccione la página **Definiciones**. Después, busque y seleccione la **\[versión preliminar\]: Auditar las recomendaciones de Azure Security Benchmark e implementar extensiones de VM compatibles específicas** (iniciativa de directiva integrada).

> [!IMPORTANT]
> Cada control que se muestra a continuación está asociado a una o varias definiciones de [Azure Policy](../../../policy/overview.md). Estas directivas pueden ayudarle a [evaluar el cumplimiento](../../../policy/how-to/get-compliance-data.md) del control; sin embargo, a menudo no hay una coincidencia 1:1 o completa entre un control y una o varias directivas. Como tal, el **cumplimiento** con Azure Policy solo se refiere a las propias directivas; esto no garantiza que sea totalmente compatible con todos los requisitos de un control. Además, el estándar de cumplimiento puede incluir controles que no se abordan con las definiciones de Azure Policy en este momento. Por lo tanto, el cumplimiento en Azure Policy es solo una vista parcial del estado general de cumplimiento. Las asociaciones entre los controles y las definiciones de Azure Policy para este ejemplo de plano técnico de cumplimiento pueden cambiar con el tiempo. Para ver el historial de cambios, consulte el [historial de confirmación de GitHub](https://github.com/MicrosoftDocs/azure-docs/commits/master/articles/governance/blueprints/samples/azure-security-benchmark/control-mapping.md).

## <a name="11-protect-resources-using-network-security-groups-or-azure-firewall-on-your-virtual-network"></a>1.1 Protección de los recursos mediante grupos de seguridad de red o Azure Firewall en su red virtual

- Las subredes deben estar asociadas con un grupo de seguridad de red.
- Las recomendaciones de protección de red adaptable se deben aplicar en las máquinas virtuales accesibles desde Internet
- Las máquinas virtuales deben estar conectadas a una red virtual aprobada
- Las máquinas virtuales accesibles desde Internet deben estar protegidas con grupos de seguridad de red
- Service Bus debe usar un punto de conexión del servicio de red virtual
- App Service debe usar un punto de conexión del servicio de red virtual
- SQL Server debe usar un punto de conexión del servicio de red virtual
- El centro de eventos debe usar un punto de conexión del servicio de red virtual
- Cosmos DB debe usar un punto de conexión de servicio de red virtual
- Key Vault debe usar un punto de conexión del servicio de red virtual
- Auditar el acceso de red sin restricciones a cuentas de almacenamiento
- Las cuentas de almacenamiento deben usar un punto de conexión del servicio de red virtual
- Container Registry debe usar un punto de conexión del servicio de red virtual
- Las redes virtuales deben usar la puerta de enlace de red virtual especificada
- Los intervalos IP autorizados deben definirse en los servicios de Kubernetes
- \[Versión preliminar\]: el reenvío de IP en la máquina virtual debe estar deshabilitado
- Las máquinas virtuales accesibles desde Internet deben estar protegidas con grupos de seguridad de red
- El control de acceso de red Just-In-Time se debe aplicar en las máquinas virtuales.
- Se deben cerrar los puertos de administración en las máquinas virtuales

## <a name="12-monitor-and-log-the-configuration-and-traffic-of-vnets-subnets-and-nics"></a>1.2 Supervisión y registro de la configuración y el tráfico de redes virtuales, subredes y NIC

- Network Watcher debe estar habilitado

## <a name="13-protect-critical-web-applications"></a>1.3 Protección de las aplicaciones web críticas

- Asegúrese de que la aplicación web tenga la opción "Client Certificates (Incoming client certificates)" activada.
- CORS no debe permitir que todos los recursos accedan a las aplicaciones web
- CORS no debe permitir que todos los recursos accedan a Function App
- CORS no debería permitir que todos los recursos accedan a la aplicación de API.
- Se debe desactivar la depuración remota para las aplicaciones web
- Se debe desactivar la depuración remota para Function App
- Se debe desactivar la depuración remota para API Apps

## <a name="14-deny-communications-with-known-malicious-ip-addresses"></a>1.4 Denegación de las comunicaciones con direcciones IP malintencionadas conocidas

- Se debe habilitar DDoS Protection estándar
- El control de acceso de red Just-In-Time se debe aplicar en las máquinas virtuales.
- Las recomendaciones de protección de red adaptable se deben aplicar en las máquinas virtuales accesibles desde Internet

## <a name="15-record-network-packets-and-flow-logs"></a>1.5 Registro de los paquetes de red y registros de flujo

- Network Watcher debe estar habilitado

## <a name="111-use-automated-tools-to-monitor-network-resource-configurations-and-detect-changes"></a>1.11 Uso de herramientas automatizadas para supervisar las configuraciones de los recursos de red y detectar cambios

- se implementan los requisitos previos para auditar las configuraciones de las VM Windows en "Opciones de seguridad: Acceso a la red"
- Mostrar los resultados de la auditoría de las configuraciones de las máquinas virtuales Windows en "Opciones de seguridad - Cliente de redes de Microsoft"
- Implementar los requisitos previos para auditar las configuraciones de las máquinas virtuales Windows en "Opciones de seguridad - Seguridad de la red"
- Mostrar los resultados de la auditoría de las configuraciones de las máquinas virtuales Windows en "Opciones de seguridad - Seguridad de red"
- Implementar los requisitos previos para auditar las configuraciones de las máquinas virtuales Windows en "Opciones de seguridad - Servidor de red Microsoft"
- Mostrar los resultados de la auditoría de las configuraciones de las máquinas virtuales Windows en "Opciones de seguridad - Servidor de red Microsoft"
- Implementar los requisitos previos para auditar las configuraciones de las máquinas virtuales Windows en "Plantillas administrativas - Red"
- Mostrar los resultados de la auditoría de las configuraciones de las máquinas virtuales Windows en "Plantillas administrativas - Red"

## <a name="22-configure-central-security-log-management"></a>2.2 Configuración de la administración central de registros de seguridad

- El agente de Log Analytics debe instalarse en máquinas virtuales
- El agente de Log Analytics debe instalarse en Virtual Machine Scale Sets
- Implementar los requisitos previos para realizar una auditoría de las máquinas virtuales Windows en que el agente de Log Analytics no esté conectado según lo esperado
- Mostrar los resultados de la auditoría de las máquinas virtuales Windows en que el agente de Log Analytics no esté conectado según lo esperado
- El perfil de registro de Azure Monitor debe recopilar los registros de las categorías "write", "delete" y "action"
- Azure Monitor debe recopilar los registros de actividad de todas las regiones
- El aprovisionamiento automático del agente de supervisión de Log Analytics debe estar habilitado en su suscripción

## <a name="23-enable-audit-logging-for-azure-resources"></a>2.3 Habilitación del registro de auditoría para los recursos de Azure

- Los registros de diagnóstico de Azure Data Lake Store deben estar habilitados
- Los registros de diagnóstico de Logic Apps deben estar habilitados
- Los registros de diagnóstico de IoT Hub deben estar habilitados
- Se deben habilitar los registros de diagnóstico en las cuentas de Batch
- Los registros de diagnóstico de Virtual Machine Scale Sets deben estar habilitados
- Los registros de diagnóstico del centro de eventos deben estar habilitados
- Los registros de diagnóstico de los servicios de Search deben estar habilitados.
- Los registros de diagnóstico de App Services deben estar habilitados
- Los registros de diagnóstico de Data Lake Analytics deben estar habilitados
- Los registros de diagnóstico en Key Vault deben estar habilitados
- Los registros de diagnóstico de Service Bus deben estar habilitados
- Se deben habilitar los registros de diagnóstico en Azure Stream Analytics
- La auditoría de SQL Server debe estar habilitada
- Auditar la configuración de diagnóstico

## <a name="24-collect-security-logs-from-operating-systems"></a>2.4 Recopilación de registros de seguridad de sistemas operativos

- El aprovisionamiento automático del agente de supervisión de Log Analytics debe estar habilitado en su suscripción
- El agente de Log Analytics debe instalarse en máquinas virtuales
- El agente de Log Analytics debe instalarse en Virtual Machine Scale Sets
- Implementar los requisitos previos para realizar una auditoría de las máquinas virtuales Windows en que el agente de Log Analytics no esté conectado según lo esperado
- Mostrar los resultados de la auditoría de las máquinas virtuales Windows en que el agente de Log Analytics no esté conectado según lo esperado

## <a name="27-enable-alerts-for-anomalous-activity"></a>2.7 Habilitación de alertas para actividades anómalas

- Es necesario seleccionar el plan de tarifa estándar de Security Center.
- La seguridad avanzada de datos debe estar habilitada en los servidores SQL Server
- La opción Advanced Data Security debe estar habilitada en las instancias administradas de SQL
- Los tipos de Advanced Threat Protection deben estar establecidos en "Todos" en la configuración de Advanced Data Security de SQL Server
- Los tipos de Advanced Threat Protection deben estar establecidos en "Todos" en la configuración de Advanced Data Security de la instancia administrada de SQL Server

## <a name="28-centralize-anti-malware-logging"></a>2.8 Centralización del registro antimalware

- Microsoft Antimalware para Azure debe estar configurado para actualizar automáticamente las firmas de protección
- Supervisión de la falta de Endpoint Protection en Azure Security Center
- La solución de protección del punto de conexión debe instalarse en las máquinas virtuales

## <a name="31-maintain-an-inventory-of-administrative-accounts"></a>3.1 Mantenimiento de un inventario de cuentas administrativas

- Debe designar un máximo de tres propietarios para la suscripción
- Debe haber más de un propietario asignado a su suscripción
- Las cuentas externas con permisos de propietario deben quitarse de la suscripción
- Las cuentas en desuso con permisos de propietario deben quitarse de la suscripción

## <a name="33-use-dedicated-administrative-accounts"></a>3.3 Uso de cuentas administrativas dedicadas

- Implementar los requisitos previos para realizar una auditoría de las máquinas virtuales Windows en las que el grupo Administradores no contenga únicamente los miembros especificados
- Mostrar los resultados de la auditoría de las máquinas virtuales Windows en las que el grupo Administradores no contenga únicamente los miembros especificados
- Implementar los requisitos previos para auditar las máquinas virtuales Windows en las que el grupo Administradores no contenga todos los miembros especificados
- Mostrar los resultados de las auditorías de las máquinas virtuales Windows en las que el grupo Administradores no contenga todos los miembros especificados
- Implementar los requisitos previos para realizar una auditoría de las VM Windows en que el grupo Administradores contenga algún miembro especificado
- Mostrar los resultados de la auditoría de las máquinas virtuales Windows en las que el grupo Administradores contenga algún miembro especificado
- Debe designar un máximo de tres propietarios para la suscripción
- Debe haber más de un propietario asignado a su suscripción

## <a name="35-use-multi-factor-authentication-for-all-azure-active-directory-based-access"></a>3.5 Uso de la autenticación multifactor para todo el acceso basado en Azure Active Directory

- MFA debe estar habilitada en las cuentas con permisos de propietario en la suscripción
- MFA debe estar habilitada en las cuentas con permisos de escritura en la suscripción
- MFA debe estar habilitada en las cuentas con permisos de lectura en la suscripción

## <a name="37-log-and-alert-on-suspicious-activity-from-administrative-accounts"></a>3.7 Registro y alerta de la actividad sospechosa desde cuentas administrativas

- Es necesario seleccionar el plan de tarifa estándar de Security Center.

## <a name="39-use-azure-active-directory"></a>3.9 Uso de Azure Active Directory

- El administrador de Azure Active Directory debe aprovisionarse para servidores SQL Server
- Los clústeres de Service Fabric solo deben usar Azure Active Directory para la autenticación de cliente
- Asegúrese de que el registro con Azure Active Directory esté habilitado en la aplicación de API.
- Asegúrese de que el registro con Azure Active Directory esté habilitado en la aplicación web.
- Asegúrese de que el registro con Azure Active Directory esté habilitado en la aplicación de funciones.

## <a name="310-regularly-review-and-reconcile-user-access"></a>3.10 Revisión y conciliación de manera periódica del acceso de los usuarios

- Las cuentas en desuso deben quitarse de la suscripción
- Las cuentas en desuso con permisos de propietario deben quitarse de la suscripción
- Las cuentas externas con permisos de lectura deben quitarse de la suscripción
- Las cuentas externas con permisos de escritura deben quitarse de la suscripción
- Las cuentas externas con permisos de propietario deben quitarse de la suscripción

## <a name="41-maintain-an-inventory-of-sensitive-information"></a>4.1 Mantenimiento de un inventario de información confidencial

- Los datos confidenciales de las bases de datos SQL deben clasificarse

## <a name="44-encrypt-all-sensitive-information-in-transit"></a>4.4 Cifrado de toda la información confidencial en tránsito

- Se debe habilitar la transferencia segura a las cuentas de almacenamiento
- La versión más reciente de TLS debe usarse en la aplicación de API.
- La versión más reciente de TLS debe usarse en la aplicación web.
- La versión más reciente de TLS debe usarse en la aplicación de funciones.
- Acceso a Function App solo a través de HTTPS
- Acceso a la aplicación web solo a través de HTTPS
- Acceso a API App solo a través de HTTPS
- La aplicación de la conexión SSL debe estar habilitada para los servidores de bases de datos MySQL
- La aplicación de la conexión SSL debe estar habilitada para los servidores de bases de datos PostgreSQL
- Solo se deben habilitar las conexiones seguras a Redis Cache

## <a name="45-use-an-active-discovery-tool-to-identify-sensitive-data"></a>4.5 Uso de una herramienta de detección activa para identificar datos confidenciales

- Los datos confidenciales de las bases de datos SQL deben clasificarse
- La seguridad avanzada de datos debe estar habilitada en los servidores SQL Server
- La opción Advanced Data Security debe estar habilitada en las instancias administradas de SQL

## <a name="46-use-azure-rbac-to-control-access-to-resources"></a>4.6 Uso de RBAC de Azure para controlar el acceso a los recursos

- En Kubernetes Services se debe usar el control de acceso basado en rol (RBAC)
- Auditar el uso de reglas de RBAC personalizadas

## <a name="48-encrypt-sensitive-information-at-rest"></a>4.8 Cifrado en reposo de la información confidencial

- Se debe permitir el cifrado de datos transparente en bases de datos SQL
- El cifrado de discos debe aplicarse en máquinas virtuales
- Los discos sin asignar deben cifrarse
- El protector de TDE de SQL Server debe estar cifrado con su propia clave
- El protector de TDE de la instancia administrada SQL debe estar cifrado con su propia clave
- Las variables de cuenta de automatización deben cifrarse
- Se debe establecer la propiedad ClusterProtectionLevel en EncryptAndSign en los clústeres de Service Fabric

## <a name="49-log-and-alert-on-changes-to-critical-azure-resources"></a>4.9 Registro y alerta de cambios en los recursos críticos de Azure

- Azure Monitor debe recopilar los registros de actividad de todas las regiones

## <a name="51-run-automated-vulnerability-scanning-tools"></a>5.1 Ejecución de herramientas de análisis de vulnerabilidades automatizadas

- La evaluación de vulnerabilidades debe estar activada en sus servidores de SQL Server.
- La evaluación de vulnerabilidad debe estar habilitada en las instancias administradas de SQL.
- \[Versión preliminar\] La evaluación de vulnerabilidad debe estar habilitada en Virtual Machines.
- La configuración de Evaluación de vulnerabilidad para SQL Server debe contener una dirección de correo electrónico para recibir los informes de los exámenes

## <a name="52-deploy-automated-operating-system-patch-management-solution"></a>5.2 Implementación de una solución de administración de revisiones de sistema operativo automatizada

- Se deben instalar actualizaciones del sistema en las máquinas
- Se deben instalar las actualizaciones del sistema en los conjuntos de escalado de máquinas virtuales
- Asegúrese de que la versión de .NET Framework es la más reciente, si se usa como parte de la aplicación de funciones.
- Asegúrese de que la versión de .NET Framework es la más reciente, si se usa como parte de la aplicación web.
- Asegúrese de que la versión de .NET Framework es la más reciente, si se usa como parte de la aplicación de API.

## <a name="53-deploy-automated-third-party-software-patch-management-solution"></a>5.3 Implementación de una solución automatizada de administración de revisiones de software de terceros

- Asegúrese de que la versión de PHP es la más reciente si se usa como parte de la aplicación de API.
- Asegúrese de que la versión de PHP es la más reciente si se usa como parte de la aplicación web.
- Asegúrese de que la versión de PHP es la más reciente si se usa como parte de la aplicación de funciones.
- Asegúrese de que la versión de Java es la más reciente si se usa como parte de la aplicación web.
- Asegúrese de que la versión de Java es la más reciente si se usa como parte de la aplicación de funciones.
- Asegúrese de que la versión de Java es la más reciente si se usa como parte de la aplicación de API.
- Asegúrese de que la versión de Python es la más reciente si se usa como parte de la aplicación web.
- Asegúrese de que la versión de Python" es la más reciente si se usa como parte de la aplicación de funciones.
- Asegúrese de que la versión de Python es la más reciente si se usa como parte de la aplicación de API.
- Kubernetes Services se debe actualizar a una versión de Kubernetes no vulnerable.

## <a name="55-use-a-risk-rating-process-to-prioritize-the-remediation-of-discovered-vulnerabilities"></a>5.5 Use un proceso de clasificación de riesgos para priorizar la corrección de las vulnerabilidades detectadas

- Se deben corregir las vulnerabilidades mediante una solución de evaluación de vulnerabilidades
- Se deben corregir las vulnerabilidades en la configuración de seguridad en las máquinas
- Se deben corregir las vulnerabilidades en la configuración de seguridad de los conjuntos de escalado de máquinas virtuales
- Se deben corregir las vulnerabilidades en las configuraciones de seguridad de contenedor
- Se deben corregir las vulnerabilidades de las bases de datos SQL

## <a name="68-use-only-approved-applications"></a>6.8 Uso exclusivo de aplicaciones aprobadas

- Es necesario seleccionar el plan de tarifa estándar de Security Center.
- Deben habilitarse los controles de aplicaciones adaptables en las máquinas virtuales

## <a name="69-use-only-approved-azure-services"></a>6.9 Uso exclusivo de servicios de Azure aprobados

- Se deben migrar las máquinas virtuales a nuevos recursos de Azure Resource Manager
- Se deben migrar las cuentas de almacenamiento a los nuevos recursos de Azure Resource Manager

## <a name="610-implement-approved-application-list"></a>6.10 Implementación de la lista de aplicaciones aprobadas

- Es necesario seleccionar el plan de tarifa estándar de Security Center.
- Deben habilitarse los controles de aplicaciones adaptables en las máquinas virtuales

## <a name="73-maintain-secure-azure-resource-configurations"></a>7.3 Mantenimiento de configuraciones de recursos de Azure seguras

- \[Versión preliminar\]: Las directivas de seguridad de pod deben definirse en los servicios de Kubernetes

## <a name="74-maintain-secure-operating-system-configurations"></a>7.4 Mantenimiento de configuraciones seguras para los sistemas operativos

- Se deben corregir las vulnerabilidades en la configuración de seguridad en las máquinas
- Se deben corregir las vulnerabilidades en las configuraciones de seguridad de contenedor
- Se deben corregir las vulnerabilidades en la configuración de seguridad de los conjuntos de escalado de máquinas virtuales

## <a name="79-implement-automated-configuration-monitoring-for-azure-services"></a>7.9 Implementación de la supervisión de la configuración automatizada para los servicios de Azure

- \[Versión preliminar\]: Las directivas de seguridad de pod deben definirse en los servicios de Kubernetes

## <a name="710-implement-automated-configuration-monitoring-for-operating-systems"></a>7.10 Implementación de la supervisión de la configuración automatizada para los sistemas operativos

- Se deben corregir las vulnerabilidades en la configuración de seguridad en las máquinas
- Se deben corregir las vulnerabilidades en las configuraciones de seguridad de contenedor
- Se deben corregir las vulnerabilidades en la configuración de seguridad de los conjuntos de escalado de máquinas virtuales

## <a name="711-manage-azure-secrets-securely"></a>7.11 Administración segura de los secretos de Azure

- Los objetos del almacén de claves deben poder recuperarse

## <a name="712-manage-identities-securely-and-automatically"></a>7.12 Administración de identidades de forma segura y automática

- La identidad administrada debe usarse en la aplicación de funciones.
- La identidad administrada debe usarse en la aplicación web.
- La identidad administrada debe usarse en la aplicación de API.

## <a name="81-use-centrally-managed-anti-malware-software"></a>8.1 Uso de software antimalware administrado centralmente

- Supervisión de la falta de Endpoint Protection en Azure Security Center
- La solución de protección del punto de conexión debe instalarse en las máquinas virtuales

## <a name="82-pre-scan-files-to-be-uploaded-to-non-compute-azure-resources"></a>8.2 Análisis previo de los archivos que se van a cargar en recursos de Azure que no son de proceso

- Es necesario seleccionar el plan de tarifa estándar de Security Center.

## <a name="83-ensure-anti-malware-software-and-signatures-are-updated"></a>8.3 Garantía de la actualización del software y las firmas antimalware

- Microsoft Antimalware para Azure debe estar configurado para actualizar automáticamente las firmas de protección

## <a name="91-ensure-regular-automated-back-ups"></a>9.1 Garantía de copias de seguridad automáticas periódicas

- La copia de seguridad con redundancia geográfica a largo plazo debe estar habilitada para las instancias de Azure SQL Database.
- La copia de seguridad con redundancia geográfica debe estar habilitada para Azure Database for MySQL.
- La copia de seguridad con redundancia geográfica debe estar habilitada para Azure Database for PostgreSQL.
- La copia de seguridad con redundancia geográfica debe estar habilitada para Azure Database for MariaDB.
- Azure Backup debe estar habilitado para Virtual Machines.

## <a name="92-perform-complete-system-backups-and-backup-any-customer-managed-keys"></a>9.2 Realización de copias de seguridad completas del sistema y copia de seguridad de las claves administradas por el cliente

- La copia de seguridad con redundancia geográfica a largo plazo debe estar habilitada para las instancias de Azure SQL Database.
- La copia de seguridad con redundancia geográfica debe estar habilitada para Azure Database for MySQL.
- La copia de seguridad con redundancia geográfica debe estar habilitada para Azure Database for PostgreSQL.
- La copia de seguridad con redundancia geográfica debe estar habilitada para Azure Database for MariaDB.
- Azure Backup debe estar habilitado para Virtual Machines.

## <a name="94-ensure-protection-of-backups-and-customer-managed-keys"></a>9.4 Garantía de la protección de las copias de seguridad y las claves administradas del cliente

- Los objetos del almacén de claves deben poder recuperarse

## <a name="102-create-an-incident-scoring-and-prioritization-procedure"></a>10.2 Creación de un procedimiento de priorización y puntuación de incidentes

- Es necesario seleccionar el plan de tarifa estándar de Security Center.

## <a name="104-provide-security-incident-contact-details-and-configure-alert-notifications-for-security-incidents"></a>10.4 Provisión de detalles de contacto de incidentes de seguridad y configuración de notificaciones de alerta para incidentes de seguridad

- Es necesario proporcionar una dirección de correo electrónico de contacto de seguridad para la suscripción
- Es necesario proporcionar un número de teléfono de contacto de seguridad para la suscripción
- La configuración de seguridad de datos avanzada para SQL Server debe contener una dirección de correo electrónico para recibir alertas de seguridad
- La configuración de seguridad de datos avanzada para una instancia administrada SQL debe contener una dirección de correo electrónico para recibir alertas de seguridad
- Las notificaciones por correo electrónico para administradores y propietarios de suscripciones deben estar habilitadas en la configuración de seguridad avanzada de datos de SQL Server
- Las notificaciones a los administradores y a los propietarios de la suscripción deben estar habilitadas en la configuración de seguridad avanzada de datos de la instancia administrada de SQL

## <a name="next-steps"></a>Pasos siguientes

Ahora que ha revisado la asignación de controles del plano técnico de Azure Security Benchmark, visite Azure Policy en Azure Portal para asignar la iniciativa:

> [!div class="nextstepaction"]
> [Azure Policy](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyMenuBlade/Definitions)

Artículos adicionales sobre planos técnicos y cómo utilizarlos:

- Información acerca del [ciclo de vida del plano técnico](../../concepts/lifecycle.md).
- Descubra cómo utilizar [parámetros estáticos y dinámicos](../../concepts/parameters.md).
- Aprenda a personalizar el [orden de secuenciación de planos técnicos](../../concepts/sequencing-order.md).
- Averigüe cómo usar el [bloqueo de recursos de planos técnicos](../../concepts/resource-locking.md).
- Aprenda a [actualizar las asignaciones existentes](../../how-to/update-existing-assignments.md).
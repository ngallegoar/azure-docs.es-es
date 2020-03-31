---
title: 'Prácticas recomendadas de seguridad de base de datos: Microsoft Azure'
description: En este artículo se proporciona un conjunto de prácticas recomendadas para la reforzar la seguridad de las bases de datos de Azure.
services: security
documentationcenter: na
author: TerryLanfear
manager: barbkess
editor: tomsh
ms.assetid: ''
ms.service: security
ms.subservice: security-fundamentals
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/06/2019
ms.author: terrylan
ms.openlocfilehash: 0f2e0257c5bf855b0d9be61c43b68b4e30b3d80d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "80125103"
---
# <a name="azure-database-security-best-practices"></a>Procedimientos recomendados para la seguridad de las bases de datos de Azure
En este artículo se describen los procedimientos recomendados para la seguridad de base de datos.

Los procedimientos recomendados se basan en un consenso de opinión y son válidos para las funcionalidades y conjuntos de características actuales de la plataforma Azure. Como las opiniones y las tecnologías cambian con el tiempo, este artículo se actualiza de forma periódica para reflejar dichos cambios.

## <a name="secure-databases"></a>Seguridad de las bases de datos
La seguridad es un aspecto importante a la hora de administrar bases de datos, y siempre ha sido una prioridad para [Azure SQL Database](../../sql-database/index.yml). Las bases de datos pueden protegerse de forma estricta para ayudar a satisfacer la mayoría de los requisitos de seguridad o legales, como HIPAA, ISO 27001/27002 y PCI DSS nivel 1. En el [sitio del Centro de confianza de Microsoft](https://azure.microsoft.com/support/trust-center/services/) hay disponible una lista actualizada de certificaciones de cumplimiento de seguridad. También puede colocar las bases de datos en centros de datos de Azure específicos en función de los requisitos normativos.

## <a name="use-firewall-rules-to-restrict-database-access"></a>Uso de reglas de firewall para restringir el acceso de las bases de datos
Microsoft Azure SQL Database ofrece un servicio de base de datos relacional para Azure y otras aplicaciones basadas en Internet. Para proporcionar seguridad de acceso, SQL Database controla el acceso mediante:

- Reglas de firewall que limitan la conectividad mediante una dirección IP.
- Mecanismos de autenticación que solicitan a los usuarios sus identidades.
- Mecanismos de autorización que limitan los usuarios a acciones y datos específicos.

Los firewalls impiden todo acceso al servidor de bases de datos, excepto a aquellos equipos a los que haya concedido permiso. Asimismo, otorgan acceso a las bases de datos según la dirección IP de origen de cada solicitud.

La siguiente imagen muestra dónde se establece un firewall de servidor en SQL Database:

![Reglas de firewall](./media/database-best-practices/azure-database-security-best-practices-Fig1.png)

El servicio Azure SQL Database solo está disponible a través del puerto TCP 1433. Para obtener acceso a una instancia de SQL Database desde el equipo, asegúrese de que el firewall de equipos cliente permite la comunicación TCP saliente en el puerto TCP 1433. Bloquee las conexiones entrantes en el puerto TCP 1433 mediante reglas de firewall, si no necesita estas conexiones para otras aplicaciones.

Como parte del proceso de conexión, las conexiones de máquinas virtuales de Azure se redirigen a una dirección IP y un puerto que son únicos para cada rol de trabajo. El número de puerto está comprendido en el rango del 11000 al 11999. Para obtener más información sobre los puertos TCP, vea [Puertos más allá de 1433 para ADO.NET 4.5](../../sql-database/sql-database-develop-direct-route-ports-adonet-v12.md).

Para más información general acerca de las reglas de firewall de SQL Database, consulte [Introducción a las reglas de firewall de Azure SQL Database](../../sql-database/sql-database-firewall-configure.md).

> [!Note]
> Además de reglas de IP, el firewall administra reglas de red virtual. Las reglas de red virtual se basan en los puntos de conexión de servicio de red virtual. Es posible que las reglas de red virtual sean preferibles a las reglas de IP en algunos casos. Para obtener más información, vea [Uso de reglas y puntos de conexión de servicio de red virtual para Azure SQL Database y SQL Data Warehouse](../../sql-database/sql-database-vnet-service-endpoint-rule-overview.md).

## <a name="enable-database-authentication"></a>Habilitación de la autenticación de bases de datos
SQL Database admite dos tipos de autenticación: autenticación de SQL Server y autenticación de Azure AD.

### <a name="sql-server-authentication"></a>*Autenticación de SQL Server*

Entre las ventajas se incluyen las siguientes:

- Permite a SQL Database admitir entornos con sistemas operativos mixtos, donde todos los usuarios no son autenticados por un dominio de Windows.
- Permite a SQL Database admitir aplicaciones anteriores y otras proporcionadas por socios que requieren autenticación de SQL Server.
- Permite a los usuarios conectarse desde dominios desconocidos o que no son de confianza. Un ejemplo es una aplicación en la que los clientes establecidos se conectan con los inicios de sesión de SQL Server asignados para recibir el estado de sus pedidos.
- Permite a SQL Database admitir aplicaciones basadas en web en las que los usuarios crean sus propias identidades.
- Permite a los programadores de software distribuir sus aplicaciones mediante el uso de una compleja jerarquía de permisos basada en inicios de sesión de SQL Server conocidos y preestablecidos.

> [!NOTE]
> La autenticación de SQL Server no puede usar el protocolo de seguridad Kerberos.

Si usa autenticación de SQL Server, debe:

- Administrar las credenciales seguras usted mismo.
- Proteger las credenciales en la cadena de conexión.
- Proteger (potencialmente) las credenciales transmitidas a través de la red desde el servidor web a la base de datos. Para obtener más información, vea [Conectar con SQL Server mediante Autenticación de SQL en ASP.NET 2.0](/previous-versions/msp-n-p/ff648340(v=pandp.10)).

### <a name="azure-active-directory-ad-authentication"></a>*Autenticación de Azure Active Directory (AD)*
La autenticación de Azure AD es un mecanismo de conexión a Azure SQL Database y [SQL Data Warehouse](../../synapse-analytics/sql-data-warehouse/sql-data-warehouse-overview-what-is.md) mediante identidades de Azure AD. Con la autenticación de Azure AD, puede administrar las identidades de los usuarios de la base de datos y otros servicios de Microsoft en una ubicación central. La administración de identificadores central ofrece una ubicación única para administrar usuarios de base de datos y simplifica la administración de permisos.

> [!NOTE]
> Se recomienda el uso de la autenticación de Azure AD por encima de la autenticación de SQL Server.

Entre las ventajas se incluyen las siguientes:

- Ofrece una alternativa a la autenticación de SQL Server.
- Ayuda a detener la proliferación de identidades de usuario en los servidores de base de datos.
- Permite la rotación de contraseñas en un solo lugar.
- Los clientes pueden administrar los permisos de la base de datos con grupos externos (Azure AD).
- Puede eliminar el almacenamiento de contraseñas mediante la habilitación de la autenticación integrada de Windows y otras formas de autenticación compatibles con Azure Active Directory.
- Usa usuarios de base de datos independiente para autenticar identidades en el nivel de base de datos.
- Admite la autenticación basada en token para las aplicaciones que se conectan a SQL Database.
- Admite AD FS (federación de dominios) o autenticación nativa de usuario y contraseña para una instancia local de Azure Active Directory sin sincronización de dominios.
- Azure AD admite conexiones de SQL Server Management Studio que usan la autenticación universal de Active Directory, lo que incluye Multi-Factor Authentication. Multi-Factor Authentication incluye una sólida autenticación con una gama de opciones de comprobación: llamada de teléfono, mensaje de texto, tarjetas inteligentes con PIN o notificación de aplicación móvil. Para obtener más información, vea [Compatibilidad de SSMS con Azure AD MFA con SQL Database y SQL Data Warehouse](../../sql-database/sql-database-ssms-mfa-authentication.md).

En los pasos de configuración se incluyen los siguientes procedimientos para configurar y usar la autenticación de Azure AD:

- Cree y rellene una instancia de Azure AD.
- Opcional: asocie o cambie la instancia de Active Directory que esté asociada a la suscripción de Azure.
- Cree un administrador de Azure Active Directory para Azure SQL Database o [Azure SQL Data Warehouse](https://azure.microsoft.com/services/sql-data-warehouse/).
- Configure los equipos cliente.
- Cree usuarios de bases de datos independientes en la base de datos y asignados a identidades de Azure AD.
- Conéctese a la base de datos mediante identidades de Azure AD.

Puede encontrar información detallada en [Uso de la autenticación de Azure Active Directory para autenticación con SQL Database, Instancia administrada o SQL Data Warehouse](../../sql-database/sql-database-aad-authentication.md).

## <a name="protect-your-data-by-using-encryption-and-row-level-security"></a>Protección de los datos con cifrado y seguridad de nivel de fila
El [cifrado de datos transparente de Azure SQL Database](../../sql-database/transparent-data-encryption-azure-sql.md) ayuda a proteger los datos en disco y protege frente a accesos no autorizados al hardware. También realiza cifrado y descifrado de la base de datos en tiempo real, copias de seguridad asociadas y archivos de registro de transacciones en reposo sin necesidad de efectuar cambios en la aplicación. El cifrado de datos transparente se encarga de cifrar el almacenamiento de toda una base de datos mediante una clave simétrica denominada clave de cifrado de base de datos.

Incluso cuando se cifra el almacenamiento completo, es importante cifrar también la base de datos. Se trata de una implementación del enfoque defensivo en profundidad para la protección de los datos. Si usa Azure SQL Database y quiere proteger datos confidenciales (como números de tarjetas de crédito o de la seguridad social), puede cifrar las bases de datos con el cifrado AES de 256 bits validado por FIPS 140-2. Este cifrado cumple los requisitos de muchos estándares del sector (por ejemplo, HIPAA y PCI).

Los archivos relacionados con la [extensión del grupo de búferes (BPE)](https://docs.microsoft.com/sql/database-engine/configure-windows/buffer-pool-extension) no se cifran cuando se cifra una base de datos mediante cifrado de datos transparente. Debe usar herramientas de cifrado de nivel de sistema de archivos como [BitLocker](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc732774(v=ws.11)) o el [Sistema de cifrado de archivos (EFS)](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc749610(v%3dws.10)) para los archivos relacionados con la BPE.

Dado que un usuario autorizado, como un administrador de seguridad o de base de datos, puede acceder a los datos aun cuando la base de datos esté cifrada con cifrado de datos transparente, también se deben seguir estas recomendaciones:

- Habilite la autenticación de SQL Server en el nivel de base de datos.
- Use la autenticación de Azure AD mediante [roles RBAC](/azure/role-based-access-control/overview).
- Asegúrese de que los usuarios y las aplicaciones usen cuentas independientes para autenticarse. De este modo, puede limitar los permisos concedidos a usuarios y aplicaciones y reducir el riesgo de actividad malintencionada.
- Implemente seguridad de nivel de base de datos mediante roles fijos de base de datos (como db_datareader o db_datawriter). También puede crear roles personalizados para la aplicación a fin de conceder permisos explícitos a objetos de base de datos seleccionados.

Si desea conocer otras formas de proteger los datos, considere:

- [Cifrado de nivel de celda](/sql/relational-databases/security/encryption/encrypt-a-column-of-data) para cifrar columnas concretas, o incluso celdas de datos, con distintas claves de cifrado.
- [Always Encrypted](/sql/relational-databases/security/encryption/always-encrypted-database-engine), que permite a los clientes cifrar información confidencial dentro de aplicaciones cliente y no revelar las claves de cifrado al motor de base de datos (SQL Database o SQL Server). En consecuencia, Always Encrypted proporciona una separación entre quienes poseen los datos (y pueden verlos) y quienes administran los datos (pero no deben tener acceso a los mismos).
- [Seguridad de nivel de fila](/sql/relational-databases/security/row-level-security), que permite a los clientes controlar el acceso a las filas de una tabla de base de datos en función de las características del usuario que ejecuta una consulta. (Por ejemplo, la pertenencia a un grupo y el contexto de ejecución).

Las organizaciones que no usan el cifrado de nivel de base de datos pueden ser más susceptibles a ataques que ponen en peligro los datos ubicados en bases de datos SQL.

Para obtener más información sobre el cifrado de datos transparente de SQL Database, lea el artículo [Cifrado de datos transparente con Azure SQL Database](https://msdn.microsoft.com/library/0bf7e8ff-1416-4923-9c4c-49341e208c62.aspx).

## <a name="enable-database-auditing"></a>Habilitación de la auditoría de bases de datos
La auditoría de una instancia del motor de base de datos de SQL Server o una base de datos individual implica el seguimiento y el registro de eventos. En SQL Server, puede crear auditorías que contengan especificaciones para eventos de nivel de servidor y para eventos de nivel de base de datos. Los eventos auditados se pueden escribir en los registros de eventos o en los archivos de auditoría.

Hay varios niveles de auditoría de SQL Server, según los requisitos legales o de estándares para la instalación. La auditoría de SQL Server proporciona herramientas y procesos para habilitar, almacenar y ver auditorías en varios objetos de servidor y base de datos.

La [auditoría de Azure SQL Database](/azure/sql-database/sql-database-auditing) realiza un seguimiento de eventos de base de datos y los escribe en un registro de auditoría de la cuenta de Azure Storage.

La auditoría puede ayudarle a satisfacer el cumplimiento normativo, a comprender la actividad de las bases de datos y a detectar discrepancias y anomalías que podrían apuntar a problemas del negocio o infracciones de seguridad. La auditoría facilita la observancia de estándares de cumplimiento, pero no garantiza este.

Para obtener más información sobre la auditoría de bases de datos y cómo habilitarla, vea [Introducción a la auditoría de bases de datos SQL](/azure/sql-database/sql-database-auditing).

## <a name="enable-database-threat-detection"></a>Habilitación de la detección de amenazas para las bases de datos
La protección contra amenazas va más allá de la detección. La protección contra amenazas de base de datos incluye:

- Detectar y clasificar los datos más confidenciales, para poder protegerlos.
- Implementar configuraciones seguras en la base de datos para poder protegerla.
- Detectar amenazas potenciales a medida que se producen para responder a ellas y corregirlas rápidamente.

**Procedimiento recomendado**: detectar, clasificar y etiquetar los datos confidenciales de las bases de datos.   
**Detalle**: clasifique los datos de la base de datos SQL al habilitar [Detección y clasificación de datos](/azure/sql-database/sql-database-data-discovery-and-classification) en Azure SQL Database. Puede supervisar el acceso a los datos confidenciales en el panel de Azure o descargar informes.

**Procedimiento recomendado**: realizar un seguimiento de las vulnerabilidades de la base de datos para mejorar la seguridad de esta de manera proactiva.   
**Detalle**: use el servicio [Evaluación de vulnerabilidad](/azure/sql-database/sql-vulnerability-assessment) de Azure SQL Database, que busca posibles vulnerabilidades de la base de datos. El servicio emplea una base de conocimiento de reglas que marcan vulnerabilidades de seguridad y muestran las desviaciones de los procedimientos recomendados, como errores de configuración, permisos excesivos y datos confidenciales sin protección.

Las reglas se basan en los procedimientos recomendados de Microsoft y se centran en los problemas de seguridad que presentan mayores riesgos para la base de datos y sus valiosos datos. Tratan los problemas de nivel de base de datos y los problemas de seguridad de nivel de servidor, como la configuración del firewall de servidor y los permisos de nivel de servidor. Estas reglas también representan muchos de los requisitos de diferentes organismos reguladores para cumplir sus estándares de cumplimiento.

**Procedimiento recomendado**: habilitar detección de amenazas.  
**Detalle**: habilite [Detección de amenazas](/azure/sql-database/sql-database-threat-detection) de Azure SQL Database para obtener alertas de seguridad y recomendaciones sobre cómo investigar y mitigar las amenazas. Se obtienen alertas sobre actividades sospechosas en las bases de datos, posibles vulnerabilidades y ataques por inyección de código SQL, así como sobre patrones anómalos de acceso y consulta a las bases de datos.

[Advanced Threat Protection](/azure/sql-database/sql-advanced-threat-protection) es un paquete unificado de funcionalidades avanzadas de seguridad de SQL. Incluye los servicios mencionados anteriormente: Detección y clasificación de datos, Evaluación de vulnerabilidad y Detección de amenazas. Proporciona una ubicación única para habilitar y administrar estas funcionalidades.

La habilitación de estas funcionalidades ayuda a:

- Cumplir los requisitos de cumplimiento de normas y los estándares relacionados con la privacidad de datos.
- Controlar el acceso a las bases de datos y endurecer su seguridad.
- Supervisar un entorno de base de datos dinámico en el que es complicado realizar un seguimiento de los cambios.
- Detectar posibles amenazas y responder a ellas.

Además, la detección de amenazas integra las alertas con Azure Security Center para obtener una vista centralizada del estado de seguridad de todos los recursos de Azure.

## <a name="next-steps"></a>Pasos siguientes
Vea [Patrones y procedimientos recomendados de seguridad en Azure](best-practices-and-patterns.md) para obtener más procedimientos recomendados de seguridad que pueda aplicar cuando diseñe, implemente y administre las soluciones en la nube mediante Azure.

En los siguientes recursos se ofrece más información general sobre la seguridad de Azure y los servicios de Microsoft relacionados:
* [Blog del equipo de seguridad de Azure](https://blogs.msdn.microsoft.com/azuresecurity/): ofrece información actualizada sobre lo último en seguridad de Azure
* [Microsoft Security Response Center](https://technet.microsoft.com/library/dn440717.aspx): aquí podrá notificar vulnerabilidades de seguridad de Microsoft, incluidos problemas con Azure, o también mediante correo electrónico a secure@microsoft.com.

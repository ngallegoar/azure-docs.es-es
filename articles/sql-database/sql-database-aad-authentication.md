---
title: Azure Active Directory
description: Obtenga información sobre cómo usar Azure Active Directory para la autenticación con SQL Database, Instancia administrada y Azure Synapse Analytics
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.custom: azure-synapse
ms.devlang: ''
ms.topic: conceptual
author: GithubMirek
ms.author: mireks
ms.reviewer: vanto, carlrab
ms.date: 03/27/2020
ms.openlocfilehash: 58f40bc7406048df2b052bea799bcbf6466fbbae
ms.sourcegitcommit: 7581df526837b1484de136cf6ae1560c21bf7e73
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/31/2020
ms.locfileid: "80421102"
---
# <a name="use-azure-active-directory-authentication-for-authentication-with-sql"></a>Usar la autenticación de Azure Active Directory para autenticación con SQL

La autenticación de Azure Active Directory es un mecanismo de conexión a Azure [SQL Database](sql-database-technical-overview.md), la [instancia administrada](sql-database-managed-instance.md) y [Azure Synapse Analytics (conocido anteriormente como Azure SQL Data Warehouse)](../synapse-analytics/sql-data-warehouse/sql-data-warehouse-overview-what-is.md) mediante identidades de Azure Active Directory (Azure AD). 

> [!NOTE]
> Este artículo se aplica a servidor SQL Azure y a las instancias de SQL Database y Azure Synapse. Para simplificar, se usa SQL Database cuando se hace referencia tanto a SQL Database como a Azure Synapse.

Con la autenticación de Azure AD puede administrar centralmente las identidades de los usuarios de la base de datos y otros servicios de Microsoft en una ubicación central. La administración de identificadores central ofrece una ubicación única para administrar usuarios de base de datos y simplifica la administración de permisos. Entre las ventajas se incluyen las siguientes:

- Ofrece una alternativa a la autenticación de SQL Server.
- Ayuda a detener la proliferación de identidades de usuario en los servidores de base de datos.
- Permite la rotación de contraseñas en un solo lugar
- Los clientes pueden administrar los permisos de la base de datos con grupos externos (Azure AD).
- Puede eliminar el almacenamiento de contraseñas mediante la habilitación de la autenticación integrada de Windows y otras formas de autenticación compatibles con Azure Active Directory.
- La autenticación de Azure AD utiliza usuarios de base de datos independiente para autenticar las identidades en el nivel de base de datos.
- Azure AD admite la autenticación basada en token para las aplicaciones que se conectan a SQL Database.
- La autenticación de Azure AD admite lo siguiente:
  - Identidades solo en la nube de Azure AD
  - Identidades híbridas de Azure AD que admiten:
    - Autenticación en la nube con dos opciones asociadas al inicio de sesión único de conexión directa (SSO): Autenticación de **paso a través** y **hash de contraseña**
    - Autenticación federada
  - Para obtener más información acerca de los métodos de autenticación de Azure AD y cuál de ellos elegir, consulte el siguiente artículo:
    - [Seleccione el método de autenticación adecuado para su solución de identidad híbrida de Azure Active Directory](../active-directory/hybrid/choose-ad-authn.md)
- Azure AD admite conexiones de SQL Server Management Studio que usan la autenticación universal de Active Directory, lo cual incluye Multi-Factor Authentication (MFA).  MFA incluye una sólida autenticación con una gama de sencillas opciones de comprobación: llamada de teléfono, mensaje de texto, tarjetas inteligentes con PIN o notificación de aplicación móvil. Para obtener más información, consulte [Compatibilidad de SSMS con Azure AD MFA con SQL Database y Azure Synapse](sql-database-ssms-mfa-authentication.md).
- Azure AD admite conexiones similares desde SQL Server Data Tools (SSDT) que usan la autenticación interactiva de Active Directory. Para obtener más información, consulte [Compatibilidad de Azure Active Directory con SQL Server Data Tools (SSDT)](/sql/ssdt/azure-active-directory).

> [!NOTE]  
> La conexión a una instancia de SQL Server que se ejecute en una máquina virtual de Azure no se admite si se usa una cuenta de Azure Active Directory. Utilice en su lugar una cuenta de Active Directory del dominio.  

En los pasos de configuración se incluyen los siguientes procedimientos para configurar y usar la autenticación de Azure Active Directory.

1. Cree y rellene una instancia de Azure AD.
2. Opcional: asocie o cambie la instancia de Active Directory que esté asociada a la suscripción de Azure.
3. Cree un administrador de Azure Active Directory para el servidor de Azure SQL Database, la instancia administrada o [Azure Synapse](https://azure.microsoft.com/services/sql-data-warehouse/).
4. Configure los equipos cliente.
5. Cree usuarios de bases de datos independientes en la base de datos y asignados a identidades de Azure AD.
6. Conéctese a la base de datos mediante identidades de Azure AD.

> [!NOTE]
> Para aprender a crear y rellenar Azure AD y, posteriormente, configurarlo con Azure SQL Database, la instancia administrada y Azure Synapse, consulte [Configuración de Azure AD con Azure SQL Database](sql-database-aad-authentication-configure.md).

## <a name="trust-architecture"></a>Arquitectura de confianza

- Para admitir la contraseña de usuario nativa de Azure AD, se considera solo la parte de la nube y Azure SQL Database o Azure AD.
- Para admitir las credenciales de inicio de sesión único de Windows (o el usuario y contraseña para las credenciales de Windows), use las credenciales de Azure Active Directory desde un dominio federado o administrado que esté configurado para el inicio de sesión único de conexión directa con autenticación de paso a través y hash de contraseña. Para más información, consulte [Inicio de sesión único de conexión directa de Azure Active Directory](../active-directory/hybrid/how-to-connect-sso.md).
- Para admitir la autenticación federada (o el usuario o contraseña para las credenciales de Windows), se requiere la comunicación con el bloque ADFS.

Para obtener más información sobre las identidades híbridas de Azure AD, la configuración y la sincronización, consulte los siguientes artículos:

- Autenticación de hash de contraseñas: [Implementación de la sincronización de hash de contraseñas con la sincronización de Azure AD Connect](../active-directory/hybrid/how-to-connect-password-hash-synchronization.md).
- Autenticación de paso a través: [Autenticación de paso a través de Azure Active Directory](../active-directory/hybrid/how-to-connect-pta-quick-start.md).
- Autenticación federada: [Implementación de Servicios de federación de Active Directory en Azure](/windows-server/identity/ad-fs/deployment/how-to-connect-fed-azure-adfs) y [Azure AD Connect y la federación](../active-directory/hybrid/how-to-connect-fed-whatis.md).

Para obtener una autenticación federada de ejemplo con la infraestructura de ADFS (o el usuario y contraseña para las credenciales de Windows), consulte el diagrama siguiente. Las flechas indican las rutas de comunicación.

![diagrama de autenticación de aad][1]

En el diagrama siguiente se indica la federación, la confianza y las relaciones de hospedaje que permiten que un cliente se conecte a una base de datos mediante el envío de un token que una instancia de Azure AD autenticó y que es de confianza para la base de datos. El cliente 1 puede representar una instancia de Azure Active Directory con usuarios nativos o una instancia de AD con usuarios federados. El cliente 2 representa una posible solución, incluidos los usuarios importados; en este ejemplo proceden de una instancia de Azure Active Directory federada con ADFS sincronizado con Azure Active Directory. Es importante comprender que el acceso a una base de datos mediante la autenticación de Azure AD requiere que la suscripción de hospedaje esté asociada a Azure AD. Debe utilizarse la misma suscripción para crear el servidor SQL Server que hospeda Azure SQL Database o Azure Synapse.

![relación de suscripción][2]

## <a name="administrator-structure"></a>Estructura del administrador

Cuando se usa la autenticación de Azure AD, existen dos cuentas de administrador para el servidor de SQL Database y la instancia administrada: el administrador del servidor SQL Server original y el administrador de Azure AD. Los mismos conceptos se aplican a Azure Synapse. Solo el administrador basado en una cuenta de Azure AD puede crear el primer usuario de base de datos independiente en Azure AD en una base de datos de usuario. El inicio de sesión del administrador de Azure AD puede ser un usuario de Azure AD o un grupo de Azure AD. Cuando el administrador es una cuenta de grupo, cualquier miembro del grupo lo puede usar, lo que permite varios administradores de Azure AD para la instancia de SQL Server. Mediante el uso de la cuenta de grupo como un administrador, se mejora la administración al permitir agregar y quitar miembros de grupo de forma centralizada en Azure AD sin cambiar los usuarios ni los permisos de SQL Database. Solo un administrador de Azure AD (un usuario o grupo) se puede configurar en cualquier momento.

![estructura de administración][3]

## <a name="permissions"></a>Permisos

Para crear nuevos usuarios, debe tener el permiso `ALTER ANY USER` en la base de datos. El permiso `ALTER ANY USER` se puede conceder a cualquier usuario de la base de datos. Las cuentas de administrador del servidor también disponen del permiso `ALTER ANY USER`, así como los usuarios de la base de datos con los permisos `CONTROL ON DATABASE` o `ALTER ON DATABASE` para esa base de datos y los miembros del rol de la base de datos `db_owner`.

Para crear un usuario de base de datos independiente en Azure SQL Database, la instancia administrada o Azure Synapse, debe conectarse a la base de datos con una identidad de Azure AD. Para crear el primer usuario de base de datos independiente , debe conectarse a la base de datos con un administrador de Azure AD (que es el propietario de la base de datos). Esto se muestra en [Configuración y administración de la autenticación de Azure Active Directory con SQL Database o Azure Synapse](sql-database-aad-authentication-configure.md). La autenticación de Azure AD solo es posible si se creó el administrador de Azure AD para la instancia de Azure SQL Database o Azure Synapse. Si se ha quitado el administrador de Azure Active Directory desde el servidor, los usuarios de Azure Active Directory creados previamente en SQL Server ya no podrán conectarse a la base de datos utilizando sus credenciales de Azure Active Directory.

## <a name="azure-ad-features-and-limitations"></a>Características y limitaciones de Azure AD

- Los siguientes miembros de Azure AD se pueden aprovisionar en el servidor de Azure SQL Server o Azure Synapse:

  - Miembros nativos: un miembro creado en Azure AD en el dominio administrado o en un dominio personalizado. Para más información, consulte [Incorporación de su nombre de dominio personalizado a Azure Active Directory](../active-directory/active-directory-domains-add-azure-portal.md).
  - Los miembros de un dominio de Active Directory federado con Azure Active Directory en un dominio administrado configurado para el inicio de sesión único de conexión directa mediante la autenticación de paso a través o de hash de contraseña. Para obtener más información, consulte [Microsoft Azure ya admite la federación con Windows Server Active Directory](https://azure.microsoft.com/blog/windows-azure-now-supports-federation-with-windows-server-active-directory//) e [Inicio de sesión único de conexión directa mediante Azure Active Directory](../active-directory/hybrid/how-to-connect-sso.md).
  - Miembros importados de otros directorios de Azure AD que son miembros nativos o miembros de dominio federado.
  - Grupos de Active Directory creados como grupos de seguridad.

- Los usuarios de Azure AD que forman parte de un grupo que tenga el rol de servidor `db_owner` no pueden usar la sintaxis **[CREATE DATABASE SCOPED CREDENTIAL](/sql/t-sql/statements/create-database-scoped-credential-transact-sql)** en Azure SQL Database y Azure Synapse. Verá este error:

    `SQL Error [2760] [S0001]: The specified schema name 'user@mydomain.com' either does not exist or you do not have permission to use it.`

    Conceda el rol `db_owner` directamente al usuario de Azure AD para mitigar el problema de **CREATE DATABASE SCOPED CREDENTIAL**.

- Estas funciones del sistema devuelven valores NULL cuando se ejecutan en las entidades de seguridad de Azure AD:

  - `SUSER_ID()`
  - `SUSER_NAME(<admin ID>)`
  - `SUSER_SNAME(<admin SID>)`
  - `SUSER_ID(<admin name>)`
  - `SUSER_SID(<admin name>)`

### <a name="managed-instances"></a>Instancias administradas

- Se admiten las entidades de seguridad (inicios de sesión) del servidor y los usuarios de Azure AD como característica en vista previa para las [instancias administradas](sql-database-managed-instance.md).
- Sin embargo, no se admite el establecimiento de dichas entidades (inicios de sesión) de Azure AD asignadas a un grupo de Azure AD como propietarias de la base de datos en [instancias administradas](sql-database-managed-instance.md).
    - Una ampliación de este escenario es que cuando se agrega un grupo como parte del rol de servidor `dbcreator`, los usuarios de este grupo pueden conectarse a la instancia administrada y crear bases de datos, pero no podrán acceder a ellas. El motivo es que el propietario de la nueva base de datos es SA y no el usuario de Azure AD. Este problema no se manifiesta si el usuario se agrega al rol de servidor `dbcreator`.
- Se admite la administración del agente SQL y las ejecuciones de los trabajos en las entidades de seguridad (inicios de sesión) de un servidor de Azure AD.
- Las entidades de seguridad (inicios de sesión) de un servidor de Azure AD pueden ejecutar operaciones de copia de seguridad y restauración de bases de datos.
- Se admite la auditoría de todas las instrucciones relacionadas con las entidades de seguridad (inicios de sesión) de un servidor de Azure AD y los eventos de autenticación.
- Se admite la conexión de administrador dedicada para las entidades de seguridad (inicios de sesión) de un servidor de Azure AD que son miembros del rol del servidor de administrador del sistema.
    - Se admite mediante la utilidad SQLCMD y SQL Server Management Studio.
- Se admiten desencadenadores de inicio de sesión para los eventos de inicio de sesión procedentes de las entidades de seguridad (inicios de sesión) de un servidor de Azure AD.
- Se puede configurar Service Broker y el correo electrónico de la base de datos mediante una entidad de seguridad (inicio de sesión) de un servidor de Azure AD.


## <a name="connecting-using-azure-ad-identities"></a>Conexión mediante identidades de Azure AD

La autenticación de Azure Active Directory admite los siguientes métodos de conexión a una base de datos mediante identidades de Azure AD:

- Contraseña de Azure Active Directory
- Azure Active Directory integrado
- Azure Active Directory Universal con MFA
- Mediante la autenticación de token de aplicación

Se admiten los siguientes métodos de autenticación para las entidades de seguridad (inicios de sesión) de un servidor de Azure AD:

- Contraseña de Azure Active Directory
- Azure Active Directory integrado
- Azure Active Directory Universal con MFA


### <a name="additional-considerations"></a>Consideraciones adicionales

- Para mejorar la capacidad de administración, se recomienda que aprovisione un grupo dedicado de Azure AD como administrador.   
- Solo se puede configurar un administrador de Azure AD (un usuario o grupo) en un servidor de Azure SQL Database o Azure Synapse al mismo tiempo.
  - La adición de entidades de seguridad (inicios de sesión) de un servidor de Azure AD para instancias administradas ofrece la posibilidad de crear varias de estas entidades que se pueden agregar al rol `sysadmin`.
- Inicialmente, solo un administrador de Azure AD para SQL Server puede conectarse al servidor de Azure SQL Database, la instancia administrada o Azure Synapse con una cuenta de Azure Active Directory. El administrador de Active Directory puede configurar los usuarios de la base de datos de Azure AD sucesivos.   
- Se recomienda establecer el tiempo de espera de conexión a 30 segundos.   
- SQL Server 2016 Management Studio y SQL Server Data Tools para Visual Studio 2015 (versión 14.0.60311.1 abril de 2016 o posterior) admiten la autenticación de Azure Active Directory. (La autenticación de Azure AD es compatible con el **proveedor de datos .NET Framework para SqlServer**; al menos la versión 4.6 de .NET Framework). Por lo tanto, las versiones más recientes de estas herramientas y aplicaciones de capa de datos (DAC y .BACPAC) pueden usar la autenticación de Azure AD.   
- A partir de la versión 15.0.1, la [utilidad sqlcmd](/sql/tools/sqlcmd-utility) y la [utilidad bcp](/sql/tools/bcp-utility) admiten la autenticación interactiva de Active Directory con MFA.
- SQL Server Data Tools para Visual Studio 2015 requiere al menos la versión de abril de 2016 de Data Tools (versión 14.0.60311.1). Actualmente, los usuarios de Azure AD no se muestran en el explorador de objetos de SSDT. Como solución alternativa, vea los usuarios de [sys.database_principals](https://msdn.microsoft.com/library/ms187328.aspx).   
- [Microsoft JDBC Driver 6.0 para SQL Server](https://www.microsoft.com/download/details.aspx?id=11774) es compatible con la autenticación de Azure AD. Consulte también [Configurar las propiedades de conexión](https://msdn.microsoft.com/library/ms378988.aspx).   
- PolyBase no se puede autenticar mediante la autenticación de Azure AD.   
- La autenticación de Azure AD se admite para SQL Database con las hojas **Importar base de datos** y **Exportar base de datos de Azure Portal**. La importación y exportación mediante la autenticación de Azure AD también se admite desde el comando de PowerShell.   
- La autenticación de Azure AD se admite para SQL Database, la instancia administrada y Azure Synapse mediante la CLI. Para obtener más información, consulte [Configuración y administración de la autenticación de Azure Active Directory con SQL Database o Azure Synapse](sql-database-aad-authentication-configure.md) y [SQL Server - az sql server](https://docs.microsoft.com/cli/azure/sql/server).

## <a name="next-steps"></a>Pasos siguientes

- Para aprender a crear y rellenar Azure AD y, posteriormente, configurarlo con Azure SQL Database o Azure Synapse, consulte [Configuración y administración de la autenticación de Azure Active Directory con SQL Database, la instancia administrada o Azure Synapse](sql-database-aad-authentication-configure.md).
- Para ver un tutorial sobre el uso de entidades de seguridad (inicios de sesión) de un servidor de Azure AD con instancias administradas, consulte [Entidades de seguridad (inicios de sesión) de un servidor de Azure AD con instancias administradas](sql-database-managed-instance-aad-security-tutorial.md).
- Para obtener información general de los inicios de sesión, usuarios, roles de base de datos y permisos de SQL Database, consulte [Inicios de sesión, usuarios, roles de base de datos y permisos](sql-database-manage-logins.md).
- Para más información acerca de las entidades de seguridad de bases de datos, consulte [Entidades de seguridad](https://msdn.microsoft.com/library/ms181127.aspx).
- Para más información acerca de los roles de base de datos, consulte [Roles de nivel de base de datos](https://msdn.microsoft.com/library/ms189121.aspx).
- Para conocer la sintaxis de creación de entidades de seguridad (inicios de sesión) de un servidor de Azure AD para instancias administradas, consulte [CREATE LOGIN](/sql/t-sql/statements/create-login-transact-sql?view=azuresqldb-mi-current).
- Para más información general acerca de las reglas de firewall de SQL Database, consulte [Introducción a las reglas de firewall de Azure SQL Database](sql-database-firewall-configure.md).

<!--Image references-->

[1]: ./media/sql-database-aad-authentication/1aad-auth-diagram.png
[2]: ./media/sql-database-aad-authentication/2subscription-relationship.png
[3]: ./media/sql-database-aad-authentication/3admin-structure.png
[4]: ./media/sql-database-aad-authentication/4select-subscription.png
[5]: ./media/sql-database-aad-authentication/5ad-settings-portal.png
[6]: ./media/sql-database-aad-authentication/6edit-directory-select.png
[7]: ./media/sql-database-aad-authentication/7edit-directory-confirm.png
[8]: ./media/sql-database-aad-authentication/8choose-ad.png
[9]: ./media/sql-database-aad-authentication/9ad-settings.png
[10]: ./media/sql-database-aad-authentication/10choose-admin.png
[11]: ./media/sql-database-aad-authentication/11connect-using-int-auth.png
[12]: ./media/sql-database-aad-authentication/12connect-using-pw-auth.png
[13]: ./media/sql-database-aad-authentication/13connect-to-db.png

---
title: Autorización del acceso a servidores y bases de datos a través de inicios de sesión y cuentas de usuario
description: Obtenga información sobre cómo Azure SQL Database y Azure Synapse Analytics autentican el acceso de los usuarios por medio de inicios de sesión y cuentas de usuario. También aprenderá a usar roles de base de datos y permisos explícitos para autorizar a los inicios de sesión y a los usuarios para que puedan realizar acciones y consultar datos.
keywords: seguridad de la Base de datos SQL, administración de seguridad de la base de datos, seguridad de inicio de sesión, seguridad de la base de datos, acceso a la base de datos
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: VanMSFT
ms.author: vanto
ms.reviewer: carlrab
ms.date: 03/23/2020
ms.openlocfilehash: 98c15fe11b64e8c177e60a2ea1eb7c50eaf69353
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "80124802"
---
# <a name="authorizing-database-access-to-authenticated-users-to-sql-database-and-azure-synapse-analytics-using-logins-and-user-accounts"></a>Autorización del acceso a bases de datos para usuarios autenticados en SQL Database y Azure Synapse Analytics a través de inicios de sesión y cuentas de usuario

En este artículo, aprenderá lo siguiente:

- Opciones para configurar Azure SQL Database y Azure Synapse Analytics (antes, Azure SQL Data Warehouse) para permitir a los usuarios realizar tareas administrativas y acceder a los datos almacenados en esas bases de datos
- Cómo configurar el acceso y la autorización después de crear inicialmente una base de datos de Azure SQL Database
- Cómo agregar inicios de sesión y cuentas de usuario a la base de datos maestra y a las cuentas de usuario y, tras ello, conceder permisos administrativos a esas cuentas
- Cómo agregar cuentas de usuario a las bases de datos de usuario, estén asociadas a inicios de sesión o sean cuentas de usuario independientes
- Configurar cuentas de usuario con permisos en bases de datos de usuario usando roles de base de datos y permisos explícitos

> [!IMPORTANT]
> Por motivos de simplicidad, durante el resto de este artículo nos referiremos a las bases de datos de Azure SQL Database y de Azure Synapse Analytics (antes, Azure SQL Data Warehouse) de manera colectiva como "bases de datos" o como "Azure SQL".

## <a name="authentication-and-authorization"></a>Autenticación y autorización

Por [**autenticación**](sql-database-security-overview.md#authentication) se entiende el proceso según el cual se demuestra que el usuario es quien dice ser. Un usuario se conecta a una base de datos a través de una cuenta de usuario.
Cuando un usuario intenta conectarse a una base de datos, proporciona una cuenta de usuario y la información de autenticación. El usuario se autentica con uno de los dos métodos de autenticación siguientes:

- [Autenticación SQL](https://docs.microsoft.com/sql/relational-databases/security/choose-an-authentication-mode#connecting-through-sql-server-authentication).

  Con este método de autenticación, el usuario envía un nombre de cuenta de usuario y una contraseña asociada para establecer una conexión. Esta contraseña se almacena en la base de datos maestra de cuentas de usuario vinculadas a un inicio de sesión, o bien en la base de datos que contiene las cuentas de usuario no vinculadas a un inicio de sesión.
- [Autenticación con Azure Active Directory](sql-database-aad-authentication.md)

  Con este método de autenticación, el usuario envía un nombre de cuenta de usuario y solicita que el servicio use la información de credenciales que hay almacenada en Azure Active Directory.

**Inicios de sesión y usuarios**: En Azure SQL, una cuenta de usuario en una base de datos puede estar asociada a un inicio de sesión que está almacenado en la base de datos maestra, o bien puede ser un nombre de usuario que está almacenado en una base de datos individual.

- Un **inicio de sesión** es una cuenta individual en la base de datos maestra que se puede vincular a una cuenta de usuario en una o más bases de datos. Con un inicio de sesión, la información de credenciales de la cuenta de usuario se almacena en el propio inicio de sesión.
- Una **cuenta de usuario** es una cuenta individual en una base de datos que puede estar vinculada a un inicio de sesión, si bien esto no es obligatorio. En el caso de una cuenta de usuario que no está vinculada a un inicio de sesión, la información de las credenciales se almacena con la cuenta de usuario.

La [**autorización**](sql-database-security-overview.md#authorization) para acceder a los datos y realizar diversas acciones se administran con roles de base de datos y permisos explícitos. El término autorización hace referencia a los permisos asignados a un usuario, y determina qué puede hacer ese usuario. La autorización se controla por medio de las [pertenencias a roles](https://docs.microsoft.com/sql/relational-databases/security/authentication-access/database-level-roles) y los [permisos de nivel de objeto](https://docs.microsoft.com/sql/relational-databases/security/permissions-database-engine) de la base de datos de cada cuenta de usuario. Como procedimiento recomendado, debe conceder a los usuarios los privilegios mínimos necesarios.

## <a name="existing-logins-and-user-accounts-after-creating-a-new-database"></a>Inicios de sesión y cuentas de usuario existentes después de crear una base de datos

Al crear la primera implementación de Azure SQL, hay que especificar un inicio de sesión de administrador y una contraseña asociada a ese inicio de sesión. Esta cuenta administrativa se denomina **administrador del servidor**. Durante la implementación, se configuran los siguientes inicios de sesión y usuarios en las bases de datos maestra y de usuarios:

- Se crea un inicio de sesión de SQL con privilegios administrativos usando el nombre de inicio de sesión especificado. Un [inicio de sesión](https://docs.microsoft.com/sql/relational-databases/security/authentication-access/principals-database-engine#sa-login) es una cuenta de usuario individual para iniciar sesión en SQL Database.
- A este inicio de sesión se le conceden permisos administrativos completos en todas las bases de datos, como una [entidad de seguridad de nivel de servidor](https://docs.microsoft.com/sql/relational-databases/security/authentication-access/principals-database-engine). Este inicio de sesión tiene todos los permisos disponibles en SQL Database y no se puede restringir. En una instancia administrada, este inicio de sesión se agrega al [rol fijo de servidor sysadmin](https://docs.microsoft.com/sql/relational-databases/security/authentication-access/server-level-roles) (este rol no existe en las bases de datos únicas o agrupadas).
- Se crea una [cuenta de usuario](https://docs.microsoft.com/sql/relational-databases/security/authentication-access/getting-started-with-database-engine-permissions#database-users) denominada `dbo` para este inicio de sesión en cada base de datos de usuarios. El usuario [dbo](https://docs.microsoft.com/sql/relational-databases/security/authentication-access/principals-database-engine) posee todos los permisos de base de datos en la base de datos y tiene asignado el rol fijo de base de datos `db_owner`. Más adelante en este artículo hablaremos de otros roles fijos de base de datos.

Para saber cuáles son las cuentas de administrador de una base de datos, abra Azure Portal y vaya a la pestaña **Propiedades** de su servidor o instancia administrada.

![Administradores de SQL Server](media/sql-database-manage-logins/sql-admins.png)

![Administradores de SQL Server](media/sql-database-manage-logins/sql-admins2.png)

> [!IMPORTANT]
> Una vez creado, el nombre de inicio de sesión de administrador no se puede cambiar. Para restablecer la contraseña del administrador del servidor lógico, vaya a [Azure Portal](https://portal.azure.com), haga clic en **Servidores SQL Server**, seleccione el servidor en la lista y haga clic en **Restablecer contraseña**. Para restablecer la contraseña de un servidor de instancia administrada, vaya a Azure Portal, haga clic en la instancia y, después, en **Restablecer contraseña**. También puede usar PowerShell o la CLI de Azure.

## <a name="create-additional-logins-and-users-having-administrative-permissions"></a>Creación de inicios de sesión y usuarios adicionales con permisos administrativos

En este momento, la instancia de Azure SQL está configurada para el acceso a través de un único inicio de sesión de SQL y una cuenta de usuario. Si quiere crear más inicios de sesión con permisos administrativos completos o parciales, dispone de las siguientes opciones (en función del modo de implementación):

- **Crear una cuenta de administrador de Azure Active Directory que tenga permisos administrativos completos**

  Habilite la autenticación de Azure Active Directory y cree un inicio de sesión de administrador de Azure AD. Una cuenta de Azure Active Directory se puede configurar como administrador de la implementación de SQL Database con permisos administrativos completos. Puede tratarse de una cuenta individual o una cuenta de un grupo de seguridad. Si quiere usar cuentas de Azure AD para conectarse a SQL Database, **debe** haber un administrador de Azure AD configurado. Para más información sobre cómo habilitar la autenticación de Azure AD de todos los tipos de implementación de SQL Database, vea los siguientes artículos:

  - [Usar la autenticación de Azure Active Directory para autenticación con SQL](sql-database-aad-authentication.md)
  - [Configuración y administración de la autenticación de Azure Active Directory con SQL](sql-database-aad-authentication-configure.md)

- **En una implementación de instancia administrada, crear inicios de sesión de SQL con permisos administrativos completos**

  - Cree un inicio de sesión de SQL Server adicional en la instancia administrada.
  - Agregue el inicio de sesión al [rol fijo de servidor sysadmin](https://docs.microsoft.com/sql/relational-databases/security/authentication-access/server-level-roles) usando la instrucción [ALTER SERVER ROLE](https://docs.microsoft.com/sql/t-sql/statements/alter-server-role-transact-sql). Este inicio de sesión tendrá permisos administrativos completos.
  - También puede crear un [inicio de sesión de Azure AD](sql-database-aad-authentication-configure.md?tabs=azure-powershell#new-azure-ad-admin-functionality-for-mi) usando la sintaxis <a href="/sql/t-sql/statements/create-login-transact-sql?view=azuresqldb-mi-current">CREATE LOGIN</a>.

- **En una implementación de base de datos única o agrupada, crear inicios de sesión de SQL con permisos administrativos restringidos**

  - Cree un inicio de sesión de SQL adicional en la base de datos maestra para una implementación de base de datos única o agrupada, o bien para una implementación de instancia administrada.
  - Cree una cuenta de usuario en la base de datos maestra asociada a ese nuevo inicio de sesión.
  - Agregue la cuenta de usuario al rol `dbmanager`, al rol `loginmanager` o a ambos en la base de datos `master` usando la instrucción [ALTER SERVER ROLE](https://docs.microsoft.com/sql/t-sql/statements/alter-server-role-transact-sql) (en Azure Synapse Analytics, use la instrucción [sp_addrolemember](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-addrolemember-transact-sql)).

  > [!NOTE]
  > Los roles `dbmanager` y `loginmanager` **no** pertenecen a las implementaciones de instancia administrada.

  Los miembros de estos [roles especiales de base de datos maestra](https://docs.microsoft.com/sql/relational-databases/security/authentication-access/database-level-roles#special-roles-for--and-) de las bases de datos únicas o agrupadas confieren autoridad a los usuarios para crear y administrar bases de datos, o para crear y administrar inicios de sesión. En el caso de las bases de datos creadas por un usuario que es miembro del rol `dbmanager`, el miembro se asigna al rol fijo de base de datos `db_owner`, y puede iniciar sesión en la base de datos y administrarla usando la cuenta de usuario `dbo`. Estos roles no tienen permisos explícitos fuera de la base de datos maestra.

  > [!IMPORTANT]
  > En una base de datos única o agrupada no se pueden crear inicios de sesión de SQL adicionales con permisos administrativos completos.

## <a name="create-accounts-for-non-administrator-users"></a>Creación de cuentas para usuarios que no son administradores

Se pueden crear cuentas para los usuarios no administrativos recurriendo a uno de estos dos métodos:

- **Crear un inicio de sesión**

  Cree un inicio de sesión de SQL en la base de datos maestra. Luego, cree una cuenta de usuario en cada base de datos a la que el usuario necesite tener acceso y asocie la cuenta de usuario a ese inicio de sesión. Este método es preferible cuando el usuario debe acceder a varias bases de datos y se quieren mantener todas las contraseñas sincronizadas, pero plantea algunas dificultades cuando se usa con la replicación geográfica, ya que hay que crear el inicio de sesión tanto en el servidor principal como en los secundarios. Para más información, vea [Configuración y administración de la seguridad de Azure SQL Database para la restauración geográfica o la conmutación por error](sql-database-geo-replication-security-config.md).
- **Crear una cuenta de usuario**

  Cree una cuenta de usuario en la base de datos a la que un usuario necesite tener acceso (esto se conoce también como [usuario independiente](https://docs.microsoft.com/sql/relational-databases/security/contained-database-users-making-your-database-portable)).

  - Con una base de datos única o agrupada, siempre se puede crear este tipo de cuentas de usuario.
  - Con una base de datos de instancia administrada que no admite [entidades de seguridad de Azure AD Server](sql-database-aad-authentication-configure.md?tabs=azure-powershell#create-contained-database-users-in-your-database-mapped-to-azure-ad-identities), este tipo de cuentas de usuario solo se puede crear en una [base de datos independiente](https://docs.microsoft.com/sql/relational-databases/databases/contained-databases). Con una instancia administrada que admite [entidades de seguridad de Azure AD Server](sql-database-aad-authentication-configure.md?tabs=azure-powershell#create-contained-database-users-in-your-database-mapped-to-azure-ad-identities), se pueden crear cuentas de usuario para autenticarse en esa instancia administrada sin necesidad de que los usuarios de la base de datos deban crearse como usuarios de base de datos independientes.

  Con este método, la información de autenticación del usuario se almacena en cada base de datos y se replica automáticamente en las bases de datos con replicación geográfica, pero si la misma cuenta existe en varias bases de datos y se usa la autenticación de SQL, deberá sincronizar las contraseñas manualmente. Además, si un usuario tiene una cuenta en varias bases de datos con distintas contraseñas, recordarlas puede suponer un problema.

> [!IMPORTANT]
> Para crear usuarios independientes asignados a identidades de Azure AD, debe iniciar sesión con una cuenta de Azure AD que sea administrador en SQL Database. En una instancia administrada, un inicio de sesión de SQL con permisos `sysadmin` también puede crear un usuario o inicio de sesión de Azure AD.

Para obtener ejemplos que reflejan cómo crear inicios de sesión y usuarios, vea:

- [Creación de un inicio de sesión para bases de datos únicas o agrupadas](https://docs.microsoft.com/sql/t-sql/statements/create-login-transact-sql?view=azuresqldb-current#examples-1)
- [Creación de un inicio de sesión para una base de datos de instancia administrada](https://docs.microsoft.com/sql/t-sql/statements/create-login-transact-sql?view=azuresqldb-mi-current#examples-2)
- [Creación de un inicio de sesión para una base de datos de Azure Synapse Analytics](https://docs.microsoft.com/sql/t-sql/statements/create-login-transact-sql?view=azure-sqldw-latest#examples-3)
- [Crear usuario](https://docs.microsoft.com/sql/t-sql/statements/create-user-transact-sql#examples)
- [Creación de usuarios independientes de Azure AD](sql-database-aad-authentication-configure.md#create-contained-database-users-in-your-database-mapped-to-azure-ad-identities)

> [!TIP]
> Para ver un tutorial de seguridad que incluye cómo crear un usuario independiente de SQL Server en una base de datos única o agrupada, vea [Tutorial: Protección de una base de datos única o agrupada](sql-database-security-tutorial.md).

## <a name="using-fixed-and-custom-database-roles"></a>Uso de roles de base de datos fijos y personalizados

Después de crear una cuenta de usuario en una base de datos, ya sea basada en un inicio de sesión o para un usuario independiente, puede autorizar a ese usuario para realizar diversas acciones y acceder a los datos de una base de datos determinada. Se pueden usar los siguientes métodos para autorizar el acceso:

- **Roles fijos de base de datos**

  Agregue la cuenta de usuario a un [rol fijo de base de datos](https://docs.microsoft.com/sql/relational-databases/security/authentication-access/database-level-roles). Existen nueve roles fijos de base de datos, cada uno con un conjunto definido de permisos. Los roles fijos de base de datos más comunes son: **db_owner**, **db_ddladmin**, **db_datawriter**, **db_datareader**, **db_denydatawriter** y **db_denydatareader**. **db_owner** se usa habitualmente para conceder permiso completo solo a algunos usuarios. Los restantes roles fijos de base de datos son útiles para obtener rápidamente una base de datos simple en la fase de desarrollo, pero no se recomiendan para la mayoría de las bases de datos de producción. Por ejemplo, el rol fijo de base de datos **db_datareader** concede acceso de lectura a todas las tablas de la base de datos, lo que es más de lo estrictamente necesario.

  - Para agregar un usuario a un rol fijo de base de datos:

    - En Azure SQL Database, use la instrucción [ALTER ROLE](https://docs.microsoft.com/sql/t-sql/statements/alter-role-transact-sql). Para obtener ejemplos, vea [Ejemplos de ALTER ROLE](https://docs.microsoft.com/sql/t-sql/statements/alter-role-transact-sql#examples).
    - En Azure Synapse Analytics, use la instrucción [sp_addrolemember](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-addrolemember-transact-sql). Para obtener ejemplos, vea [Ejemplos de sp_addrolemember](https://docs.microsoft.com/sql/t-sql/statements/alter-role-transact-sql).

- **Rol de base de datos personalizado**

  Cree un rol de base de datos personalizado usando la instrucción [CREATE ROLE](https://docs.microsoft.com/sql/t-sql/statements/create-role-transact-sql). Un rol personalizado permite crear sus propios roles de base de datos definidos por el usuario y otorgar minuciosamente a cada rol los permisos mínimos necesarios para la necesidad empresarial. Después, puede agregar usuarios a ese rol personalizado. Cuando un usuario es miembro de varios roles, realiza la agregación de los permisos de todos ellos.
- **Conceder permisos directamente**

  Conceda [permisos](https://docs.microsoft.com/sql/relational-databases/security/permissions-database-engine) directamente a la cuenta de usuario. En SQL Database, hay más de 100 permisos que pueden conceder o denegar individualmente. Muchos de estos permisos están anidados. Por ejemplo, el permiso `UPDATE` de un esquema incluye el permiso `UPDATE` en cada tabla dentro de dicho esquema. Al igual que en la mayoría de los sistemas de permisos, la denegación de un permiso anula su concesión. Dada la naturaleza anidada y el número de permisos, es preciso realizar un estudio meticuloso para diseñar un sistema de permisos apropiado para proteger adecuadamente la base de datos. Empiece con la lista de permisos de [Permisos (motor de base de datos)](https://docs.microsoft.com/sql/relational-databases/security/permissions-database-engine) y revise el [gráfico a tamaño de póster](https://docs.microsoft.com/sql/relational-databases/security/media/database-engine-permissions.png) de los permisos.

## <a name="using-groups"></a>Uso de grupos

En una administración eficaz del acceso, en vez de los permisos asignados a usuarios individuales, se usan los permisos asignados a grupos de seguridad de Active Directory, así como roles fijos o personalizados.

- Si se usa la autenticación de Azure Active Directory, incluya a los usuarios de Azure Active Directory en un grupo de seguridad de Azure Active Directory. Cree un usuario de base de datos independiente para el grupo. Asigne a uno o varios usuarios de base de datos un rol de base de datos personalizado con permisos específicos adecuados para ese grupo de usuarios.

- Si se usa la autenticación de SQL, cree usuarios de base de datos independientes en la base de datos. Asigne a uno o varios usuarios de base de datos un rol de base de datos personalizado con permisos específicos adecuados para ese grupo de usuarios.

  > [!NOTE]
  > También se pueden usar grupos para usuarios de bases de datos no independientes.

Debe familiarizarse con las siguientes características que pueden utilizarse para limitar o elevar los permisos:

- La [suplantación](https://docs.microsoft.com/dotnet/framework/data/adonet/sql/customizing-permissions-with-impersonation-in-sql-server) y la [firma de módulos](https://docs.microsoft.com/dotnet/framework/data/adonet/sql/signing-stored-procedures-in-sql-server) se pueden usar para elevar los permisos temporalmente de forma segura.
- [seguridad del nivel de fila](https://docs.microsoft.com/sql/relational-databases/security/row-level-security) permite filtrar las filas que puede ver el usuario.
- [enmascaramiento de datos](sql-database-dynamic-data-masking-get-started.md) puede utilizarse para limitar la exposición de información confidencial.
- [procedimientos almacenados](https://docs.microsoft.com/sql/relational-databases/stored-procedures/stored-procedures-database-engine) puede utilizarse para limitar las acciones que se pueden realizar en la base de datos.

## <a name="next-steps"></a>Pasos siguientes

Para una información general de las características de seguridad de SQL Database, consulte [SQL security overview](sql-database-security-overview.md) (Información general acerca de la seguridad de SQL).

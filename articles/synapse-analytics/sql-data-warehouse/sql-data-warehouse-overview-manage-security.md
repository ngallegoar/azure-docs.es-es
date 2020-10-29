---
title: Protección de bases de datos
description: Sugerencias para proteger una base de datos y desarrollar soluciones en el recurso de un grupo de SQL de Synapse.
author: julieMSFT
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: sql-dw
ms.date: 04/17/2018
ms.author: jrasnick
ms.reviewer: igorstan
ms.custom: seo-lt-2019
tags: azure-synapse
ms.openlocfilehash: c94924c973a1095a4bebf6231d9853968facc1b2
ms.sourcegitcommit: 59f506857abb1ed3328fda34d37800b55159c91d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/24/2020
ms.locfileid: "92516890"
---
# <a name="secure-a-database-in-azure-synapse"></a>Protección de una base de datos en Azure Synapse

> [!div class="op_single_selector"]
>
> * [Información general sobre seguridad](sql-data-warehouse-overview-manage-security.md)
> * [Autenticación](sql-data-warehouse-authentication.md)
> * [Cifrado (Portal)](sql-data-warehouse-encryption-tde.md)
> * [Cifrado (T-SQL)](sql-data-warehouse-encryption-tde-tsql.md)

Este artículo le guiará a través de los aspectos básicos de la protección del grupo de SQL de Synapse. En concreto, este artículo le ayuda a empezar a trabajar con los recursos para limitar el acceso, proteger los datos y supervisar las actividades en una base de datos aprovisionada mediante un grupo de SQL.

## <a name="connection-security"></a>Seguridad de conexión

Seguridad de conexión hace referencia a cómo restringir y proteger las conexiones a la base de datos mediante reglas de firewall y cifrado de las conexiones.

Las reglas de firewall las usan tanto el [servidor lógico con SQL Server](../../azure-sql/database/logical-servers.md) como sus bases de datos para rechazar los intentos de conexión desde direcciones IP que no se hayan aprobado explícitamente. Para permitir conexiones desde la dirección IP pública de la máquina cliente o de la aplicación, primero debe crear una regla de firewall de nivel de servidor mediante Azure Portal, la API de REST o PowerShell.

Como procedimiento recomendado, debe restringir los intervalos de direcciones IP que se permite que atraviesen el firewall de nivel de servidor tanto como sea posible.  Para acceder al grupo de SQL desde su equipo local, asegúrese de que el firewall de su red y del equipo local permita la comunicación de salida en el puerto TCP 1433.  

Azure Synapse Analytics usa las reglas de firewall de IP de nivel de servidor. No es compatible con las de nivel de base de datos. Para más información, consulte [Reglas de firewall de Azure SQL Database](../../azure-sql/database/firewall-configure.md?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json).

Las conexiones al grupo de SQL están cifradas de forma predeterminada.  Se pasa por alto la modificación de la configuración de conexión para deshabilitar el cifrado.

## <a name="authentication"></a>Authentication

La autenticación indica a cómo demostrar su identidad al conectarse a la base de datos. Actualmente, el grupo de SQL admite la autenticación de SQL Server con un nombre de usuario y una contraseña, y con Azure Active Directory.

Al crear el servidor de la base de datos, especificó un inicio de sesión de "administrador de servidor" con un nombre de usuario y una contraseña. Con estas credenciales, puede autenticarse en cualquier base de datos de ese servidor como propietario, o "dbo" a través de la autenticación en SQL Server.

Sin embargo, como práctica recomendada, los usuarios de su organización deben usar una cuenta diferente para autenticarse. De esta manera puede limitar los permisos concedidos a la aplicación y reducir los riesgos de actividad malintencionada en caso de que el código de aplicación sea vulnerable a ataques de inyección SQL.

Para crear un usuario autenticado de SQL Server, conéctese a la base de datos **maestra** en el servidor con su inicio de sesión de administrador de servidor y cree un nuevo inicio de sesión de servidor.  Es una buena idea crear también un usuario en la base de datos maestra. La creación de un usuario en la base de datos maestra posibilita el inicio de sesión mediante herramientas como SSMS sin especificar ningún nombre de base de datos.  También permite el uso del Explorador de objetos para ver todas las bases de datos en un servidor.

```sql
-- Connect to master database and create a login
CREATE LOGIN ApplicationLogin WITH PASSWORD = 'Str0ng_password';
CREATE USER ApplicationUser FOR LOGIN ApplicationLogin;
```

A continuación, conéctese a la **base de datos del grupo de SQL** con el inicio de sesión de administrador de servidor y cree un usuario de base de datos basado en el inicio de sesión de servidor que creó.

```sql
-- Connect to the database and create a database user
CREATE USER ApplicationUser FOR LOGIN ApplicationLogin;
```

Para dar permiso a un usuario para realizar operaciones adicionales, como la creación de inicios de sesión o de nuevas bases de datos, también necesitará que se le asignen los roles `Loginmanager` y `dbmanager` en la base de datos maestra.

Para obtener más información sobre estos roles adicionales y la autenticación en una instancia de SQL Database, consulte [Administración de bases de datos e inicios de sesión en Azure SQL Database](../../azure-sql/database/logins-create-manage.md?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json).  Para más información sobre la conexión mediante Azure Active Directory, consulte [Conexión mediante autenticación de Azure Active Directory](sql-data-warehouse-authentication.md).

## <a name="authorization"></a>Authorization

La autorización se refiere a lo que puede hacer en una base de datos una vez que se haya autenticado y conectado. Los privilegios de autorización se determinan mediante los permisos y pertenencias a roles. Como procedimiento recomendado, debe conceder a los usuarios los privilegios mínimos necesarios. Para administrar roles, puede usar los siguientes procedimientos almacenados:

```sql
EXEC sp_addrolemember 'db_datareader', 'ApplicationUser'; -- allows ApplicationUser to read data
EXEC sp_addrolemember 'db_datawriter', 'ApplicationUser'; -- allows ApplicationUser to write data
```

La cuenta de administrador de servidor con la que se está conectando forma parte de db_owner, que tiene autoridad para realizar cualquier acción en la base de datos. Guarde esta cuenta para implementar las actualizaciones de los esquemas y otras operaciones de administración. Utilice la cuenta "ApplicationUser" con permisos más limitados para conectarse desde la aplicación a la base de datos con los privilegios mínimos que necesita la aplicación.

Existen varias formas de limitar aún más lo que los usuarios pueden hacer en la base de datos:

* Los [permisos](/sql/relational-databases/security/permissions-database-engine?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest) pormenorizados permiten controlar qué operaciones se pueden realizar en columnas individuales, tablas, vistas, esquemas, procedimientos y otros objetos de la base de datos. Use los permisos granulares para tener el máximo control y conceder los permisos mínimos necesarios.
* Los [roles de base de datos](/sql/relational-databases/security/authentication-access/database-level-roles?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest) que no sean db_datareader y db_datawriter se pueden usar para crear cuentas de usuario de aplicación más eficaces o cuentas de administración menos eficaces. Los roles d base de datos fijos integrados proporcionan una manera fácil de conceder permisos, pero pueden dar lugar a la concesión de más permisos que son necesarios.
* [procedimientos almacenados](/sql/relational-databases/stored-procedures/stored-procedures-database-engine?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest) puede utilizarse para limitar las acciones que se pueden realizar en la base de datos.

En el ejemplo siguiente, se concede acceso de lectura a un esquema definido por el usuario.

```sql
--CREATE SCHEMA Test
GRANT SELECT ON SCHEMA::Test to ApplicationUser
```

La administración de bases de datos y servidores lógicos desde Azure Portal o mediante la API de Azure Resource Manager la controlan las asignaciones de roles de su cuenta de usuario del portal. Para más información, consulte [Incorporación o eliminación de asignaciones de roles mediante Azure Portal](../../role-based-access-control/role-assignments-portal.md?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json).

## <a name="encryption"></a>Cifrado

El Cifrado de datos transparente (TDE) ayuda en la protección frente a las amenazas de actividad malintencionada al realizar el cifrado y el descifrado de los datos en reposo. Cuando se cifra la base de datos, los archivos de registro de copias de seguridad y transacciones asociados se cifran sin necesidad de realizar ningún cambio en las aplicaciones. TDE cifra el almacenamiento de una base de datos completa mediante el uso de una clave simétrica denominada clave de cifrado de base de datos.

En SQL Database, la clave de cifrado de base de datos está protegida por un certificado de servidor integrado. El certificado de servidor integrado es único para cada servidor. Microsoft gira automáticamente estos certificados al menos cada 90 días. El algoritmo de cifrado que se usa es AES-256. Para ver una descripción general de TDE, consulte [Cifrado de datos transparente (TDE)](/sql/relational-databases/security/encryption/transparent-data-encryption?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest).

Puede cifrar la base de datos mediante [Azure Portal](sql-data-warehouse-encryption-tde.md) o [T-SQL](sql-data-warehouse-encryption-tde-tsql.md).

## <a name="next-steps"></a>Pasos siguientes

Para ver detalles y ejemplos sobre la conexión del almacenamiento con diferentes protocolos, consulte [Conexión a un grupo de SQL](../sql/connect-overview.md).

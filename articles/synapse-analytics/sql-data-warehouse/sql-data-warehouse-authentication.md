---
title: Autenticación de un grupo de SQL dedicado (anteriormente SQL DW)
description: Obtenga información sobre la autenticación en el grupo de SQL dedicado (anteriormente SQL DW) en Azure Synapse Analytics mediante la autenticación de Azure Active Directory (Azure AD) o SQL Server.
services: synapse-analytics
author: julieMSFT
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: sql-dw
ms.date: 04/02/2019
ms.author: jrasnick
ms.reviewer: igorstan
ms.custom: seo-lt-2019
tag: azure-synapse
ms.openlocfilehash: b2b5ca024046c5bc46fff756c55688d3ff0cfea1
ms.sourcegitcommit: 6a350f39e2f04500ecb7235f5d88682eb4910ae8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/01/2020
ms.locfileid: "96451963"
---
# <a name="authenticate-to-dedicated-sql-pool-formerly-sql-dw-in-azure-synapse-analytics"></a>Autenticación en un grupo de SQL dedicado (anteriormente SQL DW) en Azure Synapse Analytics

Obtenga información sobre la autenticación en el grupo de SQL dedicado (anteriormente SQL DW) en Azure Synapse mediante la autenticación de Azure Active Directory (Azure AD) o SQL Server.

Para conectarse a un grupo de SQL dedicado (anteriormente SQL DW), debe transmitir las credenciales de seguridad para realizar la autenticación. Después de establecer una conexión, determinados valores de conexión se configuran como parte del establecimiento de la sesión de la consulta.  

Para más información sobre la seguridad y cómo habilitar las conexiones con el grupo de SQL dedicado (anteriormente SQL DW), consulte la [documentación sobre cómo proteger una base de datos](sql-data-warehouse-overview-manage-security.md).

## <a name="sql-authentication"></a>Autenticación SQL

Para conectarse al grupo de SQL dedicado (anteriormente SQL DW), debe proporcionar la información siguiente:

* Nombre de servidor completo
* Especificar la autenticación de SQL
* Nombre de usuario
* Contraseña
* Base de datos predeterminada (opcional)

De forma predeterminada, la conexión se realiza a la base de datos *maestra* y no a la base de datos de usuario. Para conectarse a la base de datos de usuario puede hacer dos cosas:

* Especificar la base de datos predeterminada al registrar el servidor con el Explorador de objetos de SQL Server en SSDT, SSMS o en la cadena de conexión de la aplicación. Por ejemplo, incluya el parámetro InitialCatalog para una conexión ODBC.
* Resalte la base de datos de usuario antes de crear una sesión en SSDT.

> [!NOTE]
> La instrucción **USE MyDatabase;** de Transact-SQL no se admite para cambiar la base de datos de una conexión. Puede encontrar instrucciones sobre cómo conectarse a un grupo de SQL con SSDT en el artículo [Consulta con Visual Studio](sql-data-warehouse-query-visual-studio.md).

## <a name="azure-active-directory-authentication"></a>Autenticación con Azure Active Directory

La autenticación de [Azure Active Directory](../../active-directory/fundamentals/active-directory-whatis.md?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json) es un mecanismo que permite conectar con el grupo de SQL mediante identidades de Azure Active Directory (Azure AD). Con la autenticación de Azure Active Directory puede administrar centralmente las identidades de los usuarios de la base de datos y otros servicios de Microsoft en una ubicación central. La administración de id. central ofrece un lugar único para administrar los usuarios del grupo de SQL dedicado (anteriormente SQL DW) y simplifica la administración de permisos.

### <a name="benefits"></a>Ventajas

Entre las ventajas de Azure Active Directory, se incluyen:

* Proporciona una alternativa a la autenticación de SQL Server.
* Ayuda a detener la proliferación de identidades de usuario en los servidores.
* Permite la rotación de contraseñas en un solo lugar
* Administra los permisos de la base de datos con grupos externos (Azure AD).
* Elimina el almacenamiento de contraseñas mediante la habilitación de la autenticación integrada de Windows y otras formas de autenticación compatibles con Azure Active Directory.
* Usa usuarios de base de datos independiente para autenticar las identidades en el nivel de base de datos.
* Admite la autenticación basada en tokens para las aplicaciones que se conectan al grupo de SQL.
* Admite Multi-Factor Authentication mediante autenticación universal de Active Directory para varias herramientas, incluidas [SQL Server Management Studio](../../azure-sql/database/authentication-mfa-ssms-overview.md?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json) y [SQL Server Data Tools](/sql/ssdt/azure-active-directory?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest).

> [!NOTE]
> Azure Active Directory todavía es relativamente nuevo y tiene algunas limitaciones. Para asegurarse de que Azure Active Directory sea una buena elección para su entorno, consulte [Características y limitaciones de Azure AD](../../azure-sql/database/authentication-aad-overview.md?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json#azure-ad-features-and-limitations), específicamente las consideraciones adicionales.

### <a name="configuration-steps"></a>Pasos de configuración

Siga estos pasos para configurar la autenticación de Azure Active Directory.

1. Crear y rellenar un Azure Active Directory.
2. Opcional: Asociar o cambiar la instancia de Active Directory que esté asociada a la suscripción de Azure.
3. Crear un administrador de Azure Active Directory para Azure Synapse.
4. Configurar los equipos cliente.
5. Crear usuarios de base de datos independiente  en la base de datos y asignados a identidades de Azure AD.
6. Conectarse al grupo de SQL mediante identidades de Azure AD.

Actualmente los usuarios de Azure Active Directory no se muestran en el Explorador de objetos de SSDT. Como solución alternativa, vea los usuarios de [sys.database_principals](/sql/relational-databases/system-catalog-views/sys-database-principals-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest).

### <a name="find-the-details"></a>Búsqueda de los detalles

* Los pasos para configurar y usar la autenticación de Azure Active Directory son casi idénticos para Azure SQL Database y Synapse SQL en Azure Synapse. Siga los pasos detallados del tema [Conexión a SQL Database o al grupo de SQL mediante la autenticación de Azure Active Directory](../../azure-sql/database/authentication-aad-overview.md?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json).
* Cree roles de base de datos personalizados y agrégueles usuarios. A continuación, conceda permisos específicos a los roles. Para obtener más información, consulte [Introducción a los permisos de los motores de bases de datos](/sql/relational-databases/security/authentication-access/getting-started-with-database-engine-permissions?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest).

## <a name="next-steps"></a>Pasos siguientes

Para empezar a realizar consultas con Visual Studio u otras aplicaciones, consulte [Consulta con Visual Studio](sql-data-warehouse-query-visual-studio.md).

---
title: Conexión a guías de inicio rápido y consultas
description: Guías de inicio rápido de Azure SQL Database que muestran cómo conectarse a una base de datos de Azure SQL y consultarla.
services: sql-database
ms.service: sql-database
ms.subservice: service
ms.custom: ''
ms.devlang: ''
ms.topic: quickstart
author: stevestein
ms.author: sstein
ms.reviewer: ''
ms.date: 12/18/2018
ms.openlocfilehash: 05ba54c10cc693f81115f364d7a400e2275a3875
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/29/2020
ms.locfileid: "80528231"
---
# <a name="quickstarts-azure-sql-database-connect-and-query"></a>Guías de inicio rápido: Conexión y consulta de Azure SQL Database

En el documento siguiente se incluyen vínculos a ejemplos de Azure que muestran cómo conectarse a una base de datos de Azure SQL y realizar consultas en ella. También se proporcionan algunas recomendaciones de seguridad de nivel de transporte.

## <a name="quickstarts"></a>Guías de inicio rápido

| |  |
|---|---|
|[SQL Server Management Studio](sql-database-connect-query-ssms.md)|Este inicio rápido muestra cómo usar SSMS para conectarse a una base de datos de Azure SQL, para después usar las instrucciones Transact-SQL para consultar, insertar, actualizar y eliminar datos en la base de datos.|
|[Azure Data Studio](https://docs.microsoft.com/sql/azure-data-studio/quickstart-sql-database?toc=/azure/sql-database/toc.json)|Este inicio rápido muestra cómo usar Azure Data Studio para conectarse a una base de datos de Azure SQL y luego usar instrucciones de Transact-SQL (T-SQL) para crear el elemento TutorialDB empleado en los tutoriales de Azure Data Studio.|
|[Azure Portal](sql-database-connect-query-portal.md)|Esta guía de inicio rápido muestra cómo usar el Editor de consultas para conectarse a una base de datos SQL y, después, usar instrucciones Transact-SQL para realizar consultas, insertar datos, actualizarlos y eliminarlos de la base de datos.|
|[Visual Studio Code](sql-database-connect-query-vscode.md)|Esta guía de inicio rápido muestra cómo usar Visual Studio Code para conectarse a una base de datos de Azure SQL y después usar las instrucciones Transact-SQL para consultar, insertar, actualizar y eliminar datos en la base de datos.|
|[.NET con Visual Studio](sql-database-connect-query-dotnet-visual-studio.md)|Esta guía de inicio rápido muestra cómo se usa .NET Framework para crear un programa de C# con Visual Studio que se conecte a una base de datos de Azure SQL y que use instrucciones Transact-SQL para consultar los datos.|
|[.NET Core](sql-database-connect-query-dotnet-core.md)|Esta guía de inicio rápido muestra cómo se usa .NET Core en Windows, Linux o macOS para crear un programa de C# que se conecte a una base de datos de Azure SQL y que use instrucciones Transact-SQL para consultar los datos.|
|[Go](sql-database-connect-query-go.md)|En esta guía de inicio rápido se muestra cómo usar Go para conectarse a una base de datos de Azure SQL. También se muestran las instrucciones Transact-SQL para consultar y modificar los datos.|
|[Java](sql-database-connect-query-java.md)|Esta guía de inicio rápido muestra cómo utilizar Java para conectarse a una base de datos de Azure SQL y luego utilizar instrucciones Transact-SQL para consultar los datos.|
|[Node.js](sql-database-connect-query-nodejs.md)|En esta guía de inicio rápido se muestra cómo se usa Node.js para crear un programa que se conecte a una base de datos de Azure SQL y que use instrucciones Transact-SQL para consultar los datos.|
|[PHP](sql-database-connect-query-php.md)|En esta guía de inicio rápido se muestra cómo se usa PHP para crear un programa que se conecte a una base de datos de Azure SQL y que use instrucciones Transact-SQL para consultar los datos.|
|[Python](sql-database-connect-query-python.md)|Esta guía de inicio rápido muestra cómo se usa Python para conectarse a una base de datos de Azure SQL y usar instrucciones Transact-SQL para consultar los datos. |
|[Ruby](sql-database-connect-query-ruby.md)|En esta guía de inicio rápido se muestra cómo se usa Ruby para crear un programa que se conecte a una base de datos de Azure SQL y que use instrucciones Transact-SQL para consultar los datos.|
|[R](sql-database-connect-query-r.md)|En esta guía de inicio rápido se muestra cómo se usa R con Azure SQL Machine Learning Services para crear un programa y que se conecte a una base de datos de Azure SQL y use instrucciones Transact-SQL para consultar los datos.|
|||

## <a name="tls-considerations-for-sql-database-connectivity"></a>Consideraciones de TLS para la conectividad de SQL Database
El protocolo Seguridad de capa de transporte (TLS) se usa en todos los controladores que Microsoft proporciona o admite para la conexión a Azure SQL Database. No es necesario realizar ninguna configuración especial. En todas las conexiones a SQL Server o Azure SQL Database, se recomienda que todas las aplicaciones establezcan las siguientes configuraciones o sus equivalentes:

 - **Encrypt = On**
 - **TrustServerCertificate = Off**

Algunos sistemas usan palabras clave diferentes pero equivalentes para esas palabras clave de configuración. Estas configuraciones garantizan que el controlador cliente comprueba la identidad del certificado TLS recibido del servidor.

También se recomienda deshabilitar TLS 1.1 y 1.0 en el cliente si debe cumplir con el estándar de seguridad de datos de la industria de tarjetas de pago, o PCI-DSS (Payment Card Industry - Data Security Standard).

Puede que los controladores que no son de Microsoft no usen TLS de forma predeterminada. Esto puede influir al conectarse a Azure SQL Database. Las aplicaciones con controladores insertados quizás no le permitan controlar estas configuraciones de conexión. Se recomienda que examine la seguridad de estos controladores y aplicaciones antes de usarlos en sistemas que interactúan con datos confidenciales.

## <a name="libraries"></a>Bibliotecas

Puede usar varias bibliotecas y plataformas para conectarse a Azure SQL Database. Vea nuestros [tutoriales de introducción](https://aka.ms/sqldev) para iniciarse rápidamente en los lenguajes de programación como C#, Java, Node.js, PHP y Python. A continuación, compile una aplicación mediante el uso de SQL Server en Linux o Windows o Docker en macOS.

En la siguiente tabla se enumeran las bibliotecas de conectividad o *controladores* que las aplicaciones cliente pueden utilizar desde una variedad de lenguajes para conectarse y que usan SQL Server de forma local o en la nube. Puede utilizarlas en Docker, Windows o Linux y usarlas para conectarse a Azure SQL Database y Azure SQL Data Warehouse. 

| Idioma | Plataforma | Recursos adicionales | Descargar | Introducción |
| :-- | :-- | :-- | :-- | :-- |
| C# | Windows, Linux, macOS | [Microsoft ADO.NET para SQL Server](https://docs.microsoft.com/sql/connect/ado-net/microsoft-ado-net-sql-server) | [Descargar](https://www.microsoft.com/net/download/) | [Introducción](https://www.microsoft.com/sql-server/developer-get-started/csharp/ubuntu)
| Java | Windows, Linux, macOS | [Controlador JDBC de Microsoft para SQL Server](https://msdn.microsoft.com/library/mt484311.aspx) | [Descargar](https://go.microsoft.com/fwlink/?linkid=852460) |  [Introducción](https://www.microsoft.com/sql-server/developer-get-started/java/ubuntu)
| PHP | Windows, Linux, macOS| [Controlador SQL de PHP para SQL Server](https://docs.microsoft.com/sql/connect/php/microsoft-php-driver-for-sql-server) | [Descargar](https://docs.microsoft.com/sql/connect/php/download-drivers-php-sql-server) | [Introducción](https://www.microsoft.com/sql-server/developer-get-started/php/ubuntu/)
| Node.js | Windows, Linux, macOS | [Controlador de Node.js para SQL Server](https://msdn.microsoft.com/library/mt652093.aspx) | [Instalación](https://msdn.microsoft.com/library/mt652094.aspx) |  [Introducción](https://www.microsoft.com/sql-server/developer-get-started/node/ubuntu)
| Python | Windows, Linux, macOS | [Controlador de Python para SQL Server](https://msdn.microsoft.com/library/mt652092.aspx) | Opciones de instalación: <br/> \* [pymssql](https://msdn.microsoft.com/library/mt694094.aspx) <br/> \* [pyodbc](https://msdn.microsoft.com/library/mt763257.aspx) |  [Introducción](https://www.microsoft.com/sql-server/developer-get-started/python/ubuntu)
| Ruby | Windows, Linux, macOS | [Controlador de Ruby para SQL Server](https://msdn.microsoft.com/library/mt691981.aspx) | [Instalación](https://msdn.microsoft.com/library/mt711041.aspx) | [Introducción](https://www.microsoft.com/sql-server/developer-get-started/ruby/ubuntu)
| C++ | Windows, Linux, macOS | [Microsoft ODBC driver for SQL Server](https://msdn.microsoft.com/library/mt654048(v=sql.1).aspx) | [Descargar](https://msdn.microsoft.com/library/mt654048(v=sql.1).aspx) |  

En la tabla siguiente se muestran ejemplos de marcos de asignación relacional de objetos (ORM) y marcos web que las aplicaciones cliente pueden utilizar con SQL Server de forma local o en la nube. Puede utilizar los marcos en Docker, Windows o Linux y usarlos para conectarse a SQL Database y SQL Data Warehouse. 

| Idioma | Plataforma | ORM |
| :-- | :-- | :-- |
| C# | Windows, Linux, macOS | [Entity Framework](https://docs.microsoft.com/ef)<br>[Entity Framework Core](https://docs.microsoft.com/ef/core/index) |
| Java | Windows, Linux, macOS |[Hibernate ORM](https://hibernate.org/orm)|
| PHP | Windows, Linux, macOS | [Laravel (Eloquent)](https://laravel.com/docs/eloquent)<br>[Doctrine](https://www.doctrine-project.org/projects/orm.html) |
| Node.js | Windows, Linux, macOS | [Sequelize ORM](https://sequelize.org/) |
| Python | Windows, Linux, macOS |[Django](https://www.djangoproject.com/) |
| Ruby | Windows, Linux, macOS | [Ruby on Rails](https://rubyonrails.org/) |
||||

## <a name="next-steps"></a>Pasos siguientes

- Para información sobre la arquitectura de conectividad, consulte [Arquitectura de conectividad de Azure SQL Database](sql-database-connectivity-architecture.md).
- Búsqueda de [controladores de SQL Server](https://msdn.microsoft.com/library/mt654049.aspx) que se usan para conectarse desde aplicaciones cliente
- Conexión a SQL Database:
  - [Conexión a SQL Database mediante .NET (C#)](sql-database-connect-query-dotnet.md) 
  - [Conexión a SQL Database mediante PHP en Windows](sql-database-connect-query-php.md) 
  - [Conexión a SQL Database mediante Node.js](sql-database-connect-query-nodejs.md) 
  - [Conexión a SQL Database mediante Java con JDBC en Windows](sql-database-connect-query-java.md) 
  - [Conexión a SQL Database mediante Python](sql-database-connect-query-python.md)
  - [Conexión a SQL Database mediante Ruby](sql-database-connect-query-ruby.md)
- Ejemplos de código de la lógica de reintento:
  - [Conectar de forma resistente a SQL con ADO.NET][step-4-connect-resiliently-to-sql-with-ado-net-a78n]
  - [Paso 4: Conectar de forma resistente a SQL con PHP][step-4-connect-resiliently-to-sql-with-php-p42h]

<!-- Link references. -->

[step-4-connect-resiliently-to-sql-with-ado-net-a78n]: https://docs.microsoft.com/sql/connect/ado-net/step-4-connect-resiliently-sql-ado-net

[step-4-connect-resiliently-to-sql-with-php-p42h]: https://docs.microsoft.com/sql/connect/php/step-4-connect-resiliently-to-sql-with-php

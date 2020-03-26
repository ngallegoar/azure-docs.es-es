---
title: Uso de Ruby para consultar
description: En este tema se muestra cómo usar Ruby para crear un programa que se conecta a una base de datos de Azure SQL y realiza consultas mediante instrucciones Transact-SQL.
services: sql-database
ms.service: sql-database
ms.subservice: development
ms.custom: ''
ms.devlang: ruby
ms.topic: quickstart
author: stevestein
ms.author: sstein
ms.reviewer: ''
ms.date: 03/25/2019
ms.openlocfilehash: 4bef55b049ee542efdb9d72d13fa196c989c75ec
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/26/2020
ms.locfileid: "73826920"
---
# <a name="quickstart-use-ruby-to-query-an-azure-sql-database"></a>Inicio rápido: Uso de Ruby para consultar una base de datos de Azure SQL

Esta guía de inicio rápido muestra cómo se usa [Ruby](https://www.ruby-lang.org) para conectarse a una base de datos de Azure SQL y consultar datos con instrucciones Transact-SQL.

## <a name="prerequisites"></a>Prerrequisitos

Para completar este tutorial de inicio rápido, debe cumplir los siguientes requisitos previos:

- Una base de datos de Azure SQL. Puede utilizar uno de estos inicios rápidos para crear y configurar una base de datos en Azure SQL Database:

  || Base de datos única | instancia administrada |
  |:--- |:--- |:---|
  | Crear| [Portal](sql-database-single-database-get-started.md) | [Portal](sql-database-managed-instance-get-started.md) |
  || [CLI](scripts/sql-database-create-and-configure-database-cli.md) | [CLI](https://medium.com/azure-sqldb-managed-instance/working-with-sql-managed-instance-using-azure-cli-611795fe0b44) |
  || [PowerShell](scripts/sql-database-create-and-configure-database-powershell.md) | [PowerShell](scripts/sql-database-create-configure-managed-instance-powershell.md) |
  | Configuración | [Regla de firewall de IP en el nivel de servidor](sql-database-server-level-firewall-rule.md)| [Conectividad desde una máquina virtual](sql-database-managed-instance-configure-vm.md)|
  |||[Conectividad desde el sitio](sql-database-managed-instance-configure-p2s.md)
  |Cargar datos|Adventure Works cargado por inicio rápido|[Restauración de World Wide Importers](sql-database-managed-instance-get-started-restore.md)
  |||Restauración o importación de Adventure Works a partir del archivo [BACPAC](sql-database-import.md) desde [GitHub](https://github.com/Microsoft/sql-server-samples/tree/master/samples/databases/adventure-works)|
  |||

  > [!IMPORTANT]
  > Los scripts de este artículo se escriben para utilizar la base de datos Adventure Works. Con una instancia administrada, debe importar la base de datos Adventure Works en una base de datos de instancia o modificar los scripts de este artículo para utilizar la base de datos Wide World Importers.
  
- Ruby y el software relacionado para el sistema operativo:
  
  - **MacOS**: instale Homebrew, rbenv y ruby-build, Ruby, FreeTDS y TinyTDS. Consulte los pasos 1.2, 1.3, 1.4, 1.5 y 2.1 del artículo sobre la [creación de aplicaciones de Ruby con SQL Server en macOS](https://www.microsoft.com/sql-server/developer-get-started/ruby/mac/).
  
  - **Ubuntu**: instale los requisitos previos de Ruby, rbenv y ruby-build, Ruby, FreeTDS y TinyTDS. Consulte los pasos 1.2, 1.3, 1.4, 1.5 y 2.1 en [Create Ruby apps using SQL Server on Ubuntu](https://www.microsoft.com/sql-server/developer-get-started/ruby/ubuntu/) (Creación de aplicaciones de Ruby con SQL Server en Ubuntu).
  
  - **Windows**: instale Ruby, Ruby Devkit y TinyTDS. Consulte [Configurar el entorno de desarrollo para el desarrollo Ruby](/sql/connect/ruby/step-1-configure-development-environment-for-ruby-development).

## <a name="get-sql-server-connection-information"></a>Obtención de información de conexión de SQL Server

Obtención de la información de conexión necesaria para conectarse a Azure SQL Database. En los procedimientos siguientes, necesitará el nombre completo del servidor o nombre de host, el nombre de la base de datos y la información de inicio de sesión.

1. Inicie sesión en [Azure Portal](https://portal.azure.com/).

2. Vaya a las páginas **SQL Database** o **Instancias administradas de SQL**.

3. En la página **Información general**, revise el nombre completo del servidor junto a **Nombre del servidor** para una única base de datos o el nombre completo del servidor junto a **Host** para una instancia administrada. Para copiar el nombre del servidor o nombre de host, mantenga el cursor sobre él y seleccione el icono **Copiar**. 

## <a name="create-code-to-query-your-sql-database"></a>Creación del código para consultar la base de datos SQL

1. En el editor de texto o de código, cree un archivo denominado *sqltest.rb*.
   
1. Agregue el código siguiente: Sustituya los valores de su base de datos de Azure SQL por `<server>`, `<database>`, `<username>` y `<password>`.
   
   >[!IMPORTANT]
   >El código de este ejemplo usa los datos de AdventureWorksLT de ejemplo, que puede elegir como origen al crear la base de datos. Si la base de datos tiene datos distintos, use las tablas de su propia base de datos en la consulta SELECT. 
   
   ```ruby
   require 'tiny_tds'
   server = '<server>.database.windows.net'
   database = '<database>'
   username = '<username>'
   password = '<password>'
   client = TinyTds::Client.new username: username, password: password, 
       host: server, port: 1433, database: database, azure: true
   
   puts "Reading data from table"
   tsql = "SELECT TOP 20 pc.Name as CategoryName, p.name as ProductName
           FROM [SalesLT].[ProductCategory] pc
           JOIN [SalesLT].[Product] p
           ON pc.productcategoryid = p.productcategoryid"
   result = client.execute(tsql)
   result.each do |row|
       puts row
   end
   ```

## <a name="run-the-code"></a>Ejecución del código

1. En el símbolo del sistema, ejecute el siguiente comando:

   ```bash
   ruby sqltest.rb
   ```
   
1. Compruebe que se devuelven las 20 primeras filas de categoría o productos de la base de datos. 

## <a name="next-steps"></a>Pasos siguientes
- [Diseño de su primera base de datos de Azure SQL](sql-database-design-first-database.md).
- [Repositorio de GitHub para TinyTDS](https://github.com/rails-sqlserver/tiny_tds).
- [Informe de los problemas y realización de preguntas sobre TinyTDS](https://github.com/rails-sqlserver/tiny_tds/issues).
- [Controlador de Ruby para SQL Server](https://docs.microsoft.com/sql/connect/ruby/ruby-driver-for-sql-server/).

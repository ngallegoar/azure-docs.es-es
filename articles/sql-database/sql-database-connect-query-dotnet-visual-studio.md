---
title: Uso de Visual Studio con .NET y C# para realizar consultas
description: Uso de Visual Studio para crear una aplicación de C# que se conecta a una base de datos de Azure SQL Database y la consulta con instrucciones Transact-SQL.
services: sql-database
ms.service: sql-database
ms.subservice: development
ms.custom: ''
ms.devlang: dotnet
ms.topic: quickstart
author: stevestein
ms.author: sstein
ms.reviewer: ''
ms.date: 12/31/2019
ms.openlocfilehash: 75f84f87fc59a870a0c88fd24cb1b1b8894b3e2a
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/26/2020
ms.locfileid: "79209567"
---
# <a name="quickstart-use-net-and-c-in-visual-studio-to-connect-to-and-query-an-azure-sql-database"></a>Inicio rápido: uso de .NET y C# en Visual Studio para conectar a una base de datos de Azure SQL Database y consultarla

En este inicio rápido se muestra cómo usar [.NET Framework](https://www.microsoft.com/net/) y el código de C# en Visual Studio para consultar una base de datos de Azure SQL Database con instrucciones Transact-SQL.

## <a name="prerequisites"></a>Prerrequisitos

Para completar este inicio rápido necesita instalar:

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

- [Visual Studio 2019](https://www.visualstudio.com/downloads/) Community, Professional o Enterprise.

## <a name="get-sql-server-connection-information"></a>Obtención de información de conexión de SQL Server

Obtención de la información de conexión necesaria para conectarse a Azure SQL Database. En los procedimientos siguientes, necesitará el nombre completo del servidor o nombre de host, el nombre de la base de datos y la información de inicio de sesión.

1. Inicie sesión en [Azure Portal](https://portal.azure.com/).

2. Vaya a las páginas **SQL Database** o **Instancias administradas de SQL**.

3. En la página **Información general**, revise el nombre completo del servidor junto a **Nombre del servidor** para una única base de datos o el nombre completo del servidor junto a **Host** para una instancia administrada. Para copiar el nombre del servidor o nombre de host, mantenga el cursor sobre él y seleccione el icono **Copiar**. 

## <a name="create-code-to-query-the-sql-database"></a>Creación del código para consultar la base de datos SQL

1. Abra Visual Studio, seleccione **Archivo** > **Nuevo** > **Proyecto**. 
   
1. En el cuadro de diálogo **Nuevo proyecto**, seleccione **Visual C#** y, luego, **Aplicación de consola (.NET Framework)** .
   
1. Escriba *sqltest* en el nombre del proyecto y, luego, seleccione **Aceptar**. El proyecto nuevo se crea. 
   
1. Seleccione **Proyecto** > **Administrar paquetes NuGet**. 
   
1. En **Administrador de paquetes NuGet**, seleccione la pestaña **Examinar** y busque y seleccione **System.Data.SqlClient**.
   
1. En la página **System.Data.SqlClient**, seleccione **Instalar**. 
   - Si se le solicita, seleccione **Aceptar** para seguir con la instalación. 
   - Si aparece la ventana **Aceptación de la licencia**, seleccione **Acepto**.
   
1. Cuando se complete la instalación, puede cerrar el **Administrador de paquetes NuGet**. 
   
1. En el editor de código, reemplace el contenido de **Program.cs** por el código siguiente. Reemplace los valores de `<server>`, `<username>`, `<password>` y `<database>`.
   
   >[!IMPORTANT]
   >El código de este ejemplo usa los datos de AdventureWorksLT de ejemplo, que puede elegir como origen al crear la base de datos. Si la base de datos tiene datos distintos, use las tablas de su propia base de datos en la consulta SELECT. 
   
   ```csharp
   using System;
   using System.Data.SqlClient;
   using System.Text;
   
   namespace sqltest
   {
       class Program
       {
           static void Main(string[] args)
           {
               try 
               { 
                   SqlConnectionStringBuilder builder = new SqlConnectionStringBuilder();
                   builder.DataSource = "<server>.database.windows.net"; 
                   builder.UserID = "<username>";            
                   builder.Password = "<password>";     
                   builder.InitialCatalog = "<database>";
   
                   using (SqlConnection connection = new SqlConnection(builder.ConnectionString))
                   {
                       Console.WriteLine("\nQuery data example:");
                       Console.WriteLine("=========================================\n");
                       
                       StringBuilder sb = new StringBuilder();
                       sb.Append("SELECT TOP 20 pc.Name as CategoryName, p.name as ProductName ");
                       sb.Append("FROM [SalesLT].[ProductCategory] pc ");
                       sb.Append("JOIN [SalesLT].[Product] p ");
                       sb.Append("ON pc.productcategoryid = p.productcategoryid;");
                       String sql = sb.ToString();
   
                       using (SqlCommand command = new SqlCommand(sql, connection))
                       {
                           connection.Open();
                           using (SqlDataReader reader = command.ExecuteReader())
                           {
                               while (reader.Read())
                               {
                                   Console.WriteLine("{0} {1}", reader.GetString(0), reader.GetString(1));
                               }
                           }
                       }                    
                   }
               }
               catch (SqlException e)
               {
                   Console.WriteLine(e.ToString());
               }
               Console.ReadLine();
           }
       }
   }
   ```

## <a name="run-the-code"></a>Ejecución del código

1. Para ejecutar la aplicación, seleccione **Depurar** > **Iniciar depuración**, o bien seleccione **Iniciar** en la barra de herramientas o presione **F5**.
1. Compruebe que se devuelven las 20 primeras filas de categoría o productos de la base de datos y cierre la ventana de la aplicación.

## <a name="next-steps"></a>Pasos siguientes

- Obtenga información acerca de cómo [conectarse y consultar una base de datos de Azure SQL mediante .NET Core](sql-database-connect-query-dotnet-core.md) en Windows, Linux y macOS.  
- Para más información, consulte [Introducción a .NET Core en Windows, Linux y macOS mediante la línea de comandos](/dotnet/core/tutorials/using-with-xplat-cli).
- Obtenga información acerca de cómo [diseñar la primera base de datos de Azure SQL con SSMS](sql-database-design-first-database.md) o [diseñar la primera base de datos de Azure SQL mediante .NET](sql-database-design-first-database-csharp.md).
- Para más información acerca de. NET, consulte la [Documentación de .NET](https://docs.microsoft.com/dotnet/).
- Ejemplo de lógica de reintento: [Conexión resistente a SQL con ADO.NET][step-4-connect-resiliently-to-sql-with-ado-net-a78n].


<!-- Link references. -->

[step-4-connect-resiliently-to-sql-with-ado-net-a78n]: https://docs.microsoft.com/sql/connect/ado-net/step-4-connect-resiliently-sql-ado-net


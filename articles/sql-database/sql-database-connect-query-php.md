---
title: Uso de PHP para realizar consultas
description: Cómo usar PHP para crear un programa que se conecte a una base de datos de Azure SQL y realice consultas ella mediante instrucciones T-SQL.
services: sql-database
ms.service: sql-database
ms.subservice: development
ms.devlang: php
ms.topic: quickstart
author: stevestein
ms.author: sstein
ms.reviewer: v-masebo
ms.date: 02/12/2019
ms.openlocfilehash: ae119dd23da670f16c0239b14119519c431e6326
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/26/2020
ms.locfileid: "73827008"
---
# <a name="quickstart-use-php-to-query-an-azure-sql-database"></a>Guía de inicio rápido: Uso de PHP para consultar una base de datos de Azure SQL Database

En este artículo se muestra cómo usar [PHP](https://php.net/manual/en/intro-whatis.php) para conectarse a una base de datos de Azure SQL. Luego puede usar instrucciones de T-SQL para consultar los datos.

## <a name="prerequisites"></a>Prerequisites

Para completar este ejemplo, asegúrese de que cumple los siguientes requisitos previos:

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

- Software relacionado con PHP instalado para el sistema operativo:

  - **MacOS**: instale PHP, el controlador ODBC y, luego, el controlador PHP para SQL Server. Consulte los [pasos 1, 2 y 3](/sql/connect/php/installation-tutorial-linux-mac).

  - **Linux**: instale PHP, el controlador ODBC y, luego, el controlador PHP para SQL Server. Consulte los [pasos 1, 2 y 3](/sql/connect/php/installation-tutorial-linux-mac).

  - **Windows**, instale PHP para IIS Express y Chocolatey, y después instale el controlador ODBC y SQLCMD. Consulte [pasos 1.2 y 1.3](https://www.microsoft.com/sql-server/developer-get-started/php/windows/).

## <a name="get-sql-server-connection-information"></a>Obtención de información de conexión de SQL Server

Obtención de la información de conexión necesaria para conectarse a Azure SQL Database. En los procedimientos siguientes, necesitará el nombre completo del servidor o nombre de host, el nombre de la base de datos y la información de inicio de sesión.

1. Inicie sesión en [Azure Portal](https://portal.azure.com/).

2. Vaya a las páginas **SQL Database** o **Instancias administradas de SQL**.

3. En la página **Información general**, revise el nombre completo del servidor junto a **Nombre del servidor** para una única base de datos o el nombre completo del servidor junto a **Host** para una instancia administrada. Para copiar el nombre del servidor o nombre de host, mantenga el cursor sobre él y seleccione el icono **Copiar**.

## <a name="add-code-to-query-database"></a>Adición de código a una base de datos de consulta

1. En el editor de texto, cree un nuevo archivo, *sqltest.php*.  

1. Reemplace su contenido por el código siguiente. Agregue los valores apropiados para el servidor, la base de datos, el usuario y la contraseña.

   ```PHP
   <?php
       $serverName = "your_server.database.windows.net"; // update me
       $connectionOptions = array(
           "Database" => "your_database", // update me
           "Uid" => "your_username", // update me
           "PWD" => "your_password" // update me
       );
       //Establishes the connection
       $conn = sqlsrv_connect($serverName, $connectionOptions);
       $tsql= "SELECT TOP 20 pc.Name as CategoryName, p.name as ProductName
            FROM [SalesLT].[ProductCategory] pc
            JOIN [SalesLT].[Product] p
            ON pc.productcategoryid = p.productcategoryid";
       $getResults= sqlsrv_query($conn, $tsql);
       echo ("Reading data from table" . PHP_EOL);
       if ($getResults == FALSE)
           echo (sqlsrv_errors());
       while ($row = sqlsrv_fetch_array($getResults, SQLSRV_FETCH_ASSOC)) {
        echo ($row['CategoryName'] . " " . $row['ProductName'] . PHP_EOL);
       }
       sqlsrv_free_stmt($getResults);
   ?>
   ```

## <a name="run-the-code"></a>Ejecución del código

1. En el símbolo del sistema, ejecute la aplicación.

   ```bash
   php sqltest.php
   ```

1. Compruebe que se han devuelto las 20 primeras filas y cierre la ventana de la aplicación.

## <a name="next-steps"></a>Pasos siguientes

- [Diseño de su primera base de datos de Azure SQL](sql-database-design-first-database.md)

- [Controladores de Microsoft para PHP para SQL Server](https://github.com/Microsoft/msphpsql/)

- [Informe de los problemas y realización de preguntas](https://github.com/Microsoft/msphpsql/issues)

- [Ejemplo de lógica de reintento: conexión resistente a SQL con PHP](/sql/connect/php/step-4-connect-resiliently-to-sql-with-php)

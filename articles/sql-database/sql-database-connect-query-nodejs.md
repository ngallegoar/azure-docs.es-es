---
title: Uso de Node.js para consultar una base de datos
description: Cómo usar Node.js para crear un programa que se conecte a una base de datos de Azure SQL y realice consultas mediante instrucciones T-SQL.
services: sql-database
ms.service: sql-database
ms.subservice: development
ms.devlang: nodejs
ms.topic: quickstart
author: stevestein
ms.author: sstein
ms.reviewer: v-masebo
ms.date: 03/25/2019
ms.custom: seo-javascript-september2019, seo-javascript-october2019
ms.openlocfilehash: c0da38a41bf613237ea3b164d70e4729a7284ca7
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/26/2020
ms.locfileid: "76768599"
---
# <a name="quickstart-use-nodejs-to-query-an-azure-sql-database"></a>Guía de inicio rápido: Uso de Node.js para consultar una base de datos de Azure SQL Database

En esta guía de inicio rápido se muestra cómo se usa Node.js para conectarse a una base de datos de Azure SQL y cómo se utilizan las instrucciones T-SQL para consultar los datos.

## <a name="prerequisites"></a>Prerequisites

- Una cuenta de Azure con una suscripción activa. [Cree una cuenta gratuita](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio).
- Una [instancia de Azure SQL Database](sql-database-single-database-get-started.md).
- Software relacionado con [Node.js](https://nodejs.org)

  # <a name="macos"></a>[macOS](#tab/macos)

  Instale Homebrew y Node.js, después instale el controlador ODBC y SQLCMD mediante los pasos **1.2** y **1.3** en [Creación de aplicaciones Node.js mediante SQL Server en macOS](https://www.microsoft.com/sql-server/developer-get-started/node/mac/).

  # <a name="ubuntu"></a>[Ubuntu](#tab/ubuntu)

  Instale Node.js, después instale el controlador ODBC y SQLCMD mediante los pasos **1.2** y **1.3** en [Creación de aplicaciones Node.js mediante SQL Server en Ubuntu](https://www.microsoft.com/sql-server/developer-get-started/node/ubuntu/).

  # <a name="windows"></a>[Windows](#tab/windows)

  Instale Chocolatey y Node.js, después instale el controlador ODBC y SQLCMD mediante los pasos **1.2** y **1.3** en [Creación de aplicaciones Node.js mediante SQL Server en Windows](https://www.microsoft.com/sql-server/developer-get-started/node/windows/).

  ---

> [!IMPORTANT]
> Los scripts de este artículo se escriben para utilizar la base de datos **Adventure Works**.

> [!NOTE]
> También puede elegir una instancia administrada de Azure SQL.
>
> Para crear y configurar, use [Azure portal](sql-database-managed-instance-get-started.md), [PowerShell](scripts/sql-database-create-configure-managed-instance-powershell.md)o la [CLI](https://medium.com/azure-sqldb-managed-instance/working-with-sql-managed-instance-using-azure-cli-611795fe0b44). Después, configure la conectividad [local](sql-database-managed-instance-configure-p2s.md) o de la [máquina virtual](sql-database-managed-instance-configure-vm.md).
>
> Para cargar los datos, consulte la [restauración con BACPAC](sql-database-import.md) con el archivo [Adventure Works](https://github.com/Microsoft/sql-server-samples/tree/master/samples/databases/adventure-works) o consulte la [restauración de la base de datos de Wide World Importers](sql-database-managed-instance-get-started-restore.md).

## <a name="get-sql-server-connection-information"></a>Obtención de información de conexión de SQL Server

Obtención de la información de conexión necesaria para conectarse a Azure SQL Database. En los procedimientos siguientes, necesitará el nombre completo del servidor o nombre de host, el nombre de la base de datos y la información de inicio de sesión.

1. Inicie sesión en [Azure Portal](https://portal.azure.com/).

2. Vaya a las páginas **Bases de datos SQL** o **Instancias administradas de SQL**.

3. En la página **Información general**, revise el nombre completo del servidor junto a **Nombre del servidor** para una única base de datos o el nombre completo del servidor junto a **Host** para una instancia administrada. Para copiar el nombre del servidor o nombre de host, mantenga el cursor sobre él y seleccione el icono **Copiar**. 

## <a name="create-the-project"></a>Creación del proyecto

Abra un símbolo del sistema y cree una carpeta denominada *sqltest*. Abra la carpeta que ha creado y ejecute el comando siguiente:

  ```bash
  npm init -y
  npm install tedious
  ```

## <a name="add-code-to-query-database"></a>Adición de código a una base de datos de consulta

1. En el editor de texto, cree un archivo, *sqltest.py*.

1. Reemplace su contenido por el código siguiente. Agregue los valores apropiados para el servidor, la base de datos, el usuario y la contraseña.

    ```js
    const { Connection, Request } = require("tedious");

    // Create connection to database
    const config = {
      authentication: {
        options: {
          userName: "username", // update me
          password: "password" // update me
        },
        type: "default"
      },
      server: "your_server.database.windows.net", // update me
      options: {
        database: "your_database", //update me
        encrypt: true
      }
    };

    const connection = new Connection(config);

    // Attempt to connect and execute queries if connection goes through
    connection.on("connect", err => {
      if (err) {
        console.error(err.message);
      } else {
        queryDatabase();
      }
    });

    function queryDatabase() {
      console.log("Reading rows from the Table...");

      // Read all rows from table
      const request = new Request(
        `SELECT TOP 20 pc.Name as CategoryName,
                       p.name as ProductName
         FROM [SalesLT].[ProductCategory] pc
         JOIN [SalesLT].[Product] p ON pc.productcategoryid = p.productcategoryid`,
        (err, rowCount) => {
          if (err) {
            console.error(err.message);
          } else {
            console.log(`${rowCount} row(s) returned`);
          }
        }
      );

      request.on("row", columns => {
        columns.forEach(column => {
          console.log("%s\t%s", column.metadata.colName, column.value);
        });
      });
      
      connection.execSql(request);
    }
    ```

> [!NOTE]
> El ejemplo de código usa la base de datos de ejemplo para SQL Azure **AdventureWorksLT**.

## <a name="run-the-code"></a>Ejecución del código

1. En el símbolo del sistema, ejecute el programa.

    ```bash
    node sqltest.js
    ```

1. Compruebe que se han devuelto las 20 primeras filas y cierre la ventana de la aplicación.

## <a name="next-steps"></a>Pasos siguientes

- [Controlador de Microsoft Node.js para SQL Server](/sql/connect/node-js/node-js-driver-for-sql-server)

- Conéctese y realice consultas en Windows, Linux y macOS con [.NET Core](sql-database-connect-query-dotnet-core.md), [Visual Studio Code](sql-database-connect-query-vscode.md) o [SSMS](sql-database-connect-query-ssms.md) (solo Windows)

- [Introducción a .NET Core en Windows, Linux y macOS mediante la línea de comandos](/dotnet/core/tutorials/using-with-xplat-cli)

- Diseño de la primera base de datos de Azure SQL mediante [.NET](sql-database-design-first-database-csharp.md) o [SSMS](sql-database-design-first-database.md)

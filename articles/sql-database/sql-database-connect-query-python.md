---
title: Uso de Python para consultar una base de datos
description: En este tema se explica cómo usar Python para crear un programa que se conecta a una base de datos de Azure SQL y realiza consultas mediante instrucciones Transact-SQL.
services: sql-database
ms.service: sql-database
ms.subservice: development
ms.custom: seo-python-october2019
ms.devlang: python
ms.topic: quickstart
author: stevestein
ms.author: sstein
ms.reviewer: ''
ms.date: 03/25/2019
ms.openlocfilehash: e82f8feae0096202e48a58296dd2e9d21bb61885
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/26/2020
ms.locfileid: "76768570"
---
# <a name="quickstart-use-python-to-query-an-azure-sql-database"></a>Guía de inicio rápido: Uso de Python para consultar una base de datos de Azure SQL Database

En este inicio rápido se muestra cómo se usa Python para conectarse a una base de datos de Azure SQL y usar instrucciones T-SQL para consultar los datos.

## <a name="prerequisites"></a>Prerequisites

- Una cuenta de Azure con una suscripción activa. [Cree una cuenta gratuita](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio).
- Una [instancia de Azure SQL Database](sql-database-single-database-get-started.md).
- [Python](https://python.org/downloads) 3 y software relacionado

  # <a name="macos"></a>[macOS](#tab/macos)

  Para instalar Homebrew y Python, el controlador ODBC y SQLCMD, y el controlador de Python para SQL Server, use los pasos **1.2**, **1.3** y **2.1** en [Creación de aplicaciones de Python con SQL Server en macOS](https://www.microsoft.com/sql-server/developer-get-started/python/mac/).

  Para más información, consulte [Microsoft ODBC Driver en macOS](/sql/connect/odbc/linux-mac/installing-the-microsoft-odbc-driver-for-sql-server).

  # <a name="ubuntu"></a>[Ubuntu](#tab/ubuntu)

  Para instalar Python y otros paquetes necesarios, use `sudo apt-get install python python-pip gcc g++ build-essential`.

  Para instalar el controlador ODBC, SQLCMD y el controlador de Python para SQL Server, consulte el artículo para [configurar un entorno para el desarrollo de pyodbc Python](/sql/connect/python/pyodbc/step-1-configure-development-environment-for-pyodbc-python-development#linux).

  Para más información, consulte [Microsoft ODBC Driver en Linux](/sql/connect/odbc/linux-mac/installing-the-microsoft-odbc-driver-for-sql-server).

  # <a name="windows"></a>[Windows](#tab/windows)

  Para instalar Python, el controlador ODBC, SQLCMD y el controlador de Python para SQL Server, consulte el artículo para [configurar un entorno para el desarrollo de pyodbc Python](/sql/connect/python/pyodbc/step-1-configure-development-environment-for-pyodbc-python-development#windows).

  Para más información, consulte [Microsoft ODBC Driver](/sql/connect/odbc/microsoft-odbc-driver-for-sql-server).

---

> [!IMPORTANT]
> Los scripts de este artículo se escriben para utilizar la base de datos **Adventure Works**.

> [!NOTE]
> También puede elegir una instancia administrada de Azure SQL.
>
> Para crear y configurar, use [Azure portal](sql-database-managed-instance-get-started.md), [PowerShell](scripts/sql-database-create-configure-managed-instance-powershell.md)o la [CLI](https://medium.com/azure-sqldb-managed-instance/working-with-sql-managed-instance-using-azure-cli-611795fe0b44). Después, configure la conectividad [local](sql-database-managed-instance-configure-p2s.md) o de la [máquina virtual](sql-database-managed-instance-configure-vm.md).
>
> Para cargar los datos, consulte la [restauración con BACPAC](sql-database-import.md) con el archivo [Adventure Works](https://github.com/Microsoft/sql-server-samples/tree/master/samples/databases/adventure-works) o consulte la [restauración de la base de datos de Wide World Importers](sql-database-managed-instance-get-started-restore.md).

Para explorar aún más Python y Azure SQL Database, consulte las [bibliotecas de Azure SQL Database para Python](/python/api/overview/azure/sql), el [repositorio de pyodbc](https://github.com/mkleehammer/pyodbc/wiki/) y un [ejemplo de pyodbc](https://github.com/mkleehammer/pyodbc/wiki/Getting-started).

## <a name="get-sql-server-connection-information"></a>Obtención de información de conexión de SQL Server

Obtención de la información de conexión necesaria para conectarse a Azure SQL Database. En los procedimientos siguientes, necesitará el nombre completo del servidor o nombre de host, el nombre de la base de datos y la información de inicio de sesión.

1. Inicie sesión en [Azure Portal](https://portal.azure.com/).

2. Vaya a las páginas **Bases de datos SQL** o **Instancias administradas de SQL**.

3. En la página **Información general**, revise el nombre completo del servidor junto a **Nombre del servidor** para una única base de datos o el nombre completo del servidor junto a **Host** para una instancia administrada. Para copiar el nombre del servidor o nombre de host, mantenga el cursor sobre él y seleccione el icono **Copiar**.

## <a name="create-code-to-query-your-sql-database"></a>Creación del código para consultar la base de datos SQL 

1. En el editor de texto, cree un archivo llamado *sqltest.py*.  
   
1. Agregue el código siguiente: Sustituya sus propios valores de \<servidor>, \<base de datos>, \<nombre de usuario> y \<contraseña>.
   
   >[!IMPORTANT]
   >El código de este ejemplo usa los datos de AdventureWorksLT de ejemplo, que puede elegir como origen al crear la base de datos. Si la base de datos tiene datos distintos, use las tablas de su propia base de datos en la consulta SELECT. 
   
   ```python
   import pyodbc
   server = '<server>.database.windows.net'
   database = '<database>'
   username = '<username>'
   password = '<password>'
   driver= '{ODBC Driver 17 for SQL Server}'
   cnxn = pyodbc.connect('DRIVER='+driver+';SERVER='+server+';PORT=1433;DATABASE='+database+';UID='+username+';PWD='+ password)
   cursor = cnxn.cursor()
   cursor.execute("SELECT TOP 20 pc.Name as CategoryName, p.name as ProductName FROM [SalesLT].[ProductCategory] pc JOIN [SalesLT].[Product] p ON pc.productcategoryid = p.productcategoryid")
   row = cursor.fetchone()
   while row:
       print (str(row[0]) + " " + str(row[1]))
       row = cursor.fetchone()
   ```
   

## <a name="run-the-code"></a>Ejecución del código

1. En el símbolo del sistema, ejecute el siguiente comando:

   ```cmd
   python sqltest.py
   ```

1. Compruebe que se devuelven las 20 primeras filas de categoría o productos y, después, cierre la ventana Comandos.

## <a name="next-steps"></a>Pasos siguientes

- [Diseño de su primera base de datos de Azure SQL](sql-database-design-first-database.md)
- [Controladores de Microsoft para Python para SQL Server](https://docs.microsoft.com/sql/connect/python/python-driver-for-sql-server/)
- [Centro para desarrolladores de Python](https://azure.microsoft.com/develop/python/?v=17.23h)


---
title: 'SSMS: Conexión y consulta de datos'
description: Aprenda a conectarse a SQL Database en Azure con SQL Server Management Studio (SSMS). A continuación, ejecute instrucciones de Transact-SQL (T-SQL) para consultar y editar los datos.
keywords: conexión a base de datos sql,sql server management studio
services: sql-database
ms.service: sql-database
ms.subservice: service
ms.custom: ''
ms.devlang: ''
ms.topic: quickstart
author: stevestein
ms.author: sstein
ms.reviewer: ''
ms.date: 03/10/2020
ms.openlocfilehash: 31bd47128a272e75d7021180b536fe6bf7420f55
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/26/2020
ms.locfileid: "79299301"
---
# <a name="quickstart-use-sql-server-management-studio-to-connect-and-query-an-azure-sql-database"></a>Inicio rápido: uso de SQL Server Management Studio para conectarse a una base de datos de Azure SQL Database y realizar consultas en ella

En este inicio rápido aprenderá a usar SQL Server Management Studio (SSMS) para conectarse a una base de datos de Azure SQL.

## <a name="prerequisites"></a>Prerrequisitos

Para completar este inicio rápido son necesarios los siguientes elementos:

- [SQL Server Management Studio (SSMS)](/sql/ssms/download-sql-server-management-studio-ssms/).
- La base de datos de ejemplo AdventureWorksLT. Si necesita una copia de trabajo de la base de datos AdventureWorksLT, cree una con el inicio rápido [Crear una base de datos de Azure SQL](sql-database-single-database-get-started.md).
    - Los scripts de este artículo están escritos para utilizar la base de datos AdventureWorksLT. Si usa una instancia administrada, tiene que importar la base de datos AdventureWorks en una base de datos de instancia o modificar los scripts de este artículo para utilizar la base de datos Wide World Importers.

Si simplemente desea ejecutar algunas consultas ad hoc sin instalar SSMS, consulte [Inicio rápido: Uso del editor de consultas de Azure Portal para consultar una base de datos SQL](sql-database-connect-query-portal.md).

## <a name="get-sql-server-connection-information"></a>Obtención de información de conexión de SQL Server

Obtenga la información de conexión que necesita para conectarse a su base de datos. Para completar este inicio rápido necesitará el nombre completo del servidor o nombre de host, el nombre de la base de datos y la información de inicio de sesión.

1. Inicie sesión en [Azure Portal](https://portal.azure.com/).

2. Vaya a la **base de datos SQL** o la **instancia administrada de SQL** que desea consultar.

3. En la página **Información general**, copie el nombre completo del servidor. Junto a **Nombre de servidor** para una base de datos única, o el nombre de servidor completo junto a **Host** en el caso de una instancia administrada. El nombre completo tiene el siguiente aspecto: *servername.database.windows.net*, excepto que en lugar de servername tiene el nombre de su servidor.

## <a name="connect-to-your-database"></a>Conectarse a la base de datos

En SSMS, conéctese al servidor de Azure SQL Database.

> [!IMPORTANT]
> Un servidor de Azure SQL Database escucha en el puerto 1433. Para conectarse a un servidor de SQL Database desde detrás de un firewall corporativo, el firewall debe tener abierto este puerto.

1. Abra SSMS.

2. Aparecerá el cuadro de diálogo **Conectar con el servidor** . Escriba la siguiente información:

   | Configuración      | Valor sugerido    | Descripción |
   | ------------ | ------------------ | ----------- |
   | **Tipo de servidor** | Motor de base de datos | Valor requerido. |
   | **Nombre del servidor** | Nombre completo del servidor | Algo como: **servername.database.windows.net**. |
   | **Autenticación** | Autenticación de SQL Server | En este tutorial se usa la autenticación de SQL. |
   | **Inicio de sesión** | Identificador de usuario de la cuenta de administrador del servidor | El identificador de usuario de la cuenta de administrador del servidor que se usó para crear el servidor. |
   | **Contraseña** | Contraseña segura de la cuenta de administrador | La contraseña de la cuenta de administrador del servidor que se usó para crear el servidor. |
   ||||

   ![conectar con el servidor](./media/sql-database-connect-query-ssms/connect.png)  

3. Seleccione **Opciones** en el cuadro de diálogo **Conectar con el servidor**. En el menú desplegable **Conectar con el servidor**, seleccione **mySampleDatabase**. Al completar el inicio rápido en la [sección Prerrequisitos](#prerequisites) se crea una base de datos AdventureWorksLT denominada mySampleDatabase. Si la copia de trabajo de la base de datos AdventureWorks tiene un nombre diferente a mySampleDatabase, selecciónelo en su lugar.

   ![conectar a base de datos en el servidor](./media/sql-database-connect-query-ssms/options-connect-to-db.png)  

4. Seleccione **Conectar**. Se abre la ventana del Explorador de objetos.

5. Para ver los objetos de la base de datos, expanda **Bases de datos** y luego expanda el nodo de su base de datos.

   ![Objetos mySampleDatabase](./media/sql-database-connect-query-ssms/connected.png)  

## <a name="query-data"></a>Consultar datos

Ejecute el código Transact-SQL [SELECT](https://msdn.microsoft.com/library/ms189499.aspx) para consultar los 20 primeros productos por categoría.

1. En el Explorador de objetos, haga clic con el botón derecho en **mySampleDatabase** y seleccione **Nueva consulta**. Se abre una ventana de consulta conectada a la base de datos.

2. Pegue la consulta SQL siguiente en la ventana de consulta:

   ```sql
   SELECT pc.Name as CategoryName, p.name as ProductName
   FROM [SalesLT].[ProductCategory] pc
   JOIN [SalesLT].[Product] p
   ON pc.productcategoryid = p.productcategoryid;
   ```

3. En la barra de herramienta, seleccione **Ejecutar** para ejecutar la consulta y recuperar los datos de las tablas `Product` y `ProductCategory`.

    ![consulta para recuperar datos desde las tabla Product y ProductCategory](./media/sql-database-connect-query-ssms/query2.png)

### <a name="insert-data"></a>Insertar datos

Ejecute el código Transact-SQL [INSERT](https://msdn.microsoft.com/library/ms174335.aspx) para crear un nuevo producto en la tabla `SalesLT.Product`.

1. Reemplace la consulta anterior por esta otra.

   ```sql
   INSERT INTO [SalesLT].[Product]
           ( [Name]
           , [ProductNumber]
           , [Color]
           , [ProductCategoryID]
           , [StandardCost]
           , [ListPrice]
           , [SellStartDate] )
     VALUES
           ('myNewProduct'
           ,123456789
           ,'NewColor'
           ,1
           ,100
           ,100
           ,GETDATE() );
   ```

2. Seleccione **Ejecutar** para insertar una nueva fila en la tabla `Product`. El panel **Mensajes** muestra **(1 fila afectada)** .

#### <a name="view-the-result"></a>Ver el resultado

1. Reemplace la consulta anterior por esta otra.

   ```sql
   SELECT * FROM [SalesLT].[Product]
   WHERE Name='myNewProduct'
   ```

2. Seleccione **Execute**(Ejecutar). Aparecen los siguientes resultados.

   ![resultado de la consulta de la tabla Product](./media/sql-database-connect-query-ssms/result.png)

### <a name="update-data"></a>Actualización de datos

Ejecute el código Transact-SQL [UPDATE](https://msdn.microsoft.com/library/ms177523.aspx) para modificar el nuevo producto.

1. Reemplace la consulta anterior por esta que devuelve el nuevo registro creado anteriormente:

   ```sql
   UPDATE [SalesLT].[Product]
   SET [ListPrice] = 125
   WHERE Name = 'myNewProduct';
   ```

2. Seleccione **Ejecutar** para actualizar la fila especificada en la tabla `Product`. El panel **Mensajes** muestra **(1 fila afectada)** .

### <a name="delete-data"></a>Eliminación de datos

Ejecute el código Transact-SQL [DELETE](https://msdn.microsoft.com/library/ms189835.aspx) para eliminar el nuevo producto.

1. Reemplace la consulta anterior por esta otra.

   ```sql
   DELETE FROM [SalesLT].[Product]
   WHERE Name = 'myNewProduct';
   ```

2. Seleccione **Ejecutar** para eliminar la fila especificada en la tabla `Product`. El panel **Mensajes** muestra **(1 fila afectada)** .

## <a name="next-steps"></a>Pasos siguientes

- Para más información sobre SSMS, consulte [SQL Server Management Studio](https://msdn.microsoft.com/library/ms174173.aspx).
- Para conectarse y consultar mediante Azure Portal, consulte [Conexión y consulta con el editor de consultas SQL de Azure Portal](sql-database-connect-query-portal.md).
- Para conectarse y consultar con Visual Studio, vea [Conexión y consultas con Visual Studio Code](sql-database-connect-query-vscode.md).
- Para conectarse y consultar con .NET, vea [Conexión y consultas con .NET](sql-database-connect-query-dotnet.md).
- Para conectarse y consultar con PHP, vea [Conexión y consultas con PHP](sql-database-connect-query-php.md).
- Para conectarse y consultar con Node.js, vea [Conexión y consultas con Node.js](sql-database-connect-query-nodejs.md).
- Para conectarse y consultar con Java, vea [Conexión y consultas con Java](sql-database-connect-query-java.md).
- Para conectarse y consultar con Python, vea [Conexión y consultas con Python](sql-database-connect-query-python.md).
- Para conectarse y consultar con Ruby, vea [Conexión y consultas con Ruby](sql-database-connect-query-ruby.md).

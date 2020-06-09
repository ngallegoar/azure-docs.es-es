---
title: Uso de Java para consultar una base de datos
description: Se muestra cómo usar Java para crear un programa que se conecta a una base de datos de Azure SQL Database o Instancia administrada de Azure SQL y realiza consultas mediante instrucciones T-SQL.
titleSuffix: Azure SQL Database & SQL Managed Instance
services: sql-database
ms.service: sql-database
ms.subservice: development
ms.devlang: java
ms.topic: quickstart
author: stevestein
ms.author: sstein
ms.reviewer: v-masebo
ms.date: 05/29/2020
ms.custom: seo-java-july2019. seo-java-august2019, sqldbrb=2 
ms.openlocfilehash: 6be52d2d3472888607bbd6276b4794184bb11273
ms.sourcegitcommit: 309cf6876d906425a0d6f72deceb9ecd231d387c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/01/2020
ms.locfileid: "84267399"
---
# <a name="quickstart-use-java-to-query-a-database-in-azure-sql-database-or-azure-sql-managed-instance"></a>Inicio rápido: Uso de Java para consultar una base de datos de Azure SQL Database o Instancia administrada de Azure SQL
[!INCLUDE[appliesto-sqldb-sqlmi](../includes/appliesto-sqldb-sqlmi.md)]

En este inicio rápido se utiliza Java para conectarse a una base de datos de Azure SQL Database o Instancia administrada de Azure SQL y se usan instrucciones T-SQL para consultar los datos.

## <a name="prerequisites"></a>Requisitos previos

Para completar este inicio rápido necesita instalar:

- Una cuenta de Azure con una suscripción activa. [Cree una cuenta gratuita](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio).

  || SQL Database | Instancia administrada de SQL | SQL Server en máquina virtual de Azure |
  |:--- |:--- |:---|:---|
  | Crear| [Portal](single-database-create-quickstart.md) | [Portal](../managed-instance/instance-create-quickstart.md) | [Portal](../virtual-machines/windows/sql-vm-create-portal-quickstart.md)
  || [CLI](scripts/create-and-configure-database-cli.md) | [CLI](https://medium.com/azure-sqldb-managed-instance/working-with-sql-managed-instance-using-azure-cli-611795fe0b44) |
  || [PowerShell](scripts/create-and-configure-database-powershell.md) | [PowerShell](../managed-instance/scripts/create-configure-managed-instance-powershell.md) | [PowerShell](../virtual-machines/windows/sql-vm-create-powershell-quickstart.md)
  | Configuración | [Regla de firewall de IP en el nivel de servidor](firewall-create-server-level-portal-quickstart.md)| [Conectividad desde una máquina virtual](../managed-instance/connect-vm-instance-configure.md)|
  |||[Conectividad desde un entorno local](../managed-instance/point-to-site-p2s-configure.md) | [Conexión a una instancia de SQL Server](../virtual-machines/windows/sql-vm-create-portal-quickstart.md)
  |Cargar datos|Adventure Works cargado por inicio rápido|[Restauración de World Wide Importers](../managed-instance/restore-sample-database-quickstart.md) | [Restauración de World Wide Importers](../managed-instance/restore-sample-database-quickstart.md) |
  |||Restauración o importación de Adventure Works desde un archivo [BACPAC](database-import.md) desde [GitHub](https://github.com/Microsoft/sql-server-samples/tree/master/samples/databases/adventure-works)| Restauración o importación de Adventure Works desde un archivo [BACPAC](database-import.md) desde [GitHub](https://github.com/Microsoft/sql-server-samples/tree/master/samples/databases/adventure-works)|
  |||

- Software relacionado con [Java](/sql/connect/jdbc/microsoft-jdbc-driver-for-sql-server)

  # <a name="macos"></a>[macOS](#tab/macos)

  Instale Homebrew y Java y, después, instale Maven con los pasos **1.2** y **1.3** de [Creación de aplicaciones de Java con SQL Server en macOS](https://www.microsoft.com/sql-server/developer-get-started/java/mac/).

  # <a name="ubuntu"></a>[Ubuntu](#tab/ubuntu)

  Instale Java, instale el kit de desarrollo de Java y, después, instale Maven con los pasos **1,2**, **1.3** y **1.4** de [Creación de aplicaciones de Java con SQL Server en Ubuntu](https://www.microsoft.com/sql-server/developer-get-started/java/ubuntu/).

  # <a name="windows"></a>[Windows](#tab/windows)

  Instale Java y, después, instale Maven con los pasos **1.2** y **1.3** de [Creación de aplicaciones de Java con SQL Server en Windows](https://www.microsoft.com/sql-server/developer-get-started/java/windows/).

  ---

> [!IMPORTANT]
> Los scripts de este artículo se escriben para utilizar la base de datos **Adventure Works**.

> [!NOTE]
> También puede elegir una instancia administrada de Azure SQL.
>
> Para crearlo y configurarlo, use [Azure Portal](../managed-instance/instance-create-quickstart.md), [PowerShell](../managed-instance/scripts/create-configure-managed-instance-powershell.md)o la [CLI](https://medium.com/azure-sqldb-managed-instance/working-with-sql-managed-instance-using-azure-cli-611795fe0b44) y, después, configure una conectividad [local](../managed-instance/point-to-site-p2s-configure.md) o de [máquina virtual](../managed-instance/connect-vm-instance-configure.md).
>
> Para cargar los datos, consulte la [restauración con BACPAC](database-import.md) con el archivo [Adventure Works](https://github.com/Microsoft/sql-server-samples/tree/master/samples/databases/adventure-works) o consulte la [restauración de la base de datos de Wide World Importers](../managed-instance/restore-sample-database-quickstart.md).

## <a name="get-server-connection-information"></a>Obtención de información de conexión del servidor

Obtenga la información de conexión necesaria para conectarse a la base de datos de Azure SQL Database. En los procedimientos siguientes, necesitará el nombre completo del servidor o nombre de host, el nombre de la base de datos y la información de inicio de sesión.

1. Inicie sesión en [Azure Portal](https://portal.azure.com/).

2. Seleccione **Bases de datos SQL** o abra la página **Instancias administradas de SQL**.

3. En la página **Información general**, revise el nombre completo del servidor junto a **Nombre del servidor** para una base de datos de Azure SQL Database o el nombre completo (o la dirección IP) del servidor junto a **Host** para una instancia administrada de Azure SQL o SQL Server en la máquina virtual de Azure. Para copiar el nombre del servidor o nombre de host, mantenga el cursor sobre él y seleccione el icono **Copiar**.

> [!NOTE]
> Para obtener información de la conexión de SQL Server en una máquina virtual de Azure, consulte [Conexión a SQL Server](../virtual-machines/windows/sql-vm-create-portal-quickstart.md#connect-to-sql-server).

## <a name="create-the-project"></a>Creación del proyecto

1. Desde el símbolo del sistema, cree un nuevo proyecto de Maven llamado *sqltest*.

    ```bash
    mvn archetype:generate "-DgroupId=com.sqldbsamples" "-DartifactId=sqltest" "-DarchetypeArtifactId=maven-archetype-quickstart" "-Dversion=1.0.0" --batch-mode
    ```

1. Cambie a la carpeta *sqltest* y abra *pom.xml* con el editor de texto. Agregue **Microsoft JDBC Driver para SQL Server** a las dependencias del proyecto mediante el código siguiente.

    ```xml
    <dependency>
        <groupId>com.microsoft.sqlserver</groupId>
        <artifactId>mssql-jdbc</artifactId>
        <version>7.0.0.jre8</version>
    </dependency>
    ```

1. También en *pom.xml*, agregue las siguientes propiedades al proyecto. Si no tiene una sección de propiedades, puede agregarla después de las dependencias.

   ```xml
   <properties>
       <maven.compiler.source>1.8</maven.compiler.source>
       <maven.compiler.target>1.8</maven.compiler.target>
   </properties>
   ```

1. Guarde y cierre el archivo *pom.xml*.

## <a name="add-code-to-query-the-database"></a>Adición de código para consultar la base de datos

1. Ya debe tener un archivo denominado *App.java* en el proyecto Maven ubicado en:

   *..\sqltest\src\main\java\com\sqldbsamples\App.java*

1. Abra el archivo y reemplace el contenido por el código siguiente. Agregue los valores apropiados para el servidor, la base de datos, el usuario y la contraseña.

    ```java
    package com.sqldbsamples;

    import java.sql.Connection;
    import java.sql.Statement;
    import java.sql.PreparedStatement;
    import java.sql.ResultSet;
    import java.sql.DriverManager;

    public class App {

        public static void main(String[] args) {

            // Connect to database
            String hostName = "your_server.database.windows.net"; // update me
            String dbName = "your_database"; // update me
            String user = "your_username"; // update me
            String password = "your_password"; // update me
            String url = String.format("jdbc:sqlserver://%s:1433;database=%s;user=%s;password=%s;encrypt=true;"
                + "hostNameInCertificate=*.database.windows.net;loginTimeout=30;", hostName, dbName, user, password);
            Connection connection = null;

            try {
                connection = DriverManager.getConnection(url);
                String schema = connection.getSchema();
                System.out.println("Successful connection - Schema: " + schema);

                System.out.println("Query data example:");
                System.out.println("=========================================");

                // Create and execute a SELECT SQL statement.
                String selectSql = "SELECT TOP 20 pc.Name as CategoryName, p.name as ProductName "
                    + "FROM [SalesLT].[ProductCategory] pc "  
                    + "JOIN [SalesLT].[Product] p ON pc.productcategoryid = p.productcategoryid";

                try (Statement statement = connection.createStatement();
                ResultSet resultSet = statement.executeQuery(selectSql)) {

                    // Print results from select statement
                    System.out.println("Top 20 categories:");
                    while (resultSet.next())
                    {
                        System.out.println(resultSet.getString(1) + " "
                            + resultSet.getString(2));
                    }
                    connection.close();
                }
            }
            catch (Exception e) {
                e.printStackTrace();
            }
        }
    }
    ```

   > [!NOTE]
   > El ejemplo de código usa la base de datos de ejemplo **AdventureWorksLT** en Azure SQL Database.

## <a name="run-the-code"></a>Ejecución del código

1. En el símbolo del sistema, ejecute la aplicación.

    ```bash
    mvn package -DskipTests
    mvn -q exec:java "-Dexec.mainClass=com.sqldbsamples.App"
    ```

1. Compruebe que se han devuelto las 20 primeras filas y cierre la ventana de la aplicación.

## <a name="next-steps"></a>Pasos siguientes

- [Diseño de la primera base de datos en Azure SQL Database](design-first-database-tutorial.md)  
- [Controlador JDBC de Microsoft para SQL Server](https://github.com/microsoft/mssql-jdbc)  
- [Informe de los problemas y realización de preguntas](https://github.com/microsoft/mssql-jdbc/issues)  

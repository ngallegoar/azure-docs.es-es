---
title: Uso de Java para consultar una base de datos
description: En este tema se muestra cómo usar Java para crear un programa que se conecta a una base de datos de Azure SQL y realiza consultas mediante instrucciones T-SQL.
services: sql-database
ms.service: sql-database
ms.subservice: development
ms.devlang: java
ms.topic: quickstart
author: ajlam
ms.author: andrela
ms.reviewer: v-masebo
ms.date: 03/25/2019
ms.custom: seo-java-july2019. seo-java-august2019
ms.openlocfilehash: 034f92ca3b7552373ae69148d09d58d3a5dd166a
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/26/2020
ms.locfileid: "76768655"
---
# <a name="quickstart-use-java-to-query-an-azure-sql-database"></a>Guía de inicio rápido: Uso de Java para consultar una base de datos de Azure SQL Database

En este inicio rápido se muestra cómo utilizar Java para conectarse a una base de datos de Azure SQL y luego utilizar instrucciones T-SQL para consultar los datos.

## <a name="prerequisites"></a>Prerequisites

- Una cuenta de Azure con una suscripción activa. [Cree una cuenta gratuita](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio).
- Una [instancia de Azure SQL Database](sql-database-single-database-get-started.md).
- Software relacionado con [Java](/sql/connect/jdbc/microsoft-jdbc-driver-for-sql-server)

  # <a name="macos"></a>[macOS](#tab/macos)

  Instale Homebrew y Java y, a continuación, instale Maven con los pasos **1.2** y **1.3** de [Creación de aplicaciones de Java con SQL Server en macOS](https://www.microsoft.com/sql-server/developer-get-started/java/mac/).

  # <a name="ubuntu"></a>[Ubuntu](#tab/ubuntu)

  Instale Java, el kit de desarrollo de Java y, a continuación, instale Maven con los pasos **1,2**, **1.3** y **1.4** de [Creación de aplicaciones de Java con SQL Server en Ubuntu](https://www.microsoft.com/sql-server/developer-get-started/java/ubuntu/).

  # <a name="windows"></a>[Windows](#tab/windows)

  Instale Java y, a continuación, instale Maven con los pasos **1.2** y **1.3** de [Creación de aplicaciones de Java con SQL Server en Windows](https://www.microsoft.com/sql-server/developer-get-started/java/windows/).

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

2. Seleccione **Bases de datos SQL** o abra la página **Instancias administradas de SQL**.

3. En la página **Información general**, revise el nombre completo del servidor junto a **Nombre del servidor** para una única base de datos o el nombre completo del servidor junto a **Host** para una instancia administrada. Para copiar el nombre del servidor o nombre de host, mantenga el cursor sobre él y seleccione el icono **Copiar**. 

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

## <a name="add-code-to-query-database"></a>Adición de código a una base de datos de consulta

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
   > El ejemplo de código usa la base de datos de ejemplo para SQL Azure **AdventureWorksLT**.

## <a name="run-the-code"></a>Ejecución del código

1. En el símbolo del sistema, ejecute la aplicación.

    ```bash
    mvn package -DskipTests
    mvn -q exec:java "-Dexec.mainClass=com.sqldbsamples.App"
    ```

1. Compruebe que se han devuelto las 20 primeras filas y cierre la ventana de la aplicación.

## <a name="next-steps"></a>Pasos siguientes

- [Diseño de su primera base de datos de Azure SQL](sql-database-design-first-database.md)  

- [Controlador JDBC de Microsoft para SQL Server](https://github.com/microsoft/mssql-jdbc)  

- [Informe de los problemas y realización de preguntas](https://github.com/microsoft/mssql-jdbc/issues)  

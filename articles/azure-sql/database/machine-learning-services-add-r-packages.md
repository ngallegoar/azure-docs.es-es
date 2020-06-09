---
title: Incorporación de un paquete de R a Machine Learning Services (versión preliminar)
titleSuffix: Azure SQL Database Machine Learning Services (preview)
description: En este artículo se explica cómo instalar un paquete de R que aún no se haya instalado en Azure SQL Database Machine Learning Services (versión preliminar).
services: sql-database
ms.service: sql-database
ms.subservice: machine-learning
ms.custom: ''
ms.devlang: r
ms.topic: tutorial
author: garyericson
ms.author: garye
ms.reviewer: davidph
manager: cgronlun
ms.date: 04/29/2019
ROBOTS: NOINDEX
ms.openlocfilehash: 289b1346fce2e79a2c1e546f5e42e98734ef800c
ms.sourcegitcommit: 053e5e7103ab666454faf26ed51b0dfcd7661996
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/27/2020
ms.locfileid: "84024421"
---
# <a name="add-an-r-package-to-azure-sql-database-machine-learning-services-preview"></a>Incorporación de un paquete de R a Azure SQL Database Machine Learning Services (versión preliminar)
[!INCLUDE[appliesto-sqldb](../includes/appliesto-sqldb.md)]

En este artículo se explica cómo agregar un paquete de R que aún no se haya instalado en Azure SQL Database Machine Learning Services (versión preliminar).

[!INCLUDE[ml-preview-note](../../../includes/sql-database-ml-preview-note.md)]

## <a name="prerequisites"></a>Requisitos previos

- Instalar [R](https://www.r-project.org) y [RStudio Desktop](https://www.rstudio.com/products/rstudio/download/) en la máquina local. R está disponible para Windows, macOS y Linux. En este artículo se da por supuesto que usa Windows.

- En este artículo se incluye un ejemplo de uso de [Azure Data Studio](https://docs.microsoft.com/sql/azure-data-studio/what-is) o [SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/sql-server-management-studio-ssms) (SSMS) para ejecutar un script de R en Azure SQL Database. Puede ejecutar los scripts de R con herramientas de consulta o de otro tipo de administración de base de datos, pero en este ejemplo se da por hecho que se usa Azure Data Studio o SSMS.
   
> [!NOTE]
> No se puede instalar un paquete mediante la ejecución de un script de R con **sp_execute_external_script** en Azure Data Studio o SSMS. Solo puede instalar y quitar paquetes mediante la línea de comandos de R y RStudio como se describe en este artículo. Una vez instalado el paquete, puede acceder a las funciones del paquete de un script de R con **sp_execute_external_script**.

## <a name="list-r-packages"></a>Lista de paquetes de R

Microsoft proporciona una serie de paquetes de R preinstalados con Machine Learning Services en la instancia de Azure SQL Database.
Ejecute el siguiente comando en Azure Data Studio o SSMS para ver una lista de los paquetes de R instalados.

1. Abra Azure Data Studio o SSMS y conéctese a la instancia de Azure SQL Database.

1. Ejecute el siguiente comando:

```sql
EXECUTE sp_execute_external_script @language = N'R'
    , @script = N'
OutputDataSet <- data.frame(installed.packages()[,c("Package", "Version", "Depends", "License")]);'
WITH RESULT SETS((
            Package NVARCHAR(255)
            , Version NVARCHAR(100)
            , Depends NVARCHAR(4000)
            , License NVARCHAR(1000)
            ));
```

La salida debe tener una apariencia similar a la siguiente.

**Resultados**

![Paquetes instalados en R](./media/machine-learning-services-add-r-packages/r-installed-packages.png)

## <a name="add-a-package-with-sqlmlutils"></a>Incorporación de un paquete con sqlmlutils

Si necesita usar un paquete que no esté ya instalado en su instancia de Azure SQL Database, puede instalarlo mediante [sqlmlutils](https://github.com/Microsoft/sqlmlutils). **sqlmlutils** es un paquete diseñado para ayudar a los usuarios a interactuar con bases de datos Microsoft SQL Server y SQL Server, y a ejecutar código de R o de Python en SQL desde un cliente de R o de Python. Actualmente,en Azure SQL Database solo se admite la versión de R de **sqlmlutils**.

En el siguiente ejemplo instalará el paquete **[glue](https://cran.r-project.org/web/packages/glue/)** , que formatea e interpola cadenas. Con estos pasos se instalan **sqlmlutils** y **RODBCext** (un requisito previo para **sqlmlutils**) y se agrega el paquete **glue**.

### <a name="install-sqlmlutils"></a>Instalación de **sqlmlutils**

1. Descargue la última versión del archivo ZIP **sqlmlutils** de https://github.com/Microsoft/sqlmlutils/tree/master/R/dist en la máquina local. No es necesario descomprimir el archivo.

1. Abra un **símbolo del sistema** y ejecute los siguientes comandos para instalar **RODBCext** y **sqlmlutils** en la máquina local. Sustituya la ruta de acceso completa al archivo ZIP **sqlmlutils** descargado (en el ejemplo se da por hecho que el archivo se encuentra en la carpeta Documentos).
    
    ```console
    R -e "install.packages('RODBCext', repos='https://cran.microsoft.com')"
    R CMD INSTALL %UserProfile%\Documents\sqlmlutils_0.5.1.zip
    ```

    La salida que se ve debe ser similar a la siguiente.

    ```text
    In R CMD INSTALL
    * installing to library 'C:/Users/<username>/Documents/R/win-library/3.5'
    package sqlmlutils successfully unpacked and MD5 sums checked
    ```

    > [!TIP]
    > Si recibe el error: "'R' no se reconoce como un comando interno o externo, programa o archivo por lotes ejecutable", es probable que la ruta a R.exe no esté incluida en su variable de entorno **PATH** en Windows. Puede agregar la ruta de acceso a la variable de entorno o ir a la carpeta en el símbolo del sistema (por ejemplo, `cd C:\Program Files\R\R-3.5.3\bin`) y después volver a intentar ejecutar el comando.

### <a name="add-the-package"></a>Incorporación del paquete

1. Abra RStudio y cree un nuevo archivo de **script de R**. 

1. Use el siguiente código de R para instalar un paquete **glue** mediante **sqlmlutils**. Sustituya los valores por la información de conexión de Azure SQL Database propia.

    ```R
    library(sqlmlutils)
    connection <- connectionInfo(
    server= "yourserver.database.windows.net",
    database = "yourdatabase",
    uid = "yoursqluser",
    pwd = "yoursqlpassword")
    
    sql_install.packages(connectionString = connection, pkgs = "glue", verbose = TRUE, scope = "PUBLIC")
    ```

    > [!TIP]
    > El **ámbito** puede ser **PÚBLICO** o **PRIVADO**. El ámbito público es útil para que el administrador de bases de datos instale paquetes que todos los usuarios pueden usar. El ámbito privado hace que el paquete esté disponible solo para el usuario que lo instala. Si no especifica el ámbito, el ámbito predeterminado es **PRIVADO**.

### <a name="verify-the-package"></a>Comprobación del paquete

Ejecute el siguiente script de R en RStudio para comprobar que el paquete **glue** se ha instalado. Use la misma **conexión** definida en el paso anterior.

```R
r<-sql_installed.packages(connectionString = connection, fields=c("Package", "Version", "Depends", "License"))
View(r)
```

**Resultados**

![Contenido de la tabla RTestData](./media/machine-learning-services-add-r-packages/r-verify-package-install.png)

### <a name="use-the-package"></a>Uso del paquete

Una vez instalado el paquete, puede usarlo en un script de R mediante **sp_execute_external_script**.

1. Abra Azure Data Studio o SSMS y conéctese a la instancia de Azure SQL Database.

1. Ejecute el siguiente comando:

    ```sql
    EXECUTE sp_execute_external_script @language = N'R'
        , @script = N'
    library(glue)
    
    name <- "Fred"
    age <- 50
    anniversary <- as.Date("2020-06-14")
    text <- glue(''My name is {name}, '',
    ''my age next year is {age + 1}, '',
    ''my anniversary is {format(anniversary, "%A, %B %d, %Y")}.'')
    
    print(text)
    ';
    ```

    Verá el siguiente resultado en la pestaña **Mensajes**.

    **Resultados**

    ```text
    My name is Fred, my age next year is 51, my anniversary is Sunday, June 14, 2020.
    ```

### <a name="remove-the-package"></a>Eliminación del paquete

Si desea quitar el paquete, ejecute el siguiente script de R en RStudio. Use la misma **conexión** definida en el paso anterior.

```R
sql_remove.packages(connectionString = connection, pkgs = "glue", scope = "PUBLIC")
```

> [!TIP]
> Otra manera de instalar un paquete de R en la instancia de Azure SQL Database es cargarlo desde una secuencia de bytes con la instrucción de T-SQL **CREATE EXTERNAL LIBRARY**. Consulte la sección [Create a library from a byte stream](/sql/t-sql/statements/create-external-library-transact-sql#create-a-library-from-a-byte-stream) (Creación de una biblioteca desde una secuencia de bytes) de la documentación de referencia [CREATE EXTERNAL LIBRARY](https://docs.microsoft.com/sql/t-sql/statements/create-external-library-transact-sql) (CREAR BIBLIOTECA EXTERNA).

## <a name="next-steps"></a>Pasos siguientes

Para más información acerca de Azure SQL Database Machine Learning Services con R (versión preliminar), consulte los siguientes artículos.

- [Azure SQL Database Machine Learning Services con R (versión preliminar)](machine-learning-services-overview.md)
- [Escribir funciones de R avanzadas en Azure SQL Database Machine Learning Services (versión preliminar)](machine-learning-services-functions.md)
- [Trabajar con datos SQL y R en Azure SQL Database Machine Learning Services (versión preliminar)](machine-learning-services-data-issues.md)
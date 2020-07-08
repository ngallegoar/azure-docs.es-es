---
title: Uso de R con Azure SQL Database Machine Learning Services (versión preliminar) para consultar una base de datos
titleSuffix: Azure SQL Database Machine Learning Services (preview)
description: En este artículo se muestra cómo usar un script de R con Azure SQL Database Machine Learning Services para conectarse a una base de datos de Azure SQL Database y consultarla mediante instrucciones Transact-SQL.
services: sql-database
ms.service: sql-database
ms.subservice: machine-learning
ms.custom: sqldbrb=2 
ms.devlang: python
ms.topic: quickstart
author: garyericson
ms.author: garye
ms.reviewer: davidph, carlrab
manager: cgronlun
ms.date: 05/29/2019
ROBOTS: NOINDEX
ms.openlocfilehash: adc3cbd7b98d915a0b598227b459891cb0e0bfff
ms.sourcegitcommit: e132633b9c3a53b3ead101ea2711570e60d67b83
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/07/2020
ms.locfileid: "86042516"
---
# <a name="quickstart-use-r-with-azure-sql-database-machine-learning-services-preview-to-query-a-database"></a>Inicio rápido: Uso de R con Azure SQL Database Machine Learning Services (versión preliminar) para consultar una base de datos 

[!INCLUDE[appliesto-sqldb](../includes/appliesto-sqldb.md)]

En este inicio rápido, se utiliza R con Azure SQL Database Machine Learning Services para conectarse a una base de datos de Azure SQL Database y usar instrucciones T-SQL para consultar los datos.

[!INCLUDE[ml-preview-note](../../../includes/sql-database-ml-preview-note.md)]

## <a name="prerequisites"></a>Requisitos previos

- Una cuenta de Azure con una suscripción activa. [Cree una cuenta gratuita](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio).
- Una base de datos de [Azure SQL Database](single-database-create-quickstart.md)
- [Machine Learning Services](machine-learning-services-overview.md) con R habilitado.
- [SQL Server Management Studio](/sql/ssms/sql-server-management-studio-ssms) (SSMS)

> [!IMPORTANT]
> Los scripts de este artículo se escriben para utilizar la base de datos **Adventure Works**.

Machine Learning Services con R es una característica de Azure SQL Database que se usa para ejecutar scripts de R en bases de datos. Para más información, consulte el [proyecto R](https://www.r-project.org/).

## <a name="get-the-sql-server-connection-information"></a>Obtención de información de conexión de SQL Server

Obtenga la información de conexión necesaria para conectarse a la base de datos de Azure SQL Database. En los procedimientos siguientes, necesitará el nombre completo del servidor o nombre de host, el nombre de la base de datos y la información de inicio de sesión.

1. Inicie sesión en [Azure Portal](https://portal.azure.com/).

2. Vaya a las páginas **Bases de datos SQL** o **Instancias administradas de SQL**.

3. En la página **Información general**, revise el nombre completo del servidor junto a **Nombre del servidor** para una base de datos de Azure SQL Database o el nombre completo del servidor junto a **Host** para una instancia administrada de Instancia administrada de Azure SQL. Para copiar el nombre del servidor o nombre de host, mantenga el cursor sobre él y seleccione el icono **Copiar**.

## <a name="create-code-to-query-your-database"></a>Creación de código para consultar la base de datos

1. Abra **SQL Server Management Studio** y conéctese a la base de datos.

   Si necesita ayuda para conectarse, consulte [Inicio rápido: Uso de SQL Server Management Studio para conectarse a una base de datos de Azure SQL Database y realizar consultas en ella](connect-query-ssms.md).

1. Pase el script de R completo al procedimiento almacenado [sp_execute_external_script](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql).

   El script se pasa mediante el argumento `@script`. Todo lo que contenga el argumento `@script` tiene que ser código de R válido.
   
   >[!IMPORTANT]
   >El código de este ejemplo usa los datos de AdventureWorksLT de ejemplo, que puede elegir como origen al crear la base de datos. Si la base de datos tiene datos distintos, use las tablas de su propia base de datos en la consulta SELECT. 

    ```sql
    EXECUTE sp_execute_external_script
    @language = N'R'
    , @script = N'OutputDataSet <- InputDataSet;'
    , @input_data_1 = N'SELECT TOP 20 pc.Name as CategoryName, p.name as ProductName FROM [SalesLT].[ProductCategory] pc JOIN [SalesLT].[Product] p ON pc.productcategoryid = p.productcategoryid'
    ```

   > [!NOTE]
   > Si recibe algún error, es posible que la versión preliminar pública de Machine Learning Services (con R) no esté habilitada para la base de datos. Consulte los [requisitos previos](#prerequisites) anteriores.

## <a name="run-the-code"></a>Ejecución del código

1. Ejecute el procedimiento almacenado [sp_execute_external_script](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql).

1. Compruebe que se devuelven las 20 primeras filas de categoría o productos en la ventana de **mensajes**.

## <a name="next-steps"></a>Pasos siguientes

- [Diseño de la primera base de datos en Azure SQL Database](design-first-database-tutorial.md)
- [Machine Learning Services (con R) en Azure SQL Database](machine-learning-services-overview.md)
- [Crear y ejecutar scripts de R sencillos en Azure SQL Database Machine Learning Services (versión preliminar)](r-script-create-quickstart.md)

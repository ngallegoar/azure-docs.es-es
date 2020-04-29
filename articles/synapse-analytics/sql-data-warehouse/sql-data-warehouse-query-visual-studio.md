---
title: Conexión con VSTS
description: Consulte Azure Synapse Analytics con Visual Studio.
services: synapse-analytics
author: kevinvngo
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: ''
ms.date: 08/15/2019
ms.author: kevin
ms.reviewer: igorstan
ms.custom: seo-lt-2019
ms.openlocfilehash: 174ee07e389e598fed6ed8487e60303fbce81f77
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/28/2020
ms.locfileid: "81416050"
---
# <a name="connect-to-azure-synapse-analytics-with-visual-studio-and-ssdt"></a>Conexión a Azure Synapse Analytics con Visual Studio y SSDT
> [!div class="op_single_selector"]
> * [Power BI](sql-data-warehouse-get-started-visualize-with-power-bi.md)
> * [Azure Machine Learning](sql-data-warehouse-get-started-analyze-with-azure-machine-learning.md)
> * [Visual Studio](sql-data-warehouse-query-visual-studio.md)
> * [sqlcmd](../sql/get-started-connect-sqlcmd.md) 
> * [SSMS](sql-data-warehouse-query-ssms.md)
> 
> 

Use Visual Studio para realizar consultas en un grupo de SQL dentro de Azure Synapse en solo unos minutos. Este método usa la extensión SQL Server Data Tools (SSDT) de Visual Studio 2019. 

## <a name="prerequisites"></a>Prerrequisitos
Para utilizar este tutorial, necesitará:

* Un grupo de SQL existente. Para crear uno, vea [Creación de un grupo de SQL](create-data-warehouse-portal.md).
* SSDT para Visual Studio. Si tiene Visual Studio, probablemente ya tenga SSDT para Visual Studio. Para ver instrucciones y opciones de instalación, consulte [Instalación de Visual Studio 2015 y SSDT para Almacenamiento de datos SQL](sql-data-warehouse-install-visual-studio.md).
* El nombre del servidor SQL completo. Para encontrar esta información, vea [Conexión a un grupo de SQL](../sql/connect-overview.md).

## <a name="1-connect-to-your-sql-pool"></a>1. Conexión al grupo de SQL
1. Abra Visual Studio 2019.
2. Abra el Explorador de objetos de SQL Server al seleccionar **Ver** > **Explorador de objetos de SQL Server**.
   
    ![Explorador de objetos de SQL Server](./media/sql-data-warehouse-query-visual-studio/open-ssdt.png)
3. Haga clic en el botón **Agregar SQL Server** .
   
    ![Agregar SQL Server](./media/sql-data-warehouse-query-visual-studio/add-server.png)
4. Rellene los campos en la ventana Conectar al servidor.
   
    ![Conectar con el servidor](./media/sql-data-warehouse-query-visual-studio/connection-dialog.png)
   
   * **Nombre del servidor**. Escriba el **nombre del servidor** definido anteriormente.
   * **Autenticación**. Seleccione **Autenticación de SQL Server** o **Autenticación integrada de Active Directory**.
   * **Nombre de usuario** y **Contraseña**. Escriba el nombre de usuario y la contraseña si la autenticación de SQL Server se seleccionó anteriormente.
   * Haga clic en **Conectar**.
5. Para explorar, expanda su Azure SQL Server. Puede ver las bases de datos asociadas al servidor. Expanda AdventureWorksDW para ver las tablas de la base de datos de ejemplo.
   
    ![Explorar AdventureWorksDW](./media/sql-data-warehouse-query-visual-studio/explore-sample.png)

## <a name="2-run-a-sample-query"></a>2. Ejecución de una consulta de ejemplo
Ahora que se ha establecido una conexión a la base de datos, pasemos a escribir una consulta.

1. Haga clic con el botón derecho en la base de datos en el Explorador de objetos de SQL Server.
2. Seleccione **Nueva consulta**. Se abrirá una nueva ventana de consulta.
   
    ![Nueva consulta](./media/sql-data-warehouse-query-visual-studio/new-query2.png)
3. Copie la siguiente consulta T-SQL en la ventana de consulta:
   
    ```sql
    SELECT COUNT(*) FROM dbo.FactInternetSales;
    ```
4. Ejecute la consulta al hacer clic en la flecha verde o use el siguiente acceso directo: `CTRL`+`SHIFT`+`E`.
   
    ![Ejecutar consulta](./media/sql-data-warehouse-query-visual-studio/run-query.png)
5. Consulte los resultados de la consulta. En este ejemplo, la tabla FactInternetSales tiene 60398 filas.
   
    ![Resultados de la consulta](./media/sql-data-warehouse-query-visual-studio/query-results.png)

## <a name="next-steps"></a>Pasos siguientes
Ahora que puede conectarse y realizar consultas, intente realizar la [Visualización de datos con Power BI](sql-data-warehouse-get-started-visualize-with-power-bi.md).

Para configurar el entorno para la autenticación de Azure Active Directory, vea [Autenticación en un grupo de SQL](sql-data-warehouse-authentication.md).

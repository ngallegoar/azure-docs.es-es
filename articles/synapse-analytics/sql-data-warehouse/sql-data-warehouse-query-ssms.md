---
title: Conectarse con SSMS
description: Use SQL Server Management Studio (SSMS) para conectarse a Azure Synapse Analytics y realizar consultas.
services: synapse-analytics
author: XiaoyuMSFT
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: sql-dw
ms.date: 04/17/2018
ms.author: xiaoyul
ms.reviewer: igorstan
ms.custom: seo-lt-2019
ms.openlocfilehash: 772e4253244f5e13a60f155549d5f5513695e5fa
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "85200756"
---
# <a name="connect-to-azure-synapse-analytics-with-sql-server-management-studio-ssms"></a>Conectarse a Azure Synapse Analytics con SQL Server Management Studio (SSMS)

> [!div class="op_single_selector"]
>
> * [Power BI](sql-data-warehouse-get-started-visualize-with-power-bi.md)
> * [Azure Machine Learning](sql-data-warehouse-get-started-analyze-with-azure-machine-learning.md)
> * [Visual Studio](sql-data-warehouse-query-visual-studio.md)
> * [sqlcmd](sql-data-warehouse-get-started-connect-sqlcmd.md)
> * [SSMS](sql-data-warehouse-query-ssms.md)

Use SQL Server Management Studio (SSMS) para conectarse a un almacenamiento de datos de Azure Synapse y realizar consultas.

## <a name="prerequisites"></a>Prerrequisitos

Para utilizar este tutorial, necesitará:

* Un grupo de SQL existente. Para crear uno, vea [Creación de un grupo de SQL](create-data-warehouse-portal.md).
* SQL Server Management Studio (SSMS) instalado. [Descargue SSMS](/sql/ssms/download-sql-server-management-studio-ssms?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest) de forma gratuita si aún no lo tiene.
* El nombre del servidor SQL completo. Para encontrar esta información, vea [Conexión a un grupo de SQL](sql-data-warehouse-connect-overview.md).

## <a name="1-connect-to-your-sql-pool"></a>1. Conexión al grupo de SQL

1. Abra SSMS.
2. Abra el Explorador de objetos al seleccionar **Archivo** > **Conectar Explorador de objetos**.

    ![Explorador de objetos de SQL Server](./media/sql-data-warehouse-query-ssms/connect-object-explorer.png)
3. Rellene los campos en la ventana Conectar al servidor.

   ![Conectar con el servidor](./media/sql-data-warehouse-query-ssms/connect-object-explorer1.png)

   * **Nombre del servidor**. Escriba el **nombre del servidor** definido anteriormente.
   * **Autenticación**. Seleccione **Autenticación de SQL Server** o **Autenticación integrada de Active Directory**.
   * **Nombre de usuario** y **Contraseña**. Escriba el nombre de usuario y la contraseña si la autenticación de SQL Server se seleccionó anteriormente.
   * Haga clic en **Conectar**.
4. Para explorar, expanda su Azure SQL Server. Puede ver las bases de datos asociadas al servidor. Expanda AdventureWorksDW para ver las tablas de la base de datos de ejemplo.

   ![Explorar AdventureWorksDW](./media/sql-data-warehouse-query-ssms/explore-tables.png)

## <a name="2-run-a-sample-query"></a>2. Ejecución de una consulta de ejemplo

Ahora que se ha establecido una conexión a la base de datos, pasemos a escribir una consulta.

1. Haga clic con el botón derecho en la base de datos en el Explorador de objetos de SQL Server.
2. Seleccione **Nueva consulta**. Se abrirá una nueva ventana de consulta.

   ![Nueva consulta](./media/sql-data-warehouse-query-ssms/new-query.png)
3. Copie la siguiente consulta T-SQL en la ventana de consulta:

   ```sql
   SELECT COUNT(*) FROM dbo.FactInternetSales;
   ```

4. Ejecute la consulta al hacer clic en `Execute` o use el siguiente acceso directo: `F5`.

   ![Ejecutar consulta](./media/sql-data-warehouse-query-ssms/execute-query.png)
5. Consulte los resultados de la consulta. En este ejemplo, la tabla FactInternetSales tiene 60398 filas.

   ![Resultados de la consulta](./media/sql-data-warehouse-query-ssms/results.png)

## <a name="next-steps"></a>Pasos siguientes

Ahora que puede conectarse y realizar consultas, intente realizar la [Visualización de datos con Power BI](sql-data-warehouse-get-started-visualize-with-power-bi.md). Para configurar el entorno para la autenticación de Azure Active Directory, vea [Autenticación en un grupo de SQL](sql-data-warehouse-authentication.md).

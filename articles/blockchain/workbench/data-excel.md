---
title: Usar datos de Azure Blockchain Workbench en Microsoft Excel
description: Aprenda a cargar y ver los datos de la base de datos SQL de Azure Blockchain Workbench (versión preliminar) en Microsoft Excel.
ms.date: 09/05/2019
ms.topic: article
ms.reviewer: mmercuri
ms.openlocfilehash: a84858ead83782cc9b6ef1b1d7f905172600fb8a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "74326047"
---
# <a name="view-azure-blockchain-workbench-data-with-microsoft-excel"></a>Ver datos de Azure Blockchain Workbench en Microsoft Excel

Puede usar Microsoft Excel para ver los datos de la base de datos de SQL de Azure Blockchain Workbench. En este artículo se proporcionan los pasos que necesita realizar para lo siguiente:

* Conectarse a la base de datos de Blockchain Workbench desde Microsoft Excel
* Examinar las tablas y vistas de base de datos de Blockchain Workbench
* Cargar los datos de vista de Blockchain Workbench en Excel

## <a name="connect-to-the-blockchain-workbench-database"></a>Conectarse a la base de datos de Blockchain Workbench

Para conectarse a la base de datos de Blockchain Workbench:

1. Abra Microsoft Excel.
2. En la pestaña **Satos**, seleccione **Obtener datos**.
3. Elija **Desde Azure** y seleccione **Desde Azure SQL Database**.

   ![Conexión a una base de datos de Azure SQL](./media/data-excel/connect-sql-db.png)

4. En el cuadro de diálogo **Base de datos de SQL Server**:

    * En **Servidor**, escriba el nombre del servidor de Blockchain Workbench.
    * En **Base de datos (opcional)** , escriba el nombre de la base de datos.

   ![Proporcionar el servidor de bases de datos y la base de datos](./media/data-excel/provide-server-db.png)

5. En la barra de navegación del cuadro de diálogo **Base de datos de SQL Server**, seleccione **Base de datos**. Escriba su **nombre de usuario** y **contraseña**, y haga clic en **Conectar**.

    > [!NOTE]
    > Si usa las credenciales creadas durante el proceso de implementación de Azure Blockchain Workbench, el **nombre de usuario** es `dbadmin`. La **contraseña** es la que creó al implementar Blockchain Workbench.
    
   ![Proporcionar las credenciales de acceso a la base de datos](./media/data-excel/provide-credentials.png)

## <a name="look-at-database-tables-and-views"></a>Examinar las tablas y vistas de la base de datos

Después de conectarse a la base de datos, se abre el cuadro de diálogo del navegador de Excel. Puede usar el navegador para examinar las tablas y vistas en la base de datos. Las vistas están diseñadas para la creación de informes y sus nombres tienen el prefijo **vw**.

   ![Vista previa de navegador de Excel de una vista](./media/data-excel/excel-navigator.png)

## <a name="load-view-data-into-an-excel-workbook"></a>Cargar los datos de vista en un libro de Excel

En el ejemplo siguiente se muestra cómo puede cargar datos desde una vista en un libro de Excel.

1. En la barra de desplazamiento del **navegador**, seleccione la vista **vwContractAction**. En la vista previa de **vwContractAction** se muestran todas las acciones relacionadas con un contrato en la base de datos de Blockchain Workbench.
2. Seleccione **Cargar** para recuperar todos los datos de la vista y colocarlos en el libro de Excel.

   ![Datos cargados desde una vista](./media/data-excel/view-data.png)

Ahora que tiene los datos cargados, puede usar las características de Excel para crear sus propios informes con los datos de transacciones y metadatos de la base de datos de Azure Blockchain Workbench.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Vistas de base de datos de Azure Blockchain Workbench](database-views.md)
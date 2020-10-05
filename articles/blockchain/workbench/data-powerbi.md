---
title: Usar datos de Azure Blockchain Workbench en Microsoft Power BI
description: Obtenga información sobre cómo cargar y ver los datos de la base de datos de SQL de Azure Blockchain Workbench en Microsoft Power BI.
ms.date: 04/22/2020
ms.topic: how-to
ms.reviewer: sunri
ms.openlocfilehash: 7e0e585ce45616c2402972c725b502f4b704d1cd
ms.sourcegitcommit: 03662d76a816e98cfc85462cbe9705f6890ed638
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/15/2020
ms.locfileid: "90532382"
---
# <a name="using-azure-blockchain-workbench-data-with-microsoft-power-bi"></a>Usar datos de Azure Blockchain Workbench con Microsoft Power BI

Microsoft Power BI ofrece la posibilidad de generar fácilmente informes eficaces a partir de bases de datos SQL mediante Power BI Desktop y luego publicarlos en [https://www.powerbi.com](https://www.powerbi.com).

En este artículo se incluye un tutorial paso a paso sobre cómo conectarse a la base de datos de SQL de Azure Blockchain Workbench desde Power BI Desktop, crear un informe e implementar este en powerbi.com.

## <a name="prerequisites"></a>Prerequisites

* Descargue [Power BI Desktop](https://powerbi.microsoft.com/desktop/).

## <a name="connecting-power-bi-to-data-in-azure-blockchain-workbench"></a>Conexión de Power BI a los datos de Azure Blockchain Workbench

1.  Abra Power BI Desktop.
2.  Seleccione **Obtener datos**.

    ![Obtener datos](./media/data-powerbi/get-data.png)
3.  Seleccione **SQL Server** de la lista de tipos de orígenes de datos.

4.  Proporcione el nombre del servidor y la base de datos en el cuadro de diálogo. Especifique si desea importar los datos o realizar una **DirectQuery**. Seleccione **Aceptar**.

    ![Selección de SQL Server](./media/data-powerbi/select-sql.png)

5.  Proporcione las credenciales de base de datos para acceder a Azure Blockchain Workbench. Seleccione **Base de datos** y escriba sus credenciales.

    Si usa las credenciales creadas por el proceso de implementación de Azure Blockchain Workbench, el nombre de usuario es **dbadmin** y la contraseña es la que proporcionó durante la implementación.

    ![Configuración de la base de datos de SQL](./media/data-powerbi/db-settings.png)

6.  Una vez conectado a la base de datos, el cuadro de diálogo **Navegador** muestra las tablas y vistas disponibles en la base de datos. Las vistas están diseñadas para la creación de informes y sus nombres tienen el prefijo **vw**.

    ![Captura de pantalla de Power BI Desktop con el cuadro de diálogo Navegador con vwContractAction seleccionado.](./media/data-powerbi/navigator.png)

7.  Seleccione las vistas que desea incluir. Para fines de demostración, incluimos **vwContractAction**, que proporciona detalles sobre todas las acciones que han tenido lugar en un contrato.

    ![Seleccionar vistas](./media/data-powerbi/select-views.png)

Ahora puede crear y publicar informes, tal y como lo haría normalmente con Power BI.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Vistas de base de datos de Azure Blockchain Workbench](database-views.md)
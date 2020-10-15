---
title: Obtener los detalles de la base de datos de Azure Blockchain Workbench
description: Aprenda a obtener información del servidor de bases de datos y de la base de datos de Azure Blockchain Workbench (versión preliminar).
ms.date: 09/05/2019
ms.topic: how-to
ms.reviewer: mmercuri
ms.openlocfilehash: 898360d8e22803e17ff4da7d3b63f5595cad47ba
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "85254657"
---
# <a name="get-information-about-your-azure-blockchain-workbench-database"></a>Obtener información sobre la base de datos de Azure Blockchain Workbench

En este artículo se muestra cómo obtener información detallada sobre la base de datos de Azure Blockchain Workbench (versión preliminar).

## <a name="overview"></a>Información general

La información sobre las aplicaciones, los flujos de trabajo y la ejecución de contrato inteligente se proporciona mediante vistas de base de datos en la base de datos de SQL de Blockchain Workbench. Los desarrolladores pueden usar esta información cuando usan herramientas como, por ejemplo, Microsoft Excel, Power BI, Visual Studio y SQL Server Management Studio.

Antes de que un desarrollador pueda conectarse a la base de datos, necesita lo siguiente:

* Acceso de cliente externo permitido en el firewall de base de datos. En este artículo sobre la configuración de un artículo de firewall de base de datos se explica cómo permitir el acceso.
* El nombre del servidor de base de datos y de la base de datos.

## <a name="connect-to-the-blockchain-workbench-database"></a>Conectarse a la base de datos de Blockchain Workbench

Para conectarse a la base de datos:

1. Inicie sesión en Azure Portal con una cuenta que tenga permisos de **Propietario** para los recursos de Azure Blockchain Workbench.
2. En el panel de la izquierda, seleccione **Grupos de recursos**.
3. Elija el nombre del grupo de recursos para la implementación de Blockchain Workbench.
4. Seleccione **Tipo** para ordenar la lista de recursos y luego elija el servidor **SQL server**. En la lista ordenada en la captura de pantalla siguiente se muestran dos bases de datos, "master" y otra que usa "lhgn" como el **prefijo de recurso**.

   ![Lista de recursos de Blockchain Workbench ordenados](./media/getdb-details/sorted-workbench-resource-list.png)

5. Para ver información detallada sobre la base de datos de Blockchain Workbench, seleccione el vínculo de la base de datos con el **prefijo de recurso** que proporcionó para la implementación de Blockchain Workbench.

   ![Detalles de la base de datos](./media/getdb-details/workbench-db-details.png)

El nombre del servidor de base de datos y el nombre de la base de datos permiten conectarse a la base de datos de Blockchain Workbench mediante la herramienta de desarrollo o generación de informes.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Vistas de base de datos de Azure Blockchain Workbench](database-views.md)
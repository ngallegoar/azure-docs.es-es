---
title: Carga de datos en Azure SQL Data Warehouse
description: Uso de Azure Data Factory para copiar datos en Azure SQL Data Warehouse
services: data-factory
ms.author: jingwang
author: linda33wj
manager: shwang
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 06/22/2018
ms.openlocfilehash: dc6d8c9da749e7ee60713f4357dc6d46051d89f2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "80131269"
---
# <a name="load-data-into-azure-sql-data-warehouse-by-using-azure-data-factory"></a>Carga de datos en Azure SQL Data Warehouse mediante Azure Data Factory

[Azure SQL Data Warehouse](../synapse-analytics/sql-data-warehouse/sql-data-warehouse-overview-what-is.md) es una base de datos de escalado horizontal y basada en la nube capaz de procesar volúmenes masivos de datos (tanto relacionales como no relacionales). Basado en la arquitectura de procesamiento paralelo masivo (MPP), está mejorado para controlar las cargas de trabajo empresariales. Ofrece elasticidad en la nube con la flexibilidad para escalar almacenamiento y proceso de forma independiente.

La introducción a Azure SQL Data Warehouse es ahora más fácil que nunca si se usa Azure Data Factory. Azure Data Factory es un servicio de integración de datos en la nube totalmente administrado. Puede utilizar el servicio para rellenar un almacenamiento de SQL Data Warehouse con los datos del sistema actual y ahorrar tiempo al crear las soluciones de análisis.

Azure Data Factory ofrece los siguientes beneficios para cargar datos en Azure SQL Data Warehouse:

* **Fácil de configurar**: con un asistente intuitivo en cinco pasos sin necesidad de scripting.
* **Amplia compatibilidad para el almacenamiento de datos**: compatibilidad integrada para un amplio conjunto de almacenes de datos tanto locales como en la nube. Para una lista detallada, consulte la tabla de [almacenes de datos admitidos](copy-activity-overview.md#supported-data-stores-and-formats).
* **Seguro y compatible**: los datos se transfieren a través de HTTPS o ExpressRoute. La presencia del servicio global garantiza que los datos nunca abandonan el límite geográfico.
* **Rendimiento sin precedentes mediante PolyBase**: Polybase es la forma más eficaz de mover datos a Azure SQL Data Warehouse. Mediante la característica de blob de almacenamiento provisional, puede alcanzar velocidades de carga altas para todos los tipos de almacenes de datos, incluido Azure Blob Storage y Data Lake Store. (Polybase es compatible con Azure Blob Storage y Azure Data Lake Store de forma predeterminada). Para más información, consulte el [rendimiento de la actividad de copia](copy-activity-performance.md).

En este artículo se muestra cómo usar la herramienta para copiar datos de Data Factory a fin de _cargar datos de Azure SQL Database en Azure SQL Data Warehouse_. Puede seguir los mismos pasos para copiar datos de otros tipos de almacenes de datos.

> [!NOTE]
> Para más información, consulte [Copia de datos con Azure SQL Data Warehouse como origen o destino mediante Azure Data Factory](connector-azure-sql-data-warehouse.md).

## <a name="prerequisites"></a>Prerrequisitos

* Suscripción de Azure: Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/) antes de empezar.
* Azure SQL Data Warehouse: Este almacén de datos contiene los datos que se copian de SQL Database. Si no tiene ninguna instancia de Azure SQL Data Warehouse, consulte las instrucciones de [Creación de una instancia de SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-get-started-tutorial.md).
* Azure SQL Database: este tutorial copia los datos de una base de datos de Azure SQL con los datos de ejemplo de Adventure Works LT. Puede crear una base de datos SQL si sigue las instrucciones que aparecen en [Creación de una base de datos de Azure SQL](../sql-database/sql-database-get-started-portal.md).
* Cuenta de almacenamiento de Azure: Azure Storage se usa como blob de _almacenamiento provisional_ en la operación de copia masiva. Si no dispone de una cuenta de almacenamiento de Azure, consulte las instrucciones de [Creación de una cuenta de almacenamiento](../storage/common/storage-account-create.md).

## <a name="create-a-data-factory"></a>Crear una factoría de datos

1. En el menú de la izquierda, seleccione **Crear un recurso** > **Datos y análisis** > **Data Factory**:

   ![Selección de la factoría de datos en el panel Nuevo](./media/quickstart-create-data-factory-portal/new-azure-data-factory-menu.png)

2. En la página **Nueva factoría de datos**, proporcione los valores de los campos que se muestran en la imagen siguiente:

   ![Página New data factory (Nueva factoría de datos)](./media/load-azure-sql-data-warehouse/new-azure-data-factory.png)

    * **Name**: escriba un nombre único global para la factoría de datos de Azure. Si recibe el error "El nombre de la factoría de datos \"LoadSQLDWDemo\" no está disponible", escriba un nombre diferente. Por ejemplo, podría utilizar el nombre _**suNombre**_ **ADFTutorialDataFactory**. Intente crear de nuevo la factoría de datos. Para conocer las reglas de nomenclatura de los artefactos de Data Factory, consulte [Data Factory: reglas de nomenclatura](naming-rules.md).
    * **Suscripción**: seleccione la suscripción de Azure donde desea crear la factoría de datos. 
    * **Grupo de recursos**: seleccione un grupo de recursos existente en la lista desplegable o seleccione la opción **Crear nuevo** y escriba el nombre de un grupo de recursos. Para obtener más información sobre los grupos de recursos, consulte [Uso de grupos de recursos para administrar los recursos de Azure](../azure-resource-manager/management/overview.md).  
    * **Versión**: seleccione **V2**.
    * **Ubicación**: Seleccione la ubicación de la factoría de datos. Solo las ubicaciones admitidas se muestran en la lista desplegable. Los almacenes de datos que las factorías de datos usan pueden estar en otras ubicaciones y regiones. Estos almacenes de datos incluyen Azure Data Lake Store, Azure Storage, Azure SQL Database, etc.

3. Seleccione **Crear**.
4. Una vez completada la creación, vaya a la factoría de datos. Verá la página principal de **Factoría de datos**, tal y como se muestra en la siguiente imagen:

   ![Página principal Factoría de datos](./media/load-azure-sql-data-warehouse/data-factory-home-page.png)

   Seleccione el icono **Author & Monitor** (Creación y supervisión) para iniciar la aplicación de integración de datos en una pestaña independiente.

## <a name="load-data-into-azure-sql-data-warehouse"></a>Carga de datos en Azure SQL Data Warehouse

1. En la página **Get started** (Introducción), seleccione el icono **Copy Data** (Copiar datos) para iniciar la herramienta para copiar datos:

   ![Icono de la herramienta Copy Data](./media/load-azure-sql-data-warehouse/copy-data-tool-tile.png)
1. En la página **Properties** (Propiedades), especifique **CopyFromSQLToSQLDW** en el campo **Task name** (Nombre de la tarea) y seleccione **Next** (Siguiente):

    ![Página de propiedades](./media/load-azure-sql-data-warehouse/copy-data-tool-properties-page.png)

1. En la página **Almacén de datos de origen**, realice los pasos siguientes:

    a. Haga clic en **+ Crear nueva conexión**:

    ![Página Source data store (Almacén de datos de origen)](./media/load-azure-sql-data-warehouse/new-source-linked-service.png)

    b. Seleccione **Azure SQL Database** en la galería y seleccione **Continuar**. Puede escribir "SQL" en el cuadro de búsqueda para filtrar los conectores.

    ![Seleccione Azure SQL DB](./media/load-azure-sql-data-warehouse/select-azure-sql-db-source.png)

    c. En la página **New Linked Service** (Nuevo servicio vinculado) seleccione el nombre del servidor y el de la base de datos en la lista desplegable y especifique el nombre de usuario y la contraseña. Haga clic en **Test connection** (Prueba de conexión) para validar la configuración y, después, seleccione **Finish** (Finalizar).

    ![Configurar Azure SQL DB](./media/load-azure-sql-data-warehouse/configure-azure-sql-db.png)

    d. Seleccione el servicio vinculado recién creado como origen y, a continuación, haga clic en **Siguiente**.

    ![Selección del servicio vinculado de origen](./media/load-azure-sql-data-warehouse/select-source-linked-service.png)

1. En la página **Select tables from which to copy the data or use a custom query** (Seleccionar tablas de donde copiar los datos o usar una consulta personalizada), escriba **SalesLT** para filtrar las tablas. Elija el cuadro **(Select all)** (Seleccionar todo) para usar todas las tablas para la copia y, a continuación, seleccione **Next** (Siguiente):

    ![Selección de las tablas de origen](./media/load-azure-sql-data-warehouse/select-source-tables.png)

1. En la página **Almacén de datos de destino**, realice los pasos siguientes:

    a. Haga clic en **+ Crear nueva conexión** para agregar una conexión.

    ![Página del almacén de datos receptor](./media/load-azure-sql-data-warehouse/new-sink-linked-service.png)

    b. Seleccione **Azure SQL Data Warehouse** en la galería y seleccione **Siguiente**.

    ![Seleccionar Azure SQL DW](./media/load-azure-sql-data-warehouse/select-azure-sql-dw-sink.png)

    c. En la página **New Linked Service** (Nuevo servicio vinculado) seleccione el nombre del servidor y el de la base de datos en la lista desplegable y especifique el nombre de usuario y la contraseña. Haga clic en **Test connection** (Prueba de conexión) para validar la configuración y, después, seleccione **Finish** (Finalizar).

    ![Configurar Azure SQL DW](./media/load-azure-sql-data-warehouse/configure-azure-sql-dw.png)

    d. Seleccione el servicio vinculado recién creado como receptor y, a continuación, haga clic en **Siguiente**.

    ![Seleccione el servicio vinculado de receptor](./media/load-azure-sql-data-warehouse/select-sink-linked-service.png)

1. En la página **Table mapping** (Asignación de tabla), revise el contenido y seleccione **Next** (Siguiente). Se muestra una asignación de tabla inteligente. Las tablas de origen se asignan a las tablas de destino en función de los nombres de tabla. Si la tabla de origen no existe en el destino, Azure Data Factory crea una con el mismo nombre de manera predeterminada. También se puede asignar una tabla de origen a una tabla de destino existente.

   > [!NOTE]
   > La creación automática de la tabla para el receptor de SQL Data Warehouse se aplica cuando el origen es SQL Server o Azure SQL Database. Si copia datos desde otro almacén de datos de origen, debe crear previamente el esquema en el receptor de Azure SQL Data Warehouse antes de copiar los datos.

   ![Página Table mapping (Asignación de tabla)](./media/load-azure-sql-data-warehouse/table-mapping.png)

1. En la página **Schema mapping** (Asignación de esquema), revise el contenido y seleccione **Next** (Siguiente). La asignación inteligente de tabla se basa en el nombre de las columnas. Si permite que Data Factory cree automáticamente las tablas, la conversión de tipos de datos puede producirse cuando haya incompatibilidades entre los almacenes de origen y de destino. Si hay una conversión de tipos de datos no compatibles entre las columnas de origen y de destino, verá un mensaje de error junto a la tabla correspondiente.

    ![Página Schema mapping (Asignación de esquema)](./media/load-azure-sql-data-warehouse/schema-mapping.png)

1. En la página **Settings** (Configuración), siga estos pasos:

    a. En la sección **Staging settings** (Configuración provisional), haga clic en **+ New** (+Nuevo) para crear un almacenamiento provisional. El almacenamiento se usa para almacenar provisionalmente los datos antes de cargarlos en SQL Data Warehouse mediante PolyBase. Una vez que se completa la copia, los datos provisionales en Azure Storage se limpian automáticamente.

    ![Configurar almacenamiento provisional](./media/load-azure-sql-data-warehouse/configure-staging.png)

    b. En la página **New Linked Service** (Nuevo servicio vinculado), seleccione su cuenta de almacenamiento y seleccione **Finish** (Finalizar).

    ![Configurar Azure Storage](./media/load-azure-sql-data-warehouse/configure-blob-storage.png)

    c. En la sección **Advanced settings** (Configuración avanzada), anule la selección de la opción **Use type default** (Usar valor predeterminado de type) y seleccione **Next** (Siguiente).

    ![Configurar PolyBase](./media/load-azure-sql-data-warehouse/configure-polybase.png)

1. En la página **Summary** (Resumen), revise la configuración y seleccione **Next** (Siguiente):

    ![Página de resumen](./media/load-azure-sql-data-warehouse/summary-page.png)
1. En la página **Deployment** (Implementación), seleccione **Monitor** (Supervisión) para supervisar la canalización (tarea):

    ![Página Deployment (Implementación)](./media/load-azure-sql-data-warehouse/deployment-page.png)
1. Observe que la pestaña **Monitor** (Supervisión) de la izquierda se selecciona automáticamente. La columna **Actions** (Acciones) incluye los vínculos para ver los detalles de la ejecución de actividad y volver a ejecutar la canalización:

    ![La supervisión de la canalización se ejecuta](./media/load-azure-sql-data-warehouse/pipeline-monitoring.png)
1. Para ver las ejecuciones de actividad asociadas con la ejecución de canalización, seleccione el vínculo **View Activity Runs** (Ver ejecuciones de actividad) en la columna **Actions** (Acciones). Para volver a la vista de ejecuciones de canalización, seleccione el vínculo **Pipelines** (Canalizaciones) de la parte superior. Seleccione **Refresh** (Actualizar) para actualizar la lista.

    ![Supervisión de las ejecuciones de actividad](./media/load-azure-sql-data-warehouse/activity-monitoring.png)

1. Para supervisar los detalles de la ejecución de cada actividad de copia, seleccione el vínculo **Details** (Detalles) en **Actions** (Acciones) en la vista de supervisión de la actividad. Puede supervisar detalles como el volumen de datos copiados desde el origen al receptor, el rendimiento de los datos, los pasos de ejecución con su duración correspondiente y las configuraciones que se utilizan:

    ![Detalles de la supervisión de la ejecución de actividad](./media/load-azure-sql-data-warehouse/monitor-activity-run-details.png)

## <a name="next-steps"></a>Pasos siguientes

Continúe en el artículo siguiente para información sobre la compatibilidad de Azure SQL Data Warehouse:

> [!div class="nextstepaction"]
>[Copia de datos con Azure SQL Data Warehouse como origen o destino mediante Azure Data Factory](connector-azure-sql-data-warehouse.md)

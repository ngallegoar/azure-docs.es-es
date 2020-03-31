---
title: Uso de ingesta con un solo clic para ingerir datos en Azure Data Explorer
description: Obtenga información sobre cómo ingerir (cargar) datos en Azure Data Explorer mediante la ingesta con un solo clic.
author: orspod
ms.author: orspodek
ms.reviewer: tzgitlin
ms.service: data-explorer
ms.topic: conceptual
ms.date: 10/31/2019
ms.openlocfilehash: 4a53f7e68501ce7f9b19dea0822d3896ec241fb8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "75444548"
---
# <a name="use-one-click-ingestion-to-ingest-data-into-azure-data-explorer"></a>Uso de ingesta con un solo clic para ingerir datos en Azure Data Explorer

En este artículo se muestra cómo usar la ingesta con un solo clic para la ingesta rápida de una nueva tabla en formato JSON o CSV. Los datos se pueden ingerir desde el almacenamiento o desde un archivo local en una tabla existente o en una tabla nueva. Si usa el asistente intuitivo y de un solo clic, los datos se ingieren en unos minutos. Una vez que se ingieran los datos, puede editar la tabla y ejecutar consultas mediante la interfaz de usuario web de Azure Data Explorer.

## <a name="prerequisites"></a>Prerequisites

* Si no tiene una suscripción a Azure, cree una [cuenta gratuita de Azure](https://azure.microsoft.com/free/) antes de empezar.
* Inicie sesión en [la aplicación](https://dataexplorer.azure.com/).
* Cree [un clúster y una base de datos de Azure Data Explorer](create-cluster-database-portal.md).
* Inicie sesión en la [interfaz de usuario web](https://dataexplorer.azure.com/) y [agregue una conexión al clúster](/azure/data-explorer/web-query-data#add-clusters).

## <a name="ingest-new-data"></a>Ingesta de nuevos datos

1. Haga clic con el botón derecho en la *base de datos* o la fila de la *tabla* en el menú izquierdo de la interfaz de usuario web y seleccione **Ingest new data (Preview)** [Ingerir datos nuevos (versión preliminar)].

    ![Seleccione la ingesta con un solo clic en la interfaz de usuario web](media/ingest-data-one-click/one-click-ingestion-in-webui.png)   
 
1. En la ventana **Ingest new data (preview)** [Ingerir datos nuevos (versión preliminar)], seleccione la pestaña **Source** (Origen), complete los **detalles del proyecto**:

    * En **Table** (Tabla), seleccione un nombre de tabla en el menú desplegable o **Create new** (Crear nuevo) para crear una tabla.
    * En **Ingestion type** (Tipo de ingesta), seleccione **from storage** (desde almacenamiento) o **from file** (desde archivo).
      * Si seleccionó **from storage** (desde almacenamiento), escriba **Vínculo a almacenamiento** para agregar la dirección URL. Use una [dirección URL de SAS de blob](/azure/vs-azure-tools-storage-explorer-blobs#get-the-sas-for-a-blob-container) para las cuentas de almacenamiento privadas. 
      * Si seleccionó **Desde archivo**, seleccione **Examinar** y arrastre el archivo al cuadro.
    * Seleccione **Editar esquema** para ver y editar la configuración de la columna de la tabla.
 
    ![Detalles del origen de la ingesta con un solo clic](media/ingest-data-one-click/one-click-ingestion-source.png) 

    > [!TIP]
    > Si selecciona **Ingest new data (Preview)** [Ingerir datos nuevos (versión preliminar)], en una fila de *tabla*, el nombre de la tabla seleccionada aparecerá en los **Detalles del proyecto**.

1. Si seleccionó una tabla existente, se abrirá la ventana **Map columns** (Asignar columnas) para asignar columnas de datos de origen a las columnas de la tabla de destino. 
    * Use **Omit column** (Omitir columna) para quitar una columna de destino de la tabla.
    * Use **New column** (Nueva columna) para agregar una nueva columna a la tabla.

    ![Ventana Asignar columnas](media/ingest-data-one-click/one-click-map-columns-window.png)

1. En la pestaña **Schema** (Esquema):

    * Seleccione **Compression type** (Tipo de compresión) en el menú desplegable y, a continuación, **Uncompressed** (Sin comprimir) o **GZip**.
    * Seleccione **Data format** (Formato de datos) en la lista desplegable y, después, seleccione **JSON**, **CSV**, **TSV**, **SCSV**, **SOHSV**, **TSVE** o **PSV**. 
        * Al seleccionar el formato **JSON**, también debe seleccionar los **niveles JSON**, de 1 a 10. Los niveles afectan a la representación de datos de la columna de tabla. 
        * Si selecciona un formato distinto de JSON, debe seleccionar la casilla **Include column names** (Incluir los nombres de columna) para omitir la fila de encabezado del archivo.
    * **Mapping name** (Nombre de asignación) se establece automáticamente, pero se puede editar.
    * Si seleccionó una tabla existente, puede seleccionar el botón **Map columns** (Asignar columnas) para abrir la ventana **Map columns** (Asignar columnas).

    ![Esquema del formato CSV de ingesta con un solo clic](media/ingest-data-one-click/one-click-csv-format.png)

1. Encima del panel del **Editor**, seleccione el botón **v** para abrir el editor. En el editor, puede ver y copiar las consultas automáticas generadas a partir de los valores que ha introducido. 

1. En la tabla: 
    * Haga clic con el botón derecho en los encabezados de columna nuevos para **cambiar el tipo de datos**, **cambiar el nombre de la columna**, **eliminar la columna**, **filtrar de menor a mayor**o **filtrar de mayor a menor**. En las columnas existentes, solo está disponible la ordenación de datos. 
    * Haga doble clic en el nuevo nombre de columna para editarlo.

1. Seleccione **Start ingestion** (Iniciar ingesta) para crear una tabla y una asignación, y comenzar la ingesta de datos.

    ![Esquema del formato JSON de ingesta con un solo clic](media/ingest-data-one-click/one-click-json-format.png) 
 
## <a name="query-data"></a>Consultar datos

1. En la ventana **Data ingestion completed** (Ingesta de datos completada), los tres pasos se marcarán con marcas de verificación verdes si la ingesta de datos se completa correctamente.
 
    ![Ingesta de datos con un solo clic completada](media/ingest-data-one-click/one-click-data-ingestion-complete.png)

1. Seleccione el botón **v** para abrir la consulta. Copie en la interfaz de usuario web para editar la consulta.

1. El menú de la derecha contiene las opciones **Quick queries** (Consultas rápidas) y **Tools** (Herramientas). 

    * **Quick queries** (Consultas rápidas) incluye vínculos a la interfaz de usuario web con consultas de ejemplo.
    * **Tools** (Herramientas) incluye un vínculo a los **comandos de eliminación** en la interfaz de usuario web, que le permiten ejecutar el comando `.drop` correspondiente para solucionar problemas.

    > [!TIP]
    > Podría perder datos cuando use los comandos `.drop`. Úselos con precaución.

## <a name="next-steps"></a>Pasos siguientes

* [Consulta de datos en la interfaz de usuario web de Azure Data Explorer](web-query-data.md)
* [Escritura de consultas para Azure Data Explorer mediante el lenguaje de consulta Kusto](write-queries.md)

---
title: Herramienta de datos para copiar archivos nuevos y actualizados de forma incremental
description: Cree una instancia de Azure Data Factory y, luego, use la herramienta Copiar datos para cargar incrementalmente archivos nuevos según LastModifiedDate.
services: data-factory
author: dearandyxu
ms.author: yexu
ms.reviewer: ''
manager: ''
ms.service: data-factory
ms.workload: data-services
ms.devlang: na
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 11/09/2020
ms.openlocfilehash: f94975b91a332e480a1b570c29f02040a1047f75
ms.sourcegitcommit: dc342bef86e822358efe2d363958f6075bcfc22a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/12/2020
ms.locfileid: "94555420"
---
# <a name="incrementally-copy-new-and-changed-files-based-on-lastmodifieddate-by-using-the-copy-data-tool"></a>Copia incremental de archivos nuevos y modificados según LastModifiedDate con la herramienta Copiar datos

[!INCLUDE[appliesto-adf-xxx-md](includes/appliesto-adf-xxx-md.md)]

En este tutorial, usará Azure Portal para crear una instancia de Data Factory. Luego, usará la herramienta Copiar datos para crear una canalización que copie incrementalmente solo los archivos nuevos y modificados desde una instancia de Azure Blob Storage a otro. Usa `LastModifiedDate` para determinar qué archivos se van a copiar.

Después de completar los pasos que se indican aquí, Azure Data Factory examinará todos los archivos del almacén de origen, aplicará el filtro de archivos por `LastModifiedDate` y copiará en el almacén de destino solo los archivos nuevos o que se han actualizado desde la última vez. Tenga en cuenta que si Data Factory examina un gran número de archivos, debe esperar largos períodos. El análisis de archivos tarda mucho tiempo, incluso cuando se reduce la cantidad de datos copiados.

> [!NOTE]
> Si no está familiarizado con Data Factory, consulte [Introducción a Azure Data Factory](introduction.md).

En este tutorial va a completar estas tareas:

> [!div class="checklist"]
> * Creación de una factoría de datos.
> * Uso de la herramienta Copy Data para crear una canalización.
> * Supervisión de las ejecuciones de canalización y actividad.

## <a name="prerequisites"></a>Prerrequisitos

* **Suscripción de Azure**: Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/) antes de empezar.
* **Cuenta de Azure Storage**: Use Blob Storage para los almacenes de datos de origen y receptor. Si no dispone de una cuenta de Azure Storage, siga las instrucciones que aparecen en [Creación de una cuenta de Azure Storage](../storage/common/storage-account-create.md).

## <a name="create-two-containers-in-blob-storage"></a>Crear dos contenedores en Azure Blob Storage

Complete estos pasos para preparar su instancia de Blob Storage para el tutorial:

1. Cree un contenedor denominado **source**. Puede usar varias herramientas para realizar esta tarea, como el [Explorador de Azure Storage](https://storageexplorer.com/).

2. Cree un contenedor denominado **destination**.

## <a name="create-a-data-factory"></a>Crear una factoría de datos

1. En el panel izquierdo, seleccione **Crear un recurso**. Seleccione **Integración** > **Data Factory**:

   ![Selección de Data Factory](./media/doc-common-process/new-azure-data-factory-menu.png)

2. En la página **Nueva factoría de datos**, en **Nombre**, escriba **ADFTutorialDataFactory**.

   El nombre de la factoría de datos debe ser globalmente único. Es posible que reciba este mensaje de error:

   ![Mensaje de error de nombre no disponible](./media/doc-common-process/name-not-available-error.png)

   Si recibe un mensaje de error sobre el valor de nombre, escriba un nombre diferente para la factoría de datos. Por ejemplo, utilice _**suNombre**_ **ADFTutorialDataFactory**. Para conocer las reglas de nomenclatura de los artefactos de Data Factory, consulte [Data Factory: reglas de nomenclatura](naming-rules.md).
3. En **Suscripción**, seleccione la suscripción de Azure en la que creará la factoría de datos nueva.
4. En **Grupo de recursos**, complete uno de estos pasos:

    * Seleccione **Usar existente** y, luego, seleccione un grupo de recursos existente de la lista.

    * Seleccione **Crear nuevo** y escriba un nombre para el grupo de recursos.
         
    Para más información sobre los grupos de recursos, consulte [Uso de grupos de recursos para administrar los recursos de Azure](../azure-resource-manager/management/overview.md).

5. En **Versión**, seleccione **V2**.
6. En **Ubicación**, seleccione la ubicación de la factoría de datos. Solo las ubicaciones admitidas aparecen en la lista. Los almacenes de datos (por ejemplo, Azure Storage y Azure SQL Database) y los procesos (por ejemplo, Azure HDInsight) que Data Factory usa pueden estar en otras ubicaciones o regiones.
8. Seleccione **Crear**.
9. Una vez creada la factoría de datos, aparece la página principal de Data Factory.
10. Para abrir la interfaz de usuario de Azure Data Factory en otra pestaña, seleccione el icono **Author & Monitor** (Creación y supervisión):

    ![Página principal Factoría de datos](./media/doc-common-process/data-factory-home-page.png)

## <a name="use-the-copy-data-tool-to-create-a-pipeline"></a>Uso de la herramienta Copy Data para crear una canalización

1. En la página **Let's get started** (Comencemos), seleccione el icono **Copy Data** (Copiar datos) para iniciar la herramienta Copiar datos:

   ![Icono Copy Data (Copiar datos)](./media/doc-common-process/get-started-page.png)

2. En la página **Propiedades**, realice los pasos siguientes:

    a. En **Nombre de la tarea**, escriba **DeltaCopyFromBlobPipeline**.

    b. En **Task cadence o Task schedule** (Cadencia de tareas o Programación de tareas), seleccione **Run regularly on schedule** (Ejecutar periódicamente según programación).

    c. En **Tipo de desencadenador**, seleccione **Tumbling window** (Ventana de saltos de tamaño constante).

    d. En **Recurrence** (Periodicidad), establezca **15 minutos**.

    e. Seleccione **Next** (Siguiente).

    La interfaz de usuario de Data Factory crea una canalización con el nombre de la tarea especificado.

    ![Página de propiedades de Copy data (Copiar datos)](./media/tutorial-incremental-copy-lastmodified-copy-data-tool/copy-data-tool-properties-page.png)

3. En la página **Almacén de datos de origen**, complete estos pasos:

    a. Seleccione **Create new connection** (Crear nueva conexión) para agregar una conexión.

    b. Seleccione **Azure Blob Storage** en la galería y, a continuación, seleccione **Continuar**:

    ![Seleccionar Azure Blob Storage](./media/tutorial-incremental-copy-lastmodified-copy-data-tool/source-data-store-page-select-blob.png)

    c. En la página **New Linked Service (Azure Blob Storage)** (Nuevo servicio vinculado [Azure Blob Storage]), seleccione la cuenta de almacenamiento de la lista **Nombre de la cuenta de almacenamiento**. Pruebe la conexión y seleccione **Crear**.

    d. Seleccione el servicio vinculado nuevo y, luego, seleccione **Siguiente**:

   ![Seleccionar el servicio vinculado nuevo](./media/tutorial-incremental-copy-lastmodified-copy-data-tool/source-data-store-page-select-linkedservice.png)

4. En la página **Choose the input file or folder** (Elegir el archivo o la carpeta de entrada), complete los siguientes pasos:

    a. Busque y seleccione la carpeta **source** y, después, seleccione **Elegir**.

    ![Seleccione el archivo o la carpeta de entrada.](./media/tutorial-incremental-copy-lastmodified-copy-data-tool/choose-input-file-folder.png)

    b. En **File loading behavior** (Comportamiento de carga de archivos), seleccione **Incremental load: LastModifiedDate** (Carga incremental: LastModifiedDate).

    c. Seleccione **Binary copy** (Copia binaria) y, luego, seleccione **Siguiente**:

     ![Página de elección del archivo o la carpeta de entrada](./media/tutorial-incremental-copy-lastmodified-copy-data-tool/check-binary-copy.png)

5. En la página **Almacén de datos de destino**, seleccione el servicio **AzureBlobStorage** que creó. Se trata de la misma cuenta de almacenamiento que el almacén de datos de origen. Luego, seleccione **Siguiente**.

6. En la página **Choose the output file or folder** (Elegir el archivo o la carpeta de salida), complete los siguientes pasos:

    a. Busque y seleccione la carpeta **destination** y, después, seleccione **Elegir**:

    ![Página de elección del archivo o la carpeta de salida](./media/tutorial-incremental-copy-lastmodified-copy-data-tool/choose-output-file-folder.png)

    b. Seleccione **Next** (Siguiente).

7. En la página **Settings** (Configuración), seleccione **Next** (Siguiente).

8. En la página **Summary** (Resumen), revise la configuración y seleccione **Next** (Siguiente).

    ![Página de resumen](./media/tutorial-incremental-copy-lastmodified-copy-data-tool/summary-page.png)

9. En la página **Deployment** (Implementación), seleccione **Monitor** (Supervisión) para supervisar la canalización (tarea).

    ![Página Deployment (Implementación)](./media/tutorial-incremental-copy-lastmodified-copy-data-tool/deployment-page.png)

10. Observe que la pestaña **Monitor** (Supervisión) de la izquierda se selecciona automáticamente. La aplicación cambia a la pestaña **Monitor** (Supervisión). Verá el estado de la canalización. Seleccione **Refresh** (Actualizar) para actualizar la lista. Seleccione el vínculo bajo **PIPELINE NAME** (NOMBRE DE CANALIZACIÓN) para ver los detalles de la ejecución de actividad o para volver a ejecutar la canalización.

    ![Actualizar la lista y ver los detalles de la ejecución de actividad](./media/tutorial-incremental-copy-lastmodified-copy-data-tool/monitor-pipeline-runs-1.png)

11. Como solo hay una actividad (actividad de copia) en la canalización, solo verá una entrada. Para detalles sobre la operación de copia, seleccione el vínculo **Detalles** (el icono de gafas) en la columna **ACTIVITY NAME** (NOMBRE DE ACTIVIDAD). Para detalles sobre las propiedades, consulte [Introducción a la actividad de copia](copy-activity-overview.md).

    ![Actividad de copia en la canalización](./media/tutorial-incremental-copy-lastmodified-copy-data-tool/monitor-pipeline-runs2.png)

    Como no hay archivos en el contenedor de origen de la cuenta de Blob Storage, no verá ningún archivo copiado en el contenedor de destino en la cuenta:

    ![No hay archivos en los contenedores source o destination](./media/tutorial-incremental-copy-lastmodified-copy-data-tool/monitor-pipeline-runs3.png)

12. Cree un archivo de texto vacío y asígnele el nombre **file1.txt**. Cargue este archivo de texto en el contenedor de origen en su cuenta de almacenamiento. Puede usar varias herramientas para realizar estas tareas, como el [Explorador de Azure Storage](https://storageexplorer.com/).

    ![Crear file1.txt y cargar en el contenedor de origen](./media/tutorial-incremental-copy-lastmodified-copy-data-tool/monitor-pipeline-runs3-1.png)

13. Para volver a la vista **Ejecuciones de canalización**, seleccione **Todas las ejecuciones de la canalización** y espere a que la misma canalización se vuelva a desencadenar automáticamente.  

14. Cuando se complete la segunda ejecución de canalización, siga los mismos pasos mencionados anteriormente para revisar los detalles de la ejecución de actividad.  

    Verá que un archivo (file1.txt) se copió del contenedor de origen al contenedor de destino de la cuenta de Blob Storage:

    ![file1.txt se copió del contenedor de origen al contenedor de destino](./media/tutorial-incremental-copy-lastmodified-copy-data-tool/monitor-pipeline-runs6.png)

15. Cree otro archivo de texto vacío y asígnele el nombre **file2.txt**. Cargue este archivo de texto en el contenedor de origen en su cuenta de Blob Storage.

16. Repita los pasos 13 y 14 en el segundo archivo. Verá que solamente un archivo (file2.txt) se copió del contenedor de origen al contenedor de destino de la cuenta de almacenamiento durante la ejecución de canalización.  

    También puede comprobar que solo se copió un archivo mediante el uso del [Explorador de Azure Storage](https://storageexplorer.com/) para examinar los archivos:

    ![Examen de archivos mediante el Explorador de Azure Storage](./media/tutorial-incremental-copy-lastmodified-copy-data-tool/monitor-pipeline-runs8.png)


## <a name="next-steps"></a>Pasos siguientes
Vaya al tutorial siguiente para aprender a transformar los datos mediante el uso de un clúster de Apache Spark en Azure:

> [!div class="nextstepaction"]
>[Transformación de datos en la nube mediante una actividad de Spark en Azure Data Factory](tutorial-transform-data-spark-portal.md)

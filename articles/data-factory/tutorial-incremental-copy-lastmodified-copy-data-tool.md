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
ms.date: 3/18/2020
ms.openlocfilehash: 743f9dd8f7998178a75d716f4da22efee2b3bc79
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "80131031"
---
# <a name="incrementally-copy-new-and-changed-files-based-on-lastmodifieddate-by-using-the-copy-data-tool"></a>Copia incremental de archivos nuevos y modificados según LastModifiedDate con la herramienta Copiar datos

En este tutorial, usará Azure Portal para crear una instancia de Data Factory. Luego, usará la herramienta Copiar datos para crear una canalización que copie incrementalmente solo los archivos nuevos y modificados, según el valor de **LastModifiedDate**, desde un Azure Blob Storage a otro.

Al hacerlo, ADF examinará todos los archivos del almacén de origen, aplicará el filtro de archivos con LastModifiedDate y copiará solo los archivos nuevos y actualizados desde la última vez en el almacén de destino.  Tenga en cuenta que, si deja que ADF examine enormes cantidades de archivos, aunque solo copie algunos archivos en destino, aún así tendrá que prever que tarde bastante, ya que el examen de archivos puede exigir mucho tiempo.   

> [!NOTE]
> Si no está familiarizado con Azure Data Factory, consulte [Introducción a Azure Data Factory](introduction.md).

En este tutorial, realizará las siguientes tareas:

> [!div class="checklist"]
> * Creación de una factoría de datos.
> * Uso de la herramienta Copy Data para crear una canalización.
> * Supervisión de las ejecuciones de canalización y actividad.

## <a name="prerequisites"></a>Prerrequisitos

* **Suscripción de Azure**: Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/) antes de empezar.
* **Cuenta de Azure Storage**: Use Blob Storage como almacén de datos de _origen_ y _receptor_. Si no dispone de una cuenta de almacenamiento de Azure, consulte las instrucciones de [Creación de una cuenta de almacenamiento](../storage/common/storage-account-create.md).

### <a name="create-two-containers-in-blob-storage"></a>Crear dos contenedores en Azure Blob Storage

Haga lo siguiente para preparar su instancia de Blob Storage para el tutorial.

1. Cree un contenedor denominado **source**. Puede usar varias herramientas para realizar esta tarea, como el [Explorador de Azure Storage](https://storageexplorer.com/).

2. Cree un contenedor denominado **destination**.

## <a name="create-a-data-factory"></a>Crear una factoría de datos

1. En el menú de la izquierda, seleccione **Crear un recurso** > **Datos y análisis** > **Data Factory**:

   ![Selección de la factoría de datos en el panel Nuevo](./media/doc-common-process/new-azure-data-factory-menu.png)

2. En la página **Nueva factoría de datos**, en **Nombre**, escriba **ADFTutorialDataFactory**.

   El nombre de la factoría de datos debe ser _globalmente único_. Puede aparecer el siguiente mensaje de error:

   ![Mensaje de error de nueva factoría de datos](./media/doc-common-process/name-not-available-error.png)

   Si recibe un mensaje de error sobre el valor de nombre, escriba un nombre diferente para la factoría de datos. Por ejemplo, utilice _**suNombre**_ **ADFTutorialDataFactory**. Para conocer las reglas de nomenclatura de los artefactos de Data Factory, consulte [Data Factory: reglas de nomenclatura](naming-rules.md).
3. Seleccione la **suscripción** de Azure en la que va a crear la nueva instancia de Data Factory.
4. Para **Grupo de recursos**, realice uno de los siguientes pasos:

    * Seleccione en primer lugar **Usar existente** y después un grupo de recursos de la lista desplegable.

    * Seleccione **Crear nuevo** y escriba el nombre de un grupo de recursos. 
         
    Para más información sobre los grupos de recursos, consulte [Uso de grupos de recursos para administrar los recursos de Azure](../azure-resource-manager/management/overview.md).

5. En **Versión**, seleccione **V2**.
6. En **Ubicación**, seleccione la ubicación de la factoría de datos. Solo las ubicaciones admitidas se muestran en la lista desplegable. Los almacenes de datos (por ejemplo, Azure Storage y SQL Database) y los procesos (por ejemplo, Azure HDInsight) que Data Factory usa pueden estar en otras ubicaciones o regiones.
8. Seleccione **Crear**.
9. Una vez finalizada la creación, se muestra la página principal de **Data Factory**.
10. Para abrir la interfaz de usuario de Azure Data Factory en otra pestaña, seleccione el icono **Author & Monitor** (Creación y supervisión).

    ![Página principal Factoría de datos](./media/doc-common-process/data-factory-home-page.png)

## <a name="use-the-copy-data-tool-to-create-a-pipeline"></a>Uso de la herramienta Copy Data para crear una canalización

1. En la página **Let's get started** (Comencemos), seleccione el icono **Copy Data** (Copiar datos) para iniciar la herramienta Copiar datos.

   ![Icono de la herramienta Copy Data](./media/doc-common-process/get-started-page.png)

2. En la página **Propiedades**, realice los pasos siguientes:

    a. En **Nombre de la tarea**, escriba **DeltaCopyFromBlobPipeline**.

    b. En **Task cadence** (Cadencia de tareas) o **Task schedule** (Programación de tareas) seleccione **Run regularly on schedule** (Ejecutar periódicamente según programación).

    c. En **Trigger Type** (Tipo de desencadenador), seleccione **Tumbling Window** (Ventana de saltos de tamaño constante).

    d. En **Recurrence** (Periodicidad), establezca **15 minutos**.

    e. Seleccione **Next** (Siguiente).

    La interfaz de usuario de Data Factory crea una canalización con el nombre de la tarea especificado.

    ![Página de propiedades](./media/tutorial-incremental-copy-lastmodified-copy-data-tool/copy-data-tool-properties-page.png)

3. En la página **Almacén de datos de origen**, realice los pasos siguientes:

    a. Seleccione **+ Create new connection** (+ Crear nueva conexión) para agregar una conexión.

    b. Seleccione **Azure Blob Storage** en la galería y, a continuación, seleccione **Continue** (Continuar).

    ![Página Source data store (Almacén de datos de origen)](./media/tutorial-incremental-copy-lastmodified-copy-data-tool/source-data-store-page-select-blob.png)

    c. En la página **New Linked Service (Azure Blob Storage)** (Nuevo servicio vinculado [Azure Blob Storage]), seleccione la cuenta de almacenamiento de la lista **Nombre de la cuenta de almacenamiento**. Pruebe la conexión y, después, seleccione **Create** (Crear).

    d. Seleccione el servicio vinculado recién creado y, luego, seleccione **Continue** (Continuar).

   ![Página Source data store (Almacén de datos de origen)](./media/tutorial-incremental-copy-lastmodified-copy-data-tool/source-data-store-page-select-linkedservice.png)

4. En la página **Choose the input file or folder** (Elegir el archivo o la carpeta de entrada), complete los siguientes pasos:

    a. Busque y seleccione la carpeta **source** y, después, seleccione **Choose** (Elegir).

    ![Seleccione el archivo o la carpeta de entrada.](./media/tutorial-incremental-copy-lastmodified-copy-data-tool/choose-input-file-folder.png)

    b. En **File loading behavior** (Comportamiento de carga de archivos), seleccione **Incremental load: LastModifiedDate** (Carga incremental: LastModifiedDate).

    c. Seleccione **Binary copy** (Copia binaria) y seleccione **Next** (Siguiente).

     ![Seleccione el archivo o la carpeta de entrada.](./media/tutorial-incremental-copy-lastmodified-copy-data-tool/check-binary-copy.png)

5. En la página **Almacén de datos de destino**, seleccione el elemento **AzureBlobStorage** que creó. Se trata de la misma cuenta de almacenamiento que el almacén de datos de origen. Luego, seleccione **Siguiente**.

6. En la página **Choose the output file or folder** (Elegir el archivo o la carpeta de salida), complete los siguientes pasos:

    a. Busque y seleccione la carpeta **destination** y, después, seleccione **Choose** (Elegir).

    ![Elección del archivo o la carpeta de salida](./media/tutorial-incremental-copy-lastmodified-copy-data-tool/choose-output-file-folder.png)

    b. Seleccione **Next** (Siguiente).

7. En la página **Settings** (Configuración), seleccione **Next** (Siguiente).

8. En la página **Summary** (Resumen), revise la configuración y seleccione **Next** (Siguiente).

    ![Página de resumen](./media/tutorial-incremental-copy-lastmodified-copy-data-tool/summary-page.png)

9. En la página **Deployment** (Implementación), seleccione **Monitor** (Supervisión) para supervisar la canalización (tarea).

    ![Página Deployment (Implementación)](./media/tutorial-incremental-copy-lastmodified-copy-data-tool/deployment-page.png)

10. Observe que la pestaña **Monitor** (Supervisión) de la izquierda se selecciona automáticamente. La aplicación cambia a la pestaña **Monitor** (Supervisión). Verá el estado de la canalización. Seleccione **Refresh** (Actualizar) para actualizar la lista. Haga clic en el vínculo en **PIPELINE NAME** (NOMBRE DE CANALIZACIÓN) para ver los detalles de la ejecución de actividad o volver a ejecutar la canalización. 

    ![Actualizar la lista y seleccionar View Activity Runs (Ver ejecuciones de actividad)](./media/tutorial-incremental-copy-lastmodified-copy-data-tool/monitor-pipeline-runs1.png)

11. Como solo hay una actividad (actividad de copia) en la canalización, solo verá una entrada. Para obtener más información sobre la operación de copia, seleccione el vínculo **Detalles** (icono de gafas) en la columna **ACTIVITY NAME**. Para más información sobre las propiedades, consulte [Introducción a la actividad de copia](copy-activity-overview.md). 

    ![La actividad de copia está en la canalización](./media/tutorial-incremental-copy-lastmodified-copy-data-tool/monitor-pipeline-runs2.png)

    Como no hay ningún archivo en el contenedor **source** en la cuenta de Blob Storage, no verá ningún archivo copiado en el contenedor **destination** en la cuenta de Blob Storage.

    ![No hay archivos en los contenedores source o destination](./media/tutorial-incremental-copy-lastmodified-copy-data-tool/monitor-pipeline-runs3.png)

12. Cree un archivo de texto vacío y asígnele el nombre **file1.txt**. Cargue este archivo de texto en el contenedor **source** en su cuenta de almacenamiento. Puede usar varias herramientas para realizar estas tareas, como el [Explorador de Azure Storage](https://storageexplorer.com/).

    ![Crear file1.txt y cargar en el contenedor source](./media/tutorial-incremental-copy-lastmodified-copy-data-tool/monitor-pipeline-runs3-1.png)

13. Para volver a la vista **Ejecuciones de canalización**, seleccione **Todas las ejecuciones de la canalización** y espere a que la misma canalización se vuelva a desencadenar automáticamente.  

    ![Selección de All Pipeline Runs (Todas las ejecuciones de canalizaciones)](./media/tutorial-incremental-copy-lastmodified-copy-data-tool/monitor-pipeline-runs4.png)

14. Cuando se complete la segunda ejecución de canalización, siga los mismos pasos mencionados anteriormente para revisar los detalles de la ejecución de actividad.  

    Verá que un archivo (file1.txt) se ha copiado del contenedor **source** al contenedor **destination** de la cuenta de Blob Storage.

    ![File1.txt se ha copiado del contenedor source al contenedor destination](./media/tutorial-incremental-copy-lastmodified-copy-data-tool/monitor-pipeline-runs6.png)

15. Cree otro archivo de texto vacío y asígnele el nombre **file2.txt**. Cargue este archivo de texto en el contenedor **source** en su cuenta de Blob Storage.

16. Repita los pasos 13 y 14 en este segundo archivo. Verá que solamente un archivo (file2.txt) se ha copiado del contenedor **source** al contenedor **destination** de la cuenta de almacenamiento en la siguiente ejecución de canalización.  

    Esto se puede confirmar también usando el [Explorador de Azure Storage](https://storageexplorer.com/) para examinar los archivos.

    ![Examen de archivos mediante el Explorador de Azure Storage](./media/tutorial-incremental-copy-lastmodified-copy-data-tool/monitor-pipeline-runs8.png)


## <a name="next-steps"></a>Pasos siguientes
Pase al tutorial siguiente para obtener información sobre la transformación de datos usando un clúster de Apache Spark en Azure:

> [!div class="nextstepaction"]
>[Transformación de datos en la nube mediante una actividad de Spark en Azure Data Factory](tutorial-transform-data-spark-portal.md)

---
title: Copia incremental de nuevos archivos por el nombre de archivo con particiones de tiempo
description: Cree una instancia de Azure Data Factory y, luego, use la herramienta Copiar datos para cargar incrementalmente archivos nuevos solo por el nombre de archivo con particiones de tiempo.
services: data-factory
documentationcenter: ''
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
ms.openlocfilehash: ae66bb025f2a49a79120fe86e0de7c4a3ccf26ca
ms.sourcegitcommit: dc342bef86e822358efe2d363958f6075bcfc22a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/12/2020
ms.locfileid: "94555386"
---
# <a name="incrementally-copy-new-files-based-on-time-partitioned-file-name-by-using-the-copy-data-tool"></a>Copia incremental de nuevos archivos por el nombre de archivo con particiones de tiempo mediante la herramienta Copiar datos

[!INCLUDE[appliesto-adf-xxx-md](includes/appliesto-adf-xxx-md.md)]

En este tutorial, usará Azure Portal para crear una factoría de datos. Luego, use la herramienta Copiar datos para crear una canalización que copie incrementalmente los archivos nuevos por el nombre de archivo con particiones de tiempo de una instancia de Azure Blob Storage a otra.

> [!NOTE]
> Si no está familiarizado con Azure Data Factory, consulte [Introducción a Azure Data Factory](introduction.md).

En este tutorial, realizará los siguientes pasos:

> [!div class="checklist"]
> * Creación de una factoría de datos.
> * Uso de la herramienta Copy Data para crear una canalización.
> * Supervisión de las ejecuciones de canalización y actividad.

## <a name="prerequisites"></a>Prerrequisitos

* **Suscripción de Azure**: Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/) antes de empezar.
* **Cuenta de Azure Storage**: Use Blob Storage como almacén de datos de _origen_ y _receptor_. Si no dispone de una cuenta de almacenamiento de Azure, consulte las instrucciones de [Creación de una cuenta de almacenamiento](../storage/common/storage-account-create.md).

### <a name="create-two-containers-in-blob-storage"></a>Crear dos contenedores en Azure Blob Storage

Haga lo siguiente para preparar su instancia de Blob Storage para el tutorial.

1. Cree un contenedor denominado **source**.  Cree una ruta de acceso de carpeta como **2020/03/17/03** en el contenedor. Cree un archivo de texto vacío y asígnele el nombre **file1.txt**. Cargue el archivo file1.txt en la ruta de acceso de carpeta **source/2020/03/17/03** en su cuenta de almacenamiento.  Puede usar varias herramientas para realizar estas tareas, como el [Explorador de Azure Storage](https://storageexplorer.com/).

    ![cargar archivos](./media/tutorial-incremental-copy-partitioned-file-name-copy-data-tool/upload-file.png)

    > [!NOTE]
    > Ajuste el nombre de carpeta con la hora UTC correspondiente.  Por ejemplo, si la hora UTC actual es 3:38 a. m. del 17 de marzo de 2020, puede crear la ruta de acceso de carpeta como **source/2020/03/17/03/** por la regla de **source/{Year}/{Month}/{Day}/{Hour}/** .

2. Cree un contenedor denominado **destination**. Puede usar varias herramientas para realizar estas tareas, como el [Explorador de Azure Storage](https://storageexplorer.com/).

## <a name="create-a-data-factory"></a>Crear una factoría de datos

1. En el menú de la izquierda, seleccione **Crear un recurso** > **Integración** > **Data Factory**:

   ![Selección de la factoría de datos en el panel Nuevo](./media/doc-common-process/new-azure-data-factory-menu.png)

2. En la página **Nueva factoría de datos**, en **Nombre**, escriba **ADFTutorialDataFactory**.

    El nombre de la factoría de datos debe ser _globalmente único_. Puede aparecer el siguiente mensaje de error:

   ![Mensaje de error de nueva factoría de datos](./media/doc-common-process/name-not-available-error.png)

   Si recibe un mensaje de error sobre el valor de nombre, escriba un nombre diferente para la factoría de datos. Por ejemplo, utilice _**suNombre**_ **ADFTutorialDataFactory**. Para conocer las reglas de nomenclatura de los artefactos de Data Factory, consulte [Data Factory: reglas de nomenclatura](naming-rules.md).
3. Seleccione la **suscripción** de Azure en la que quiere crear la nueva factoría de datos.
4. Para **Grupo de recursos**, realice uno de los siguientes pasos:

    a. Seleccione en primer lugar **Usar existente** y después un grupo de recursos de la lista desplegable.

    b. Seleccione **Crear nuevo** y escriba el nombre de un grupo de recursos. 
         
    Para más información sobre los grupos de recursos, consulte [Uso de grupos de recursos para administrar los recursos de Azure](../azure-resource-manager/management/overview.md).

5. En **Versión**, seleccione **V2** como versión.
6. En **Ubicación**, seleccione la ubicación de la factoría de datos. Solo las ubicaciones admitidas se muestran en la lista desplegable. Los almacenes de datos (por ejemplo, Azure Storage y SQL Database) y los procesos (por ejemplo, Azure HDInsight) que usa la factoría de datos pueden estar en otras ubicaciones o regiones.
7. Seleccione **Crear**.
8. Una vez finalizada la creación, se muestra la página principal de **Data Factory**.
9. Para abrir la interfaz de usuario de Azure Data Factory en otra pestaña, seleccione el icono **Author & Monitor** (Creación y supervisión).

    ![Página principal Factoría de datos](./media/doc-common-process/data-factory-home-page.png)


## <a name="use-the-copy-data-tool-to-create-a-pipeline"></a>Uso de la herramienta Copy Data para crear una canalización

1. En la página **Comencemos**, seleccione el título **Copiar datos** para iniciar la herramienta Copiar datos.

   ![Icono de la herramienta Copy Data](./media/doc-common-process/get-started-page.png)

2. En la página **Propiedades**, realice los pasos siguientes:

    a. En **Nombre de la tarea**, escriba **DeltaCopyFromBlobPipeline**.

    b. En **Task cadence o Task schedule** (Cadencia de tareas o Programación de tareas), seleccione **Run regularly on schedule** (Ejecutar periódicamente según programación).

    c. En **Tipo de desencadenador**, seleccione **Tumbling Window** (Ventana de saltos de tamaño constante).

    d. En **Periodicidad**, establezca **1 hora**.

    e. Seleccione **Next** (Siguiente).

    La interfaz de usuario de Data Factory crea una canalización con el nombre de la tarea especificado.

    ![Página de propiedades](./media/tutorial-incremental-copy-partitioned-file-name-copy-data-tool/copy-data-tool-properties-page.png)
3. En la página **Almacén de datos de origen**, realice los pasos siguientes:

    a. Haga clic en **+ Crear nueva conexión** para agregar una conexión.
    
    b. Seleccione Azure Blob Storage en la galería y, a continuación, seleccione Continuar.
    
    c. En la página **New Linked Service (Azure Blob Storage)** [Nuevo servicio vinculado (Azure Blob Storage)], especifique un nombre para el servicio vinculado. Seleccione la suscripción a Azure y, a continuación, su cuenta de almacenamiento de la lista **Nombre de la cuenta de almacenamiento**. Pruebe la conexión y, después, seleccione **Create** (Crear).

    ![Página Source data store (Almacén de datos de origen)](./media/tutorial-incremental-copy-partitioned-file-name-copy-data-tool/source-data-store-page-linkedservice.png)

    d. Seleccione el servicio vinculado recién creado en la página **Almacén de datos de origen** y, a continuación, haga clic en **Siguiente**.

4. En la página **Choose the input file or folder** (Elegir el archivo o la carpeta de entrada), lleve a cabo los siguientes pasos:

    a. Busque y seleccione el contenedor **source** y, después, seleccione **Elegir**.

    ![Captura de pantalla que muestra el cuadro de diálogo Choose the input file or folder (Elegir archivo o carpeta de entrada).](./media/tutorial-incremental-copy-partitioned-file-name-copy-data-tool/choose-input-file-folder.png)

    b. En **File loading behavior** (Comportamiento de carga de archivos), seleccione **Incremental load: time-partitioned folder/file names** (Carga Incremental: nombres de archivo/carpeta con particiones de tiempo).

    c. Escriba la ruta de acceso de carpeta dinámica como **source/{year}/{month}/{day}/{hour}/** y cambie el formato como se muestra en la captura de pantalla siguiente. Seleccione **Binary copy** (Copia binaria) y haga clic en **Siguiente**.

    ![Captura de pantalla que muestra el cuadro de diálogo Choose the input file or folder (Elegir archivo o carpeta de entrada) con una carpeta seleccionada.](./media/tutorial-incremental-copy-partitioned-file-name-copy-data-tool/check-binary-copy.png)     

5. En la página **Destination data store** (Almacén de datos de destino), seleccione el elemento **AzureBlobStorage**, que es la misma cuenta de almacenamiento que el almacén de origen de datos y, a continuación, haga clic en **Siguiente**.

    ![Página Destination data store (Almacén de datos de destino)](./media/tutorial-incremental-copy-partitioned-file-name-copy-data-tool/destination-data-store-page-select-linkedservice.png)
6. En la página **Choose the output file or folder** (Elegir el archivo o la carpeta de salida), realice los pasos siguientes:

    a. Busque y seleccione la carpeta **destination** y, después, seleccione **Elegir**.

    b. Escriba la ruta de acceso de carpeta dinámica como **destination/{year}/{month}/{day}/{hour}/** y cambie el formato de la siguiente manera:

    ![Captura de pantalla que muestra el cuadro de diálogo Choose the output file or folder (Elegir archivo o carpeta de salida).](./media/tutorial-incremental-copy-partitioned-file-name-copy-data-tool/output-file-name.png)

    c. Haga clic en **Next**.

    ![Captura de pantalla que muestra el cuadro de diálogo Choose the output file or folder (Elegir archivo o carpeta de salida) con Next (Siguiente) seleccionada.](./media/tutorial-incremental-copy-partitioned-file-name-copy-data-tool/click-next-after-output-folder.png)
7. En la página **Settings** (Configuración), seleccione **Next** (Siguiente).

8. En la página **Summary** (Resumen), revise la configuración y seleccione **Next** (Siguiente).

    ![Página de resumen](./media/tutorial-incremental-copy-partitioned-file-name-copy-data-tool/summary-page.png)

9. En la página **Deployment** (Implementación), seleccione **Monitor** (Supervisión) para supervisar la canalización (tarea).
    ![Página de implementación](./media/tutorial-incremental-copy-partitioned-file-name-copy-data-tool/deployment-page.png)

10. Observe que la pestaña **Monitor** (Supervisión) de la izquierda se selecciona automáticamente.  Debe esperar a que la canalización se ejecute cuando se desencadene automáticamente (después de una hora aproximadamente). Cuando se ejecute, haga clic en el vínculo del nombre de canalización **DeltaCopyFromBlobPipeline** para ver los detalles de la ejecución de la actividad o vuelva a ejecutar la canalización. Seleccione **Refresh** (Actualizar) para actualizar la lista.

    ![Captura de pantalla que muestra la pestaña Pipeline runs (Ejecuciones de canalizaciones).](./media/tutorial-incremental-copy-partitioned-file-name-copy-data-tool/monitor-pipeline-runs-1.png)
11. Como solo hay una actividad (actividad de copia) en la canalización, solo verá una entrada. Ajuste el ancho de columna de la columnas de **origen** y **destino** (si es necesario) para mostrar más detalles. Puede ver que el archivo de origen (file1.txt) se ha copiado de *source/2020/03/17/03/* a *destination/2020/03/17/03/* con el mismo nombre de archivo. 

    ![Captura de pantalla que muestra los detalles de ejecución de las canalizaciones.](./media/tutorial-incremental-copy-partitioned-file-name-copy-data-tool/monitor-pipeline-runs2.png)

    Esto también se puede confirmar usando el Explorador de Azure Storage(https://storageexplorer.com/) ) para examinar los archivos.

    ![Captura de pantalla que muestra los detalles de ejecución de la canalización respecto al destino.](./media/tutorial-incremental-copy-partitioned-file-name-copy-data-tool/monitor-pipeline-runs3.png)

12. Cree otro archivo de texto vacío con el nuevo nombre **file2.txt**. Cargue el archivo file2.txt en la ruta de acceso de carpeta **source/2020/03/17/04** de su cuenta de almacenamiento. Puede usar varias herramientas para realizar estas tareas, como el [Explorador de Azure Storage](https://storageexplorer.com/).

    > [!NOTE]
    > Es posible que ya sepa que debe crear una nueva ruta de acceso de carpeta. Ajuste el nombre de carpeta con la hora UTC correspondiente.  Por ejemplo, si la hora UTC actual es 4:20 a. m. del 17 de marzo de 2020, puede crear la ruta de acceso de carpeta como **source/2020/03/17/04/** por la regla de **{Year}/{Month}/{Day}/{Hour}/** .

13. Para volver a la vista **Ejecuciones de canalización**, seleccione **Todas las ejecuciones de la canalización** y espere a que la misma canalización se desencadene de nuevo automáticamente al cabo de una hora.  

    ![Captura de pantalla que muestra el vínculo All pipeline runs (Todas las ejecuciones de la canalización) para volver a esa página.](./media/tutorial-incremental-copy-partitioned-file-name-copy-data-tool/monitor-pipeline-runs5.png)

14. Seleccione el nuevo vínculo **DeltaCopyFromBlobPipeline** para la segunda ejecución de canalización cuando se realice y repita el proceso para revisar los detalles. Puede ver que el archivo de origen (file2.txt) se ha copiado de **source/2020/03/17/04/** a **destination/2020/03/17/04/** con el mismo nombre de archivo. Esto también se puede confirmar usando el Explorador de Azure Storage (https://storageexplorer.com/) ) para examinar los archivos en el contenedor **destination**.


## <a name="next-steps"></a>Pasos siguientes
Pase al tutorial siguiente para obtener información acerca de la transformación de datos mediante el uso de un clúster de Spark en Azure:

> [!div class="nextstepaction"]
>[Transformar datos con un clúster de Spark en la nube](tutorial-transform-data-spark-portal.md)

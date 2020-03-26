---
title: Uso de Azure Portal para crear una canalización de factoría de datos
description: En este tutorial se proporcionan instrucciones paso a paso para usar Azure Portal para crear una factoría de datos con una canalización. La canalización usa la actividad de copia para copiar datos de Azure Blob Storage a SQL Database.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: shwang
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.topic: tutorial
ms.custom: seo-lt-2019
ms.date: 06/21/2018
ms.author: jingwang
ms.openlocfilehash: 135a18f275137e72b5ff4d79f6a32bd39bd9c00c
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/24/2020
ms.locfileid: "75977403"
---
# <a name="copy-data-from-azure-blob-storage-to-a-sql-database-by-using-azure-data-factory"></a>Copia de datos de Azure Blob Storage a SQL Database mediante Azure Data Factory
En este tutorial, creará una factoría de datos mediante la interfaz de usuario (UI) de Azure Data Factory. La canalización de esta factoría de datos copia los datos de Azure Blob Storage a Azure SQL Database. El patrón de configuración de este tutorial se aplica a la copia de un almacén de datos basado en archivos a un almacén de datos relacional. Para obtener una lista de los almacenes de datos que se admiten como orígenes y receptores, consulte la tabla de [almacenes de datos admitidos](copy-activity-overview.md#supported-data-stores-and-formats).

> [!NOTE]
> - Si no está familiarizado con Data Factory, consulte [Introducción a Azure Data Factory](introduction.md).

En este tutorial, realizará los siguientes pasos:

> [!div class="checklist"]
> * Creación de una factoría de datos.
> * Creación de una canalización con una actividad de copia.
> * Realización de la serie de pruebas de la canalización.
> * Desencadenamiento manual de la canalización.
> * Desencadenamiento de la canalización de forma programada.
> * Supervisión de las ejecuciones de canalización y actividad.

## <a name="prerequisites"></a>Prerrequisitos
* **Suscripción de Azure**. Si no tiene una suscripción a Azure, cree una [cuenta gratuita de Azure](https://azure.microsoft.com/free/) antes de empezar.
* **Cuenta de Azure Storage**. Usará Blob Storage como almacén de datos de *origen*. Si no tiene una cuenta de almacenamiento, consulte [Crear una cuenta de almacenamiento](../storage/common/storage-account-create.md) para crear una.
* **Azure SQL Database**. Usará la base de datos como un almacén de datos *receptor*. Si no tiene una base de datos SQL, consulte [Creación de una instancia de Azure SQL Database](../sql-database/sql-database-get-started-portal.md) para crear una.

### <a name="create-a-blob-and-a-sql-table"></a>Creación de un blob y una tabla SQL

Ahora, prepare su almacenamiento de blobs y su base de datos SQL para el tutorial mediante los pasos siguientes:

#### <a name="create-a-source-blob"></a>Creación de un blob de origen

1. Inicie el Bloc de notas. Copie el texto siguiente y guárdelo como un archivo **emp.txt** en el disco:

    ```
    John,Doe
    Jane,Doe
    ```

1. Cree un contenedor denominado **adftutorial** en su instancia de Blob Storage. Cree una carpeta denominada **input** en este contenedor. A continuación, cargue el archivo **emp.txt** en la carpeta **input**. Use Azure Portal o herramientas como [Explorador de Azure Storage](https://storageexplorer.com/) para realizar estas tareas.

#### <a name="create-a-sink-sql-table"></a>Creación de una tabla SQL receptora

1. Use el siguiente script de SQL para crear la tabla **dbo.emp** en la base de datos SQL:

    ```sql
    CREATE TABLE dbo.emp
    (
        ID int IDENTITY(1,1) NOT NULL,
        FirstName varchar(50),
        LastName varchar(50)
    )
    GO

    CREATE CLUSTERED INDEX IX_emp_ID ON dbo.emp (ID);
    ```

1. Permita que los servicios de Azure accedan a SQL Server. Asegúrese de que **Permitir el acceso a servicios de Azure** esté **Activado** para SQL Server de forma que Data Factory pueda escribir datos en su instancia de SQL Server. Para verificar y activar esta configuración, vaya a Azure SQL Server > Información general > Establecer el firewall del servidor > establezca la opción **Permitir el acceso a servicios de Azure** en **Activada**.

## <a name="create-a-data-factory"></a>Crear una factoría de datos
En este paso, creará una factoría de datos e iniciará la interfaz de usuario de Data Factory para crear una canalización en la factoría de datos.

1. Abra **Microsoft Edge** o **Google Chrome**. Actualmente, la interfaz de usuario de Data Factory solo se admite en los exploradores web Microsoft Edge y Google Chrome.
2. En el menú de la izquierda, seleccione **Crear un recurso** > **Analytics** > **Data Factory**:

   ![Selección de la factoría de datos en el panel Nuevo](./media/doc-common-process/new-azure-data-factory-menu.png)

3. En la página **Nueva factoría de datos**, en **Nombre**, escriba **ADFTutorialDataFactory**.

   El nombre de la instancia de Azure Data Factory debe ser *único de forma global*. Si recibe un mensaje de error sobre el valor de nombre, escriba un nombre diferente para la factoría de datos. (Por ejemplo, utilice SuNombreADFTutorialDataFactory). Para conocer las reglas de nomenclatura de los artefactos de Data Factory, consulte [Azure Data Factory: reglas de nomenclatura](naming-rules.md).

     ![Nueva factoría de datos](./media/doc-common-process/name-not-available-error.png)
4. Seleccione la **suscripción** de Azure en la que quiere crear la factoría de datos.
5. Para **Grupo de recursos**, realice uno de los siguientes pasos:

    a. Seleccione en primer lugar **Usar existente**y después un grupo de recursos de la lista desplegable.

    b. Seleccione **Crear nuevo**y escriba el nombre de un grupo de recursos. 
         
    Para más información sobre los grupos de recursos, consulte [Uso de grupos de recursos para administrar los recursos de Azure](../azure-resource-manager/management/overview.md). 
6. En **Versión**, seleccione **V2**.
7. En **Ubicación**, seleccione la ubicación de la factoría de datos. En la lista desplegable solo se muestran las ubicaciones que se admiten. Los almacenes de datos (por ejemplo, Azure Storage y SQL Database) y los procesos (por ejemplo, Azure HDInsight) que usa la factoría de datos pueden estar en otras regiones.
8. Seleccione **Crear**.
9. Una vez finalizada la creación, verá el aviso en el centro de notificaciones. Seleccione **Ir al recurso** para ir a la página de Data Factory.
10. Haga clic en **Author & Monitor** (Creación y supervisión) para iniciar la interfaz de usuario de Data Factory en una pestaña independiente.


## <a name="create-a-pipeline"></a>Crear una canalización
En este paso, creará una canalización con una actividad de copia en la factoría de datos. La actividad de copia realiza la copia de los datos de Blob Storage a SQL Database. En el [tutorial de inicio rápido](quickstart-create-data-factory-portal.md),creó una canalización mediante estos pasos:

1. Creación del servicio vinculado.
1. Creación del conjunto de datos de entrada y salida.
1. Creación de una canalización

En este tutorial, comenzará a crear la canalización. A continuación, creará servicios vinculados y conjuntos de datos cuando los necesite para configurar la canalización.

1. En la página **Let's get started** (Introducción) seleccione **Create pipeline** (Crear canalización).

   ![Creación de una canalización](./media/doc-common-process/get-started-page.png)
1. En la pestaña **General** de la canalización, escriba **CopyPipeline** como **nombre** de la canalización.

1. En el cuadro de herramientas **Activities** (Actividades), expanda la categoría **Move and Transform** (Mover y transformar) y arrastre y suelte la actividad **Copy Data** (Copiar datos) desde el cuadro de herramientas hasta la superficie de diseño de la canalización. Especifique **CopyFromBlobToSql** en **Name** (Nombre).

    ![Actividad de copia](./media/tutorial-copy-data-portal/drag-drop-copy-activity.png)

### <a name="configure-source"></a>Configuración del origen

1. Vaya a la pestaña **Source** (Origen). Haga clic en **+ New** (+ Nuevo) para crear un conjunto de datos de origen.

1. En el cuadro de diálogo **New Dataset** (Nuevo conjunto de datos), seleccione **Azure Blob Storage** y, después, seleccione **Continue** (Continuar). Los datos de origen están en Blob Storage, así que seleccionará **Azure Blob Storage** como conjunto de datos de origen.

1. En el cuadro de diálogo **Select Format** (Seleccionar formato), elija el tipo de formato de los datos y, después, seleccione **Continue** (Continuar).

    ![Tipo de formato de datos](./media/doc-common-process/select-data-format.png)

1. En el cuadro de diálogo **Set Properties** (Establecer propiedades), escriba **SourceBlobDataset** como nombre. Junto al cuadro de texto **Linked service** (Servicio vinculado), seleccione **+ New** (+Nuevo).

1. En la ventana **New Linked Service (Azure Blob Storage)** [Nuevo servicio vinculado (Azure Blob Storage)], escriba **AzureStorageLinkedService** como nombre y seleccione la cuenta de almacenamiento en la lista **Nombre de la cuenta de almacenamiento**. Pruebe la conexión y, a continuación, seleccione **Finish** (Finalizar) para implementar el servicio vinculado.

1. Una vez creado el servicio vinculado, se vuelve a ir a la página **Set Properties** (Establecer propiedades). Junto a **File path** (Ruta de acceso del archivo), seleccione **Browse** (Examinar).

1. Vaya a la carpeta **adftutorial/input**, seleccione el archivo **emp.txt** y, luego, **Finish** (Finalizar).

1. Va automáticamente a la página de canalización. En la pestaña **Source** (Origen), confirme que se selecciona **SourceBlobDataset**. Para obtener una vista previa de los datos de esta página, seleccione **Preview data** (Vista previa de los datos).

    ![Conjunto de datos de origen](./media/tutorial-copy-data-portal/source-dataset-selected.png)

### <a name="configure-sink"></a>Configuración del receptor

1. Vaya a la pestaña **Sink** (Receptor) y seleccione **+ New** (+Nuevo) para crear un conjunto de datos del receptor.

1. En el cuadro de diálogo **New Datase** (Nuevo conjunto de datos), escriba "SQL" en el cuadro de búsqueda para filtrar los conectores, seleccione **Azure SQL Database** y, después, seleccione **Continue** (Continuar). En este tutorial, copiará los datos en una base de datos SQL.

1. En el cuadro de diálogo **Set Properties** (Establecer propiedades), escriba **OutputSqlDataset** como nombre. Junto al cuadro de texto **Linked service** (Servicio vinculado), seleccione **+ New** (+Nuevo). Un conjunto de datos debe estar asociado con un servicio vinculado. El servicio vinculado tiene la cadena de conexión que usa Data Factory para conectarse a la base de datos SQL en tiempo de ejecución. El conjunto de datos especifica el contenedor, la carpeta y el archivo (opcional) donde se copian los datos.

1. En el cuadro de diálogo **New Linked Service (Azure SQL Database)** [Nuevo servicio vinculado (Azure SQL Database)], realice los siguientes pasos:

    a. En **Name** (Nombre), escriba **AzureSqlDatabaseLinkedService**.

    b. En **Server name** (Nombre del servidor), seleccione su instancia de SQL Server.

    c. En **Database name** (Nombre de la base de datos), seleccione la base de datos SQL.

    d. En **User name** (Nombre de usuario), escriba el nombre del usuario.

    e. En **Password** (Contraseña), escriba la contraseña del usuario.

    f. Seleccione **Test connection** (Prueba de conexión) para probar la conexión.

    g. Seleccione **Finish** (Finalizar) para implementar el servicio vinculado.

    ![Guardar nuevo servicio vinculado](./media/tutorial-copy-data-portal/new-azure-sql-linked-service-window.png)

1. Va automáticamente al cuadro de diálogo **Set Properties** (Establecer propiedades). En **Table** (Tabla), seleccione **[dbo].[emp]** . Después, seleccione **Finalizar**.

1. Vaya a la pestaña con la canalización y, en **Sink Dataset** (Conjunto de datos del receptor), confirme que se ha seleccionado **OutputSqlDataset**.

    ![Pestaña Pipeline (Canalización)](./media/tutorial-copy-data-portal/pipeline-tab-2.png)       

Opcionalmente, puede asignar el esquema del origen al correspondiente esquema de destino. Para ello, siga las instrucciones de [Asignación de esquemas en la actividad de copia](copy-activity-schema-and-type-mapping.md).

## <a name="validate-the-pipeline"></a>Comprobación de la canalización
Para validar la canalización, seleccione **Validar** en la barra de herramientas.

Puede ver el código JSON asociado a la canalización. Para ello, haga clic en **Code** (Código) en la parte superior derecha.

## <a name="debug-and-publish-the-pipeline"></a>Depuración y publicación de la canalización
Puede depurar una canalización antes de publicar artefactos (servicios vinculados, conjuntos de datos y canalizaciones) en Data Factory o en su propio repositorio Git de Azure Repos.

1. Para depurar la canalización, seleccione **Depurar** en la barra de herramientas. Verá el estado de ejecución de la canalización en la pestaña **Output** (Salida) en la parte inferior de la ventana.

1. Una vez que la canalización se pueda ejecutar correctamente, en la barra de herramientas superior, seleccione **Publicar todo**. Esta acción publica las entidades (conjuntos de datos y canalizaciones) que creó para Data Factory.

1. Espere a que aparezca el mensaje **Successfully published** (Publicado correctamente). Para ver los mensajes de notificación, haga clic en **Mostrar notificaciones** en la parte superior derecha (botón de campana).

## <a name="trigger-the-pipeline-manually"></a>Desencadenamiento manual de la canalización
En este paso, desencadenará manualmente la canalización que publicó en el paso anterior.

1. Seleccione **Add Trigger** (Agregar desencadenador) en la barra de herramientas y, después, seleccione **Trigger Now** (Desencadenar ahora). En la página **Pipeline Run** (Ejecución de canalización), seleccione **Finish** (Finalizar).  

1. Vaya a la pestaña **Monitor** (Supervisar) de la izquierda. Verá una ejecución de canalización que se desencadena de forma manual. Puede usar los vínculos de la columna **Actions** (Acciones) para ver los detalles de la actividad y volver a ejecutar la canalización.

    ![La supervisión de la canalización se ejecuta](./media/tutorial-copy-data-portal/monitor-pipeline.png)

1. Para ver las ejecuciones de actividad asociadas con la ejecución de canalización, seleccione el vínculo **View Activity Runs** (Ver ejecuciones de actividad) en la columna **Actions** (Acciones). En este ejemplo, solo hay una actividad, así que solo verá una entrada en la lista. Para ver detalles sobre la operación de copia, seleccione el vínculo **Details** (Detalles) (icono de gafas) en la columna **Actions** (Acciones). Seleccione **Pipeline Runs** (Ejecuciones de canalización) en la parte superior para volver a la vista Pipeline Runs (Ejecuciones de canalización). Para actualizar la vista, seleccione **Refresh** (Actualizar).

    ![Supervisión de las ejecuciones de actividad](./media/tutorial-copy-data-portal/view-activity-runs.png)

1. Compruebe que se agregan dos filas más a la tabla **emp** de la base de datos SQL.

## <a name="trigger-the-pipeline-on-a-schedule"></a>Desencadenamiento de la canalización de forma programada
En esta programación, creará un desencadenador de programación para la canalización. El desencadenador ejecuta la canalización de acuerdo con la programación especificada, como diariamente o cada hora. Aquí establece el desencadenador para que se ejecute cada minuto hasta la fecha y hora de finalización especificadas.

1. Vaya a la pestaña **Author** (Creador) a la izquierda, por encima de la pestaña Monitor (Supervisar).

1. Vaya a la canalización, haga clic en **Add Trigger** (Agregar desencadenador) en la barra de herramientas y seleccione **New/Edit** (Nuevo/Editar).

1. En el cuadro de diálogo **Add Triggers** (Agregar desencadenadores), seleccione **+ New** (+ Nuevo) para el área **Choose trigger** (Elegir desencadenador).

1. En la ventana **New Trigger** (Nuevo desencadenador), lleve a cabo los siguientes pasos:

    a. En **Name** (Nombre), escriba **RunEveryMinute**.

    b. En **End** (Fin), seleccione **On Date** (El día).

    c. En **End On** (Finaliza el), seleccione la lista desplegable.

    d. Seleccione la opción **current day** (día actual). De forma predeterminada, el día de finalización se establece en el día siguiente.

    e. Actualice la parte de **End Time** (Hora de finalización) con unos cuantos minutos que pasen de la fecha y hora actuales. El desencadenador se activa únicamente después de publicar los cambios. Si lo establece solo en un par de minutos de diferencia y no lo publica para entonces, no verá una ejecución de desencadenador.

    f. Seleccione **Aplicar**.

    g. En la opción **Activated** (Activado), seleccione **Yes** (Sí).

    h. Seleccione **Next** (Siguiente).

    ![Botón Activated (Activado)](./media/tutorial-copy-data-portal/trigger-activiated-next.png)

    > [!IMPORTANT]
    > Con cada ejecución de canalización se asocia un costo, así que establezca la fecha de finalización correctamente.
1. En la página **Trigger Run Parameters** (Parámetros de ejecución de desencadenador), revise la advertencia y luego seleccione **Finish** (Finalizar). La canalización de este ejemplo no toma ningún parámetro.

1. Haga clic en **Publicar todo** para publicar el cambio.

1. Vaya a la pestaña **Monitor** (Supervisión) de la izquierda para ver las ejecuciones de canalización desencadenadas.

    ![Ejecuciones de canalización desencadenadas](./media/tutorial-copy-data-portal/triggered-pipeline-runs.png)   

1. Para cambiar de la vista **Pipeline Runs** (Ejecuciones de canalización) a la vista **Trigger Runs** (Ejecuciones de desencadenador), seleccione **Trigger Runs** (Ejecuciones de desencadenador) en la parte superior de la ventana.

1. Verá las ejecuciones de desencadenador en una lista.

1. Compruebe que se insertan dos filas por minuto (para cada ejecución de canalización) en la tabla **emp** hasta la hora de finalización especificada.

## <a name="next-steps"></a>Pasos siguientes
La canalización de este ejemplo copia los datos de una ubicación a otra de Blob Storage. Ha aprendido a:

> [!div class="checklist"]
> * Creación de una factoría de datos.
> * Creación de una canalización con una actividad de copia.
> * Realización de la serie de pruebas de la canalización.
> * Desencadenamiento manual de la canalización.
> * Desencadenamiento de la canalización de forma programada.
> * Supervisión de las ejecuciones de canalización y actividad.


Para aprender a copiar datos desde el entorno local a la nube, avance al tutorial siguiente:

> [!div class="nextstepaction"]
>[Copia de datos del entorno local a la nube](tutorial-hybrid-copy-portal.md)

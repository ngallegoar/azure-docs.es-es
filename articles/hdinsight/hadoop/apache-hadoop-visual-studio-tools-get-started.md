---
title: 'Apache Hadoop y Herramientas de Data Lake para Visual Studio : Azure HDInsight'
description: Aprenda a instalar y utilizar Herramientas de Data Lake para Visual Studio para conectarse a clústeres de Apache Hadoop en Azure HDInsight y luego ejecutar consultas de Hive.
keywords: herramientas hadoop, consulta hive, visual studio, visual studio hadoop
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017,seodec18
ms.topic: conceptual
ms.date: 10/29/2019
ms.openlocfilehash: 4ad58bc2d61f063dce2c23f60a65dcbec48a2303
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "79233720"
---
# <a name="use-data-lake-tools-for-visual-studio-to-connect-to-azure-hdinsight-and-run-apache-hive-queries"></a>Conectarse a Azure HDInsight y ejecutar consultas de Apache Hive con Herramientas de Data Lake para Visual Studio

Aprenda a usar Herramientas de Microsoft Azure Data Lake y Stream Analytics para Visual Studio (también conocido como Herramientas de Data Lake) para conectarse a [clústeres de Apache Hadoop en Azure HDInsight](apache-hadoop-introduction.md) y enviar consultas de Hive.  

Para obtener más información sobre cómo usar HDInsight, consulte [Introducción a HDInsight](apache-hadoop-linux-tutorial-get-started.md).  

Para obtener más información acerca de cómo conectarse a un clúster de Apache Storm, consulte [Desarrollo de topologías de C# para Apache Storm con Herramientas de Data Lake para Visual Studio](../storm/apache-storm-develop-csharp-visual-studio-topology.md).

Data Lake Tools para Visual Studio se puede utilizar para acceder a Azure Data Lake Analytics y a HDInsight. Para más información acerca de Data Lake Tools, consulte [Desarrollo de scripts U-SQL mediante Data Lake Tools para Visual Studio](../../data-lake-analytics/data-lake-analytics-data-lake-tools-get-started.md).

## <a name="prerequisites"></a>Prerequisites

Para completar este artículo y usar Herramientas de Data Lake para Visual Studio, se necesitan los siguientes elementos:

* Un clúster de HDInsight de Azure. Para crear un clúster de HDInsight, consulte [Introducción a Apache Hadoop en Azure HDInsight](apache-hadoop-linux-tutorial-get-started.md). Para ejecutar consultas interactivas de Apache Hive, necesita un clúster de [HDInsight Interactive Query](../interactive-query/apache-interactive-query-get-started.md).  

* [Visual Studio](https://visualstudio.microsoft.com/downloads/). La edición [Visual Studio Community](https://visualstudio.microsoft.com/vs/community/) es gratuita. Las instrucciones que se muestran aquí se aplican a [Visual Studio 2019](https://visualstudio.microsoft.com/downloads/).

## <a name="install-data-lake-tools-for-visual-studio"></a>Instalación de Data Lake Tools para Visual Studio  

Siga las instrucciones adecuadas para instalar la versión que necesita de Herramientas de Data Lake para Visual Studio:

- Para Visual Studio 2017 o Visual Studio 2019:

    Durante la instalación de Visual Studio, asegúrese de incluir la carga de trabajo **Desarrollo de Azure** o la carga de trabajo de **Almacenamiento y procesamiento de datos**.  

    Para las instalaciones existentes de Visual Studio, vaya a la barra de menús del IDE y seleccione **Herramientas** > **Obtener herramientas y características** para abrir el Instalador de Visual Studio. En la pestaña **Cargas de trabajo**, seleccione al menos la carga de trabajo **Desarrollo de Azure** (en **Web y nube**) o la carga de trabajo **Almacenamiento y procesamiento de datos** (en **Otros conjuntos de herramientas**).

  ![Selección de la carga de trabajo en el Instalador de Visual Studio](./media/apache-hadoop-visual-studio-tools-get-started/vs-installation.png)

- Para Visual Studio 2015:

    [Descargue Herramientas de Data Lake](https://www.microsoft.com/download/details.aspx?id=49504). Elija la versión de Data Lake Tools que coincida con su versión de Visual Studio.

## <a name="update-data-lake-tools-for-visual-studio"></a>Actualización de Herramientas de Data Lake para Visual Studio  

Después, asegúrese de actualizar Herramientas de Data Lake a la versión más reciente.

1. Abra Visual Studio.

2. En la ventana de **inicio**, seleccione **Continuar sin código**.

3. En la barra de menús del IDE de Visual Studio, elija **Extensiones** > **Administrar extensiones**.

4. En el cuadro de diálogo **Administrar extensiones**, expanda el nodo **Actualizaciones**.

5. Si la lista de actualizaciones disponibles incluye **Herramientas de Azure Data Lake y Stream Analytics**, selecciónela. A continuación, haga clic en el botón **Actualizar**. Después de que aparezca y desaparezca el cuadro de diálogo **Descargar e instalar**, Visual Studio agregará la extensión **Herramientas de Azure Data Lake y Stream Analytics** a la programación de actualizaciones.

6. Cierre todas las ventanas de Visual Studio. Aparecerá el cuadro de diálogo **Instalador de VSIX**.

7. Seleccione **Licencia** para leer los términos de la licencia y, a continuación, seleccione **Cerrar** para volver al cuadro de diálogo **Instalador de VSIX**.

8. Seleccione **Modificar**. Comienza la instalación de la actualización de la extensión. Pasado un tiempo, el cuadro de diálogo cambia para mostrar que ha terminado de realizar modificaciones. Seleccione **Cerrar** y, luego, reinicie Visual Studio para completar la instalación.

> [!NOTE]  
> Puede utilizar solo Data Lake Tools, versión 2.3.0.0 o posterior, para conectarse a clústeres de Interactive Query y ejecutar consultas de Hive interactivas.

## <a name="connect-to-azure-subscriptions"></a>Conexión a suscripciones de Azure

Herramientas de Data Lake para Visual Studio se puede usar para conectarse a clústeres de HDInsight, realizar algunas operaciones básicas de administración y ejecutar consultas de Hive.

> [!NOTE]  
> Para más información acerca de cómo conectarse a un clúster de Hadoop genérico, consulte [Cómo escribir y enviar consultas de Hive mediante Visual Studio](https://blogs.msdn.microsoft.com/xiaoyong/2015/05/04/how-to-write-and-submit-hive-queries-using-visual-studio/).

### <a name="connect-to-an-azure-subscription"></a>Conexión a una suscripción de Azure

Para conectarse a su suscripción de Azure:

1. Abra Visual Studio.

2. En la ventana de **inicio**, seleccione **Continuar sin código**.

3. En la barra de menús del IDE, vaya a **Ver** > **Explorador de servidores**.

4. En el **Explorador de servidores**, haga clic con el botón derecho en **Azure**, seleccione **Conectar a la suscripción de Microsoft Azure** y complete el proceso de autenticación. En el **Explorador de servidores**, expanda **Azure** > **HDInsight** para ver una lista de los clústeres de HDInsight existentes.

5. Si no tiene ningún clúster, cree uno mediante Azure Portal, Azure PowerShell o el SDK de HDInsight. Para obtener más información, consulte [Configuración de clústeres en HDInsight](../hdinsight-hadoop-provision-linux-clusters.md).

   ![Lista de clústeres de HDInsight, Explorador de servidores, Visual Studio](./media/apache-hadoop-visual-studio-tools-get-started/hdinsight-visual-studio-tools-server-explorer.png)

6. Expanda un clúster de HDInsight. El clúster contiene nodos para **bases de datos de Hive**, una cuenta de almacenamiento predeterminada, todas las cuentas de almacenamiento vinculadas adicionales y el **registro del servicio Hadoop**. Puede expandir las entidades.

Una vez conectado a su suscripción de Azure, puede realizar las siguientes tareas.

### <a name="connect-to-azure-from-visual-studio"></a>Conexión a Azure desde Visual Studio

Para conectarse a Azure Portal desde Visual Studio:

1. En el **Explorador de servidores**, expanda **Azure** > **HDInsight** y seleccione el clúster.

2. Haga clic con el botón derecho en un clúster de HDInsight y seleccione **Administrar clústeres en Azure Portal**.

### <a name="offer-questions-and-feedback-from-visual-studio"></a>Preguntas y comentarios desde Visual Studio

Para hacer preguntas y realizar comentarios desde Visual Studio:

1. En el Explorador de servidores, seleccione **Azure** > **HDInsight**.

2. Haga clic con el botón derecho en **HDInsight** y seleccione **Foro de MSDN** para formular preguntas, o bien **Enviar comentarios** para aportar sus comentarios.

## <a name="link-to-or-edit-a-cluster"></a>Vinculación o edición de clústeres

> [!NOTE]
> Actualmente, el único tipo de clúster de HDInsight al que se puede vincular es un tipo Hive.

Para vincular un clúster de HDInsight:

1. Haga clic con el botón derecho en **HDInsight** y, después, seleccione **Vincular un clúster de HDInsight** para mostrar el cuadro de diálogo **Vincular un clúster de HDInsight**.

2. Escriba una **dirección URL de conexión** con el formato *https\://\<nombre del&nbsp;clúster>.azurehdinsight.net*. El **nombre del clúster** se rellena automáticamente con la parte del nombre del clúster de su dirección URL cuando se desplaza a otro campo. Escriba un **nombre de usuario** y una **contraseña** y seleccione **Siguiente**.

    ![Vincular un clúster, HDInsight, Visual Studio](./media/apache-hadoop-visual-studio-tools-get-started/hdinsight-visual-studio-tools-link-cluster-dialog.png)

3. Seleccione **Finalizar**. Si la vinculación del clúster se realiza correctamente, el clúster se muestra en el nodo **HDInsight**.

Para actualizar un clúster vinculado, haga clic con el botón derecho en el clúster y seleccione **Editar**. Después, puede actualizar la información del clúster.

![Editar un clúster vinculado, HDInsight, Visual Studio](./media/apache-hadoop-visual-studio-tools-get-started/hdinsight-visual-studio-tools-link-cluster-update.png)

## <a name="explore-linked-resources"></a>Exploración de recursos vinculados
Desde el Explorador de servidores, podrá ver la cuenta de almacenamiento predeterminada y las cuentas de almacenamiento vinculadas. Expanda la cuenta de almacenamiento predeterminada para ver los contenedores en la cuenta de almacenamiento. Se marcarán la cuenta de almacenamiento predeterminada y el contenedor predeterminado.

![Recursos vinculados de Herramientas de Data Lake para Visual Studio en el Explorador de servidores](./media/apache-hadoop-visual-studio-tools-get-started/hdinsight-visual-studio-tools-linked-resources.png)

Haga clic con el botón derecho en un contenedor y seleccione **Ver contenedor** para ver su contenido. Después de abrir un contenedor, puede usar los botones de la barra de herramientas **Actualizar** (para actualizar la lista de contenido), **Cargar blobs**, **Eliminar blobs seleccionados**, **Abrir un blob** y **Guardar como** (para descargar los blobs seleccionados).

![Lista de contenedores y operaciones de blobs, clúster de HDInsight, Visual Studio](./media/apache-hadoop-visual-studio-tools-get-started/hdinsight-visual-studio-tools-blob-operations.png)

## <a name="run-interactive-apache-hive-queries"></a>Ejecución de consultas interactivas de Apache Hive
[Apache Hive](https://hive.apache.org) es una infraestructura de almacenamiento de datos que se basa en Hadoop. Hive se utiliza para el análisis, las consultas y resumen de los datos. Data Lake Tools para Visual Studio se puede usar para ejecutar consultas de Hive desde Visual Studio. Para obtener más información sobre Hive, consulte [¿Qué son Apache Hive y HiveQL en Azure HDInsight?](hdinsight-use-hive.md).

[Interactive Query en Azure HDInsight](../interactive-query/apache-interactive-query-get-started.md) usa [Hive en LLAP](https://cwiki.apache.org/confluence/display/Hive/LLAP) en Apache Hive 2.1. Interactive Query aporta interactividad a las complejas consultas de estilo de almacenamiento de datos en grandes conjuntos de datos almacenados. La ejecución de consultas de Hive en Interactive Query es mucho más rápida que los trabajos por lotes tradicionales de Hive. 

> [!NOTE]  
> Solo se pueden ejecutar consultas de Hive interactivas al conectarse a un clúster de [HDInsight Interactive Query](../interactive-query/apache-interactive-query-get-started.md).

También se puede utilizar Data Lake Tools para Visual Studio para ver el contenido de un trabajo de Hive. Data Lake Tools para Visual Studio recopila y muestra los registros Yarn de determinados trabajos de Hive.

En el **Explorador de servidores**, vaya a **Azure** > **HDInsight** y seleccione el clúster.  Este nodo es el punto de partida en el **Explorador de servidores** para las secciones que siguen.

### <a name="view-hivesampletable"></a>Ver hivesampletable

Todos los clústeres de HDInsight tienen una tabla de Hive de ejemplo denominada `hivesampletable`.  

En el clúster, seleccione **Bases de datos de Hive** > **predeterminado** > **hivesampletable**.

- Para ver el esquema de `hivesampletable`:

    Expanda **hivesampletable**. Se muestran los nombres y los tipos de datos de las columnas de `hivesampletable`.

- Para ver los datos de `hivesampletable`:

    Haga clic con el botón derecho en **hivesampletable** y seleccione **Ver las 100 primeras filas**. La lista con 100 resultados aparece en la ventana **Tabla de Hive: hivesampletable**. Esta acción equivale a ejecutar la siguiente consulta de Hive mediante el uso del controlador ODBC de Hive:

    `SELECT * FROM hivesampletable LIMIT 100`

    Puede personalizar el recuento de filas si cambia **Número de filas**; puede elegir 50, 100, 200 o 1000 filas en la lista desplegable.

### <a name="create-hive-tables"></a>Crear tablas de Hive
Para crear una tabla de Hive, puede usar la GUI o las consultas de Hive. Para obtener información acerca del uso de consultas de Hive, consulte [Creación y ejecución de consultas de Hive](#create-and-run-hive-queries).

1. En el clúster, seleccione **Bases de datos de Hive** > **predeterminado**.

2. Haga clic con el botón derecho en **predeterminado** y seleccione **Crear tabla**.

3. Configure la tabla.

4. Seleccione el botón **Crear tabla** para enviar el trabajo, de este modo se crea la nueva tabla de Hive.

    ![Ventana Crear tabla, Hive, clúster de HDInsight, Visual Studio](./media/apache-hadoop-visual-studio-tools-get-started/hdinsight-visual-studio-tools-create-hive-table.png)

### <a name="create-and-run-hive-queries"></a>Creación y ejecución de consultas de Hive
Para crear y ejecutar consultas de Hive tiene dos opciones:

* Crear consultas ad hoc
* Crear una aplicación de Hive

#### <a name="create-an-ad-hoc-query"></a>Creación de una consulta ad hoc

Para crear y ejecutar una consulta ad hoc:

1. Haga clic con el botón derecho en el clúster en que desee ejecutar la consulta y seleccione **Escribir una consulta de Hive**.  

2. Especifique una consulta de Hive.

    El editor de Hive es compatible con IntelliSense. Data Lake Tools para Visual Studio es compatible con la carga de metadatos remotos cuando se edita un script de Hive. Por ejemplo, si escribe `SELECT * FROM`, IntelliSense enumera todos los nombres de tabla sugeridos. Cuando se especifica un nombre de tabla, IntelliSense enumera los nombres de columna. Las herramientas admiten casi todas las instrucciones DML de Hive, subconsultas y UDF integradas.

    ![Ejemplo 1 de IntelliSense, consulta ad hoc de Hive, clúster de HDInsight, Visual Studio](./media/apache-hadoop-visual-studio-tools-get-started/hdinsight-visual-studio-tools-intellisense-table-names.png)

    ![Ejemplo 2 de IntelliSense, consulta ad hoc de Hive, clúster de HDInsight, Visual Studio](./media/apache-hadoop-visual-studio-tools-get-started/hdinsight-visual-studio-tools-intellisense-column-names.png)

    > [!NOTE]  
    > IntelliSense solo sugiere los metadatos del clúster que se seleccionan en la barra de herramientas de HDInsight.

    Esta es una consulta de ejemplo que puede usar:

    ```sql
    SELECT devicemodel, COUNT(devicemodel) AS deviceCount
    FROM hivesampletable
    GROUP BY devicemodel
    ORDER BY devicemodel
    ```

3. Elija el modo de ejecución:

    * **Interactivo**  

        En la primera lista desplegable, elija **Interactivo** y, a continuación, seleccione **Ejecutar**.

        ![Modo interactivo, consulta ad hoc de Hive, clúster de HDInsight, Visual Studio](./media/apache-hadoop-visual-studio-tools-get-started/hdinsight-query-execute.png)  

    * **Batch**  

        En la primera lista desplegable, elija **Por lotes** y, a continuación, seleccione **Enviar** (o seleccione el icono desplegable situado junto a **Enviar** y elija **Avanzado**).

        ![Modo por lotes, consulta ad hoc de Hive, clúster de HDInsight, Visual Studio](./media/apache-hadoop-visual-studio-tools-get-started/hdinsight-query-batch.png)

        Si selecciona la opción de envío avanzado, aparecerá el cuadro de diálogo **Enviar script**. Configure el **nombre del trabajo**, los **argumentos**, las **configuraciones adicionales** y el **estado de directorio** del script.

        ![Cuadro de diálogo Enviar script, consulta ad hoc de Hive, clúster de HDInsight, Visual Studio](./media/apache-hadoop-visual-studio-tools-get-started/hdinsight-visual-studio-tools-submit-jobs-advanced.png)

      > [!NOTE]  
      > No se pueden enviar lotes a clústeres de Interactive Query.  Hay que usar el modo interactivo.

#### <a name="create-a-hive-application"></a>Crear una aplicación de Hive

Para crear y ejecutar una solución de Hive:

1. En la barra de menús, elija **Archivo** > **Nuevo** > **Proyecto**.

2. En la ventana **Crear un nuevo proyecto**, seleccione el cuadro de búsqueda y escriba **Hive**. A continuación, elija **Aplicación de Hive** y seleccione **Siguiente**.

3. En la ventana **Configure su nuevo proyecto**, escriba un **nombre de proyecto**, elija o cree la **ubicación** del proyecto y, después, seleccione **Crear**.

    ![Nueva aplicación de Hive, ventana Configure su nuevo proyecto, HDInsight, Visual Studio](./media/apache-hadoop-visual-studio-tools-get-started/hdinsight-visual-studio-tools-new-hive-project.png)

4. En el **Explorador de soluciones**, haga doble clic en **Script.hql** para abrir el script.

### <a name="view-job-summary-and-output"></a>Visualización de salida y resumen del trabajo

El resumen del trabajo varía ligeramente entre los modos por **lotes** e **interactivo**.

![Ventanas de resumen de los trabajos de Hive, modo por lotes y modo interactivo, Visual Studio](./media/apache-hadoop-visual-studio-tools-get-started/hdinsight-job-summary.png)

Use el icono **Actualizar** para actualizar el estado hasta que el estado del trabajo cambie a **Finalizado**.  

* Para obtener detalles del trabajo del modo por **lotes**, seleccione los vínculos de la parte inferior para ver **Consulta de trabajo**, **Salida del trabajo**, **Registro de trabajo** o **Ver registros de Yarn**.

* Para obtener detalles del trabajo del modo **interactivo**, vea los paneles **Salida** y **Salida de HiveServer2**.

    ![Salida de trabajos interactivos de Hive, clúster de HDInsight, Visual Studio](./media/apache-hadoop-visual-studio-tools-get-started/hdinsight-job-details.png)

### <a name="view-job-graph"></a>Visualización del gráfico del trabajo

Actualmente, solo se muestran gráficos de trabajo de los trabajos de Hive que usan Tez como motor de ejecución.  Para obtener más información sobre la habilitación de Tez, consulte [¿Qué son Apache Hive y HiveQL en Azure HDInsight?](hdinsight-use-hive.md).  Vea también [Uso de Apache Tez en lugar de MapReduce](../hdinsight-hadoop-optimize-hive-query.md#use-apache-tez-instead-of-map-reduce).  

Para ver todos los operadores que hay en el vértice, haga doble clic en los vértices del gráfico del trabajo. También puede señalar a un operador concreto para ver más detalles de él.

Aun habiendo especificado Tez como motor de ejecución, es posible que no se muestre el gráfico del trabajo si no hay iniciada ninguna aplicación Tez.  Esta situación puede darse porque el trabajo no contiene instrucciones DML o porque las instrucciones DML pueden volver sin iniciar una aplicación Tez. Por ejemplo, `SELECT * FROM table1` no iniciará la aplicación Tez.

![Gráfico de trabajo de Apache Hive, Visual Studio](./media/apache-hadoop-visual-studio-tools-get-started/hdinsight-fast-path-hive-execution.png)

### <a name="view-task-execution-detail"></a>Visualización de los detalles de ejecución de la tarea

En el gráfico del trabajo, se puede seleccionar **Detalles de ejecución de la tarea** para obtener información estructurada y visualizada de los trabajos de Hive. También puede obtener más información del trabajo. Si se produce algún problema de rendimiento, puede usar la vista para obtener más información sobre el mismo. Por ejemplo, puede recuperar información acerca de cómo funciona cada tarea e información detallada acerca de cada tarea (lectura y escritura de datos, hora de programación, de inicio o de fin, etc.). Esta información se puede usar para ajustar las configuraciones de trabajo o la arquitectura del sistema en función de la información visualizada.

![Vista Ejecución de tareas, Herramientas de Data Lake para Visual Studio](./media/apache-hadoop-visual-studio-tools-get-started/hdinsight-visual-studio-tools-task-execution-view.png)

### <a name="view-hive-jobs"></a>Ver trabajos de Hive

Puede ver consultas de trabajo, salidas de trabajo, registros de trabajo y registros Yarn para trabajos de Hive.

En la versión más reciente de la herramienta puede ver el contenido de los trabajos de Hive al recopilar y exponer los registros Yarn. Un registro Yarn puede ayudarle a investigar problemas de rendimiento. Para más información acerca de la forma en que HDInsight recopila registros Yarn, consulte [Acceso a registros de aplicación de YARN de Apache Hadoop](../hdinsight-hadoop-access-yarn-app-logs-linux.md).

Para ver trabajos de Hive:

1. Haga clic con el botón derecho en un clúster de HDInsight y seleccione **Ver trabajos**.

    ![Ver trabajos, Apache Hive, clúster de HDInsight, Visual Studio](./media/apache-hadoop-visual-studio-tools-get-started/hdinsight-visual-studio-tools-view-hive-jobs.png)

    Aparece una lista de los trabajos de Hive que se ejecutaron en el clúster.  

2. Seleccione un trabajo. En la ventana **Resumen de trabajos de Hive**, seleccione uno de los siguientes vínculos:
    - **Consulta de trabajo**
    - **Salida de trabajo**
    - **Registro de trabajo**  
    - **Registro de Yarn**

## <a name="run-apache-pig-scripts"></a>Ejecución de scripts de Apache Pig

1. En la barra de menús, elija **Archivo** > **Nuevo** > **Proyecto**.

2. En la ventana de **inicio**, seleccione el cuadro de búsqueda y escriba **Pig**. Seleccione **Aplicación Pig** y, después, **Siguiente**.

3. En la ventana **Configure su nuevo proyecto**, escriba un **nombre de proyecto** y elija o cree la **ubicación** del proyecto. Seleccione **Crear**.

4. En el **Explorador de soluciones** del IDE, haga doble clic en **Script.pig** para abrir el script.

## <a name="feedback-and-known-issues"></a>Comentarios y problemas conocidos

* Se ha corregido un problema en el que no se muestran los resultados que comienzan por valores null. Si está bloqueado debido a este problema, póngase en contacto con el equipo de soporte técnico.

* La codificación del script HQL que crea Visual Studio depende de la configuración regional del usuario. El script no se ejecuta correctamente si se carga en un clúster como un archivo binario.

## <a name="next-steps"></a>Pasos siguientes

En este artículo, ha aprendido a usar el paquete Data Lake Tools para Visual Studio para establecer conexión a los clústeres de HDInsight desde Visual Studio. También ha aprendido a ejecutar una consulta de Hive. Para obtener más información, consulte estos artículos:

* [Ejecución de las consultas de Apache Hive mediante Herramientas de Data Lake para Visual Studio](apache-hadoop-use-hive-visual-studio.md)
* [¿Qué son Apache Hive y HiveQL en Azure HDInsight?](hdinsight-use-hive.md)
* [Creación de un clúster de Apache Hadoop: plantilla](apache-hadoop-linux-tutorial-get-started.md)
* [Envío de trabajos de Apache Hadoop en HDInsight](submit-apache-hadoop-jobs-programmatically.md)
* [Análisis de datos de Twitter con Apache Hive y Apache Hadoop en HDInsight](../hdinsight-analyze-twitter-data-linux.md)

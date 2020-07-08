---
title: Depuración de trabajos de Apache Spark que se ejecutan en Azure HDInsight
description: Usar la interfaz de usuario de YARN, la de Spark y el servidor de historial de Spark para realizar un seguimiento y depurar trabajos que se ejecutan en un clúster de Spark en HDInsight de Azure
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: how-to
ms.custom: hdinsightactive,seoapr2020
ms.date: 04/23/2020
ms.openlocfilehash: 817b8976f5d014d990945816c2df1c7e8ed729fb
ms.sourcegitcommit: 124f7f699b6a43314e63af0101cd788db995d1cb
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/08/2020
ms.locfileid: "86084892"
---
# <a name="debug-apache-spark-jobs-running-on-azure-hdinsight"></a>Depuración de trabajos de Apache Spark que se ejecutan en Azure HDInsight

En este artículo, aprenderá a realizar un seguimiento de trabajos de Apache Spark que se ejecutan en clústeres de HDInsight y a depurarlos. Depure con la interfaz de usuario de YARN de Apache Hadoop, la interfaz de usuario de Spark y el servidor de historial de Spark. Se inicia un trabajo de Spark mediante un cuaderno disponible en el clúster de Spark, **Machine Learning: Predictive analysis on food inspection data using MLLib** (Aprendizaje automático: análisis predictivo en datos de inspección de alimentos con MLLib). Siga estos pasos para realizar el seguimiento de una aplicación que haya enviado mediante cualquier otro enfoque, por ejemplo, **spark-submit**.

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

## <a name="prerequisites"></a>Prerrequisitos

* Un clúster de Apache Spark en HDInsight. Para obtener instrucciones, vea [Creación de clústeres Apache Spark en HDInsight de Azure](apache-spark-jupyter-spark-sql.md).

* Debería haber iniciado la ejecución del cuaderno, **[Machine learning: Predictive analysis on food inspection data using MLLib](apache-spark-machine-learning-mllib-ipython.md)** (Aprendizaje automático: análisis predictivo en datos de inspección de alimentos con MLLib). Para obtener instrucciones sobre cómo ejecutar este cuaderno, siga el vínculo.  

## <a name="track-an-application-in-the-yarn-ui"></a>Seguimiento de una aplicación en la interfaz de usuario de YARN

1. Inicie la interfaz de usuario de YARN. Seleccione **Yarn** en **Paneles de clúster**.

    ![Inicio de la interfaz de usuario de YARN desde Azure Portal](./media/apache-spark-job-debugging/launch-apache-yarn-ui.png)

   > [!TIP]  
   > También puede iniciar la interfaz de usuario de YARN desde la de Ambari. Para iniciar la interfaz de usuario de Ambari, seleccione **Inicio de Ambari** en **Paneles de clúster**. En la interfaz de usuario de Ambari, vaya a **YARN** > **Vínculos rápidos** > la instancia activa de Resource Manager > **interfaz de usuario de Resource Manager**.

2. Dado que se inició el trabajo de Spark mediante cuadernos de Jupyter Notebook, la aplicación se llama **remotesparkmagics** (el nombre de todas las aplicaciones que se inician desde los cuadernos). Seleccione el identificador de la aplicación, que se encuentra junto al nombre de la aplicación, para más información sobre el trabajo. Esta acción inicia la vista de la aplicación.

    ![El servidor de historial de Spark busca el identificador de aplicación de Spark](./media/apache-spark-job-debugging/find-application-id1.png)

    Para estas aplicaciones que se inician desde los cuadernos de Jupyter Notebook, el estado es siempre **RUNNING** (EN EJECUCIÓN) hasta que sale del cuaderno.

3. En la vista de la aplicación, puede explorar más en profundidad para averiguar los contenedores asociados con la aplicación y los registros (stdout y stderr). También puede iniciar la interfaz de usuario de Spark haciendo clic en el vínculo correspondiente a la **Dirección URL de seguimiento**, tal como se muestra a continuación.

    ![El servidor de historial de Spark descarga los registros de contenedor](./media/apache-spark-job-debugging/download-container-logs.png)

## <a name="track-an-application-in-the-spark-ui"></a>Seguimiento de una aplicación en la interfaz de usuario de Spark

En la interfaz de usuario de Spark, puede explorar en profundidad los trabajos de Spark generados por la aplicación que ha iniciado anteriormente.

1. Para iniciar la interfaz de usuario de Spark, desde la vista de la aplicación, seleccione el vínculo en la **Dirección URL de seguimiento**, como se muestra en la captura de pantalla anterior. Puede ver todos los trabajos de Spark iniciados por la aplicación que se ejecuta en el cuaderno de Jupyter Notebook.

    ![Pestaña de trabajos del servidor de historial de Spark](./media/apache-spark-job-debugging/view-apache-spark-jobs.png)

2. Seleccione la pestaña **Executors** (Ejecutores) para ver la información de procesamiento y almacenamiento de cada elemento de ejecución. También puede recuperar la pila de llamadas seleccionando el vínculo **Thread Dump** (Volcado de subprocesos).

    ![Pestaña de los ejecutores del servidor de historial de Spark](./media/apache-spark-job-debugging/view-spark-executors.png)

3. Seleccione la pestaña **Stages** (Fases) para ver las fases asociadas a la aplicación.

    ![Pestaña de fases del historial de Spark](./media/apache-spark-job-debugging/view-apache-spark-stages.png "Ver fases de Spark")

    Cada fase puede tener varias tareas de las que puede ver las estadísticas de ejecución, como se muestra a continuación.

    ![Detalles de la pestaña de fases del historial de Spark](./media/apache-spark-job-debugging/view-spark-stages-details.png "Ver detalles de fases de Spark")

4. En la página de detalles de la fase, puede iniciar la visualización de DAG. Expanda el vínculo **DAG Visualization** (Visualización de DAG) en la parte superior de la página, como se muestra a continuación.

    ![Ver visualización DAG de las fases de Spark](./media/apache-spark-job-debugging/view-spark-stages-dag-visualization.png)

    El DAG o grafo acíclico dirigido representa las distintas fases de la aplicación. Cada cuadro azul en el grafo representa una operación de Spark que se invoca desde la aplicación.

5. En la página de detalles de la fase, también puede iniciar la vista de escala de tiempo de la aplicación. Expanda el vínculo **Event Timeline** (Escala de tiempo del evento) en la parte superior de la página, como se muestra a continuación.

    ![Ver escala de tiempo de eventos de fases de Spark](./media/apache-spark-job-debugging/view-spark-stages-event-timeline.png)

    Esta imagen permite mostrar los eventos de Spark en forma de una escala de tiempo. La vista de la escala de tiempo está disponible en tres niveles: entre trabajos, dentro de un trabajo y dentro de una fase. La imagen anterior captura la vista de la escala de tiempo para una fase determinada.

   > [!TIP]  
   > Si selecciona la casilla **Enable zooming** (Habilitar zoom), puede desplazarse a izquierda y derecha en la vista de escala de tiempo.

6. Otras pestañas de la interfaz de usuario de Spark proporcionan también información útil acerca de la instancia de Spark.

   * Pestaña Almacenamiento: si la aplicación crea varios RDD, podrá encontrar información sobre estos en esta pestaña.
   * Pestaña Environment (Entorno): esta pestaña proporciona información útil sobre la instancia de Spark, como:
     * La versión de la escala
     * El directorio de registro de eventos asociado con el clúster
     * El número de núcleos del ejecutor de la aplicación

## <a name="find-information-about-completed-jobs-using-the-spark-history-server"></a>Búsqueda de información acerca de los trabajos completados con el servidor de historial de Spark

Cuando se completa un trabajo, se conserva la información sobre este en el servidor de historial de Spark.

1. Para iniciar el servidor de historial de Spark, en la página **Información general**, seleccione **Servidor de historial de Spark** en **Paneles de clúster**.

    ![Azure Portal inicia el servidor de historial de Spark](./media/apache-spark-job-debugging/launch-spark-history-server.png "Iniciar servidor de historial 1 de Spark")

   > [!TIP]  
   > También puede iniciar la interfaz de usuario del servidor de historial de Spark desde la de Ambari. Para iniciar la interfaz de usuario de Ambari, en la hoja de información general, seleccione **Inicio de Ambari** en **Paneles de clúster**. En la interfaz de usuario de Ambari, vaya a **Spark2** > **Quick Links** (Vínculos rápidos)  > **Spark2 History Server UI** (UI del servidor de historial de Spark).

2. Aparecerá una lista de todas las aplicaciones completadas. Seleccione un identificador de aplicación para explorar en profundidad una aplicación y obtener más información.

    ![Aplicaciones completadas por el servidor de historial de Spark](./media/apache-spark-job-debugging/view-completed-applications.png "Iniciar servidor de historial 2 de Spark")

## <a name="see-also"></a>Consulte también

* [Administración de recursos para el clúster Apache Spark en HDInsight de Azure](apache-spark-resource-manager.md)
* [Depuración de trabajos de Apache Spark mediante el servidor de historial de Spark extendido](apache-azure-spark-history-server.md)
* [Depuración de aplicaciones de Apache Spark con Azure Toolkit for IntelliJ mediante SSH](apache-spark-intellij-tool-debug-remotely-through-ssh.md)

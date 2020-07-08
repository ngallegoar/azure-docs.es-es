---
title: Administración de recursos de un clúster Apache Spark en Azure HDInsight
description: Aprenda a administrar recursos de administración para clústeres Spark en Azure HDInsight a fin de mejorar el rendimiento.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: how-to
ms.custom: hdinsightactive
ms.date: 12/06/2019
ms.openlocfilehash: 5427077a4b07917c8852d0a63c815195e776b9de
ms.sourcegitcommit: 124f7f699b6a43314e63af0101cd788db995d1cb
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/08/2020
ms.locfileid: "86085198"
---
# <a name="manage-resources-for-apache-spark-cluster-on-azure-hdinsight"></a>Administración de recursos de un clúster Apache Spark en Azure HDInsight

Aprenda a acceder a interfaces como la interfaz de usuario de [Apache Ambari](https://ambari.apache.org/), la de [Apache Hadoop YARN](https://hadoop.apache.org/docs/current/hadoop-yarn/hadoop-yarn-site/YARN.html) y el [servidor de historial de Spark](./apache-azure-spark-history-server.md) asociados al clúster de [Apache Spark](https://spark.apache.org/), y a ajustar la configuración del clúster para optimizar el rendimiento.

## <a name="open-the-spark-history-server"></a>Apertura del servidor de historial de Spark

Servidor de historial de Spark es la interfaz de usuario web para aplicaciones de Spark completadas y en ejecución. Es una extensión de la interfaz de usuario web de Spark. Para obtener información completa, consulte [Servidor de historial de Spark](./apache-azure-spark-history-server.md).

## <a name="open-the-yarn-ui"></a>Apertura de la interfaz de usuario de YARN

Puede utilizar la interfaz de usuario de YARN para supervisar las aplicaciones que se encuentran en ejecución en el clúster Spark.

1. En [Azure Portal](https://portal.azure.com/), abra el clúster de Spark. Para obtener más información, consulte [Enumeración y visualización de clústeres](../hdinsight-administer-use-portal-linux.md#showClusters).

2. Desde **Paneles de clúster**, seleccione **Yarn**. Cuando se le pida, escriba las credenciales de administrador del clúster Spark.

    ![Iniciar interfaz de usuario de YARN](./media/apache-spark-resource-manager/azure-portal-dashboard-yarn.png)

   > [!TIP]  
   > También puede iniciar la interfaz de usuario de YARN desde la de Ambari. En la interfaz de usuario de Ambari, vaya a **YARN** > **Quick Links** (Vínculos rápidos)  > **Active** (Activo)  > **Resource Manager UI** (Interfaz de usuario del Administrador de recursos).

## <a name="optimize-clusters-for-spark-applications"></a>Optimice los clústeres para las aplicaciones Spark

Los tres parámetros clave que se pueden utilizar para la configuración de Spark según los requisitos de la aplicación son `spark.executor.instances`, `spark.executor.cores` y `spark.executor.memory`. Un ejecutor es un proceso que se inicia para una aplicación Spark. Se ejecuta en el nodo de trabajo y es responsable de realizar las tareas de la aplicación. El número predeterminado de ejecutores y el tamaño de estos para cada clúster se calcula en función del número de nodos de trabajo y el tamaño de estos. Esta información se almacena en `spark-defaults.conf` en los nodos principales del clúster.

Los tres parámetros de configuración se pueden configurar en el nivel de clúster (para todas las aplicaciones que se ejecutan en el clúster) o se pueden especificar también para cada aplicación individual.

### <a name="change-the-parameters-using-ambari-ui"></a>Cambio de los parámetros mediante la interfaz de usuario de Ambari

1. Desde la interfaz de usuario de Ambari, vaya a **Spark2** > **Configs** (Configuraciones)  > **Custom spark2-defaults** (spark2-defaults personalizado).

    ![Establecimiento de los parámetros mediante Ambari personalizado](./media/apache-spark-resource-manager/ambari-ui-spark2-configs.png "Establecimiento de los parámetros mediante Ambari personalizado")

1. Los valores predeterminados son necesarios para hacer que se ejecuten cuatro aplicaciones Spark simultáneamente en el clúster. Puede cambiar estos valores desde la interfaz de usuario, como se muestra en la captura de pantalla siguiente:

    ![Establecimiento de los parámetros mediante Ambari](./media/apache-spark-resource-manager/ambari-ui-spark2-defaults.png "Establecer los parámetros mediante Ambari")

1. Seleccione **Save** (Guardar) para guardar los cambios de configuración. En la parte superior de la página se le pide que reinicie todos los servicios afectados. Seleccione **Reiniciar**.

    ![Reiniciar los servicios](./media/apache-spark-resource-manager/apache-ambari-restart-services.png)

### <a name="change-the-parameters-for-an-application-running-in-jupyter-notebook"></a>Cambio de los parámetros de una aplicación que se ejecuta en Jupyter Notebook

Para aplicaciones que se ejecutan en Jupyter Notebook, puede utilizar la instrucción mágica `%%configure` para realizar los cambios de configuración. Idealmente, debe realizar estos cambios al comienzo de la aplicación, antes de ejecutar la primera celda de código. Esto garantiza que la configuración se aplique a la sesión de Livy cuando se cree. Si desea cambiar la configuración en una fase posterior de la aplicación, debe utilizar el parámetro `-f` . Sin embargo, al hacerlo, se perderán todos los progresos en la aplicación.

El siguiente fragmento de código muestra cómo cambiar la configuración de una aplicación que se ejecuta en Jupyter.

```scala
%%configure
{"executorMemory": "3072M", "executorCores": 4, "numExecutors":10}
```

Los parámetros de configuración deben pasarse como una cadena JSON y deben estar en la siguiente línea después de la instrucción mágica, como se muestra en la columna de ejemplo.

### <a name="change-the-parameters-for-an-application-submitted-using-spark-submit"></a>Cambio de los parámetros de una aplicación enviada mediante spark-submit

El siguiente comando es un ejemplo de cómo cambiar los parámetros de configuración de una aplicación de lote que se envía mediante `spark-submit`.

```scala
spark-submit --class <the application class to execute> --executor-memory 3072M --executor-cores 4 –-num-executors 10 <location of application jar file> <application parameters>
```

### <a name="change-the-parameters-for-an-application-submitted-using-curl"></a>Cambio de los parámetros de una aplicación enviada mediante cURL

El siguiente comando es un ejemplo de cómo cambiar los parámetros de configuración de una aplicación de lote que se envía mediante cURL.

```bash
curl -k -v -H 'Content-Type: application/json' -X POST -d '{"file":"<location of application jar file>", "className":"<the application class to execute>", "args":[<application parameters>], "numExecutors":10, "executorMemory":"2G", "executorCores":5' localhost:8998/batches
```

### <a name="change-these-parameters-on-a-spark-thrift-server"></a>Cambiar estos parámetros en un servidor Thrift de Spark

El servidor Thrift de Spark proporciona acceso JDBC/ODBC a un clúster de Spark y se utiliza para enviar consultas de Spark SQL. Herramientas como Power BI, Tableau, etc. usan el protocolo ODBC para comunicarse con el servidor Thrift de Spark y ejecutar consultas de Spark SQL como una aplicación Spark. Cuando se crea un clúster de Spark, se inician dos instancias del servidor Thrift de Spark, una en cada nodo principal. Cada servidor Thrift de Spark está visible como una aplicación Spark en la interfaz de usuario de YARN.

El servidor Thrift de Spark utiliza la asignación dinámica de ejecutores de Spark y, por tanto, las instancias de `spark.executor.instances` no se utilizan. En su lugar, el servidor Thrift de Spark usa `spark.dynamicAllocation.maxExecutors` y `spark.dynamicAllocation.minExecutors` para especificar el recuento de ejecutores. Los parámetros de configuración `spark.executor.cores` y `spark.executor.memory` se usan para modificar el tamaño del ejecutor. Puede cambiar estos parámetros como se muestra en los siguientes pasos:

* Expanda la categoría **Advanced spark2-thrift-sparkconf** (spark2-thrift-sparkconf avanzado) para actualizar los parámetros `spark.dynamicAllocation.maxExecutors` y `spark.dynamicAllocation.minExecutors`.

    ![Configuración del servidor Thrift de Spark](./media/apache-spark-resource-manager/ambari-ui-advanced-thrift-sparkconf.png "Configurar el servidor Thrift de Spark")

* Expanda la categoría **Custom spark2-thrift-sparkconf** (spark2-thrift-sparkconf personalizado) para actualizar los parámetros `spark.executor.cores` y `spark.executor.memory`

    ![Configuración del parámetro del servidor Thrift de Spark](./media/apache-spark-resource-manager/ambari-ui-custom-thrift-sparkconf.png "Configuración del parámetro del servidor Thrift de Spark")

### <a name="change-the-driver-memory-of-the-spark-thrift-server"></a>Cambiar la memoria del controlador del servidor Thrift de Spark

La memoria del controlador del servidor Thrift de Spark se ha configurado al 25 % del tamaño de la RAM del nodo principal, suponiendo que el tamaño total de la RAM del nodo principal sea superior a 14 GB. Puede utilizar la interfaz de usuario de Ambari para cambiar la configuración de memoria del controlador, como se muestra en la captura de pantalla siguiente:

Desde la interfaz de usuario de Ambari, vaya a **Spark2** > **Configs** (Configuraciones)  > **Advanced spark2-env** (spark2-env avanzado). A continuación, proporcione el valor de **spark_thrift_cmd_opts**.

## <a name="reclaim-spark-cluster-resources"></a>Reclamar recursos de clúster de Spark

Debido a la asignación dinámica de Spark, los únicos recursos consumidos por el servidor Thrift son los recursos de los dos maestros de aplicación. Para recuperar estos recursos debe detener los servicios del servidor Thrift que se estén ejecutando en el clúster.

1. En la interfaz de usuario de Ambari, en el panel izquierdo, seleccione **Spark2**.

2. En la siguiente página, seleccione **Spark2 Thrift Servers**(Servidores Thrift de Spark2).

    ![Reinicio del servidor Thrift 1](./media/apache-spark-resource-manager/ambari-ui-spark2-thrift-servers.png "Reinicio del servidor Thrift 1")

3. Aparecerán los dos nodos principales en los que se ejecuta el servidor Thrift de Spark2. Seleccione uno.

    ![Reinicio del servidor Thrift 2](./media/apache-spark-resource-manager/restart-thrift-server-2.png "Reinicio del servidor Thrift 2")

4. La siguiente página muestra todos los servicios que se ejecutan en ese nodo principal. En la lista, seleccione el botón desplegable situado junto al servidor Thrift de Spark2 y, a continuación, seleccione **Stop**(Detener).

    ![Reinicio del servidor Thrift 3](./media/apache-spark-resource-manager/ambari-ui-spark2-thriftserver-restart.png "Reinicio del servidor Thrift 3")
5. Repita también estos pasos en el otro nodo principal.

## <a name="restart-the-jupyter-service"></a>Reiniciar el servicio de Jupyter

Inicie la interfaz de usuario web de Ambari tal como se muestra al principio del artículo. En el panel de navegación izquierdo, seleccione **Jupyter**, **Service Actions** (Acciones de servicio) y **Restart All** (Reiniciar todo). Esta acción iniciará el servicio de Jupyter en todos los nodos principales.

![Reinicio de Jupyter](./media/apache-spark-resource-manager/apache-ambari-restart-jupyter.png "Reiniciar Jupyter")

## <a name="monitor-resources"></a>Supervisión de recursos

Inicie la interfaz de usuario web de Yarn tal como se muestra al principio del artículo. En la tabla de métricas del clúster, compruebe los valores de **Memory Used** (Memoria usada) y **Memory Total** (Memoria total). Si los dos valores son similares, puede que no haya recursos suficientes para iniciar la siguiente aplicación. Lo mismo se aplica a las columnas **VCores Used** (Núcleos virtuales usados) y **VCores Total** (Núcleos virtuales totales). Además, si en la vista principal hay una aplicación con el estado **ACCEPTED** (ACEPTADO) y no pasa al estado **RUNNING** (EN EJECUCIÓN) o **FAILED** (ERROR), podría ser una señal de que no hay suficientes recursos para comenzar.

![Límite de recursos](./media/apache-spark-resource-manager/apache-ambari-resource-limit.png "Límite de recursos")

## <a name="kill-running-applications"></a>Terminar las aplicaciones en ejecución

1. En la interfaz de usuario de Yarn, en el panel izquierdo, seleccione **Running** (En ejecución). En la lista de aplicaciones en ejecución, determine la aplicación que se eliminará y seleccione el **ID** (Identificador).

    ![Eliminación de aplicación 1](./media/apache-spark-resource-manager/apache-ambari-kill-app1.png "Eliminación de aplicación 1")

2. Seleccione **Kill Application** (Eliminar aplicación) en la esquina superior derecha y luego **OK** (Aceptar).

    ![Eliminación de aplicación 2](./media/apache-spark-resource-manager/apache-ambari-kill-app2.png "Eliminación de aplicación 2")

## <a name="see-also"></a>Consulte también

* [Track and debug jobs running on an Apache Spark cluster in HDInsight (Seguimiento y depuración de trabajos que se ejecutan en un clúster de Apache Spark en HDInsight)](apache-spark-job-debugging.md)

### <a name="for-data-analysts"></a>Para analistas de datos

* [Apache Spark con Machine Learning: Uso de Spark en HDInsight para analizar la temperatura de un edificio mediante datos de HVAC](apache-spark-ipython-notebook-machine-learning.md)
* [Apache Spark con Machine Learning: Uso de Spark en HDInsight para predecir los resultados de la inspección de alimentos](apache-spark-machine-learning-mllib-ipython.md)
* [Análisis de registros de un sitio web mediante Apache Spark en HDInsight](apache-spark-custom-library-website-log-analysis.md)
* [Análisis de datos de telemetría de Application Insights con Apache Spark en HDInsight](apache-spark-analyze-application-insight-logs.md)

### <a name="for-apache-spark-developers"></a>Para los desarrolladores de Apache Spark

* [Crear una aplicación independiente con Scala](apache-spark-create-standalone-application.md)
* [Ejecución de trabajos de forma remota en un clúster de Apache Spark mediante Apache Livy](apache-spark-livy-rest-interface.md)
* [Uso del complemento de herramientas de HDInsight para IntelliJ IDEA para crear y enviar aplicaciones de Spark Scala](apache-spark-intellij-tool-plugin.md)
* [Use HDInsight Tools Plugin for IntelliJ IDEA to debug Apache Spark applications remotely](apache-spark-intellij-tool-plugin-debug-jobs-remotely.md) (Uso del complemento de herramientas de HDInsight para IntelliJ IDEA para depurar aplicaciones de Apache Spark de forma remota)
* [Uso de cuadernos de Apache Zeppelin con un clúster de Apache Spark en HDInsight](apache-spark-zeppelin-notebook.md)
* [Kernels disponible para Jupyter Notebook en clústeres Apache Spark para HDInsight](apache-spark-jupyter-notebook-kernels.md)
* [Uso de paquetes externos con cuadernos de Jupyter Notebook](apache-spark-jupyter-notebook-use-external-packages.md)
* [Instalación de un cuaderno de Jupyter Notebook en el equipo y conexión al clúster de Apache Spark en HDInsight de Azure](apache-spark-jupyter-notebook-install-locally.md)

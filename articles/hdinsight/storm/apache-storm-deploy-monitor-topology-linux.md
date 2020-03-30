---
title: Implementación y administración de topologías de Apache Storm en Azure HDInsight
description: Obtenga información sobre cómo implementar, supervisar y administrar topologías de Apache Storm mediante el panel de Storm en HDInsight basado en Linux. Utilice herramientas de Hadoop para Visual Studio
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive
ms.date: 12/18/2019
ms.openlocfilehash: e890289230b3215bd102d8c5a78dca4f1b7b90f8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "79233448"
---
# <a name="deploy-and-manage-apache-storm-topologies-on-azure-hdinsight"></a>Implementación y administración de topologías de Apache Storm en Azure HDInsight

En este documento, aprenderá los aspectos básicos de administración y supervisión de las topologías de [Apache Storm](https://storm.apache.org/) que se ejecutan en clústeres de Storm en HDInsight.

## <a name="prerequisites"></a>Prerequisites

* Un clúster de Apache Storm en HDInsight. Consulte el artículo sobre la [Creación de clústeres de Apache Hadoop mediante Azure Portal](../hdinsight-hadoop-create-linux-clusters-portal.md) y seleccione **Storm** como **Tipo de clúster**.

* (Opcional) Estar familiarizado con Secure Shell (SSH) y la copia segura (SCP). Para más información, consulte [Conexión a través de SSH con HDInsight (Apache Hadoop)](../hdinsight-hadoop-linux-use-ssh-unix.md).

* (Opcional) Visual Studio, Azure SDK 2.5.1 o versiones más recientes y Herramientas de Data Lake para Visual Studio. Para obtener más información, consulte [Apache Hadoop y Herramientas de Data Lake Tools para Visual Studio](../hadoop/apache-hadoop-visual-studio-tools-get-started.md).

## <a name="submit-a-topology-using-visual-studio"></a>Envío de una topología mediante Visual Studio

Puede usar las Herramientas de Data Lake para Visual Studio para enviar topologías de C# o híbridas a su clúster de Storm. Los pasos siguientes usan una aplicación de muestra. Para obtener información sobre la creación de topologías con las Herramientas de Data Lake, vea [Topologías de Apache Storm con Visual Studio y C#](apache-storm-develop-csharp-visual-studio-topology.md).

1. Si todavía no tiene instalada la versión más reciente de las Herramientas de Data Lake para Visual Studio, consulte [Uso de Herramientas de Data Lake para Visual Studio](../hadoop/apache-hadoop-visual-studio-tools-get-started.md).

    > [!NOTE]  
    > Las herramientas de Azure Data Lake y Stream Analytics anteriormente se llamaban Herramientas de HDInsight para Visual Studio.
    >
    > Las herramientas de Azure Data Lake y Stream Analytics para Visual Studio están incluidas en la **carga de trabajo de desarrollo de Azure** para Visual Studio 2019.

1. Inicie Visual Studio.

1. En la ventana **Inicio**, seleccione **Crear un proyecto**.

1. En la ventana **Crear un nuevo proyecto**, seleccione el cuadro de búsqueda y escriba `Storm`. A continuación, elija **Storm Sample** en la lista de resultados y seleccione **Siguiente**.

1. En la ventana **Configurar el nuevo proyecto**, escriba un **Nombre de proyecto** y vaya a una **Ubicación** donde guardar el nuevo proyecto o créela. Seleccione **Crear**.

    ![Configuración de una nueva ventana de proyecto, Visual Studio](./media/apache-storm-deploy-monitor-topology-linux/apache-storm-sample1.png)

1. En el **Explorador de servidores**, haga clic con el botón derecho en **Azure**, seleccione **Conectar a la suscripción de Microsoft Azure...** y complete el proceso de inicio de sesión.

1. Desde el **Explorador de soluciones**, haga clic con el botón derecho en el proyecto y seleccione **Enviar a Storm en HDInsight**.

    > [!NOTE]  
    > Si se le solicita, introduzca las credenciales de inicio de sesión de su suscripción de Azure. Si tiene más de una suscripción, inicie sesión en la que contenga el clúster de Storm en HDInsight.

1. En el cuadro de diálogo **Enviar topología**, en la lista desplegable **Clúster de Storm**, elija el clúster de Storm en HDInsight y seleccione **Enviar**. Puede supervisar si el envío es correcto consultando el panel **Salida**.

## <a name="submit-a-topology-using-ssh-and-the-storm-command"></a>Envío de una topología con SSH y el comando Storm

1. Use el [comando SSH](../hdinsight-hadoop-linux-use-ssh-unix.md) para conectarse al clúster. Modifique el comando siguiente: reemplace CLUSTERNAME por el nombre del clúster y, luego, escriba el comando:

    ```cmd
    ssh sshuser@CLUSTERNAME-ssh.azurehdinsight.net
    ```

1. Desde la sesión SSH, use el siguiente comando para iniciar la topología de ejemplo **WordCount**:

    ```bash
    storm jar /usr/hdp/current/storm-client/contrib/storm-starter/storm-starter-topologies-*.jar org.apache.storm.starter.WordCountTopology WordCount
    ```

    Este comando inicia la topología WordCount de ejemplo en el clúster. Esta topología generará frases de forma aleatoria y contará la aparición de cada palabra en las oraciones.

    > [!NOTE]  
    > Al enviar la topología al clúster, primero debe copiar el archivo .jar que contiene el clúster antes de usar el comando `storm`. Para copiar el archivo en el clúster, puede usar el comando `scp`. Por ejemplo, escriba `scp FILENAME.jar USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:FILENAME.jar`:
    >
    > El ejemplo *WordCount* y otros ejemplos de Storm-Starter ya están incluidos en el clúster en `/usr/hdp/current/storm-client/contrib/storm-starter/`.

## <a name="submit-a-topology-programmatically"></a>Envío de una topología de manera programática

Puede implementar mediante programación una topología con el servicio Nimbus. [https://github.com/Azure-Samples/hdinsight-java-deploy-storm-topology](https://github.com/Azure-Samples/hdinsight-java-deploy-storm-topology) proporciona un ejemplo de aplicación Java que muestra cómo implementar e iniciar una topología a través del servicio Nimbus.

## <a name="monitor-and-manage-a-topology-in-visual-studio"></a>Supervisión y administración de una topología en Visual Studio

Al enviar una topología con Visual Studio, aparece la ventana **Ver topologías de Storm**. Seleccione la topología de la lista para ver información acerca de la topología de ejecución.

![Supervisión de una topología, ventana Ver topologías de Storm, Visual Studio](./media/apache-storm-deploy-monitor-topology-linux/visual-studio-monitor.png)

> [!NOTE]  
> También puede ver **topologías de Storm** desde el **Explorador de servidores**. Expanda **Azure** > **HDInsight**, haga clic con el botón derecho en un clúster de Storm en HDInsight y luego seleccione **Ver topologías de Storm**.

Seleccione la forma de los spouts o bolts para ver información sobre estos componentes. Aparece una información sobre herramientas que contiene información sobre componentes para el elemento seleccionado.

### <a name="deactivate-and-reactivate-a-topology"></a>Desactivación y reactivación de una topología

Al desactivar una topología se pone en pausa hasta que se termina o se reactiva. Para realizar estas operaciones, use los botones **Desactivar** y **Reactivar** del área **Acciones** en la parte superior de la ventana **Ver topologías de Storm**.

### <a name="rebalance-a-topology"></a>Reequilibrio de una topología

El reequilibrio de una topología permite que el sistema revise el paralelismo de la topología. Por ejemplo, si cambió el tamaño del clúster para agregar más notas, el reequilibrio permite que una topología vea los nodos nuevos.

Para reequilibrar una topología, use el botón **Reequilibrar** en el área **Acciones** de la ventana **Ver topologías de Storm**.

> [!WARNING]  
> El reequilibrio de una topología desactiva la topología, redistribuye los trabajadores de manera uniforme en el clúster y luego devuelve la topología al estado en el que se encontraba antes de que se produjera el reequilibrio. Si la topología estaba activa, se activa de nuevo. Si se desactivó la topología, seguirá desactivada.

### <a name="kill-a-running-topology"></a>Eliminar una topología de ejecución

Las topologías de Storm continúan ejecutándose hasta que se detienen o se elimina el clúster. Para detener una topología, use el botón **Terminar** en el área **Acciones**.

## <a name="monitor-and-manage-a-topology-using-ssh-and-the-storm-command"></a>Supervisión y administración de una topología con SSH y el comando Storm

La utilidad `storm` le permite trabajar con topologías en ejecución desde la línea de comandos. Use `storm -h` para obtener una lista completa de comandos.

### <a name="list-topologies"></a>Topologías de lista

Use el siguiente comando para enumerar todas las topologías en ejecución:

```shell
storm list
```

Este comando devuelve información similar al texto siguiente:

```shell
Topology_name        Status     Num_tasks  Num_workers  Uptime_secs
-------------------------------------------------------------------
WordCount            ACTIVE     29         2            263
```

### <a name="deactivate-and-reactivate-a-topology"></a>Desactivación y reactivación de una topología

Al desactivar una topología se pone en pausa hasta que se termina o se reactiva. Use el comando siguiente para desactivar o reactivar:

```shell
storm Deactivate TOPOLOGYNAME
```

```shell
storm Activate TOPOLOGYNAME
```

### <a name="kill-a-running-topology"></a>Eliminar una topología de ejecución

Las topologías de Storm, una vez iniciadas, se siguen ejecutando hasta que se detengan. Para detener una topología, use el siguiente comando:

```shell
storm kill TOPOLOGYNAME
```

### <a name="rebalance-a-topology"></a>Reequilibrio de una topología

El reequilibrio de una topología permite que el sistema revise el paralelismo de la topología. Por ejemplo, si cambió el tamaño del clúster para agregar más notas, el reequilibrio permite que una topología vea los nodos nuevos.

> [!WARNING]  
> El reequilibrio de una topología desactiva la topología, redistribuye los trabajadores de manera uniforme en el clúster y luego devuelve la topología al estado en el que se encontraba antes de que se produjera el reequilibrio. Si la topología estaba activa, se activa de nuevo. Si se desactivó, seguirá desactivada.

```shell
storm rebalance TOPOLOGYNAME
```

## <a name="monitor-and-manage-a-topology-using-the-storm-ui"></a>Supervisión y administración de una topología mediante la interfaz de usuario de Storm

La interfaz de usuario de Storm ofrece una interfaz web para trabajar con topologías en ejecución y se incluye en el clúster de HDInsight. Para ver la interfaz de usuario de Storm, use un explorador web para abrir `https://CLUSTERNAME.azurehdinsight.net/stormui`, donde *CLUSTERNAME*, es el nombre del clúster.

> [!NOTE]  
> Si se le pide que proporcione un nombre de usuario y una contraseña, escriba el nombre de usuario del administrador de clústeres y la contraseña que usó al crear el clúster.

### <a name="storm-ui-main-page"></a>Página principal de la interfaz de usuario de Storm

La página principal de la interfaz de usuario de Storm ofrece la siguiente información:

| Sección | Descripción |
| --- | --- |
| Resumen del clúster| información básica sobre el clúster de Storm. |
| Resumen de Nimbus | Una lista de información básica de Nimbus. |
| Resumen de las topologías | una lista de las topologías en ejecución. Para ver más información sobre una topología específica, seleccione su vínculo en la columna **Nombre**. |
| Resumen de supervisor | información acerca del supervisor de Storm. Para ver los recursos de trabajo asociados a un supervisor específico, seleccione su vínculo en la columna **Host** o **Id.** . |
| Configuración de Nimbus | configuración de Nimbus del clúster. |

La página principal de la interfaz de usuario de Storm es similar a esta página web:

![Página principal, interfaz de usuario de Storm, topologías de Apache Storm, Azure](./media/apache-storm-deploy-monitor-topology-linux/apache-storm-web-ui-main-page.png)

#### <a name="topology-summary"></a>Resumen de las topologías

Si selecciona un vínculo desde la sección **Resumen de la topología** , se mostrará la siguiente información sobre la topología.

| Sección | Descripción |
| --- | --- |
| Resumen de las topologías | información básica sobre la topología. |
| Acciones de topología| Acciones de administración que puede realizar para la topología. Las acciones disponibles se describen más adelante en esta sección. |
| Estadísticas de topología | estadísticas sobre la topología. Para establecer el período de tiempo para una entrada en esta sección, seleccione su vínculo en la columna **Ventana**. |
| Spouts *(período de tiempo)* | los spouts que usa la topología. Para ver más información sobre un spout específico, seleccione su vínculo en la columna **Id.** |
| Bolts *(período de tiempo)* | los bolts que usa la topología. Para ver más información sobre un bolt específico, seleccione su vínculo en la columna **Id.** |
| Recursos de trabajo | Una lista de recursos de trabajo. Para ver más información sobre un recurso de trabajo específico, seleccione su vínculo en la columna **Host**. |
| Visualización de la topología | Un botón **Show Visualization** (Mostrar visualización) muestra una visualización de la topología. |
| Configuración de la topología | la configuración de la topología seleccionada. |

La página de resumen de la topología de Storm es similar a esta página web:

![Página de resumen de la topología, interfaz de usuario de Storm, Apache Storm, Azure](./media/apache-storm-deploy-monitor-topology-linux/apache-storm-web-ui-topology-summary.png)

En la sección **Topology actions** (Acciones de topología), puede seleccionar los siguientes botones para realizar una acción:

| Botón | Descripción |
| --- | --- |
| Activar | reanuda el procesamiento de una topología desactivada. |
| Desactivación | pausa una topología en ejecución. |
| Reequilibrar | ajusta el paralelismo de la topología. Debe reequilibrar las topologías en ejecución después de haber cambiado el número de nodos del clúster. Esta operación permite que la topología ajuste el paralelismo para compensar el número de nodos adicionales o reducidos del clúster.<br/><br/>Para más información, consulte la entrada de blog <a href="https://storm.apache.org/documentation/Understanding-the-parallelism-of-a-Storm-topology.html" target="_blank">Understanding the parallelism of an Apache Storm topology</a> (Entender el paralelismo de una topología de Apache Storm).
| Terminar | finaliza una topología de Storm tras el tiempo de espera especificado. |
| Depurar | Inicia una sesión de depuración para la topología en ejecución. |
| Detener la depuración | Finaliza la sesión de depuración para la topología en ejecución. |
| Nivel de registro de cambios | Modifica el nivel de registro de depuración. |

##### <a name="spout-and-bolt-summary"></a>Resumen de spouts y bolts

Si se selecciona un spout en la sección **Spouts** o **Bolts**, se muestra la siguiente información sobre el elemento seleccionado:

| Sección | Descripción |
| --- | --- |
| Resumen de componentes | información básica acerca del spout o bolt. |
| Acciones de componente | Botones **Depurar** y **Detener depuración**. |
| Estadísticas de spout o bolt | estadísticas sobre el spout o bolt. Para establecer el período de tiempo para una entrada en esta sección, seleccione su vínculo en la columna **Ventana**. |
| (Solo bolt)<br/>Estadísticas de entrada *(período de tiempo)* | información sobre los flujos de entrada consumidos por el bolt. |
| Estadísticas de salida *(período de tiempo)* | información sobre los flujos de datos emitidos por este spout o bolt. |
| Generación de perfiles y depuración | Controles para la generación de perfiles y la depuración de los componentes en esta página. Puede establecer el valor de **Status / Timeout (Minutes)** (Estado/tiempo de espera [minutos]), y puede seleccionar botones para **JStack**, **Restart Worker** (Reiniciar trabajo) y **Montón**. |
| Ejecutores *(período de tiempo)* | información sobre las instancias del spout o bolt. Para ver un registro de información de diagnóstico generado por esta instancia, seleccione la entrada **Puerto** para un ejecutor específico. También puede ver los recursos de trabajo asociados a un ejecutor específico seleccionando su vínculo en la columna **Host**. |
| Errors | cualquier información de error de este spout o bolt. |

La página de resumen del bolt de Storm es similar a esta página web:

![Página de resumen del bolt, interfaz de usuario de Storm, Apache Storm, Azure](./media/apache-storm-deploy-monitor-topology-linux/apache-storm-web-ui-bolt-summary.png)

## <a name="monitor-and-manage-the-topology-using-the-rest-api"></a>Supervisión y administración de una topología mediante la API de REST

La interfaz de usuario de Storm se basa en la API de REST, lo que le permite realizar tareas similares de administración y supervisión mediante la API de REST. Puede usar la API de REST para crear herramientas personalizadas para administrar y supervisar las topologías de Storm.

Para más información, consulte la [API REST de la interfaz de usuario de Apache Storm](https://storm.apache.org/releases/current/STORM-UI-REST-API.html). La siguiente información es específica para usar la API de REST con Apache Storm en HDInsight.

> [!IMPORTANT]  
> La API de REST de Storm no está disponible públicamente a través de Internet. Se debe acceder mediante un túnel SSH hasta el nodo principal del clúster de HDInsight. Para más información sobre la creación y el uso de un túnel SSH, consulte [Uso de la tunelización SSH para acceder a Azure HDInsight](../hdinsight-linux-ambari-ssh-tunnel.md).

### <a name="base-uri"></a>URI base

El URI base para la API de REST en clústeres de HDInsight basados en Linux está disponible en la dirección URL `https://HEADNODEFQDN:8744/api/v1/`, donde puede sustituir *HEADNODEFQDN* por el nodo principal. El nombre de dominio del nodo principal se genera durante la creación del clúster y no es estático.

Puede encontrar el nombre de dominio completo (FQDN) del nodo principal del clúster de varias maneras:

| Método de detección de FQDN | Descripción |
| --- | --- |
| Sesión SSH | use el comando `headnode -f` desde una sesión SSH conectada al clúster. |
| Web Ambari | En la página web del clúster de Ambari (`https://CLUSTERNAME.azurehdinsight.net`), seleccione **Servicios** en la parte superior de la página y, luego, seleccione **Storm**. En la pestaña **Resumen**, seleccione **Storm UI Server** (Servidor de IU de Storm). El FQDN del nodo que hospeda la IU de Storm y la API de REST se muestra en la parte superior de la página. |
| API de REST de Ambari | use el comando `curl -u admin -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/STORM/components/STORM_UI_SERVER"` para recuperar información sobre el nodo en que se ejecutan la interfaz de usuario de Storm y la API REST. Reemplace las dos instancias de *CLUSTERNAME* por el nombre del clúster. Cuando se le solicite, escriba la contraseña de la cuenta de usuario (administrador). En la respuesta, la entrada "host_name" del JSON contiene el FQDN del nodo. |

### <a name="authentication"></a>Authentication

Las solicitudes a la API de REST deben usar la *autenticación básica*, por lo que tiene que usar el nombre y la contraseña de administrador para el clúster de HDInsight.

> [!NOTE]  
> Dado que la autenticación básica se envía mediante texto no cifrado, *siempre* debe usar HTTPS para proteger las comunicaciones con el clúster.

### <a name="return-values"></a>Valores devueltos

La información que devuelve la API de REST solo puede utilizarse desde el clúster. Por ejemplo, no se puede acceder desde Internet al nombre de dominio completo (FQDN) devuelto para servidores de [Apache Zookeeper](https://zookeeper.apache.org/).

## <a name="next-steps"></a>Pasos siguientes

Aprenda a [desarrollar topologías basadas en Java con Apache Maven](apache-storm-develop-java-topology.md).

Para una lista con más topologías de ejemplo, consulte [Topologías de ejemplo para Apache Storm en Azure HDInsight](apache-storm-example-topology.md).

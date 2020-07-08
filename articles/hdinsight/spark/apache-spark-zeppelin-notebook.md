---
title: 'Zeppelin Notebooks y clúster de Apache Spark: Azure HDInsight'
description: Instrucciones paso a paso sobre cómo usar cuadernos de Zeppelin con clústeres Apache Spark en Azure HDInsight.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: how-to
ms.custom: hdinsightactive,seoapr2020
ms.date: 04/23/2020
ms.openlocfilehash: a692f4dd86d110f7f0a91a862a7b16ac28345de5
ms.sourcegitcommit: 124f7f699b6a43314e63af0101cd788db995d1cb
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/08/2020
ms.locfileid: "86084535"
---
# <a name="use-apache-zeppelin-notebooks-with-apache-spark-cluster-on-azure-hdinsight"></a>Uso de cuadernos de Apache Zeppelin con un clúster Apache Spark en Azure HDInsight

Los clústeres de HDInsight Spark incluyen cuadernos de [Apache Zeppelin](https://zeppelin.apache.org/). Use los cuadernos para ejecutar los trabajos de Apache Spark. En este artículo, aprenderá a usar Zeppelin Notebook en un clúster de HDInsight.

## <a name="prerequisites"></a>Prerrequisitos

* Un clúster de Apache Spark en HDInsight. Para obtener instrucciones, vea [Creación de clústeres Apache Spark en HDInsight de Azure](apache-spark-jupyter-spark-sql.md).
* El esquema de URI para el almacenamiento principal de clústeres. El esquema sería `wasb://` para Azure Blob Storage, `abfs://` para Azure Data Lake Storage Gen2 o `adl://` para Azure Data Lake Storage Gen1. Si se habilita la transferencia segura para Blob Storage, el identificador URI sería `wasbs://`.  Para más información, consulte [Requerir transferencia segura en Azure Storage](../../storage/common/storage-require-secure-transfer.md).

## <a name="launch-an-apache-zeppelin-notebook"></a>Inicio de un cuaderno de Apache Zeppelin

1. Desde la **Introducción** el clúster de Spark, seleccione el **cuaderno de Zeppelin** en los **paneles de clúster**. Escriba las credenciales de administrador del clúster.  

   > [!NOTE]  
   > También puede comunicarse con su equipo portátil ligero Zeppelin en el clúster si abre la siguiente dirección URL en el explorador. Reemplace **CLUSTERNAME** por el nombre del clúster:
   >
   > `https://CLUSTERNAME.azurehdinsight.net/zeppelin`

2. Cree un nuevo notebook. En el panel del encabezado, vaya a **Notebook** > **Create new note** (Cuaderno > Crear nueva nota).

    ![Creación de un nuevo cuaderno de Zeppelin](./media/apache-spark-zeppelin-notebook/hdinsight-create-zeppelin-notebook.png "Crear un nuevo cuaderno de Zeppelin")

    Especifique un nombre para el cuaderno y haga clic en **Create Note** (Crear nota).

3. Por otro lado, asegúrese de que en el encabezado del cuaderno aparece el estado conectado. Esto se indica mediante un punto verde que se encuentra en la esquina superior derecha.

    ![Estado del cuaderno de Zeppelin](./media/apache-spark-zeppelin-notebook/hdinsight-zeppelin-connected.png "Estado del cuaderno de Zeppelin")

4. Cargue los datos de ejemplo en una tabla temporal. Cuando cree un clúster de Spark en HDInsight, el archivo de datos de ejemplo `hvac.csv` se copia en la cuenta de almacenamiento asociada en `\HdiSamples\SensorSampleData\hvac`.

    En el párrafo vacío que se crea de manera predeterminada en el nuevo cuaderno, pegue el siguiente fragmento.

    ```scala
    %livy2.spark
    //The above magic instructs Zeppelin to use the Livy Scala interpreter

    // Create an RDD using the default Spark context, sc
    val hvacText = sc.textFile("wasbs:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")

    // Define a schema
    case class Hvac(date: String, time: String, targettemp: Integer, actualtemp: Integer, buildingID: String)

    // Map the values in the .csv file to the schema
    val hvac = hvacText.map(s => s.split(",")).filter(s => s(0) != "Date").map(
        s => Hvac(s(0),
                s(1),
                s(2).toInt,
                s(3).toInt,
                s(6)
        )
    ).toDF()

    // Register as a temporary table called "hvac"
    hvac.registerTempTable("hvac")
    ```

    Presione **MAYÚS+ENTRAR** o seleccione el botón **Reproducir** del párrafo para ejecutar el fragmento de código. El estado en la esquina derecha del párrafo debería avanzar de READY (Listo), PENDING (Pendiente) o RUNNING (En ejecución) a FINISHED (Finalizado). El resultado se muestra en la parte inferior del mismo párrafo. La captura de pantalla es similar a esta imagen:

    ![Creación de una tabla temporal de datos sin procesar](./media/apache-spark-zeppelin-notebook/hdinsight-zeppelin-load-data.png "Crear una tabla temporal de datos sin procesar")

    También puede proporcionar un título para cada párrafo. Desde la esquina derecha del párrafo, seleccione el ícono **Settings** (Configuración) con forma de rueda dentada y seleccione **Show title** (Mostrar título).  

    > [!NOTE]  
    > El intérprete %spark2 no se admite en los cuadernos de Zeppelin en todas las versiones de HDInsight y el intérprete %sh no se admite en HDInsight 4.0 y versiones posteriores.

5. Ahora puede ejecutar instrucciones de Spark SQL en la tabla `hvac`. Pegue la siguiente consulta en un nuevo párrafo. La consulta recupera el identificador del edificio. También la diferencia entre la temperatura objetivo y la real para cada edificio en una fecha determinada. Presione **MAYÚS + ENTRAR**.

    ```sql
    %sql
    select buildingID, (targettemp - actualtemp) as temp_diff, date from hvac where date = "6/1/13"
    ```  

    La instrucción **%sql** del principio le indica al cuaderno que utilice el intérprete de Livy Scala.

6. Seleccione el icono del **gráfico de barras** para cambiar la visualización.  La **configuración**, que aparece después de haber seleccionado el **gráfico de barras**, le permite elegir las **claves** y los **valores**.  En la captura de pantalla siguiente se muestra el resultado.

    ![Ejecución de una instrucción Spark SQL mediante el cuaderno 1](./media/apache-spark-zeppelin-notebook/hdinsight-zeppelin-spark-query-1.png "Ejecución de una instrucción Spark SQL mediante el cuaderno 1")

7. También puede ejecutar instrucciones Spark SQL usando variables en la consulta. El siguiente fragmento de código muestra cómo definir una variable `Temp` en la consulta con los valores posibles con los que quiere hacer la consulta. Cuando ejecuta la consulta por primera vez, se rellena una lista desplegable automáticamente con los valores especificados para la variable.

    ```sql
    %sql  
    select buildingID, date, targettemp, (targettemp - actualtemp) as temp_diff from hvac where targettemp > "${Temp = 65,65|75|85}"
    ```

    Pegue este fragmento de código en un nuevo párrafo y presione **MAYÚS + ENTRAR**. A continuación, seleccione **65** en la lista desplegable **Temp**.

8. Seleccione el icono del **gráfico de barras** para cambiar la visualización.  Seguidamente, seleccione **configuración** y realice los siguientes cambios:

   * **Grupos:**  Agregue **targettemp**.  
   * **Valores:** 1. Quite la **fecha**.  2. Agregue **temp_diff**.  3.  Cambie el agregador de **SUM** a **AVG**.  

     En la captura de pantalla siguiente se muestra el resultado.

     ![Ejecución de una instrucción Spark SQL mediante el cuaderno 2](./media/apache-spark-zeppelin-notebook/hdinsight-zeppelin-spark-query-2.png "Ejecución de una instrucción Spark SQL mediante el cuaderno 2")

## <a name="how-do-i-use-external-packages-with-the-notebook"></a>Uso de paquetes externos con el cuaderno

El cuaderno de Zeppelin Notebook en un clúster Apache Spark de HDInsight puede usar paquetes externos que haya aportado la comunidad y que no estén incluidos en el clúster. Busque el [repositorio de Maven](https://search.maven.org/) para obtener una lista completa de los paquetes que están disponibles. También puede obtener una lista de paquetes disponibles de otras fuentes. Por ejemplo, dispone de la lista completa de los paquetes externos aportados por la comunidad en [Spark Packages](https://spark-packages.org/)(Paquetes Spark).

En este artículo, aprenderá a utilizar el paquete [spark csv](https://search.maven.org/#artifactdetails%7Ccom.databricks%7Cspark-csv_2.10%7C1.4.0%7Cjar) con el cuaderno de Jupyter Notebook.

1. Abra la configuración del intérprete. Desde la esquina superior derecha, seleccione el nombre de usuario registrado y, a continuación, seleccione **Interpreter** (Intérprete).

    ![Inicio del intérprete](./media/apache-spark-zeppelin-notebook/zeppelin-launch-interpreter.png "Salida de Hive")

2. Desplácese hasta **livy2** y seleccione la opción de **editar**.

    ![Modificación de la configuración del intérprete 1](./media/apache-spark-zeppelin-notebook/zeppelin-use-external-package-1.png "Modificación de la configuración del intérprete 1")

3. Vaya a la clave `livy.spark.jars.packages` y establezca su valor en el formato `group:id:version`. Por ejemplo, si desea usar el paquete [spark-csv](https://search.maven.org/#artifactdetails%7Ccom.databricks%7Cspark-csv_2.10%7C1.4.0%7Cjar), debe establecer el valor de la clave en `com.databricks:spark-csv_2.10:1.4.0`.

    ![Modificación de la configuración del intérprete 2](./media/apache-spark-zeppelin-notebook/zeppelin-use-external-package-2.png "Modificación de la configuración del intérprete 2")

    Seleccione **Guardar** y luego **Aceptar** para reiniciar el intérprete de Livy.

4. Si desea saber cómo acceder al valor de la clave especificada anteriormente, siga estos pasos.

    a. Busque el paquete en el repositorio de Maven. En este artículo, hemos utilizado [spark-csv](https://search.maven.org/#artifactdetails%7Ccom.databricks%7Cspark-csv_2.10%7C1.4.0%7Cjar).

    b. En el repositorio, recopile los valores de **GroupId**, **ArtifactId** y **Version**.

    ![Uso de paquetes externos con cuadernos de Jupyter Notebook](./media/apache-spark-zeppelin-notebook/use-external-packages-with-jupyter.png "Uso de paquetes externos con cuadernos de Jupyter Notebook")

    c. Concatene los tres valores separados por dos puntos ( **:** ).

    ```
    com.databricks:spark-csv_2.10:1.4.0
    ```

## <a name="where-are-the-zeppelin-notebooks-saved"></a>¿Dónde se guardan los cuadernos de Zeppelin Notebook?

Los cuadernos de Zeppelin Notebook se guardan en los nodos principales del clúster. Por tanto, si se elimina el clúster, también se eliminarán los cuadernos. Si desea guardar los cuadernos para utilizarlos más adelante en otros clústeres, debe exportarlos cuando haya terminado de ejecutar los trabajos. Para exportar un cuaderno, haga clic en el icono **Export** (Exportar), tal y como se muestra en la imagen siguiente.

![Descarga del cuaderno](./media/apache-spark-zeppelin-notebook/zeppelin-download-notebook.png "Descarga del cuaderno")

De este modo, el cuaderno se guarda como un archivo JSON en la ubicación de descarga.

## <a name="use-shiro-to-configure-access-to-zeppelin-interpreters-in-enterprise-security-package-esp-clusters"></a>Uso de `Shiro` para configurar el acceso a los intérpretes de Zeppelin en clústeres de Enterprise Security Package (ESP)

Como se indicó anteriormente, el intérprete `%sh` no se admite desde HDInsight 4.0 en adelante. Además, dado que el intérprete `%sh` introduce posibles problemas de seguridad, como el acceso a keytabs mediante comandos de shell, se ha quitado también de los clústeres de ESP de HDInsight 3.6. Es decir, el intérprete `%sh` no está disponible cuando se hace clic en **Crear nueva nota** ni en la interfaz de usuario del intérprete de forma predeterminada.

Los usuarios de dominio con privilegios pueden usar el archivo `Shiro.ini` para controlar el acceso a la interfaz de usuario del intérprete. Solo estos usuarios pueden crear nuevos intérpretes `%sh` y establecer permisos en cada nuevo intérprete `%sh`. Para controlar el acceso mediante el archivo `shiro.ini`, siga estos pasos:

1. Defina un nuevo rol con un nombre de grupo de dominio existente. En el ejemplo siguiente, `adminGroupName` es un grupo de usuarios con privilegios de AAD. No use caracteres especiales ni espacios en blanco en el nombre del grupo. Los caracteres después de `=` conceden los permisos para este rol. `*` significa que el grupo tiene permisos completos.

    ```
    [roles]
    adminGroupName = *
    ```

2. Agregue el nuevo rol para el acceso a los intérpretes de Zeppelin. En el ejemplo siguiente, a todos los usuarios de `adminGroupName` se les concede acceso a los intérpretes de Zeppelin y pueden crear intérpretes nuevos. Puede colocar varios roles entre corchetes en `roles[]`, separados por comas. A continuación, los usuarios que tienen los permisos necesarios pueden acceder a los intérpretes de Zeppelin.

    ```
    [urls]
    /api/interpreter/** = authc, roles[adminGroupName]
    ```

## <a name="livy-session-management"></a>Administración de sesiones de Livy

El primer párrafo de código del cuaderno de Zeppelin crea una nueva sesión de Livy en el clúster. Esta sesión se compartirá con todos los cuadernos de Zeppelin Notebook que cree en el futuro. Si la sesión Livy se elimina por cualquier motivo, los trabajos no se ejecutarán desde el cuaderno de Zeppelin Notebook.

En este caso, debe seguir los pasos que se indican a continuación para poder ejecutar trabajos desde un cuaderno de Zeppelin Notebook.  

1. Reinicie el intérprete de Livy desde el cuaderno de Zeppelin Notebook. Para ello, abra la configuración del intérprete haciendo clic en el nombre del usuario conectado que encontrará en la esquina superior derecha y después en **Interpreter** (Intérprete).

    ![Inicio del intérprete](./media/apache-spark-zeppelin-notebook/zeppelin-launch-interpreter.png "Salida de Hive")

2. Desplácese hasta **livy2** y seleccione la opción para **reiniciar**.

    ![Reinicio del intérprete de Livy](./media/apache-spark-zeppelin-notebook/hdinsight-zeppelin-restart-interpreter.png "Reinicio del intérprete Zeppelin")

3. Ejecute una celda de código desde el cuaderno de Zeppelin Notebook existente. Este código creará una nueva sesión de Livy en el clúster de HDInsight.

## <a name="general-information"></a>Información general

### <a name="validate-service"></a>Validar servicio

Para validar el servicio desde Ambari, navegue hasta `https://CLUSTERNAME.azurehdinsight.net/#/main/services/ZEPPELIN/summary`, donde CLUSTERNAME es el nombre del clúster.

Para validar el servicio desde una línea de comandos, use SSH en el nodo principal. Cambie el usuario a Zeppelin con el comando `sudo su zeppelin`. Comandos de estado:

|Get-Help |Descripción |
|---|---|
|`/usr/hdp/current/zeppelin-server/bin/zeppelin-daemon.sh status`|Estado del servicio.|
|`/usr/hdp/current/zeppelin-server/bin/zeppelin-daemon.sh --version`|Versión del servicio.|
|`ps -aux | grep zeppelin`|Identificar PID.|

### <a name="log-locations"></a>Ubicaciones de registro

|Servicio |Path |
|---|---|
|zeppelin-server|/usr/hdp/current/zeppelin-server/|
|Registros del servidor|/var/log/zeppelin|
|Intérprete de configuración, `Shiro`, site.xml, log4j|/usr/hdp/current/zeppelin-server/conf o /etc/zeppelin/conf|
|Directorio de PID|/var/run/zeppelin|

### <a name="enable-debug-logging"></a>Habilitación del registro de depuración

1. Vaya a `https://CLUSTERNAME.azurehdinsight.net/#/main/services/ZEPPELIN/summary`, donde CLUSTERNAME es el nombre del clúster.

1. Navegue hasta **CONFIGs** > **Advanced zeppelin-log4j-properties** > **log4j_properties_content**.

1. Cambie `log4j.appender.dailyfile.Threshold = INFO` por `log4j.appender.dailyfile.Threshold = DEBUG`.

1. Agregue `log4j.logger.org.apache.zeppelin.realm=DEBUG`.

1. Guarde los cambios y reinicie el servicio.

## <a name="next-steps"></a>Pasos siguientes

* [Información general: Apache Spark en Azure HDInsight](apache-spark-overview.md)
* [Kernels disponible para Jupyter Notebook en clústeres Apache Spark para HDInsight](apache-spark-jupyter-notebook-kernels.md)
* [Instalación de un cuaderno de Jupyter Notebook en el equipo y conexión al clúster de Apache Spark en HDInsight de Azure](apache-spark-jupyter-notebook-install-locally.md)

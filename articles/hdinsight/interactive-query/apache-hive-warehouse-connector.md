---
title: 'Apache Spark y Apache Hive, conector de Hive Warehouse : Azure HDInsight'
description: Aprenda a integrar Apache Spark y Apache Hive con el conector de Hive Warehouse en Azure HDInsight.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: hrasheed
ms.service: hdinsight
ms.topic: conceptual
ms.custom: seoapr2020
ms.date: 04/28/2020
ms.openlocfilehash: 77623a89e52a5e15fbb4159ff49d9377e53e7d4c
ms.sourcegitcommit: eaec2e7482fc05f0cac8597665bfceb94f7e390f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/29/2020
ms.locfileid: "82509540"
---
# <a name="integrate-apache-spark-and-apache-hive-with-the-hive-warehouse-connector"></a>Integración de Apache Spark y Apache Hive con el conector de Hive Warehouse

Hive Warehouse Connector (HWC) de Apache es una biblioteca que le permite trabajar más fácilmente con Apache Spark y Apache Hive. Es más fácil, ya que admite tareas como mover datos entre DataFrames de Spark y tablas de Hive. Y también dirigir datos de streaming de Spark a tablas de Hive. El conector de Hive Warehouse funciona como un puente entre Spark y Hive. Admite Scala, Java y Python para el desarrollo.

Hive Warehouse Connector permite aprovechar las características únicas de Hive y Spark. Se usan para compilar aplicaciones de Big Data eficaces. Apache Hive ofrece compatibilidad con las transacciones de base de datos de atomicidad, coherencia, aislamiento, durabilidad (ACID). Para obtener más información sobre ACID y las transacciones en Hive, consulte [Hive Transactions](https://cwiki.apache.org/confluence/display/Hive/Hive+Transactions) (Transacciones de Hive). Hive también ofrece controles de seguridad detallados a través de Apache Ranger y el procesamiento analítico de baja latencia que no está disponible en Apache Spark.

Apache Spark tiene una API de streaming estructurada que proporciona funcionalidades de streaming que no están disponibles en Apache Hive. A partir de HDInsight 4.0, Apache Spark 2.3.1 y Apache Hive 3.1.0 tienen tiendas de metadatos independientes. Estas tiendas de metadatos independientes pueden dificultar la interoperabilidad. El conector de Hive Warehouse simplifica el uso conjunto de Spark y Hive. La biblioteca HWC carga los datos de los demonios de LLAP (Procesamiento analítico de baja latencia) en los ejecutores de Spark en paralelo. De este modo, es más eficaz y adaptable que usar una conexión JDBC estándar de Spark a Hive.

![arquitectura del conector de Hive Warehouse](./media/apache-hive-warehouse-connector/hive-warehouse-connector-architecture.png)

Algunas de las operaciones compatibles con el conector de Hive Warehouse son:

* Descripción de una tabla
* Creación de una tabla de datos en formato ORC
* Selección de datos de Hive y recuperación de un elemento DataFrame
* Escritura de un elemento DataFrame en Hive en un lote
* Ejecución de una instrucción de actualización de Hive
* Lectura de datos de la tabla de Hive, transformación en Spark y escritura en una nueva tabla de Hive
* Escritura de un flujo de Spark o DataFrame en Hive mediante Hive Streaming

## <a name="hive-warehouse-connector-setup"></a>Configuración del conector de Hive Warehouse

Siga estos pasos para configurar el conector de Hive Warehouse entre un clúster de Spark e Interactive Query en Azure HDInsight:

### <a name="create-clusters"></a>Creación de clústeres

1. Cree un clúster de HDInsight Spark **4.0** con una cuenta de almacenamiento y una red virtual de Azure personalizada. Para obtener información sobre cómo crear un clúster en una red virtual de Azure, consulte [Add HDInsight to an existing virtual network](../../hdinsight/hdinsight-plan-virtual-network-deployment.md#existingvnet) (Adición de HDInsight a una red virtual existente).

1. Cree un clúster de Interactive Query de HDInsight (LLAP) **4.0** con la misma cuenta de almacenamiento y la red virtual de Azure que el clúster de Spark.

### <a name="modify-hosts-file"></a>Modificación del archivo de hosts

Copie la información del nodo del archivo `/etc/hosts` en headnode0 de su clúster de Interactive Query y concatene la información al archivo `/etc/hosts` de headnode0 de su clúster de Spark. Este paso permitirá al clúster de Spark resolver las direcciones IP de los nodos del clúster de Interactive Query. Vea el contenido del archivo actualizado con `cat /etc/hosts`. La salida final debe tener un aspecto similar a lo que se muestra en la captura de pantalla siguiente.

![archivo de hosts del conector de Hive Warehouse](./media/apache-hive-warehouse-connector/hive-warehouse-connector-hosts-file.png)

### <a name="gather-preliminary-information"></a>Recopilación de la información preliminar

#### <a name="from-your-interactive-query-cluster"></a>Desde el clúster de Interactive Query

1. Vaya a la página de Apache Ambari Hive del clúster con `https://LLAPCLUSTERNAME.azurehdinsight.net/#/main/services/HIVE/configs`, donde `LLAPCLUSTERNAME` es el nombre del clúster de Interactive Query.

1. Vaya a **Advanced** > **General** > **hive.metastore.uris** y anote el valor. El valor puede ser similar a `thrift://iqgiro.rekufuk2y2cezcbowjkbwfnyvd.bx.internal.cloudapp.net:9083,thrift://hn1-iqgiro.rekufuk2y2cezcbowjkbwfnyvd.bx.internal.cloudapp.net:9083`.

1. Vaya a **Advanced** > **Advanced Hive-site** > **hive.zookeeper.quorum** y anote el valor. El valor puede ser similar a `zk0-iqgiro.rekufuk2y2cezcbowjkbwfnyvd.bx.internal.cloudapp.net:2181,zk1-iqgiro.rekufuk2y2cezcbowjkbwfnyvd.bx.internal.cloudapp.net:2181,zk4-iqgiro.rekufuk2y2cezcbowjkbwfnyvd.bx.internal.cloudapp.net:2181`.

#### <a name="from-your-apache-spark-cluster"></a>Desde el clúster de Apache Spark

1. Vaya a la página de Apache Ambari Hive del clúster con `https://SPARKCLUSTERNAME.azurehdinsight.net/#/main/services/HIVE/configs`, donde `SPARKCLUSTERNAME` es el nombre del clúster de Apache Spark.

1. Vaya a **Advanced** > **Advanced hive-interactive-site** > **hive.llap.daemon.service.hosts** y anote el valor. El valor puede ser similar a `@llap0`.

### <a name="configure-spark-cluster-settings"></a>Configuración de un clúster de Spark

Desde la interfaz de usuario web de Spark Ambari, vaya a **Spark2** > **CONFIGS** > **Custom spark2-defaults**.

![Configuración de Apache Ambari Spark2](./media/apache-hive-warehouse-connector/hive-warehouse-connector-spark2-ambari.png)

Seleccione **Add Property...** (Agregar propiedad) según sea necesario para agregar o actualizar el valor siguiente:

| Clave | Value |
|----|----|
|`spark.hadoop.hive.llap.daemon.service.hosts`|El valor obtenido anteriormente de **hive.llap.daemon.service.hosts**.|
|`spark.sql.hive.hiveserver2.jdbc.url`|`jdbc:hive2://LLAPCLUSTERNAME.azurehdinsight.net:443/;user=admin;password=PWD;ssl=true;transportMode=http;httpPath=/hive2`. Establézcalo en la cadena de conexión JDBC, que se conecta a Hiveserver2 en el clúster de Interactive Query. REEMPLACE `LLAPCLUSTERNAME` por el nombre del clúster de Interactive Query. Reemplace `PWD` por la contraseña real.|
|`spark.datasource.hive.warehouse.load.staging.dir`|`wasbs://STORAGE_CONTAINER_NAME@STORAGE_ACCOUNT_NAME.blob.core.windows.net/tmp`. Establézcalo en un directorio de almacenamiento provisional adecuado y compatible con HDFS. Si tiene dos clústeres diferentes, el directorio de almacenamiento provisional debe ser una carpeta del directorio de almacenamiento provisional de la cuenta de almacenamiento del clúster de LLAP para que HiveServer2 tenga acceso a ella.  Reemplace `STORAGE_ACCOUNT_NAME` por el nombre de la cuenta de almacenamiento que usa el clúster y `STORAGE_CONTAINER_NAME` es el nombre del contenedor de almacenamiento.|
|`spark.datasource.hive.warehouse.metastoreUri`|El valor obtenido anteriormente de **hive.metastore.uris**.|
|`spark.security.credentials.hiveserver2.enabled`|`false` para el modo de implementación del cliente YARN.|
|`spark.hadoop.hive.zookeeper.quorum`|El valor obtenido anteriormente de **hive.zookeeper.quorum**.|

Guarde los cambios y reinicie los componentes según sea necesario.

## <a name="using-the-hive-warehouse-connector"></a>Uso del conector de Hive Warehouse

### <a name="connecting-and-running-queries"></a>Conexión y ejecución de consultas

Puede elegir entre varios métodos diferentes para conectarse a su clúster de Interactive Query y ejecutar consultas mediante el conector de Hive Warehouse. Entre los métodos admitidos se incluyen las herramientas siguientes:

* [spark-shell](../spark/apache-spark-shell.md)
* PySpark
* spark-submit
* [Zeppelin](../spark/apache-spark-zeppelin-notebook.md)
* [Livy](../spark/apache-spark-livy-rest-interface.md)

Todos los ejemplos proporcionados en este artículo se ejecutarán a través de spark-shell.

Para iniciar una sesión de spark-shell, realice los pasos siguientes:

1. SSH en el nodo principal del clúster de Apache Spark. Para obtener información acerca de cómo conectarse al clúster con SSH, consulte [Conexión a HDInsight (Apache Hadoop) mediante SSH](../../hdinsight/hdinsight-hadoop-linux-use-ssh-unix.md).

1. Ejecute el siguiente comando para iniciar el shell de Spark:

    ```bash
    spark-shell --master yarn \
    --jars /usr/hdp/current/hive_warehouse_connector/hive-warehouse-connector-assembly-<STACK_VERSION>.jar \
    --conf spark.security.credentials.hiveserver2.enabled=false
    ```

    Verá un mensaje de bienvenida y un aviso `scala>` donde puede escribir comandos.

1. Después de iniciar spark-shell, se puede iniciar una instancia del conector de Hive Warehouse mediante los siguientes comandos:

    ```scala
    import com.hortonworks.hwc.HiveWarehouseSession
    val hive = HiveWarehouseSession.session(spark).build()
    ```

### <a name="connecting-and-running-queries-on-enterprise-security-package-esp-clusters"></a>Conexión y ejecución de consultas en los clústeres de Enterprise Security Package (ESP)

Enterprise Security Package (ESP) proporciona funcionalidades de nivel empresarial, como la autenticación basada en Active Directory. Y la compatibilidad con varios usuarios y el control de acceso basado en roles para clústeres de Apache Hadoop en Azure HDInsight. Para más información, consulte [Uso de Enterprise Security Package en HDInsight](../domain-joined/apache-domain-joined-architecture.md).

1. SSH en el nodo principal del clúster de Apache Spark.

1. Escriba `kinit` e inicie sesión con un usuario de dominio.

1. Inicie spark-shell con la lista completa de parámetros de configuración como se muestra a continuación. Todos los valores en letras mayúsculas que se encuentran entre corchetes angulares deben especificarse en función de su clúster. Si necesita averiguar los valores de entrada para alguno de los parámetros siguientes, vea la sección [Configuración de Hive Warehouse Connector](#hive-warehouse-connector-setup).

    ```bash
    spark-shell --master yarn \
    --jars /usr/hdp/current/hive_warehouse_connector/hive-warehouse-connector-assembly-<STACK_VERSION>.jar \
    --conf spark.security.credentials.hiveserver2.enabled=false
    --conf spark.hadoop.hive.llap.daemon.service.hosts='<LLAP_APP_NAME>'
    --conf spark.sql.hive.hiveserver2.jdbc.url='jdbc:hive2://<ZOOKEEPER_QUORUM>;serviceDiscoveryMode=zookeeper;zookeeperNamespace=hiveserver2-interactive'
    --conf spark.datasource.hive.warehouse.load.staging.dir='<STAGING_DIR>'
    --conf spark.datasource.hive.warehouse.metastoreUri='<METASTORE_URI>'
    --conf spark.hadoop.hive.zookeeper.quorum='<ZOOKEEPER_QUORUM>'
   ```

### <a name="creating-spark-dataframes-from-hive-queries"></a>Creación de DataFrames de Spark a partir de consultas de Hive

Los resultados de todas las consultas mediante la biblioteca HWC se devuelven como un elemento DataFrame. En los ejemplos siguientes se muestra cómo crear una consulta básica.

```scala
hive.setDatabase("default")
val df = hive.executeQuery("select * from hivesampletable")
df.filter("state = 'Colorado'").show()
```

Los resultados de la consulta son DataFrames de Spark, que se pueden usar con bibliotecas de Spark como MLIB y SparkSQL.

### <a name="writing-out-spark-dataframes-to-hive-tables"></a>Escritura de DataFrames de Spark en tablas de Hive

Spark no admite la escritura en tablas ACID administradas de Hive de forma nativa. Con HWC, sin embargo, puede escribir cualquier elemento DataFrame en una tabla de Hive. Puede ver como funciona esta funcionalidad en el ejemplo siguiente:

1. Cree una tabla denominada `sampletable_colorado` y especifique sus columnas con el comando siguiente:

    ```scala
    hive.createTable("sampletable_colorado").column("clientid","string").column("querytime","string").column("market","string").column("deviceplatform","string").column("devicemake","string").column("devicemodel","string").column("state","string").column("country","string").column("querydwelltime","double").column("sessionid","bigint").column("sessionpagevieworder","bigint").create()
    ```

1. Filtre la tabla `hivesampletable` en que la columna `state` es igual a `Colorado`. Esta consulta de la tabla de Hive se devuelve como un elemento DataFrame de Spark. A continuación, el elemento DataFrame se guarda en la tabla de Hive `sampletable_colorado` utilizando la función `write`.

    ```scala
    hive.table("hivesampletable").filter("state = 'Colorado'").write.format(HiveWarehouseSession.HIVE_WAREHOUSE_CONNECTOR).option("table","sampletable_colorado").save()
    ```

1. Vea los resultados con el siguiente comando:

    ```scala
    hive.table("sampletable_colorado").show()
    ```

    ![visualización de la tabla de Hive del conector de Hive Warehouse](./media/apache-hive-warehouse-connector/hive-warehouse-connector-show-hive-table.png)

### <a name="structured-streaming-writes"></a>Escrituras de flujos estructurados

Al usar el conector de Hive Warehouse, puede emplear los flujos de Spark para escribir datos en las tablas de Hive.

Siga los pasos que se indican a continuación para crear una instancia de Hive Warehouse Connector. En el ejemplo se ingieren datos de una secuencia de Spark en el puerto de localhost 9999 en una tabla de Hive.

1. Siga los pasos iniciales de [Conexión y ejecución de consultas](#connecting-and-running-queries).

1. Inicie el flujo de Spark con el siguiente comando:

    ```scala
    val lines = spark.readStream.format("socket").option("host", "localhost").option("port",9999).load()
    ```

1. Genere datos para el flujo de Spark que creó. Para ello, siga estos pasos:
    1. Abra una segunda sesión de SSH en el mismo clúster de Spark.
    1. En el símbolo del sistema, escriba `nc -lk 9999`. Este comando usa la utilidad `netcat` para enviar datos desde la línea de comandos al puerto especificado.

1. Vuelva a la primera sesión de SSH y cree una nueva tabla de Hive para contener los datos de streaming. En spark-shell, escriba el siguiente comando:

    ```scala
    hive.createTable("stream_table").column("value","string").create()
    ```

1. Después, escriba los datos de streaming en la tabla recién creada mediante el siguiente comando:

    ```scala
    lines.filter("value = 'HiveSpark'").writeStream.format(HiveWarehouseSession.STREAM_TO_STREAM).option("database", "default").option("table","stream_table").option("metastoreUri",spark.conf.get("spark.datasource.hive.warehouse.metastoreUri")).option("checkpointLocation","/tmp/checkpoint1").start()
    ```

    >[!Important]
    > Actualmente, las opciones `metastoreUri` y `database` deben establecerse de forma manual debido a un problema conocido en Apache Spark. Para obtener más información acerca de este problema, consulte  [SPARK-25460](https://issues.apache.org/jira/browse/SPARK-25460).

1. Vuelva a la segunda sesión de SSH y escriba los valores siguientes:

    ```bash
    foo
    HiveSpark
    bar
    ```

1. Vuelva a la primera sesión de SSH y observe la breve actividad. Use el siguiente comando para ver los datos:

    ```scala
    hive.table("stream_table").show()
    ```

Use **Ctrl + C** para detener `netcat` en la segunda sesión de SSH. Use `:q` para salir de spark-shell en la primera sesión de SSH.

### <a name="securing-data-on-spark-esp-clusters"></a>Protección de datos en los clústeres ESP de Spark

1. Cree una tabla `demo` con algunos datos de ejemplo mediante los comandos siguientes:

    ```scala
    create table demo (name string);
    INSERT INTO demo VALUES ('HDinsight');
    INSERT INTO demo VALUES ('Microsoft');
    INSERT INTO demo VALUES ('InteractiveQuery');
    ```

1. Ver el contenido de la tabla con el siguiente comando. Antes de aplicar la directiva, en la tabla `demo` se muestra la columna completa.

    ```scala
    hive.executeQuery("SELECT * FROM demo").show()
    ```

    ![tabla de demostración antes de aplicar la directiva de Ranger](./media/apache-hive-warehouse-connector/hive-warehouse-connector-table-before-ranger-policy.png)

1. Aplique una directiva de enmascaramiento de columnas que solo muestre los últimos cuatro caracteres de la columna.  
    1. Abra la interfaz de usuario de administrador de Ranger en `https://CLUSTERNAME.azurehdinsight.net/ranger/`.
    1. Haga clic en el servicio de Hive de su clúster en **Hive**.
        ![ranger service manager](./media/apache-hive-warehouse-connector/hive-warehouse-connector-ranger-service-manager.png)
    1. Haga clic en la pestaña **Enmascaramiento** y, a continuación, en **Agregar nueva directiva**.

        ![lista de directivas de Hive/Ranger del conector de Hive Warehouse](./media/apache-hive-warehouse-connector/hive-warehouse-connector-ranger-hive-policy-list.png)

    a. Proporcione un nombre de directiva. seleccione las opciones siguientes. Para la base de datos: **valor predeterminado**; tabla de Hive: **demostración**; columna de Hive: **nombre**; usuario: **rsadmin2**; tipos de acceso: **select** (determinados) y, en el menú **Select Masking Option** (Seleccionar opción de enmascaramiento), **Partial mask: show last 4** (Máscara parcial: mostrar últimos 4). Haga clic en **Agregar**.
                ![crear directiva](./media/apache-hive-warehouse-connector/hive-warehouse-connector-ranger-create-policy.png)
1. Vuelva a consultar el contenido de la tabla. Después de aplicar la directiva de Ranger, solo podemos ver los cuatro últimos caracteres de la columna.

    ![tabla de demostración antes de aplicar la directiva de Ranger](./media/apache-hive-warehouse-connector/hive-warehouse-connector-table-after-ranger-policy.png)

## <a name="next-steps"></a>Pasos siguientes

* [Uso de Interactive Query con HDInsight](./apache-interactive-query-get-started.md)
* [Ejemplos de la interacción con el conector de Hive Warehouse mediante Zeppelin, Livy, spark-submit y pyspark](https://community.hortonworks.com/articles/223626/integrating-apache-hive-with-apache-spark-hive-war.html)

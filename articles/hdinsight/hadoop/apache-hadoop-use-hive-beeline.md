---
title: Uso de Apache Beeline con Apache Hive en Azure HDInsight
description: Aprenda a usar el cliente de Beeline para ejecutar consultas de Hive con Hadoop en HDInsight. Beeline es una utilidad para trabajar con HiveServer2 sobre JDBC.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: seoapr2020
ms.date: 04/17/2020
ms.openlocfilehash: 2396207c88716420d299382006a270eb747ddc03
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/28/2020
ms.locfileid: "82192670"
---
# <a name="use-the-apache-beeline-client-with-apache-hive"></a>Usar el cliente de Apache Beeline con Apache Hive

Aprenda a usar [Apache Beeline](https://cwiki.apache.org/confluence/display/Hive/HiveServer2+Clients#HiveServer2Clients-Beeline–NewCommandLineShell) para ejecutar consultas de Apache Hive en HDInsight.

Beeline es un cliente de Hive que se incluye en los nodos principales del clúster de HDInsight. Para instalar Beeline localmente, consulte a continuación [Instalación de cliente de Beeline](#install-beeline-client). Beeline usa JDBC para conectarse a HiveServer2, un servicio hospedado en el clúster de HDInsight. Beeline también se puede usar para tener acceso a Hive en HDInsight de forma remota a través de Internet. En los ejemplos siguientes se proporcionan las cadenas de conexión más habituales usadas para conectarse a HDInsight desde Beeline.

## <a name="types-of-connections"></a>Tipos de conexiones

### <a name="from-an-ssh-session"></a>Desde una sesión de SSH

Si se conecta desde una sesión de SSH a un nodo principal del clúster, puede conectarse a la dirección `headnodehost` en el puerto `10001`:

```bash
beeline -u 'jdbc:hive2://headnodehost:10001/;transportMode=http'
```

---

### <a name="over-an-azure-virtual-network"></a>A través de una instancia de Azure Virtual Network

Si se conecta desde un cliente a HDInsight a través de una instancia de Azure Virtual Network, debe proporcionar el nombre de dominio completo (FQDN) de un nodo principal del clúster. Puesto que esta conexión se realiza directamente a los nodos del clúster, se usa el puerto `10001`:

```bash
beeline -u 'jdbc:hive2://<headnode-FQDN>:10001/;transportMode=http'
```

Reemplace `<headnode-FQDN>` por el nombre de dominio completo de un nodo principal primario. Para buscar el nombre de dominio completo de un nodo principal, use la información del documento [Administración de clústeres de HDInsight con la API REST de Apache Ambari](../hdinsight-hadoop-manage-ambari-rest-api.md#get-the-fqdn-of-cluster-nodes).

---

### <a name="to-hdinsight-enterprise-security-package-esp-cluster-using-kerberos"></a>Al clúster de Enterprise Security Package (ESP) de HDInsight mediante Kerberos

Al conectarse desde un cliente a un clúster de Enterprise Security Package (ESP) unido a Azure Active Directory (AAD)-DS en una máquina del mismo dominio del clúster, también debe especificar el nombre de dominio `<AAD-Domain>` y el nombre de una cuenta de usuario de dominio que tenga permisos para acceder al clúster `<username>`:

```bash
kinit <username>
beeline -u 'jdbc:hive2://<headnode-FQDN>:10001/default;principal=hive/_HOST@<AAD-Domain>;auth-kerberos;transportMode=http' -n <username>
```

Reemplace `<username>` con el nombre de una cuenta en el dominio con permisos para obtener acceso al clúster. Reemplace `<AAD-DOMAIN>` por el nombre de la instancia de Azure Active Directory (AAD) a la que está unido el clúster. Use una cadena en mayúsculas para el valor `<AAD-DOMAIN>`; de lo contrario, la credencial no se encontrará. Consulte en `/etc/krb5.conf` los nombres de dominio kerberos si es necesario.

Para buscar la dirección URL de JDBC desde Ambari:

1. En un explorador web, vaya a `https://CLUSTERNAME.azurehdinsight.net/#/main/services/HIVE/summary`, donde `CLUSTERNAME` es el nombre del clúster. Asegúrese de que HiveServer2 está en ejecución.

1. Use el portapapeles para copiar la dirección URL de JDBC de HiveServer2.

---

### <a name="over-public-or-private-endpoints"></a>A través de puntos de conexión públicos o privados

Al conectarse a un clúster mediante los puntos de conexión públicos o privados, debe proporcionar el nombre de la cuenta de inicio de sesión (valor predeterminado `admin`) y la contraseña del clúster. Por ejemplo, al usar Beeline desde un sistema cliente para conectarse a la dirección `clustername.azurehdinsight.net`. Esta conexión se realiza a través del puerto `443` y se cifra mediante TLS/SSL.

Reemplace `clustername` por el nombre del clúster de HDInsight. Reemplace `admin` por la cuenta de inicio de sesión del clúster. Para los clústeres de ESP, use el UPN completo (por ejemplo, user@domain.com). Reemplace `password` por la contraseña de la cuenta de inicio de sesión del clúster.

```bash
beeline -u 'jdbc:hive2://clustername.azurehdinsight.net:443/;ssl=true;transportMode=http;httpPath=/hive2' -n admin -p 'password'
```

o para el punto de conexión privado:

```bash
beeline -u 'jdbc:hive2://clustername-int.azurehdinsight.net:443/;ssl=true;transportMode=http;httpPath=/hive2' -n admin -p 'password'
```

Los puntos de conexión privados apuntan a un equilibrador de carga básico al que solo se puede acceder desde las redes virtuales emparejadas en la misma región. Consulte cuáles son las [restricciones relacionadas con Emparejamiento de VNet global y los equilibradores de carga](../../virtual-network/virtual-networks-faq.md#what-are-the-constraints-related-to-global-vnet-peering-and-load-balancers) para más información. Puede usar el comando `curl` con la opción `-v` para solucionar cualquier problema de conectividad con puntos de conexión públicos o privados antes de usar Beeline.

---

### <a name="use-beeline-with-apache-spark"></a>Uso de Beeline con Apache Spark

Apache Spark proporciona su propia implementación de HiveServer2 que, en algunas ocasiones, se denomina servidor Thrift de Spark. Este servicio usa Spark SQL para resolver consultas en lugar de Hive. Además, puede proporcionar un mejor rendimiento en función de la consulta.

#### <a name="through-public-or-private-endpoints"></a>A través de puntos de conexión públicos o privados

La cadena de conexión usada es ligeramente distinta. En lugar de contener `httpPath=/hive2`, usa `httpPath/sparkhive2`. Reemplace `clustername` por el nombre del clúster de HDInsight. Reemplace `admin` por la cuenta de inicio de sesión del clúster. Para los clústeres de ESP, use el UPN completo (por ejemplo, user@domain.com). Reemplace `password` por la contraseña de la cuenta de inicio de sesión del clúster.

```bash
beeline -u 'jdbc:hive2://clustername.azurehdinsight.net:443/;ssl=true;transportMode=http;httpPath=/sparkhive2' -n admin -p 'password'
```

o para el punto de conexión privado:

```bash
beeline -u 'jdbc:hive2://clustername-int.azurehdinsight.net:443/;ssl=true;transportMode=http;httpPath=/sparkhive2' -n admin -p 'password'
```

Los puntos de conexión privados apuntan a un equilibrador de carga básico al que solo se puede acceder desde las redes virtuales emparejadas en la misma región. Consulte cuáles son las [restricciones relacionadas con Emparejamiento de VNet global y los equilibradores de carga](../../virtual-network/virtual-networks-faq.md#what-are-the-constraints-related-to-global-vnet-peering-and-load-balancers) para más información. Puede usar el comando `curl` con la opción `-v` para solucionar cualquier problema de conectividad con puntos de conexión públicos o privados antes de usar Beeline.

---

#### <a name="from-cluster-head-or-inside-azure-virtual-network-with-apache-spark"></a>Desde el nodo principal del clúster o en Azure Virtual Network con Apache Spark

Con la conexión directa desde el nodo principal del clúster o desde un recurso de la misma red virtual de Azure que la del clúster de HDInsight, debe utilizarse el puerto `10002` para el servidor Thrift de Spark en lugar de `10001`. El ejemplo siguiente muestra cómo conectarse directamente al nodo principal:

```bash
/usr/hdp/current/spark2-client/bin/beeline -u 'jdbc:hive2://headnodehost:10002/;transportMode=http'
```

---

## <a name="prerequisites-for-examples"></a>Requisitos previos para los ejemplos

* Un clúster de Hadoop en HDInsight. Consulte [Introducción a HDInsight en Linux](./apache-hadoop-linux-tutorial-get-started.md).

* Observe el esquema de URI para el almacenamiento principal del clúster. Por ejemplo, `wasb://` para Azure Storage, `abfs://` para Azure Data Lake Storage Gen2 o `adl://` para Azure Data Lake Storage Gen1. Si se habilita la transferencia segura para Azure Storage, el identificador URI es `wasbs://`. Para más información, consulte el artículo sobre la [transferencia segura](../../storage/common/storage-require-secure-transfer.md).

* Opción 1: Un cliente SSH. Para más información, consulte [Conexión a través de SSH con HDInsight (Apache Hadoop)](../hdinsight-hadoop-linux-use-ssh-unix.md). En la mayoría de los pasos descritos en este documento se da por hecho que está usando Beeline desde una sesión de SSH al clúster.

* Opción 2:  Cliente de Beeline local.

## <a name="run-a-hive-query"></a>Ejecución de una consulta de Hive

Este ejemplo se basa en el uso del cliente de Beeline desde una conexión SSH.

1. Abra una conexión de SSH al clúster con el código siguiente. Reemplace `sshuser` por el usuario de SSH del clúster y `CLUSTERNAME` por el nombre de su clúster. Cuando se le solicite, escriba la contraseña de la cuenta de usuario de SSH.

    ```cmd
    ssh sshuser@CLUSTERNAME-ssh.azurehdinsight.net
    ```

2. Conéctese a HiveServer2 con el cliente de Beeline de la sesión de SSH abierta. Para ello, escriba el comando siguiente:

    ```bash
    beeline -u 'jdbc:hive2://headnodehost:10001/;transportMode=http'
    ```

3. Los comandos de Beeline empiezan con un carácter `!`. Por ejemplo `!help` muestra la ayuda. Sin embargo, el `!` se puede omitir en algunos comandos. Por ejemplo, `help` también funciona.

    Hay un `!sql`, que se usa para ejecutar instrucciones de HiveQL. Sin embargo, HiveQL es de uso tan frecuente que puede omitir el elemento `!sql`que le precede. Las dos instrucciones siguientes son equivalentes:

    ```hiveql
    !sql show tables;
    show tables;
    ```

    En un nuevo clúster, solo debe aparecer una tabla: **hivesampletable**.

4. Use el siguiente comando para mostrar el esquema de hivesampletable:

    ```hiveql
    describe hivesampletable;
    ```

    Este comando devuelve la siguiente información:

        +-----------------------+------------+----------+--+
        |       col_name        | data_type  | comment  |
        +-----------------------+------------+----------+--+
        | clientid              | string     |          |
        | querytime             | string     |          |
        | market                | string     |          |
        | deviceplatform        | string     |          |
        | devicemake            | string     |          |
        | devicemodel           | string     |          |
        | state                 | string     |          |
        | country               | string     |          |
        | querydwelltime        | double     |          |
        | sessionid             | bigint     |          |
        | sessionpagevieworder  | bigint     |          |
        +-----------------------+------------+----------+--+

    Esta información describe las columnas de la tabla.

5. Escriba las siguientes instrucciones para crear una tabla denominada **log4jLogs** con los datos de ejemplo proporcionados con el clúster de HDInsight: (Revise según sea necesario en función de su esquema de URI).

    ```hiveql
    DROP TABLE log4jLogs;
    CREATE EXTERNAL TABLE log4jLogs (
        t1 string,
        t2 string,
        t3 string,
        t4 string,
        t5 string,
        t6 string,
        t7 string)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
    STORED AS TEXTFILE LOCATION 'wasbs:///example/data/';
    SELECT t4 AS sev, COUNT(*) AS count FROM log4jLogs
        WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log'
        GROUP BY t4;
    ```

    Estas instrucciones realizan las acciones siguientes:

    |. |Descripción |
    |---|---|
    |DROP TABLE|Si la tabla existe, se elimina.|
    |CREATE EXTERNAL TABLE|Crea una tabla **externa** en Hive. Las tablas externas solo almacenan la definición de Tabla en Hive. Los datos permanecen en la ubicación original.|
    |FORMATO DE FILA|Indica el formato de los datos. En este caso, los campos de cada registro se separan mediante un espacio.|
    |STORED AS TEXTFILE LOCATION|Indica dónde se almacenan los datos y en qué formato de archivo.|
    |SELECT|Selecciona un recuento de todas las filas donde la columna **t4** contiene el valor **[ERROR]** . Esta consulta devuelve un valor de **3** ya que hay tres filas que contienen este valor.|
    |INPUT__FILE__NAME LIKE '%.log'|Hive intenta aplicar el esquema a todos los archivos en el directorio. En este caso, el directorio contiene archivos que no coinciden con el esquema. Para evitar que haya datos inservibles en los resultados, esta instrucción indica a Hive que solo se deben devolver datos de archivos que terminen en .log.|

   > [!NOTE]  
   > Las tablas externas se deben utilizar cuando se espera que un origen externo actualice los datos subyacentes. Por ejemplo, un proceso de carga de datos automatizado o una operación de MapReduce.
   >
   > La eliminación de una tabla externa **no** elimina los datos, solamente la definición de tabla.

    La salida de este comando es similar al texto siguiente:

        INFO  : Tez session hasn't been created yet. Opening session
        INFO  :

        INFO  : Status: Running (Executing on YARN cluster with App id application_1443698635933_0001)

        INFO  : Map 1: -/-      Reducer 2: 0/1
        INFO  : Map 1: 0/1      Reducer 2: 0/1
        INFO  : Map 1: 0/1      Reducer 2: 0/1
        INFO  : Map 1: 0/1      Reducer 2: 0/1
        INFO  : Map 1: 0/1      Reducer 2: 0/1
        INFO  : Map 1: 0(+1)/1  Reducer 2: 0/1
        INFO  : Map 1: 0(+1)/1  Reducer 2: 0/1
        INFO  : Map 1: 1/1      Reducer 2: 0/1
        INFO  : Map 1: 1/1      Reducer 2: 0(+1)/1
        INFO  : Map 1: 1/1      Reducer 2: 1/1
        +----------+--------+--+
        |   sev    | count  |
        +----------+--------+--+
        | [ERROR]  | 3      |
        +----------+--------+--+
        1 row selected (47.351 seconds)

6. Salga de Beeline:

    ```bash
    !exit
    ```

## <a name="run-a-hiveql-file"></a>Ejecución de un archivo de HiveQL

Este ejemplo es una continuación del ejemplo anterior. Use los pasos siguientes para crear un archivo y, luego, ejecútelo mediante Beeline.

1. Use el comando siguiente para crear un archivo denominado **query.hql**:

    ```bash
    nano query.hql
    ```

1. Use el texto siguiente como contenido del archivo. Esta consulta crea una nueva tabla "interna" denominada **errorLogs**:

    ```hiveql
    CREATE TABLE IF NOT EXISTS errorLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string) STORED AS ORC;
    INSERT OVERWRITE TABLE errorLogs SELECT t1, t2, t3, t4, t5, t6, t7 FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log';
    ```

    Estas instrucciones realizan las acciones siguientes:

    |. |Descripción |
    |---|---|
    |CREATE TABLE IF NOT EXISTS|Si todavía no existe la tabla, se crea. Dado que no se utiliza la palabra clave **EXTERNAL**, esta instrucción crea una tabla interna. Las tablas internas se guardan en el almacenamiento de datos de Hive y Hive las administra por completo.|
    |STORED AS ORC|almacena los datos en el formato de columnas de filas optimizadas (ORC, Optimized Row Columnar). ORC es un formato altamente optimizado y eficiente para almacenar datos de Hive.|
    |INSERT OVERWRITE ... SELECT|selecciona en la tabla **log4jLogs** las filas que contienen **[ERROR]** y, después, inserta los datos en la tabla **errorLogs**.|

    > [!NOTE]  
    > A diferencia de las tablas externas , la eliminación de una tabla interna también elimina los datos subyacentes.

1. Para guardar el archivo, use **Ctrl**+**X**, escriba **Y** y, finalmente, presione **Entrar**.

1. Use el siguiente código para ejecutar el archivo mediante Beeline:

    ```bash
    beeline -u 'jdbc:hive2://headnodehost:10001/;transportMode=http' -i query.hql
    ```

    > [!NOTE]  
    > El parámetro `-i` inicia Beeline y ejecuta las instrucciones del archivo `query.hql`. Una vez que se completa la consulta, llegará al aviso `jdbc:hive2://headnodehost:10001/>`. También puede ejecutar un archivo mediante el parámetro `-f` que sale de Beeline después de que la consulta se completa.

1. Para comprobar que la tabla **errorLogs** se creó, use la siguiente instrucción para devolver todas las filas de **errorLogs**:

    ```hiveql
    SELECT * from errorLogs;
    ```

    Se deben devolver tres filas de datos y todas ellas deben contener **[ERROR]** en la columna t4.

        +---------------+---------------+---------------+---------------+---------------+---------------+---------------+--+
        | errorlogs.t1  | errorlogs.t2  | errorlogs.t3  | errorlogs.t4  | errorlogs.t5  | errorlogs.t6  | errorlogs.t7  |
        +---------------+---------------+---------------+---------------+---------------+---------------+---------------+--+
        | 2012-02-03    | 18:35:34      | SampleClass0  | [ERROR]       | incorrect     | id            |               |
        | 2012-02-03    | 18:55:54      | SampleClass1  | [ERROR]       | incorrect     | id            |               |
        | 2012-02-03    | 19:25:27      | SampleClass4  | [ERROR]       | incorrect     | id            |               |
        +---------------+---------------+---------------+---------------+---------------+---------------+---------------+--+
        3 rows selected (0.813 seconds)

## <a name="install-beeline-client"></a>Instalación de un cliente de Beeline

Aunque Beeline se incluye en los nodos principales, puede que desee instalarlo localmente.  Los pasos de instalación en una máquina local se basan en un [subsistema de Windows para Linux](https://docs.microsoft.com/windows/wsl/install-win10).

1. Listas de actualizaciones. Escriba los comandos siguientes en su shell de Bash:

    ```bash
    sudo apt-get update
    ```

1. Instale Java si no está instalado. Puede hacerlo con el comando `which java`.

    1. Si no hay instalado ningún paquete de Java, escriba el siguiente comando:

        ```bash
        sudo apt install openjdk-11-jre-headless
        ```

    1. Abra el archivo bashrc (a menudo se encuentra en ~/.bashrc): `nano ~/.bashrc`.

    1. Modifique el archivo bashrc. Agregue la siguiente línea al final del archivo:

        ```bash
        export JAVA_HOME=/usr/lib/jvm/java-1.11.0-openjdk-amd64
        ```

        Para guardar el archivo, presione **Ctrl+x**, luego **Y** y después Entrar.

1. Descargue los archivos de Hadoop y Beeline, y escriba los siguientes comandos:

    ```bash
    wget https://archive.apache.org/dist/hadoop/core/hadoop-2.7.3/hadoop-2.7.3.tar.gz
    wget https://archive.apache.org/dist/hive/hive-1.2.1/apache-hive-1.2.1-bin.tar.gz
    ```

1. Desempaquete los archivos y escriba los comandos siguientes:

    ```bash
    tar -xvzf hadoop-2.7.3.tar.gz
    tar -xvzf apache-hive-1.2.1-bin.tar.gz
    ```

1. Modifique aún más el archivo bashrc. Deberá identificar la ruta de acceso en la que se desempaquetan los archivos. Si usa el [subsistema de Windows para Linux](https://docs.microsoft.com/windows/wsl/install-win10) y siguió los pasos de manera exacta, la ruta de acceso sería `/mnt/c/Users/user/`, donde `user` es el nombre de usuario.

    1. Abra el archivo `nano ~/.bashrc`

    1. Modifique los comandos siguientes con la ruta de acceso adecuada y, a continuación, escríbalos al final del archivo bashrc:

        ```bash
        export HADOOP_HOME=/path_where_the_archives_were_unpacked/hadoop-2.7.3
        export HIVE_HOME=/path_where_the_archives_were_unpacked/apache-hive-1.2.1-bin
        PATH=$PATH:$HIVE_HOME/bin
        ```

    1. Para guardar el archivo, presione **Ctrl+x**, luego **Y** y después Entrar.

1. Cierre y vuelva a abrir la sesión de Bash.

1. Pruebe la conexión. Use el formato de conexión de [A través de puntos de conexión públicos o privados](#over-public-or-private-endpoints), arriba.

## <a name="next-steps"></a>Pasos siguientes

* Para obtener más información general sobre Hive en HDInsight, consulte [Uso de Apache Hive con Apache Hadoop en HDInsight](hdinsight-use-hive.md).

* Si quiere más información sobre otras formas en que puede trabajar con Hadoop en HDInsight, consulte [Uso de MapReduce con Apache Hadoop en HDInsight](hdinsight-use-mapreduce.md).

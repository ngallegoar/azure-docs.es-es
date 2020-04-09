---
title: Migración de cargas de trabajo de Hive de Azure HDInsight 3.6 a HDInsight 4.0
description: Aprenda a migrar las cargas de trabajo de Apache Hive en HDInsight 3.6 a 4.0 de HDInsight.
author: msft-tacox
ms.author: tacox
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 11/13/2019
ms.openlocfilehash: ec96189185a06c1fcbd95eed6216ade47f3089c3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "79214656"
---
# <a name="migrate-azure-hdinsight-36-hive-workloads-to-hdinsight-40"></a>Migración de cargas de trabajo de Hive de Azure HDInsight 3.6 a HDInsight 4.0

Este documento le muestra cómo migrar las cargas de trabajo de Apache Hive y LLAP en HDInsight 3.6 a HDInsight 4.0. HDInsight 4.0 proporciona las características más recientes de Hive y LLAP como vistas materializadas y almacenamiento en caché de resultados de consulta. Al migrar las cargas de trabajo a HDInsight 4.0, puede utilizar muchas características más recientes de Hive 3 que no están disponibles en HDInsight 3.6.

En este artículo se tratan los siguientes asuntos:

* Migración de los metadatos de Hive a HDInsight 4.0
* Migración segura de tablas ACID y non-ACID
* Conservación de las directivas de seguridad de Hive en las versiones de HDInsight
* Ejecución de consultas y depuración de HDInsight 3.6 a HDInsight 4.0

Una de las ventajas de Hive es la capacidad de exportar los metadatos a una base de datos externa (denominada Hive Metastore). **Hive Metastore** es responsable de almacenar las estadísticas de tabla, incluida la ubicación de almacenamiento de la tabla, nombres de columna e información de índices de tabla. El esquema de base de datos de Hive Metastore difiere entre las versiones de Hive. La manera recomendada de actualizar el metastore de Hive de forma segura es crear una copia y actualizarla en lugar del entorno de producción actual.

## <a name="copy-metastore"></a>Copia del metastore de Hive

HDInsight 3.6 y HDInsight 4.0 requieren esquemas diferentes de Metastore y no pueden compartir una misma instancia de Hive Metastore.

### <a name="external-metastore"></a>Metastore de Hive externo

Cree una nueva copia de la instancia externa de Hive Metastore. Si se usa un metastore de Hive externo, una de las formas seguras y sencillas de realizar una copia de dicho metastore es [restaurar la base de datos](../../sql-database/sql-database-recovery-using-backups.md#point-in-time-restore) con un nombre distinto mediante la función de restauración de datos de SQL Database.  Consulte [Uso de repositorios de metadatos externos en Azure HDInsight](../hdinsight-use-external-metadata-stores.md) para más información acerca de cómo adjuntar una instancia externa de Metastore a un clúster de HDInsight.

### <a name="internal-metastore"></a>Metastore de Hive interno

Si se usa el metastore de Hive interno, se pueden usar las consultas para exportar las definiciones de objetos en el metastore de Hive e importarlas en una base de datos nueva.

Una vez completado este script, se supone que el clúster anterior ya no se usará para acceder a las tablas o bases de datos a las que se hace referencia en el script.

> [!NOTE]
> En el caso de las tablas ACID, se creará una nueva copia de los datos que se encuentran debajo de la tabla.

1. Conéctese al clúster de HDInsight con un [cliente Secure Shell (SSH)](../hdinsight-hadoop-linux-use-ssh-unix.md).

1. Conéctese a HiveServer2 con el [cliente de Beeline](../hadoop/apache-hadoop-use-hive-beeline.md) de la sesión de SSH abierta. Para ello, escriba el comando siguiente:

    ```hiveql
    for d in `beeline -u "jdbc:hive2://localhost:10001/;transportMode=http" --showHeader=false --silent=true --outputformat=tsv2 -e "show databases;"`; 
    do
        echo "Scanning Database: $d"
        echo "create database if not exists $d; use $d;" >> alltables.hql; 
        for t in `beeline -u "jdbc:hive2://localhost:10001/$d;transportMode=http" --showHeader=false --silent=true --outputformat=tsv2 -e "show tables;"`;
        do
            echo "Copying Table: $t"
            ddl=`beeline -u "jdbc:hive2://localhost:10001/$d;transportMode=http" --showHeader=false --silent=true --outputformat=tsv2 -e "show create table $t;"`;

            echo "$ddl;" >> alltables.hql;
            lowerddl=$(echo $ddl | awk '{print tolower($0)}')
            if [[ $lowerddl == *"'transactional'='true'"* ]]; then
                if [[ $lowerddl == *"partitioned by"* ]]; then
                    # partitioned
                    raw_cols=$(beeline -u "jdbc:hive2://localhost:10001/$d;transportMode=http" --showHeader=false --silent=true --outputformat=tsv2 -e "show create table $t;" | tr '\n' ' ' | grep -io "CREATE TABLE .*" | cut -d"(" -f2- | cut -f1 -d")" | sed 's/`//g');
                    ptn_cols=$(beeline -u "jdbc:hive2://localhost:10001/$d;transportMode=http" --showHeader=false --silent=true --outputformat=tsv2 -e "show create table $t;" | tr '\n' ' ' | grep -io "PARTITIONED BY .*" | cut -f1 -d")" | cut -d"(" -f2- | sed 's/`//g');
                    final_cols=$(echo "(" $raw_cols "," $ptn_cols ")")

                    beeline -u "jdbc:hive2://localhost:10001/$d;transportMode=http" --showHeader=false --silent=true --outputformat=tsv2 -e "create external table ext_$t $final_cols TBLPROPERTIES ('transactional'='false');";
                    beeline -u "jdbc:hive2://localhost:10001/$d;transportMode=http" --showHeader=false --silent=true --outputformat=tsv2 -e "insert into ext_$t select * from $t;";
                    staging_ddl=`beeline -u "jdbc:hive2://localhost:10001/$d;transportMode=http" --showHeader=false --silent=true --outputformat=tsv2 -e "show create table ext_$t;"`;
                    dir=$(echo $staging_ddl | grep -io " LOCATION .*" | grep -m1 -o "'.*" | sed "s/'[^-]*//2g" | cut -c2-);

                    parsed_ptn_cols=$(echo $ptn_cols| sed 's/ [a-z]*,/,/g' | sed '$s/\w*$//g');
                    echo "create table flattened_$t $final_cols;" >> alltables.hql;
                    echo "load data inpath '$dir' into table flattened_$t;" >> alltables.hql;
                    echo "insert into $t partition($parsed_ptn_cols) select * from flattened_$t;" >> alltables.hql;
                    echo "drop table flattened_$t;" >> alltables.hql;
                    beeline -u "jdbc:hive2://localhost:10001/$d;transportMode=http" --showHeader=false --silent=true --outputformat=tsv2 -e "drop table ext_$t";
                else
                    # not partitioned
                    beeline -u "jdbc:hive2://localhost:10001/$d;transportMode=http" --showHeader=false --silent=true --outputformat=tsv2 -e "create external table ext_$t like $t TBLPROPERTIES ('transactional'='false');";
                    staging_ddl=`beeline -u "jdbc:hive2://localhost:10001/$d;transportMode=http" --showHeader=false --silent=true --outputformat=tsv2 -e "show create table ext_$t;"`;
                    dir=$(echo $staging_ddl | grep -io " LOCATION .*" | grep -m1 -o "'.*" | sed "s/'[^-]*//2g" | cut -c2-);

                    beeline -u "jdbc:hive2://localhost:10001/$d;transportMode=http" --showHeader=false --silent=true --outputformat=tsv2 -e "insert into ext_$t select * from $t;";
                    echo "load data inpath '$dir' into table $t;" >> alltables.hql;
                    beeline -u "jdbc:hive2://localhost:10001/$d;transportMode=http" --showHeader=false --silent=true --outputformat=tsv2 -e "drop table ext_$t";
                fi
            fi
            echo "$ddl" | grep -q "PARTITIONED\s*BY" && echo "MSCK REPAIR TABLE $t;" >> alltables.hql;
        done;
    done
    ```

    Este comando genera un archivo denominado **alltables.hql**.

1. Cierre la sesión de SSH. Después, escriba un comando SCP para descargar **alltables.hql** localmente.

    ```bash
    scp sshuser@CLUSTERNAME-ssh.azurehdinsight.net:alltables.hql c:/hdi
    ```

1. Cargue **alltables.hql** al clúster de HDInsight *nuevo*.

    ```bash
    scp c:/hdi/alltables.hql sshuser@CLUSTERNAME-ssh.azurehdinsight.net:/home/sshuser/
    ```

1. Después, use SSH para conectarse al clúster de HDInsight *nuevo*. En la sesión de SSH, ejecute el código siguiente:

    ```bash
    beeline -u "jdbc:hive2://localhost:10001/;transportMode=http" -i alltables.hql
    ```


## <a name="upgrade-metastore"></a>Actualización del metastore de Hive

Una vez que la **copia** del metastore de Hive ha finalizado, ejecute un script de actualización de esquema en [Acción de script](../hdinsight-hadoop-customize-cluster-linux.md) en el clúster de HDInsight 3.6 existente para actualizar el metastore de Hive nuevo al esquema de Hive 3. Esto permite adjuntar la base de datos como metastore de Hive de HDInsight 4.0.

Use los valores de la tabla más adelante. Reemplace `SQLSERVERNAME DATABASENAME USERNAME PASSWORD` por los valores adecuados para el metastore de Hive **copiado**, separados por espacios. No incluya ". database.windows.net" al especificar el nombre del servidor de SQL.

|Propiedad | Value |
|---|---|
|Tipo de script|- Personalizado|
|Nombre|Actualización de Hive|
|URI de script de Bash|`https://hdiconfigactions.blob.core.windows.net/hivemetastoreschemaupgrade/launch-schema-upgrade.sh`|
|Tipos de nodo|Head|
|Parámetros|SQLSERVERNAME DATABASENAME USERNAME PASSWORD|

> [!Warning]  
> La actualización que convierte el esquema de metadatos de HDInsight 3.6 en el esquema de HDInsight 4.0. no se puede revertir.

Se puede comprobar la actualización mediante la ejecución de la consulta SQL siguiente en la base de datos:

```sql
select * from dbo.version
```

## <a name="migrate-hive-tables-to-hdinsight-40"></a>Migración de tablas de Hive en HDInsight 4.0

Después de completar el conjunto de pasos anterior para migrar Hive Metastore a HDInsight 4.0, las tablas y bases de datos que se registran en la instancia de Metastore serán visibles desde dentro del clúster de HDInsight 4.0 mediante la ejecución de `show tables` o `show databases` desde dentro del clúster. Consulte [Ejecución de consultas entre diferentes versiones de HDInsight](#query-execution-across-hdinsight-versions) para obtener información sobre la ejecución de consultas en los clústeres de HDInsight 4.0.

Los datos reales de las tablas, sin embargo, no son accesibles hasta que el clúster tenga acceso a las cuentas de almacenamiento necesarias. Para asegurarse de que el clúster de HDInsight 4.0 puede acceder a los mismos datos que el antiguo clúster de HDInsight 3.6, complete los pasos siguientes:

1. Determine la cuenta de almacenamiento de Azure de la tabla o base de datos.

1. Si ya se está ejecutando el clúster de HDInsight 4.0, asocie la cuenta de almacenamiento de Azure al clúster mediante Ambari. Si todavía no ha creado el clúster de HDInsight 4.0, asegúrese de que la cuenta de almacenamiento de Azure se especifica como la principal o como una cuenta de almacenamiento de clúster secundaria. Para más información acerca de cómo agregar cuentas de almacenamiento a clústeres de HDInsight, consulte [Adición de más cuentas de almacenamiento a HDInsight](../hdinsight-hadoop-add-storage.md).

## <a name="deploy-new-hdinsight-40-and-connect-to-the-new-metastore"></a>Implementación del nuevo HDInsight 4.0 y conexión al metastore de Hive nuevo

Una vez finalizada la actualización del esquema, implemente un nuevo clúster de HDInsight 4.0 y conecte el metastore de Hive actualizado. Si ya se ha implementado 4.0, establézcalo para que pueda conectarse al metastore de Hive desde Ambari.

## <a name="run-schema-migration-script-from-hdinsight-40"></a>Ejecución del script de migración de esquema desde HDInsight 4.0

Las tablas se tratan de manera diferente en HDInsight 3.6 y HDInsight 4.0. Por este motivo, no pueden compartir las mismas tablas para los clústeres de distintas versiones. Si desea usar HDInsight 3.6 al mismo tiempo que HDInsight 4.0, tiene que tener copias independientes de los datos para cada versión.

La carga de trabajo de Hive puede incluir una mezcla de tablas ACID y non-ACID. Una diferencia clave entre Hive en HDInsight 3.6 (Hive 2) y Hive en HDInsight 4.0 (Hive 3) es el cumplimiento de las propiedades ACID para las tablas. En HDInsight 3.6, para habilitar el cumplimiento de ACID en Hive requiere configuración adicional, mientras que en HDInsight 4.0 las tablas son compatibles con ACID de forma predeterminada. La única acción necesaria antes de efectuar una migración es ejecutar una compactación principal con la tabla ACID en el clúster de 3.6. Desde la vista de Hive o Beeline, ejecute la consulta siguiente:

```sql
alter table myacidtable compact 'major';
```

Esta compactación es necesaria porque las tablas ACID de HDInsight 3.6 y HDInsight 4.0 reconocen los valores delta ACID de forma diferente. La compactación aplica un inicio limpio que garantiza la coherencia. La sección 4 de la [documentación de migración de Hive](https://docs.hortonworks.com/HDPDocuments/Ambari-2.7.3.0/bk_ambari-upgrade-major/content/prepare_hive_for_upgrade.html) contiene una guía para la compactación masiva de tablas ACID de HDInsight 3.6.

Una vez que haya completado los pasos de compactación y migración del metastore de Hive, puede migrar el almacenamiento real. Después de completar la migración del almacenamiento de Hive, el almacenamiento de HDInsight 4.0 tendrá las siguientes propiedades:

|3.6 |4.0 |
|---|---|
|Tablas externas|Tablas externas|
|Tablas administradas no transaccionales|Tablas externas|
|Tablas administradas transaccionales|Tablas administradas|

Es posible que tenga que ajustar las propiedades del almacenamiento antes de ejecutar la migración. Por ejemplo, si espera que haya acceso de terceros a alguna tabla (por ejemplo, a un clúster de HDInsight 3.6), esa tabla tiene que ser externa una vez completada la migración. En HDInsight 4.0, todas las tablas administradas son transaccionales. Por lo tanto, solo los clústeres de HDInsight 4.0 pueden acceder a las tablas administradas en HDInsight 4.0.

Una vez establecidas las propiedades de tabla correctamente, ejecute la herramienta de migración de almacenamiento de Hive desde uno de los nodos principales del clúster mediante el shell de SSH:

1. Conéctese a su nodo principal del clúster mediante SSH. Para obtener instrucciones, consulte [Conexión a HDInsight mediante SSH](../hdinsight-hadoop-linux-use-ssh-unix.md)
1. Abra un shell de inicio de sesión como el usuario de Hive ejecutando `sudo su - hive`
1. Determine la versión de la pila de la plataforma de datos ejecutando `ls /usr/hdp`. Esto mostrará una cadena de versión que debe usar en el siguiente comando.
1. Ejecute el comando siguiente en el shell. Reemplace `STACK_VERSION` por la cadena de versión del paso anterior:

```bash
/usr/hdp/STACK_VERSION/hive/bin/hive --config /etc/hive/conf --service  strictmanagedmigration --hiveconf hive.strict.managed.tables=true -m automatic --modifyManagedTables
```

Una vez que la herramienta de migración termina, el almacenamiento de Hive estará listo para HDInsight 4.0.

> [!Important]  
> No se deba acceder a las tablas administradas en 4.0 HDInsight (incluidas las migradas desde 3.6) desde otros servicios o aplicaciones, incluidos los clústeres de HDInsight 3.6.

## <a name="secure-hive-across-hdinsight-versions"></a>Protección de Hive en diferentes versiones de HDInsight

Desde HDInsight 3.6, HDInsight se integra con Azure Active Directory mediante Enterprise Security Package (ESP) de HDInsight. ESP utiliza Kerberos y Apache Ranger para administrar los permisos de recursos específicos dentro del clúster. Las directivas de Ranger implementadas con Hive en HDInsight 3.6 se pueden migrar a HDInsight 4.0 con los pasos siguientes:

1. Vaya al panel de Ranger Service Manager en el clúster de HDInsight 3.6.
2. Vaya a la directiva denominada **HIVE** y expórtela a un archivo json.
3. Asegúrese de que todos los usuarios a los que se hace referencia en el archivo json de directiva exportada existen en el nuevo clúster. Si en el json de directiva aparece una referencia a un usuario que no existe en el nuevo clúster, tendrá que agregar el usuario al nuevo clúster o quitar la referencia de la directiva.
4. Vaya al panel de **Ranger Service Manager** en el clúster de HDInsight 4.0.
5. Vaya a la directiva denominada **HIVE** e importe el archivo json de directiva de ranger del paso 2.

## <a name="check-compatibility-and-modify-codes-as-needed-in-test-app"></a>Comprobación de la compatibilidad y modificación de los códigos según sea necesario en la aplicación de prueba

Al migrar cargas de trabajo como programas y consultas existentes, consulte las notas de la versión y la documentación para obtener información sobre los cambios y aplique los cambios según sea necesario. Si el clúster de HDInsight 3.6 usa Spark y un metastore de Hive compartidos, será necesaria una [configuración adicional mediante Hive Warehouse Connector](./apache-hive-warehouse-connector.md).

## <a name="deploy-new-app-for-production"></a>Implementación de una nueva aplicación para producción

Para cambiar al clúster nuevo, por ejemplo, se puede instalar una nueva aplicación cliente y usarla como nuevo entorno de producción, o bien puede actualizar la aplicación cliente existente y cambiar a HDInsight 4.0.

## <a name="switch-hdinsight-40-to-the-production"></a>Cambio de HDInsight 4.0 a la producción

Si se han creado diferencias en el metastore de Hive durante las pruebas, deberá actualizar los cambios justo antes de cambiar. En este caso, puede exportar e importar el metastore de Hive y, después, actualizarlo de nuevo.

## <a name="remove-the-old-production"></a>Eliminación de la producción anterior

Una vez que haya confirmado que la versión está completa y totalmente operativa, puede quitar la versión 3.6 y el metastore de Hive anterior. Asegúrese de que todo se ha migrado antes de eliminar el entorno.

## <a name="query-execution-across-hdinsight-versions"></a>Ejecución de consultas entre diferentes versiones de HDInsight

Hay dos maneras de ejecutar y depurar las consultas de Hive/LLAP dentro de un clúster de HDInsight 3.6. HiveCLI proporciona una experiencia de línea de comandos y la vista de Hive/vista de Tez proporciona un flujo de trabajo basado en una GUI.

En HDInsight 4.0, HiveCLI se ha reemplazado por Beeline. HiveCLI es un cliente de Thrift para Hiveserver 1 y Beeline es un cliente JDBC que proporciona acceso a Hiveserver 2. Beeline también se puede usar para conectarse a cualquier otro punto de conexión de la base de datos compatible con JDBC. Beeline está disponible como componente integrado en HDInsight 4.0 sin necesidad de instalación.

En HDInsight 3.6, el cliente de la interfaz gráfica de usuario para interactuar con el servidor de Hive es la vista de Hive de Ambari. HDInsight 4.0 no se incluye en la vista Ambari. Hemos proporcionado una manera para que nuestros clientes usen Data Analytics Studio (DAS), que no es un servicio principal de HDInsight. DAS no se suministra integrado con clústeres de HDInsight y no es un paquete oficialmente compatible. Sin embargo, DAS se puede instalar en el clúster mediante una [acción de script](../hdinsight-hadoop-customize-cluster-linux.md), de la forma siguiente:

|Propiedad | Value |
|---|---|
|Tipo de script|- Personalizado|
|Nombre|DAS|
|URI de script de Bash|`https://hdiconfigactions.blob.core.windows.net/dasinstaller/LaunchDASInstaller.sh`|
|Tipos de nodo|Head|

Espere entre 10 y 15 minutos y luego inicie Data Analytics Studio mediante esta dirección URL: `https://CLUSTERNAME.azurehdinsight.net/das/`.

Es posible que sea necesaria una actualización de la interfaz de usuario de Ambari o un reinicio de todos sus componentes para acceder a DAS.

Una vez instalado DAS, si no ve las consultas que haya ejecutado en el visor de consultas, realice los pasos siguientes:

1. Establezca las configuraciones de Hive, Tez y DAS, como se describe en [esta guía para solucionar problemas de instalación de DAS](https://docs.hortonworks.com/HDPDocuments/DAS/DAS-1.2.0/troubleshooting/content/das_queries_not_appearing.html).
2. Asegúrese de que las siguientes configuraciones de directorio de almacenamiento de Azure son blobs en páginas, y que aparecen en `fs.azure.page.blob.dirs`:
    * `hive.hook.proto.base-directory`
    * `tez.history.logging.proto-base-dir`
3. Reinicie HDFS, Tez, Hive y DAS en ambos nodos principales.

## <a name="next-steps"></a>Pasos siguientes

* [Anuncio de HDInsight 4.0](../hdinsight-version-release.md)
* [Análisis a fondo de HDInsight 4.0](https://azure.microsoft.com/blog/deep-dive-into-azure-hdinsight-4-0/)
* [Tablas ACID de Hive 3](https://docs.hortonworks.com/HDPDocuments/HDP3/HDP-3.1.0/using-hiveql/content/hive_3_internals.html)

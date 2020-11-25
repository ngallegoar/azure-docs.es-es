---
title: Migración de cargas de trabajo de Hive de Azure HDInsight 3.6 a HDInsight 4.0
description: Aprenda a migrar las cargas de trabajo de Apache Hive en HDInsight 3.6 a 4.0 de HDInsight.
author: msft-tacox
ms.author: tacox
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: how-to
ms.date: 11/13/2019
ms.openlocfilehash: bcc0faa8fdbd61ab3e3e0886256f7c796e5a98e2
ms.sourcegitcommit: a43a59e44c14d349d597c3d2fd2bc779989c71d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/25/2020
ms.locfileid: "96011513"
---
# <a name="migrate-azure-hdinsight-36-hive-workloads-to-hdinsight-40"></a>Migración de cargas de trabajo de Hive de Azure HDInsight 3.6 a HDInsight 4.0

Este documento le muestra cómo migrar las cargas de trabajo de Apache Hive y LLAP en HDInsight 3.6 a HDInsight 4.0. HDInsight 4.0 proporciona las características más recientes de Hive y LLAP como vistas materializadas y almacenamiento en caché de resultados de consulta. Al migrar las cargas de trabajo a HDInsight 4.0, puede utilizar muchas características más recientes de Hive 3 que no están disponibles en HDInsight 3.6.

En este artículo se tratan los siguientes asuntos:

* Migración de los metadatos de Hive a HDInsight 4.0
* Migración segura de tablas ACID y non-ACID
* Conservación de las directivas de seguridad de Hive en las versiones de HDInsight
* Ejecución de consultas y depuración de HDInsight 3.6 a HDInsight 4.0

Una de las ventajas de Hive es la capacidad de exportar los metadatos a una base de datos externa (denominada Hive Metastore). **Hive Metastore** es responsable de almacenar las estadísticas de tabla, incluida la ubicación de almacenamiento de la tabla, nombres de columna e información de índices de tabla. HDInsight 3.6 y HDInsight 4.0 requieren esquemas diferentes de Metastore y no pueden compartir una misma instancia de Hive Metastore. La manera recomendada de actualizar el metastore de Hive de forma segura es actualizar una copia en lugar del original en el entorno de producción actual. Este documento requiere que los clústeres originales y nuevos tengan acceso a la misma cuenta de almacenamiento. Por lo tanto, no trata la migración de datos a otra región.

## <a name="migrate-from-external-metastore"></a>Migración desde metastore externo

### <a name="1-run-major-compaction-on-acid-tables-in-hdinsight-36"></a>1. Ejecutar la compactación principal en tablas ACID en HDInsight 3.6

Las tablas ACID de HDInsight 3.6 y HDInsight 4.0 reconocen los valores delta ACID de forma diferente. La única acción necesaria antes de efectuar una migración es ejecutar una compactación principal en cada tabla ACID en el clúster de HDInsight 3.6. Para más información sobre la compactación, consulte el [manual del lenguaje Hive](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+DDL#LanguageManualDDL-AlterTable/Partition/Compact).

### <a name="2-copy-sql-database"></a>2. Copiar la base de datos SQL
Cree una nueva copia de la instancia externa de Hive Metastore. Si se usa un metastore externo, una de las formas seguras y sencillas de realizar una copia de dicho metastore es [restaurar la base de datos](../../azure-sql/database/recovery-using-backups.md#point-in-time-restore) con un nombre distinto, usando para ello la función `RESTORE`.  Consulte [Uso de repositorios de metadatos externos en Azure HDInsight](../hdinsight-use-external-metadata-stores.md) para más información acerca de cómo adjuntar una instancia externa de Metastore a un clúster de HDInsight.

### <a name="3-upgrade-metastore-schema"></a>3. Actualizar el esquema de metastore
Una vez que la **copia** del metastore de Hive ha finalizado, ejecute un script de actualización de esquema en [Acción de script](../hdinsight-hadoop-customize-cluster-linux.md) en el clúster de HDInsight 3.6 existente para actualizar el metastore de Hive nuevo al esquema de Hive 3. (Para este paso no es necesario que el nuevo metastore esté conectado a un clúster). Esto permite adjuntar la base de datos como metastore de Hive de HDInsight 4.0.

Use los valores de la tabla más adelante. Reemplace `SQLSERVERNAME DATABASENAME USERNAME PASSWORD` por los valores adecuados para la **copia** del metastore de Hive, separados por espacios. No incluya ". database.windows.net" al especificar el nombre del servidor de SQL.

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

### <a name="4-deploy-a-new-hdinsight-40-cluster"></a>4. Implementar un nuevo clúster de HDInsight 4.0

1. Especifique el metastore actualizado como el nuevo metastore de Hive del clúster.

1. Los datos reales de las tablas, sin embargo, no son accesibles hasta que el clúster tenga acceso a las cuentas de almacenamiento necesarias.
Asegúrese de que las cuentas de almacenamiento de las tablas de Hive del clúster de HDInsight 3.6 se especifican como cuentas de almacenamiento principales o secundarias del nuevo clúster de HDInsight 4.0.
Para más información acerca de cómo agregar cuentas de almacenamiento a clústeres de HDInsight, consulte [Adición de más cuentas de almacenamiento a HDInsight](../hdinsight-hadoop-add-storage.md).

### <a name="5-complete-migration-with-a-post-upgrade-tool-in-hdinsight-40"></a>5. Completar la migración con una herramienta posterior a la actualización en HDInsight 4.0

De forma predeterminada, las tablas administradas deben ser compatibles con ACID en HDInsight 4.0. Una vez que haya completado la migración del metastore, ejecute una herramienta posterior a la actualización para que las tablas que no estaban previamente administradas por ACID sean compatibles con el clúster de HDInsight 4.0. Esta herramienta aplicará la conversión siguiente:

|3.6 |4.0 |
|---|---|
|Tablas externas|Tablas externas|
|Tablas no administradas por ACID|Tablas externas con la propiedad 'external.table.purge'='true'|
|Tablas administradas por ACID|Tablas administradas por ACID|

Ejecute la herramienta posterior a la actualización de Hive desde el clúster de HDInsight 4.0 con el shell de SSH:

1. Conéctese a su nodo principal del clúster mediante SSH. Para obtener instrucciones, consulte [Conexión a HDInsight mediante SSH](../hdinsight-hadoop-linux-use-ssh-unix.md)
1. Abra un shell de inicio de sesión como el usuario de Hive ejecutando `sudo su - hive`
1. Ejecute el comando siguiente en el shell.

    ```bash
    STACK_VERSION=$(hdp-select status hive-server2 | awk '{ print $3; }')
    /usr/hdp/$STACK_VERSION/hive/bin/hive --config /etc/hive/conf --service  strictmanagedmigration --hiveconf hive.strict.managed.tables=true -m automatic --modifyManagedTables
    ```

Una vez que la herramienta finalice, el almacenamiento de Hive estará listo para HDInsight 4.0.

## <a name="migrate-from-internal-metastore"></a>Migración desde metastore interno

Si el clúster de HDInsight 3.6 usa un metastore de Hive interno, siga los pasos que se indican a continuación para ejecutar un script, que generará consultas de Hive para exportar definiciones de objetos desde el metastore.

Los clústeres de HDInsight 3.6 y 4.0 deben usar la misma cuenta de almacenamiento.

> [!NOTE]
>
> * En el caso de las tablas ACID, se creará una nueva copia de los datos que se encuentran debajo de la tabla.
>
> * Este script solo admite la migración de bases de datos, tablas y particiones de Hive. Otros objetos de metadatos, como vistas, funciones definidas por el usuario y restricciones de tabla deben copiarse manualmente.
>
> * Una vez completado este script, se supone que el clúster anterior ya no se usará para acceder a las tablas o bases de datos a las que se hace referencia en el script.
>
> * Todas las tablas administradas pasarán a ser transaccionales en HDInsight 4.0. Si lo desea, puede conservar la tabla como no transaccional; para ello, exporte los datos a una tabla externa con la propiedad 'external.table.purge'='true'. Por ejemplo,
>
>    ```SQL
>    create table tablename_backup like tablename;
>    insert overwrite table tablename_backup select * from tablename;
>    create external table tablename_tmp like tablename;
>    insert overwrite table tablename_tmp select * from tablename;
>    alter table tablename_tmp set tblproperties('external.table.purge'='true');
>    drop table tablename;
>    alter table tablename_tmp rename to tablename;
>    ```

1. Conéctese al clúster de HDInsight 3.6 con un [cliente Secure Shell (SSH)](../hdinsight-hadoop-linux-use-ssh-unix.md).

1. Desde la sesión SSH abierta, descargue el siguiente archivo de script para generar un archivo llamado **alltables.hql**.

    ```bash
    wget https://hdiconfigactions.blob.core.windows.net/hivemetastoreschemaupgrade/exporthive_hdi_3_6.sh
    chmod 755 exporthive_hdi_3_6.sh
    ```

    * Para un clúster de HDInsight normal, sin ESP, simplemente ejecute `exporthive_hdi_3_6.sh`.

    * Para un clúster con ESP, ejecute kinit y cambie los argumentos a Beeline: ejecute lo siguiente, definiendo USER y DOMAIN para el usuario de Azure AD con permisos de Hive completos.

        ```bash
        USER="USER"  # replace USER
        DOMAIN="DOMAIN"  # replace DOMAIN
        DOMAIN_UPPER=$(printf "%s" "$DOMAIN" | awk '{ print toupper($0) }')
        kinit "$USER@$DOMAIN_UPPER"
        ```

        ```bash
        hn0=$(grep hn0- /etc/hosts | xargs | cut -d' ' -f4)
        BEE_CMD="beeline -u 'jdbc:hive2://$hn0:10001/default;principal=hive/_HOST@$DOMAIN_UPPER;auth-kerberos;transportMode=http' -n "$USER@$DOMAIN" --showHeader=false --silent=true --outputformat=tsv2 -e"
        ./exporthive_hdi_3_6.sh "$BEE_CMD"
        ```

1. Cierre la sesión de SSH. Después, escriba un comando SCP para descargar **alltables.hql** localmente.

    ```bash
    scp sshuser@CLUSTERNAME-ssh.azurehdinsight.net:alltables.hql c:/hdi
    ```

1. Cargue **alltables.hql** al clúster de HDInsight *nuevo*.

    ```bash
    scp c:/hdi/alltables.hql sshuser@CLUSTERNAME-ssh.azurehdinsight.net:/home/sshuser/
    ```

1. Después, use SSH para conectarse al clúster de HDInsight 4.0 *nuevo*. Ejecute el siguiente código desde una sesión de SSH en el clúster:

    Sin ESP:

    ```bash
    beeline -u "jdbc:hive2://localhost:10001/;transportMode=http" -f alltables.hql
    ```

    Con ESP:

    ```bash
    USER="USER"  # replace USER
    DOMAIN="DOMAIN"  # replace DOMAIN
    DOMAIN_UPPER=$(printf "%s" "$DOMAIN" | awk '{ print toupper($0) }')
    kinit "$USER@$DOMAIN_UPPER"
    ```

    ```bash
    hn0=$(grep hn0- /etc/hosts | xargs | cut -d' ' -f4)
    beeline -u "jdbc:hive2://$hn0:10001/default;principal=hive/_HOST@$DOMAIN_UPPER;auth-kerberos;transportMode=http" -n "$USER@$DOMAIN" -f alltables.hql
    ```

La herramienta posterior a la actualización para la migración del metastore externo no es aplicable aquí, ya que las tablas no administradas por ACID de HDInsight 3.6 se convierten en tablas administradas por ACID en HDInsight 4.0.

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

Hay dos maneras de ejecutar y depurar las consultas de Hive/LLAP dentro de un clúster de HDInsight 3.6. HiveCLI proporciona una experiencia de línea de comandos y la [vista de Tez/Hive](../hadoop/apache-hadoop-use-hive-ambari-view.md) proporciona un flujo de trabajo basado en una interfaz gráfica de usuario.

En HDInsight 4.0, HiveCLI se ha reemplazado por Beeline. La vista Tez/Hive proporciona un flujo de trabajo basado en una interfaz gráfica de usuario. HiveCLI es un cliente de Thrift para Hiveserver 1 y Beeline es un cliente JDBC que proporciona acceso a Hiveserver 2. Beeline también se puede usar para conectarse a cualquier otro punto de conexión de base de datos compatible con JDBC. Beeline está disponible como componente integrado en HDInsight 4.0 sin necesidad de instalación.

## <a name="next-steps"></a>Pasos siguientes

* [Anuncio de HDInsight 4.0](../hdinsight-version-release.md)
* [Análisis a fondo de HDInsight 4.0](https://azure.microsoft.com/blog/deep-dive-into-azure-hdinsight-4-0/)
* [Tablas ACID de Hive 3](https://docs.hortonworks.com/HDPDocuments/HDP3/HDP-3.1.0/using-hiveql/content/hive_3_internals.html)
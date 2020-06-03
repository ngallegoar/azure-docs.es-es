---
title: 'Flujos de trabajo de Apache Oozie y Enterprise Security: Azure HDInsight'
description: Proteja los flujos de trabajo de Apache Oozie con Enterprise Security Package de Azure HDInsight. Aprenda a definir un flujo de trabajo de Oozie y enviar un trabajo de Oozie.
author: omidm1
ms.author: omidm
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive,seodec18,seoapr2020
ms.date: 05/14/2020
ms.openlocfilehash: 36c04480c46cea904b072c659c5c2642a28e1f27
ms.sourcegitcommit: fdec8e8bdbddcce5b7a0c4ffc6842154220c8b90
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/19/2020
ms.locfileid: "83647573"
---
# <a name="run-apache-oozie-in-azure-hdinsight-clusters-with-enterprise-security-package"></a>Ejecución de Apache Oozie en clústeres Azure HDInsight con Enterprise Security Package

Apache Oozie es un sistema de coordinación y flujos de trabajo que administra trabajos de Apache Hadoop. Oozie se integra con la pila de Hadoop y admite los siguientes trabajos:

- Apache MapReduce
- Apache Pig
- Apache Hive
- Apache Sqoop

Oozie también puede usarse para programar trabajos específicos de un sistema, como scripts de shell o programas Java.

## <a name="prerequisite"></a>Requisito previo

Un clúster Azure HDInsight Hadoop con Enterprise Security Package (ESP). Consulte [Configuración de clústeres de HDInsight con Enterprise Security Package](./apache-domain-joined-configure-using-azure-adds.md).

> [!NOTE]  
> Para obtener instrucciones detalladas sobre el uso de Oozie en clústeres que no son ESP, vea el artículo sobre el [uso de flujos de trabajo de Apache Oozie en Azure HDInsight basado en Linux](../hdinsight-use-oozie-linux-mac.md).

## <a name="connect-to-an-esp-cluster"></a>Conexión a un clúster ESP

Para más información, consulte [Conexión a través de SSH con HDInsight (Hadoop)](../hdinsight-hadoop-linux-use-ssh-unix.md).

1. Conéctese al clúster de HDInsight con SSH:

    ```bash
    ssh [DomainUserName]@<clustername>-ssh.azurehdinsight.net
    ```

1. Para verificar la autenticación Kerberos correcta, utilice el comando `klist`. Si no es así, utilice `kinit` para iniciar la autenticación Kerberos.

1. Inicie sesión en la puerta de enlace de HDInsight para registrar el token de OAuth necesario para acceder a Azure Data Lake Storage:

    ```bash
    curl -I -u [DomainUserName@Domain.com]:[DomainUserPassword] https://<clustername>.azurehdinsight.net
    ```

    Un código de respuesta de estado **200 Correcto** indica que el registro se ha realizado con éxito. Compruebe el nombre de usuario y la contraseña si recibe una respuesta, como puede ser la 401.

## <a name="define-the-workflow"></a>Definición del flujo de trabajo

Las definiciones de flujo de trabajo de Oozie se escriben en el lenguaje de definición de proceso de Apache Hadoop (hPDL). hPDL es un lenguaje de definición de procesos XML. Use los pasos siguientes para definir el flujo de trabajo:

1. Configuración del área de trabajo del usuario de dominio:

   ```bash
   hdfs dfs -mkdir /user/<DomainUser>
   cd /home/<DomainUserPath>
   cp /usr/hdp/<ClusterVersion>/oozie/doc/oozie-examples.tar.gz .
   tar -xvf oozie-examples.tar.gz
   hdfs dfs -put examples /user/<DomainUser>/
   ```

   Reemplace `DomainUser` con el nombre de usuario del dominio.
   Reemplace `DomainUserPath` con la ruta de acceso del directorio principal del usuario de dominio.
   Reemplace `ClusterVersion` con la versión de su plataforma de datos de clúster.

2. Use la instrucción siguiente para crear y editar un archivo nuevo:

   ```bash
   nano workflow.xml
   ```

3. Cuando se abra el editor nano, escriba el siguiente XML como contenido del archivo:

    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <workflow-app xmlns="uri:oozie:workflow:0.4" name="map-reduce-wf">
       <credentials>
          <credential name="metastore_token" type="hcat">
             <property>
                <name>hcat.metastore.uri</name>
                <value>thrift://<active-headnode-name>-<clustername>.<Domain>.com:9083</value>
             </property>
             <property>
                <name>hcat.metastore.principal</name>
                <value>hive/_HOST@<Domain>.COM</value>
             </property>
          </credential>
          <credential name="hs2-creds" type="hive2">
             <property>
                <name>hive2.server.principal</name>
                <value>${jdbcPrincipal}</value>
             </property>
             <property>
                <name>hive2.jdbc.url</name>
                <value>${jdbcURL}</value>
             </property>
          </credential>
       </credentials>
       <start to="mr-test" />
       <action name="mr-test">
          <map-reduce>
             <job-tracker>${jobTracker}</job-tracker>
             <name-node>${nameNode}</name-node>
             <prepare>
                <delete path="${nameNode}/user/${wf:user()}/examples/output-data/mrresult" />
             </prepare>
             <configuration>
                <property>
                   <name>mapred.job.queue.name</name>
                   <value>${queueName}</value>
                </property>
                <property>
                   <name>mapred.mapper.class</name>
                   <value>org.apache.oozie.example.SampleMapper</value>
                </property>
                <property>
                   <name>mapred.reducer.class</name>
                   <value>org.apache.oozie.example.SampleReducer</value>
                </property>
                <property>
                   <name>mapred.map.tasks</name>
                   <value>1</value>
                </property>
                <property>
                   <name>mapred.input.dir</name>
                   <value>/user/${wf:user()}/${examplesRoot}/input-data/text</value>
                </property>
                <property>
                   <name>mapred.output.dir</name>
                   <value>/user/${wf:user()}/${examplesRoot}/output-data/mrresult</value>
                </property>
             </configuration>
          </map-reduce>
          <ok to="myHive2" />
          <error to="fail" />
       </action>
       <action name="myHive2" cred="hs2-creds">
          <hive2 xmlns="uri:oozie:hive2-action:0.2">
             <job-tracker>${jobTracker}</job-tracker>
             <name-node>${nameNode}</name-node>
             <jdbc-url>${jdbcURL}</jdbc-url>
             <script>${hiveScript2}</script>
             <param>hiveOutputDirectory2=${hiveOutputDirectory2}</param>
          </hive2>
          <ok to="myHive" />
          <error to="fail" />
       </action>
       <action name="myHive" cred="metastore_token">
          <hive xmlns="uri:oozie:hive-action:0.2">
             <job-tracker>${jobTracker}</job-tracker>
             <name-node>${nameNode}</name-node>
             <configuration>
                <property>
                   <name>mapred.job.queue.name</name>
                   <value>${queueName}</value>
                </property>
             </configuration>
             <script>${hiveScript1}</script>
             <param>hiveOutputDirectory1=${hiveOutputDirectory1}</param>
          </hive>
          <ok to="end" />
          <error to="fail" />
       </action>
       <kill name="fail">
          <message>Oozie job failed, error message[${wf:errorMessage(wf:lastErrorNode())}]</message>
       </kill>
       <end name="end" />
    </workflow-app>
    ```

4. Reemplace `clustername` por el nombre del clúster.

5. Para guardar el archivo, seleccione **Ctrl+X**. Escriba **Y**. Luego seleccione **Entrar**.

    El flujo de trabajo se divide en dos partes:

   - **Credential.** Esta sección toma las credenciales que se usarán para autenticar las acciones de Oozie:

     En este ejemplo se usa la autenticación para las acciones de Hive. Para más información, consulte [Action Authentication](https://oozie.apache.org/docs/4.2.0/DG_ActionAuthentication.html) (Autenticación de acciones).

     El servicio de credenciales permite a las acciones de Oozie suplantar al usuario para obtener acceso a los servicios de Hadoop.

   - **Acción.** Esta sección tiene tres acciones: map-reduce, Hive server 2 y Hive server 1:

     - La acción map-reduce ejecuta un ejemplo desde un paquete de Oozie para asignación-reducción que genera el recuento de palabras agregado.

     - Las acciones Hive server 2 y Hive server 1 ejecutan una consulta en una tabla de Hive de ejemplo proporcionada con HDInsight.

     Las acciones Hive usan las credenciales definidas en la sección de credenciales para la autenticación mediante la palabra clave `cred` en el elemento action.

6. Use el comando siguiente para copiar el archivo `workflow.xml` en `/user/<domainuser>/examples/apps/map-reduce/workflow.xml`:

    ```bash
    hdfs dfs -put workflow.xml /user/<domainuser>/examples/apps/map-reduce/workflow.xml
    ```

7. Reemplace `domainuser` con el nombre de usuario para el dominio.

## <a name="define-the-properties-file-for-the-oozie-job"></a>Definición del archivo de propiedades para el trabajo de Oozie

1. Use la instrucción siguiente para crear y editar un archivo nuevo para las propiedades del trabajo:

    ```bash
    nano job.properties
    ```

2. Cuando se abra el editor nano, use el siguiente XML como contenido del archivo:

   ```bash
   nameNode=adl://home
   jobTracker=headnodehost:8050
   queueName=default
   examplesRoot=examples
   oozie.wf.application.path=${nameNode}/user/[domainuser]/examples/apps/map-reduce/workflow.xml
   hiveScript1=${nameNode}/user/${user.name}/countrowshive1.hql
   hiveScript2=${nameNode}/user/${user.name}/countrowshive2.hql
   oozie.use.system.libpath=true
   user.name=[domainuser]
   jdbcPrincipal=hive/<active-headnode-name>.<Domain>.com@<Domain>.COM
   jdbcURL=[jdbcurlvalue]
   hiveOutputDirectory1=${nameNode}/user/${user.name}/hiveresult1
   hiveOutputDirectory2=${nameNode}/user/${user.name}/hiveresult2
   ```

   - Use la URI `adl://home` para la propiedad `nameNode` si usa Azure Data Lake Storage Gen1 como almacenamiento de clúster principal. Si usa Azure Blob Storage, cámbiela a `wasb://home`. Si usa Azure Data Lake Storage Gen2, cámbiela a `abfs://home`.
   - Reemplace `domainuser` con el nombre de usuario para el dominio.  
   - Reemplace `ClusterShortName` por el nombre corto del clúster. Por ejemplo, si el nombre del clúster es https:// *[vínculo de ejemplo]* sechadoopcontoso.azurehdisnight.net, `clustershortname` son los seis primeros caracteres del clúster: **sechad**.  
   - Reemplace `jdbcurlvalue` con la dirección URL de JDBC de la configuración de Hive. Un ejemplo es jdbc:hive2://headnodehost:10001/;transportMode=http.
   - Para guardar el archivo, seleccione Ctrl+X, escriba `Y` y, a continuación, seleccione **Entrar**.

   Este archivo de propiedades tiene que estar presente localmente al ejecutar trabajos de Oozie.

## <a name="create-custom-hive-scripts-for-oozie-jobs"></a>Creación de scripts personalizados de Hive para trabajos de Oozie

Puede crear los dos scripts de Hive para Hive server 1 y Hive server 2 como se muestra en las secciones siguientes.

### <a name="hive-server-1-file"></a>Archivo de Hive server 1

1. Cree y edite un archivo para las acciones de tipo Hive server 1:

    ```bash
    nano countrowshive1.hql
    ```

2. Cree el script:

    ```sql
    INSERT OVERWRITE DIRECTORY '${hiveOutputDirectory1}'
    ROW FORMAT DELIMITED FIELDS TERMINATED BY ','
    select devicemake from hivesampletable limit 2;
    ```

3. Guarde el archivo en el Sistema de archivos distribuido de Apache Hadoop (HDFS):

    ```bash
    hdfs dfs -put countrowshive1.hql countrowshive1.hql
    ```

### <a name="hive-server-2-file"></a>Archivo de Hive server 2

1. Cree y edite un archivo para las acciones de tipo Hive server 2:

    ```bash
    nano countrowshive2.hql
    ```

2. Cree el script:

    ```sql
    INSERT OVERWRITE DIRECTORY '${hiveOutputDirectory1}' 
    ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' 
    select devicemodel from hivesampletable limit 2;
    ```

3. Guarde el archivo en HDFS:

    ```bash
    hdfs dfs -put countrowshive2.hql countrowshive2.hql
    ```

## <a name="submit-oozie-jobs"></a>Envío de trabajos de Oozie

El envío de trabajos de Oozie para clústeres ESP es como el envío de trabajos de Oozie en los clústeres no ESP.

Para obtener más información, vea [Uso de Apache Oozie con Apache Hadoop para definir y ejecutar un flujo de trabajo en Azure HDInsight basado en Linux](../hdinsight-use-oozie-linux-mac.md).

## <a name="results-from-an-oozie-job-submission"></a>Resultados de un envío de trabajos de Oozie

Los trabajos de Oozie se ejecutan para el usuario. Tanto los registros de auditoría YARN de Apache Hadoop como los de Apache Ranger muestran los trabajos que se ejecutan como el usuario suplantado. La salida de la interfaz de línea de comandos de un trabajo de Oozie es similar al código siguiente:

```output
Job ID : 0000015-180626011240801-oozie-oozi-W
------------------------------------------------------------------------------------------------
Workflow Name : map-reduce-wf
App Path      : adl://home/user/alicetest/examples/apps/map-reduce/wf.xml
Status        : SUCCEEDED
Run           : 0
User          : alicetest
Group         : -
Created       : 2018-06-26 19:25 GMT
Started       : 2018-06-26 19:25 GMT
Last Modified : 2018-06-26 19:30 GMT
Ended         : 2018-06-26 19:30 GMT
CoordAction ID: -

Actions
------------------------------------------------------------------------------------------------
ID                      Status  Ext ID          ExtStatus   ErrCode
------------------------------------------------------------------------------------------------
0000015-180626011240801-oozie-oozi-W@:start:    OK  -           OK      -
------------------------------------------------------------------------------------------------
0000015-180626011240801-oozie-oozi-W@mr-test    OK  job_1529975666160_0051  SUCCEEDED   -
------------------------------------------------------------------------------------------------
0000015-180626011240801-oozie-oozi-W@myHive2    OK  job_1529975666160_0053  SUCCEEDED   -
------------------------------------------------------------------------------------------------
0000015-180626011240801-oozie-oozi-W@myHive OK  job_1529975666160_0055  SUCCEEDED   -
------------------------------------------------------------------------------------------------
0000015-180626011240801-oozie-oozi-W@end    OK  -           OK      -
-----------------------------------------------------------------------------------------------
```

Los registros de auditoría de Ranger para las acciones Hive server 2 muestran a Oozie ejecutando la acción por el usuario. Las vistas de Ranger y YARN son visibles solamente para el administrador de clúster.

## <a name="configure-user-authorization-in-oozie"></a>Configuración de la autorización de usuario en Oozie

Oozie por sí solo tiene una configuración de autorización de usuario que puede impedir a los usuarios detener o eliminar trabajos de otros usuarios. Para habilitar esta configuración, establezca `oozie.service.AuthorizationService.security.enabled` en `true`. 

Para obtener más información, vea [Apache Oozie Installation and Configuration](https://oozie.apache.org/docs/3.2.0-incubating/AG_Install.html) (Instalación y configuración de Apache Oozie).

Para los componentes como Hive server 1, donde el complemento Ranger no está disponible o no es compatible, solo es posible usar la autorización HDFS general. La autorización más específica solo está disponible mediante los complementos Ranger.

## <a name="get-the-oozie-web-ui"></a>Obtención de la interfaz de usuario web de Oozie

La interfaz de usuario web de Oozie ofrece una vista basada en web en el estado de los trabajos de Oozie en el clúster. Para obtener la interfaz de usuario web, lleve a cabo los pasos siguientes en clústeres ESP:

1. Agregue un [nodo perimetral](../hdinsight-apps-use-edge-node.md) y habilite la [autenticación SSH Kerberos](../hdinsight-hadoop-linux-use-ssh-unix.md).

2. Siga los pasos en [Interfaz de usuario web de Oozie](../hdinsight-use-oozie-linux-mac.md) para habilitar la tunelización de SSH al nodo perimetral y tener acceso a la interfaz de usuario web.

## <a name="next-steps"></a>Pasos siguientes

- [Uso de Apache Oozie con Apache Hadoop para definir y ejecutar un flujo de trabajo en Azure HDInsight basado en Linux](../hdinsight-use-oozie-linux-mac.md).
- [Conexión a HDInsight (Apache Hadoop) con SSH](../hdinsight-hadoop-linux-use-ssh-unix.md#authentication-domain-joined-hdinsight).

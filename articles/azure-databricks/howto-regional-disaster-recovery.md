---
title: Recuperación ante desastres regional para Azure Databricks
description: En este artículo se describe un enfoque para realizar la recuperación ante desastres en Azure Databricks.
services: azure-databricks
author: mamccrea
ms.author: mamccrea
ms.service: azure-databricks
ms.workload: big-data
ms.topic: conceptual
ms.date: 03/13/2019
ms.openlocfilehash: 2604d5b357feacce3493b4a4ded971144262611d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "77161943"
---
# <a name="regional-disaster-recovery-for-azure-databricks-clusters"></a>Recuperación ante desastres regional para clústeres de Azure Databricks

En este artículo se describe una arquitectura de recuperación ante desastres útil para los clústeres de Azure Databricks y los pasos necesarios para lograr ese diseño.

## <a name="azure-databricks-architecture"></a>Arquitectura de Azure Databricks

En un nivel alto, al crear un área de trabajo de Azure Databricks desde Azure Portal, se implementa un [dispositivo administrado](../azure-resource-manager/managed-applications/overview.md) como un recurso de Azure en su suscripción, en la región de Azure seleccionada (por ejemplo, Oeste de EE. UU.). Esta aplicación se implementa en una instancia de [Azure Virtual Network](../virtual-network/virtual-networks-overview.md) con un [grupo de seguridad de red](../virtual-network/manage-network-security-group.md) y una cuenta de Azure Storage, disponible en su suscripción. La red virtual proporciona seguridad a nivel de perímetro para el área de trabajo de Databricks y está protegida a través del grupo de seguridad de red. En el área de trabajo, puede crear clústeres de Databricks proporcionando el trabajo y tipo de máquina virtual de controlador y versión del runtime de Databricks. Los datos persistentes están disponibles en la cuenta de almacenamiento, que puede ser de Azure Blob Storage o Azure Data Lake Storage. Una vez creado el clúster, puede ejecutar trabajos a través de puntos de conexión ODBC/JDBC, blocs de notas, las API REST, adjuntándolos a un clúster concreto.

El plano de control de Databricks administra y supervisa el entorno de área de trabajo de Databricks. Cualquier operación de administración, como crear un clúster, se iniciará desde el plano de control. Todos los metadatos, como los trabajos programados, se almacenan en una base de datos de Azure con replicación geográfica para la tolerancia a errores.

![Arquitectura de Databricks](media/howto-regional-disaster-recovery/databricks-architecture.png)

Una de las ventajas de esta arquitectura es que los usuarios pueden conectar Azure Databricks a cualquier recurso de almacenamiento en su cuenta. Una ventaja clave es que proceso (Azure Databricks) y almacenamiento se pueden escalar independientemente uno del otro.

## <a name="how-to-create-a-regional-disaster-recovery-topology"></a>Cómo crear una topología de recuperación ante desastres regionales

Como se observa en la descripción de la arquitectura anterior, hay una serie de componentes utilizados para una canalización de macrodatos con Azure Databricks: Azure Storage, Azure Database y otros orígenes de datos. Azure Databricks es el *proceso* para la canalización de macrodatos. Es *efímero* por naturaleza, lo que significa que mientras los datos siguen estando disponibles en Azure Storage, el *proceso* (clúster de Azure Databricks) se puede finalizar para que no tenga que pagar por proceso cuando no lo necesita. El *proceso* (Azure Databricks) y los orígenes de almacenamiento tienen que estar en la misma región para que los trabajos no sufran una latencia elevada.  

Para crear su propia topología de recuperación ante desastres regional, siga estos requisitos:

   1. Aprovisione varias áreas de trabajo de Azure Databricks en distintas regiones de Azure. Por ejemplo, puede crear el área de trabajo principal de Azure Databricks en Este de EE. UU. 2. Cree el área de trabajo secundaria de recuperación ante desastres de Azure Databricks en una región distinta, por ejemplo, Oeste de EE. UU.

   2. Use el [almacenamiento con redundancia geográfica](../storage/common/storage-redundancy.md). Los datos asociados a Azure Databricks se almacenan de forma predeterminada en Azure Storage. Los resultados de un trabajo de Databricks también se almacenan en Azure Blob Storage, con el fin de que los datos procesados sean duraderos y tengan una alta disponibilidad después de que el clúster se finalice. Como el clúster de Databricks y de Storage tienen la misma ubicación, tiene que usar almacenamiento con redundancia geográfica para poder obtener acceso a los datos en una región secundaria si la región primaria ya no es accesible.

   3. Una vez creada la región secundaria, tiene que migrar los usuarios, las carpetas de usuario, blocs de notas, configuración de clúster, configuración de trabajos, bibliotecas, almacenamiento, scripts de init y volver a configurar el control de acceso. En la siguiente sección se describen detalles adicionales.

## <a name="detailed-migration-steps"></a>Pasos detallados de la migración

1. **Configuración de la interfaz de línea de comandos de Databricks en el equipo**

   En este artículo se muestra una serie de ejemplos de código que usan la interfaz de línea de comandos para la mayoría de los pasos automatizados, ya que se trata de un contenedor fácil de usar de la API REST de Azure Databricks.

   Antes de realizar los pasos de migración, instale la CLI de Databricks en el equipo de escritorio o en una máquina virtual en donde va a realizar el trabajo. Para más información, consulte [Instalación de la CLI de Databricks](/azure/databricks/dev-tools/databricks-cli)

   ```bash
   pip install databricks-cli
   ```

   > [!NOTE]
   > Los scripts de Python proporcionados en este artículo se espera que funcione con Python 2.7 + < 3.x.

2. **Configuración de dos perfiles.**

   Configure uno para el área de trabajo principal y otro para el área de trabajo secundaria:

   ```bash
   databricks configure --profile primary
   databricks configure --profile secondary
   ```

   Los bloques de código de este artículo cambian entre los perfiles en cada paso mediante el comando correspondiente del área de trabajo. Asegúrese de que los nombres de los perfiles creados se sustituyen en cada bloque de código.

   ```python
   EXPORT_PROFILE = "primary"
   IMPORT_PROFILE = "secondary"
   ```

   Puede cambiar manualmente en la línea de comandos si es necesario:

   ```bash
   databricks workspace ls --profile primary
   databricks workspace ls --profile secondary
   ```

3. **Migración de usuarios de Azure Active Directory**

   Agregue manualmente los mismos usuarios de Azure Active Directory que existen en el área de trabajo principal al área de trabajo secundaria.

4. **Migración de las carpetas de usuario y los blocs de notas**

   Use el siguiente código de Python para migrar los entornos de usuario de espacio aislado, que incluyen la estructura de carpetas anidadas y blocks de notas por usuario.

   > [!NOTE]
   > Las bibliotecas no se copian en este paso, ya que la API subyacente no es compatible con ellas.

   Copie y guarde el siguiente script de Python en un archivo y ejecútelo en la línea de comandos de Databricks. Por ejemplo, `python scriptname.py`.

   ```python
   from subprocess import call, check_output

   EXPORT_PROFILE = "primary"
   IMPORT_PROFILE = "secondary"

   # Get a list of all users
   user_list_out = check_output(["databricks", "workspace", "ls", "/Users", "--profile", EXPORT_PROFILE])
   user_list = user_list_out.splitlines()

   # Export sandboxed environment (folders, notebooks) for each user and import into new workspace.
   # Libraries are not included with these APIs / commands.

   for user in user_list:
     print "Trying to migrate workspace for user " + user

     call("mkdir -p " + user, shell=True)
     export_exit_status = call("databricks workspace export_dir /Users/" + user + " ./" + user + " --profile " + EXPORT_PROFILE, shell=True)

     if export_exit_status==0:
       print "Export Success"
       import_exit_status = call("databricks workspace import_dir ./" + user + " /Users/" + user + " --profile " + IMPORT_PROFILE, shell=True)
       if import_exit_status==0:
         print "Import Success"
       else:
         print "Import Failure"
     else:
       print "Export Failure"

   print "All done"
   ```

5. **Migración de configuraciones de clúster**

   Una vez que se han migrado los blocs de notas, tiene la opción de migrar las configuraciones de clúster a la nueva área de trabajo. Se trata de un paso casi completamente automatizado utilizando la CLI de Databricks, excepto si desea realizar una migración selectiva de configuración de clúster, en lugar de una para todos.

   > [!NOTE]
   > Lamentablemente, no hay ningún punto de conexión de configuración de creación de clúster, y este script intenta crear cada clúster de forma inmediata. Si no hay suficientes núcleos disponibles en su suscripción, se puede producir un error en la creación del clúster. El error se puede ignorar, siempre y cuando la configuración se haya transferido correctamente.

   El siguiente script proporcionado imprime una asignación de los identificadores de clúster antiguos a los nuevos, que podrían usarse para la migración de trabajo más adelante (para los trabajos que están configurados para usar clústeres existentes).

   Copie y guarde el siguiente script de Python en un archivo y ejecútelo en la línea de comandos de Databricks. Por ejemplo, `python scriptname.py`.

   ```python
   from subprocess import call, check_output
   import json, os

   EXPORT_PROFILE = "primary"
   IMPORT_PROFILE = "secondary"

   # Get all clusters info from old workspace
   clusters_out = check_output(["databricks", "clusters", "list", "--profile", EXPORT_PROFILE])
   clusters_info_list = clusters_out.splitlines()

   # Create a list of all cluster ids
   clusters_list = []
   ##for cluster_info in clusters_info_list: clusters_list.append(cluster_info.split(None, 1)[0])

   for cluster_info in clusters_info_list: 
      if cluster_info != '':
         clusters_list.append(cluster_info.split(None, 1)[0])

   # Optionally filter cluster ids out manually, so as to create only required ones in new workspace

   # Create a list of mandatory / optional create request elements
   cluster_req_elems = ["num_workers","autoscale","cluster_name","spark_version","spark_conf","node_type_id","driver_node_type_id","custom_tags","cluster_log_conf","spark_env_vars","autotermination_minutes","enable_elastic_disk"]

   print(str(len(clusters_list)) + " clusters found in the primary site" )

   print ("---------------------------------------------------------")
   # Try creating all / selected clusters in new workspace with same config as in old one.
   cluster_old_new_mappings = {}
   i = 0
   for cluster in clusters_list:
      i += 1
      print("Checking cluster " + str(i) + "/" + str(len(clusters_list)) + " : " + cluster)
      cluster_get_out = check_output(["databricks", "clusters", "get", "--cluster-id", cluster, "--profile", EXPORT_PROFILE])
      print ("Got cluster config from old workspace")

       # Remove extra content from the config, as we need to build create request with allowed elements only
      cluster_req_json = json.loads(cluster_get_out)
      cluster_json_keys = cluster_req_json.keys()

      #Don't migrate Job clusters
      if cluster_req_json['cluster_source'] == u'JOB' : 
         print ("Skipping this cluster as it is a Job cluster : " + cluster_req_json['cluster_id'] )
         print ("---------------------------------------------------------")
         continue

      for key in cluster_json_keys:
         if key not in cluster_req_elems:
            cluster_req_json.pop(key, None)

      # Create the cluster, and store the mapping from old to new cluster ids

      #Create a temp file to store the current cluster info as JSON
      strCurrentClusterFile = "tmp_cluster_info.json" 

      #delete the temp file if exists
      if os.path.exists(strCurrentClusterFile) : 
         os.remove(strCurrentClusterFile)

      fClusterJSONtmp = open(strCurrentClusterFile,"w+")
      fClusterJSONtmp.write(json.dumps(cluster_req_json))
      fClusterJSONtmp.close()

      #cluster_create_out = check_output(["databricks", "clusters", "create", "--json", json.dumps(cluster_req_json), "--profile", IMPORT_PROFILE])
      cluster_create_out = check_output(["databricks", "clusters", "create", "--json-file", strCurrentClusterFile , "--profile", IMPORT_PROFILE])
      cluster_create_out_json = json.loads(cluster_create_out)
      cluster_old_new_mappings[cluster] = cluster_create_out_json['cluster_id']

      print ("Cluster create request sent to secondary site workspace successfully")
      print ("---------------------------------------------------------")

      #delete the temp file if exists
      if os.path.exists(strCurrentClusterFile) : 
         os.remove(strCurrentClusterFile)

   print ("Cluster mappings: " + json.dumps(cluster_old_new_mappings))
   print ("All done")
   print ("P.S. : Please note that all the new clusters in your secondary site are being started now!")
   print ("       If you won't use those new clusters at the moment, please don't forget terminating your new clusters to avoid charges")
   ```

6. **Migración de la configuración de trabajos**

   Si ha migrado las configuraciones de clúster en el paso anterior, puede optar a migrar las configuraciones de trabajo a la nueva área de trabajo. Se trata de un paso completamente automatizado utilizando la CLI de Databricks, excepto si desea realizar una migración selectiva de configuración de trabajo, en lugar de una para todos los trabajos.

   > [!NOTE]
   > La configuración para un trabajo programado contiene también la información de "programación", por lo que de forma predeterminada empezará a trabajar según el tiempo configurado en cuanto se haya migrado. Por lo tanto, el bloque de código siguiente quita cualquier información de programación durante la migración (para evitar ejecuciones duplicadas en áreas de trabajo antiguas y nuevas). Configure las programaciones de esos trabajos una vez que esté listo para la migración.

   La configuración del trabajo requiere una configuración para un clúster nuevo o uno ya existente. Si usa el clúster existente, el script o código a continuación intentará reemplazar el antiguo identificador del clúster con el nuevo.

   Copie y guarde el siguiente script de Python en un archivo. Reemplace el valor de `old_cluster_id` y `new_cluster_id` con el resultado de migración de clúster realizada en el paso anterior. Ejecútelo en la línea de comandos de la CLI de Databricks, por ejemplo, `python scriptname.py`.

   ```python
   from subprocess import call, check_output
   import json

   EXPORT_PROFILE = "primary"
   IMPORT_PROFILE = "secondary"

   # Please replace the old to new cluster id mappings from cluster migration output
   cluster_old_new_mappings = {"old_cluster_id": "new_cluster_id"}

   # Get all jobs info from old workspace
   try:
     jobs_out = check_output(["databricks", "jobs", "list", "--profile", EXPORT_PROFILE])
     jobs_info_list = jobs_out.splitlines()
   except:
     print "No jobs to migrate"
     sys.exit(0)

   # Create a list of all job ids
   jobs_list = []
   for jobs_info in jobs_info_list:
     jobs_list.append(jobs_info.split(None, 1)[0])

   # Optionally filter job ids out manually, so as to create only required ones in new workspace

   # Create each job in the new workspace based on corresponding settings in the old workspace

   for job in jobs_list:
     print "Trying to migrate " + job

     job_get_out = check_output(["databricks", "jobs", "get", "--job-id", job, "--profile", EXPORT_PROFILE])
     print "Got job config from old workspace"

     job_req_json = json.loads(job_get_out)  
     job_req_settings_json = job_req_json['settings']

     # Remove schedule information so job doesn't start before proper cutover
     job_req_settings_json.pop('schedule', None)

     # Replace old cluster id with new cluster id, if job configured to run against an existing cluster
     if 'existing_cluster_id' in job_req_settings_json:
       if job_req_settings_json['existing_cluster_id'] in cluster_old_new_mappings:
         job_req_settings_json['existing_cluster_id'] = cluster_old_new_mappings[job_req_settings_json['existing_cluster_id']]
       else:
         print "Mapping not available for old cluster id " + job_req_settings_json['existing_cluster_id']
         continue

     call(["databricks", "jobs", "create", "--json", json.dumps(job_req_settings_json), "--profile", IMPORT_PROFILE])
     print "Sent job create request to new workspace successfully"

   print "All done"
   ```

7. **Migración de bibliotecas**

   Actualmente no hay ninguna manera sencilla de migrar las bibliotecas de un área de trabajo a otra. Vuelva a instalar esas bibliotecas en la nueva área de trabajo manualmente. Es posible automatizar usando la combinación de la [CLI de DBFS](https://github.com/databricks/databricks-cli#dbfs-cli-examples) para cargar bibliotecas personalizadas en el área de trabajo y la [CLI de bibliotecas](https://github.com/databricks/databricks-cli#libraries-cli).

8. **Migración de montajes de Azure Blob Storage y Azure Data Lake Storage**

   Vuelva a montar manualmente todos los puntos de montaje de [Azure Blob Storage](/azure/databricks/data/data-sources/azure/azure-storage) y [Azure Data Lake Storage (Gen 2)](/azure/databricks/data/data-sources/azure/azure-datalake-gen2) mediante una solución basada en cuaderno. Los recursos de almacenamiento se habrían montado en el área de trabajo principal, y esto mismo tiene que repetirse en el área de trabajo secundario. No hay ninguna API externa para montajes.

9. **Migración de los scripts de inicialización de clúster**

   Los scripts de inicialización de clúster se pueden migrar desde la antigua a la nueva área de trabajo mediante la [CLI de DBFS](https://github.com/databricks/databricks-cli#dbfs-cli-examples). Primero copie los scripts necesarios de `dbfs:/dat abricks/init/..` en el escritorio local o máquina virtual. A continuación, copie esos scripts en la nueva área de trabajo en la misma ruta de acceso.

   ```bash
   // Primary to local
   dbfs cp -r dbfs:/databricks/init ./old-ws-init-scripts --profile primary

   // Local to Secondary workspace
   dbfs cp -r old-ws-init-scripts dbfs:/databricks/init --profile secondary
   ```

10. **Reconfiguración y reaplicación manual del control de acceso.**

    Si el área de trabajo principal existente está configurada para usar el nivel Premium (SKU), es probable que también esté usando la [característica Access Control](/azure/databricks/administration-guide/access-control/index).

    Si usa la característica Access Control, vuelva a aplicar manualmente el control de acceso a los recursos (block de notas, clústeres, trabajos, tablas).

## <a name="disaster-recovery-for-your-azure-ecosystem"></a>Recuperación ante desastres del ecosistema de Azure

Si usa otros servicios de Azure, asegúrese de implementar los procedimientos recomendados de la recuperación ante desastres también para ellos. Por ejemplo, si elige usar una instancia de Hive Metastore externa, debe tener en cuenta la recuperación ante desastres de [Azure SQL Server](../sql-database/sql-database-disaster-recovery.md), [Azure HDInsight](../hdinsight/hdinsight-high-availability-linux.md) o [Azure Database for MySQL ](../mysql/concepts-business-continuity.md). Para obtener información general acerca de la recuperación ante desastres, consulte [Error y recuperación ante desastres para aplicaciones de Azure](https://docs.microsoft.com/azure/architecture/resiliency/disaster-recovery-azure-applications).

## <a name="next-steps"></a>Pasos siguientes

Para más información, consulte la [documentación de Azure Databricks](index.yml).
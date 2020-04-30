---
title: Solución de problemas de acciones de script en Azure HDInsight
description: Pasos para la solución de problemas generales de acciones de script en Azure HDInsight.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: troubleshooting
ms.custom: seoapr2020
ms.date: 04/21/2020
ms.openlocfilehash: e2a2f6abfd6b7c644e95649f3c9832e4cc986037
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/28/2020
ms.locfileid: "82188453"
---
# <a name="troubleshoot-script-actions-in-azure-hdinsight"></a>Solución de problemas de acciones de script en Azure HDInsight

En este artículo se describen los pasos de solución de problemas y las posibles soluciones para los problemas que se producen al usar clústeres de Azure HDInsight.

## <a name="viewing-logs"></a>Visualización de los registros

Puede usar la interfaz de usuario web de Apache Ambari para ver la información registrada por las acciones de script. Si se produce un error en el script durante la creación, los registros se encuentran en la cuenta de almacenamiento predeterminada del clúster. Esta sección proporciona información sobre cómo recuperar los registros mediante ambas opciones.

### <a name="apache-ambari-web-ui"></a>Interfaz de usuario web de Apache Ambari

1. En un explorador web, vaya a `https://CLUSTERNAME.azurehdinsight.net`, donde `CLUSTERNAME` es el nombre del clúster.

1. En la barra de la parte superior de la página, seleccione la entrada **ops**. Una lista muestra las operaciones actuales y anteriores realizadas en el clúster mediante Ambari.

    ![Barra de interfaz de usuario web de Ambari con ops seleccionado](./media/troubleshoot-script-action/hdi-apache-ambari-nav.png)

1. Busque las entradas que tienen **run\_customscriptaction** en la columna **Operaciones**. Estas entradas se crean al ejecutarse las acciones de script.

    ![Operaciones de acción de script de Apache Ambari](./media/troubleshoot-script-action/ambari-script-action.png)

    Para ver la salida **STDOUT** y **STDERR**, seleccione la entrada **run\customscriptaction** y profundice mediante los vínculos. Esta salida se genera cuando se ejecuta el script y puede contener información útil.

### <a name="default-storage-account"></a>Cuenta de almacenamiento predeterminada

Si se produce un error al crear el clúster debido a un error de script, los registros permanecen en la cuenta de almacenamiento del clúster.

* Los registros de almacenamiento están disponibles en `\STORAGE_ACCOUNT_NAME\DEFAULT_CONTAINER_NAME\custom-scriptaction-logs\CLUSTER_NAME\DATE`.

    ![Registros de acción de script](./media/troubleshoot-script-action/script-action-logs-in-storage.png)

    En este directorio, los registros se organizan por separado para el **nodo principal**, el **nodo de trabajo** y el **nodo de Zookeeper**. Consulte los siguientes ejemplos:

    * **Nodo principal**: `<ACTIVE-HEADNODE-NAME>.cloudapp.net`

    * **Nodo de trabajo**: `<ACTIVE-WORKERNODE-NAME>.cloudapp.net`

    * **Nodo de Zookeeper**: `<ACTIVE-ZOOKEEPERNODE-NAME>.cloudapp.net`

* Todos los **stdout** y **stderr** del host correspondiente se cargan en la cuenta de almacenamiento. Hay un archivo **output-\*.txt** y **errors-\*.txt** para cada acción de script. El archivo **output-*.txt** contiene información sobre el identificador URI del script que se ejecutó en el host. El siguiente texto es un ejemplo de esta información:

        'Start downloading script locally: ', u'https://hdiconfigactions.blob.core.windows.net/linuxrconfigactionv01/r-installer-v01.sh'

* Es posible crear repetidamente un clúster de la acción de script con el mismo nombre. En tal caso, puede distinguir los registros pertinentes según el nombre de la carpeta de **fecha**. Por ejemplo, la estructura de carpetas de un clúster (**mycluster**) creado en diferentes fechas es similar a las siguientes entradas de registro:

    `\STORAGE_ACCOUNT_NAME\DEFAULT_CONTAINER_NAME\custom-scriptaction-logs\mycluster\2015-10-04` `\STORAGE_ACCOUNT_NAME\DEFAULT_CONTAINER_NAME\custom-scriptaction-logs\mycluster\2015-10-05`

* Si crea un clúster de acción de script con el mismo nombre en el mismo día, puede usar el prefijo único para identificar los archivos de registro correspondientes.

* Si crea un clúster cerca de las 12:00 a. m. (medianoche), es posible que los archivos de registro abarquen dos días. En tal caso, verá dos carpetas de fecha diferentes para el mismo clúster.

* La carga de los archivos de registro en el contenedor predeterminado puede tardar hasta 5 minutos, especialmente para grandes clústeres. Por lo tanto, si desea tener acceso a los registros, no debe eliminar inmediatamente el clúster si se produce un error en una acción de script.

## <a name="ambari-watchdog"></a>Guardián Ambari

No cambie la contraseña del guardián Ambari (hdinsightwatchdog) en el clúster de HDInsight basado en Linux. Un cambio de la contraseña impide ejecutar nuevas acciones de script en el clúster de HDInsight.

## <a name="cant-import-name-blobservice"></a>No se puede importar el nombre BlobService

__Síntomas__. se produce un error en la acción del script. Se muestra un texto similar al error siguiente al ver la operación en Ambari:

```
Traceback (most recent call list):
  File "/var/lib/ambari-agent/cache/custom_actions/scripts/run_customscriptaction.py", line 21, in <module>
    from azure.storage.blob import BlobService
ImportError: cannot import name BlobService
```

__Causa__. Este error se produce si actualiza el cliente de Azure Storage de Python que se incluye con el clúster de HDInsight. HDInsight espera el cliente de Azure Storage 0.20.0.

__Resolución__. Para resolver este error, conéctese manualmente a cada nodo del clúster mediante `ssh`. Ejecute el siguiente comando para volver a instalar la versión del cliente de almacenamiento correcta:

```bash
sudo pip install azure-storage==0.20.0
```

Para obtener información acerca de cómo conectarse al clúster con SSH, consulte [Conexión a HDInsight (Apache Hadoop) con SSH](hdinsight-hadoop-linux-use-ssh-unix.md).

## <a name="history-doesnt-show-the-scripts-used-during-cluster-creation"></a>El historial no muestra los scripts usados durante la creación del clúster

Si el clúster se creó antes del 15 de marzo de 2016, puede que no vea una entrada en el historial de acciones de script. Cambiar el tamaño del clúster hace que los scripts aparezcan en el historial de acciones de script.

Hay dos excepciones:

* El clúster se creó antes del 1 de septiembre de 2015. Esta es la fecha en que se presentaron las acciones de script. Cualquier clúster creado con anterioridad a esta fecha no ha podido usar acciones de script para su creación.

* Durante la creación del clúster, ha usado varias acciones de script o bien ha utilizado el mismo nombre para varios scripts, o el mismo nombre y el mismo identificador URI, pero diferentes parámetros para varios scripts. En estos casos, aparece el siguiente error:

    ```
    No new script actions can be run on this cluster because of conflicting script names in existing scripts. Script names provided at cluster creation must be all unique. Existing scripts are run on resize.
    ```

## <a name="next-steps"></a>Pasos siguientes

Si su problema no aparece o es incapaz de resolverlo, visite uno de nuestros canales para obtener ayuda adicional:

* Obtenga respuestas de expertos de Azure mediante el [soporte técnico de la comunidad de Azure](https://azure.microsoft.com/support/community/).

* Póngase en contacto con [@AzureSupport](https://twitter.com/azuresupport), la cuenta oficial de Microsoft Azure para mejorar la experiencia del cliente. Esta cuenta pone en contacto a la comunidad de Azure con los recursos adecuados: respuestas, soporte técnico y expertos.

* Si necesita más ayuda, puede enviar una solicitud de soporte técnico desde [Azure Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/). Seleccione **Soporte técnico** en la barra de menús o abra la central **Ayuda + soporte técnico**. Para obtener información más detallada, revise [Creación de una solicitud de soporte técnico de Azure](https://docs.microsoft.com/azure/azure-portal/supportability/how-to-create-azure-support-request). La suscripción a Microsoft Azure incluye acceso al soporte técnico para facturación y administración de suscripciones. El soporte técnico se proporciona a través de uno de los [planes de soporte técnico de Azure](https://azure.microsoft.com/support/plans/).
---
title: Solución de problemas de Apache Oozie en Azure HDInsight
description: Solución de determinados errores de Apache Oozie en Azure HDInsight.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: troubleshooting
ms.date: 04/27/2020
ms.openlocfilehash: fb795a9d7100019b2b1820c592f87025b77f5878
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "86045865"
---
# <a name="troubleshoot-apache-oozie-in-azure-hdinsight"></a>Solución de problemas de Apache Oozie en Azure HDInsight

Con la interfaz de usuario de Oozie, puede ver los registros de Oozie. La IU de Oozie también contiene vínculos a registros de JobTracker para tareas de MapReduce iniciadas por el flujo de trabajo. El patrón para la solución de problemas debe ser el siguiente:

1. Ver el trabajo en la interfaz de usuario web de Oozie.

2. Si hay un error o falla en una acción determinada, seleccione la acción para ver si el campo **Mensaje de error** ofrece más información sobre el error.

3. Si está disponible, use la dirección URL de la acción para ver más detalles (por ejemplo, registros de JobTracker) para la acción.

Los siguientes son errores específicos que pueden surgir, así como el procedimiento para resolverlos.

## <a name="ja009-cant-initialize-cluster"></a>JA009: no se puede inicializar el clúster

### <a name="issue"></a>Problema

el estado del trabajo cambia a **SUSPENDED** (SUSPENDIDO). Los detalles del trabajo mostrarán el estado de `RunHiveScript` como **START_MANUAL**. Si se selecciona la acción, aparecerá el mensaje de error siguiente:

```output
JA009: Cannot initialize Cluster. Please check your configuration for map
```

### <a name="cause"></a>Causa

las direcciones de Azure Blob Storage usadas en el archivo **job.xml** no incluyen el nombre del contenedor de almacenamiento ni de la cuenta de almacenamiento. El formato de dirección de Blob Storage debe ser `wasbs://containername@storageaccountname.blob.core.windows.net`.

### <a name="resolution"></a>Solución

cambiar las direcciones de Blob Storage que usa el trabajo.

---

## <a name="ja002-oozie-isnt-allowed-to-impersonate-ltusergt"></a>JA002: Oozie no tiene permiso para suplantar a &lt;USER&gt;

### <a name="issue"></a>Problema

el estado del trabajo cambia a **SUSPENDED** (SUSPENDIDO). Los detalles del trabajo mostrarán el estado de `RunHiveScript` como **START_MANUAL**. Al seleccionar la acción, se muestra el mensaje de error siguiente:

```output
JA002: User: oozie is not allowed to impersonate <USER>
```

### <a name="cause"></a>Causa

la configuración actual de permisos no permite a Oozie suplantar la cuenta de usuario especificada.

### <a name="resolution"></a>Solución

Oozie puede suplantar a los usuarios del grupo **`users`** . Utilice el `groups USERNAME` para ver los grupos de los que es miembro la cuenta de usuario. Si el usuario no es miembro del grupo **`users`** , use el siguiente comando para agregar el usuario al grupo:

```bash
sudo adduser USERNAME users
```

> [!NOTE]  
> Puede tardar varios minutos antes de que HDInsight reconozca que el usuario se ha agregado al grupo.

---

## <a name="launcher-error-sqoop"></a>ERROR del selector (Sqoop)

### <a name="issue"></a>Problema

el estado del trabajo cambia a **KILLED** (TERMINADO). Los detalles del trabajo mostrarán el estado de `RunSqoopExport` como **ERROR**. Al seleccionar la acción, se muestra el mensaje de error siguiente:

```output
Launcher ERROR, reason: Main class [org.apache.oozie.action.hadoop.SqoopMain], exit code [1]
```

### <a name="cause"></a>Causa

Sqoop no puede cargar el controlador de base de datos necesario para acceder a la base de datos.

### <a name="resolution"></a>Solución

al usar Sqoop desde un trabajo de Oozie, debe incluir el controlador de base de datos con los demás recursos (por ejemplo, workflow.xml) que usa el trabajo. Asimismo, haga referencia al archivo que contiene el controlador de base de datos desde la sección `<sqoop>...</sqoop>` de workflow.xml.

Por ejemplo, para el ejemplo de trabajo del artículo [Uso de flujos de trabajo de Oozie de Hadoop](hdinsight-use-oozie-linux-mac.md), se usarían los pasos siguientes:

1. Copie el archivo `mssql-jdbc-7.0.0.jre8.jar` en el directorio **/tutorials/useoozie**:

    ```bash
    hdfs dfs -put /usr/share/java/sqljdbc_7.0/enu/mssql-jdbc-7.0.0.jre8.jar /tutorials/useoozie/mssql-jdbc-7.0.0.jre8.jar
    ```

2. Modifique `workflow.xml` para agregar el siguiente XML en una nueva línea encima de `</sqoop>`:

    ```xml
    <archive>mssql-jdbc-7.0.0.jre8.jar</archive>
    ```

## <a name="next-steps"></a>Pasos siguientes

Si su problema no aparece o es incapaz de resolverlo, visite uno de nuestros canales para obtener ayuda adicional:

* Obtenga respuestas de expertos de Azure mediante el [soporte técnico de la comunidad de Azure](https://azure.microsoft.com/support/community/).

* Póngase en contacto con [@AzureSupport](https://twitter.com/azuresupport), la cuenta oficial de Microsoft Azure para mejorar la experiencia del cliente. Esta cuenta pone en contacto a la comunidad de Azure con los recursos adecuados: respuestas, soporte técnico y expertos.

* Si necesita más ayuda, puede enviar una solicitud de soporte técnico desde [Azure Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/). Seleccione **Soporte técnico** en la barra de menús o abra la central **Ayuda + soporte técnico**. Para obtener información más detallada, revise [Creación de una solicitud de soporte técnico de Azure](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request). La suscripción a Microsoft Azure incluye acceso al soporte técnico para facturación y administración de suscripciones. El soporte técnico se proporciona a través de uno de los [planes de soporte técnico de Azure](https://azure.microsoft.com/support/plans/).

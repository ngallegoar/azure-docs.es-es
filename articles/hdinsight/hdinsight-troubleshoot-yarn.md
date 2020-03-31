---
title: Solución de problemas de YARN en Azure HDInsight
description: Obtenga respuestas a las preguntas más comunes sobre cómo trabajar con Apache Hadoop YARN y Azure HDInsight.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: troubleshooting
ms.date: 08/15/2019
ms.openlocfilehash: f0c7b966b9fa7580809d2df0f4d05a7146ca0fd1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "79233540"
---
# <a name="troubleshoot-apache-hadoop-yarn-by-using-azure-hdinsight"></a>Solucione problemas de YARN de Apache Hadoop con Azure HDInsight.

Obtenga información sobre los principales problemas y sus soluciones al trabajar con cargas útiles de Apache Hadoop YARN en Apache Ambari.

## <a name="how-do-i-create-a-new-yarn-queue-on-a-cluster"></a>¿Cómo se crea una nueva cola de YARN en un clúster?

### <a name="resolution-steps"></a>Pasos de resolución

Para crear una nueva cola de YARN y equilibrar la asignación de capacidad en todas las colas, siga estos pasos en Ambari.

En este ejemplo, se cambia la capacidad de las dos colas existentes (**default** (predeterminado) y **thriftsvr**) del 50% al 25%, lo que proporciona una capacidad del 50% a la nueva cola (spark).

| Cola | Capacity | Capacidad máxima |
| --- | --- | --- |
| default | 25 % | 50 % |
| thrftsvr | 25 % | 50 % |
| spark | 50 % | 50 % |

1. Seleccione el icono **Ambari Views** (Vistas de Ambari) y, a continuación, seleccione el patrón de cuadrícula. A continuación, seleccione **YARN Queue Manager** (Administrador de colas de YARN).

    ![Administrador de colas de YARN del panel de Apache Ambari](media/hdinsight-troubleshoot-yarn/apache-yarn-create-queue-1.png)
2. Seleccione la cola **default**.

    ![YARN de Apache Ambari: seleccionar cola predeterminada](media/hdinsight-troubleshoot-yarn/apache-yarn-create-queue-2.png)
3. Para la cola **default**, cambie la **capacidad** del 50% al 25%. Para la cola **thriftsvr**, cambie la **capacidad** al 25%.

    ![Cambio de la capacidad al 25 % para las colas default y thriftsvr](media/hdinsight-troubleshoot-yarn/apache-yarn-create-queue-3.png)
4. Seleccione **Add Queue** (Agregar cola) para crear una nueva cola.

    ![Panel de YARN de Apache Ambari: agregar cola](media/hdinsight-troubleshoot-yarn/apache-yarn-create-queue-4.png)

5. Asigne un nombre a la cola nueva.

    ![Panel de YARN de Apache Ambari: nombre de la cola](media/hdinsight-troubleshoot-yarn/apache-yarn-create-queue-5.png)  

6. Deje los valores de **Capacity** (Capacidad) en el 50 % y seleccione el botón **Actions** (Acciones).

    ![YARN de Apache Ambari: seleccionar acción](media/hdinsight-troubleshoot-yarn/apache-yarn-create-queue-6.png)  
7. Seleccione **Save and Refresh Queues** (Guardar y actualizar colas).

    ![Selección de Save and Refresh Queues (Guardar y actualizar colas)](media/hdinsight-troubleshoot-yarn/apache-yarn-create-queue-7.png)  

Estos cambios están visibles inmediatamente en la interfaz de usuario de YARN Scheduler.

### <a name="additional-reading"></a>Lecturas adicionales

- [YARN CapacityScheduler de Apache Hadoop](https://hadoop.apache.org/docs/r2.7.2/hadoop-yarn/hadoop-yarn-site/CapacityScheduler.html)

## <a name="how-do-i-download-yarn-logs-from-a-cluster"></a>¿Cómo se descargan registros de YARN desde un clúster?

### <a name="resolution-steps"></a>Pasos de resolución

1. Conéctese al clúster de HDInsight con un cliente Secure Shell (SSH). Para más información, consulte [Lecturas adicionales](#additional-reading-2).

1. Enumere todos los identificadores de aplicación de las aplicaciones Yarn que se están ejecutando con el siguiente comando:

    ```apache
    yarn top
    ```

    Los identificadores aparecen en la columna **APPLICATIONID**. Puede descargar los registros desde la columna **APPLICATIONID**.

    ```apache
    YARN top - 18:00:07, up 19d, 0:14, 0 active users, queue(s): root
    NodeManager(s): 4 total, 4 active, 0 unhealthy, 0 decommissioned, 0 lost, 0 rebooted
    Queue(s) Applications: 2 running, 10 submitted, 0 pending, 8 completed, 0 killed, 0 failed
    Queue(s) Mem(GB): 97 available, 3 allocated, 0 pending, 0 reserved
    Queue(s) VCores: 58 available, 2 allocated, 0 pending, 0 reserved
    Queue(s) Containers: 2 allocated, 0 pending, 0 reserved

                      APPLICATIONID USER             TYPE      QUEUE   #CONT  #RCONT  VCORES RVCORES     MEM    RMEM  VCORESECS    MEMSECS %PROGR       TIME NAME
     application_1490377567345_0007 hive            spark  thriftsvr       1       0       1       0      1G      0G    1628407    2442611  10.00   18:20:20 Thrift JDBC/ODBC Server
     application_1490377567345_0006 hive            spark  thriftsvr       1       0       1       0      1G      0G    1628430    2442645  10.00   18:20:20 Thrift JDBC/ODBC Server
    ```

1. Descargue los registros de contenedor de YARN para todos los maestros de aplicación con el siguiente comando:

    ```apache
    yarn logs -applicationIdn logs -applicationId <application_id> -am ALL > amlogs.txt
    ```

    Este comando crea un archivo de registro denominado amlogs.txt.

1. Descargue los registros de contenedor de YARN solo para el maestro de aplicación más reciente con el siguiente comando:

    ```apache
    yarn logs -applicationIdn logs -applicationId <application_id> -am -1 > latestamlogs.txt
    ```

    Este comando crea un archivo de registro denominado latestamlogs.txt.

1. Descargue los registros de contenedor de YARN para los dos primeros maestros de aplicación con el siguiente comando:

    ```apache
    yarn logs -applicationIdn logs -applicationId <application_id> -am 1,2 > first2amlogs.txt
    ```

    Este comando crea un archivo de registro denominado first2amlogs.txt.

1. Descargue todos los registros de contenedor de YARN con el siguiente comando:

    ```apache
    yarn logs -applicationIdn logs -applicationId <application_id> > logs.txt
    ```

    Este comando crea un archivo de registro denominado logs.txt.

1. Descargue el registro de YARN de un contenedor determinado con el siguiente comando:

    ```apache
    yarn logs -applicationIdn logs -applicationId <application_id> -containerId <container_id> > containerlogs.txt
    ```

    Este comando crea un archivo de registro denominado containerlogs.txt.

### <a name="additional-reading"></a><a name="additional-reading-2"></a>Lecturas adicionales

- [Conexión a HDInsight (Apache Hadoop) con SSH](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-linux-use-ssh-unix)
- [Apache Hadoop Yarn concepts and applications](https://hadoop.apache.org/docs/r2.7.4/hadoop-yarn/hadoop-yarn-site/WritingYarnApplications.html#Concepts_and_Flow) (Conceptos y aplicaciones de YARN en Apache Hadoop)

## <a name="next-steps"></a>Pasos siguientes

Si su problema no aparece o es incapaz de resolverlo, visite uno de nuestros canales para obtener ayuda adicional:

- Obtenga respuestas de expertos de Azure mediante el [soporte técnico de la comunidad de Azure](https://azure.microsoft.com/support/community/).

- Póngase en contacto con [@AzureSupport](https://twitter.com/azuresupport), la cuenta oficial de Microsoft Azure para mejorar la experiencia del cliente. Esta cuenta pone en contacto a la comunidad de Azure con los recursos adecuados: respuestas, soporte técnico y expertos.

- Si necesita más ayuda, puede enviar una solicitud de soporte técnico desde [Azure Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/). Seleccione **Soporte técnico** en la barra de menús o abra la central **Ayuda + soporte técnico**. Para obtener información más detallada, revise [Creación de una solicitud de soporte técnico de Azure](https://docs.microsoft.com/azure/azure-portal/supportability/how-to-create-azure-support-request). La suscripción a Microsoft Azure incluye acceso al soporte técnico para facturación y administración de suscripciones. El soporte técnico se proporciona a través de uno de los [planes de soporte técnico de Azure](https://azure.microsoft.com/support/plans/).

---
title: Excepción al ejecutar consultas desde la vista de Apache Ambari Hive en Azure HDInsight
description: Pasos para la solución de problemas al ejecutar consultas de Apache Hive a través de la vista de Apache Ambari Hive en Azure HDInsight.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: troubleshooting
ms.date: 12/23/2019
ms.openlocfilehash: 809b2e383eb57b730fd76ec2194764178aa810c0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "75895046"
---
# <a name="exception-when-running-queries-from-apache-ambari-hive-view-in-azure-hdinsight"></a>Excepción al ejecutar consultas desde la vista de Apache Ambari Hive en Azure HDInsight

En este artículo se describen los pasos de solución de problemas y las posibles soluciones para los problemas que se producen al usar clústeres de Azure HDInsight.

## <a name="issue"></a>Problema

Al ejecutar una consulta de Apache Hive desde la vista de Apache Ambari Hive, recibe el siguiente mensaje de error de forma intermitente:

```error
Cannot create property 'errors' on string '<!DOCTYPE html PUBLIC '-//W3C//DTD XHTML 1.0 Strict//EN' 'http://www.w3.org/TR/xhtml1/DTD/xhtml1-strict.dtd'>
<html xmlns='http://www.w3.org/1999/xhtml'>
<head>
<title>IIS 8.5 Detailed Error - 502.3 - Bad Gateway</title>
```

## <a name="cause"></a>Causa

Tiempo de espera agotado para la puerta de enlace.

El tiempo de espera de la puerta de enlace es de 2 minutos. Las consultas de la vista de Ambari Hive se envían al punto de conexión `/hive2` a través de la puerta de enlace. Una vez que la consulta se ha compilado y aceptado correctamente, HiveServer devuelve un `queryid`. Los clientes seguirán sondeando el estado de la consulta. Durante este proceso, si HiveServer no devuelve una respuesta HTTP en 2 minutos, la puerta de enlace de HDI produce un error de tiempo de espera de puerta de enlace 502.3 para el autor de la llamada. Los errores pueden producirse cuando la consulta se envía para su procesamiento (más probable) y también en la llamada de obtención del estado (menos probable). Los usuarios podrían ver cualquiera de ellos.

Se supone que el subproceso del controlador HTTP es rápido: prepare el trabajo y devuelva un `queryid`. Sin embargo, debido a varios motivos, todos los subprocesos del controlador pueden estar ocupados, lo que provoca tiempos de espera para las nuevas consultas y las llamadas de obtención del estado.

### <a name="responsibilities-of-the-http-handler-thread"></a>Responsabilidades del subproceso del controlador HTTP

Cuando el cliente envía una consulta a HiveServer, realiza las siguientes acciones en el subproceso en primer plano:

* Analizar la solicitud y realizar la comprobación semántica
* Adquirir el bloqueo
* Buscar metastore, si es necesario
* Compilar la consulta (DDL o DML)
* Preparar un plan de consulta
* Realizar la autorización (ejecutar todas las directivas de Ranger aplicables en clústeres seguros)

## <a name="resolution"></a>Solución

Algunas recomendaciones generales para mejorar la situación:

* Si usa un metastore de Hive externo, compruebe las métricas de base de datos y asegúrese de que la base de datos no esté sobrecargada. Considere la posibilidad de escalar la capa de base de datos de metastore.

* Asegúrese de que las operaciones paralelas estén activadas (esto permite que los subprocesos del controlador HTTP se ejecuten en paralelo). Para comprobar el valor, inicie [Apache Ambari](../hdinsight-hadoop-manage-ambari.md) y vaya a **Hive** > **Configuraciones** > **Avanzado** > **Sito de Hive personalizado**. El valor de `hive.server2.parallel.ops.in.session` debe ser `true`.

* Asegúrese de que el SKU de la VM del clúster no sea demasiado pequeño para la carga. Considere la posibilidad de dividir el trabajo entre varios clústeres. Para más información, consulte [Elección de un tipo de clúster](../hdinsight-capacity-planning.md#choose-a-cluster-type).

* Si se ha instalado Ranger en el clúster, compruebe si hay demasiadas directivas de Ranger que deban evaluarse para cada consulta. Busque directivas duplicadas o innecesarias.

* Compruebe el valor de **Tamaño del montón de HiveServer2** de Ambari. Vaya a **Hive** > **Configuraciones** > **Configuración** > **Optimización**. Asegúrese de que el valor sea superior a 10 GB. Ajústelo según sea necesario para optimizar el rendimiento.

* Asegúrese de que la consulta de Hive esté bien ajustada. Para obtener más información, consulte [Optimización de las consultas de Azure Hive en Azure HDInsight](../hdinsight-hadoop-optimize-hive-query.md).

## <a name="next-steps"></a>Pasos siguientes

Si su problema no aparece o es incapaz de resolverlo, visite uno de nuestros canales para obtener ayuda adicional:

* Obtenga respuestas de expertos de Azure mediante el [soporte técnico de la comunidad de Azure](https://azure.microsoft.com/support/community/).

* Póngase en contacto con [@AzureSupport](https://twitter.com/azuresupport), la cuenta oficial de Microsoft Azure para mejorar la experiencia del cliente. Esta cuenta pone en contacto a la comunidad de Azure con los recursos adecuados: respuestas, soporte técnico y expertos.

* Si necesita más ayuda, puede enviar una solicitud de soporte técnico desde [Azure Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/). Seleccione **Soporte técnico** en la barra de menús o abra la central **Ayuda + soporte técnico**. Para obtener información más detallada, revise [Creación de una solicitud de soporte técnico de Azure](https://docs.microsoft.com/azure/azure-portal/supportability/how-to-create-azure-support-request). La suscripción a Microsoft Azure incluye acceso al soporte técnico para facturación y administración de suscripciones. El soporte técnico se proporciona a través de uno de los [planes de soporte técnico de Azure](https://azure.microsoft.com/support/plans/).

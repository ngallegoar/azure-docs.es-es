---
title: 'Comprensión y resolución de errores de WebHCat en HDInsight: Azure'
description: Aprenda cómo tratar errores comunes devueltos por WebHCat en HDInsight y cómo resolverlos.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: troubleshooting
ms.custom: hdinsightactive
ms.date: 04/14/2020
ms.openlocfilehash: 021bfc0b87b0da800728eda26d9f5222bd52bc1e
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "86086966"
---
# <a name="understand-and-resolve-errors-received-from-webhcat-on-hdinsight"></a>Entender y resolver errores recibidos de WebHCat en HDInsight

Obtener información acerca de los errores recibidos al utilizar WebHCat con HDInsight y cómo resolverlos. Las herramientas de cliente como Azure PowerShell y Data Lake Tools para Visual Studio usan WebHCat internamente.

## <a name="what-is-webhcat"></a>¿Qué es WebHCat?

[WebHCat](https://cwiki.apache.org/confluence/display/Hive/WebHCat) es una API REST para [HCatalog](https://cwiki.apache.org/confluence/display/Hive/HCatalog), una capa de administración de almacenamiento y tablas para Apache Hadoop. WebHCat está habilitado de forma predeterminada en los clústeres de HDInsight y se usa en varias herramientas para enviar trabajos, obtener el estado del trabajo, etc., sin iniciar sesión en el clúster.

## <a name="modifying-configuration"></a>Modificación de la configuración

Algunos de los errores que se muestran en este documento se producen porque se ha superado un máximo configurado. Cuando el paso de resolución mencione que puede cambiar un valor, use Apache Ambari (web o API de REST) para modificar el valor. Para más información, consulte [Administración de HDInsight mediante Apache Ambari](hdinsight-hadoop-manage-ambari.md)

### <a name="default-configuration"></a>Configuración predeterminada

Si se superan los siguientes valores predeterminados, puede degradar el rendimiento de WebHCat o provocar errores:

| Configuración | Qué hace | Valor predeterminado |
| --- | --- | --- |
| [yarn.scheduler.capacity.maximum-applications][maximum-applications] |El número máximo de trabajos que pueden estar activos de manera simultánea (pendientes o en ejecución) |10 000 |
| [templeton.exec.max-procs][max-procs] |El número máximo de solicitudes que se pueden atender de manera simultánea |20 |
| [mapreduce.jobhistory.max-age-ms][max-age-ms] |El número de días que se conservará el historial de trabajos |7 días |

## <a name="too-many-requests"></a>Demasiadas solicitudes

**Código de estado HTTP**: 429

| Causa | Resolución |
| --- | --- |
| Ha superado el número máximo de solicitudes simultáneas atendidas por WebHCat por minuto (el valor predeterminado es 20) |Reduzca la carga de trabajo para asegurarse de que no envía más del número máximo de solicitudes simultáneas o aumenta el límite de solicitudes simultáneas al modificar `templeton.exec.max-procs`. Consulte [Modificación de la configuración](#modifying-configuration) para más información. |

## <a name="server-unavailable"></a>Servidor no disponible

**Código de estado HTTP**: 503

| Causa | Resolución |
| --- | --- |
| Este código de estado suele producirse durante la conmutación por error entre el nodo principal primario y secundario para el clúster. |Espere dos minutos y vuelva a intentar la operación. |

## <a name="bad-request-content-could-not-find-job"></a>Contenido de solicitud incorrecta: no se encontró el trabajo

**Código de estado HTTP**: 400

| Causa | Resolución |
| --- | --- |
| Los detalles de los trabajos se han limpiado con el limpiador del historial de trabajos |El período de retención predeterminado para el historial de trabajos es de 7 días. El período de retención predeterminado puede cambiarse modificando `mapreduce.jobhistory.max-age-ms`. Consulte [Modificación de la configuración](#modifying-configuration) para más información. |
| Se ha suprimido el trabajo debido a una conmutación por error. |Vuelva a intentar el envío de trabajos durante un tiempo máximo de dos minutos |
| Se usó un identificador de trabajo no válido |Compruebe si el identificador de trabajo es correcto |

## <a name="bad-gateway"></a>Puerta de enlace incorrecta

**Código de estado HTTP**: 502

| Causa | Resolución |
| --- | --- |
| La recolección de elementos no utilizados internos se está produciendo en el proceso de WebHCat |Espere a que termine la recolección de elementos no utilizados o reinicie el servicio de WebHCat |
| Tiempo de espera de una respuesta desde el servicio de ResourceManager. Este error se puede producir cuando el número de aplicaciones activas alcanza el máximo configurado (el valor predeterminado es 10.000) |Espere a que finalice los trabajos actualmente en ejecución o aumente el límite de trabajos simultáneos modificando `yarn.scheduler.capacity.maximum-applications`. Consulte [Modificación de la configuración](#modifying-configuration) para más información. |
| Al intentar recuperar todos los trabajos a través de la llamada [GET /jobs](https://cwiki.apache.org/confluence/display/Hive/WebHCat+Reference+Jobs) mientras `Fields` está establecido en `*` |No recupere *todos* los detalles del trabajo. En su lugar, use `jobid` para recuperar detalles de trabajos solo mayores que un id. de trabajo determinado. O bien, no use `Fields` |
| El servicio de WebHCat está inactivo durante la conmutación por error del nodo principal |Espere dos minutos y vuelva a intentar la operación |
| Hay más de 500 trabajos pendientes enviados a través de WebHCat |Espere hasta que hayan finalizado los trabajos pendientes actualmente antes de enviar más trabajos |

## <a name="next-steps"></a>Pasos siguientes

Si su problema no aparece o es incapaz de resolverlo, visite uno de nuestros canales para obtener más soporte técnico:

* Obtenga respuestas de expertos de Azure mediante el [soporte técnico de la comunidad de Azure](https://azure.microsoft.com/support/community/).

* Póngase en contacto con [@AzureSupport](https://twitter.com/azuresupport), la cuenta oficial de Microsoft Azure para mejorar la experiencia del cliente. Esta cuenta pone en contacto a la comunidad de Azure con los recursos adecuados: respuestas, soporte técnico y expertos.

* Si necesita más ayuda, puede enviar una solicitud de soporte técnico desde [Azure Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/). Seleccione **Soporte técnico** en la barra de menús o abra la central **Ayuda + soporte técnico**. Para obtener información más detallada, revise [Creación de una solicitud de soporte técnico de Azure](https://docs.microsoft.com/azure/azure-portal/supportability/how-to-create-azure-support-request). La suscripción a Microsoft Azure incluye acceso al soporte técnico para facturación y administración de suscripciones. El soporte técnico se proporciona a través de uno de los [planes de soporte técnico de Azure](https://azure.microsoft.com/support/plans/).

[maximum-applications]: https://docs.cloudera.com/HDPDocuments/HDP2/HDP-2.1.3/bk_system-admin-guide/content/setting_application_limits.html
[max-procs]: https://cwiki.apache.org/confluence/display/Hive/WebHCat+Configure#WebHCatConfigure-WebHCatConfiguration
[max-age-ms]: https://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.0.6.0/ds_Hadoop/hadoop-mapreduce-client/hadoop-mapreduce-client-core/mapred-default.xml

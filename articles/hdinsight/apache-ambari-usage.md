---
title: Uso de Apache Ambari en Azure HDInsight
description: Explicación de cómo se usa Apache Ambari en Azure HDInsight.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 02/05/2020
ms.openlocfilehash: e7c5ca1ce314c85162986591b2cd83907811bf72
ms.sourcegitcommit: d767156543e16e816fc8a0c3777f033d649ffd3c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/26/2020
ms.locfileid: "92542393"
---
# <a name="apache-ambari-usage-in-azure-hdinsight"></a>Uso de Apache Ambari en Azure HDInsight

HDInsight usa Apache Ambari para la implementación y administración de clústeres. En cada nodo (nodo principal, nodo de trabajo, zookeeper y nodo perimetral, si existe) se ejecutan agentes de Ambari. El servidor de Ambari solo se ejecuta en el nodo principal (hn0 o hn1). Solo se debe ejecutar una instancia del servidor de Ambari al mismo tiempo. El controlador de conmutación por error de HDInsight se encarga de controlar esto. Cuando uno de los nodos principales está inactivo porque se va a reiniciar o necesita mantenimiento, el otro nodo principal se volverá activo y se iniciará el servidor de Ambari en el segundo nodo principal.

Toda la configuración del clúster debe realizarse mediante la [interfaz de usuario de Ambari](./hdinsight-hadoop-manage-ambari.md); los cambios locales se sobrescribirán cuando se reinicie el nodo.

## <a name="failover-controller-services"></a>Servicios de controlador de conmutación por error

El controlador de conmutación por error de HDInsight también es responsable de actualizar la dirección IP del host de nodo principal, que señala al nodo principal activo actual. Todos los agentes de Ambari están configurados para informar de su estado y latido al host de nodo principal. El controlador de conmutación por error es un conjunto de servicios que se ejecutan en cada nodo del clúster; si no se ejecutan, es posible que la conmutación por error del nodo principal no funcione correctamente y que termine con el error HTTP 502 al intentar acceder al servidor de Ambari.

Para comprobar qué nodo principal está activo, una manera es conectarse mediante SSH a uno de los nodos del clúster y, después, ejecutar `ping headnodehost` y comparar la IP con la de las dos nodos principales.

Si los servicios del controlador de conmutación por error no están en ejecución, puede que la conmutación por error del nodo principal no se realice correctamente, lo que puede acabar con que el servidor de Ambari no se ejecute. Para comprobar que los servicios del controlador de conmutación por error están en ejecución, ejecute:

```bash
ps -ef | grep failover
```

## <a name="logs"></a>Registros

En el nodo principal activo, puede consultar los registros de servidor de Ambari en:

```
/var/log/ambari-server/ambari-server.log
/var/log/ambari-server/ambari-server-check-database.log
```

En cualquier nodo del clúster, puede consultar los registros del agente de Ambari en:

```bash
/var/log/ambari-agent/ambari-agent.log
```

## <a name="service-start-sequences"></a>Secuencias de inicio del servicio

Es la secuencia de inicio del servicio en el momento del arranque:

1. El agente de HDInsight inicia los servicios del controlador de conmutación por error.
1. Los servicios del controlador de conmutación por error inician el agente Ambari en cada nodo y el servidor de Ambari en el nodo principal activo.

## <a name="ambari-database"></a>Base de datos de Ambari

HDInsight crea la base de datos en SQL Database en segundo plano para que funcione como la base de datos del servidor de Ambari. El [nivel de servicio es S0](../azure-sql/database/elastic-pool-scale.md) de forma predeterminada.

En el caso de clústeres con un número de nodos de trabajo mayor que 16 al crear el clúster, S2 es el nivel de servicio de base de datos.

## <a name="takeaway-points"></a>Aspectos a tener en cuenta

No inicie o detenga manualmente los servicios ambari-server o ambari-agent, a menos que intente reiniciar el servicio para solucionar un problema. Para forzar una conmutación por error, puede reiniciar el nodo principal activo.

No modifique nunca manualmente ningún archivo de configuración en ningún nodo del clúster; deje que la interfaz de usuario de Ambari haga el trabajo por usted.

## <a name="next-steps"></a>Pasos siguientes

* [Administración de clústeres de HDInsight con la interfaz de usuario web de Apache Ambari](hdinsight-hadoop-manage-ambari.md)
* [Administración de clústeres de HDInsight mediante la API REST de Apache Ambari](hdinsight-hadoop-manage-ambari-rest-api.md)

Si su problema no aparece o es incapaz de resolverlo, visite uno de nuestros canales para obtener más soporte técnico:

* Obtenga respuestas de expertos de Azure mediante el [soporte técnico de la comunidad de Azure](https://azure.microsoft.com/support/community/).

* Póngase en contacto con [@AzureSupport](https://twitter.com/azuresupport), la cuenta oficial de Microsoft Azure para mejorar la experiencia del cliente. Esta cuenta pone en contacto a la comunidad de Azure con los recursos adecuados: respuestas, soporte técnico y expertos.

* Si necesita más ayuda, puede enviar una solicitud de soporte técnico desde [Azure Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/). Seleccione **Soporte técnico** en la barra de menús o abra la central **Ayuda + soporte técnico** . Para obtener información más detallada, revise [Creación de una solicitud de soporte técnico de Azure](../azure-portal/supportability/how-to-create-azure-support-request.md). La suscripción a Microsoft Azure incluye acceso al soporte técnico para facturación y administración de suscripciones. El soporte técnico se proporciona a través de uno de los [planes de soporte técnico de Azure](https://azure.microsoft.com/support/plans/).
---
title: La interfaz de usuario de Apache Ambari muestra los hosts y servicios inactivos en Azure HDInsight.
description: Solución de problemas de la interfaz de usuario de Apache Ambari cuando muestra los hosts y servicios inactivos en Azure HDInsight
ms.service: hdinsight
ms.topic: troubleshooting
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.date: 08/02/2019
ms.openlocfilehash: 5340b1c7a6510595376789bc5777e6fb6f07dd4a
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "75895632"
---
# <a name="scenario-apache-ambari-ui-shows-down-hosts-and-services-in-azure-hdinsight"></a>Escenario: La interfaz de usuario de Apache Ambari muestra los hosts y servicios inactivos en Azure HDInsight.

En este artículo se describen los pasos de solución de problemas y las posibles soluciones para los problemas que se producen al usar clústeres de Azure HDInsight.

## <a name="issue"></a>Problema

Se puede acceder a la interfaz de usuario de Apache Ambari, pero muestra que casi todos los servicios están inactivos y todos los hosts que muestran latido se han perdido.

## <a name="cause"></a>Causa

En la mayoría de los escenarios, se trata de un problema con el servidor de Ambari que no se está ejecutando en el nodo principal activo. Compruebe qué nodo principal es el nodo principal activo y asegúrese de que ambari-server se ejecuta en el correcto. No inicie manualmente ambari-server, deje que el servicio del controlador de conmutación por error sea responsable de iniciar ambari-server en el nodo principal correcto. Reinicie el nodo principal activo para forzar una conmutación por error.

Los problemas de red también pueden causar este problema. En cada nodo del clúster, consulte si puede hacer ping `headnodehost`. Hay una situación poco frecuente en la que no se puede conectar un nodo de clúster a `headnodehost`:

```
$>telnet headnodehost 8440
... No route to host
```

## <a name="resolution"></a>Solución

Normalmente, el reinicio del nodo principal activo solucionará este problema. En caso contrario, póngase en contacto con el equipo de soporte técnico de HDInsight.

## <a name="next-steps"></a>Pasos siguientes

Si su problema no aparece o es incapaz de resolverlo, visite uno de nuestros canales para obtener ayuda adicional:

* Obtenga respuestas de expertos de Azure mediante el [soporte técnico de la comunidad de Azure](https://azure.microsoft.com/support/community/).

* Póngase en contacto con [@AzureSupport](https://twitter.com/azuresupport), la cuenta oficial de Microsoft Azure para mejorar la experiencia del cliente, que pone en contacto a la comunidad de Azure con los recursos adecuados: respuestas, soporte técnico y expertos.

* Si necesita más ayuda, puede enviar una solicitud de soporte técnico desde [Azure Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/). Seleccione **Soporte técnico** en la barra de menús o abra la central **Ayuda + soporte técnico**. Para obtener información más detallada, revise [Creación de una solicitud de soporte técnico de Azure](https://docs.microsoft.com/azure/azure-portal/supportability/how-to-create-azure-support-request). La suscripción a Microsoft Azure incluye acceso al soporte técnico para facturación y administración de suscripciones. El soporte técnico se proporciona a través de uno de los [planes de soporte técnico de Azure](https://azure.microsoft.com/support/plans/).

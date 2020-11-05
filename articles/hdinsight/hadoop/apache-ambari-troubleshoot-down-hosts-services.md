---
title: La interfaz de usuario de Apache Ambari muestra los hosts y servicios inactivos en Azure HDInsight.
description: Solución de problemas de la interfaz de usuario de Apache Ambari cuando muestra los hosts y servicios inactivos en Azure HDInsight
ms.service: hdinsight
ms.topic: troubleshooting
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.date: 08/02/2019
ms.openlocfilehash: 9c9676fa1457be014098b8190986c617262e628d
ms.sourcegitcommit: 7863fcea618b0342b7c91ae345aa099114205b03
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/03/2020
ms.locfileid: "93286503"
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

[!INCLUDE [troubleshooting next steps](../../../includes/hdinsight-troubleshooting-next-steps.md)]
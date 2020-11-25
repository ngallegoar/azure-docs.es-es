---
title: Error de creación del clúster con DomainNotFound en Azure HDInsight
description: Pasos de solución de problemas y soluciones posibles para problemas que se producen al interactuar con clústeres de Azure HDInsight.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: troubleshooting
ms.date: 01/23/2020
ms.openlocfilehash: 79cd3b687dc8a2eb838146a6dc8a40b184a55fe6
ms.sourcegitcommit: a43a59e44c14d349d597c3d2fd2bc779989c71d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/25/2020
ms.locfileid: "96005952"
---
# <a name="scenario-cluster-creation-fails-with-domainnotfound-error-in-azure-hdinsight"></a>Escenario: Error de creación del clúster con DomainNotFound en Azure HDInsight

En este artículo se describen los pasos de solución de problemas y las posibles soluciones para los problemas que se producen al usar clústeres de Azure HDInsight.

## <a name="issue"></a>Problema

No se puede crear el clúster de HDI seguro (Enterprise Security Package) con el mensaje de error `DomainNotFound`.

## <a name="cause"></a>Causa

Configuración incorrecta de DNS.

## <a name="resolution"></a>Solución

Cuando se implementan los clústeres unidos a un dominio, HDI crea un nombre de usuario y una contraseña internos en AAD DS (para cada clúster) y une todos los nodos del clúster a este dominio. La unión al dominio se realiza mediante las herramientas de Samba. Asegúrese de que se cumplen los siguientes requisitos previos:

* El nombre de dominio debe resolverse mediante DNS.
* La dirección IP de los controladores de dominio debe establecerse en la configuración de DNS de la red virtual donde se va a implementar el clúster.
* Si la red virtual está emparejada con la red virtual de AAD DS, esto debe hacerse manualmente.
* Si usa reenviadores DNS, el nombre de dominio debe resolverse correctamente dentro de la red virtual.
* Las directivas de seguridad (NSG) no deben bloquear la unión al dominio.

### <a name="additional-debugging-steps"></a>Pasos de depuración adicionales

* Implemente una máquina virtual Windows en la misma subred y una la máquina a un dominio con un nombre de usuario y una contraseña (esto puede hacerse mediante la interfaz de usuario del panel de control).

* Implemente una máquina virtual Ubuntu en la misma subred y una la máquina a un dominio.
  * Conéctese a la máquina mediante SSH.
  * unidad de búsqueda de sudo
  * Ejecute el script con el nombre de usuario y la contraseña.
  * El script hará ping, creará los archivos de configuración necesarios y, luego, el dominio. Si se realiza correctamente, significa que la configuración de DNS es adecuada.

## <a name="next-steps"></a>Pasos siguientes

[!INCLUDE [troubleshooting next steps](../../../includes/hdinsight-troubleshooting-next-steps.md)]
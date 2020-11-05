---
title: No se puede registrar en el clúster de Azure HDInsight
description: Solución de los problemas por los que no se puede iniciar sesión en el clúster de Apache Hadoop en Azure HDInsight
ms.service: hdinsight
ms.topic: troubleshooting
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.date: 07/31/2019
ms.openlocfilehash: 222c48ab2b9b718bc373e710f37f906e68bd31d4
ms.sourcegitcommit: 7863fcea618b0342b7c91ae345aa099114205b03
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/03/2020
ms.locfileid: "93289020"
---
# <a name="scenario-unable-to-log-into-azure-hdinsight-cluster"></a>Escenario: No se puede registrar en el clúster de Azure HDInsight

En este artículo se describen los pasos de solución de problemas y las posibles soluciones para los problemas que se producen al usar clústeres de Azure HDInsight.

## <a name="issue"></a>Problema

No se puede registrar en el clúster de Azure HDInsight.

## <a name="cause"></a>Causa

Los motivos pueden variar. Al iniciar sesión en los paneles de clúster o la aplicación, recuerde usar el "inicio de sesión de clúster" o las credenciales de HTTP. Cuando se conecte remotamente, use sus credenciales de Shell seguro (SSH) o del Escritorio remoto.

## <a name="resolution"></a>Solución

Para resolver problemas habituales, pruebe uno o varios de los pasos siguientes.

* Intente abrir el panel del clúster en una nueva pestaña del explorador en modo de privacidad.

* Para los clústeres Linux, si no puede recuperar las credenciales de SSH, puede [restablecer las credenciales en la interfaz de usuario de Ambari](../hdinsight-administer-use-portal-linux.md#change-passwords).

## <a name="next-steps"></a>Pasos siguientes

[!INCLUDE [troubleshooting next steps](../../../includes/hdinsight-troubleshooting-next-steps.md)]
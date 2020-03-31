---
title: Alertas obsoletas de Apache Ambari en Azure HDInsight
description: Debate y análisis de posibles causas y soluciones para las alertas obsoletas de Apache Ambari en HDInsight.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: troubleshooting
ms.date: 01/22/2020
ms.openlocfilehash: f9dfcb930e3fe4f862f9f51ff00270d0eb0c66ca
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "77539117"
---
# <a name="scenario-apache-ambari-stale-alerts-in-azure-hdinsight"></a>Escenario: Alertas obsoletas de Apache Ambari en Azure HDInsight

En este artículo se describen los pasos de solución de problemas y las posibles soluciones para los problemas que se producen al usar clústeres de Azure HDInsight.

## <a name="issue"></a>Problema

En la interfaz de usuario de Apache Ambari, es posible que vea una alerta similar a esta:

![Ejemplo de alertas obsoletas de Apache Ambari](./media/apache-ambari-troubleshoot-stale-alerts/ambari-stale-alerts-example.png)

## <a name="cause"></a>Causa

Los agentes de Ambari supervisan continuamente el estado de muchos recursos. Se pueden configurar *alertas* para que le notifiquen si determinadas propiedades del clúster se encuentran dentro de los umbrales predeterminados. Una vez que se ejecuta cada comprobación de recursos, si se cumple la condición de la alerta, los agentes de Ambari notifican el estado al servidor de Ambari y desencadenan una alerta. Si una alerta no se comprueba según el intervalo de su perfil de alerta, el servidor desencadena una alerta que indica las *alertas obsoletas del servidor de Ambari*.

Hay varios motivos por los que es posible que una comprobación de estado no se ejecute en su intervalo definido:

* Los hosts están sometidos a un uso intensivo (uso elevado de la CPU), por lo que el agente de Ambari no puede obtener suficientes recursos del sistema para ejecutar las alertas a tiempo.

* El clúster está ocupado ejecutando muchos trabajos o servicios durante un período de carga intensiva.

* Un pequeño número de hosts del clúster hospeda muchos componentes y, por tanto, son necesarios para ejecutar muchas alertas. Si el número de componentes es grande, es posible que los trabajos de alerta pierdan los intervalos programados.

## <a name="resolution"></a>Solución

Pruebe los métodos siguientes para resolver problemas con alertas obsoletas de Ambari.

### <a name="increase-the-alert-interval-time"></a>Aumento del tiempo de intervalo de alerta

Puede aumentar el valor de un intervalo de alerta individual, en función del tiempo de respuesta y la carga del clúster:

1. En la interfaz de usuario de Apache Ambari, seleccione la pestaña **Alerts** (Alertas).
1. Seleccione el nombre de la definición de alerta que quiera.
1. En la definición, seleccione **Edit** (Editar).
1. Aumente el valor de **Check Interval** (Intervalo de comprobación) y, luego, seleccione **Save** (Guardar).

### <a name="increase-the-alert-interval-time-for-ambari-server-alerts"></a>Aumento del tiempo de intervalo de alerta para las alertas del servidor de Ambari

1. En la interfaz de usuario de Apache Ambari, seleccione la pestaña **Alerts** (Alertas).
1. En la lista desplegable **Groups** (Grupos), seleccione **AMBARI Default** (Valor predeterminado de AMBARI).
1. Seleccione la alerta **Ambari Server Alerts** (Alertas del servidor de Ambari).
1. En la definición, seleccione **Edit** (Editar).
1. Aumente el valor de **Check Interval** (Intervalo de comprobación).
1. Aumente el valor de **Interval Multiplier** (Multiplicador de intervalo) y, luego, seleccione **Save** (Guardar).

### <a name="disable-and-reenable-the-alert"></a>Deshabilitación y nueva habilitación de la alerta

Para descartar una alerta obsoleta, deshabilítela y, luego, vuelva a habilitarla:

1. En la interfaz de usuario de Apache Ambari, seleccione la pestaña **Alerts** (Alertas).
1. Seleccione el nombre de la definición de alerta que quiera.
1. En la definición, seleccione **Enabled** (Habilitado), en el extremo derecho de la interfaz de usuario.
1. En la ventana emergente **Confirmation** (Confirmación), seleccione **Confirm Disable** (Confirmar deshabilitación).
1. Espere unos segundos a que se borren todas las "instancias" de la alerta que se muestran en la página.
1. En la definición, seleccione **Disabled** (Deshabilitado), en el extremo derecho de la interfaz de usuario.
1. En la ventana emergente **Confirmation** (Confirmación), seleccione **Confirm Enable** (Confirmar habilitación).

### <a name="increase-the-alert-grace-period"></a>Aumento del período de gracia de la alerta

Antes de que un agente de Ambari informe de que una alerta configurada no ha seguido su programación, hay un período de gracia. Si la alerta perdió el tiempo programado pero se ejecutó dentro del período de gracia, no se genera la alerta obsoleta.

El valor predeterminado de `alert_grace_period` es 5 segundos. Puede configurar esta opción en /etc/ambari-agent/conf/ambari-agent.ini. En los hosts en los que se producen alertas obsoletas a intervalos regulares, intente aumentar el valor a 10. Después, reinicie el agente de Ambari.

## <a name="next-steps"></a>Pasos siguientes

Si el problema no se ha mencionado aquí o no puede resolverlo, visite uno de los siguientes canales para obtener más ayuda:

* Obtenga respuestas de expertos en Azure en [Soporte técnico de la comunidad de Azure](https://azure.microsoft.com/support/community/).

* Conéctese con [@AzureSupport](https://twitter.com/azuresupport) en Twitter. Esta es la cuenta oficial de Microsoft Azure para mejorar la experiencia del cliente. Pone en contacto a la comunidad de Azure con los recursos adecuados: respuestas, soporte técnico y expertos.

* Si necesita más ayuda, envíe una solicitud de soporte técnico desde [Azure Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/). Para llegar ahí, seleccione Ayuda ( **?** ) en el menú del portal o abra el panel **Ayuda y soporte técnico**. Para más información, consulte [Creación de una solicitud de soporte técnico de Azure](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request). 

  La suscripción a Microsoft Azure incluye soporte técnico para la administración y la facturación de suscripciones. El soporte técnico está disponible mediante los [planes de soporte técnico de Azure](https://azure.microsoft.com/support/plans/).

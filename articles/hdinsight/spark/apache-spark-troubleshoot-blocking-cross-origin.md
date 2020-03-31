---
title: 'Error 404 de Jupyter: "Blocking Cross Origin API": Azure HDInsight'
description: Error 404 "No encontrado" del servidor de Jupyter debido a "Cross Origin Blocking API" en Azure HDInsight
ms.service: hdinsight
ms.topic: troubleshooting
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.date: 07/29/2019
ms.openlocfilehash: e241657186582955d21981f7dfe18856724aa692
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "75894410"
---
# <a name="scenario-jupyter-server-404-not-found-error-due-to-blocking-cross-origin-api-in-azure-hdinsight"></a>Escenario: Error 404 "No encontrado" del servidor de Jupyter debido a "Cross Origin Blocking API" en Azure HDInsight

En este artículo se describen los pasos de solución de problemas y las posibles soluciones para los problemas que se producen al usar componentes de Apache Spark en clústeres de Azure HDInsight.

## <a name="issue"></a>Problema

Al acceder al servicio Jupyter en HDInsight, verá un cuadro de error que indica "No encontrado". Si comprueba los registros de Jupyter, verá algo parecido a esto:

```log
[W 2018-08-21 17:43:33.352 NotebookApp] 404 PUT /api/contents/PySpark/notebook.ipynb (10.16.0.144) 4504.03ms referer=https://pnhr01hdi-corpdir.msappproxy.net/jupyter/notebooks/PySpark/notebook.ipynb
Blocking Cross Origin API request.  
Origin: https://xxx.xxx.xxx, Host: pnhr01.j101qxjrl4zebmhb0vmhg044xe.ax.internal.cloudapp.net:8001
```

También puede ver una dirección IP en el campo "Origin" (Origen) en el registro de Jupyter.

## <a name="cause"></a>Causa

Este error puede deberse a un par de cosas:

- Si ha configurado reglas de grupo de seguridad de red (NSG) para restringir el acceso al clúster. Restringir el acceso con reglas de NSG aún le permitirá acceder directamente a Apache Ambari y a otros servicios mediante la dirección IP en lugar del nombre del clúster. Sin embargo, al acceder a Jupyter, podría ver un error 404 "No encontrado".

- Si ha dado a la puerta de enlace de HDInsight un nombre de DNS personalizado distinto del `xxx.azurehdinsight.net` estándar.

## <a name="resolution"></a>Solución

1. Modifique los archivos de jupyter.py en estos dos lugares:

    ```bash
    /var/lib/ambari-server/resources/common-services/JUPYTER/1.0.0/package/scripts/jupyter.py
    /var/lib/ambari-agent/cache/common-services/JUPYTER/1.0.0/package/scripts/jupyter.py
    ```

1. Busque la línea que dice: `NotebookApp.allow_origin='\"https://{2}.{3}\"'` Y cámbiela a: `NotebookApp.allow_origin='\"*\"'`.

1. Reinicie el servicio de Jupyter desde Ambari.

1. Al escribir `ps aux | grep jupyter` en el símbolo del sistema se debe mostrar que permite la conexión de cualquier dirección URL.

Esta es una opción menos segura que la configuración ya implementada. Pero se supone que el acceso al clúster está restringido y que alguien externo tiene permitido conectarse al clúster, ya que hemos implementado el NSG.

## <a name="next-steps"></a>Pasos siguientes

Si su problema no aparece o es incapaz de resolverlo, visite uno de nuestros canales para obtener ayuda adicional:

* Obtenga respuestas de expertos de Azure mediante el [soporte técnico de la comunidad de Azure](https://azure.microsoft.com/support/community/).

* Póngase en contacto con [@AzureSupport](https://twitter.com/azuresupport), la cuenta oficial de Microsoft Azure para mejorar la experiencia del cliente, que pone en contacto a la comunidad de Azure con los recursos adecuados: respuestas, soporte técnico y expertos.

* Si necesita más ayuda, puede enviar una solicitud de soporte técnico desde [Azure Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/). Seleccione **Soporte técnico** en la barra de menús o abra la central **Ayuda + soporte técnico**. Para obtener información más detallada, revise [Creación de una solicitud de soporte técnico de Azure](https://docs.microsoft.com/azure/azure-portal/supportability/how-to-create-azure-support-request). La suscripción a Microsoft Azure incluye acceso al soporte técnico para facturación y administración de suscripciones. El soporte técnico se proporciona a través de uno de los [planes de soporte técnico de Azure](https://azure.microsoft.com/support/plans/).

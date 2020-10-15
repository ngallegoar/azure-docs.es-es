---
title: 'Las combinaciones de Apache Hive conducen a un error de OutOfMemory: Azure HDInsight'
description: Tratamiento de los errores de OutOfMemory "límite de sobrecarga de GC superado"
ms.service: hdinsight
ms.topic: troubleshooting
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.date: 07/30/2019
ms.openlocfilehash: ab334dfb15044fd0734a107c12003ca2c1f86906
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "75895184"
---
# <a name="scenario-joins-in-apache-hive-leads-to-an-outofmemory-error-in-azure-hdinsight"></a>Escenario: Las combinaciones de Apache Hive conducen a un error de OutOfMemory en Azure HDInsight

En este artículo se describen los pasos para la solución de problemas y las posibles soluciones para los problemas que se producen al usar componentes de Interactive Query en clústeres de Azure HDInsight.

## <a name="issue"></a>Problema

El comportamiento predeterminado de las combinaciones de Apache Hive es cargar todo el contenido de una tabla en memoria para que se pueda realizar una combinación sin tener que llevar a cabo un paso de asignación/reducción. Si la tabla de Hive es demasiado grande para que quepa en la memoria, se puede producir un error en la consulta.

## <a name="cause"></a>Causa

Al ejecutar combinaciones en Hive de tamaño suficiente, se produce el siguiente error:

```
Caused by: java.lang.OutOfMemoryError: GC overhead limit exceeded error.
```

## <a name="resolution"></a>Solución

Impida que Hive cargue tablas en memoria en las combinaciones (en lugar de realizar un paso de asignación/reducción) mediante el establecimiento del siguiente valor de configuración de Hive:

```
hive.auto.convert.join=false
```

## <a name="next-steps"></a>Pasos siguientes

Si con el establecimiento de este valor no se resuelve el problema, puede hacer lo siguiente:

* Obtenga respuestas de expertos de Azure mediante el [soporte técnico de la comunidad de Azure](https://azure.microsoft.com/support/community/).

* Póngase en contacto con [@AzureSupport](https://twitter.com/azuresupport), la cuenta oficial de Microsoft Azure para mejorar la experiencia del cliente, que pone en contacto a la comunidad de Azure con los recursos adecuados: respuestas, soporte técnico y expertos.

* Si necesita más ayuda, puede enviar una solicitud de soporte técnico desde [Azure Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/). Seleccione **Soporte técnico** en la barra de menús o abra la central **Ayuda + soporte técnico**. Para obtener información más detallada, revise [Creación de una solicitud de soporte técnico de Azure](https://docs.microsoft.com/azure/azure-portal/supportability/how-to-create-azure-support-request). La suscripción a Microsoft Azure incluye acceso al soporte técnico para facturación y administración de suscripciones. El soporte técnico se proporciona a través de uno de los [planes de soporte técnico de Azure](https://azure.microsoft.com/support/plans/).

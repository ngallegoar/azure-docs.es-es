---
title: Configuración de la programación de revisión del SO para clústeres de Azure HDInsight
description: Aprenda a configurar la programación de la aplicación de revisión del SO para clústeres de HDInsight basado en Linux.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: how-to
ms.custom: hdinsightactive
ms.date: 01/21/2020
ms.openlocfilehash: 005bfd4b7e7d062640a79896ccd3d7f76e3d6fb3
ms.sourcegitcommit: 3792cf7efc12e357f0e3b65638ea7673651db6e1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/29/2020
ms.locfileid: "91447208"
---
# <a name="configure-the-os-patching-schedule-for-linux-based-hdinsight-clusters"></a>Configuración de la programación de aplicación de revisiones del SO para clústeres de HDInsight basado en Linux

> [!IMPORTANT]
> Las imágenes de Ubuntu estarán disponibles para la creación del clúster de HDInsight tres meses después de publicarse. Los clústeres en ejecución no se revisan automáticamente. Los clientes deben usar acciones de script u otros mecanismos para revisar un clúster en ejecución. Como procedimiento recomendado, puede ejecutar estas acciones de script y aplicar las actualizaciones de seguridad justo después de crear el clúster.

HDInsight permite realizar tareas comunes en el clúster, como instalar revisiones del sistema operativo, aplicar actualizaciones de seguridad y reiniciar nodos. Estas tareas se realizan mediante los dos scripts siguientes que se pueden ejecutar como [acciones de script](hdinsight-hadoop-customize-cluster-linux.md) y que se pueden configurar con parámetros:

- `schedule-reboots.sh` - Realice un reinicio inmediato o programe un reinicio en los nodos de clúster.
- `install-updates-schedule-reboots.sh` - Instale todas las actualizaciones, solo las actualizaciones del kernel más las de seguridad o solo las actualizaciones del kernel.

> [!NOTE]  
> Las acciones de script no aplicarán automáticamente actualizaciones en todos los ciclos futuros de actualización. Ejecute los scripts cada vez que se deban aplicar nuevas actualizaciones para instalar las actualizaciones y, luego, reinicie la máquina virtual.

## <a name="preparation"></a>Preparación

Realice la revisión en un entorno que no sea de producción representativo antes de la implementación en producción. Desarrolle un plan para probar adecuadamente el sistema antes de la revisión real.

De vez en cuando, desde una sesión de SSH con el clúster, puede que reciba un mensaje que indica que hay actualizaciones de seguridad disponibles. El mensaje puede parecerse a este:

```
89 packages can be updated.
82 updates are security updates.

*** System restart required ***

Welcome to Spark on HDInsight.

```

La revisión es opcional y según su criterio.

## <a name="restart-nodes"></a>Reinicio de nodos
  
El script [schedule-reboots](https://hdiconfigactions.blob.core.windows.net/linuxospatchingrebootconfigv02/schedule-reboots.sh) establece el tipo de reinicio que se realizará en las máquinas en el clúster. Al enviar la acción de script, configúrela para que se aplique en los tres tipos de nodo: nodo principal, nodo de trabajo y ZooKeeper. Si el script no se aplica a un tipo de nodo, las máquinas virtuales de ese tipo de nodo no se actualizarán ni se reiniciarán.

El script `schedule-reboots script` acepta un parámetro numérico:

| Parámetro | Valores aceptados | Definición |
| --- | --- | --- |
| Tipo de reinicio que se va a realizar | 1 o 2 | Un valor de 1 habilita el reinicio programado (en 12 o 24 horas). Un valor de 2 habilita el reinicio inmediato (en 5 minutos). Si no se especifica ningún parámetro, el valor predeterminado es 1. |  

## <a name="install-updates-and-restart-nodes"></a>Instalación de actualizaciones y reinicio de nodos

El script [install-updates-schedule-reboots.sh](https://hdiconfigactions.blob.core.windows.net/linuxospatchingrebootconfigv02/install-updates-schedule-reboots.sh) proporciona opciones para instalar distintos tipos de actualizaciones y reiniciar la máquina virtual.

El script `install-updates-schedule-reboots` acepta dos parámetros numéricos, como se describe en la siguiente tabla:

| Parámetro | Valores aceptados | Definición |
| --- | --- | --- |
| Tipo de actualizaciones que se van a instalar | 0, 1 o 2 | Un valor de 0 instala solo las actualizaciones del kernel. Un valor de 1 instala todas las actualizaciones y un valor de 2 solo instala las actualizaciones del kernel más las de seguridad. Si no se proporciona ningún parámetro, el predeterminado es 0. |
| Tipo de reinicio que se va a realizar | 0, 1 o 2 | Un valor de 0 deshabilita el reinicio. Un valor de 1 habilita el reinicio programado y un valor de 2 habilita el reinicio inmediato. Si no se proporciona ningún parámetro, el predeterminado es 0. El usuario debe cambiar el parámetro de entrada 1 por el parámetro de entrada 2. |

> [!NOTE]
> Debe marcar un script como persistente después de aplicarlo a un clúster existente. De lo contrario, en todos los nodos que se creen mediante operaciones de escalado se usará la programación de aplicación de revisión predeterminada. Si aplica el script como parte del proceso de creación del clúster, se marcará como persistente de forma automática.

> [!NOTE]
> La opción Reinicio programado realiza un reinicio automático gradual de los nodos de clúster con revisiones durante un período de 12 a 24 horas y tiene en cuenta las consideraciones de alta disponibilidad, dominio de actualización y dominio de error. El reinicio programado no termina las cargas de trabajo en ejecución, pero puede quitar provisionalmente la capacidad del clúster cuando los nodos no están disponibles, lo que produce tiempos de procesamiento más largos. 

## <a name="next-steps"></a>Pasos siguientes

Para conocer los pasos específicos sobre cómo usar las acciones de script, consulte las secciones siguientes en [Personalización de los clústeres de HDInsight basado en Linux mediante acciones de script](hdinsight-hadoop-customize-cluster-linux.md):

- [Uso de una acción de script durante la creación de un clúster](hdinsight-hadoop-customize-cluster-linux.md#script-action-during-cluster-creation)
- [Aplicación de una acción de script a un clúster en ejecución](hdinsight-hadoop-customize-cluster-linux.md#script-action-to-a-running-cluster)

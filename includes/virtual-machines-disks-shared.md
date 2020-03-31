---
title: archivo de inclusión
description: archivo de inclusión
services: virtual-machines
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 02/18/2020
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: a14ae76e15c1adb59917e61fbcbdaa34a7efa2d8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "77472030"
---
Los discos compartidos de Azure (versión preliminar) son una nueva característica de Azure Managed Disks que permite adjuntar un disco administrado de Azure en varias máquinas virtuales (VM) al mismo tiempo. Si adjunta un disco administrado en varias VM, podrá implementar nuevas aplicaciones en clúster o migrar las existentes a Azure.

## <a name="how-it-works"></a>Funcionamiento

Las VM del clúster pueden leer o escribir en el disco adjunto en función de la reserva elegida por la aplicación en clúster mediante las [reservas persistentes de SCSI](https://www.t10.org/members/w_spc3.htm) (SCSI PR). SCSI PR es un conocido estándar del sector aprovechado por aplicaciones que se ejecutan en el entorno local de la red de área de almacenamiento (SAN). La habilitación de SCSI PR en un disco administrado le permite migrar estas aplicaciones a Azure tal cual.

Los discos administrados con discos compartidos habilitados ofrecen almacenamiento en bloque compartido, al que pueden acceder varias VM, y se exponen como números de unidad lógica (LUN). Los LUN se presentan como un iniciador (VM) de un destino (disco). Estos LUN tienen el aspecto de un almacenamiento conectado directamente (DAS) o de una unidad local en la VM.

Los discos administrados con discos compartidos habilitados no ofrecen de forma nativa un sistema de archivos totalmente administrado al que se pueda obtener acceso mediante SMB/NFS. Deberá usar un administrador de clústeres, como el Clúster de conmutación por error de Windows Server (WSFC) o Pacemaker, que controle la comunicación del nodo de clúster, así como el bloqueo de escritura.

## <a name="limitations"></a>Limitaciones

[!INCLUDE [virtual-machines-disks-shared-limitations](virtual-machines-disks-shared-limitations.md)]

## <a name="disk-sizes"></a>Tamaños de disco

[!INCLUDE [virtual-machines-disks-shared-sizes](virtual-machines-disks-shared-sizes.md)]

## <a name="sample-workloads"></a>Ejemplo de cargas de trabajo

### <a name="windows"></a>Windows

La mayoría de los clústeres basados en Windows se compilan en WSFC, que controla toda la infraestructura central de la comunicación del nodo de clúster, lo que permite que las aplicaciones aprovechen los patrones de acceso en paralelo. WSFC habilita tanto las opciones basadas en CSV como las que no están basadas en CSV en función de la versión de Windows Server. Para obtener más información, consulte [Creación de un clúster de conmutación por error](https://docs.microsoft.com/windows-server/failover-clustering/create-failover-cluster).

Entre las aplicaciones populares que se ejecutan en WSFC se incluyen:

- Instancias del clúster de conmutación por error (FCI) de SQL Server Versions
- Servidor de archivos de escalabilidad horizontal (SoFS)
- Servidor de archivos para uso general (carga de trabajo IW)
- Disco de perfil de usuario del servidor de Escritorio remoto (RDS UPD)
- ASCS/SCS de SAP

### <a name="linux"></a>Linux

Los clústeres de Linux pueden aprovechar administradores de clústeres como [Pacemaker](https://wiki.clusterlabs.org/wiki/Pacemaker). Pacemaker se basa en [Corosync](http://corosync.github.io/corosync/), lo que permite las comunicaciones de clúster para las aplicaciones implementadas en entornos de alta disponibilidad. Algunos sistemas de archivos en clúster comunes incluyen [ocfs2](https://oss.oracle.com/projects/ocfs2/) y [gfs2](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html/global_file_system_2/ch-overview-gfs2). Puede manipular las reservas y los registros mediante utilidades como [fence_scsi](http://manpages.ubuntu.com/manpages/eoan/man8/fence_scsi.8.html) y [sg_persist](https://linux.die.net/man/8/sg_persist).

## <a name="persistent-reservation-flow"></a>Flujo de reserva persistente

En el siguiente diagrama se ilustra un ejemplo de aplicación de base de datos en clúster de dos nodos que aprovecha SCSI PR para habilitar la conmutación por error de un nodo a otro.

![Clúster de dos nodos. Una aplicación que se ejecuta en el clúster controla el acceso al disco.](media/virtual-machines-disks-shared-disks/shared-disk-updated-two-node-cluster-diagram.png)

El flujo es el siguiente:

1. La aplicación en clúster que se ejecuta tanto en la VM1 de Azure como en la VM2 registra su intención para leer o escribir en el disco.
1. La instancia de la aplicación en la VM1 toma una reserva exclusiva para escribir en el disco.
1. Esta reserva se aplica en el disco de Azure y la base de datos ya puede escribir en el disco de forma exclusiva. Las escrituras de la instancia de la aplicación en la VM2 no se realizarán correctamente.
1. Si la instancia de la aplicación en la VM1 deja de funcionar, la instancia en la VM2 ya puede iniciar una conmutación por error de la base de datos y hacerse cargo del disco.
1. Ahora, esta reserva se aplica en el disco de Azure y este ya no aceptará escrituras de la VM1. Solo aceptará escrituras de la VM2.
1. La aplicación en clúster puede completar la conmutación por error de la base de datos y atender las solicitudes de la VM2.

En el diagrama siguiente se ilustra otra carga de trabajo en clúster común que consta de varios nodos que leen datos del disco para la ejecución de procesos paralelos, como el entrenamiento de modelos de Machine Learning.

![Clúster de VM de cuatro nodos; cada nodo registra la intención que hay que escribir y la aplicación toma la reserva exclusiva para controlar correctamente los resultados de escritura.](media/virtual-machines-disks-shared-disks/shared-disk-updated-machine-learning-trainer-model.png)

El flujo es el siguiente:

1. La aplicación en clúster que se ejecuta en todas las VM registra la intención de leer o escribir en el disco.
1. La instancia de la aplicación en la VM1 toma una reserva exclusiva para escribir en el disco mientras abre las lecturas en el disco de otras VM.
1. Esta reserva se aplica en el disco de Azure.
1. Ya se pueden leer todos los nodos del clúster desde el disco. Solo un nodo reescribe los resultados en el disco en nombre de los demás nodos del clúster.
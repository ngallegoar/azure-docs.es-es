---
title: Exclusión de discos de máquina virtual de Hyper-V de la recuperación ante desastres en Azure con Azure Site Recovery
description: Cómo excluir discos de máquina virtual de Hyper-V de la replicación en Azure con Azure Site Recovery.
author: mayurigupta13
manager: rochakm
ms.topic: conceptual
ms.author: mayg
ms.date: 11/12/2019
ms.openlocfilehash: 373cffe96119af4a2fc0d74e2090e6cc24dcaf8f
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "86131244"
---
# <a name="exclude-disks-from-replication"></a>Excluir discos de la replicación

En este artículo se describe cómo excluir discos al replicar máquinas virtuales de Hyper-V en Azure. Es posible que quiera excluir discos de la replicación por varios motivos:

- Asegurarse de que los datos sin importancia que se abandonaron en el disco excluido no se repliquen.
- Optimizar el ancho de banda de replicación consumido o los recursos del lado de destino al excluir los discos que no necesita que se repliquen.
- Ahorrar recursos de almacenamiento y red al no replicar los datos que no necesita.

Antes de excluir los discos de la replicación:

- [Obtenga más información](exclude-disks-replication.md) sobre cómo excluir discos.
- Revise los [escenarios de exclusión comunes ](exclude-disks-replication.md#typical-scenarios) y los [ejemplos](exclude-disks-replication.md#example-1-exclude-the-sql-server-tempdb-disk) que muestran cómo la exclusión de un disco afecta a la replicación, la conmutación por error y la conmutación por recuperación.

## <a name="before-you-start"></a>Antes de comenzar

Antes de empezar, tenga en cuenta lo siguiente:

- **Replication** (Replicación): De manera predeterminada, se replican todos los discos de una máquina.
- **Tipo de disco**:
    - Puede excluir los discos básicos de la replicación.
    - No se pueden excluir los discos del sistema operativo.
    - Se recomienda no excluir discos dinámicos. Site Recovery no puede identificar cuál disco VHD es básico o dinámico en la máquina virtual invitada.  Si no se excluyen todos los discos del volumen dinámico dependientes, el disco dinámico protegido aparecerá como erróneo en la máquina virtual de conmutación por error y no se podrá acceder a los datos de ese disco.
- **Agregar/quitar/excluir discos**: una vez habilitada la replicación, no puede agregar, quitar ni excluir discos de la replicación. Si quiere agregar, quitar o excluir un disco, deberá deshabilitar la protección de la máquina virtual y volver a habilitarla.
- **Conmutación por error**: después de la conmutación por error, si las aplicaciones conmutadas por error necesitan excluir discos para poder funcionar, debe crear dichos discos manualmente. También puede integrar Azure Automation en un plan de recuperación para crear el disco durante la conmutación por error de la máquina.
- **Conmutación por recuperación**: cuando conmuta por recuperación al sitio local después de la conmutación por error, los discos que creó manualmente en Azure no se conmutarán por recuperación. Por ejemplo, si realiza la conmutación por error de tres discos y crea dos directamente en una máquina virtual de Azure, solo se conmutarán por recuperación tres discos que se conmuten por error. No puede incluir los discos creados manualmente en la conmutación por recuperación ni en la replicación inversa de las máquinas virtuales.

## <a name="exclude-disks"></a>Exclusión de discos

1. Para excluir discos cuando [habilita la replicación](./hyper-v-azure-tutorial.md) para una máquina virtual de Hyper-V, después de seleccionar las máquinas virtuales que quiere replicar, en la página **Habilitar replicación** > **Propiedades** > **Configurar propiedades**, revise la columna **Discos que se replicarán**. De forma predeterminada se seleccionan todos los discos para la replicación.
2. Si no quiere replicar un disco específico, en **Discos que se replicarán**, anule la selección de los discos que quiera excluir. 

    ![Excluir discos de la replicación](./media/hyper-v-exclude-disk/enable-replication6-with-exclude-disk.png)


## <a name="next-steps"></a>Pasos siguientes
Después de que la implementación esté configurada y en ejecución, [obtenga más información](failover-failback-overview.md) sobre los diferentes tipos de conmutación por error.

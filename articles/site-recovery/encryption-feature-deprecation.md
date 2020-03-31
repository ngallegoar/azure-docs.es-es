---
title: Desuso de característica de cifrado de datos de Azure Site Recovery | Microsoft Docs
description: Detalles sobre la característica de cifrado de datos de Azure Site Recovery
services: site-recovery
author: rajani-janaki-ram
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 11/15/2019
ms.author: rajanaki
ms.openlocfilehash: 5e74466891a5926d8ae8feb3c1c48348ecf3cfe6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "74134400"
---
# <a name="deprecation-of-site-recovery-data-encryption-feature"></a>Desuso de la característica de cifrado de datos de Site Recovery

En este documento se describen los detalles de desuso y la acción correctiva que debe realizar si usa la característica de cifrado de datos de Site Recovery al configurar la recuperación ante desastres de máquinas virtuales de Hyper-V en Azure. 

## <a name="deprecation-information"></a>Información sobre el desuso


El cifrado de datos de Site Recovery estaba disponible para los clientes que protegen máquinas virtuales de Hyper-V para asegurarse de que los datos replicados estaban protegidos contra amenazas de seguridad. Esta característica estará en desuso el **30 de diciembre de 2019**. Se está reemplazando por la característica más avanzada [Cifrado en reposo](https://azure.microsoft.com/blog/azure-site-recovery-encryption-at-rest/), que usa [Storage Service Encryption](https://docs.microsoft.com/azure/storage/common/storage-service-encryption) (SSE). Con SSE, los datos se cifran antes de guardarse en el almacenamiento y se descifran en la recuperación y, tras la conmutación por error en Azure, las máquinas virtuales se ejecutarán desde las cuentas de almacenamiento cifradas, lo que permite un mejor objetivo de tiempo de recuperación (RTO).

Tenga en cuenta que si es un cliente que usa esta característica, ya habrá recibido información con los detalles de desuso y los pasos de corrección. 


## <a name="what-are-the-implications"></a>¿Qué implica este cambio?

Después del **30 de diciembre de 2019**, cualquier máquina virtual que siga usando la característica de cifrado retirada no podrá realizar la conmutación por error. 

## <a name="required-action"></a>Acción necesaria
Para continuar con las operaciones de conmutación por error correctas y las replicaciones, siga los pasos que se mencionan a continuación:

Siga estos pasos para cada máquina virtual: 
1.  [Deshabilite la replicación](https://docs.microsoft.com/azure/site-recovery/site-recovery-manage-registration-and-protection#disable-protection-for-a-hyper-v-virtual-machine-replicating-to-azure-using-the-system-center-vmm-to-azure-scenario).
2.  [Cree una directiva de replicación nueva](https://docs.microsoft.com/azure/site-recovery/hyper-v-azure-tutorial#set-up-a-replication-policy).
3.  [Habilite la replicación](https://docs.microsoft.com/azure/site-recovery/hyper-v-vmm-azure-tutorial#enable-replication) y seleccione una cuenta de almacenamiento con SSE habilitado.

Después de completar la replicación inicial en las cuentas de almacenamiento con SSE habilitado, las máquinas virtuales utilizarán el cifrado en reposo con Azure Site Recovery.


## <a name="next-steps"></a>Pasos siguientes
Planee la realización de los pasos de corrección y ejecútelos lo más pronto posible. En caso de que tenga consultas relacionadas, póngase en contacto con Soporte técnico de Microsoft. Para más información sobre el escenario de Hyper-V a Azure, visite [este sitio](hyper-v-vmm-architecture.md).


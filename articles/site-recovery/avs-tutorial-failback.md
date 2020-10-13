---
title: Conmutación por recuperación de máquinas virtuales de Azure VMware Solution desde Azure con Azure Site Recovery
description: Aprenda a realizar la conmutación por recuperación a la nube privada de Azure VMware Solution después de la conmutación por error a Azure durante la recuperación ante desastres.
author: Harsha-CS
manager: rochakm
ms.service: site-recovery
ms.topic: tutorial
ms.date: 09/30/2020
ms.author: harshacs
ms.custom: MVC
ms.openlocfilehash: fb14e647d3444f2f0d0cb86901f93582a18848f5
ms.sourcegitcommit: 5abc3919a6b99547f8077ce86a168524b2aca350
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/07/2020
ms.locfileid: "91814291"
---
# <a name="fail-back-vms-to-azure-vmware-solution-private-cloud"></a>Conmutación por recuperación de máquinas virtuales a la nube privada de Azure VMware Solution

En este artículo se describe cómo realizar la conmutación por recuperación de máquinas virtuales de Azure en una nube privada de Azure VMware Solution, después de la [conmutación por error](avs-tutorial-failover.md) de máquinas virtuales de Azure VMware Solution a Azure con [Azure Site Recovery](site-recovery-overview.md). Después de la conmutación por recuperación, habilite la replicación para que las máquinas virtuales de Azure VMware Solution comiencen a replicarse en Azure.

## <a name="before-you-start"></a>Antes de comenzar

1. Obtenga más información sobre la [conmutación por recuperación de VMware](failover-failback-overview.md#vmwarephysical-reprotectionfailback). 
2. Asegúrese de que ha recibido y completado los pasos para [preparar la conmutación por recuperación](vmware-azure-prepare-failback.md) y que se hayan implementado todos los componentes necesarios. Los componentes incluyen un servidor de procesos en Azure, un servidor de destino maestro y una conexión VPN de sitio a sitio (o emparejamiento privado de ExpressRoute) para la conmutación por recuperación.
3. Asegúrese de que ha completado los [requisitos](avs-tutorial-reprotect.md#before-you-begin) para la reprotección y la conmutación por recuperación, y de que ha [habilitado la reprotección](avs-tutorial-reprotect.md#enable-reprotection) de máquinas virtuales de Azure, de modo que se repliquen desde Azure a la nube privada de Azure VMware Solution. Las máquinas virtuales deben estar en un estado de replicación para realizar la conmutación por recuperación.




## <a name="run-a-failover-to-fail-back"></a>Ejecución de una conmutación por error a una conmutación por recuperación

1. Asegúrese de que las máquinas virtuales de Azure están nuevamente protegidas y se replican en la nube privada de Azure VMware Solution.
    - Una máquina virtual necesita al menos un punto de recuperación para realizar la conmutación por recuperación.
    - Si realiza la conmutación por recuperación de un plan de recuperación, todas las máquinas del plan deben tener al menos un punto de recuperación.
2. En el almacén > **Elementos replicados**, seleccione la máquina virtual. Haga clic con el botón derecho en la máquina virtual > **Conmutación por error no planeada** .
3. En **Confirmar conmutación por error**, compruebe la dirección de conmutación por error (de Azure).
4. Seleccione el punto de recuperación que desee usar para la conmutación por error.
    - Se recomienda usar el punto de recuperación **Más reciente**. El punto coherente con la aplicación se encuentra detrás del último momento dado y provoca cierta pérdida de datos.
    - **Más reciente** es un punto de recuperación coherente con los bloqueos.
    - Con **Más reciente**, una máquina virtual conmuta por error a su punto disponible más reciente. Si tiene un grupo de replicación para coherencia con varias máquinas virtuales dentro de un plan de recuperación, cada máquina virtual de este grupo conmutará por error a su punto independiente más reciente.
    - Si usa un punto de recuperación coherente con la aplicación, cada máquina virtual conmuta por recuperación a su punto disponible más reciente. Si un plan de recuperación tiene un grupo de replicación, cada grupo se recuperará en su punto de recuperación disponible común.
5. La conmutación por error comienza. Site Recovery apaga las máquinas virtuales de Azure.
6. Una vez completada la conmutación por error, compruebe que todo funciona según lo previsto. Compruebe que las máquinas virtuales de Azure están apagadas. 
7. Una vez comprobado todo, haga clic con el botón derecho en la máquina virtual > **Confirmar** para finalizar el proceso de conmutación por error. Confirmar quita la máquina virtual de Azure conmutada por error. 

> [!NOTE]
> En el caso de las máquinas virtuales Windows, Site Recovery deshabilita las herramientas de VMware durante la conmutación por error. Durante la conmutación por recuperación de la máquina virtual Windows, se vuelven a habilitar las herramientas de VMware. 




## <a name="reprotect-from-azure-vmware-solution-to-azure"></a>Reprotección desde Azure VMware Solution en Azure

Después de confirmar la conmutación por recuperación, se eliminan las máquinas virtuales de Azure. La máquina virtual vuelve a la nube privada de Azure VMware Solution, pero no está protegida. Para volver a iniciar la replicación de máquinas virtuales en Azure, haga lo siguiente:

1. En el almacén > **Elementos replicados**, seleccione las máquinas virtuales conmutadas por recuperación y luego seleccione **Volver a proteger**.
2. Especifique el servidor de procesos utilizado para volver a enviar datos a Azure.
3. Seleccione **Aceptar** para volver a proteger el trabajo.

> [!NOTE]
> Una vez que una máquina virtual de Azure VMware Solution se inicia, el agente tarda un máximo de 15 en realizar el registro en el servidor de configuración. Durante este tiempo, la reprotección produce errores y devuelve un mensaje de error que indica que el agente no está instalado. Si esto sucede, espere unos minutos y vuelva a realizar la protección.

## <a name="next-steps"></a>Pasos siguientes

Una vez que el trabajo de reprotección finaliza, la máquina virtual de Azure VMware Solution se replica en Azure. Si fuera necesario, podría [ejecutar otra conmutación por error](avs-tutorial-failover.md) en Azure.


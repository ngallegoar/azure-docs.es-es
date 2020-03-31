---
title: Arquitectura de recuperación ante desastres de máquinas virtuales de VMware en Azure Site Recovery
description: En este artículo se proporciona información general sobre los componentes y la arquitectura que se usan al configurar la recuperación ante desastres de máquinas virtuales locales de VMware en Azure con Azure Site Recovery.
author: rayne-wiselman
ms.service: site-recovery
services: site-recovery
ms.topic: conceptual
ms.date: 11/06/2019
ms.author: raynew
ms.openlocfilehash: ccf258594aa68fc9b5d0189c9ada640078e0ba6f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "76514874"
---
# <a name="vmware-to-azure-disaster-recovery-architecture"></a>Arquitectura de recuperación ante desastres de VMware a Azure

En este artículo se describe la arquitectura y los procesos usados al implementar la replicación, la conmutación por error y la recuperación de máquinas virtuales (VM) de VMware entre un sitio de VMware local y Azure en la recuperación ante desastres, mediante el servicio [Azure Site Recovery](site-recovery-overview.md).


## <a name="architectural-components"></a>Componentes de la arquitectura

En la tabla y el gráfico siguientes se proporciona una visión general de los componentes que se usaron para la recuperación ante desastres de VMware en Azure.

**Componente** | **Requisito** | **Detalles**
--- | --- | ---
**Azure** | Una suscripción a Azure, una cuenta de Azure Storage para almacenamiento en caché, un disco administrado y una red de Azure. | Los datos replicados desde las máquinas virtuales locales se almacenan en Azure Storage. Las máquinas virtuales de Azure se crean con los datos replicados cuando se ejecuta una conmutación por error desde el entorno local en Azure. Las máquinas virtuales de Azure se conectan a la red virtual de Azure cuando se crean.
**Equipo del servidor de configuración** | Una sola máquina local. Se recomienda ejecutarla como una máquina virtual de VMware que pueda implementarse desde una plantilla de OVF descargada.<br/><br/> La máquina ejecuta todos los componentes locales de Site Recovery, incluido el servidor de configuración, el servidor de procesos y el servidor de destino maestro. | **Servidor de configuración**: coordina las comunicaciones entre el entorno local y Azure, además de administrar la replicación de datos.<br/><br/> **Servidor de proceso**: Se instala de forma predeterminada en el servidor de configuración. Recibe los datos de la replicación; los optimiza mediante almacenamiento en caché, compresión y cifrado, y los envía a Azure Storage. El servidor de procesos también instala Azure Site Recovery Mobility Service en las máquinas virtuales que se van a replicar y realiza la detección automática de las máquinas locales. A medida que crece la implementación, puede agregar más servidores de procesos independientes para controlar mayores volúmenes de tráfico de replicación.<br/><br/> **Servidor de destino principal**: Se instala de forma predeterminada en el servidor de configuración. Controla los datos de replicación durante la conmutación por recuperación desde Azure. En el caso de las implementaciones de gran tamaño, puede agregar un servidor de destino maestro independiente adicional para la conmutación por recuperación.
**Servidores de VMware** | Las máquinas virtuales VMware se hospedan en servidores ESXi de vSphere locales. Se recomienda un servidor vCenter para administrar los hosts. | Durante la implementación de Site Recovery, se agregan servidores VMware al almacén de Recovery Services.
**Máquinas replicadas** | Mobility Service está instalado en cada una de las máquinas virtuales de VMware que se van a replicar. | Se recomienda permitir la instalación automática desde el servidor de procesos. Si lo desea, también puede instalar manualmente el servicio o usar un método de implementación automatizada, como Configuration Manager.

**Arquitectura de VMware a Azure**

![Componentes](./media/vmware-azure-architecture/arch-enhanced.png)



## <a name="replication-process"></a>Proceso de replicación

1. Al habilitar la replicación para una VM, comienza la replicación inicial en el almacenamiento de Azure mediante la directiva de replicación especificada. Tenga en cuenta lo siguiente:
    - Para las VM de VMware, la replicación es de nivel de bloque, casi continua, mediante el agente del servicio de movilidad que se ejecuta en la VM.
    - Se aplica la configuración de directivas de replicación:
        - **Umbral de RPO**. Esta configuración no afecta a la replicación. Ayuda con la supervisión. Se genera un evento y, opcionalmente, se envía un correo electrónico, si el RPO actual supera el límite del umbral que especifique.
        - **Retención de punto de recuperación**. Esta configuración especifica cuánto tiempo atrás quiere ir cuando se produce una interrupción. La retención máxima en almacenamiento premium es de 24 horas. En el almacenamiento estándar es de 72 horas. 
        - **Instantáneas coherentes con la aplicación**. Las instantáneas coherentes con la aplicación pueden tomarse cada 1 a 12 horas, según las necesidades de su aplicación. Son instantáneas de blob de Azure estándar. El agente de movilidad que se ejecuta en una VM solicita una instantánea de VSS de acuerdo con esta configuración, y marca ese momento como punto coherente con la aplicación en el flujo de replicación.

2. El tráfico se replica en los puntos de conexión públicos de Azure Storage a través de Internet. Como alternativa, puede usar Azure ExpressRoute con [emparejamiento de Microsoft](../expressroute/expressroute-circuit-peerings.md#microsoftpeering). No se admite la replicación del tráfico entre un sitio local y Azure a través de una red privada virtual (VPN) de sitio a sitio.
3. Una vez terminada la replicación inicial, comienza la de los cambios incrementales en Azure. Los cambios marcados para una máquina se envían al servidor de procesos.
4. Comunicación se realiza como se indica a continuación:

    - Las máquinas virtuales se comunican con el servidor de configuración local en el puerto HTTPS 443 entrante para la administración de la replicación.
    - El servidor de configuración organiza la replicación con Azure a través del puerto HTTPS 443 saliente.
    - Las máquinas virtuales envían datos de replicación al servidor de procesos (que se ejecuta en la máquina del servidor de configuración) en el puerto HTTPS 9443 entrante. Este puerto se puede modificar.
    - El servidor de procesos recibe datos de replicación, los optimiza y los cifra para enviarlos después a Azure Storage a través del puerto 443 de salida.
5. Los datos de replicación registran el primer aterrizaje en una cuenta de almacenamiento en caché de Azure. Estos registros se procesan y los datos se almacenan en un disco administrado de Azure (denominado disco de inicialización ASR). Los puntos de recuperación se crean en ese disco.




**Proceso de replicación de VMware a Azure**

![Proceso de replicación](./media/vmware-azure-architecture/v2a-architecture-henry.png)

## <a name="failover-and-failback-process"></a>Proceso de conmutación por error y conmutación por recuperación

Una vez que la replicación está configurada y tras ejecutar una exploración de la recuperación ante desastres (conmutación por error de prueba) para comprobar que todo funciona según lo previsto, puede ejecutar la conmutación por error y la conmutación por recuperación, según sea necesario.

1. Va a conmutar por error una única máquina o crear planes de recuperación para conmutar por error varias máquinas virtuales. La ventaja de un plan de recuperación en lugar de la conmutación por error de una única máquina incluye:
    - Puede modelar las dependencias de aplicaciones con la inclusión de todas las máquinas virtuales por la aplicación en un único plan de recuperación.
    - Puede agregar scripts, runbooks de Azure y ponerlos en pausa para acciones manuales.
2. Después de desencadenar la conmutación por error inicial, debe confirmarla para que se inicie. Para ello, acceda a la carga de trabajo desde la máquina virtual de Azure.
3. Cuando el sitio local principal esté disponible de nuevo, podrá prepararse para la conmutación por recuperación. Para realizar la conmutación por recuperación, debe configurar una infraestructura de conmutación por recuperación, que incluya:

    * **Servidor de procesos temporal de Azure**: para realizar una conmutación por recuperación desde Azure, debe configurar una máquina virtual de Azure para que actúe como servidor de procesos y controle la replicación desde Azure. Dicha máquina virtual se puede eliminar cuando finalice la conmutación por recuperación.
    * **Conexión VPN**: para la conmutación por recuperación, necesita una conexión VPN o ExpressRoute desde la red de Azure al sitio local.
    * **Servidor de destino maestro independiente**: de manera predeterminada, el servidor de destino maestro que se instaló con el servidor de configuración en la máquina virtual local de VMware controla la conmutación por recuperación. Si necesita realizar la conmutación por recuperación en grandes volúmenes de tráfico, debe configurar un servidor de destino maestro local diferente para este propósito.
    * **Directiva de conmutación por recuperación**: para replicar de nuevo en el sitio local, necesita una directiva de conmutación por recuperación. Esta directiva se crea automáticamente cuando crea una directiva de replicación entre el entorno local y Azure.
4. Una vez instalados los componentes, la conmutación por recuperación se produce en tres acciones:

    - Fase 1 Vuelva a proteger las máquinas virtuales de modo que realicen la replicación desde Azure de vuelta a las máquinas virtuales VMware locales.
    -  Fase 2: Ejecute una conmutación por error en el sitio local.
    - Fase 3: Una vez que las cargas de trabajo han conmutado por recuperación, debe habilitar de nuevo la replicación de las máquinas virtuales locales.
    
 
**Conmutación por recuperación VMware desde Azure**

![Conmutación por recuperación](./media/vmware-azure-architecture/enhanced-failback.png)


## <a name="next-steps"></a>Pasos siguientes

Siga [este tutorial](vmware-azure-tutorial.md) para habilitar la replicación de VMware en Azure.

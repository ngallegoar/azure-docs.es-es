---
title: Habilitación de máquinas virtuales de VMware para la recuperación ante desastres con Azure Site Recovery
description: En este artículo, se describe cómo habilitar la replicación de máquinas virtuales de VMware para la recuperación ante desastres mediante el servicio Azure Site Recovery.
author: Rajeswari-Mamilla
ms.service: site-recovery
ms.date: 06/28/2019
ms.topic: conceptual
ms.author: ramamill
ms.openlocfilehash: 10b3e572ec61d1eff342f24a6a5a7bcba6276983
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "79228960"
---
# <a name="enable-replication-to-azure-for-vmware-vms"></a>Habilitación de máquinas virtuales de VMware en Azure

En este artículo se describe cómo habilitar la replicación de máquinas virtuales locales de VMware en Azure.

## <a name="resolve-common-issues"></a>Resolución de problemas comunes

* Cada disco debe tener menos de 4 TB.
* El disco del sistema operativo debe ser un disco básico, no uno dinámico.
* En el caso de las máquinas virtuales de generación 2 o habilitadas para UEFI, la familia del sistema operativo debe ser Windows y el disco de arranque debe tener menos de 300 GB.

## <a name="prerequisites"></a>Prerrequisitos

En este artículo se supone que ha:

- [Configurado el entorno de origen local](vmware-azure-set-up-source.md).
- [Configurado el entorno de destino en Azure](vmware-azure-set-up-target.md).
- [Comprobado los requisitos y requisitos previos](vmware-physical-azure-support-matrix.md) antes de empezar. Aspectos importantes para tener en cuenta:
    - [Sistemas operativos admitidos](vmware-physical-azure-support-matrix.md#replicated-machines) para máquinas replicadas.
    - Compatibilidad con [almacenamiento y disco](vmware-physical-azure-support-matrix.md#storage).
    - [Requisitos de Azure](vmware-physical-azure-support-matrix.md#azure-vm-requirements) con los que deben cumplir las máquinas locales.


## <a name="before-you-start"></a>Antes de comenzar
A la hora de replicar máquinas virtuales de VMware, tenga en cuenta esta información:

* Su cuenta de usuario de Azure debe tener ciertos [permisos](site-recovery-role-based-linked-access-control.md#permissions-required-to-enable-replication-for-new-virtual-machines) para habilitar la replicación de una nueva máquina virtual en Azure.
* Las máquinas virtuales de VMware se detectan cada 15 minutos. Tras la detección, es posible que las máquinas virtuales tarden 15 minutos o más en aparecer en Azure Portal. Del mismo modo, la detección puede tardar 15 minutos o más si se agregan un servidor vCenter o un host de vSphere nuevos.
* Los cambios de entorno en la máquina virtual (como la instalación de herramientas de VMware) pueden tardar 15 minutos o más en actualizarse en el portal.
* Puede comprobar la hora de la última detección de máquinas virtuales de VMware: Vea el campo **Último contacto a las** en la página **Servidores de configuración** del servidor vCenter o el host de vSphere.
* Para agregar máquinas virtuales para la replicación sin esperar a la detección programada, resalte el servidor de configuración (pero no haga clic en él) y seleccione **Actualizar**.
* Al habilitar la replicación, si la máquina virtual está preparada, el servidor de procesos instala automáticamente Mobility Service de Azure Site Recovery en ella.

## <a name="enable-replication"></a>Habilitar replicación

Antes de seguir los pasos de esta sección, tenga en cuenta la siguiente información:
* Azure Site Recovery ahora replica directamente en discos administrados para todas las nuevas replicaciones. El servidor de procesos escribe los registros de replicación en una cuenta de almacenamiento en caché de la región de destino. Estos registros se utilizan para crear puntos de recuperación en las réplicas de discos administrados que tengan la convención de nomenclatura asrseeddisk.
* La compatibilidad con PowerShell para replicar discos administrados está disponible a partir de la [versión 2.0.0 del módulo de Az.RecoveryServices en adelante](https://www.powershellgallery.com/packages/Az.RecoveryServices/2.0.0-preview). 
* En el momento de la conmutación por error, el punto de recuperación que se selecciona se usa para crear el disco administrado de destino.
* Las máquinas virtuales configuradas previamente para replicar en cuentas de almacenamiento de destino no se ven afectadas.
* La replicación en cuentas de almacenamiento para una nueva máquina virtual solo está disponible a través de una API de transferencia de estado representacional (REST) y PowerShell. Use las versiones 2016-08-10 o 2018-01-10 de la API de REST de Azure para replicar en cuentas de almacenamiento.

Siga estos pasos para habilitar la replicación:
1. Vaya al **Paso 2: Replicar la aplicación** > **Origen**. Después de habilitar la replicación por primera vez, seleccione **+Replicar** en el almacén para habilitar la replicación de otras máquinas virtuales.
2. En la página **Origen** > **Origen**, seleccione el servidor de configuración.
3. En **Tipo de máquina**, seleccione **Máquinas virtuales** o **Máquinas físicas**.
4. En **vCenter/vSphere Hypervisor**, seleccione el servidor vCenter que administra el host de vSphere, o bien seleccione el host. Este valor no es relevante si va a replicar equipos físicos.
5. Seleccione el servidor de procesos. Si no se han creado otros servidores de procesos, el servidor de procesos integrado del servidor de configuración está disponible en la lista desplegable. El estado de mantenimiento de cada servidor de procesos se indica según los límites recomendados y otros parámetros. Elija un servidor de procesos correcto. No se puede elegir un servidor de procesos [crítico](vmware-physical-azure-monitor-process-server.md#process-server-alerts). Puede [localizar y solucionar](vmware-physical-azure-troubleshoot-process-server.md) los errores **o** configurar un [servidor de procesos de escalabilidad horizontal](vmware-azure-set-up-process-server-scale.md).
    ![Ventana Habilitar replicación: Origen](media/vmware-azure-enable-replication/ps-selection.png)

> [!NOTE]
> A partir de la [versión 9.24](service-updates-how-to.md#links-to-currently-supported-update-rollups), se han incorporado alertas adicionales para mejorar las alertas de estado del servidor de procesos. Actualice los componentes de Site Recovery a la versión 9.24 o posteriores para que se generen todas las alertas.

6. En **Destino**, seleccione la suscripción y el grupo de recursos donde quiere crear las máquinas virtuales conmutadas por error. Elija el modelo de implementación que quiere usar en Azure para las máquinas virtuales conmutadas por error.
2. Seleccione la red y la subred de Azure a las que se van a conectar las máquinas virtuales de Azure después de la conmutación por error. La red debe estar en la misma región que el almacén del servicio Site Recovery.

   Seleccione **Configurar ahora para las máquinas seleccionadas** para aplicar la configuración de red a todas las máquinas virtuales que seleccione para protección. Seleccione **Configurar más tarde** para seleccionar la red de Azure por máquina virtual. Si no dispone de una red, debe crear una. Para crear una red mediante Azure Resource Manager, seleccione **Crear nueva**. Seleccione una subred si es aplicable y luego **Aceptar**.
   
   ![Ventana Habilitar replicación: Destino](./media/vmware-azure-enable-replication/enable-rep3.png)

1. En **Máquinas virtuales** > **Seleccionar máquinas virtuales**, seleccione cada máquina virtual que quiera replicar. Solo puede seleccionar aquellas máquinas virtuales para las que se pueda habilitar la replicación. Después, seleccione **Aceptar**. Si no puede ver o seleccionar una máquina virtual concreta, vea [La máquina de origen no aparece en Azure Portal](https://aka.ms/doc-plugin-VM-not-showing) para resolver el problema.

    ![Ventana Habilitar replicación: Seleccionar máquinas virtuales](./media/vmware-azure-enable-replication/enable-replication5.png)

1. En **Propiedades** > **Configurar propiedades**, seleccione la cuenta que usa el servidor de procesos para instalar automáticamente Mobility Service de Site Recovery en la máquina virtual. Además, elija el tipo de disco administrado de destino en el que replicar en función de los patrones de actividad de datos.
10. De forma predeterminada, se replican todos los discos de una máquina virtual de origen. Para excluir discos de la replicación, desactive la casilla **Incluir** de aquellos discos que no quiera replicar. Después, seleccione **Aceptar**. Puede establecer propiedades adicionales más adelante. Obtenga más información sobre la [exclusión de discos](vmware-azure-exclude-disk.md).

    ![Ventana Habilitar replicación: Configurar propiedades](./media/vmware-azure-enable-replication/enable-replication6.png)

1. En **Configuración de replicación** > **Establecer configuración de replicación**, compruebe que se haya seleccionado la directiva de replicación correcta. Puede modificar la configuración de la directiva de replicación en **Configuración** > **Directivas de replicación** > ***nombre de directiva*** > **Editar configuración**. Los cambios aplicados a una directiva también se aplican a las máquinas virtuales nuevas y replicadas.
1. Habilite **Coherencia de múltiples VM** si quiere recopilar las máquinas virtuales en un grupo de replicación. Especifique un nombre para el grupo y seleccione **Aceptar**.

    > [!NOTE]
    >    * Las máquinas virtuales de un grupo de replicación se replican al mismo tiempo y comparten puntos de recuperación coherentes con los bloqueos y coherentes con la aplicación cuando conmutan por error.
    >    * Recopile las máquinas virtuales y los servidores físicos juntos para que reflejen las cargas de trabajo. La habilitación de la coherencia entre varias VM puede afectar al rendimiento de la carga de trabajo. Hágalo únicamente si las máquinas virtuales ejecutan la misma carga de trabajo y necesita coherencia.

    ![Ventana Habilitar replicación](./media/vmware-azure-enable-replication/enable-replication7.png)
    
1. Seleccione **Habilitar replicación**. Puede hacer un seguimiento del progreso del trabajo **Habilitar protección** en **Configuración** > **Trabajos** > **Trabajos de Site Recovery**. La máquina virtual estará preparada para la conmutación por error después de que finalice el trabajo de **Finalizar protección**.

## <a name="view-and-manage-vm-properties"></a>Visualización y administración de las propiedades de la máquina virtual

Después, compruebe las propiedades de la máquina virtual de origen. Recuerde que el nombre de la máquina virtual de Azure debe cumplir los [requisitos para las máquinas virtuales de Azure](vmware-physical-azure-support-matrix.md#replicated-machines).

1. Vaya a **Configuración** > **Elementos replicados** y seleccione la máquina virtual. En la página **Información esencial** se muestra información sobre la configuración y el estado de las máquinas virtuales.
1. En **Propiedades** puede ver la información de replicación y conmutación por error de la máquina virtual.
1. En **Proceso y red** > **Propiedades de Compute**, puede cambiar varias propiedades de la máquina virtual. 

    ![Ventana Propiedades de proceso y red](./media/vmware-azure-enable-replication/vmproperties.png)

    * Nombre de la VM de Azure: modifique el nombre en caso necesario para que cumpla los requisitos de Azure.
    * Tamaño de la VM de destino o Tipo de máquina virtual: el tamaño predeterminado de la máquina virtual se selecciona basándose en unos cuantos parámetros que incluyen el número de discos, el número de tarjetas NIC, el recuento de núcleos de CPU, la memoria y los tamaños de rol de máquina virtual disponibles en la región de Azure de destino. Azure Site Recovery elige el primer tamaño de máquina virtual disponible que satisface todos los criterios. Puede seleccionar otro tamaño de máquina virtual según sea necesario en cualquier momento anterior a la conmutación por error. Tenga en cuenta que el tamaño del disco de la máquina virtual también se basa en el tamaño del disco de origen y solo puede modificarse después de la conmutación por error. Obtenga más información sobre los tamaños de disco y las tasas de IOPS en [Objetivos de escalabilidad y rendimiento para discos de máquinas virtuales con Windows](../virtual-machines/windows/disk-scalability-targets.md).

    *  Grupo de recursos: puede seleccionar un [grupo de recursos](https://docs.microsoft.com/azure/virtual-machines/windows/infrastructure-resource-groups-guidelines) desde el que la máquina virtual forme parte de una conmutación por error posterior. Puede cambiar este valor en cualquier momento antes de la conmutación por error. Después de la conmutación por error, si se migra la máquina virtual a otro grupo de recursos, la configuración de protección de dicha máquina virtual se interrumpe.
    * Conjunto de disponibilidad: puede seleccionar un [conjunto de disponibilidad](https://docs.microsoft.com/azure/virtual-machines/windows/infrastructure-availability-sets-guidelines) si la máquina debe formar parte de una conmutación por error posterior. Al seleccionar un conjunto de disponibilidad, tenga en cuenta la siguiente información:

        * Solo se muestran los conjuntos de disponibilidad que pertenecen al grupo de recursos especificado.  
        * Las máquinas virtuales de distintas redes virtuales no pueden formar parte del mismo conjunto de disponibilidad.
        * Solo las máquinas virtuales del mismo tamaño pueden formar parte de un conjunto de disponibilidad.
1. También puede agregar información sobre la red, la subred y la dirección IP de destino asignadas a la máquina virtual de Azure.
2. En **Discos** puede ver los discos de datos y del sistema operativo en la máquina virtual que se va a replicar.

### <a name="configure-networks-and-ip-addresses"></a>Configuración de redes y direcciones IP

Puede establecer la dirección IP de destino. Si no proporciona ninguna dirección, la máquina virtual conmutada por error usa DHCP. Si establece una dirección que no esté disponible en el momento de la conmutación por error, esta operación no funcionará. Si la dirección está disponible en la red de conmutación por error de prueba, se puede usar la misma dirección IP de destino para la conmutación por error de prueba.

El número de adaptadores de red viene determinado por el tamaño que se especifique para la máquina virtual de destino, de la siguiente manera:

- Si el número de adaptadores de red en la máquina virtual de origen es menor o igual que el número de adaptadores permitido para el tamaño de la máquina virtual de destino, el destino tiene el mismo número de adaptadores que el origen.
- Si el número de adaptadores de la máquina virtual de origen supera el número permitido para el tamaño de la máquina virtual de destino, se usa el tamaño máximo de destino. Por ejemplo, si una máquina virtual de origen tiene dos adaptadores de red y el tamaño de la máquina virtual de destino admite cuatro, la máquina virtual de destino tiene dos adaptadores. Si la máquina virtual de origen tiene dos adaptadores pero el tamaño de destino solo admite uno, la máquina virtual de destino tiene solo un adaptador.
- Si la máquina virtual tiene varios adaptadores de red, todos ellos se conectan a la misma red. Además, el primero de ellos que se muestra en la lista se convierte en el adaptador de red *predeterminado* en la máquina virtual de Azure. 

### <a name="azure-hybrid-benefit"></a>Ventaja híbrida de Azure

Los clientes de Microsoft Software Assurance pueden usar la Ventaja híbrida de Azure para ahorrar en los costos de licencia de equipos Windows Server que se migran a Azure. La ventaja también se aplica a la recuperación ante desastres de Azure. Si es beneficiario, puede asignar la ventaja a la máquina virtual que Site Recovery crea si se produce una conmutación por error. Para ello, sigue estos pasos:
1. Vaya a **Propiedades de proceso y red** en la máquina virtual replicada.
2. Responda cuando se le pregunte si tiene una licencia de Windows Server que le convierta en beneficiario de la Ventaja híbrida de Azure.
3. Confirme que tiene una licencia de Windows Server con Software Assurance que puede usar para aplicar la ventaja a la máquina virtual que se va a crear en la conmutación por error.
4. Guarde la configuración de la máquina virtual replicada.

Más información sobre la [Ventaja híbrida de Azure](https://aka.ms/azure-hybrid-benefit-pricing).



## <a name="next-steps"></a>Pasos siguientes

Una vez que la máquina virtual alcance un estado protegido, pruebe una [conmutación por error](site-recovery-failover.md) para comprobar si la aplicación aparece en Azure.

* Consulte cómo [borrar el registro y la configuración de la protección](site-recovery-manage-registration-and-protection.md) para deshabilitar la replicación.
* Obtenga información sobre cómo [automatizar la replicación de las máquinas virtuales con Powershell](vmware-azure-disaster-recovery-powershell.md).

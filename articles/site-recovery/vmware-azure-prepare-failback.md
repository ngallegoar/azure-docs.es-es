---
title: Preparación de máquinas virtuales de VMware para la reprotección y la conmutación por recuperación con Azure Site Recovery
description: Prepare el entorno para conmutar por recuperación máquinas virtuales de VMware después de una conmutación por error con Azure Site Recovery.
ms.topic: conceptual
ms.date: 12/24/2019
ms.openlocfilehash: 5a330f8cba31640d0116ca3d5ccab352ce5b3509
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "85847741"
---
# <a name="prepare-for-reprotection-and-failback-of-vmware-vms"></a>Preparación para la reprotección y conmutación por recuperación de máquinas virtuales de VMware

Tras una [conmutar por error](site-recovery-failover.md) máquinas virtuales de VMware locales o servidores físicos a Azure, hay que reproteger las máquinas virtuales de Azure que se crearon tras la conmutación por error para que, así, vuelvan a replicarse en el sitio local. Si dispone de una replicación desde Azure a un entorno local, puede conmutar por recuperación realizando una conmutación por error de Azure al entorno local cuando esté listo.

Antes de continuar, compóngase una idea general rápida con este vídeo sobre cómo conmutar por recuperación de Azure a un sitio local.<br /><br />
> [!VIDEO https://channel9.msdn.com/Series/Azure-Site-Recovery/VMware-to-Azure-with-ASR-Video5-Failback-from-Azure-to-On-premises/player]

## <a name="reprotectionfailback-components"></a>Reprotección y conmutación por recuperación de componentes

Para poder reproteger y conmutar por recuperación desde Azure, es necesario disponer de una serie de componentes y configuraciones.

**Componente**| **Detalles**
--- | ---
**Servidor de configuración local** | El servidor de configuración local debe estar en ejecución y conectado a Azure.<br/><br/> La máquina virtual que se conmute por recuperación debe existir en la base de datos del servidor de configuración. Si algún desastre afecta al servidor de configuración, restáurelo con la misma dirección IP para asegurarse de que la conmutación por recuperación funciona.<br/><br/>  Si las direcciones IP de los elementos replicados se retuvieran en la conmutación por error, se deberá establecer una conectividad de sitio a sitio (o una conectividad ExpressRoute) entre las máquinas virtuales de Azure y la NIC de conmutación por recuperación del servidor de configuración. En el caso de las direcciones IP retenidas, el servidor de configuración debe tener dos NIC: una para la conectividad de las máquinas de origen y otra para la conectividad de conmutación por recuperación de Azure. Así, se evitará que los intervalos de direcciones de la subred del origen y las máquinas virtuales conmutadas por error se superpongan.
**Servidor de procesos en Azure** | Necesita un servidor de procesos en Azure para poder conmutar por recuperación a su sitio local.<br/><br/> El servidor de procesos recibe datos de la máquina virtual de Azure protegida y los envía al sitio local.<br/><br/> Se necesita una red de baja latencia entre el servidor de procesos y la máquina virtual protegida, por lo que se recomienda implementar el servidor de procesos en Azure, ya que así logrará un mayor rendimiento de la replicación.<br/><br/> Como prueba de concepto, puede usar el servidor de procesos local y ExpressRoute con emparejamiento privado.<br/><br/> El servidor de procesos debe estar en la red de Azure en la que está la máquina virtual conmutada por error. El servidor de procesos también debe ser capaz de comunicarse con el servidor de configuración local y el servidor de destino maestro.
**Servidor de destino maestro independiente** | El servidor de destino maestro recibe los datos de la conmutación por recuperación y, de forma predeterminada, un servidor de destino maestro de Windows se ejecuta en el servidor de configuración local.<br/><br/> Un servidor de destino maestro puede tener hasta 60 discos conectados. Las máquinas virtuales que se conmutan por recuperación tienen un número global superior a 60 discos, o bien, si se conmutan por recuperación grandes volúmenes de tráfico, cree un servidor de destino maestro independiente para la conmutación por recuperación.<br/><br/> Si los equipos se aglutinan en un grupo de replicación para mantener la coherencia entre varias máquinas virtuales, dichas máquinas virtuales deben ser todas Windows o todas Linux. ¿Por qué? Porque todas las máquinas virtuales de un grupo de replicación deben usar el mismo servidor de destino maestro, y el servidor de destino maestro debe tener el mismo sistema operativo (con la misma versión o una superior) que los de las máquinas replicadas.<br/><br/> El servidor de destino maestro no debe tener ninguna instantánea en sus discos; de lo contrario, la reprotección y la conmutación por recuperación no funcionarán.<br/><br/> El destino maestro no puede tener ninguna controladora SCSI paravirtual. La controladora solo puede ser una controladora de LSI Logic. Sin ella, se produce un error en la reprotección.
**Directiva de replicación de conmutación por recuperación** | Para replicar de nuevo en el sitio local, necesita una directiva de conmutación por recuperación. Esta directiva se crea automáticamente cuando se crea una directiva de replicación en Azure.<br/><br/> La directiva se asocia automáticamente al servidor de configuración. Está establecida en un umbral de RPO de 15 minutos, una retención de puntos de recuperación de 24 horas y una frecuencia de instantánea coherente con la aplicación de 60 minutos. La directiva no se puede modificar. 
**Emparejamiento privado de VPN de sitio a sitio/ExpressRoute** | Para replicar datos, la reprotección y la conmutación por recuperación requieren un emparejamiento privado de VPN de sitio a sitio o de ExpressRoute. 


## <a name="ports-for-reprotectionfailback"></a>Puertos de reprotección/conmutación por recuperación

La reprotección y la conmutación por recuperación precisan que haya abiertos una serie de puertos. En el siguiente gráfico se ilustran los puertos y el flujo de reprotección/conmutación por recuperación.

![Puertos para conmutación por error y conmutación por recuperación](./media/vmware-azure-reprotect/failover-failback.png)


## <a name="deploy-a-process-server-in-azure"></a>Implementación de un servidor de procesos en Azure

1. [Configure un servidor de procesos](vmware-azure-set-up-process-server-azure.md) en Azure para la conmutación por recuperación.
2. Asegúrese de que las máquinas virtuales de Azure pueden acceder al servidor de procesos. 
3. Asegúrese de que la red de emparejamiento privado de conexión VPN de sitio a sitio o de ExpressRoute tiene ancho de banda suficiente para enviar datos desde el servidor de procesos al sitio local.

## <a name="deploy-a-separate-master-target-server"></a>Implementación de un servidor de destino maestro independiente

1. Tenga en cuenta los [requisitos y limitaciones](#reprotectionfailback-components) del servidor de destino maestro.
2. Cree un servidor de destino maestro de [Windows](site-recovery-plan-capacity-vmware.md#deploy-additional-master-target-servers) o de [Linux](vmware-azure-install-linux-master-target.md) para que el sistema operativo coincida con el de las máquinas virtuales que quiere reproteger y conmutar por recuperación.
3. Asegúrese de no usar Storage vMotion en el servidor de destino maestro, o puede que la conmutación por recuperación no se realice. La máquina virtual no se podrá iniciar porque no habrá discos disponibles para ella.
    - Para evitar esto, excluya los servidores de destino maestros de la lista de vMotion.
    - Si un servidor de destino maestro se somete a una tarea de Storage vMotion después de la reprotección, los discos de máquina virtual protegidos conectados a dicho servidor migrarán al destino de la tarea de vMotion. Si intenta conmutar por recuperación después de esto, se produce un error de desconexión de discos porque estos no se detectan. Esto hará que resulte difícil encontrar los discos en las cuentas de almacenamiento. Si esto sucede, encuéntrelos manualmente y conéctelos a la máquina virtual. Después de esto, la máquina virtual local se puede arrancar.

4. Agregue una unidad de retención al servidor de destino maestro Windows existente. Agregue un nuevo disco y formatee la unidad. La unidad de retención se utiliza para detener los momentos dados en los que la máquina virtual se replica de vuelta en el sitio local. Tenga en cuenta los siguientes criterios. Si no se cumplen, la unidad no aparecerá en la lista del servidor de destino maestro:
    - El volumen no se usa para ningún otro propósito (como un destino de replicación) y no está en modo bloqueado.
    - El volumen no es un volumen de caché. El volumen de instalación personalizado del servidor de procesos y el destino maestro no es apto para un volumen de retención. Cuando el servidor de procesos y el destino maestro se instalan en un volumen, este es un volumen de memoria caché del destino maestro.
    - El tipo de sistema de archivos del volumen no es FAT ni FAT32.
    - La capacidad del volumen es distinta de cero.
    - El volumen de retención predeterminado para Windows es el volumen R.
    - El volumen de retención predeterminado para Linux es /mnt/retention.

5. Agregue una unidad si está utilizando un servidor de procesos existente. La nueva unidad debe cumplir los requisitos del último paso. Si la unidad de retención no está presente, no aparece en la lista de selección desplegable en el portal. Después de agregar una unidad al destino maestro local, pasa un máximo de quince minutos hasta que la unidad aparece en la selección en el portal. Puede actualizar el servidor de configuración si la unidad no aparece pasados 15 minutos.
6. Instale las herramientas de VMware o de open-vm-tools en el servidor de destino maestro. Sin las herramientas, los almacenes de datos del host ESXi del destino maestro no se pueden detectar.
7. Establezca la opción disk.EnableUUID=true en los parámetros de configuración de la máquina virtual de destino maestro en VMware. Si la fila no existe, agréguela. Este parámetro es necesario a fin de proporcionar un UUID uniforme al VMDK, para que se monte correctamente.
8. Compruebe los requisitos de acceso de vCenter Server:
    - Si la máquina virtual que se quiere conmutar por recuperación está en un host ESXi administrado por VMware vCenter Server, el servidor de destino maestro necesita poder acceder al archivo de disco de máquina virtual (VMDK) de la máquina virtual local para escribir los datos replicados en los discos de la máquina virtual. Asegúrese de que el almacén de datos de la máquina virtual local está montado en el host del destino maestro con acceso de lectura y escritura.
    - Si la máquina virtual no está en un host ESXi administrado por VMware vCenter Server, Site Recovery creará una máquina virtual al realizar la reprotección. Esta máquina virtual se crea en el host ESXi en el que se crea a su vez la máquina virtual de servidor de destino maestro. Elija el host ESXi con cuidado para crear la máquina virtual en el host que quiera. El disco duro de la máquina virtual debe estar en un almacén de datos que sea accesible para el host en el que se esté ejecutando el servidor de destino maestro.
    - Otra opción (si es que existe la máquina virtual local para realizar la conmutación por recuperación) es eliminar esta máquina antes de realizar una conmutación por recuperación. En este caso, la conmutación por recuperación crea una máquina virtual en el mismo host que el host ESXi del destino maestro. Cuando se conmuta por recuperación a otra ubicación, los datos se recuperan en el mismo almacén de datos y el mismo host ESXi que los usados por el servidor de destino maestro local.
9. En cuanto a las máquinas físicas que conmutan por recuperación a máquinas virtuales de VMware, es necesario completar la detección del host en el que se ejecuta el servidor de destino maestro antes de poder volver a proteger la máquina.
10. Confirme que el host ESXi en el que se creó a máquina virtual de destino maestro tiene al menos un almacén de datos de sistema de archivos de máquina virtual (VMFS) asociado. Si no hay ningún almacén de datos VMFS asociado, la entrada de almacén de datos de la configuración de reprotección estará vacía y no podrá continuar.


## <a name="next-steps"></a>Pasos siguientes

[Reproteger](vmware-azure-reprotect.md) una máquina virtual.

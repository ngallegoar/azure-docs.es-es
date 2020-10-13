---
title: Preparación de la recuperación ante desastres de máquinas virtuales de Azure VMware Solution con Azure Site Recovery
description: Aprenda a preparar servidores de Azure VMware Solution para la recuperación ante desastres en Azure mediante el servicio Azure Site Recovery.
author: Harsha-CS
manager: rochakm
ms.service: site-recovery
ms.topic: tutorial
ms.date: 09/29/2020
ms.author: harshacs
ms.custom: MVC
ms.openlocfilehash: 9b04faf6797d04404dc0c5d617af2fd62a68c49a
ms.sourcegitcommit: 5abc3919a6b99547f8077ce86a168524b2aca350
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/07/2020
ms.locfileid: "91814308"
---
# <a name="prepare-azure-vmware-solution-servers-for-disaster-recovery-to-azure"></a>Preparación de servidores de Azure VMware Solution para la recuperación ante desastres en Azure

En este artículo se describe cómo preparar servidores de Azure VMware Solution para la recuperación ante desastres en Azure mediante el servicio [Azure Site Recovery](site-recovery-overview.md). 

Este es el segundo tutorial de una serie que muestra cómo configurar la recuperación ante desastres en Azure para máquinas virtuales de Azure VMware Solution. En el primer tutorial, se [configuraron los componentes de Azure](avs-tutorial-prepare-azure.md) necesarios para la recuperación ante desastres de Azure VMware Solution.


En este artículo aprenderá a:

> [!div class="checklist"]
> * Preparar una cuenta en el servidor vCenter o en el host ESXi de vSphere para automatizar la detección de VM.
> * Preparar una cuenta para la instalación automática de Mobility Service en máquinas virtuales de VMware.
> * Revisar los requisitos y el soporte técnico del servidor de VMware y de las máquinas virtuales.
> * Preparar la conexión con las máquinas virtuales de Azure después de la conmutación por error.

> [!NOTE]
> Los tutoriales muestran la ruta de implementación más sencilla para un escenario. Usan opciones predeterminadas siempre que es posible y no muestran todos los valores y las rutas de acceso posibles. Para obtener instrucciones detalladas, consulte el artículo de la sección de procedimientos de la tabla de contenido de Site Recovery.

## <a name="before-you-start"></a>Antes de comenzar

Asegúrese de que ha preparado Azure como se describe en el [primer tutorial de esta serie](avs-tutorial-prepare-azure.md).

## <a name="prepare-an-account-for-automatic-discovery"></a>Preparación de una cuenta de detección automática

Site Recovery necesita acceder a los servidores de Azure VMware Solution para:

- Detectar automáticamente las máquinas virtuales. Se requiere al menos una cuenta de solo lectura.
- Coordinar la replicación, la conmutación por error y la conmutación por recuperación. Necesita una cuenta que pueda ejecutar operaciones como la creación y la eliminación de discos, así como la activación de máquinas virtuales.

Cree la cuenta como se indica a continuación:

1. Para usar una cuenta dedicada, cree un rol en el nivel de vCenter. Asigne al rol un nombre como **Azure_Site_Recovery**.
2. Asigne al rol los permisos que se resumen en la tabla siguiente.
3. Cree un usuario en el servidor vCenter o el host de vSphere. Asigne el rol al usuario.

### <a name="vmware-account-permissions"></a>Permisos de cuenta de VMware

**Task** | **Rol o permisos** | **Detalles**
--- | --- | ---
**Detección de máquina virtual** | Al menos un usuario de solo lectura<br/><br/> Objeto de centro de datos  –> Propagar al objeto secundario, rol = solo lectura | El usuario se asigna en el nivel de centro de datos y tiene acceso a todos los objetos de este.<br/><br/> Para restringir el acceso, asigne el rol **Sin acceso** con **Propagate to child object** (Propagar a objeto secundario) a los objetos secundarios (hosts de vSphere, almacenes de datos, máquinas virtuales y redes).
**Replicación completa, conmutación por error, conmutación por recuperación** |  Cree un rol (Azure_Site_Recovery) con los permisos necesarios y, después, asígnelo a un grupo o usuario de VMware.<br/><br/> Objeto de centro de datos –> Propagar a objeto secundario, rol = Azure_Site_Recovery<br/><br/> Almacén de datos -> Asignar espacio, examinar almacén de datos, operaciones de archivo de bajo nivel, quitar archivo, actualizar archivos de máquina virtual<br/><br/> Red -> Asignación de red<br/><br/> Recursos-> Asignar máquina virtual al grupo de recursos, migrar apagado de máquina virtual, migrar máquinas virtuales encendidas<br/><br/> Tareas -> Crear tarea, actualizar tarea<br/><br/> Máquina virtual -> Configuración<br/><br/> Máquina virtual -> Interactuar -> Responder a pregunta, conexión de dispositivos, configurar soporte de CD, Configurar soporte de disquete, apagar, encender, instalación de herramientas de VMware<br/><br/> Máquina virtual -> Inventario -> Crear, registrar, anular registro<br/><br/> Máquina virtual -> Aprovisionamiento -> Permitir descarga de máquina virtual, permitir carga de archivos de máquina virtual<br/><br/> Máquina virtual -> Instantáneas -> Quitar instantáneas | El usuario se asigna en el nivel de centro de datos y tiene acceso a todos los objetos de este.<br/><br/> Para restringir el acceso, asigne el rol **Sin acceso** con **Propagate to child object** (Propagar a objeto secundario) a los objetos secundarios (hosts de vSphere, almacenes de datos, máquinas virtuales y redes).

## <a name="prepare-an-account-for-mobility-service-installation"></a>Preparación de una cuenta para la instalación de Mobility Service

Mobility Service debe estar instalado en las máquinas que desee replicar. Site Recovery puede realizar una instalación de inserción de este servicio cuando se habilita la replicación de una máquina, o bien puede instalarlo manualmente o mediante herramientas de instalación.

- En este tutorial, se va a instalar Mobility Service con la instalación de inserción.
- Para esta instalación de inserción, debe preparar una cuenta que Site Recovery pueda utilizar para acceder a la máquina virtual. Deberá especificar esta cuenta al configurar la recuperación ante desastres en la consola de Azure.

Prepare la cuenta como se indica a continuación:

Prepare un dominio o cuenta local con permisos para instalar en la máquina virtual.

- **Máquinas virtuales Windows**: para instalar en máquinas virtuales Windows, si no usa una cuenta de dominio, deshabilite el control Acceso de usuarios remotos en la máquina local. Para ello, en el Registro, en **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System**, agregue la entrada DWORD **LocalAccountTokenFilterPolicy** con un valor de 1.
- **Máquinas virtuales Linux**: para instalar en máquinas virtuales Linux, prepare una cuenta raíz en el servidor Linux de origen.


## <a name="check-vmware-requirements"></a>Comprobación de los requisitos de VMware

Asegúrese de que los servidores y las máquinas virtuales de VMware cumplen los requisitos.

1. Compruebe las [versiones de software](../azure-vmware/concepts-private-clouds-clusters.md#vmware-software-versions) de Azure VMware Solution.
2. Compruebe los [requisitos del servidor de VMware](vmware-physical-azure-support-matrix.md#on-premises-virtualization-servers).
3. Para máquinas virtuales Linux, [compruebe](vmware-physical-azure-support-matrix.md#linux-file-systemsguest-storage) los requisitos de almacenamiento y el sistema de archivos. 
4. Compruebe la compatibilidad de la [red](vmware-physical-azure-support-matrix.md#network) y del [almacenamiento](vmware-physical-azure-support-matrix.md#storage). 
5. Compruebe lo que es compatible para [redes Azure](vmware-physical-azure-support-matrix.md#azure-vm-network-after-failover), [almacenamiento](vmware-physical-azure-support-matrix.md#azure-storage), y [proceso](vmware-physical-azure-support-matrix.md#azure-compute), después de la conmutación por error.
6. Las máquinas virtuales Azure VMware Solution que replique en Azure tienen que cumplir los [requisitos de la máquina virtual de Azure](vmware-physical-azure-support-matrix.md#azure-vm-requirements).
7. En las máquinas virtuales Linux, el nombre de dispositivo o el nombre de punto de montaje debe ser único. Asegúrese de que los nombres de dos dispositivos o puntos de montaje sean distintos. Tenga en cuenta que el nombre no distingue mayúsculas de minúsculas. Por ejemplo, no puede asignar a dos dispositivos de la misma máquina virtual el nombre _device1_ y _Device1_.


## <a name="prepare-to-connect-to-azure-vms-after-failover"></a>Preparación para la conexión a las máquinas virtuales de Azure después de la conmutación por error

Después de la conmutación por error, es posible que quiera conectarse a las máquinas virtuales de Azure desde una red de Azure VMware Solution.

Para conectarse a máquinas virtuales Windows mediante RDP después de la conmutación por error, haga lo siguiente:

- **Acceso a Internet**. Antes de realizar una conmutación por error, habilite RDP en la máquina virtual de Azure VMware Solution. Asegúrese de que se hayan agregado las reglas de TCP y UDP para el perfil **Público**, y que RDP se permite en **Firewall de Windows** > **Aplicaciones permitidas** para todos los perfiles.
- **Acceso a VPN de sitio a sitio**:
    - Antes de realizar una conmutación por error, habilite RDP en la máquina virtual de Azure VMware Solution.
    - RDP debe permitirse en **Firewall de Windows** -> **Aplicaciones y características permitidas** para redes de **dominio y privadas**.
    - Compruebe que la directiva SAN del sistema operativo está establecida en **OnlineAll**. [Más información](https://support.microsoft.com/kb/3031135).
- No debe haber actualizaciones de Windows pendientes en la máquina virtual cuando se desencadene una conmutación por error. Si hay, no podrá iniciar sesión en la máquina virtual hasta que se complete la actualización.
- En la máquina virtual de Microsoft Azure después de la conmutación por error, compruebe los **Diagnósticos de arranque** para ver una captura de pantalla de la máquina virtual. Si no puede conectarse, compruebe que se está ejecutando la máquina virtual y revise estas [sugerencias de solución de problemas](https://social.technet.microsoft.com/wiki/contents/articles/31666.troubleshooting-remote-desktop-connection-after-failover-using-asr.aspx).

Para conectarse a máquinas virtuales Linux mediante SSH después de la conmutación por error, haga lo siguiente:

- Antes de la conmutación por error, compruebe en la máquina virtual de Azure VMware Solution que el servicio Secure Shell está configurado para iniciarse automáticamente cuando arranca el sistema.
- Compruebe que las reglas de firewall permiten una conexión SSH.
- En la máquina virtual de Azure después de la conmutación por error, permita las conexiones entrantes al puerto SSH para las reglas del grupo de seguridad de red de la máquina virtual conmutada por error y para la subred de Azure a la que esta se conecta.
- [Agregue una dirección IP pública](./site-recovery-monitor-and-troubleshoot.md) para la máquina virtual.
- Puede comprobar los **Diagnósticos de arranque** para ver una captura de pantalla de la máquina virtual.


## <a name="failback-requirements"></a>Requisitos de conmutación por recuperación
Si planea realizar la conmutación por recuperación a su nube de Azure VMware Solution, hay varios [requisitos previos para la conmutación por recuperación](avs-tutorial-reprotect.md#before-you-begin). Puede prepararlos ahora, pero no es necesario. Puede prepararlos después de realizar la conmutación por error a Azure.




## <a name="next-steps"></a>Pasos siguientes
> [!div class="nextstepaction"]
> [Configuración de la recuperación ante desastres](avs-tutorial-replication.md)
- Si va a replicar varias máquinas virtuales, [realice una planificación de la capacidad](site-recovery-deployment-planner.md).

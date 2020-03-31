---
title: Compatibilidad con la evaluación y migración de Hyper-V en Azure Migrate
description: Obtenga información sobre la compatibilidad con la evaluación y migración de Hyper-V en Azure Migrate.
ms.topic: conceptual
ms.date: 01/08/2020
ms.openlocfilehash: 1eab96df7ee58a8170f75b41c5a2a06f033ced19
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "79225424"
---
# <a name="support-matrix-for-hyper-v-migration"></a>Matriz de compatibilidad para la migración de Hyper-V

En este artículo se resumen los valores de compatibilidad y las limitaciones para migrar máquinas virtuales de Hyper-V con [Azure Migrate: Server Migration](migrate-services-overview.md#azure-migrate-server-migration-tool). Si quiere obtener información sobre cómo evaluar máquinas virtuales de Hyper-V a Azure, revise la [matriz de compatibilidad de migración](migrate-support-matrix-hyper-v.md).

## <a name="migration-limitations"></a>Limitaciones de la migración

Puede seleccionar hasta 10 máquinas virtuales a la vez para la replicación. Si quiere migrar más máquinas, replique en grupos de 10.


## <a name="hyper-v-hosts"></a>Hosts de Hyper-V

| **Soporte técnico**                | **Detalles**               
| :-------------------       | :------------------- |
| **Implementación**       | El host de Hyper-V puede ser independiente o implementarse en un clúster. <br/>Es necesario instalar el software de replicación de Azure Migrate (proveedor de replicación de Hyper-V) en los hosts de Hyper-V.|
| **Permisos**           | Necesita permisos de administrador en el host de Hyper-V. |
| **Sistema operativo host** | Windows Server 2019, Windows Server 2016 o Windows Server 2012 R2. |
| **URL de acceso** | El software del proveedor de replicación en los hosts de Hyper-V necesitará tener acceso a estas direcciones URL:<br/><br/> - login.microsoftonline.com: Control de acceso y administración de identidades mediante Active Directory.<br/><br/> - *.backup.windowsazure.com: Transferencia y coordinación de datos de replicación. Direcciones URL del servicio de migración.<br/><br/> - *.blob.core.windows.net: Cargar los datos en las cuentas de almacenamiento.<br/><br/> - dc.services.visualstudio.com: Cargue los registros de aplicaciones que se usan para la supervisión interna.<br/><br/> - time.windows.com: Verificación de la sincronización de la hora entre el sistema y la hora global.
| **Acceso a puertos** |  Conexiones salientes en el puerto HTTPS 443 para enviar datos de replicación de VM.

## <a name="hyper-v-vms"></a>Máquinas virtuales de Hyper-V

| **Soporte técnico**                  | **Detalles**               
| :----------------------------- | :------------------- |
| **Sistema operativo** | Azure admite todos los sistemas operativos [Windows](https://support.microsoft.com/help/2721672/microsoft-server-software-support-for-microsoft-azure-virtual-machines) y [Linux](https://docs.microsoft.com/azure/virtual-machines/linux/endorsed-distros). |
| **Cambios necesarios para Azure** | Es posible que algunas máquinas virtuales requieran cambios para poder ejecutarse en Azure. Debe hacer los ajustes manualmente antes de la migración. Los artículos pertinentes contienen instrucciones sobre cómo hacerlo. |
| **Arranque de Linux**                 | Si/boot está en una partición dedicada, debe residir en el disco del sistema operativo y no distribuirse en varios discos.<br/> Si/boot forma parte de la partición raíz (/), la partición "/" debe estar en el disco del sistema operativo y no abarcar otros discos. |
| **Arranque UEFI**                  | La máquina virtual migrada en Azure se convertirá automáticamente en una VM de arranque del BIOS. La máquina virtual debe estar ejecutando Windows Server 2012 o una versión posterior. El disco del sistema operativo debe tener un máximo de cinco particiones y el tamaño del disco del sistema operativo debe ser inferior a 300 GB.
  |
| **Tamaño del disco**                  | 2 TB para el disco del sistema operativo y 4 TB para los discos de datos.
| **Número de discos** | Un máximo de 16 discos por VM.
| **Discos/volúmenes cifrados**    | No se admiten para la migración. |
| **Discos RDM/de acceso directo**      | No se admiten para la migración. |
| **Disco compartido** | Las VM que usan discos compartidos no se admiten para la migración.
| **NFS**                        | Los volúmenes NFS montados como volúmenes en las máquinas virtuales no se replicarán. |
| **ISCSI**                      | Las VM con destinos iSCSI no se admiten para la migración.
| **Disco de destino**                | Puede migrar las VM de Azure con discos administrados únicamente. |
| **IPv6** | No compatible.
| **Formación de equipos NIC** | No compatible.
| **Azure Site Recovery** | No se puede replicar mediante la migración del servidor de Azure Migrate si la VM está habilitada para la replicación con Azure Site Recovery.
| **Puertos** | Conexiones salientes en el puerto HTTPS 443 para enviar datos de replicación de VM.

## <a name="azure-vm-requirements"></a>Requisitos de VM de Azure

Todas las máquinas virtuales locales que se replican en Azure deben cumplir los requisitos de máquina virtual de Azure que se resumen en esta tabla.

**Componente** | **Requisitos** | **Detalles**
--- | --- | ---
Tamaño del disco del sistema operativo | Hasta 2048 GB | Se produce un error en la comprobación si no es compatible.
Número de discos del sistema operativo | 1 | Se produce un error en la comprobación si no es compatible.
Número de discos de datos | 16 o menos. | Se produce un error en la comprobación si no es compatible.
Tamaño del disco de datos | Hasta 4095 GB | Se produce un error en la comprobación si no es compatible.
Adaptadores de red | Se admiten varios adaptadores. |
VHD compartido | No compatible. | Se produce un error en la comprobación si no es compatible.
Disco FC | No compatible. | Se produce un error en la comprobación si no es compatible.
BitLocker | No compatible. | Debe deshabilitar BitLocker antes de habilitar la replicación de una máquina.
Nombre de la máquina virtual | Entre 1 y 63 caracteres.<br/> Restringido a letras, números y guiones.<br/><br/> El nombre de la máquina debe empezar y terminar con una letra o un número. |  Actualice el valor de las propiedades de la máquina en Site Recovery.
Conexión después de la migración: Windows | Para conectarse a máquinas virtuales de Azure que se ejecutan en Windows después de la migración, siga estos pasos:<br/> -Antes de la migración, habilita RDP en la máquina virtual local. Asegúrese de que se hayan agregado las reglas de TCP y UDP para el perfil **Público**, y que RDP se permite en **Firewall de Windows** > **Aplicaciones permitidas** para todos los perfiles.<br/> Para el acceso a VPN de sitio a sitio, habilite RDP y permítalo en **Firewall de Windows** -> **Aplicaciones y características permitidas** para redes de **dominio y privadas**. Además, compruebe que la directiva SAN del sistema operativo está establecida en **OnlineAll**. [Más información](prepare-for-migration.md). |
Conexión después de la migración: Linux | Para conectarse a máquinas virtuales de Azure después de la migración mediante SSH, siga estos pasos:<br/> Antes de la migración, en la máquina local, compruebe que el servicio Secure Shell está establecido en Iniciar y que las reglas de firewall permiten una conexión SSH.<br/> Tras la conmutación por error, en la máquina virtual de Azure, permita conexiones entrantes al puerto SSH para las reglas del grupo de seguridad de red de la máquina virtual conmutada por error y para la subred de Azure a la que esta se conecta. Además, agregue una dirección IP pública para la máquina virtual. |  

## <a name="next-steps"></a>Pasos siguientes

[Máquinas virtuales de Hyper-V](tutorial-migrate-hyper-v.md) para la migración.

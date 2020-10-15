---
title: Solución de problemas de una máquina virtual defectuosa de Azure mediante la virtualización anidada en Azure | Microsoft Docs
description: Cómo solucionar problemas de una máquina virtual de Azure mediante la virtualización anidada en Azure
services: virtual-machines-windows
documentationcenter: ''
author: glimoli
manager: dcscontentpm
editor: ''
tags: azure-resource-manager
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.topic: article
ms.date: 11/19/2019
ms.author: genli
ms.openlocfilehash: e1acfc3216ccfaeac035f1ff31e82c7b67c17daf
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "76119625"
---
# <a name="troubleshoot-a-faulty-azure-vm-by-using-nested-virtualization-in-azure"></a>Solución de problemas de una máquina virtual defectuosa de Azure mediante la virtualización anidada en Azure

En este artículo se muestra cómo crear un entorno de virtualización anidada en Microsoft Azure con el fin de que pueda montar el disco de la máquina virtual defectuosa en el host de Hyper-V (máquina virtual de rescate) y solucionar los problemas que puedan surgir.

## <a name="prerequisites"></a>Prerrequisitos

Para montar la máquina virtual defectuosa, la máquina virtual de rescate debe usar el mismo tipo de cuenta de almacenamiento (Estándar o Premium) que la máquina virtual con defectuosa.

## <a name="step-1-create-a-rescue-vm-and-install-hyper-v-role"></a>Paso 1: Creación de una máquina virtual de rescate e instalación del rol de Hyper-V

1.  Cree una máquina virtual de rescate:

    -  Sistema operativo: Windows Server 2016 Datacenter

    -  Tamaño: Cualquier serie V3 con al menos dos núcleos que admitan la virtualización anidada. Para obtener más información, consulte la sección [Introducción a los nuevos tamaños Dv3 y Ev3](https://azure.microsoft.com/blog/introducing-the-new-dv3-and-ev3-vm-sizes/).

    -  La ubicación, la cuenta de almacenamiento y el grupo de recursos deben ser los mismos que los de la máquina virtual defectuosa.

    -  Seleccione el mismo tipo de almacenamiento que la máquina virtual defectuosa (Estándar o Premium).

2.  Una vez creada la máquina virtual de rescate, establezca una conexión de escritorio remoto con ella.

3.  En Administrador del servidor, seleccione **Administrar** > **Agregar roles y características**.

4.  En la sección **Tipo de instalación**, seleccione **Instalación basada en características o en roles**.

5.  En la sección **Seleccionar servidor de destino**, asegúrese de que la máquina virtual de rescate esté seleccionada.

6.  Seleccione **Rol de Hyper-V** > **Agregar características**.

7.  Seleccione **Siguiente** en la sección **Características**.

8.  Si hay un conmutador virtual disponible, selecciónelo. De lo contrario, seleccione **Siguiente**.

9.  En la sección **Migración**, seleccione **Siguiente**

10. En la sección **Almacenes predeterminados**, seleccione **Siguiente**.

11. Marque la casilla para reiniciar automáticamente el servidor si es necesario.

12. Seleccione **Instalar**.

13. Permita que el servidor instale el rol de Hyper-V. Esta operación tarda unos minutos y el servidor se reiniciará automáticamente.

## <a name="step-2-create-the-faulty-vm-on-the-rescue-vms-hyper-v-server"></a>Paso 2: Creación de la máquina virtual defectuosa en el servidor de Hyper-V de la máquina virtual de rescate

1.  [Cree un disco de instantánea](troubleshoot-recovery-disks-portal-windows.md#take-a-snapshot-of-the-os-disk) para el disco del sistema operativo de la VM que tiene el problema y, a continuación, conecte el disco de instantánea a la VM de rescate.

2.  Escritorio remoto a la VM de rescate.

3.  Abra Administración de discos (diskmgmt.msc). Asegúrese de que el disco de la máquina virtual defectuosa esté establecido en **Sin conexión**.

4.  Abra el administrador de Hyper-V: En **Administrador del servidor**, seleccione el **rol Hyper-V**. Haga clic con el botón derecho en el servidor y, a continuación, seleccione **Administrador de Hyper-V**.

5.  En Administrador de Hyper-V, haga clic con el botón derecho en la máquina virtual de rescate y seleccione **Nuevo** > **Máquina virtual** > **Siguiente**.

6.  Escriba un nombre para la máquina virtual y, a continuación, seleccione **Siguiente**.

7.  Seleccione **Generación 1**.

8.  Establezca la memoria de inicio en 1024 MB o más.

9. Si procede, seleccione el conmutador de red de Hyper-V que se ha creado. De lo contrario, vaya a la página siguiente.

10. Seleccione **Conectar un disco duro virtual más adelante**.

    ![Imagen de la opción Conectar un disco duro virtual más adelante](media/troubleshoot-vm-by-use-nested-virtualization/attach-disk-later.png)

11. Seleccione **Finalizar** cuando se haya creado la máquina virtual.

12. Haga clic con el botón derecho en la máquina virtual que ha creado y, a continuación, seleccione **Configuración**.

13. Seleccione **Controlador IDE 0** y **Unidad de disco duro** y, a continuación, haga clic en **Agregar**.

    ![Imagen sobre cómo agregar una nueva unidad de disco duro](media/troubleshoot-vm-by-use-nested-virtualization/create-new-drive.png)    

14. En **Disco duro físico**, seleccione el disco de la máquina virtual defectuosa que ha conectado a la máquina virtual de Azure. Si no ve ningún disco en la lista, compruebe si el disco se ha establecido como sin conexión mediante Administración de discos.

    ![Imagen sobre al montaje del disco](media/troubleshoot-vm-by-use-nested-virtualization/mount-disk.png)  


15. Seleccione **Aplicar** y luego **Aceptar**.

16. Haga doble clic en la máquina virtual y, a continuación, iníciela.

17. Ahora puede trabajar con la máquina virtual como máquina virtual local. Puede seguir los pasos para solucionar problemas que sean necesarios.

## <a name="step-3-replace-the-os-disk-used-by-the-faulty-vm"></a>Paso 3: Reemplazo del disco del sistema operativo que usa la máquina virtual defectuosa

1.  Cuando la máquina virtual vuelva a estar en línea, apáguela en el administrador de Hyper-V.

2.  [Desmonte y desconecte el disco del sistema operativo reparado](troubleshoot-recovery-disks-portal-windows.md#unmount-and-detach-original-virtual-hard-disk
).
3.  [Reemplace el disco del sistema operativo que usa la VM con el disco del SO reparado](troubleshoot-recovery-disks-portal-windows.md#swap-the-os-disk-for-the-vm
).

## <a name="next-steps"></a>Pasos siguientes

Si tiene problemas para conectarse a la máquina virtual, consulte [Solución de problemas de conexiones RDP a una máquina virtual de Azure](troubleshoot-rdp-connection.md). Para problemas con el acceso a aplicaciones que se ejecutan en su máquina virtual, consulte [Solucionar problemas de conectividad de aplicaciones en una máquina virtual Windows](troubleshoot-app-connection.md).

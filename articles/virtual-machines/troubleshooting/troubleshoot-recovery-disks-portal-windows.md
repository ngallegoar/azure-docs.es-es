---
title: Solución de problemas de una VM de Windows en Azure Portal | Microsoft Docs
description: Aprenda a solucionar problemas de la máquina virtual Windows en Azure mediante la conexión del disco del sistema operativo a una VM de recuperación a través de Azure Portal.
services: virtual-machines-windows
documentationCenter: ''
author: genlin
manager: dcscontentpm
editor: ''
ms.service: virtual-machines-windows
ms.topic: troubleshooting
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 08/19/2018
ms.author: genli
ms.openlocfilehash: 04adf3903cd925f4507e94347251c762a576ff93
ms.sourcegitcommit: c2dd51aeaec24cd18f2e4e77d268de5bcc89e4a7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/18/2020
ms.locfileid: "94735235"
---
# <a name="troubleshoot-a-windows-vm-by-attaching-the-os-disk-to-a-recovery-vm-through-the-azure-portal"></a>Solución de problemas de una VM Windows mediante la conexión del disco del sistema operativo a una VM de recuperación a través de Azure Portal
Si la máquina virtual (VM) Windows de Azure se encuentra un error de disco o de startup, deberá realizar los pasos necesarios para solucionar problemas en el propio disco duro virtual (VHD). Un ejemplo habitual es una actualización de aplicación que devuelve un error y que impide que la VM pueda iniciarse correctamente. En este artículo se detalla cómo utilizar Azure Portal para conectar el disco duro virtual a otra VM Windows para solucionar cualquier error y, posteriormente, volver a crear la VM original. 

## <a name="recovery-process-overview"></a>Introducción al proceso de recuperación
El proceso de solución de problemas es el siguiente:

1. Detenga la máquina virtual afectada.
1. Cree una instantánea del disco del sistema operativo de la máquina virtual.
1. Cree un disco duro virtual a partir de la instantánea.
1. Conecte y monte el disco duro virtual en otra máquina virtual Windows con el fin de solucionar problemas.
1. Conéctese a la máquina virtual de solución de problemas. Edite los archivos o ejecute cualquier herramienta necesaria para solucionar los problemas del disco duro virtual original.
1. Desmonte y desconecte el disco duro virtual de la máquina virtual de solución de problemas.
1. Intercambie el disco del sistema operativo de la máquina virtual.

> [!NOTE]
> Este artículo no se aplica a las VM que tienen discos no administrados.

## <a name="take-a-snapshot-of-the-os-disk"></a>Realización de la instantánea del disco del SO
Una instantánea es una copia completa de solo lectura de un disco duro virtual. Se recomienda apagar limpiamente la máquina virtual antes de realizar una instantánea para así limpiar cualquier proceso que esté en curso. Para realizar una instantánea de un disco del sistema operativo, siga estos pasos:

1. Vaya a [Azure Portal](https://portal.azure.com). Seleccione **Máquinas virtuales** en la barra lateral y, a continuación, la VM que tenga el problema.
1. En el panel izquierdo, seleccione **Discos** y luego el nombre del disco del sistema operativo.
    ![Captura de pantalla que muestra el nombre del disco del sistema operativo en la configuración de disco.](./media/troubleshoot-recovery-disks-portal-windows/select-osdisk.png)
1. En la página **Información general** del disco del sistema operativo, seleccione **Crear instantánea**.
1. Cree una instantánea en la misma ubicación que el disco del sistema operativo.

## <a name="create-a-disk-from-the-snapshot"></a>Creación de un disco a partir de la instantánea
Para crear un disco a partir de la instantánea, siga estos pasos:

1. Seleccione **Cloud Shell** en Azure Portal.

    ![Captura de pantalla que muestra el botón para abrir Azure Cloud Shell.](./media/troubleshoot-recovery-disks-portal-windows/cloud-shell.png)
1. Ejecute los siguientes comandos de PowerShell para crear un disco administrado a partir de la instantánea. Reemplace los nombres de ejemplo con los nombres adecuados.

    ```powershell
    #Provide the name of your resource group.
    $resourceGroupName ='myResourceGroup'
    
    #Provide the name of the snapshot that will be used to create managed disks.
    $snapshotName = 'mySnapshot' 
    
    #Provide the name of the managed disk.
    $diskName = 'newOSDisk'
    
    #Provide the size of the disks in gigabytes. It should be greater than the VHD file size. In this example, the size of the snapshot is 127 GB. So we set the disk size to 128 GB.
    $diskSize = '128'
    
    #Provide the storage type for the managed disk: Premium_LRS or Standard_LRS.
    $storageType = 'Standard_LRS'
    
    #Provide the Azure region (for example, westus) where the managed disks will be located.
    #This location should be the same as the snapshot location.
    #Get all the Azure locations by using this command:
    #Get-AzLocation
    $location = 'westus'
    
    $snapshot = Get-AzSnapshot -ResourceGroupName $resourceGroupName -SnapshotName $snapshotName 
     
    $diskConfig = New-AzDiskConfig -AccountType $storageType -Location $location -CreateOption Copy -SourceResourceId $snapshot.Id
     
    New-AzDisk -Disk $diskConfig -ResourceGroupName $resourceGroupName -DiskName $diskName
    ```
3. Si los comandos se ejecutan correctamente, verá el nuevo disco en el grupo de recursos proporcionado.

## <a name="attach-the-disk-to-another-vm"></a>Conexión del disco a otra máquina virtual
Para los pasos siguientes, se usa otra máquina virtual con el fin de solucionar problemas. Después de conectar el disco a la VM de solución de problemas, puede examinar y modificar el contenido de ese disco. Este proceso permite corregir los errores de configuración o revisar otros archivos de registro de la aplicación o el sistema. Para conectar el disco a otra máquina virtual, siga estos pasos:

1. Seleccione el grupo de recursos en el portal y, a continuación, seleccione la VM de solución de problemas. Seleccione **Discos** > **Editar** > **Agregar disco de datos**.

    ![Captura de pantalla que muestra las selecciones para adjuntar un disco existente en el portal.](./media/troubleshoot-recovery-disks-portal-windows/attach-existing-disk.png)

2. En la lista **Discos de datos**, seleccione el disco del sistema operativo de la máquina virtual que identificó. Si no ve el disco del sistema operativo, asegúrese de que la VM de solución de problemas y el disco del sistema operativo se encuentran en la misma región (ubicación). 
3. Seleccione **Guardar** para aplicar los cambios.

## <a name="mount-the-attached-data-disk-to-the-vm"></a>Montaje del disco de datos conectado en la máquina virtual

1. Abra una conexión de Escritorio remoto a la máquina virtual de solución de problemas. 
2. En la VM de solución de problemas, abra **Administrador del servidor** y, a continuación, seleccione **Servicios de archivos y almacenamiento**. 

    ![Captura de pantalla que muestra la selección de los Servicios de archivos y almacenamiento en el Administrador del servidor.](./media/troubleshoot-recovery-disks-portal-windows/server-manager-select-storage.png)

3. El disco de datos se detecta y conecta automáticamente. Para ver una lista de los discos conectados, seleccione **Discos**. Puede seleccionar el disco de datos para ver información de volumen, incluida la letra de unidad. En el ejemplo siguiente se muestra el disco de datos adjunto y que usa la unidad **F**.

    ![Captura de pantalla que muestra la información del disco y el volumen adjuntos en el Administrador del servidor.](./media/troubleshoot-recovery-disks-portal-windows/server-manager-disk-attached.png)

## <a name="fix-problems-on-the-original-virtual-hard-disk"></a>Solución de problemas en el disco duro virtual original
Con el disco duro virtual existente montado, ahora puede realizar todos los pasos de mantenimiento y solución de problemas según sea necesario. Después de resolver todos los errores, realice los pasos siguientes.

## <a name="unmount-and-detach-the-original-virtual-hard-disk"></a>Desmontaje y desasociación del disco duro virtual original
Desconecte el disco duro virtual existente de la VM de solución de problemas. Tenga en cuenta que no podrá usar el disco duro virtual con ninguna otra VM hasta que se libere la concesión que asocia el disco duro virtual a la VM de solución de problemas.

1. En la sesión del Escritorio remoto de la VM, abra **Administrador del servidor** y, a continuación, seleccione **Servicios de archivos y almacenamiento**.

    ![Captura de pantalla que muestra la selección de los Servicios de archivos y almacenamiento en el Administrador del servidor.](./media/troubleshoot-recovery-disks-portal-windows/server-manager-select-storage.png)

2. Seleccione **Discos**, haga clic con el botón derecho en el disco de datos y, a continuación, seleccione **Desconectar**.

    ![Captura de pantalla que muestra cómo configurar el disco de datos sin conexión en el Administrador del servidor.](./media/troubleshoot-recovery-disks-portal-windows/server-manager-set-disk-offline.png)

3. Desconecte el disco duro virtual de la VM. Seleccione la VM en Azure Portal y haga clic en **Discos**. 
4. Seleccione **Editar**, elija el disco del sistema operativo que haya conectado y haga clic en **Eliminar**.

    ![Captura de pantalla que muestra las selecciones para desconectar un disco duro virtual existente.](./media/troubleshoot-recovery-disks-portal-windows/detach-disk.png)

    Antes de continuar, espere a que el disco de datos se elimine correctamente de la VM.

## <a name="swap-the-os-disk-for-the-vm"></a>Intercambio del disco del sistema operativo de la máquina virtual

Azure Portal ahora permite cambiar el disco del sistema operativo de la máquina virtual. Para ello, siga estos pasos.

1. Vaya a [Azure Portal](https://portal.azure.com). Seleccione **Máquinas virtuales** en la barra lateral y, a continuación, la VM que tenga el problema.
1. En el panel izquierdo, seleccione **Discos** y, a continuación, **Intercambiar disco del sistema operativo**.
   
    ![Captura de pantalla que muestra el botón para intercambiar el disco del sistema operativo en Azure Portal.](./media/troubleshoot-recovery-disks-portal-windows/swap-os-ui.png)

1. Seleccione el nuevo disco reparado y escriba el nombre de la VM para confirmar el cambio. Si no ve el disco en la lista, espere entre 10 y 15 minutos después de desconectarlo de la VM de solución de problemas. Asegúrese también de que el disco está en la misma ubicación que la máquina virtual.
1. Seleccione **Aceptar**.

## <a name="next-steps"></a>Pasos siguientes
Si tiene problemas para conectarse a la VM, consulte [Solución de problemas de conexiones del Escritorio remoto a una VM de Azure](troubleshoot-rdp-connection.md). Si tiene problemas con el acceso a aplicaciones que se ejecutan en su VM, consulte [Solución de problemas de conectividad de aplicaciones en una VM Windows](troubleshoot-app-connection.md).

Para obtener más información sobre el uso de Azure Resource Manager, consulte [Información general de Azure Resource Manager](../../azure-resource-manager/management/overview.md).



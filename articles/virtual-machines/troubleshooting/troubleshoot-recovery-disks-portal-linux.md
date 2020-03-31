---
title: Uso de una máquina virtual Linux de solución de problemas en Azure Portal | Microsoft Docs
description: Aprenda a solucionar problemas de la máquina virtual Linux mediante la conexión del disco del sistema operativo a una máquina virtual de recuperación mediante Azure Portal.
services: virtual-machines-linux
documentationCenter: ''
author: genlin
manager: dcscontentpm
editor: ''
ms.service: virtual-machines-linux
ms.topic: troubleshooting
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 08/19/2019
ms.author: genli
ms.openlocfilehash: e45de5c12f0d93645a0b1253acf8300527cafdbc
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "75374648"
---
# <a name="troubleshoot-a-linux-vm-by-attaching-the-os-disk-to-a-recovery-vm-using-the-azure-portal"></a>Solución de problemas de una máquina virtual Linux mediante la conexión del disco del sistema operativo a una máquina virtual de recuperación mediante Azure Portal
Si la máquina virtual Linux se encuentra un error de disco o de arranque, deberá realizar los pasos para solucionar problemas en el propio disco duro virtual. Un ejemplo habitual sería una entrada no válida en `/etc/fstab` que impide que la máquina virtual se pueda arrancar correctamente. En este artículo se detalla cómo utilizar Azure Portal para conectar el disco duro virtual a otra máquina virtual Linux para solucionar los errores y, posteriormente, volver a crear la máquina virtual original.

## <a name="recovery-process-overview"></a>Introducción al proceso de recuperación
El proceso de solución de problemas es el siguiente:

1. Detenga la máquina virtual afectada.
1. Realice una instantánea del disco del sistema operativo de la máquina virtual.
1. Cree un disco duro virtual a partir de la instantánea.
1. Conecte y monte el disco duro virtual en otra máquina virtual Windows con el fin de solucionar problemas.
1. Conéctese a la máquina virtual de solución de problemas. Edite los archivos o ejecute cualquier herramienta necesaria para solucionar los problemas del disco duro virtual original.
1. Desmonte y desconecte el disco duro virtual de la máquina virtual de solución de problemas.
1. Intercambie el disco del sistema operativo de la máquina virtual.

> [!NOTE]
> Este artículo no se aplica a la máquina virtual con el disco no administrado.

## <a name="determine-boot-issues"></a>Determinación de los problemas de arranque
Examine el diagnóstico de arranque y la captura de pantalla de la máquina virtual para determinar por qué la máquina virtual no es capaz de arrancar correctamente. Un ejemplo habitual sería una entrada no válida en `/etc/fstab` o un disco duro virtual subyacente que se va a eliminar o mover.

Seleccione la máquina virtual en el portal y, a continuación, desplácese hacia abajo hasta la sección **Soporte técnico y solución de problemas**. Haga clic en **Diagnósticos de arranque** para ver los mensajes de consola transmitidos desde la máquina virtual. Revise los registros de consola para ver si puede determinar por qué la máquina virtual tiene un problema. En el ejemplo siguiente se muestra una máquina virtual atascada en el modo de mantenimiento que requiere intervención manual:

![Visualización de los registros de consola de diagnóstico de arranque](./media/troubleshoot-recovery-disks-portal-linux/boot-diagnostics-error.png)

También puede hacer clic en **Captura de pantalla** en la parte superior del registro de diagnóstico de arranque para descargar una captura de la captura de pantalla de la máquina virtual.

## <a name="take-a-snapshot-of-the-os-disk"></a>Realización de la instantánea del disco del sistema operativo
Una instantánea es una copia completa de solo lectura de un disco duro virtual (VHD). Se recomienda apagar limpiamente la máquina virtual antes de realizar una instantánea para así limpiar cualquier proceso que esté en curso. Para realizar una instantánea de un disco del sistema operativo, siga estos pasos:

1. Vaya al [Portal de Azure](https://portal.azure.com). Seleccione **Máquinas virtuales** en la barra lateral y luego la máquina virtual que tiene el problema.
1. En el panel izquierdo, seleccione **Discos** y luego el nombre del disco del sistema operativo.
    ![Imagen sobre el nombre del disco del sistema operativo](./media/troubleshoot-recovery-disks-portal-windows/select-osdisk.png)
1. En la página **Información general** del disco del sistema operativo, seleccione **Crear instantánea**.
1. Cree una instantánea en la misma ubicación que el disco del sistema operativo.

## <a name="create-a-disk-from-the-snapshot"></a>Creación de un disco a partir de la instantánea
Para crear un disco a partir de la instantánea, siga estos pasos:

1. Seleccione **Cloud Shell** en Azure Portal.

    ![Imagen sobre Cloud Shell abierto](./media/troubleshoot-recovery-disks-portal-windows/cloud-shell.png)
1. Ejecute los siguientes comandos de PowerShell para crear un disco administrado a partir de la instantánea. Debe reemplazar estos nombres de ejemplo por los nombres adecuados.

    ```powershell
    #Provide the name of your resource group
    $resourceGroupName ='myResourceGroup'
    
    #Provide the name of the snapshot that will be used to create Managed Disks
    $snapshotName = 'mySnapshot' 
    
    #Provide the name of theManaged Disk
    $diskName = 'newOSDisk'
    
    #Provide the size of the disks in GB. It should be greater than the VHD file size. In this sample, the size of the snapshot is 127 GB. So we set the disk size to 128 GB.
    $diskSize = '128'
    
    #Provide the storage type for Managed Disk. Premium_LRS or Standard_LRS.
    $storageType = 'Standard_LRS'
    
    #Provide the Azure region (e.g. westus) where Managed Disks will be located.
    #This location should be same as the snapshot location
    #Get all the Azure location using command below:
    #Get-AzLocation
    $location = 'westus'
    
    $snapshot = Get-AzSnapshot -ResourceGroupName $resourceGroupName -SnapshotName $snapshotName 
     
    $diskConfig = New-AzDiskConfig -AccountType $storageType -Location $location -CreateOption Copy -SourceResourceId $snapshot.Id
     
    New-AzDisk -Disk $diskConfig -ResourceGroupName $resourceGroupName -DiskName $diskName
    ```
3. Si los comandos se ejecutan correctamente, se verá el nuevo disco en el grupo de recursos proporcionado.

## <a name="attach-disk-to-another-vm"></a>Conexión de un disco a otra máquina virtual
Para los pasos siguientes, se usa otra máquina virtual con el fin de solucionar problemas. Después de conectar el disco a la máquina virtual de solución de problemas, puede examinar y modificar el contenido del disco. Este proceso permite corregir los errores de configuración o revisar otros archivos de registro de la aplicación o el sistema. Para conectar el disco a otra máquina virtual, siga estos pasos:

1. Seleccione el grupo de recursos desde el portal y, a continuación, seleccione la máquina virtual de solución de problemas. Seleccione **Discos**, elija **Editar** y haga clic en **Agregar disco de datos**:

    ![Conexión del disco existente en el portal](./media/troubleshoot-recovery-disks-portal-windows/attach-existing-disk.png)

2. En la lista **Discos de datos**, seleccione el disco del sistema operativo de la máquina virtual que identificó. Si no ve el disco del sistema operativo, asegúrese de que la máquina virtual de solución de problemas y el disco del sistema operativo se encuentran en la misma región (ubicación). 
3. Seleccione **Guardar** para aplicar los cambios.

## <a name="mount-the-attached-data-disk"></a>Montaje del disco de datos conectado

> [!NOTE]
> Los siguientes ejemplos detallan los pasos necesarios en una máquina virtual Ubuntu. Si usas una distribución de Linux diferente como Red Hat Enterprise Linux o SUSE, el registro de ubicaciones del archivo de registro y los comandos `mount` pueden ser ligeramente diferentes. Consulte la documentación para su distribución específica para los cambios apropiados en los comandos.

1. SSH en la máquina virtual de solución de problemas con las credenciales apropiadas. Si este disco es el primer disco de datos conectado a la máquina virtual de solución de problemas, es probable que se conecte a `/dev/sdc`. Use `dmseg` para enumerar los discos conectados:

    ```bash
    dmesg | grep SCSI
    ```
    La salida es similar a la del ejemplo siguiente:

    ```bash
    [    0.294784] SCSI subsystem initialized
    [    0.573458] Block layer SCSI generic (bsg) driver version 0.4 loaded (major 252)
    [    7.110271] sd 2:0:0:0: [sda] Attached SCSI disk
    [    8.079653] sd 3:0:1:0: [sdb] Attached SCSI disk
    [ 1828.162306] sd 5:0:0:0: [sdc] Attached SCSI disk
    ```

    En el ejemplo anterior, el disco del sistema operativo está en `/dev/sda` y el disco temporal que se proporciona para cada máquina virtual está en `/dev/sdb`. Si hubiera varios discos de datos, deben estar en `/dev/sdd`, `/dev/sde`, y así sucesivamente.

2. Cree un directorio para montar el disco duro virtual existente. El ejemplo siguiente permite crear un directorio llamado `troubleshootingdisk`:

    ```bash
    sudo mkdir /mnt/troubleshootingdisk
    ```

3. Si tiene varias particiones en el disco duro virtual existente, monte la partición requerida. En el ejemplo siguiente, se monta la primera partición principal en `/dev/sdc1`:

    ```bash
    sudo mount /dev/sdc1 /mnt/troubleshootingdisk
    ```

    > [!NOTE]
    > El procedimiento recomendado consiste en montar los discos de datos en máquinas virtuales en Azure con el identificador único universal (UUID) del disco duro virtual. En este breve escenario de solución de problemas, no es necesario montar el disco duro virtual con el UUID. Sin embargo, en circunstancias normales, editar `/etc/fstab` para montar los discos duros virtuales mediante el nombre de dispositivo en lugar del UUID puede provocar que la máquina virtual no se pueda arrancar.


## <a name="fix-issues-on-original-virtual-hard-disk"></a>Solución de problemas en el disco duro virtual original
Con el disco duro virtual existente montado, ahora puede realizar todos los pasos de mantenimiento y solución de problemas según sea necesario. Una vez que se han resuelto los problemas, continúe con los pasos siguientes.

## <a name="unmount-and-detach-original-virtual-hard-disk"></a>Desmontaje y desconexión del disco duro virtual original
Una vez resueltos los errores, desconecte el disco duro virtual existente de la máquina virtual de solución de problemas. No se podrá usar el disco duro virtual en ninguna otra máquina virtual hasta que se libere la concesión que conecta el disco duro virtual a la máquina virtual de solución de problemas.

1. Desde la sesión SSH a la máquina virtual de solución de problemas, desmonte el disco duro virtual existente. Cambie primero el directorio primario del punto de montaje:

    ```bash
    cd /
    ```

    Ahora, desmonte el disco duro virtual existente. El siguiente ejemplo desmonta el dispositivo en `/dev/sdc1`:

    ```bash
    sudo umount /dev/sdc1
    ```

2. Ahora, desconecte el disco duro virtual de la máquina virtual. Seleccione la máquina virtual en el portal y haga clic en **Discos**. Seleccione el disco duro virtual existente y, a continuación, haga clic en **Desasociar**:

    ![Desconecte el disco duro virtual existente](./media/troubleshoot-recovery-disks-portal-windows/detach-disk.png)

    Espere hasta que la máquina virtual haya desconectado correctamente el disco de datos antes de continuar.

## <a name="swap-the-os-disk-for-the-vm"></a>Intercambio del disco del sistema operativo de la máquina virtual

Azure Portal ahora permite cambiar el disco del sistema operativo de la máquina virtual. Para ello, siga estos pasos.

1. Vaya al [Portal de Azure](https://portal.azure.com). Seleccione **Máquinas virtuales** en la barra lateral y luego la máquina virtual que tiene el problema.
1. En el panel izquierdo, seleccione **Discos** y luego **Intercambiar disco del sistema operativo**.
        ![Imagen sobre el intercambio del disco del sistema operativo en Azure Portal](./media/troubleshoot-recovery-disks-portal-windows/swap-os-ui.png)

1. Seleccione el nuevo disco que ha reparado y escriba el nombre de la máquina virtual para confirmar el cambio. Si no ve el disco en la lista, espere entre 10 y 15 minutos después de desconectar el disco de la máquina virtual de solución de problemas. Asegúrese también de que el disco está en la misma ubicación que la máquina virtual.
1. Seleccione Aceptar.

## <a name="next-steps"></a>Pasos siguientes
Si tiene problemas para conectarse a la máquina virtual, consulte [Solución de problemas de conexiones SSH a una máquina virtual Linux de Azure](troubleshoot-ssh-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Para problemas con el acceso a aplicaciones que se ejecutan en su máquina virtual, consulte [Solucionar problemas de conectividad de aplicaciones en una máquina virtual de Linux en Azure](../windows/troubleshoot-app-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Para más información sobre el uso de Resource Manager, consulte [Información general de Azure Resource Manager](../../azure-resource-manager/management/overview.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

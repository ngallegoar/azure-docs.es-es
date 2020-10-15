---
title: Entorno de chroot en una VM de recuperación de Linux.
description: En este artículo se describe cómo solucionar problemas del entorno de chroot en la máquina virtual (VM) de rescate en Linux.
mservices: virtual-machines-linux
documentationcenter: ''
author: kailashmsft
manager: dcscontentpm
editor: ''
tags: ''
ms.service: virtual-machines-linux
ms.topic: troubleshooting
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.date: 05/05/2020
ms.author: kaib
ms.openlocfilehash: d6605aa77cfa746f60fc3b23e120267a2614f7b1
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "82868273"
---
# <a name="chroot-environment-in-a-linux-rescue-vm"></a>Entorno de chroot en una VM de recuperación de Linux

En este artículo se describe cómo solucionar problemas del entorno de chroot en la máquina virtual (VM) de rescate en Linux.

## <a name="ubuntu-16x--ubuntu-18x"></a>Ubuntu 16.x y Ubuntu 18.x

1. Detenga o anule la asignación de la VM afectada.
1. Cree una imagen de VM de rescate de la misma versión del SO, en el mismo grupo de recursos (RSG) y ubicación con el disco administrado.
1. Use Azure Portal para tomar una instantánea del disco del SO de la máquina virtual afectada.
1. Cree un disco a partir de la instantánea del disco del SO y asócielo a la VM de rescate.
1. Una vez creado el disco, solucione los problemas del entorno de chroot en la VM de rescate.

   1. Acceda a la VM como usuario raíz con el comando siguiente:

      `#sudo su -`

   1. Para buscar el disco, use `dmesg` (el método que use para detectar el nuevo disco puede variar). En el ejemplo siguiente, se usa **dmesg** para filtrar los discos **SCSI**:

      `dmesg | grep SCSI`

      El resultado será similar al del ejemplo siguiente. En este ejemplo, queremos el disco **SDC**:

      ```
      [    0.294784] SCSI subsystem initialized
      [    0.573458] Block layer SCSI generic (bsg) driver version 0.4 loaded (major 252)
      [    7.110271] sd 2:0:0:0: [sda] Attached SCSI disk
      [    8.079653] sd 3:0:1:0: [sdb] Attached SCSI disk
      [ 1828.162306] sd 5:0:0:0: [sdc] Attached SCSI disk
      ```

   1. Use los comandos siguientes para acceder al entorno de chroot:

      ```
      #mkdir /rescue
      #mount /dev/sdc1 /rescue
      #mount /dev/sdc15 /rescue/boot/efi
      #cd /rescue

      #mount -t proc proc proc
      #mount -t sysfs sys sys/
      #mount -o bind /dev dev/
      #mount -o bind /dev/pts dev/pts/
      #mount -o bind /run run/
      #chroot /rescue
      ```

   1. Solucione los problemas del entorno de chroot.

   1. Use los comandos siguientes para salir del entorno de chroot:

      ```
      #exit

      #umount /rescue/proc/
      #umount /rescue/sys/
      #umount /rescue/dev/pts
      #umount /rescue/dev/
      #umount /rescue/run
      #cd /
      #umount /rescue/boot/efi
      #umount /rescue
      ```

      > [!NOTE]
      > Si recibe el error `unable to unmount /rescue`, agregue la opción -l al comando umount.
      >
      > Ejemplo: `umount -l /rescue`

1. Desasocie el disco de la VM de rescate y realice un intercambio de disco con la VM original.
1. Inicie la VM original y compruebe su conectividad.

## <a name="rhelcentosoracle-6x--oracle-8x--rhelcentos-7x-with-raw-partitions"></a>RHEL/Centos/Oracle 6.x y Oracle 8.x y RHEL/Centos 7.x con particiones RAW

1. Detenga o anule la asignación de la VM afectada.
1. Cree una imagen de VM de rescate de la misma versión del SO, en el mismo grupo de recursos (RSG) y ubicación con el disco administrado.
1. Use Azure Portal para tomar una instantánea del disco del SO de la máquina virtual afectada.
1. Cree un disco a partir de la instantánea del disco del SO y asócielo a la VM de rescate.
1. Una vez creado el disco, solucione los problemas del entorno de chroot en la VM de rescate.

   1. Acceda a la VM como usuario raíz con el comando siguiente:

      `#sudo su -`

   1. Para buscar el disco, use `dmesg` (el método que use para detectar el nuevo disco puede variar). En el ejemplo siguiente, se usa **dmesg** para filtrar los discos **SCSI**:

      `dmesg | grep SCSI`

      El resultado será similar al del ejemplo siguiente. En este ejemplo, queremos el disco **SDC**:

      ```
      [    0.294784] SCSI subsystem initialized
      [    0.573458] Block layer SCSI generic (bsg) driver version 0.4 loaded (major 252)
      [    7.110271] sd 2:0:0:0: [sda] Attached SCSI disk
      [    8.079653] sd 3:0:1:0: [sdb] Attached SCSI disk
      [ 1828.162306] sd 5:0:0:0: [sdc] Attached SCSI disk
      ```

   1. Use los comandos siguientes para acceder al entorno de chroot:

      ```
      #mkdir /rescue
      #mount -o nouuid /dev/sdc2 /rescue
      #mount -o nouuid /dev/sdc1 /rescue/boot/
      #cd /rescue

      #mount -t proc proc proc
      #mount -t sysfs sys sys/
      #mount -o bind /dev dev/
      #mount -o bind /dev/pts dev/pts/
      #mount -o bind /run run/
      #chroot /rescue
      ```

   1. Solucione los problemas del entorno de chroot.

   1. Use los comandos siguientes para salir del entorno de chroot:

      ```
      #exit

      #umount /rescue/proc/
      #umount /rescue/sys/
      #umount /rescue/dev/pts
      #umount /rescue/dev/
      #umount /rescue/run
      #cd /
      #umount /rescue/boot/efi
      #umount /rescue
      ```

      > [!NOTE]
      > Si recibe el error `unable to unmount /rescue`, agregue la opción -l al comando umount.
      >
      > Ejemplo: `umount -l /rescue`

1. Desasocie el disco de la VM de rescate y realice un intercambio de disco con la VM original.
1. Inicie la VM original y compruebe su conectividad.

## <a name="rhelcentos-7x-with-lvm"></a>RHEL/Centos 7.x con LVM

   > [!NOTE]
   > Si la VM original incluye el Administrador de volúmenes lógicos (LVM) en el disco del SO, cree la VM de rescate mediante la imagen con particiones RAW en el disco del SO.

1. Detenga o anule la asignación de la VM afectada.
1. Cree una imagen de VM de rescate de la misma versión del SO, en el mismo grupo de recursos (RSG) y ubicación con el disco administrado.
1. Use Azure Portal para tomar una instantánea del disco del SO de la máquina virtual afectada.
1. Cree un disco a partir de la instantánea del disco del SO y asócielo a la VM de rescate.
1. Una vez creado el disco, solucione los problemas del entorno de chroot en la VM de rescate.

   1. Acceda a la VM como usuario raíz con el comando siguiente:

      `#sudo su -`

   1. Para buscar el disco, use `dmesg` (el método que use para detectar el nuevo disco puede variar). En el ejemplo siguiente, se usa **dmesg** para filtrar los discos **SCSI**:

      `dmesg | grep SCSI`

      El resultado será similar al del ejemplo siguiente. En este ejemplo, queremos el disco **SDC**:

      ```
      [    0.294784] SCSI subsystem initialized
      [    0.573458] Block layer SCSI generic (bsg) driver version 0.4 loaded (major 252)
      [    7.110271] sd 2:0:0:0: [sda] Attached SCSI disk
      [    8.079653] sd 3:0:1:0: [sdb] Attached SCSI disk
      [ 1828.162306] sd 5:0:0:0: [sdc] Attached SCSI disk
      ```

   1. Use el siguiente comando para activar el grupo de volúmenes lógicos:

      ```
      #vgscan --mknodes
      #vgchange -ay
      #lvscan
      ```

   1. Use el comando `lsblk` para recuperar los nombres del LVM:

      ```
      [user@myvm ~]$ lsblk
      NAME              MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
      sda                 8:0    0   64G  0 disk
      ├─sda1              8:1    0  500M  0 part /boot
      ├─sda2              8:2    0   63G  0 part /
      sdb                 8:16   0    4G  0 disk
      └─sdb1              8:17   0    4G  0 part /mnt/resource
      sdc                 8:0    0   64G  0 disk
      ├─sdc1              8:1    0  500M  0 part
      ├─sdc2              8:2    0   63G  0 part
      ├─sdc3              8:3    0    2M  0 part
      ├─sdc4              8:4    0   63G  0 part
        ├─rootvg-tmplv  253:0    0    2G  0 lvm  
        ├─rootvg-usrlv  253:1    0   10G  0 lvm  
        ├─rootvg-optlv  253:2    0    2G  0 lvm  
        ├─rootvg-homelv 253:3    0    1G  0 lvm  
        ├─rootvg-varlv  253:4    0    8G  0 lvm  
        └─rootvg-rootlv 253:5    0    2G  0 lvm
      ```

   1. Use los comandos siguientes para acceder al entorno de chroot:

      ```
      #mkdir /rescue
      #mount /dev/mapper/rootvg-rootlv /rescue
      #mount /dev/mapper/rootvg-varlv /rescue/var
      #mount /dev/mapper/rootvg-homelv /rescue/home
      #mount /dev/mapper/rootvg-usrlv /rescue/usr
      #mount /dev/mapper/rootvg-tmplv /rescue/tmp
      #mount /dev/mapper/rootvg-optlv /rescue/opt
      #mount /dev/sdc2 /rescue/boot/
      #mount /dev/sdc1 /rescue/boot/efi
      #cd /rescue

      #mount -t proc proc proc
      #mount -t sysfs sys sys/
      #mount -o bind /dev dev/
      #mount -o bind /dev/pts dev/pts/
      #mount -o bind /run run/
      #chroot /rescue
      ```

   1. Solucione los problemas del entorno de chroot.

   1. Use los comandos siguientes para salir del entorno de chroot:

      ```
      #exit

      #umount /rescue/proc/
      #umount /rescue/sys/
      #umount /rescue/dev/pts
      #umount /rescue/dev/
      #umount /rescue/run
      #cd /
      #umount /rescue/boot/efi
      #umount /rescue/boot
      #umount /rescue/home
      #umount /rescue/var
      #umount /rescue/usr
      #umount /rescue/tmp
      #umount /rescue/opt
      #umount /rescue
      ```

      > [!NOTE]
      > Si recibe el error `unable to unmount /rescue`, agregue la opción -l al comando umount.
      >
      > Ejemplo: `umount -l /rescue`

1. Desasocie el disco de la VM de rescate y realice un intercambio de disco con la VM original.
1. Inicie la VM original y compruebe su conectividad.

## <a name="rhel-8x-with-lvm"></a>RHEL 8.x con LVM

   > [!NOTE]
   > Si la VM original incluye el Administrador de volúmenes lógicos (LVM) en el disco del SO, cree la VM de rescate mediante la imagen con particiones RAW en el disco del SO.

1. Detenga o anule la asignación de la VM afectada.
1. Cree una imagen de VM de rescate de la misma versión del SO, en el mismo grupo de recursos (RSG) y ubicación con el disco administrado.
1. Use Azure Portal para tomar una instantánea del disco del SO de la máquina virtual afectada.
1. Cree un disco a partir de la instantánea del disco del SO y asócielo a la VM de rescate.
1. Una vez creado el disco, solucione los problemas del entorno de chroot en la VM de rescate.

   1. Acceda a la VM como usuario raíz con el comando siguiente:

      `#sudo su -`

   1. Para buscar el disco, use `dmesg` (el método que use para detectar el nuevo disco puede variar). En el ejemplo siguiente, se usa **dmesg** para filtrar los discos **SCSI**:

      `dmesg | grep SCSI`

      El resultado será similar al del ejemplo siguiente. En este ejemplo, queremos el disco **SDC**:

      ```
      [    0.294784] SCSI subsystem initialized
      [    0.573458] Block layer SCSI generic (bsg) driver version 0.4 loaded (major 252)
      [    7.110271] sd 2:0:0:0: [sda] Attached SCSI disk
      [    8.079653] sd 3:0:1:0: [sdb] Attached SCSI disk
      [ 1828.162306] sd 5:0:0:0: [sdc] Attached SCSI disk
      ```

   1. Use el siguiente comando para activar el grupo de volúmenes lógicos:

      ```
      #vgscan --mknodes
      #vgchange -ay
      #lvscan
      ```

   1. Use el comando `lsblk` para recuperar los nombres del LVM:

      ```
      [user@myvm ~]$ lsblk
      NAME              MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
      sda                 8:0    0   64G  0 disk
      ├─sda1              8:1    0  500M  0 part /boot
      ├─sda2              8:2    0   63G  0 part /
      sdb                 8:16   0    4G  0 disk
      └─sdb1              8:17   0    4G  0 part /mnt/resource
      sdc                 8:0    0   64G  0 disk
      ├─sdc1              8:1    0  500M  0 part
      ├─sdc2              8:2    0   63G  0 part
      │ ├─rootvg-tmplv  253:0    0    2G  0 lvm  
      │ ├─rootvg-usrlv  253:1    0   10G  0 lvm  
      │ ├─rootvg-homelv 253:2    0    1G  0 lvm  
      │ ├─rootvg-varlv  253:3    0    8G  0 lvm  
      │ └─rootvg-rootlv 253:4    0    2G  0 lvm  
      ├─sdc14             8:14   0    4M  0 part
      └─sdc15             8:15   0  495M  0 part
      ```

   1. Use los comandos siguientes para acceder al entorno de chroot:

      ```
      #mkdir /rescue
      #mount /dev/mapper/rootvg-rootlv /rescue
      #mount /dev/mapper/rootvg-varlv /rescue/var
      #mount /dev/mapper/rootvg-homelv /rescue/home
      #mount /dev/mapper/rootvg-usrlv /rescue/usr
      #mount /dev/mapper/rootvg-tmplv /rescue/tmp
      #mount /dev/sdc1 /rescue/boot/
      #mount /dev/sdc15 /rescue/boot/efi
      #cd /rescue

      #mount -t proc proc proc
      #mount -t sysfs sys sys/
      #mount -o bind /dev dev/
      #mount -o bind /dev/pts dev/pts/
      #mount -o bind /run run/
      #chroot /rescue
      ```

   1. Solucione los problemas del entorno de chroot.

   1. Use los comandos siguientes para salir del entorno de chroot:

      ```
      #exit

      #umount /rescue/proc/
      #umount /rescue/sys/
      #umount /rescue/dev/pts
      #umount /rescue/dev/
      #umount /rescue/run
      #cd /
      #umount /rescue/boot/efi
      #umount /rescue/boot
      #umount /rescue/home
      #umount /rescue/var
      #umount /rescue/usr
      #umount /rescue/tmp
      #umount /rescue
      ```

      > [!NOTE]
      > Si recibe el error `unable to unmount /rescue`, agregue la opción -l al comando umount.
      >
      > Ejemplo: `umount -l /rescue`

1. Desasocie el disco de la VM de rescate y realice un intercambio de disco con la VM original.
1. Inicie la VM original y compruebe su conectividad.

## <a name="oracle-7x"></a>Oracle 7.x

1. Detenga o anule la asignación de la VM afectada.
1. Cree una imagen de VM de rescate de la misma versión del SO, en el mismo grupo de recursos (RSG) y ubicación con el disco administrado.
1. Use Azure Portal para tomar una instantánea del disco del SO de la máquina virtual afectada.
1. Cree un disco a partir de la instantánea del disco del SO y asócielo a la VM de rescate.
1. Una vez creado el disco, solucione los problemas del entorno de chroot en la VM de rescate.

   1. Acceda a la VM como usuario raíz con el comando siguiente:

      `#sudo su -`

   1. Para buscar el disco, use `dmesg` (el método que use para detectar el nuevo disco puede variar). En el ejemplo siguiente, se usa **dmesg** para filtrar los discos **SCSI**:

      `dmesg | grep SCSI`

      El resultado será similar al del ejemplo siguiente. En este ejemplo, queremos el disco **SDC**:

      ```
      [    0.294784] SCSI subsystem initialized
      [    0.573458] Block layer SCSI generic (bsg) driver version 0.4 loaded (major 252)
      [    7.110271] sd 2:0:0:0: [sda] Attached SCSI disk
      [    8.079653] sd 3:0:1:0: [sdb] Attached SCSI disk
      [ 1828.162306] sd 5:0:0:0: [sdc] Attached SCSI disk
      ```

   1. Use los comandos siguientes para acceder al entorno de chroot:

      ```
      #mkdir /rescue
      #mount -o nouuid /dev/sdc2 /rescue
      #mount -o nouuid /dev/sdc1 /rescue/boot/
      #mount /dev/sdc15 /rescue/boot/efi
      #cd /rescue

      #mount -t proc proc proc
      #mount -t sysfs sys sys/
      #mount -o bind /dev dev/
      #mount -o bind /dev/pts dev/pts/
      #mount -o bind /run run/
      ##chroot /rescue
      ```

   1. Solucione los problemas del entorno de chroot.

   1. Use los comandos siguientes para salir del entorno de chroot:

      ```
      #exit

      #umount /rescue/proc/
      #umount /rescue/sys/
      #umount /rescue/dev/pts
      #umount /rescue/dev/
      #umount /rescue/run
      #cd /
      #umount /rescue/boot/efi
      #umount /rescue/boot
      #umount /rescue
      ```

      > [!NOTE]
      > Si recibe el error `unable to unmount /rescue`, agregue la opción -l al comando umount.
      >
      > Ejemplo: `umount -l /rescue`

1. Desasocie el disco de la VM de rescate y realice un intercambio de disco con la VM original.
1. Inicie la VM original y compruebe su conectividad.

## <a name="suse-sles-12-sp4-suse-sles-12-sp4-for-sap---suse-sles-15-sp1-suse-sles-15-sp1-for-sap"></a>SUSE-SLES 12 SP4, SUSE-SLES 12 SP4 para SAP y ## SUSE-SLES 15 SP1, SUSE-SLES 15 SP1 para SAP

1. Detenga o anule la asignación de la VM afectada.
1. Cree una imagen de VM de rescate de la misma versión del SO, en el mismo grupo de recursos (RSG) y ubicación con el disco administrado.
1. Use Azure Portal para tomar una instantánea del disco del SO de la máquina virtual afectada.
1. Cree un disco a partir de la instantánea del disco del SO y asócielo a la VM de rescate.
1. Una vez creado el disco, solucione los problemas del entorno de chroot en la VM de rescate.

   1. Acceda a la VM como usuario raíz con el comando siguiente:

      `#sudo su -`

   1. Para buscar el disco, use `dmesg` (el método que use para detectar el nuevo disco puede variar). En el ejemplo siguiente, se usa **dmesg** para filtrar los discos **SCSI**:

      `dmesg | grep SCSI`

      El resultado será similar al del ejemplo siguiente. En este ejemplo, queremos el disco **SDC**:

      ```
      [    0.294784] SCSI subsystem initialized
      [    0.573458] Block layer SCSI generic (bsg) driver version 0.4 loaded (major 252)
      [    7.110271] sd 2:0:0:0: [sda] Attached SCSI disk
      [    8.079653] sd 3:0:1:0: [sdb] Attached SCSI disk
      [ 1828.162306] sd 5:0:0:0: [sdc] Attached SCSI disk
      ```

   1. Use los comandos siguientes para acceder al entorno de chroot:

      ```
      #mkdir /rescue
      #mount -o nouuid /dev/sdc4 /rescue
      #mount -o nouuid /dev/sdc3 /rescue/boot/
      #mount /dev/sdc2 /rescue/boot/efi
      #cd /rescue

      #mount -t proc proc proc
      #mount -t sysfs sys sys/
      #mount -o bind /dev dev/
      #mount -o bind /dev/pts dev/pts/
      #mount -o bind /run run/
      #chroot /rescue
      ```

   1. Solucione los problemas del entorno de chroot.

   1. Use los comandos siguientes para salir del entorno de chroot:

      ```
      #exit

      #umount /rescue/proc/
      #umount /rescue/sys/
      #umount /rescue/dev/pts
      #umount /rescue/dev/
      #umount /rescue/run
      #cd /
      #umount /rescue/boot/efi
      #umount /rescue/boot
      #umount /rescue
      ```

      > [!NOTE]
      > Si recibe el error `unable to unmount /rescue`, agregue la opción -l al comando umount.
      >
      > Ejemplo: `umount -l /rescue`

1. Desasocie el disco de la VM de rescate y realice un intercambio de disco con la VM original.
1. Inicie la VM original y compruebe su conectividad.

## <a name="next-steps"></a>Pasos siguientes

- [Solución de problemas de conexión SSH](troubleshoot-ssh-connection.md)
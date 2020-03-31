---
title: Creación y carga de un VHD de Ubuntu Linux en Azure
description: Aprenda a crear y cargar un disco duro virtual de Azure (VHD) que contiene el sistema operativo Ubuntu Linux.
author: gbowerman
ms.service: virtual-machines-linux
ms.topic: article
ms.date: 06/24/2019
ms.author: guybo
ms.openlocfilehash: 5fa3415d8663f358bf0ae48be46ac52b8f8b4b06
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "80066728"
---
# <a name="prepare-an-ubuntu-virtual-machine-for-azure"></a>Preparación de una máquina virtual Ubuntu para Azure


Ahora Ubuntu publica discos duros virtuales de Azure oficiales que se pueden descargar en [https://cloud-images.ubuntu.com/](https://cloud-images.ubuntu.com/). Si necesita crear su propia imagen de Ubuntu especializada para Azure, en lugar de usar el siguiente procedimiento manual se recomienda comenzar con estos VHD conocidos y personalizarlos según sea necesario. Las versiones más recientes de las imágenes siempre se encuentran en las siguientes ubicaciones:

* Ubuntu 12.04/precisa: [ubuntu-12.04-server-cloudimg-amd64-disk1.vhd.zip](https://cloud-images.ubuntu.com/precise/current/precise-server-cloudimg-amd64-disk1.vhd.zip)
* Ubuntu 14.04/Trusty: [ubuntu-14.04-server-cloudimg-amd64-disk1.vhd.zip](https://cloud-images.ubuntu.com/releases/trusty/release/ubuntu-14.04-server-cloudimg-amd64-disk1.vhd.zip)
* Ubuntu 16.04/Xenial: [ubuntu-16.04-server-cloudimg-amd64-disk1.vmdk](https://cloud-images.ubuntu.com/releases/xenial/release/ubuntu-16.04-server-cloudimg-amd64-disk1.vmdk)
* Ubuntu 18.04/Bionic: [bionic-server-cloudimg-amd64.vmdk](https://cloud-images.ubuntu.com/bionic/current/bionic-server-cloudimg-amd64.vmdk)
* Ubuntu 18.10/Cosmic: [cosmic-server-cloudimg-amd64.vhd.zip](http://cloud-images.ubuntu.com/releases/cosmic/release/ubuntu-18.10-server-cloudimg-amd64.vhd.zip)

## <a name="prerequisites"></a>Prerequisites
En este artículo se supone que ya ha instalado un sistema operativo Ubuntu Linux en un disco duro virtual. Existen varias herramientas para crear archivos .vhd; por ejemplo, una solución de virtualización como Hyper-V. Para obtener instrucciones, consulte [Instalación del rol de Hyper-V y configuración de una máquina Virtual](https://technet.microsoft.com/library/hh846766.aspx).

**Notas de instalación de Ubuntu**

* Consulte también [Notas generales sobre la instalación de Linux](create-upload-generic.md#general-linux-installation-notes) para obtener más consejos sobre la preparación de Linux para Azure.
* El formato VHDX no se admite en Azure, solo el **VHD fijo**.  Puede convertir el disco al formato VHD con el Administrador de Hyper-V o el cmdlet Convert-VHD.
* Al instalar el sistema Linux se recomienda utilizar las particiones estándar en lugar de un LVM (que a menudo viene de forma predeterminada en muchas instalaciones). De este modo se impedirá que el nombre del LVM entre en conflicto con las máquinas virtuales clonadas, especialmente si en algún momento hace falta adjuntar un disco de SO a otra máquina virtual para solucionar problemas. [LVM](configure-lvm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) o [RAID](configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) se pueden utilizar en discos de datos si así se prefiere.
* No cree una partición de intercambio en el disco del SO. El agente de Linux se puede configurar para crear un archivo de intercambio en el disco de recursos temporal.  Puede encontrar más información al respecto en los pasos que vienen a continuación.
* En Azure, todos los discos duros virtuales deben tener un tamaño virtual alineado con 1 MB. Al convertir un disco sin procesar en un disco duro virtual, tiene que asegurarse de que su tamaño es un múltiplo de 1 MB antes de la conversión. Para más información, consulte [Notas sobre la instalación de Linux](create-upload-generic.md#general-linux-installation-notes).

## <a name="manual-steps"></a>Pasos manuales
> [!NOTE]
> Antes de intentar crear su propia imagen personalizada de Ubuntu para Azure, considere la posibilidad de utilizar las imágenes pregeneradas y probadas de [https://cloud-images.ubuntu.com/](https://cloud-images.ubuntu.com/) en su lugar.
> 
> 

1. Seleccione la máquina virtual en el panel central del Administrador de Hyper-V.

2. Haga clic en **Conectar** para abrir la ventana de la máquina virtual.

3. Sustituya los repositorios actuales de la imagen para utilizar el repositorio de Azure de Ubuntu. Los pasos varían ligeramente en función de la versión de Ubuntu.
   
    Antes de editar `/etc/apt/sources.list` se recomienda realizar una copia de seguridad:
   
        # sudo cp /etc/apt/sources.list /etc/apt/sources.list.bak

    Ubuntu 12.04:
   
        # sudo sed -i 's/[a-z][a-z].archive.ubuntu.com/azure.archive.ubuntu.com/g' /etc/apt/sources.list
        # sudo apt-get update

    Ubuntu 14.04:
   
        # sudo sed -i 's/[a-z][a-z].archive.ubuntu.com/azure.archive.ubuntu.com/g' /etc/apt/sources.list
        # sudo apt-get update

    Ubuntu 16.04:
   
        # sudo sed -i 's/[a-z][a-z].archive.ubuntu.com/azure.archive.ubuntu.com/g' /etc/apt/sources.list
        # sudo apt-get update

4. Las imágenes de Azure de Ubuntu, ahora siguen el kernel de *habilitación de hardware* (HWE). Actualice el sistema operativo con el kernel más reciente ejecutando los comandos siguientes:

    Ubuntu 12.04:
   
        # sudo apt-get update
        # sudo apt-get install linux-image-generic-lts-trusty linux-cloud-tools-generic-lts-trusty
        # sudo apt-get install hv-kvp-daemon-init
        (recommended) sudo apt-get dist-upgrade
   
        # sudo reboot
   
    Ubuntu 14.04:
   
        # sudo apt-get update
        # sudo apt-get install linux-image-virtual-lts-vivid linux-lts-vivid-tools-common
        # sudo apt-get install hv-kvp-daemon-init
        (recommended) sudo apt-get dist-upgrade
   
        # sudo reboot

    Ubuntu 16.04:
   
        # sudo apt-get update
        # sudo apt-get install linux-generic-hwe-16.04 linux-cloud-tools-generic-hwe-16.04
        (recommended) sudo apt-get dist-upgrade

        # sudo reboot
    
    Ubuntu 18.04.04:
    
        # sudo apt-get update
        # sudo apt-get install --install-recommends linux-generic-hwe-18.04 xserver-xorg-hwe-18.04
        # sudo apt-get install --install-recommends linux-cloud-tools-generic-hwe-18.04
        (recommended) sudo apt-get dist-upgrade

        # sudo reboot
    
    **Consulte también:**
    - [https://wiki.ubuntu.com/Kernel/LTSEnablementStack](https://wiki.ubuntu.com/Kernel/LTSEnablementStack)
    - [https://wiki.ubuntu.com/Kernel/RollingLTSEnablementStack](https://wiki.ubuntu.com/Kernel/RollingLTSEnablementStack)


5. Modifique la línea de arranque de kernel para que Grub incluya los parámetros de kernel adicionales para Azure. Para ello, abra `/etc/default/grub` en un editor de texto, busque la variable llamada `GRUB_CMDLINE_LINUX_DEFAULT` (o agréguela si fuera necesario) y edítela para incluir los parámetros siguientes:
   
        GRUB_CMDLINE_LINUX_DEFAULT="console=tty1 console=ttyS0,115200n8 earlyprintk=ttyS0,115200 rootdelay=300"

    Guarde este archivo y ciérrelo, y ejecute `sudo update-grub`. Así se asegurará de que todos los mensajes de la consola se envían al primer puerto serie, lo que puede ayudar al soporte técnico de Azure con los problemas de depuración de errores.

6. Asegúrese de que el servidor SSH se haya instalado y configurado para iniciarse en el tiempo de arranque.  Este es normalmente el valor predeterminado.

7. Instale el Agente de Linux de Azure:
   
        # sudo apt-get update
        # sudo apt-get install walinuxagent

   > [!Note]
   >  El paquete `walinuxagent` puede eliminar los paquetes `NetworkManager` y `NetworkManager-gnome`, en caso de que estén instalados.


1. Ejecute los comandos siguientes para desaprovisionar la máquina virtual y prepararla para aprovisionarse en Azure:
   
        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout

1. Haga clic en **Acción -> Apagar** en el Administrador de Hyper-V. El VHD de Linux ya está listo para cargarse en Azure.

## <a name="references"></a>Referencias
[Kernel de habilitación de hardware (HWE) de Ubuntu](https://wiki.ubuntu.com/Kernel/LTSEnablementStack)

## <a name="next-steps"></a>Pasos siguientes
Ya está listo para usar el disco duro virtual de Ubuntu para crear nuevas máquinas virtuales de Azure. Si es la primera vez que carga el archivo .vhd en Azure, vea [Crear una VM Linux a partir de un disco personalizado](upload-vhd.md#option-1-upload-a-vhd).


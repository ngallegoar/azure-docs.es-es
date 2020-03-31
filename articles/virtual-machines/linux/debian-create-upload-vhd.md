---
title: Preparación de un disco duro virtual con Debian Linux
description: Aprenda a crear imágenes de VHD de Debian para implementar máquinas virtuales en Azure.
author: gbowerman
ms.service: virtual-machines-linux
ms.topic: article
ms.date: 11/13/2018
ms.author: guybo
ms.openlocfilehash: d54f7a11d929c31fee29a788eb3a2ae2cc8f2703
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "80066711"
---
# <a name="prepare-a-debian-vhd-for-azure"></a>Preparar VHD en Debian de Azure
## <a name="prerequisites"></a>Prerequisites
En esta sección, se supone que ya instaló en un sistema operativo Debian Linux desde un archivo .iso que obtuvo del [sitio web Debian](https://www.debian.org/distrib/) y que descargó a un disco duro virtual. Existen varias herramientas para crear archivos .vhd; Hyper-V es solo un ejemplo. Para obtener instrucciones acerca de cómo usar Hyper-V, consulte [Instalación del rol de Hyper-V y configuración de una máquina virtual](https://technet.microsoft.com/library/hh846766.aspx).

## <a name="installation-notes"></a>Notas de instalación
* Consulte también las [Notas generales sobre la instalación de Linux](create-upload-generic.md#general-linux-installation-notes) para obtener más consejos sobre la preparación de Linux para Azure.
* el reciente formato VHDX no se admite en Azure. Puede convertir el disco al formato VHD con el Administrador de Hyper-V o el cmdlet **convert-vhd** .
* Al instalar el sistema Linux, se recomienda utilizar las particiones estándar en lugar de un LVM (que a menudo viene de forma predeterminada en muchas instalaciones). De este modo se impedirá que el nombre del LVM entre en conflicto con las máquinas virtuales clonadas, especialmente si en algún momento hace falta adjuntar un disco de SO a otra máquina virtual para solucionar problemas. [LVM](configure-lvm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) o [RAID](configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) se pueden utilizar en discos de datos si así se prefiere.
* No cree una partición de intercambio en el disco del SO. El agente de Linux de Azure se puede configurar para crear un archivo de intercambio en el disco de recursos temporal. Puede encontrar más información en los pasos que vienen a continuación.
* En Azure, todos los discos duros virtuales deben tener un tamaño virtual alineado con 1 MB. Al convertir un disco sin procesar en un disco duro virtual, tiene que asegurarse de que su tamaño es un múltiplo de 1 MB antes de la conversión. Para obtener más información, consulte las [Notas sobre la instalación de Linux](create-upload-generic.md#general-linux-installation-notes).

## <a name="use-azure-manage-to-create-debian-vhds"></a>Uso de Azure-Manage para crear VHD Debian
Hay herramientas disponibles para generar un VHD de Debian para Azure, como los scripts [azure-manage](https://github.com/credativ/azure-manage) de [Credativ](https://www.credativ.com/). Este es el enfoque recomendado, en lugar de crear una imagen desde el principio. Por ejemplo, para crear un VHD de Debian 8, ejecute los comandos siguientes para descargar la utilidad `azure-manage` (y sus dependencias) y ejecute el script `azure_build_image`:

    # sudo apt-get update
    # sudo apt-get install git qemu-utils mbr kpartx debootstrap

    # sudo apt-get install python3-pip python3-dateutil python3-cryptography
    # sudo pip3 install azure-storage azure-servicemanagement-legacy azure-common pytest pyyaml
    # git clone https://github.com/credativ/azure-manage.git
    # cd azure-manage
    # sudo pip3 install .

    # sudo azure_build_image --option release=jessie --option image_size_gb=30 --option image_prefix=debian-jessie-azure section


## <a name="manually-prepare-a-debian-vhd"></a>Preparación manual de un VHD Debian
1. En el Administrador de Hyper-V, seleccione la máquina virtual.
2. Haga clic en **Conectar** para abrir una ventana de consola de la máquina virtual.
3. Si instaló el sistema operativo mediante una imagen ISO, marque como comentario cualquier línea relacionada con "`deb cdrom`" en `/etc/apt/source.list`.

4. Edite el archivo `/etc/default/grub` y modifique el parámetro **GRUB_CMDLINE_LINUX** tal como se muestra a continuación para incluir parámetros de kernel adicionales de Azure.
   
        GRUB_CMDLINE_LINUX="console=tty0 console=ttyS0,115200n8 earlyprintk=ttyS0,115200"

5. Recompile el sistema GRUB y ejecute:

        # sudo update-grub

6. Agregue repositorios de Azure de Debian a /etc/apt/sources.list para Debian 8 o 9:

    **Debian 8.x "Jessie"**

        deb http://debian-archive.trafficmanager.net/debian jessie main
        deb-src http://debian-archive.trafficmanager.net/debian jessie main
        deb http://debian-archive.trafficmanager.net/debian-security jessie/updates main
        deb-src http://debian-archive.trafficmanager.net/debian-security jessie/updates
        deb http://debian-archive.trafficmanager.net/debian jessie-updates main
        deb-src http://debian-archive.trafficmanager.net/debian jessie-updates main
        deb http://debian-archive.trafficmanager.net/debian jessie-backports main
        deb-src http://debian-archive.trafficmanager.net/debian jessie-backports main

    **Debian 9.x "Stretch"**

        deb http://debian-archive.trafficmanager.net/debian stretch main
        deb-src http://debian-archive.trafficmanager.net/debian stretch main
        deb http://debian-archive.trafficmanager.net/debian-security stretch/updates main
        deb-src http://debian-archive.trafficmanager.net/debian-security stretch/updates main
        deb http://debian-archive.trafficmanager.net/debian stretch-updates main
        deb-src http://debian-archive.trafficmanager.net/debian stretch-updates main
        deb http://debian-archive.trafficmanager.net/debian stretch-backports main
        deb-src http://debian-archive.trafficmanager.net/debian stretch-backports main


7. Instale el Agente de Linux de Azure:
   
        # sudo apt-get update
        # sudo apt-get install waagent

8. Para Debian 9 y versiones posteriores, se recomienda usar el nuevo kernel en la nube de Debian para las máquinas virtuales en Azure. Para instalar este nuevo kernel, en primer lugar, cree un archivo llamado /etc/apt/preferences.d/linux.pref con el siguiente contenido:
   
        Package: linux-* initramfs-tools
        Pin: release n=stretch-backports
        Pin-Priority: 500
   
    A continuación, ejecute "sudo apt-get install linux-image-cloud-amd64" para instalar el nuevo kernel en la nube de Debian.

9. Desaprovisione la máquina virtual, prepárela para aprovisionarla en Azure y ejecute:
   
        # sudo waagent –force -deprovision
        # export HISTSIZE=0
        # logout

10. Haga clic en **Acción** -> Apagar en el Administrador de Hyper-V. El VHD de Linux ya está listo para cargarse en Azure.

## <a name="next-steps"></a>Pasos siguientes
Ya está listo para usar el disco duro virtual de Debian para crear nuevas máquinas virtuales de Azure. Si es la primera vez que carga el archivo .vhd en Azure, vea [Crear una VM Linux a partir de un disco personalizado](upload-vhd.md#option-1-upload-a-vhd).


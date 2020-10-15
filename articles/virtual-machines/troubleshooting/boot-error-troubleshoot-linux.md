---
title: Solución de errores de arranque en Azure Linux Virtual Machines | Microsoft Docs
description: En este artículo se muestran enlaces a artículos para solucionar problemas de arranque en Azure Linux Virtual Machines.
services: virtual-machines-linux
documentationCenter: ''
author: vilibert
manager: spogge
editor: ''
ms.service: virtual-machines-linux
ms.topic: troubleshooting
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 11/20/2019
ms.author: vilibert
ms.openlocfilehash: 37cb201751f72918838efe5837aa0e357d483f24
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "74408562"
---
# <a name="troubleshoot-azure-linux-virtual-machines-boot-errors"></a>Solución de errores de arranque de Azure Linux Virtual Machines

En este artículo se enumeran los errores de arranque comunes que puede recibir al iniciar una máquina virtual (VM) Linux en Microsoft Azure. Para obtener más información acerca de los errores, consulte los artículos de la sección **Errores de arranque y soluciones**.

## <a name="boot-errors-and-solutions"></a>Errores de arranque y soluciones

* [Rescate de GRUB](troubleshoot-vm-boot-error.md)

## <a name="next-steps"></a>Pasos siguientes

- [Consola serie de máquinas virtuales](serial-console-linux.md)

Solucione los problemas de una máquina virtual Linux mediante la asociación del disco del sistema operativo a una máquina virtual de recuperación mediante Azure:

- [Reparación de máquinas virtuales de Azure](repair-linux-vm-using-azure-virtual-machine-repair-commands.md)

 Intercambio de disco: se puede automatizar mediante:
- [Scripts de recuperación de Power Shell](https://github.com/Azure/azure-support-scripts/tree/master/VMRecovery/ResourceManager)
- [Scripts de recuperación de Bash](https://github.com/sribs/azure-support-scripts)

- [CLI](troubleshoot-recovery-disks-linux.md)
- [Azure Portal](troubleshoot-recovery-disks-portal-linux.md)


## <a name="disk-swap-video"></a>Vídeo de intercambio de discos:

Si no tiene acceso a GRUB, vea en [este](https://youtu.be/m5t0GZ5oGAc) vídeo cómo puede automatizar fácilmente el procedimiento de intercambio de discos para recuperar la máquina virtual

## <a name="unofficial-solution"></a>Solución no oficial

También se puede intentar recuperar una máquina virtual con el script BETA no compatible [ALAR](https://github.com/malachma/azure-auto-recover)
---
title: Comprobar el sistema de archivos al iniciar una máquina virtual de Azure | Microsoft Docs
description: Aprenda a resolver el problema que muestra la máquina virtual cuando comprueba el sistema de archivos al iniciarse | Microsoft Docs
services: virtual-machines-windows
documentationCenter: ''
author: genlin
manager: dcscontentpm
editor: ''
ms.service: virtual-machines-windows
ms.topic: troubleshooting
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 08/31/2018
ms.author: genli
ms.openlocfilehash: 86938c582745cb0759eda9cd0693f407471a0529
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "77921494"
---
# <a name="windows-shows-checking-file-system-when-booting-an-azure-vm"></a>Windows muestra el mensaje "comprobando el sistema de archivos" al iniciar una máquina virtual de Azure

En este artículo se describe el error "Comprobando el sistema de archivos" que puede surgir al iniciar una máquina Virtual (VM) de Windows en Microsoft Azure.


## <a name="symptom"></a>Síntoma 

Una máquina virtual de Windows no se inicia. Cuando comprueba las capturas de pantalla de inicio en [Diagnósticos de inicio](boot-diagnostics.md), ve que el proceso Comprobar disco (chkdsk.exe) se está ejecutando con uno de los siguientes mensajes:

- Escaneando y reparando la unidad (C :)
- Comprobando el sistema de archivos de C:

## <a name="cause"></a>Causa

Si se encuentra un error NTFS en el sistema de archivos, Windows comprobará y reparará la consistencia del disco en el siguiente reinicio. Por lo general, esto sucede si la máquina virtual tuvo un reinicio inesperado o si el proceso de cierre de la máquina virtual se interrumpió de forma repentina.

## <a name="solution"></a>Solución 

Windows se iniciará normalmente después de que se complete el proceso de comprobación de disco. Si la máquina virtual está atascada en el proceso de comprobación de disco, intente ejecutar la operación Comprobar disco en la máquina virtual sin conexión:
1.  Tome una instantánea del disco del sistema operativo de la máquina virtual afectada como copia de seguridad. Para obtener más información, consulte [Instantánea de un disco](../windows/snapshot-copy-managed-disk.md).
2.  [Conecte el disco del sistema operativo a una máquina virtual de recuperación](troubleshoot-recovery-disks-portal-windows.md).  
3.  En la máquina virtual de recuperación, ejecute la opción Comprobar disco en el disco del sistema operativo conectado. En el ejemplo siguiente, la letra de unidad del disco del sistema operativo conectado es E: 
        
        chkdsk E: /f
4.  Una vez completada la comprobación de disco, desasocie el disco de la máquina virtual de recuperación y, a continuación, vuelva a asociarlo a la máquina virtual afectada como disco del sistema operativo. Para obtener más información, consulte [Solución de problemas de una máquina virtual Windows mediante la conexión del disco del sistema operativo a una máquina virtual de recuperación](troubleshoot-recovery-disks-portal-windows.md).

---
title: No se puede conectar a Azure Virtual Machines de forma remota porque la máquina virtual se inicia en modo seguro | Microsoft Docs
description: Obtenga información sobre cómo solucionar un problema en el que no se puede usar RDP en una máquina virtual porque esta se inicia en modo seguro.| Microsoft Docs
services: virtual-machines-windows
documentationCenter: ''
author: genlin
manager: dcscontentpm
editor: ''
ms.service: virtual-machines-windows
ms.topic: troubleshooting
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 11/13/2018
ms.author: genli
ms.openlocfilehash: d424cccc0a50198f3ca8c6c040afb87f44282d47
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "86508906"
---
#  <a name="cannot-rdp-to-a-vm-because-the-vm-boots-into-safe-mode"></a>No se puede usar RDP en una máquina virtual porque esta se inicia en modo seguro

En este artículo se muestra cómo resolver un problema en el que no se puede conectar a Azure Windows Virtual Machines (máquinas virtuales) porque la máquina virtual está configurada para iniciarse en modo seguro.


## <a name="symptoms"></a>Síntomas

No puede realizar una conexión RDP u otras conexiones (como HTTP) a una máquina virtual en Azure porque está configurada para iniciarse en modo seguro. Al comprobar la captura de pantalla de los [Diagnósticos de arranque](../troubleshooting/boot-diagnostics.md) en Azure Portal, puede ver que la máquina virtual se inicia de manera normal, pero que la interfaz de red no está disponible:

![Imagen sobre la interfaz de red en modo seguro](./media/troubleshoot-rdp-safe-mode/network-safe-mode.png)

## <a name="cause"></a>Causa

El servicio RDP no está disponible en el modo seguro. Solo los servicios y programas esenciales del sistema se cargan cuando la máquina virtual se inicia en modo seguro. Esto se aplica a las dos versiones diferentes del modo seguro, que son "Mínimo de arranque seguro" y "Arranque seguro con conectividad".


## <a name="solution"></a>Solución

Antes de seguir estos pasos, tome una instantánea del disco del sistema operativo de la máquina virtual afectada como copia de seguridad. Para obtener más información, consulte [Instantánea de un disco](../windows/snapshot-copy-managed-disk.md).

Para resolver este problema, use el control de serie para configurar la máquina virtual para que se inicie en modo normal o [repare la máquina virtual sin conexión](#repair-the-vm-offline) mediante una máquina virtual de recuperación.

### <a name="use-serial-control"></a>Uso del control serie

1. Conéctese a una [consola serie y abra la instancia de CMD](./serial-console-windows.md#use-cmd-or-powershell-in-serial-console
   ). Si la consola serie no está habilitada en la máquina virtual, vaya a la sección de [reparación de la máquina virtual sin conexión](#repair-the-vm-offline).
2. Consulte los datos de configuración de arranque:

    ```console
    bcdedit /enum
    ```

    Si la máquina virtual está configurada para iniciarse en modo seguro, verá una marca adicional en la sección del **cargador de arranque de Windows** denominada **safeboot**. Si no ve la marca **safeboot**, quiere decir que la máquina virtual no está en modo seguro. El contenido de este artículo no se aplica a este escenario.

    La marca **safeboot** puede aparecer con los siguientes valores:
   - mínimo
   - Red

     No se iniciará RDP en ninguno de estos dos modos. Por lo tanto, la corrección sigue siendo la misma.

     ![Imagen de la marca del modo seguro](./media/troubleshoot-rdp-safe-mode/safe-mode-tag.png)

3. Elimine la marca **safemoade** marca para que la máquina virtual se inicie en modo normal:

    ```console
    bcdedit /deletevalue {current} safeboot
    ```

4. Compruebe los datos de configuración de arranque para asegurarse de que la marca **safeboot** se quita:

    ```console
    bcdedit /enum
    ```

5. A continuación, reinicie la máquina virtual y compruebe si se ha resuelto el problema.

### <a name="repair-the-vm-offline"></a>Reparación de la máquina virtual sin conexión

#### <a name="attach-the-os-disk-to-a-recovery-vm"></a>Conecte el disco del sistema operativo a una máquina virtual de recuperación.

1. [Conecte el disco del sistema operativo a una máquina virtual de recuperación](./troubleshoot-recovery-disks-portal-windows.md).
2. Inicie una conexión mediante el Escritorio remoto a la máquina virtual de recuperación.
3. Asegúrese de que el disco aparece marcado como **En línea** en la consola de Administración de discos. Anote la letra de unidad que se asigna al disco del sistema operativo conectado.

#### <a name="enable-dump-log-and-serial-console-optional"></a>Habilitar el registro de volcado de memoria y la consola serie (opcional)

El registro de volcado de memoria y la consola serie le ayudarán a buscar otras soluciones si no se puede resolver el problema mediante la solución que se indica en este artículo.

Para habilitar el registro de volcado de memoria y Serial Console, ejecute el siguiente script.

1. Abra una sesión de símbolo del sistema con privilegios elevados (**Ejecutar como administrador**).
2. Ejecute el siguiente script:

    En este script, se supone que la letra de unidad que se asigna al disco del sistema operativo conectado es F. Reemplácela por el valor apropiado para su máquina virtual.

    ```powershell
    reg load HKLM\BROKENSYSTEM F:\windows\system32\config\SYSTEM

    REM Enable Serial Console
    bcdedit /store F:\boot\bcd /set {bootmgr} displaybootmenu yes
    bcdedit /store F:\boot\bcd /set {bootmgr} timeout 5
    bcdedit /store F:\boot\bcd /set {bootmgr} bootems yes
    bcdedit /store F:\boot\bcd /ems {<BOOT LOADER IDENTIFIER>} ON
    bcdedit /store F:\boot\bcd /emssettings EMSPORT:1 EMSBAUDRATE:115200

    REM Suggested configuration to enable OS Dump
    REG ADD "HKLM\BROKENSYSTEM\ControlSet001\Control\CrashControl" /v CrashDumpEnabled /t REG_DWORD /d 1 /f
    REG ADD "HKLM\BROKENSYSTEM\ControlSet001\Control\CrashControl" /v DumpFile /t REG_EXPAND_SZ /d "%SystemRoot%\MEMORY.DMP" /f
    REG ADD "HKLM\BROKENSYSTEM\ControlSet001\Control\CrashControl" /v NMICrashDump /t REG_DWORD /d 1 /f

    REG ADD "HKLM\BROKENSYSTEM\ControlSet002\Control\CrashControl" /v CrashDumpEnabled /t REG_DWORD /d 1 /f
    REG ADD "HKLM\BROKENSYSTEM\ControlSet002\Control\CrashControl" /v DumpFile /t REG_EXPAND_SZ /d "%SystemRoot%\MEMORY.DMP" /f
    REG ADD "HKLM\BROKENSYSTEM\ControlSet002\Control\CrashControl" /v NMICrashDump /t REG_DWORD /d 1 /f

    reg unload HKLM\BROKENSYSTEM
    ```

#### <a name="configure-the-windows-to-boot-into-normal-mode"></a>Configure Windows para arrancar en modo normal

1. Abra una sesión de símbolo del sistema con privilegios elevados (**Ejecutar como administrador**).
2. Consulte los datos de configuración de arranque. En los siguientes comandos, se supone que la letra de unidad que se asigna al disco del sistema operativo conectado es F. Reemplácela por el valor apropiado para su máquina virtual.

    ```console
    bcdedit /store F:\boot\bcd /enum
    ```

    Tome nota del nombre del identificador de la partición que tiene la carpeta **\windows**. De forma predeterminada, el nombre del identificador es "Default".

    Si la máquina virtual está configurada para iniciarse en modo seguro, verá una marca adicional en la sección del **cargador de arranque de Windows** denominada **safeboot**. Si no ve la marca **safeboot**, este artículo no es aplicable a su escenario.

    ![La imagen sobre el identificador de arranque](./media/troubleshoot-rdp-safe-mode/boot-id.png)

3. Elimine la marca **safeboot** para que la máquina virtual se inicie en modo normal:

    ```console
    bcdedit /store F:\boot\bcd /deletevalue {Default} safeboot
    ```

4. Compruebe los datos de configuración de arranque para asegurarse de que la marca **safeboot** se quita:

    ```console
    bcdedit /store F:\boot\bcd /enum
    ```

5. [Desconecte el disco del sistema operativo y vuelva a crear la máquina virtual](./troubleshoot-recovery-disks-portal-windows.md). A continuación, compruebe si se ha resuelto el problema.

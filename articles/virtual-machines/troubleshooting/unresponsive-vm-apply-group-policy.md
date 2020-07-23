---
title: La máquina virtual de Azure no responde al aplicar la directiva
description: En este artículo se proporcionan los pasos para resolver los problemas en los que la pantalla de carga no responde cuando se aplica una directiva durante el arranque en una máquina virtual de Azure.
services: virtual-machines-windows
documentationcenter: ''
author: TobyTu
manager: dcscontentpm
editor: ''
tags: azure-resource-manager
ms.assetid: a97393c3-351d-4324-867d-9329e31b5628
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.topic: troubleshooting
ms.date: 05/07/2020
ms.author: v-mibufo
ms.openlocfilehash: cbf2fe491e1fe0b553eab04ca7190da0413a3ba6
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/20/2020
ms.locfileid: "86526017"
---
# <a name="vm-is-unresponsive-when-applying-group-policy-local-users-and-groups-policy"></a>La máquina virtual no responde al aplicar la directiva de usuarios y grupos locales de directiva de grupo

En este artículo se proporcionan los pasos para resolver los problemas en los que la pantalla de carga no responde cuando una máquina virtual de Azure aplica una directiva durante el arranque.

## <a name="symptoms"></a>Síntomas

Al usar [diagnósticos de arranque](./boot-diagnostics.md) para ver una captura de pantalla de la máquina virtual, la pantalla queda bloqueada durante la carga y se muestra el mensaje: "Applying Group Policy Local Users and Groups policy" (Aplicando la directiva de usuarios y grupos locales de directiva de grupo).

:::image type="content" source="media//unresponsive-vm-apply-group-policy/applying-group-policy-1.png" alt-text="Captura de pantalla que muestra Applying Group Policy Local Users and Groups policy (Aplicando la directiva de usuarios y grupos locales de la directiva de grupo) cargándose (Windows Server 2012 R2).":::

:::image type="content" source="media/unresponsive-vm-apply-group-policy/applying-group-policy-2.png" alt-text="Captura de pantalla de Applying Group Policy Local Users and Groups policy (Aplicando la directiva de usuarios y grupos locales de la directiva de grupo) cargándose (Windows Server 2012).":::

## <a name="cause"></a>Causa

Hay bloqueos en conflicto cuando la directiva intenta limpiar los perfiles de usuario antiguos.

> [!NOTE]
> Esto solo se aplica a Windows Server 2012 y Windows Server 2012 R2.

Esta es la directiva problemática:

`Computer Configuration\Policies\Administrative Templates\System/User Profiles\Delete user profiles older than a specified number of days on system restart`

## <a name="resolution"></a>Resolución

### <a name="process-overview"></a>Información general del proceso

1. [Cree una máquina virtual de reparación y acceda a ella](#step-1-create-and-access-a-repair-vm).
1. [Deshabilite la directiva](#step-2-disable-the-policy).
1. [Habilite la consola serie y la recopilación del volcado de memoria](#step-3-enable-serial-console-and-memory-dump-collection).
1. [Recompile la máquina virtual](#step-4-rebuild-the-vm).

> [!NOTE]
> Si encuentra este error de arranque, quiere decir que el sistema operativo invitado no está operativo. Debe solucionar problemas en el modo sin conexión.

### <a name="step-1-create-and-access-a-repair-vm"></a>Paso 1: Creación de una máquina virtual de reparación y acceso a ella

1. Siga los [pasos 1a 3 de los comandos de reparación de la máquina virtual](./repair-windows-vm-using-azure-virtual-machine-repair-commands.md#repair-process-example) para preparar una máquina virtual de reparación.
2. Use Conexión a Escritorio remoto para conectarse a la máquina virtual de reparación.

### <a name="step-2-disable-the-policy"></a>Paso 2: Deshabilite la directiva.

1. En la máquina virtual de reparación, abra el Editor del Registro.
1. Busque la clave **HKEY_LOCAL_MACHINE** y seleccione **Archivo** > **Cargar subárbol** en el menú.

    :::image type="content" source="media/unresponsive-vm-apply-group-policy/registry.png" alt-text="Captura de pantalla que muestra la clave HKEY_LOCAL_MACHINE resaltada y el menú que contiene Cargar subárbol.":::

    - Puede usar Cargar subárbol para cargar las claves del Registro desde un sistema sin conexión. En este caso, el sistema es el disco roto conectado a la máquina virtual de reparación.
    - La configuración de todo el sistema se almacena en `HKEY_LOCAL_MACHINE` y se puede abreviar como "HKLM".
1. En el disco asociado, vaya al archivo `\windows\system32\config\SOFTWARE` y ábralo.

    1. Cuando se le pida un nombre, escriba BROKENSOFTWARE.
    1. Para comprobar que se cargó BROKENSOFTWARE, expanda **HKEY_LOCAL_MACHINE** y busque la clave BROKENSOFTWARE agregada.
1. Vaya a BROKENSOFTWARE y compruebe si la clave CleanupProfile existe en el subárbol cargado.

    1. Si la clave existe, se establece la directiva CleanupProfile. Su valor representa la directiva de retención medida en días. Siga con la eliminación de la clave.
    1. Si la clave no existe, no se establece la directiva CleanupProfile. [Envíe una incidencia de soporte técnico](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) que incluya el archivo memory.dmp ubicado en el directorio de Windows del disco del sistema operativo asociado.

1. Elimine la clave CleanupProfiles con este comando:

    ```
    reg delete "HKLM\BROKENSOFTWARE\Policies\Microsoft\Windows\System" /v CleanupProfiles /f
    ```
1.  Descargue el subárbol BROKENSOFTWARE con este comando:

    ```
    reg unload HKLM\BROKENSOFTWARE
    ```

### <a name="step-3-enable-serial-console-and-memory-dump-collection"></a>Paso 3: Habilite la consola serie y la recopilación del volcado de memoria.

Para habilitar la consola serie y la recopilación del volcado de memoria, ejecute el siguiente script:

1. Abra una sesión de símbolo del sistema con privilegios elevados. (Ejecute como administrador).
1. Ejecute estos comandos para habilitar la consola serie:
    
    ```
    bcdedit /store <VOLUME LETTER WHERE THE BCD FOLDER IS>:\boot\bcd /ems {<BOOT LOADER IDENTIFIER>} ON
    ```

    ```
    bcdedit /store <VOLUME LETTER WHERE THE BCD FOLDER IS>:\boot\bcd /emssettings EMSPORT:1 EMSBAUDRATE:115200
    ```
1. Asegúrese de que el espacio disponible en el disco del sistema operativo sea al menos igual que el tamaño de la memoria de la máquina virtual (RAM).

    Si no hay suficiente espacio en el disco del sistema operativo, cambie la ubicación del volcado de memoria a un disco de datos asociado con suficiente espacio disponible. Para cambiar la ubicación, reemplace "%SystemRoot%" por la letra de unidad (por ejemplo, "F:") del disco de datos en los comandos siguientes.

    **Configuración sugerida para habilitar el volcado de memoria del sistema operativo**

    Cargar el disco del sistema operativo roto:

    ```
    REG LOAD HKLM\BROKENSYSTEM <VOLUME LETTER OF BROKEN OS DISK>:\windows\system32\config\SYSTEM
    ```

    Habilitar en ControlSet001:
    
    ```
    REG ADD "HKLM\BROKENSYSTEM\ControlSet001\Control\CrashControl" /v CrashDumpEnabled /t REG_DWORD /d 1 /f 
    REG ADD "HKLM\BROKENSYSTEM\ControlSet001\Control\CrashControl" /v DumpFile /t REG_EXPAND_SZ /d "%SystemRoot%\MEMORY.DMP" /f 
    REG ADD "HKLM\BROKENSYSTEM\ControlSet001\Control\CrashControl" /v NMICrashDump /t REG_DWORD /d 1 /f 
    ```

    Habilitar en ControlSet002:
    
    ```
    REG ADD "HKLM\BROKENSYSTEM\ControlSet002\Control\CrashControl" /v CrashDumpEnabled /t REG_DWORD /d 1 /f 
    REG ADD "HKLM\BROKENSYSTEM\ControlSet002\Control\CrashControl" /v DumpFile /t REG_EXPAND_SZ /d "%SystemRoot%\MEMORY.DMP" /f 
    REG ADD "HKLM\BROKENSYSTEM\ControlSet002\Control\CrashControl" /v NMICrashDump /t REG_DWORD /d 1 /f 
    ```
    
    Descargar disco del sistema operativo roto:

    ```
    REG UNLOAD HKLM\BROKENSYSTEM
    ```

### <a name="step-4-rebuild-the-vm"></a>Paso 4: Recompilación de la máquina virtual

Siga el [paso 5 de los comandos de reparación de máquinas virtuales](./repair-windows-vm-using-azure-virtual-machine-repair-commands.md#repair-process-example) para volver a ensamblar la máquina virtual.

Si el problema se resuelve, la directiva se ha deshabilitado ahora localmente. Si lo que quiere es una solución permanente, no use la directiva CleanupProfiles en las máquinas virtuales. Use un método diferente para realizar limpiezas de perfiles.

No use esta directiva:

`Machine\Admin Templates\System\User Profiles\Delete user profiles older than a specified number of days on system restart`

## <a name="next-steps"></a>Pasos siguientes

Si tiene problemas al aplicar Windows Update, consulte [La máquina virtual no responde y aparece el error C01A001D al aplicar Windows Update](./unresponsive-vm-apply-windows-update.md).

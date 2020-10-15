---
title: Error crítico del servicio al iniciar una máquina virtual de Azure | Microsoft Docs
description: Obtenga información sobre cómo solucionar el error "0x0000005A-Error crítico del servicio" que se produce al arrancar | Microsoft Docs
services: virtual-machines-windows
documentationCenter: ''
author: genlin
manager: dcscontentpm
editor: ''
ms.service: virtual-machines-windows
ms.topic: troubleshooting
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 10/08/2018
ms.author: genli
ms.openlocfilehash: 8c3e76f1a7edffefc8773dfa548773ec0932fae6
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "86129853"
---
# <a name="windows-shows-critical-service-failed-on-blue-screen-when-booting-an-azure-vm"></a>Windows muestra el mensaje "Error crítico del servicio" en pantalla azul al arrancar una máquina virtual de Azure
En este artículo se describe el error "Error crítico del servicio" que puede surgir al iniciar una máquina virtual Windows en Microsoft Azure. Proporciona pasos de solución de problemas para ayudar a resolver los problemas. 


## <a name="symptom"></a>Síntoma 

Una máquina virtual de Windows no se inicia. Al revisar las capturas de pantalla de inicio en [Diagnósticos de arranque](./boot-diagnostics.md), ve uno de los mensajes de error siguientes en una pantalla azul:

- "Se produjo un problema con su equipo y se debe reiniciar. Puede reiniciar. Para más información sobre este problema y posibles soluciones, visite https://windows.com/stopcode. Si se llama al departamento de soporte técnico, aporte esta información: Código de detención: CRITICAL SERVICE FAILED (ERROR CRÍTICO DEL SERVICIO)" 
- "Se produjo un problema con su equipo y se debe reiniciar. Solo vamos a recopilar alguna información de error y, a continuación, lo reiniciaremos automáticamente. Si desea más información, después puede buscar en línea este error: CRITICAL_SERVICE_FAILED"

## <a name="cause"></a>Causa

Hay varias causas para los errores graves. Las causas más comunes son:
- Problema con un controlador
- Memoria o archivo de sistema dañados
- Una aplicación accede a un sector prohibido de la memoria

## <a name="solution"></a>Solución 

Para resolver este problema, [póngase en contacto con el equipo de soporte técnico y envíe un archivo de volcado](./troubleshoot-common-blue-screen-error.md#collect-memory-dump-file) que ayudará a diagnosticar el problema más rápidamente o intente la siguiente solución de autoayuda.

### <a name="attach-the-os-disk-to-a-recovery-vm"></a>Conecte el disco del sistema operativo a una máquina virtual de recuperación.

1. Tome una instantánea del disco del sistema operativo de la máquina virtual afectada como copia de seguridad. Para obtener más información, consulte [Instantánea de un disco](../windows/snapshot-copy-managed-disk.md).
2. [Conecte el disco del sistema operativo a una máquina virtual de recuperación](./troubleshoot-recovery-disks-portal-windows.md). 
3. Establezca una conexión de escritorio remoto a la máquina virtual de recuperación.

### <a name="enable-dump-logs-and-serial-console"></a>Habilitación de los registros de volcado de memoria y la consola serie

El registro de volcado de memoria y la [consola serie](./serial-console-windows.md) nos ayudarán en la solución de problemas.

Para habilitar los registros de volcado de memoria y la consola serie, ejecute el siguiente script.

1. Abra una sesión de símbolo del sistema con privilegios elevados (Ejecutar como administrador).
2. Ejecute el siguiente script:

    En este script, se supone que la letra de unidad que se asigna al disco del sistema operativo conectado es F. Reemplácela por el valor adecuado en la máquina virtual.

    ```powershell
    reg load HKLM\BROKENSYSTEM F:\windows\system32\config\SYSTEM.hiv

    REM Enable Serial Console
    bcdedit /store F:\boot\bcd /set {bootmgr} displaybootmenu yes
    bcdedit /store F:\boot\bcd /set {bootmgr} timeout 10
    bcdedit /store F:\boot\bcd /set {bootmgr} bootems yes
    bcdedit /store F:\boot\bcd /ems {<BOOT LOADER IDENTIFIER>} ON
    bcdedit /store F:\boot\bcd /emssettings EMSPORT:1 EMSBAUDRATE:115200

    REM Suggested configuration to enable OS Dump
    REG ADD "HKLM\BROKENSYSTEM\ControlSet001\Control\CrashControl" /v CrashDumpEnabled /t REG_DWORD /d 2 /f
    REG ADD "HKLM\BROKENSYSTEM\ControlSet001\Control\CrashControl" /v DumpFile /t REG_EXPAND_SZ /d "%SystemRoot%\MEMORY.DMP" /f
    REG ADD "HKLM\BROKENSYSTEM\ControlSet001\Control\CrashControl" /v NMICrashDump /t REG_DWORD /d 1 /f

    REG ADD "HKLM\BROKENSYSTEM\ControlSet002\Control\CrashControl" /v CrashDumpEnabled /t REG_DWORD /d 2 /f
    REG ADD "HKLM\BROKENSYSTEM\ControlSet002\Control\CrashControl" /v DumpFile /t REG_EXPAND_SZ /d "%SystemRoot%\MEMORY.DMP" /f
    REG ADD "HKLM\BROKENSYSTEM\ControlSet002\Control\CrashControl" /v NMICrashDump /t REG_DWORD /d 1 /f

    reg unload HKLM\BROKENSYSTEM
    ```

### <a name="replace-the-unsigned-drivers"></a>Reemplazo de los controladores no firmados

1. En la máquina virtual de recuperación, ejecute el siguiente comando en un símbolo del sistema con privilegios elevados. Este comando establece el disco del sistema operativo afectado para que inicie en modo seguro en el siguiente arranque:

    ```console
    bcdedit /store <OS DISK you attached>:\boot\bcd /set {default} safeboot minimal
    ```

    Por ejemplo, si el disco del sistema operativo conectado es la unidad F, ejecute el siguiente comando:

    ```console
    bcdedit /store F: boot\bcd /set {default} safeboot minimal
    ```

2. [Desconecte el disco del sistema operativo y, a continuación, vuelva a conectarlo a la máquina virtual afectada](troubleshoot-recovery-disks-portal-windows.md). La máquina virtual arrancará en modo seguro. Si el error no desaparece, vaya al siguiente paso opcional.
3. Abra el cuadro **Ejecutar** cuadro y ejecute el **Comprobador** para iniciar la herramienta Administrador del Comprobador de controladores.
4. Seleccione **Seleccionar automáticamente controladores no firmados** y, a continuación, haga clic en **Siguiente**.
5. Obtendrá la lista de los archivos de controlador sin firmar. Recuerde los nombres de archivo.
6. Copie las mismas versiones de estos archivos desde una máquina virtual en funcionamiento y, a continuación, reemplace estos archivos sin firmar. 

7. Elimine la configuración de arranque seguro:

    ```console
    bcdedit /store <OS DISK LETTER>:\boot\bcd /deletevalue {default} safeboot
    ```

8.  Reinicie la VM. 

### <a name="optional-analyze-the-dump-logs-in-dump-crash-mode"></a>Opcional: análisis de los registros de volcado de memoria en el modo de bloqueo de volcado de memoria

Para analizar los registros de volcado de memoria, siga estos pasos:

1. Conecte el disco del sistema operativo a una máquina virtual de recuperación.
2. En el disco del sistema operativo que ha asociado, vaya a **\windows\system32\config**. Realice una copia de seguridad de todos los archivos, por si fuera necesaria una reversión.
3. Inicie el **Editor del Registro** (regedit.exe).
4. Seleccione la clave **HKEY_LOCAL_MACHINE**. En el menú, seleccione **Archivo** > **Cargar subárbol**.
5. Vaya a la carpeta **\windows\system32\config\SYSTEM** en el disco del sistema operativo que ha asociado. Como nombre de la sección, escriba **BROKENSYSTEM**. El nuevo subárbol del Registro se muestra bajo la clave **HKEY_LOCAL_MACHINE**.
6. Vaya a **HKEY_LOCAL_MACHINE\BROKENSYSTEM\ControlSet00x\Control\CrashControl** y realice los cambios siguientes:

    AutoReboot = 0

    CrashDumpEnabled = 2
7.  Seleccione **BROKENSYSTEM**. En el menú, seleccione **Archivo** > **Descargar subárbol**.
8.  Modifique la configuración de BCD para arrancar en modo de depuración. Ejecute los siguientes comandos en un símbolo del sistema con privilegios elevados:

    ```cmd
    REM Setup some debugging flags on the boot manager
    bcdedit /store <OS DISK LETTER>:\boot\bcd /set {bootmgr} default {<IDENTIFIER OF THE WINDOWS BOOT LOADER>}
    bcdedit /store <OS DISK LETTER>:\boot\bcd /set {bootmgr} nointegritychecks on
    bcdedit /store <OS DISK LETTER>:\boot\bcd /set {bootmgr} integrityservices disable

    REM Setup some debugging flags on the boot loader
    bcdedit /store <OS DISK LETTER>:\boot\bcd /set {default} bootlog yes
    bcdedit /store <OS DISK LETTER>:\boot\bcd /set {default} bootstatuspolicy displayallfailures
    bcdedit /store <OS DISK LETTER>:\boot\bcd /set {default} nointegritychecks on
    bcdedit /store <OS DISK LETTER>:\boot\bcd /set {default} testsigning off
    bcdedit /store <OS DISK LETTER>:\boot\bcd /set {default} recoveryenabled no
    bcdedit /store <OS DISK LETTER>:\boot\bcd /set {default} integrityservices disable
    ```
9. [Desconecte el disco del sistema operativo y, a continuación, vuelva a conectarlo a la máquina virtual afectada](troubleshoot-recovery-disks-portal-windows.md).
10. Arranque la máquina virtual para ver si muestra el análisis de volcado de memoria. Busque el archivo que no se puede cargar. Debe reemplazar este archivo con un archivo de la máquina virtual en funcionamiento. 

    El siguiente es un ejemplo de análisis de volcado de memoria. Puede ver que el **ERROR** se encuentra en filecrypt.sys: "FAILURE_BUCKET_ID: 0x5A_c0000428_IMAGE_filecrypt.sys".

    ```
    kd> !analyze -v 

    ******************************************************************************* 
    * * * Bugcheck Analysis * * * 

    ******************************************************************************* 
    CRITICAL_SERVICE_FAILED (5a) Arguments: Arg1: 0000000000000001 Arg2: ffffd80f4bfe7070 Arg3: ffffb00b0513d320 Arg4: ffffffffc0000428 Debugging 
    Details: ------------------ 
    DUMP_CLASS: 1 DUMP_QUALIFIER: 401 BUILD_VERSION_STRING: 10.0.14393.1770 (rs1_release.170917-1700) 
    DUMP_TYPE: 1 BUGCHECK_P1: 1 BUGCHECK_P2: ffffd80f4bfe7070 BUGCHECK_P3: ffffb00b0513d320 BUGCHECK_P4: ffffffffc0000428 BUGCHECK_STR: 0x5A_c0000428 
    CPU_COUNT: 1 CPU_MHZ: a22 CPU_VENDOR: GenuineIntel CPU_FAMILY: 6 CPU_MODEL: 3d CPU_STEPPING: 4 DEFAULT_BUCKET_ID: WIN8_DRIVER_FAULT 
    PROCESS_NAME: System CURRENT_IRQL: 0 ANALYSIS_SESSION_HOST: MININT-6RMM091 ANALYSIS_SESSION_TIME: 11-15-2017 19:32:42.0841 
    ANALYSIS_VERSION: 10.0.16361.1001 amd64fre STACK_TEXT: ffffc701`1dc74948 fffff803`b2ff4b4a : 00000000`0000005a 00000000`00000001 ffffd80f`4bfe7070 ffffb00b`0513d320 : nt!KeBugCheckEx [d:\rs1\minkernel\ntos\ke\amd64\procstat.asm @ 127] ffffc701`1dc74950 fffff803`b3205df3 : ffffd80f`4bba9f58 ffffd80f`4bba9f58 ffffc701`1dc74b80 ffffd80f`00000006 : nt!IopLoadDriver+0x19f8e6 [d:\rs1\minkernel\ntos\ke\amd64\threadbg.asm @ 81] 
    RETRACER_ANALYSIS_TAG_STATUS: DEBUG_FLR_FAULTING_IP is not found THREAD_SHA1_HASH_MOD_FUNC: eb79608c07faa1af62c0e61f25ff6bc1d6dfdb25 THREAD_SHA1_HASH_MOD_FUNC_OFFSET: 96a3a314834bb4e8443a8b7201525fc5dfc1878b THREAD_SHA1_HASH_MOD: 30a3e915496deaace47137d5b90c3ecc03746bf6 FOLLOWUP_NAME: wintriag
    MODULE_NAME: filecrypt IMAGE_NAME: filecrypt.sys DEBUG_FLR_IMAGE_TIMESTAMP: 0 IMAGE_VERSION: STACK_COMMAND: .thread ; .cxr ; kb FAILURE_BUCKET_ID: 0x5A_c0000428_IMAGE_filecrypt.sys BUCKET_ID: 0x5A_c0000428_IMAGE_filecrypt.sys PRIMARY_PROBLEM_CLASS: 0x5A_c0000428_IMAGE_filecrypt.sys TARGET_TIME: 2017-11-13T20:51:04.000Z OSBUILD: 14393 OSSERVICEPACK: 1770 SERVICEPACK_NUMBER: 0 OS_REVISION: 0 SUITE_MASK: 144 PRODUCT_TYPE: 3 OSPLATFORM_TYPE: x64 OSNAME: Windows 10 OSEDITION: Windows 10 Server TerminalServer DataCenter OS_LOCALE: USER_LCID: 0 OSBUILD_TIMESTAMP: 2017-09-17 19:16:08 BUILDDATESTAMP_STR: 170917-1700 BUILDLAB_STR: rs1_release BUILDOSVER_STR: 10.0.14393.1770 ANALYSIS_SESSION_ELAPSED_TIME: bfc ANALYSIS_SOURCE: KM FAILURE_ID_HASH_STRING: km:0x5a_c0000428_image_filecrypt.sys FAILURE_ID_HASH: {35f25777-b01e-70a1-c502-f690dab6cb3a} FAILURE_ID_REPORT_LINK: https://go.microsoft.com/fwlink/?LinkID=397724&FailureHash=35f25777-b01e-70a1-c502-f690dab6cb3a
    ```

11. Una vez que la máquina virtual está trabajando y arranca con normalidad, elimine la configuración de volcado de memoria:

    ```cmd
    REM Restore the boot manager to default values
    bcdedit /store <OS DISK LETTER>:\boot\bcd /deletevalue {bootmgr} nointegritychecks
    bcdedit /store <OS DISK LETTER>:\boot\bcd /set {bootmgr} integrityservices enable

    REM Restore the boot loader to default values for an azure VM
    bcdedit /store <OS DISK LETTER>:\boot\bcd /deletevalue {default} bootlog
    bcdedit /store <OS DISK LETTER>:\boot\bcd /set {default} bootstatuspolicy ignoreallfailures
    bcdedit /store <OS DISK LETTER>:\boot\bcd /deletevalue {default} nointegritychecks
    bcdedit /store <OS DISK LETTER>:\boot\bcd /deletevalue {default} testsigning
    bcddit /store <OS DISK LETTER>:\boot\bcd /set {default} recoveryenabled no
    bcdedit /store <OS DISK LETTER>:\boot\bcd /set {default} integrityservicesenable
    ```

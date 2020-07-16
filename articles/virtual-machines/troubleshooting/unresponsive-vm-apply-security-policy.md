---
title: La máquina virtual de Azure no responde al aplicar una directiva de seguridad al sistema
description: En este artículo se proporcionan los pasos para resolver los problemas en los que la pantalla de carga se bloquea cuando la máquina virtual no responde al aplicar una directiva de seguridad al sistema en una máquina virtual de Azure.
services: virtual-machines-windows
documentationcenter: ''
author: TobyTu
manager: dcscontentpm
editor: ''
tags: azure-resource-manager
ms.assetid: a97393c3-351d-4324-867d-9329e31b5629
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.topic: troubleshooting
ms.date: 06/15/2020
ms.author: v-mibufo
ms.openlocfilehash: 6b50bffd1a44c0cf53f15650f5ff4d938f45df4d
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "84908066"
---
# <a name="azure-vm-is-unresponsive-while-applying-security-policy-to-the-system"></a>La máquina virtual de Azure no responde al aplicar una directiva de seguridad al sistema

En este artículo se proporcionan los pasos para resolver los problemas en los que el sistema operativo se bloquea y deja de responder mientras aplica una directiva de seguridad en una máquina virtual de Azure.

## <a name="symptoms"></a>Síntomas

Cuando use los [diagnósticos de arranque](boot-diagnostics.md) para ver la captura de pantalla de la máquina virtual, verá que dicha captura de pantalla muestra el sistema operativo bloqueado mientras arranca con el mensaje:

> "Aplicando la directiva de seguridad en el sistema".

:::image type="content" source="media/unresponsive-vm-apply-security-policy/apply-policy.png" alt-text="Captura de pantalla en la que la pantalla de inicio de Windows Server 2012 R2 está bloqueada.":::

:::image type="content" source="media/unresponsive-vm-apply-security-policy/apply-policy-2.png" alt-text="Captura de pantalla en la pantalla de inicio del sistema operativo está bloqueada.":::

## <a name="cause"></a>Causa

Son muchas las causas posibles de este problema. No podrá conocer el origen hasta que se realice un análisis del volcado de memoria.

## <a name="resolution"></a>Resolución

### <a name="process-overview"></a>Información general del proceso

1. [Creación de una VM de reparación y acceso a ella](#create-and-access-a-repair-vm)
2. [Habilitación de la consola serie y recopilación del volcado de memoria](#enable-serial-console-and-memory-dump-collection)
3. [Recompile la máquina virtual](#rebuild-the-vm).
4. [Recopilación de un archivo de volcado de memoria](#collect-the-memory-dump-file)

### <a name="create-and-access-a-repair-vm"></a>Creación de una VM de reparación y acceso a ella

1. Siga los [pasos 1 a 3 de los comandos de reparación de VM](repair-windows-vm-using-azure-virtual-machine-repair-commands.md#repair-process-example) para preparar una VM de reparación.
2. Use Conexión a Escritorio remoto para conectarse a la VM de reparación.

### <a name="enable-serial-console-and-memory-dump-collection"></a>Habilitación de la consola serie y recopilación del volcado de memoria

Para habilitar la consola serie y la recopilación de volcados de memoria, ejecute el siguiente script:

1. Abra una sesión de símbolo del sistema con privilegios elevados (Ejecutar como administrador).
2. Enumere los datos del almacén de BCD y determine el identificador del cargador de arranque, que usará en el paso siguiente.

     1. En el caso de una máquina virtual de la Generación 1, escriba el siguiente comando y anote el identificador que se muestra:

        ```console
        bcdedit /store <BOOT PARTITON>:\boot\bcd /enum
        ```

        En el comando, reemplace \<BOOT PARTITON> por la letra de la partición del disco conectado que contiene la carpeta de arranque.

        :::image type="content" source="media/unresponsive-vm-apply-security-policy/store-data.png" alt-text="Diagrama en el que aparece la salida de la lista del almacén de BCD en una máquina virtual de la Generación 1, que muestra el número de identificador bajo el cargador de arranque de Windows.":::

     2. En el caso de una máquina virtual de la Generación 2, escriba el siguiente comando y anote el identificador que se muestra:

        ```console
        bcdedit /store <LETTER OF THE EFI SYSTEM PARTITION>:EFI\Microsoft\boot\bcd /enum
        ```

        - En el comando, reemplace \<LETTER OF THE EFI SYSTEM PARTITION> por la letra de la partición del sistema EFI.
        - Puede ser útil iniciar la consola Administración de discos para identificar la partición del sistema adecuada etiquetada como "Partición de sistema EFI".
        - El identificador puede ser un GUID único o puede ser el "bootmgr" predeterminado.
3. Ejecute los siguientes comandos para habilitar la consola serie:

    ```console
    bcdedit /store <VOLUME LETTER WHERE THE BCD FOLDER IS>:\boot\bcd /ems {<BOOT LOADER IDENTIFIER>} ON
    ```

    ```console
    bcdedit /store <VOLUME LETTER WHERE THE BCD FOLDER IS>:\boot\bcd /emssettings EMSPORT:1 EMSBAUDRATE:115200
    ```

    - En el comando, reemplace \<VOLUME LETTER WHERE THE BCD FOLDER IS> por la letra de la carpeta de BCD.
    - En el comando, reemplace \<BOOT LOADER IDENTIFIER> por el identificador que encontró en el paso anterior.
4. Compruebe que el espacio disponible en el disco del sistema operativo sea mayor que el tamaño de memoria (RAM) de la VM.

    1. Si no hay espacio suficiente en el disco del sistema operativo, debe cambiar la ubicación donde se creará el archivo de volcado de memoria. En lugar de crear el archivo en el disco del sistema operativo, puede referirlo a cualquier otro disco de datos conectado a la máquina virtual que tenga suficiente espacio libre. Para cambiar la ubicación, reemplace "%SystemRoot%" por la letra de unidad (por ejemplo, "F:") del disco de datos en los comandos enumerados a continuación.
    2. Escriba los siguientes comandos (configuración de volcado recomendada):

        Cargar el disco del sistema operativo roto:

        ```console
        REG LOAD HKLM\BROKENSYSTEM <VOLUME LETTER OF BROKEN OS DISK>:\windows\system32\config\SYSTEM
        ```

        Habilitar en ControlSet001:

        ```console
        REG ADD "HKLM\BROKENSYSTEM\ControlSet001\Control\CrashControl" /v CrashDumpEnabled /t REG_DWORD /d 1 /f
        REG ADD "HKLM\BROKENSYSTEM\ControlSet001\Control\CrashControl" /v DumpFile /t REG_EXPAND_SZ /d "%SystemRoot%\MEMORY.DMP" /f
        REG ADD "HKLM\BROKENSYSTEM\ControlSet001\Control\CrashControl" /v NMICrashDump /t REG_DWORD /d 1 /f
        ```

        Habilitar en ControlSet002:

        ```console
        REG ADD "HKLM\BROKENSYSTEM\ControlSet002\Control\CrashControl" /v CrashDumpEnabled /t REG_DWORD /d 1 /f
        REG ADD "HKLM\BROKENSYSTEM\ControlSet002\Control\CrashControl" /v DumpFile /t REG_EXPAND_SZ /d "%SystemRoot%\MEMORY.DMP" /f
        REG ADD "HKLM\BROKENSYSTEM\ControlSet002\Control\CrashControl" /v NMICrashDump /t REG_DWORD /d 1 /f
        ```

        Descargar disco del sistema operativo roto:

        ```console
        REG UNLOAD HKLM\BROKENSYSTEM
        ```

### <a name="rebuild-the-vm"></a>Recompilación de la máquina virtual

Siga el [paso 5 de los comandos de reparación de la VM](repair-windows-vm-using-azure-virtual-machine-repair-commands.md#repair-process-example) para volver a ensamblar la VM.

### <a name="collect-the-memory-dump-file"></a>Recopilación de un archivo de volcado de memoria

Para resolver este problema, primero debe recopilar el archivo de volcado de memoria del bloqueo y ponerse en contacto con el soporte técnico en relación a dicho archivo. Para recopilar el archivo de volcado de memoria, siga estos pasos:

1. Conecte el disco del sistema operativo a una nueva VM de recuperación:

    - Siga los pasos 1 a 3 de los [comandos de reparación de VM](repair-windows-vm-using-azure-virtual-machine-repair-commands.md#repair-process-example) para preparar una VM de reparación.
    - Use Conexión a Escritorio remoto para conectarse a la VM de reparación.

2. Busque el archivo de volcado de memoria y envíe una incidencia de soporte técnico:

    - En la VM de reparación, vaya a la carpeta de Windows en el disco del sistema operativo conectado. Si la letra de unidad que se asigna al disco del sistema operativo conectado es `F`, debería ir a `F:\Windows`.
    - Busque el archivo memory.dmp y, luego, [envíe una incidencia de soporte técnico](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) con el archivo de volcado de memoria.
    - Si tiene problemas para encontrar el archivo memory.dmp, puede que quiera usar en su lugar [llamadas de interrupción no enmascarable (NMI) en la consola serie](serial-console-windows.md#use-the-serial-console-for-nmi-calls). Puede seguir la guía para [generar un archivo de volcado de memoria mediante llamadas NMI](/windows/client-management/generate-kernel-or-complete-crash-dump).

## <a name="next-steps"></a>Pasos siguientes

Si tiene problemas al aplicar la directiva Usuarios y grupos locales, consulte [La máquina virtual no responde al aplicar la directiva de usuarios y grupos locales de directiva de grupo](unresponsive-vm-apply-group-policy.md).
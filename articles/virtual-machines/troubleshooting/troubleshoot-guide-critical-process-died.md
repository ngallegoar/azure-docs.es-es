---
title: Error de parada de Windows
description: ''
services: virtual-machines-windows
documentationcenter: ''
author: v-miegge
manager: dcscontentpm
editor: ''
tags: azure-resource-manager
ms.assetid: 5b3ed56a-5a11-4ff9-9ee8-76aea4a5689b
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.topic: troubleshooting
ms.date: 03/26/2020
ms.author: v-mibufo
ms.openlocfilehash: c04f3b27c7214dcf821c7698796bfaea399b947d
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "86509110"
---
# <a name="windows-stop-error---0x000000ef-critical-process-died"></a>Error de parada de Windows: #0x000000EF; proceso crítico interrumpido

En este artículo se proporcionan los pasos para resolver los problemas en los que un proceso crítico se interrumpe durante el arranque en una VM Azure.

## <a name="symptom"></a>Síntoma

Si usa [diagnósticos de arranque](./boot-diagnostics.md) para ver la captura de pantalla de la VM, verá que esta muestra el error *#0x000000EF*con un mensaje que indica que *se interrumpió un proceso crítico*.

!["Se produjo un problema con su equipo y se debe reiniciar. Solo vamos a recopilar alguna información de error y, a continuación, podrá reiniciar. (## % completado) Si quiere más información, puede buscar más tarde en línea este error: 0x000000EF"](media/troubleshoot-guide-critical-process-died/1.jpg)

## <a name="cause"></a>Causa

Normalmente, se debe a un error en el proceso del sistema crítico durante el arranque. Puede obtener más información sobre los problemas de proceso críticos en "[Comprobación de error 0xEF: CRITICAL_PROCESS_DIED](/windows-hardware/drivers/debugger/bug-check-0xef--critical-process-died)".

## <a name="solution"></a>Solución

### <a name="process-overview"></a>Información general del proceso:

1. Cree una VM de reparación y acceda a ella.
2. Corrija los daños en el sistema operativo.
3. **Recomendado**: Antes de volver a compilar la VM, habilite Serial Console y la recopilación de volcado de memoria.
4. Vuelva a compilar la VM.

> [!NOTE]
> Este error de arranque indica que el sistema operativo invitado no está operativo. Ahora solucionará el problema en el modo sin conexión.

### <a name="create-and-access-a-repair-vm"></a>Creación de una VM de reparación y acceso a ella

1. Siga los [pasos 1 a 3 de los comandos de reparación de VM](./repair-windows-vm-using-azure-virtual-machine-repair-commands.md) para preparar una VM de reparación.
2. Use Conexión a Escritorio remoto para conectarse a la VM de reparación.

### <a name="fix-any-os-corruption"></a>Corrección de daños en el sistema operativo

1. Abra un símbolo del sistema con privilegios elevados.
2. Ejecute el siguiente comando del Comprobador de archivos de sistema (SFC):

   `sfc /scannow /offbootdir=<BOOT DISK DRIVE>:\ /offwindir=<BROKEN DISK DRIVE>:\windows`

   * Donde <BOOT DISK DRIVE> es el volumen de arranque de la VM de reparación (normalmente, "C:") y <BROKEN DISK DRIVE> será la letra de unidad del disco conectado de la VM interrumpida. Sustituya los símbolos mayor/menor que, así como el texto que contienen (p. ej., "<este texto>"), con la letra correspondiente.

3. A continuación, siga el [paso 5 de los comandos de reparación de la VM](./repair-windows-vm-using-azure-virtual-machine-repair-commands.md#repair-process-example) para volver a ensamblar la VM y comprobar si arranca.
4. Si la VM sigue sin arrancar, continúe con la recopilación del archivo de volcado de memoria.

### <a name="collect-the-memory-dump-file"></a>Recopilación de un archivo de volcado de memoria

Si el problema persiste después de ejecutar SFC, se necesitará el análisis de un archivo de volcado de memoria para determinar la causa del problema. Para recopilar el archivo de volcado de memoria, siga estos pasos:

### <a name="attach-the-os-disk-to-a-new-repair-vm"></a>Conecte el disco del sistema operativo a una nueva VM de recuperación

1. Siga los pasos 1 a 3 de los [comandos de reparación de VM](./repair-windows-vm-using-azure-virtual-machine-repair-commands.md) para preparar una VM de reparación.
2. Use Conexión a Escritorio remoto para conectarse a la VM de reparación.

### <a name="locate-the-dump-file-and-submit-a-support-ticket"></a>Busque el archivo de volcado de memoria y envíe una incidencia de soporte técnico

3. En la VM de reparación, vaya a la carpeta de Windows en el disco del sistema operativo conectado. Si la letra de unidad que se asigna al disco del sistema operativo conectado es *F*, deberá ir a *F:\Windows*.
4. Busque el archivo *memory.dmp* y, a continuación, [envíe una incidencia de soporte técnico](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) con el archivo de volcado de memoria.

   > [!NOTE]
   > Si no encuentra el archivo de volcado, complete los pasos siguientes para habilitar la recopilación de volcado de memoria y Serial Console, vuelva a esta sección y repita los pasos descritos en la tarea anterior para recopilar el archivo de volcado de memoria.

### <a name="recommended-before-you-rebuild-the-vm-enable-serial-console-and-memory-dump-collection"></a>Se recomienda: Antes de volver a compilar la VM, habilite Serial Console y la recopilación del volcado de memoria.

Para habilitar la recopilación de volcado de memoria y Serial Console, ejecute el siguiente script:

1. Abra una sesión de símbolo del sistema con privilegios elevados (Ejecutar como administrador).
2. Ejecute los comandos siguientes:

   Habilitación de Serial Console

   `bcdedit /store <VOLUME LETTER WHERE THE BCD FOLDER IS>:\boot\bcd /ems {<BOOT LOADER IDENTIFIER>} ON`

   `bcdedit /store <VOLUME LETTER WHERE THE BCD FOLDER IS>:\boot\bcd /emssettings EMSPORT:1 EMSBAUDRATE:115200`

   Reemplace los símbolos mayor que o menor que, así como el texto que contienen; por ejemplo, "<este texto>".

3. Verifique que el espacio disponible en el disco del sistema operativo sea igual al tamaño de memoria (RAM) de la VM.

Si no hay suficiente espacio en el disco del sistema operativo, debe cambiar la ubicación donde se creará el archivo de volcado de memoria y remitirlo a los discos de datos conectados a la VM que tengan espacio libre suficiente. Para cambiar la ubicación, reemplace "%SystemRoot%" por la letra de unidad (por ejemplo, "F:") del disco de datos en los comandos siguientes.

#### <a name="suggested-configuration-to-enable-os-dump"></a>Configuración sugerida para habilitar el volcado del sistema operativo

**Cargar disco del sistema operativo roto**:

`REG LOAD HKLM\BROKENSYSTEM <VOLUME LETTER OF BROKEN OS DISK>:\windows\system32\config\SYSTEM`

**Habilitar en ControlSet001:**

`REG ADD "HKLM\BROKENSYSTEM\ControlSet001\Control\CrashControl" /v CrashDumpEnabled /t REG_DWORD /d 1 /f`

`REG ADD "HKLM\BROKENSYSTEM\ControlSet001\Control\CrashControl" /v DumpFile /t REG_EXPAND_SZ /d "%SystemRoot%\MEMORY.DMP" /f`

`REG ADD "HKLM\BROKENSYSTEM\ControlSet001\Control\CrashControl" /v NMICrashDump /t REG_DWORD /d 1 /f`

**Habilitar en ControlSet002:**

`REG ADD "HKLM\BROKENSYSTEM\ControlSet002\Control\CrashControl" /v CrashDumpEnabled /t REG_DWORD /d 1 /f`

`REG ADD "HKLM\BROKENSYSTEM\ControlSet002\Control\CrashControl" /v DumpFile /t REG_EXPAND_SZ /d "%SystemRoot%\MEMORY.DMP" /f`

`REG ADD "HKLM\BROKENSYSTEM\ControlSet002\Control\CrashControl" /v NMICrashDump /t REG_DWORD /d 1 /f`

**Descargar disco del sistema operativo roto:**

`REG UNLOAD HKLM\BROKENSYSTEM`

### <a name="rebuild-the-original-vm"></a>Recompilación de la VM original

Siga el [paso 5 de los comandos de reparación de la VM](./repair-windows-vm-using-azure-virtual-machine-repair-commands.md#repair-process-example) para volver a ensamblar la VM.

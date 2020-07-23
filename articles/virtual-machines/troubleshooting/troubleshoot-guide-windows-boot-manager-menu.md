---
title: La máquina virtual Windows no puede arrancar debido a Administración de arranque de Windows
description: En este artículo se proporcionan los pasos para resolver problemas en los que Administración de arranque de Windows impide el arranque de una máquina virtual de Azure.
services: virtual-machines-windows
documentationcenter: ''
author: v-miegge
manager: dcscontentpm
editor: ''
tags: azure-resource-manager
ms.assetid: a97393c3-351d-4324-867d-9329e31b3598
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.topic: troubleshooting
ms.date: 03/26/2020
ms.author: v-mibufo
ms.openlocfilehash: 2457952051f575306de46e3e8145cc26678a1ef8
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/20/2020
ms.locfileid: "86526547"
---
# <a name="windows-vm-cannot-boot-due-to-windows-boot-manager"></a>La VM Windows no puede arrancar debido a Administración de arranque de Windows

En este artículo se proporcionan los pasos para resolver problemas en los que Administración de arranque de Windows impide el arranque de una máquina virtual (VM) de Azure.

## <a name="symptom"></a>Síntoma

La VM está detenida en espera de un mensaje de usuario y no arranca a menos que se le indique de forma manual.

Cuando use [Diagnósticos de arranque](./boot-diagnostics.md) para ver la captura de pantalla de la VM, verá que en la captura de pantalla se muestra Administración de arranque de Windows con el mensaje *Elija un sistema operativo que desee iniciar o presione la tecla Tabulador para seleccionar una herramienta:* .

En la Ilustración 1
 
![Administración de arranque de Windows que indica "Elija un sistema operativo que desee iniciar o presione la tecla Tabulador para seleccionar una herramienta:"](media/troubleshoot-guide-windows-boot-manager-menu/1.jpg)

## <a name="cause"></a>Causa

El error se debe a una marca de BCD *displaybootmenu* en Administración de arranque de Windows. Cuando la marca está habilitada, Administración de arranque de Windows solicita al usuario, durante el proceso de arranque, que seleccione el cargador que desea ejecutar, lo que produce un retraso en el arranque. En Azure, esta característica puede aumentar el tiempo necesario para arrancar una VM.

## <a name="solution"></a>Solución

Información general del proceso:

1. Configure para acelerar el tiempo de arranque mediante Serial Console.
2. Cree una VM de reparación y acceda a ella.
3. Configure para acelerar el tiempo de arranque en una VM de reparación.
4. **Recomendado**: Antes de volver a compilar la VM, habilite Serial Console y la recopilación de volcado de memoria.
5. Vuelva a compilar la VM.

### <a name="configure-for-faster-boot-time-using-serial-console"></a>Configuración para acelerar el tiempo de arranque mediante Serial Console

Si tiene acceso a Serial Console, hay dos formas de lograr tiempos de arranque más rápidos. Puede reducir el tiempo de espera de *displaybootmenu*, o bien quitar la marca por completo.

1. Siga las instrucciones para acceder a la [Consola serie de Azure para Windows](./serial-console-windows.md) para obtener acceso a la consola basada en texto.

   > [!NOTE]
   > Si no puede acceder a Serial Console, vaya a [Creación y acceso a una VM de reparación](#create-and-access-a-repair-vm).

2. **Opción A**: Reducir el tiempo de espera

   a. El tiempo de espera se establece en 30 segundos de forma predeterminada, pero se puede cambiar a un tiempo menor (por ejemplo, 5 segundos).

   b. Use el siguiente comando en Serial Console para ajustar el valor de tiempo de espera:

      `bcdedit /set {bootmgr} timeout 5`

3. **Opción B**: Quitar la marca BCD

   a. Para evitar el mensaje de Mostrar menú de arranque por completo, escriba el siguiente comando:

      `bcdedit /deletevalue {bootmgr} displaybootmenu`

      > [!NOTE]
      > Si no puede usar Serial Console para configurar un menor tiempo de arranque en los pasos anteriores, en su lugar puede continuar con los pasos siguientes. Ahora solucionará el problema en el modo sin conexión.

### <a name="create-and-access-a-repair-vm"></a>Creación y acceso a una VM de reparación

1. Siga los [pasos 1 a 3 de los comandos de reparación de VM](./repair-windows-vm-using-azure-virtual-machine-repair-commands.md) para preparar una VM de reparación.
2. Use Conexión a Escritorio remoto para conectarse a la VM de reparación.

### <a name="configure-for-faster-boot-time-on-a-repair-vm"></a>Configuración para acelerar el tiempo de arranque en una VM de reparación

1. Abra un símbolo del sistema con privilegios elevados.
2. Escriba lo siguiente para habilitar DisplayBootMenu:

   Use este comando para **VM de generación 1**:

   `bcdedit /store <VOLUME LETTER WHERE THE BCD FOLDER IS>:\boot\bcd /set {bootmgr} displaybootmenu yes`

   Use este comando para **VM de generación 2**:

   `bcdedit /store <VOLUME LETTER OF EFI SYSTEM PARTITION>:EFI\Microsoft\boot\bcd /set {bootmgr} displaybootmenu yes`

   Reemplace los símbolos mayor que o menor que, así como el texto que contienen, por ejemplo, "< texto aquí >".

3. Cambie el valor de tiempo de expiración a 5 segundos:

   Use este comando para **VM de generación 1**:

   `bcdedit /store <VOLUME LETTER WHERE THE BCD FOLDER IS>:\boot\bcd /set {bootmgr} timeout 5`

   Use este comando para **VM de generación 2**:

   `bcdedit /store <VOLUME LETTER OF EFI SYSTEM PARTITION>:EFI\Microsoft\boot\bcd /set {bootmgr} timeout 5`

   Reemplace los símbolos mayor que o menor que, así como el texto que contienen, por ejemplo, "< texto aquí >".

### <a name="recommended-before-you-rebuild-the-vm-enable-serial-console-and-memory-dump-collection"></a>Se recomienda: Antes de volver a compilar la VM, habilite Serial Console y la recopilación de volcado de memoria.

Para habilitar la recopilación de volcado de memoria y Serial Console, ejecute el siguiente script:

1. Abra una sesión de símbolo del sistema con privilegios elevados (Ejecutar como administrador).
2. Ejecute los comandos siguientes:

   Habilitación de Serial Console

   `bcdedit /store <VOLUME LETTER WHERE THE BCD FOLDER IS>:\boot\bcd /ems {<BOOT LOADER IDENTIFIER>} ON`

   `bcdedit /store <VOLUME LETTER WHERE THE BCD FOLDER IS>:\boot\bcd /emssettings EMSPORT:1 EMSBAUDRATE:115200`

   Reemplace los símbolos mayor que o menor que, así como el texto que contienen, por ejemplo, "< texto aquí >".

3. Verifique que el espacio disponible en el disco del sistema operativo sea igual al tamaño de memoria (RAM) de la VM.

   Si no hay suficiente espacio en el disco del sistema operativo, debe cambiar la ubicación donde se creará el archivo de volcado de memoria y remitirlo a los discos de datos conectados a la VM que tengan espacio libre suficiente. Para cambiar la ubicación, reemplace "% SystemRoot%" por la letra de unidad (por ejemplo, "F:") del disco de datos en los comandos siguientes.

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

---
title: 'Error de arranque: "This is not a bootable disk" (Este disco no es de arranque)'
description: En este artículo se proporcionan los pasos para resolver problemas en que el disco de una máquina virtual de Azure no es de arranque.
services: virtual-machines-windows
documentationcenter: ''
author: v-miegge
manager: dcscontentpm
editor: ''
tags: azure-resource-manager
ms.assetid: 5d6db4e3-c2f5-40c7-afea-54edf745f5eb
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.topic: troubleshooting
ms.date: 03/25/2020
ms.author: v-mibufo
ms.openlocfilehash: 16f6919577955bda5b04db26deb9fe78a467e364
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/20/2020
ms.locfileid: "86509042"
---
# <a name="boot-error--this-is-not-a-bootable-disk"></a>Error de arranque: This is not a bootable disk (Este disco no es de arranque)

En este artículo se proporcionan los pasos para resolver problemas en que el disco de una máquina virtual (VM) de Azure no es de arranque.

## <a name="symptoms"></a>Síntomas

Cuando use los [diagnósticos de arranque](./boot-diagnostics.md) para ver la captura de pantalla de la VM, verá que esta captura de pantalla muestra un mensaje que indica que el disco no es de arranque y le solicita que inserte un disco y presione cualquier tecla para volverlo a intentar....

   En la Ilustración 1

   ![En la figura 1 se muestra el mensaje *"This is not a bootable disk. Please insert a bootable floppy and press any key to try again..."* (Este disco no es de arranque. Inserte un disco de arranque y presione cualquier tecla para volver a intentarlo).](media/troubleshoot-guide-not-bootable-disk/1.jpg)

## <a name="cause"></a>Causa

Este mensaje de error significa que el proceso de arranque del sistema operativo no pudo encontrar ninguna partición del sistema activa. Este error también puede significar que falta una referencia en el almacén de datos de la configuración de arranque (BCD), lo que le impide encontrar la partición de Windows.

## <a name="solution"></a>Solución

### <a name="process-overview"></a>Información general del proceso

1. Cree una VM de reparación y acceda a ella.
2. Establezca el estado de la partición en Activo.
3. Corrija la partición del disco.
4. **Recomendado**: Antes de volver a compilar la VM, habilite Serial Console y la recopilación del volcado de memoria.
5. Vuelva a compilar la VM original.

   > [!NOTE]
   > Este error de arranque indica que el sistema operativo invitado no está operativo. Ahora solucionará el problema en el modo sin conexión.

### <a name="create-and-access-a-repair-vm"></a>Creación de una VM de reparación y acceso a ella

1. Siga los pasos 1 a 3 de los [comandos de reparación de VM](./repair-windows-vm-using-azure-virtual-machine-repair-commands.md) para preparar una VM de reparación.
2. Use Conexión a Escritorio remoto para conectarse a la VM de reparación.

### <a name="set-partition-status-to-active"></a>Establecer el estado de la partición en Activo

Las VM de generación 1 deben comprobar primero que la partición del sistema operativo, que contiene el almacén BCD, está marcada como *activa*. Si tiene una VM de generación 2, vaya a [Corrección de la partición de disco](#fix-the-disk-partition), ya que la marca *Estado* quedó en desuso en la generación posterior.

1. Abra un símbolo del sistema con privilegios elevados *(cmd.exe)* .
2. Escriba *diskpart* para iniciar la herramienta DISKPART.
3. Escriba *list disk* para enumerar los discos del sistema e identificar el VHD del sistema operativo conectado.
4. Cuando encuentre el VHD del sistema operativo conectado, escriba *sel disk #* para seleccionarlo.  Consulte la figura 2, donde el disco 1 es el VHD del sistema operativo conectado.

   Figura 2

   ![En la figura 2 se muestra la ventana *DISKPART*, que muestra la salida del comando list disk: el disco 0 y el disco 1 que se muestran en la tabla.  También muestra la salida del comando sel disk 1. Disk 1 es el disco seleccionado.](media/troubleshoot-guide-not-bootable-disk/2.jpg)

5. Una vez seleccionado el disco, escriba *list partition* para mostrar las particiones del disco seleccionado.
6. Una vez identificada la partición de arranque, escriba *sel partition #* para seleccionar la partición.  Normalmente, la partición de arranque tendrá un tamaño de, aproximadamente, 350 MB.  Consulte la figura 3, donde Partition 1 es la partición de arranque.

   Figura 3

   ![En la figura 3 se muestra la ventana *DISKPART* con la salida del comando *list partition*. Las particiones 1 y 2 se muestran en la tabla. También se muestra la salida del comando *sel partition 1*, donde Partition 1 es el disco seleccionado.](media/troubleshoot-guide-not-bootable-disk/3.jpg)

7. Escriba "detail partition" para comprobar el estado de la partición. Consulte la figura 4, donde la partición está definida como *Active: No* o la figura 5, donde está definida como "Active: Yes".

   Figura 4

   ![En la figura 4 se muestra la ventana *DISKPART* con la salida del comando *detail partition*, cuando la partición 1 está definida como *Active: No*](media/troubleshoot-guide-not-bootable-disk/4.jpg)

   Figura 5

   ![En la figura 5 se muestra la ventana *DISKPART* con la salida del comando *detail partition*, cuando la partición 1 está definida como *Active:  Yes*.](media/troubleshoot-guide-not-bootable-disk/5.jpg)

8. Si la partición **no está activa**, escriba *active* para cambiar la marca *Active*.
9. Para comprobar que el cambio de estado se haya realizado correctamente, escriba *detail partition*.

   Ilustración 6.

   ![En la figura 6 se muestra la ventana diskpart con la salida del comando *detail partition*, cuando la partición 1 está definida como *Active: Sí*](media/troubleshoot-guide-not-bootable-disk/6.jpg)

10. Escriba *exit* para cerrar la herramienta DISKPART y guardar los cambios de configuración.

### <a name="fix-the-disk-partition"></a>Corrección de la partición de disco

1. Abra un símbolo del sistema con privilegios elevados (cmd.exe).
2. Use el comando siguiente para ejecutar *CHKDSK* en los discos y corregir los errores:

   `chkdsk <DRIVE LETTER>: /f`

   Al agregar la opción de comando "/f", se corregirán los errores en el disco. Asegúrese de reemplazar <DRIVE LETTER> por la letra del VHD del sistema operativo conectado.

### <a name="recommended-before-you-rebuild-the-vm-enable-serial-console-and-memory-dump-collection"></a>Se recomienda: Antes de volver a compilar la VM, habilite Serial Console y la recopilación del volcado de memoria.

Para habilitar la recopilación de volcado de memoria y Serial Console, ejecute el siguiente script:

1. Abra una sesión de símbolo del sistema con privilegios elevados (Ejecutar como administrador).
2. Ejecute los comandos siguientes:

   Habilitación de Serial Console

   `bcdedit /store <VOLUME LETTER WHERE THE BCD FOLDER IS>:\boot\bcd /ems {<BOOT LOADER IDENTIFIER>} ON`

   `bcdedit /store <VOLUME LETTER WHERE THE BCD FOLDER IS>:\boot\bcd /emssettings EMSPORT:1 EMSBAUDRATE:115200`

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

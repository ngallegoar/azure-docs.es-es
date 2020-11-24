---
title: 'Error de detención de Windows: error de funcionamiento del hardware'
description: En este artículo se proporcionan los pasos para resolver problemas en los que las máquinas virtuales de Windows Server 2008 se bloquean con un mensaje de error que indica que se ha producido un error de hardware.
services: virtual-machines-windows
documentationcenter: ''
author: mibufo
editor: ''
tags: azure-resource-manager
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.topic: troubleshooting
ms.date: 11/13/2020
ms.author: v-mibufo
ms.openlocfilehash: eb4e0a246d6a33c3fad5f44b99a37997e4462f05
ms.sourcegitcommit: 8e7316bd4c4991de62ea485adca30065e5b86c67
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/17/2020
ms.locfileid: "94663380"
---
# <a name="windows-stop-error---hardware-malfunction"></a>Error de detención de Windows: error de funcionamiento del hardware

En este artículo se proporcionan los pasos para resolver problemas en los que las máquinas virtuales de Windows Server 2008 se bloquean con un mensaje de error que indica que se ha producido un error de hardware.

## <a name="symptoms"></a>Síntomas

Cuando use los [diagnósticos de arranque](./boot-diagnostics.md) para ver la captura de pantalla de la máquina virtual, verá que esta captura de pantalla muestra una pantalla azul con el mensaje siguiente:

**\*\*\* Error de funcionamiento del hardware**

**Call your vendor for support** (Llame al proveedor para obtener soporte técnico)

**\*\*\* The system has halted \*\*\*** (El sistema se ha detenido)

#### <a name="blue-screen"></a>Pantalla azul

![En la captura de pantalla se muestra una pantalla azul de bloqueo de funcionamiento defectuoso de hardware.](media/windows-stop-error-hardware-malfunction/windows-stop-error-hardware-malfunction-1.png)

#### <a name="serial-console"></a>Consola de serie

![En la captura de pantalla se muestra el mensaje "Error de funcionamiento del hardware" de la característica Consola de serie, si se ha habilitado.](media/windows-stop-error-hardware-malfunction/windows-stop-error-hardware-malfunction-2.png)

## <a name="cause"></a>Causa

Esta pantalla aparecerá cuando el sistema operativo invitado no se haya configurado correctamente y se haya enviado una Interrupción no enmascarable (NMI). El mensaje de error indica que un programa en modo kernel generó una excepción, que el controlador no detectó. Puede identificar qué excepción se generó recopilando un volcado de memoria.

## <a name="solution"></a>Solución

### <a name="process-overview"></a>Información general del proceso 

1. Configuración de la clave del registro de interrupción no enmascarable (NMI) 
2. Creación de una VM de reparación y acceso a ella 
3. Habilitación de la consola serie y recopilación del volcado de memoria 
4. Recompilación de la máquina virtual 

### <a name="set-up-the-non-maskable-interrupt-nmi-registry-key"></a>Configuración de la clave del registro de interrupción no enmascarable (NMI)

1. Con Azure Portal, reinicie la máquina virtual para que el sistema operativo invitado se arranque con normalidad. 
2. Una vez que se haya restaurado el acceso a la máquina virtual, abra un símbolo del sistema con privilegios elevados (ejecutar como administrador). 
3. Configure la clave del registro de NMI con el siguiente comando:

    ```
    REG ADD "HKLM\SYSTEM\CurrentControlSet\Control\CrashControl" /v NMICrashDump /t REG_DWORD /d 1 /f
    ```
    [Visualización de más información sobre el comando REG ADD](https://docs.microsoft.com/windows-server/administration/windows-commands/reg-add)
4. *(Opcional)* Configure la recopilación de volcado de memoria:

    ```
    REG ADD "HKLM\SYSTEM\CurrentControlSet\Control\CrashControl" /v CrashDumpEnabled /t REG_DWORD /d 1 /f  
    REG ADD "HKLM\SYSTEM\CurrentControlSet\Control\CrashControl" /v DumpFile /t REG_EXPAND_SZ /d "%SystemRoot%\MEMORY.DMP" /f 

    ```
5. *(Opcional)* Configure el acceso a la consola serie:

    ```
    BCDEDIT /ems {current} on, or bcdedit /ems '{current}' on if you are using PowerShell
    BCDEDIT /emssettings EMSPORT:1 EMSBAUDRATE:115200 
    ```
    [Visualización de más información sobre el comando BCDEDIT](https://docs.microsoft.com/windows-server/administration/windows-commands/bcdedit)
6. Reinicie la máquina virtual con el comando siguiente:

    ```
    SHUTDOWN /r /t 0 /f 
    ```
    [Visualización de más información sobre el comando SHUTDOWN](https://docs.microsoft.com/windows-server/administration/windows-commands/shutdown)

> [!IMPORTANT]
> Ahora se debería haber solucionado ya el problema.

> [!NOTE]
> Después de reiniciar, pruebe la máquina virtual para asegurarse de que funciona de la manera habitual. Si sigue teniendo problemas, puede continuar con la siguiente sección para obtener más instrucciones.

> [!TIP]
> Se recomienda configurar la clave del registro de interrupción no enmascarable (NMI) en la sección anterior, pero si la máquina virtual no se inicia normalmente después, es posible que no se hayan realizado los cambios previstos en el registro del SO invitado. En ese caso, puede seguir las instrucciones que se indican a continuación para agregar manualmente la configuración del registro.

### <a name="create-and-access-a-repair-vm"></a>Creación de una máquina virtual de reparación y acceso a ella

1. Siga los pasos 1 a 3 de los [comandos de reparación de VM](./repair-windows-vm-using-azure-virtual-machine-repair-commands.md) para preparar una VM de reparación.
2. Conéctese a la máquina virtual de reparación mediante Conexión a Escritorio remoto.

### <a name="enable-serial-console-and-memory-dump-collection"></a>Habilitación de la consola serie y recopilación de volcados de memoria

Antes de volver a compilar la máquina virtual, se recomienda habilitar la recopilación de volcado de memoria y la consola serie. Para ello, ejecute el siguiente script: 

1. Abra una sesión de símbolo del sistema con privilegios elevados (Ejecutar como administrador). 
2. Muestre los datos del almacén BCD y determine el identificador del cargador de arranque, que va a usar en el paso siguiente. 
    1. En el caso de una máquina virtual de generación 1, escriba el siguiente comando y anote el identificador que se muestra: 
 
        ```
        bcdedit /store <BOOT PARTITON>:\boot\bcd /enum
        ```
        En el comando, reemplace `<BOOT PARTITON>` por la letra de la partición del disco conectado que contiene la carpeta de arranque. 

        ![En la captura de pantalla se muestra la salida de la lista del almacén de BCD en una máquina virtual de la Generación 1, que muestra el número de identificador bajo el cargador de arranque de Windows.](media/windows-stop-error-hardware-malfunction/windows-stop-error-hardware-malfunction-3.png)
    2. En el caso de una máquina virtual de generación 2, escriba el siguiente comando y anote el identificador que se muestra:
    
        ```
        BCDEDIT /store <LETTER OF THE EFI SYSTEM PARTITION>:EFI\Microsoft\boot\bcd /enum 
        ```
        * En el comando, reemplace `<LETTER OF THE EFI SYSTEM PARTITION>` por la letra de la partición de sistema EFI.
        * Puede resultar útil iniciar la consola de Administración de discos para identificar la partición del sistema adecuada etiquetada como *Partición de sistema EFI*.
        * El identificador puede ser un GUID único o puede ser el valor predeterminado *bootmgr*.
3. Ejecute los siguientes comandos para habilitar Serial Console:

    ```
    BCDEDIT /store <VOLUME LETTER WHERE THE BCD FOLDER IS>:\boot\bcd /ems {<BOOT LOADER IDENTIFIER>} ON  
    BCDEDIT /store <VOLUME LETTER WHERE THE BCD FOLDER IS>:\boot\bcd /emssettings EMSPORT:1 EMSBAUDRATE:115200 

    ```
    * En el comando, reemplace `<VOLUME LETTER WHERE THE BCD FOLDER IS>` por la letra de la carpeta BCD.
    * En el comando, reemplace `<BOOT LOADER IDENTIFIER>` por el identificador que encontró en el paso anterior.
4. Compruebe que el espacio disponible en el disco del sistema operativo sea mayor que el tamaño de memoria (RAM) de la VM. 
    1. Si no hay espacio suficiente en el disco del sistema operativo, debe cambiar la ubicación donde se creará el archivo de volcado de memoria. En lugar de crear el archivo en el disco del sistema operativo, puede referirlo a cualquier otro disco de datos conectado a la máquina virtual que tenga suficiente espacio libre. Para cambiar la ubicación, reemplace **%SystemRoot%** por la letra de unidad (por ejemplo, **F:** ) del disco de datos en los comandos enumerados a continuación. 
    2. Escriba los siguientes comandos (configuración de volcado recomendada):

    **Cargue el subárbol del Registro desde el disco del sistema operativo roto:**

    ```
    REG LOAD HKLM\BROKENSYSTEM <VOLUME LETTER OF BROKEN OS DISK>:\windows\system32\config\SYSTEM
    ```

    **Habilitar en ControlSet001:**

    ```
    REG ADD "HKLM\BROKENSYSTEM\ControlSet001\Control\CrashControl" /v CrashDumpEnabled /t REG_DWORD /d 1 /f 
    REG ADD "HKLM\BROKENSYSTEM\ControlSet001\Control\CrashControl" /v DumpFile /t REG_EXPAND_SZ /d "%SystemRoot%\MEMORY.DMP" /f 
    REG ADD "HKLM\BROKENSYSTEM\ControlSet001\Control\CrashControl" /v NMICrashDump /t REG_DWORD /d 1 /f 
    ```

    **Habilitar en ControlSet002:**

    ```
    REG ADD "HKLM\BROKENSYSTEM\ControlSet002\Control\CrashControl" /v CrashDumpEnabled /t REG_DWORD /d 1 /f 
    REG ADD "HKLM\BROKENSYSTEM\ControlSet002\Control\CrashControl" /v DumpFile /t REG_EXPAND_SZ /d "%SystemRoot%\MEMORY.DMP" /f 
    REG ADD "HKLM\BROKENSYSTEM\ControlSet002\Control\CrashControl" /v NMICrashDump /t REG_DWORD /d 1 /f 
    ```

    **Descargar disco del sistema operativo roto:**

    ```
    REG UNLOAD HKLM\BROKENSYSTEM
    ```
### <a name="rebuild-the-virtual-machine"></a>Recompilación de la máquina virtual

* Realice el [paso 5 de los comandos de reparación de VM](./repair-windows-vm-using-azure-virtual-machine-repair-commands.md#repair-process-example) para volver a crear la VM.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Solución de problemas de arranque en máquinas virtuales de Azure](./boot-error-troubleshoot.md)
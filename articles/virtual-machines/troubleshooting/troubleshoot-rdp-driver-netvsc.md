---
title: Solución de un problema de netvsc.sys al conectarse de forma remota a una máquina virtual de Windows 10 o Windows Server 2016 en Azure | Microsoft Docs
description: Aprenda a solucionar un problema de RDP relacionadas con netsvc.sys cuando se conecte a una máquina virtual de Windows 10 o Windows Server 2016 en Azure.
services: virtual-machines-windows
documentationCenter: ''
author: genlin
manager: dcscontentpm
editor: v-jesits
ms.service: virtual-machines-windows
ms.topic: troubleshooting
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 11/19/2018
ms.author: genli
ms.openlocfilehash: 0f5a414f00ffa50114f090fc19f37b8a85428547
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/20/2020
ms.locfileid: "86509008"
---
# <a name="cannot-connect-remotely-to-a-windows-10-or-windows-server-2016-vm-in-azure-because-of-netvscsys"></a>netvsc.sys le impide conectarse de forma remota a una máquina virtual de Windows 10 o Windows Server 2016 en Azure

En este artículo se explica cómo solucionar un problema en que no existe ninguna conexión de red cuando se conecta a una máquina virtual (VM) de Windows 10 o Windows Server 2016 Datacenter en un host de Hyper-V Server 2016.

## <a name="symptoms"></a>Síntomas

No se puede conectar a una máquina virtual de Windows 10 o Windows Server 2016 de Azure mediante el Protocolo de escritorio remoto (RDP). En [Diagnósticos de arranque](boot-diagnostics.md), la pantalla muestra una cruz roja sobre la tarjeta de interfaz de red (NIC). Esta indica que la máquina virtual no tiene conectividad cuando el sistema operativo se ha cargado completamente.

Normalmente, este problema se produce en la [compilación 14393](https://support.microsoft.com/help/4093120/) y la [compilación 15063](https://support.microsoft.com/help/4015583/) de Windows. Si la versión del sistema operativo es posterior a estas versiones, en este artículo no se aplica a su caso. Para comprobar la versión del sistema, abra una sesión de CMD en [la característica de consola serie](serial-console-windows.md) y, a continuación, ejecute **Ver**.

## <a name="cause"></a>Causa

Este problema puede producirse si la versión del archivo del sistema netvsc.sys instalado es **10.0.14393.594** o **10.0.15063.0**. Estas versiones del archivo netvsc.sys pueden impedir que el sistema interactúe con la plataforma Azure.


## <a name="solution"></a>Solución

Antes de seguir estos pasos, [tome una instantánea del disco del sistema](../windows/snapshot-copy-managed-disk.md) de la máquina virtual afectada como copia de seguridad. Para solucionar este problema, utilice la consola serie o [repare la máquina virtual sin conexión](#repair-the-vm-offline) mediante la conexión del disco del sistema de la máquina virtual a una máquina virtual de recuperación.


### <a name="use-the-serial-console"></a>Uso de la consola serie

Conéctese a la [consola serie, abra una instancia de PowerShell](serial-console-windows.md) y, a continuación, realice los pasos siguientes.

> [!NOTE]
> Si la consola serie no está habilitada en la máquina virtual, vaya a la sección de [reparación de la máquina virtual sin conexión](#repair-the-vm-offline).

1. Ejecute el siguiente comando en una instancia de PowerShell para obtener la versión del archivo (**c:\windows\system32\drivers\netvsc.sys**):

   ```
   (get-childitem "$env:systemroot\system32\drivers\netvsc.sys").VersionInfo.FileVersion
   ```

2. Descargue la actualización apropiada en un disco de datos nuevo o existente que esté conectado a una máquina virtual en funcionamiento de la misma región:

   - **10.0.14393.594**: [KB4073562](https://support.microsoft.com/help/4073562) o una actualización posterior
   - **10.0.15063.0**: [KB4016240](https://support.microsoft.com/help/4016240) o una actualización posterior

3. Desconecte el disco de utilidad de la máquina virtual en funcionamiento y, a continuación, vuelva a conectarlo a la máquina virtual dañada.

4. Ejecute el comando siguiente para instalar la actualización en la máquina virtual:

   ```
   dism /ONLINE /add-package /packagepath:<Utility Disk Letter>:\<KB .msu or .cab>
   ```

5. Reinicie la VM.

### <a name="repair-the-vm-offline"></a>Reparación de la máquina virtual sin conexión

1. [Conecte el disco del sistema a una máquina virtual de recuperación](./troubleshoot-recovery-disks-portal-windows.md).

2. Inicie una conexión mediante el Escritorio remoto a la máquina virtual de recuperación.

3. Asegúrese de que el disco aparece marcado como **En línea** en la consola de Administración de discos. Anote la letra de unidad asignada al disco del sistema conectado.

4. Cree una copia de la carpeta **\Windows\System32\config** por si se necesita una reversión de los cambios.

5. En la máquina virtual de rescate, inicie el Editor del Registro (regedit.exe).

6. Seleccione la clave **HKEY_LOCAL_MACHINE** y, a continuación, seleccione **Archivo** > **Cargar subárbol** en el menú.

7. Vaya al archivo SYSTEM de la carpeta **\Windows\System32\config**.

8. Seleccione **Abrir**, escriba **BROKENSYSTEM** como nombre, expanda la clave **HKEY_LOCAL_MACHINE** y, a continuación, verá una clave adicional denominada **BROKENSYSTEM**.

9. Vaya a la siguiente ubicación:

   ```
   HKLM\BROKENSYSTEM\ControlSet001\Control\Class\{4d36e972-e325-11ce-bfc1-08002be10318}
   ```

10. En cada subclave (como 0000), examine el valor de **DriverDesc** que se muestra como **Adaptador de red de Microsoft Hyper-V**.

11. En la subclave, examine el valor de **DriverVersion**, que es la versión del controlador del adaptador de red de la máquina virtual.

12. Descargue la actualización adecuada:

    - **10.0.14393.594**: [KB4073562](https://support.microsoft.com/help/4073562) o una actualización posterior
    - **10.0.15063.0**: [KB4016240](https://support.microsoft.com/help/4016240) o una actualización posterior

13. Conecte el disco del sistema como un disco de datos en una máquina virtual de rescate en la que pueda descargar la actualización.

14. Ejecute el comando siguiente para instalar la actualización en la máquina virtual:

    ```
    dism /image:<OS Disk letter>:\ /add-package /packagepath:c:\temp\<KB .msu or .cab>
    ```

15. Ejecute el comando siguiente para desmontar los subárboles:

    ```
    reg unload HKLM\BROKENSYSTEM
    ```

16. [Desconecte el disco del sistema y vuelva a crear la máquina virtual](./troubleshoot-recovery-disks-portal-windows.md).

## <a name="need-help-contact-support"></a>¿Necesita ayuda? Ponerse en contacto con soporte técnico

Si sigue necesitando ayuda, [póngase en contacto con el servicio de soporte técnico de Azure](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) para resolver el problema rápidamente.

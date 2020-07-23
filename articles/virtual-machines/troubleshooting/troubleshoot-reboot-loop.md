---
title: Bucle de reinicio de Windows en una máquina virtual de Azure | Microsoft Docs
description: Aprenda a solucionar problemas de bucle de reinicio de Windows | Microsoft Docs
services: virtual-machines-windows
documentationCenter: ''
author: genlin
manager: dcscontentpm
editor: ''
ms.service: virtual-machines-windows
ms.topic: troubleshooting
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 10/15/2018
ms.author: genli
ms.openlocfilehash: 4c336fe9a65d7bcc44790a4bfb02bed44f028733
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/20/2020
ms.locfileid: "86500929"
---
# <a name="windows-reboot-loop-on-an-azure-vm"></a>Bucle de reinicio de Windows en una máquina virtual de Azure
En este artículo se describe el bucle de reinicio que puede experimentar en una máquina virtual (VM) Windows en Microsoft Azure.

## <a name="symptom"></a>Síntoma

Puede ocurrir cuando usa [diagnósticos de arranque](./boot-diagnostics.md) para obtener capturas de pantalla de una máquina virtual y se encuentra con que la máquina virtual se está iniciando pero el proceso de arranque se interrumpe y vuelve a empezar continuamente.

![Pantalla de inicio 1](./media/troubleshoot-reboot-loop/start-screen-1.png)

## <a name="cause"></a>Causa

El bucle de reinicio se produce debido a las siguientes causas:

### <a name="cause-1"></a>Causa 1

Hay un servicio de terceros marcado como crítico y que no se puede iniciar. Esto hace que el sistema operativo se reinicie.

### <a name="cause-2"></a>Causa 2

Se realizaron algunos cambios en el sistema operativo. Normalmente, estos están relacionados con la instalación de una actualización, de una aplicación o una nueva directiva. Es posible que deba comprobar los registros siguientes para obtener más información:

- Registros de eventos
- CBS.logWindows
- Update.log

### <a name="cause-3"></a>Causa 3

Un daño en el sistema de archivos podría ser la causa. Sin embargo, es difícil diagnosticar e identificar el cambio que provoca el daño del sistema operativo.

## <a name="solution"></a>Solución

Para resolver este problema, [haga una copia de seguridad del disco del sistema operativo](../windows/snapshot-copy-managed-disk.md) y [conecte el disco del sistema operativo a una máquina virtual de rescate](./troubleshoot-recovery-disks-portal-windows.md) y, a continuación, siga las opciones de solución pertinentes, o pruebe las soluciones una por una.

### <a name="solution-for-cause-1"></a>Solución para la causa 1

1. Una vez que el disco del sistema operativo está conectado a una máquina virtual en funcionamiento, asegúrese de que el disco está marcado como **En línea** en la consola de administración del disco y anote la letra de unidad de la partición que contiene la carpeta **\Windows**.

2. Si el disco está establecido en **Sin conexión**, vuelva a establecerlo en **En línea**.

3. Cree una copia de la carpeta **\Windows\System32\config** en caso de que se necesite una reversión de los cambios.

4. En la máquina virtual de rescate, abra el Editor del Registro de Windows (regedit).

5. Seleccione la clave **HKEY_LOCAL_MACHINE** y, a continuación, seleccione **Archivo** > **Cargar subárbol** en el menú.

6. Vaya al archivo SYSTEM de la carpeta **\Windows\System32\config**.

7. Seleccione **Abrir**, escriba **BROKENSYSTEM** como nombre, expanda la clave **HKEY_LOCAL_MACHINE** y, a continuación, verá una clave adicional denominada **BROKENSYSTEM**.

8. Compruebe desde qué ControlSet está arrancando el equipo. Verá su número de clave en la siguiente clave del registro.

    `HKEY_LOCAL_MACHINE\BROKENSYSTEM\Select\Current`

9. Compruebe cuál es la importancia del servicio de agente de la máquina virtual mediante la siguiente clave del Registro.

    `HKEY_LOCAL_MACHINE\BROKENSYSTEM\ControlSet00x\Services\RDAgent\ErrorControl`

10. Si el valor de la clave del Registro no está establecida en **2**, vaya a la siguiente mitigación.

11. Si el valor de la clave del Registro está establecida en **2**, cambie el valor de **2** a **1**.

12. Si existe alguna de las siguientes claves y tienen el valor **2** o **3**, cambie estos valores a **1** según corresponda:

    - `HKEY_LOCAL_MACHINE\BROKENSYSTEM\ControlSet00x\Services\AzureWLBackupCoordinatorSvc\ErrorControl`
    - `HKEY_LOCAL_MACHINE\BROKENSYSTEM\ControlSet00x\Services\AzureWLBackupInquirySvc\ErrorControl`
    - `HKEY_LOCAL_MACHINE\BROKENSYSTEM\ControlSet00x\Services\AzureWLBackupPluginSvc\ErrorControl`

13. Seleccione la clave **BROKENSYSTEM** y, después, seleccione **Archivo** > **Descargar subárbol** en el menú.

14. Desconecte el disco del sistema operativo de la máquina virtual de solución de problemas.

15. Extraiga el disco de la máquina virtual de solución de problemas y espere aproximadamente 2 minutos para que Azure libere este disco.

16. [Cree una nueva máquina virtual desde el disco del sistema operativo](../windows/create-vm-specialized.md).

17. Si se ha corregido el problema, es posible que deba reinstalar el agente [RDAgent](/archive/blogs/mast/install-the-vm-agent-on-an-existing-azure-vm) (WaAppAgent.exe).

### <a name="solution-for-cause-2"></a>Solución para la causa 2

Restaure la máquina virtual a la última configuración válida conocida, siga los pasos que se indican en [Cómo iniciar la VM en Windows Azure con la última configuración válida conocida](https://support.microsoft.com/help/4016731/).

### <a name="solution-for-cause-3"></a>Solución para la causa 3
>[!NOTE]
>El siguiente procedimiento solo debe usarse como último recurso. Aunque la restauración desde regback puede restaurar el acceso a la máquina, el sistema operativo no se considera estable puesto que hay una pérdida de datos en el registro entre la marca de tiempo del subárbol y el día actual. Debe crear una nueva máquina virtual y planear una migración de los datos.

1. Una vez que el disco esté conectado a una máquina virtual de solución de problemas, asegúrese de que el disco aparece marcado como **En línea** en la consola de administración de discos.

2. Cree una copia de la carpeta **\Windows\System32\config** en caso de que se necesite una reversión de los cambios.

3. Copie los archivos en la carpeta **\Windows\System32\config\regback** y reemplace los archivos de la carpeta **\Windows\System32\config**.

4. Extraiga el disco de la máquina virtual de solución de problemas y espere aproximadamente 2 minutos para que Azure libere este disco.

5. [Cree una nueva máquina virtual desde el disco del sistema operativo](../windows/create-vm-specialized.md).

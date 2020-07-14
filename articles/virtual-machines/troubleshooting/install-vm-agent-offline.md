---
title: Instalación del agente de máquina virtual de Azure en modo sin conexión | Microsoft Docs
description: Aprenda a instalar al agente de máquina virtual de Azure en modo sin conexión.
services: virtual-machines-windows
documentationcenter: ''
author: genlin
manager: dcscontentpm
editor: ''
tags: azure-resource-manager
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.topic: article
ms.date: 07/06/2020
ms.author: genli
ms.openlocfilehash: 456aa225fa8eed47ca794c54e61b77a30c93fa9a
ms.sourcegitcommit: 93462ccb4dd178ec81115f50455fbad2fa1d79ce
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/06/2020
ms.locfileid: "85983237"
---
# <a name="install-the-azure-virtual-machine-agent-in-offline-mode"></a>Instalación del agente de máquina virtual de Azure en modo sin conexión 

El agente de máquina virtual de Azure (agente de VM) proporciona características útiles, como el restablecimiento de contraseña de administrador local y la inserción de scripts. En este artículo se muestra cómo instalar al agente de máquina virtual para una máquina virtual (VM) de Windows sin conexión. 

## <a name="when-to-use-the-vm-agent-in-offline-mode"></a>Cuándo usar el agente de máquina virtual en modo sin conexión

Instale al agente de máquina virtual en modo sin conexión en los escenarios siguientes:

- La máquina virtual de Azure implementada no tiene instalado el agente de máquina virtual o el agente no funciona.
- Olvidó la contraseña de administrador de la máquina virtual o no puede acceder a ella.

## <a name="how-to-install-the-vm-agent-in-offline-mode"></a>Instalación del agente de máquina virtual en modo sin conexión

Use los pasos siguientes para instalar al agente de máquina virtual en modo sin conexión.

### <a name="step-1-attach-the-os-disk-of-the-vm-to-another-vm-as-a-data-disk"></a>Paso 1: Asociación del disco del sistema operativo de la máquina virtual a otra máquina virtual como disco de datos

1. Realice una instantánea del disco del sistema operativo de la máquina virtual afectada, cree un disco a partir de la instantánea y, a continuación, conecte el disco a una máquina virtual de solución de problemas. Para más información, consulte [Solución de problemas de una máquina virtual Windows mediante la conexión del disco del sistema operativo a una máquina virtual de recuperación mediante Azure Portal](troubleshoot-recovery-disks-portal-windows.md). Para la máquina virtual clásica, elimine la máquina virtual y conserve el disco del sistema operativo y, a continuación, conecte el disco del sistema operativo a la máquina virtual de solución de problemas.

2.  Conéctese a la máquina virtual de solucionador de problemas. Abra **Administración de equipos** > **Administración de discos**. Confirme que el disco del sistema operativo está en línea y que las letras de unidad están asignadas a las particiones de disco.

### <a name="step-2-modify-the-os-disk-to-install-the-azure-vm-agent"></a>Paso 2: Modificación del disco del sistema operativo para instalar al agente de máquina virtual de Azure

1.  Realice una conexión de Escritorio remoto a la máquina virtual de solucionador de problemas.

2.  En la máquina virtual para solucionar problemas, navegue hasta el disco del sistema operativo que conectó y abra la carpeta \windows\system32\config. Realice una copia de seguridad de todos los archivos de esta carpeta, por si fuera necesaria una reversión.

3.  Inicie el **Editor del Registro** (regedit.exe).

4.  Seleccione la clave **HKEY_LOCAL_MACHINE**. En el menú, seleccione **Archivo** > **Cargar subárbol**:

    ![Carga del subárbol](./media/install-vm-agent-offline/load-hive.png)

5.  Vaya a la carpeta \windows\system32\config\SYSTEM en el disco del sistema operativo que ha asociado. Como nombre de la sección, escriba **BROKENSYSTEM**. El nuevo subárbol del Registro se muestra bajo la clave **HKEY_LOCAL_MACHINE**.

6.  Vaya a la carpeta \windows\system32\config\SOFTWARE en el disco del sistema operativo que ha asociado. Como nombre del software del subárbol, escriba **BROKENSOFTWARE**.

7. Si el disco del sistema operativo adjunto tiene instalado el agente de máquina virtual, realice una copia de seguridad de la configuración actual. Si no tiene instalado el agente de máquina virtual, vaya al paso siguiente.
      
    1. Cambie el nombre de la carpeta \windowsazure por \windowsazure.old.

    2. Exporte los registros siguientes:
        - HKEY_LOCAL_MACHINE\BROKENSYSTEM\ControlSet001\Services\WindowsAzureGuestAgent
        - HKEY_LOCAL_MACHINE\BROKENSYSTEM\ControlSet001\Services\RdAgent

8.  Use los archivos existentes en la maquina virtual de solucionador de problemas como repositorio para la instalación del agente de máquina virtual. Complete los pasos siguientes:

    1. En la máquina virtual de solucionador de problemas, exporte las siguientes subclaves en el formato del Registro (. reg): 
        - HKEY_LOCAL_MACHINE  \SYSTEM\ControlSet001\Services\WindowsAzureGuestAgent
        - HKEY_LOCAL_MACHINE  \SYSTEM\ControlSet001\Services\RdAgent

          ![Exportación de las subclaves del Registro](./media/install-vm-agent-offline/backup-reg.png)

    2. Edite los archivos de Registro. En cada archivo, cambie el valor de entrada **SYSTEM** por **BROKENSYSTEM** (como se muestra en las siguientes imágenes) y guarde el archivo. Recuerde el elemento **ImagePath** del agente de máquina virtual actual. Tendremos que copiar la carpeta correspondiente en el disco del sistema operativo adjunto. 

        ![Cambio de los valores de subclave del Registro](./media/install-vm-agent-offline/change-reg.png)

    3. Importe los archivos del Registro en el repositorio haciendo doble clic en cada uno de ellos.

    4. Confirme que las siguientes dos subclaves se han importado correctamente en el subárbol **BROKENSYSTEM**:
        - WindowsAzureGuestAgent
        - RdAgent

    5. Copie la carpeta de instalación del agente de máquina virtual actual en el disco del sistema operativo adjunto: 

        1.  En el disco del sistema operativo que ha adjuntado, cree una carpeta denominada WindowsAzure en la ruta de acceso raíz.

        2.  Vaya a C:\WindowsAzure en la máquina virtual del solucionador de problemas, y busque cualquier carpeta con el nombre C:\WindowsAzure\GuestAgent_X.X.XXXX.XXX. Copie la carpeta GuestAgent que tenga el número de versión más reciente de C:\WindowsAzure en la carpeta WindowsAzure del disco del sistema operativo adjunto. Si no está seguro de qué carpetas deben copiarse, copie todas las carpetas denominadas GuestAgent. En la siguiente imagen se muestra un ejemplo de la carpeta GuestAgent que se copia en el disco del sistema operativo adjunto.

             ![Copiar la carpeta GuestAgent](./media/install-vm-agent-offline/copy-files.png)

9.  Seleccione **BROKENSYSTEM**. En el menú, seleccione **Archivo** > **Descargar subárbol**.

10.  Seleccione **BROKENSOFTWARE**. En el menú, seleccione **Archivo** > **Descargar subárbol**.

11.  Desconecte el disco del sistema operativo y, a continuación, [cambie el disco de la máquina virtual afectada](troubleshoot-recovery-disks-portal-windows.md#swap-the-os-disk-for-the-vm). Para la máquina virtual clásica, cree una nueva máquina virtual con el disco del sistema operativo reparado.

12.  Acceda a la máquina virtual. Tenga en cuenta que se está ejecutando RdAgent y que se están generando los registros.

Si ha creado la máquina virtual mediante el modelo de implementación de Resource Manager, ya ha terminado.

### <a name="use-the-provisionguestagent-property-for-classic-vms"></a>Utilice la propiedad ProvisionGuestAgent para las máquinas virtuales clásicas

[!INCLUDE [classic-vm-deprecation](../../../includes/classic-vm-deprecation.md)]

Si ha creado la VM mediante el modelo clásico, use el módulo de Azure PowerShell para actualizar la propiedad **ProvisionGuestAgent**. La propiedad informa a Azure de que la máquina virtual tiene instalado el agente de máquina virtual.

Para establecer la propiedad **ProvisionGuestAgent**, ejecute los siguientes comandos en Azure PowerShell:

   ```powershell
   $vm = Get-AzureVM –ServiceName <cloud service name> –Name <VM name>
   $vm.VM.ProvisionGuestAgent = $true
   Update-AzureVM –Name <VM name> –VM $vm.VM –ServiceName <cloud service name>
   ```

A continuación, ejecute el comando `Get-AzureVM` . Tenga en cuenta que la propiedad **GuestAgentStatus** está ahora rellena con datos:

   ```powershell
   Get-AzureVM –ServiceName <cloud service name> –Name <VM name>
   GuestAgentStatus:Microsoft.WindowsAzure.Commands.ServiceManagement.Model.PersistentVMModel.GuestAgentStatus
   ```

## <a name="next-steps"></a>Pasos siguientes

- [Información general del agente de máquina virtual de Azure](../extensions/agent-windows.md)
- [Características y extensiones de las máquinas virtuales para Windows](../extensions/features-windows.md)

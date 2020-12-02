---
title: Uso de herramientas remotas para solucionar problemas con la VM de Azure | Microsoft Docs
description: Obtenga información sobre PsExec, scripts de PowerShell y otras herramientas remotas que puede usar para solucionar de forma remota los problemas de máquinas virtuales de Azure sin usar RDP.
services: virtual-machines-windows
documentationcenter: ''
author: Deland-Han
manager: dcscontentpm
editor: ''
tags: ''
ms.service: virtual-machines
ms.topic: troubleshooting
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: azurecli
ms.date: 01/11/2018
ms.author: delhan
ms.openlocfilehash: c5974388c096c9bc8693c5fc2cf918989c6eadd3
ms.sourcegitcommit: d60976768dec91724d94430fb6fc9498fdc1db37
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/02/2020
ms.locfileid: "96488738"
---
# <a name="use-remote-tools-to-troubleshoot-azure-vm-issues"></a>Uso de herramientas remotas para solucionar problemas con la VM de Azure

Para solucionar problemas en una máquina virtual (VM) de Azure, puede conectarse a la VM mediante el uso de las herramientas remotas que se describen en este artículo en lugar de utilizar el Protocolo de escritorio remoto (RDP).

## <a name="serial-console"></a>Consola de serie

Use una [consola serie para Azure Virtual Machines](serial-console-windows.md) para ejecutar comandos en la máquina virtual remota de Azure.

## <a name="remote-cmd"></a>CMD remoto

Descargue [PsExec](/sysinternals/downloads/psexec). Conéctese a la máquina virtual ejecutando el comando siguiente:

```cmd
psexec \\<computer>-u user -s cmd
```

>[!NOTE]
>* El comando debe ejecutarse en un equipo que esté en la misma red virtual.
>* Los valores de DIP o HostName pueden usarse para reemplazar \<computer>.
>* El parámetro -s asegura que se invoca el comando mediante el uso de la cuenta del sistema (permisos de administrador).
>* PsExec usa los puertos TCP 135 y 445. Por lo tanto, los dos puertos deben estar abiertos en el firewall.

## <a name="run-command"></a>Comando Ejecutar

Para más información sobre cómo usar la característica de ejecución de comandos para ejecutar scripts en la máquina virtual, consulte [Ejecución de scripts de PowerShell en la máquina virtual Windows mediante Ejecutar comando](../windows/run-command.md).

## <a name="custom-script-extension"></a>Custom Script Extension

Puede usar la característica Extensión de script personalizado para ejecutar un script personalizado en la máquina virtual de destino. Para poder usar esta característica, deben cumplirse las condiciones siguientes:

* La máquina virtual debe tener conectividad.
* El agente de máquina virtual de Azure está instalado y funciona según lo previsto en la máquina virtual.
* La extensión no se instaló previamente en la máquina virtual.
 
  La extensión inserta el script solo la primera vez que se usa. Si usa esta característica más adelante, la extensión reconoce que ya se ha utilizado y el nuevo script no se carga.

Cargue el script en una cuenta de almacenamiento y genere su propio contenedor. A continuación, ejecute el siguiente script de Azure PowerShell en un equipo que tenga conectividad a la máquina virtual.

### <a name="for-classic-deployment-model-vms"></a>Para máquinas virtuales del modelo de implementación clásico

[!INCLUDE [classic-vm-deprecation](../../../includes/classic-vm-deprecation.md)]


```powershell
#Set up the basic variables.
$subscriptionID = "<<SUBSCRIPTION ID>>" 
$storageAccount = "<<STORAGE ACCOUNT>>" 
$localScript = "<<FULL PATH OF THE PS1 FILE TO EXECUTE ON THE VM>>" 
$blobName = "file.ps1" #Name you want for the blob in the storage.
$vmName = "<<VM NAME>>" 
$vmCloudService = "<<CLOUD SERVICE>>" #Resource group or cloud service where the VM is hosted. For example, for "demo305.cloudapp.net" the cloud service is going to be demo305.

#Set up the Azure PowerShell module, and ensure the access to the subscription.
Import-Module Azure
Add-AzureAccount  #Ensure login with the account associated with the subscription ID.
Get-AzureSubscription -SubscriptionId $subscriptionID | Select-AzureSubscription

#Set up the access to the storage account, and upload the script.
$storageKey = (Get-AzureStorageKey -StorageAccountName $storageAccount).Primary
$context = New-AzureStorageContext -StorageAccountName $storageAccount -StorageAccountKey $storageKey
$container = "cse" + (Get-Date -Format yyyyMMddhhmmss)<
New-AzureStorageContainer -Name $container -Permission Off -Context $context
Set-AzureStorageBlobContent -File $localScript -Container $container -Blob $blobName  -Context $context

#Push the script into the VM.
$vm = Get-AzureVM -ServiceName $vmCloudService -Name $vmName
Set-AzureVMCustomScriptExtension "CustomScriptExtension" -VM $vm -StorageAccountName $storageAccount -StorageAccountKey $storagekey -ContainerName $container -FileName $blobName -Run $blobName | Update-AzureVM
```

### <a name="for-azure-resource-manager-vms"></a>Para máquinas virtuales de Azure Resource Manager

 

```powershell
#Set up the basic variables.
$subscriptionID = "<<SUBSCRIPTION ID>>"
$storageAccount = "<<STORAGE ACCOUNT>>"
$storageRG = "<<RESOURCE GROUP OF THE STORAGE ACCOUNT>>" 
$localScript = "<<FULL PATH OF THE PS1 FILE TO EXECUTE ON THE VM>>" 
$blobName = "file.ps1" #Name you want for the blob in the storage.
$vmName = "<<VM NAME>>" 
$vmResourceGroup = "<<RESOURCE GROUP>>"
$vmLocation = "<<DATACENTER>>" 
 
#Set up the Azure PowerShell module, and ensure the access to the subscription.
Login-AzAccount #Ensure login with the account associated with the subscription ID.
Get-AzSubscription -SubscriptionId $subscriptionID | Select-AzSubscription

#Set up the access to the storage account, and upload the script.
$storageKey = (Get-AzStorageAccountKey -ResourceGroupName $storageRG -Name $storageAccount).Value[0]
$context = New-AzureStorageContext -StorageAccountName $storageAccount -StorageAccountKey $storageKey
$container = "cse" + (Get-Date -Format yyyyMMddhhmmss)
New-AzureStorageContainer -Name $container -Permission Off -Context $context
Set-AzureStorageBlobContent -File $localScript -Container $container -Blob $blobName  -Context $context

#Push the script into the VM.
Set-AzVMCustomScriptExtension -Name "CustomScriptExtension" -ResourceGroupName $vmResourceGroup -VMName $vmName -Location $vmLocation -StorageAccountName $storageAccount -StorageAccountKey $storagekey -ContainerName $container -FileName $blobName -Run $blobName
```

## <a name="remote-powershell"></a>PowerShell remoto

>[!NOTE]
>Puerto TCP 5986 (HTTPS) debe estar abierto para que pueda usar esta opción.
>
>En el caso de las máquinas virtuales de Azure Resource Manager, debe abrir el puerto 5986 en el grupo de seguridad de red (NSG). Para más información, consulte Grupos de seguridad. 
>
>En el caso de las máquinas virtuales de RDFE, debe tener un punto de conexión con un puerto privado (5986) y un puerto público. A continuación, también tendrá que abrir ese puerto público en NSG.

### <a name="set-up-the-client-computer"></a>Configuración del equipo cliente

Para usar PowerShell para conectarse de forma remota a la máquina virtual, primero debe configurar el equipo cliente para permitir la conexión. Para ello, agregue la máquina virtual a la lista de hosts de confianza de PowerShell ejecutando el comando siguiente, según corresponda.

Para agregar una máquina virtual a la lista de hosts de confianza:

```powershell
Set-Item wsman:\localhost\Client\TrustedHosts -value <ComputerName>
```

Para agregar varias máquinas virtuales a la lista de hosts de confianza:

```powershell
Set-Item wsman:\localhost\Client\TrustedHosts -value <ComputerName1>,<ComputerName2>
```

Para agregar todos los equipos a la lista de hosts de confianza:

```powershell
Set-Item wsman:\localhost\Client\TrustedHosts -value *
```

### <a name="enable-remoteps-on-the-vm"></a>Habilitar RemotePS en la máquina virtual

En el caso de las máquinas virtuales creadas con el modelo de implementación clásica, use la extensión de script personalizado para ejecutar el siguiente script:

```powershell
Enable-PSRemoting -Force
New-NetFirewallRule -Name "Allow WinRM HTTPS" -DisplayName "WinRM HTTPS" -Enabled True -Profile Any -Action Allow -Direction Inbound -LocalPort 5986 -Protocol TCP
$thumbprint = (New-SelfSignedCertificate -DnsName $env:COMPUTERNAME -CertStoreLocation Cert:\LocalMachine\My).Thumbprint
$command = "winrm create winrm/config/Listener?Address=*+Transport=HTTPS @{Hostname=""$env:computername""; CertificateThumbprint=""$thumbprint""}"
cmd.exe /C $command
```

En el caso de las máquinas virtuales de Azure Resource Manager, utilice Ejecutar comandos desde el portal para ejecutar el script EnableRemotePS:

![Comando Ejecutar](./media/remote-tools-troubleshoot-azure-vm-issues/run-command.png)

### <a name="connect-to-the-vm"></a>Conexión a la máquina virtual

Ejecute el siguiente comando en función de la ubicación del equipo cliente:

* Fuera de la red virtual o la implementación

  * Para una máquina virtual creada mediante el modelo de implementación clásica, ejecute el siguiente comando:

    ```powershell
    $Skip = New-PSSessionOption -SkipCACheck -SkipCNCheck
    Enter-PSSession -ComputerName  "<<CLOUDSERVICENAME.cloudapp.net>>" -port "<<PUBLIC PORT NUMBER>>" -Credential (Get-Credential) -useSSL -SessionOption $Skip
    ```

  * Para una máquina virtual de Azure Resource Manager, primero agregue un nombre DNS a la dirección IP pública. Para obtener pasos detallados, vea [Creación de un nombre de dominio completo en Azure Portal para una máquina virtual Windows](../create-fqdn.md). Luego, ejecute el siguiente comando:

    ```powershell
    $Skip = New-PSSessionOption -SkipCACheck -SkipCNCheck
    Enter-PSSession -ComputerName "<<DNSname.DataCenter.cloudapp.azure.com>>" -port "5986" -Credential (Get-Credential) -useSSL -SessionOption $Skip
    ```

* En la red virtual o implementación, ejecute el comando siguiente:
  
  ```powershell
  $Skip = New-PSSessionOption -SkipCACheck -SkipCNCheck
  Enter-PSSession -ComputerName  "<<HOSTNAME>>" -port 5986 -Credential (Get-Credential) -useSSL -SessionOption $Skip
  ```

>[!NOTE] 
>Al activar la marca SkipCaCheck se omite el requisito de importar un certificado a la máquina virtual cuando se inicia la sesión.

También puede usar el cmdlet Invoke-Command para ejecutar un script en la máquina virtual de forma remota.

```powershell
Invoke-Command -ComputerName "<<COMPUTERNAME>" -ScriptBlock {"<<SCRIPT BLOCK>>"}
```

## <a name="remote-registry"></a>Registro remoto

>[!NOTE]
>El puerto TCP 135 o 445 debe estar abierto para poder usar esta opción.
>
>En el caso de las máquinas virtuales de Azure Resource Manager, tiene que abrir el puerto 5986 en el NSG. Para más información, consulte Grupos de seguridad. 
>
>En el caso de las máquinas virtuales de RDFE, debe tener un punto de conexión con un puerto privado 5986 y un puerto público. También tendrá que abrir ese puerto público en NSG.

1. Desde otra máquina virtual en la misma red virtual, abra el editor del Registro (regedit.exe).

2. Seleccione **Archivo** > **Connect Network Registry** (Conectar registro de red).

   ![Editor del Registro](./media/remote-tools-troubleshoot-azure-vm-issues/remote-registry.png) 

3. Busque la máquina virtual de destino por **nombre de host** o **IP dinámica** (preferible) introduciéndola en el cuadro **Escriba el nombre del objeto que desea seleccionar**.

   ![Cuadro Escriba el nombre de objeto que desea seleccionar](./media/remote-tools-troubleshoot-azure-vm-issues/input-computer-name.png) 
 
4. Introduzca las credenciales para la VM de destino.

5. Realice los cambios necesarios en el registro.

## <a name="remote-services-console"></a>Consola de servicios remotos

>[!NOTE]
>Los puertos TCP 135 o 445 deben estar abiertos para poder usar esta opción.
>
>En el caso de las máquinas virtuales de Azure Resource Manager, tiene que abrir el puerto 5986 en el NSG. Para más información, consulte Grupos de seguridad. 
>
>En el caso de las máquinas virtuales de RDFE, debe tener un punto de conexión con un puerto privado 5986 y un puerto público. También tendrá que abrir ese puerto público en NSG.

1. Desde otra máquina virtual en la misma red virtual, abra una instancia de **Services.msc**.

2. Haga clic con el botón derecho en **Servicios (Local)** .

3. Seleccione **Conectarse a otro equipo**.

   ![Servicio remoto](./media/remote-tools-troubleshoot-azure-vm-issues/remote-services.png)

4. Escriba la dirección IP dinámica de la máquina virtual de destino.

   ![Escribir la dirección IP dinámica](./media/remote-tools-troubleshoot-azure-vm-issues/input-ip-address.png)

5. Realice los cambios necesarios en los servicios.

## <a name="next-steps"></a>Pasos siguientes

- Para más información sobre el cmdlet Enter-PSSession, consulte [Enter-PSSession](/powershell/module/microsoft.powershell.core/enter-pssession?view=powershell-5.1).
- Para más información sobre la extensión de script personalizado para Windows con el modelo de implementación clásica, consulte [Extensión del script personalizado para Windows](../extensions/custom-script-windows.md).
- PsExec forma parte de [PSTools Suite](https://download.sysinternals.com/files/PSTools.zip).
- Para más información sobre PSTools Suite, consulte [PSTools](/sysinternals/downloads/pstools).
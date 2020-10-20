---
title: 'Migración de máquinas virtuales de VMware a Azure (sin agente): PowerShell'
description: Aprenda a ejecutar una migración de máquinas virtuales de VMware sin agente con Azure Migrate mediante PowerShell.
services: ''
author: rahugup
manager: bsiva
ms.topic: tutorial
ms.date: 10/1/2020
ms.author: rahugup
ms.openlocfilehash: eed10f13b9495ab2cccfd9c57ae14ccc5d8e4a63
ms.sourcegitcommit: 2e72661f4853cd42bb4f0b2ded4271b22dc10a52
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/14/2020
ms.locfileid: "92043551"
---
# <a name="migrate-vmware-vms-to-azure-agentless---powershell"></a>Migración de máquinas virtuales de VMware a Azure (sin agente): PowerShell

En este artículo aprenderá a migrar máquinas virtuales de VMware detectadas con el método sin agente mediante Azure PowerShell para [Azure Migrate: Server Migration](migrate-services-overview.md#azure-migrate-server-migration-tool). 

Aprenderá a:

> [!div class="checklist"]
> * Recuperar máquinas virtuales de VMware detectadas en un proyecto de Azure Migrate.
> * Iniciar la replicación de las máquinas virtuales.
> * Actualizar las propiedades para replicar máquinas virtuales.
> * Supervisar la replicación.
> * Ejecute una migración de prueba para asegurarse de que todo funciona de la forma esperada.
> * Ejecutar una migración completa de la máquina virtual.

> [!NOTE]
> En los tutoriales se muestra la ruta de implementación más sencilla para un escenario, de modo que pueda configurar rápidamente una prueba de concepto. En ellos se usan las opciones predeterminadas siempre que es posible y no muestran todos los valores y rutas de acceso posibles.

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/pricing/free-trial/) antes de empezar.

## <a name="prerequisites"></a>Requisitos previos

Antes de comenzar este tutorial, debe:

1. [Complete el tutorial de detección](tutorial-discover-vmware.md) para preparar Azure y VMware para la migración.
2. Se recomienda completar el segundo tutorial para [evaluar las máquinas virtuales de VMware](tutorial-assess-vmware.md) antes de migrarlas a Azure.
3. Tiene el módulo `Az` de Azure PowerShell. Si necesita instalar o actualizar Azure PowerShell, siga esta [guía para instalar y configurar Azure PowerShell](/powershell/azure/install-az-ps).

## <a name="install-azure-migrate-powershell-module"></a>Instalación del módulo de PowerShell de Azure Migrate

El módulo de PowerShell de Azure Migrate está disponible en versión preliminar. Deberá instalar el módulo de PowerShell mediante el siguiente comando. 

```azurepowershell
Install-Module -Name Az.Migrate 
```

## <a name="sign-in-to-your-microsoft-azure-subscription"></a>Inicio de sesión en la suscripción de Microsoft Azure

Inicie sesión en su suscripción de Azure con el cmdlet `Connect-AzAccount`.

```azurepowershell
Connect-AzAccount
```

Seleccione su suscripción a Azure. Use el cmdlet `Get-AzSubscription` para obtener la lista de suscripciones de Azure a las que tiene acceso. Seleccione la suscripción de Azure donde trabajará con el proyecto de Azure Migrate mediante el cmdlet `Set-AzContext`.

```azurepowershell
Set-AzContext -SubscriptionId "xxxx-xxxx-xxxx-xxxx"
```

## <a name="retrieve-the-azure-migrate-project"></a>Recuperación del proyecto de Azure Migrate

El proyecto de Azure Migrate se usa para almacenar los metadatos de detección, valoración y migración recopilados del entorno que se va a evaluar o migrar.
En un proyecto puede realizar un seguimiento de los recursos detectados, coordinar valoraciones y realizar migraciones.

Como parte de los requisitos previos, ya había creado un proyecto de Azure Migrate. Con el cmdlet `Get-AzMigrateProject` puede recuperar los detalles de un proyecto de Azure Migrate. Deberá especificar el nombre del proyecto de Azure Migrate (`Name`) y el del grupo de recursos de ese proyecto (`ResourceGroupName`).

```azurepowershell
# Get resource group of the Azure Migrate project
$ResourceGroup = Get-AzResourceGroup -Name "MyResourceGroup"

# Get details of the Azure Migrate project
$MigrateProject = Get-AzMigrateProject -Name "MyMigrateProject" -ResourceGroupName $ResourceGroup.ResourceGroupName

# View Azure Migrate project details
$MigrateProject | ConvertTo-JSON
```

## <a name="retrieve-discovered-vms-in-an-azure-migrate-project"></a>Recuperación de las máquinas virtuales detectadas en un proyecto de Azure Migrate

Azure Migrate usa un [dispositivo de Azure Migrate](migrate-appliance-architecture.md) ligero. Como parte de los requisitos previos, habría implementado el dispositivo de Azure Migrate como máquina virtual de VMware.

Para recuperar una máquina virtual de VMware específica en un proyecto de Azure Migrate, especifique el nombre del proyecto de Azure Migrate (`ProjectName`), el grupo de recursos del proyecto (`ResourceGroupName`) y el nombre de la máquina virtual (`DisplayName`). 

```azurepowershell
# Get a specific VMware VM in an Azure Migrate project
$DiscoveredServer = Get-AzMigrateDiscoveredServer -ProjectName $MigrateProject.Name -ResourceGroupName $ResourceGroup.ResourceGroupName -DisplayName "MyTestVM"

# View discovered server details
$DiscoveredServer | ConvertTo-JSON
```
En este tutorial vamos a migrar esta máquina virtual.

También puede recuperar todas las máquinas virtuales de VMware en un proyecto Azure Migrate mediante los parámetros `ProjectName` y `ResourceGroupName`.

```azurepowershell
# Get all VMware VMs in an Azure Migrate project
$DiscoveredServers = Get-AzMigrateDiscoveredServer -ProjectName $MigrateProject.Name -ResourceGroupName $ResourceGroup.ResourceGroupName 
```
Si tiene varios dispositivos en un proyecto de Azure Migrate, puede usar los parámetros `ProjectName`, `ResourceGroupName`y `ApplianceName` para recuperar todas las máquinas virtuales detectadas mediante un dispositivo de Azure Migrate específico. 

```azurepowershell
# Get all VMware VMs discovered by an Azure Migrate Appliance in an Azure Migrate project
$DiscoveredServers = Get-AzMigrateDiscoveredServer -ProjectName $MigrateProject.Name -ResourceGroupName $ResourceGroup.ResourceGroupName -ApplianceName "MyMigrateAppliance" |Format-Table DisplayName, Name, Type

```

## <a name="initialize-replication-infrastructure"></a>Inicialización de la infraestructura de replicación

[Azure Migrate: Server Migration](migrate-services-overview.md#azure-migrate-server-migration-tool) aprovecha varios recursos de Azure para migrar máquinas virtuales. Server Migration aprovisiona los recursos siguientes en el mismo grupo de recursos que el proyecto.

- **Service Bus**: Server Migration usa Service Bus para enviar mensajes de orquestación de replicación al dispositivo.
- **Cuenta de almacenamiento de puerta de enlace**: Server Migration usa la cuenta de almacenamiento de puerta de enlace para almacenar información del estado de las máquinas virtuales que se replican.
- **Cuenta de almacenamiento de registros**: el dispositivo con Azure Migrate carga los registros de replicación de las máquinas virtuales en una cuenta de almacenamiento de registros. Azure Migrate aplica la información de replicación a los discos administrados de réplica.
- **Almacén de claves**: el dispositivo con Azure Migrate usa el almacén de claves para administrar las cadenas de conexión de Service Bus y las claves de acceso de las cuentas de almacenamiento utilizadas en la replicación.

Antes de replicar la primera máquina virtual en el proyecto de Azure Migrate, ejecute el script siguiente para aprovisionar la infraestructura de replicación. Este script aprovisiona y configura los recursos mencionados anteriormente para que pueda empezar a migrar las máquinas virtuales de VMware.

> [!NOTE]
> Un proyecto de Azure Migrate solo admite migraciones a una región de Azure. Una vez ejecutado este script, no se puede cambiar la región de destino a la que se van a migrar las máquinas virtuales de VMware.
> Si configura un nuevo dispositivo en el proyecto de Azure Migrate, tendrá que ejecutar el script `Initialize-AzMigrateReplicationInfrastructure`. 

En el artículo inicializaremos la infraestructura de replicación para poder migrar nuestras máquinas virtuales a la región `Central US`. Puede [descargar el archivo](https://github.com/Azure/azure-docs-powershell-samples/tree/master/azure-migrate/migrate-at-scale-vmware-agentles) del repositorio de GitHub o ejecutarlo con el fragmento de código siguiente. 

```azurepowershell
# Download the script from Azure Migrate GitHub repository 
Invoke-WebRequest https://raw.githubusercontent.com/Azure/azure-docs-powershell-samples/master/azure-migrate/migrate-at-scale-vmware-agentles/Initialize-AzMigrateReplicationInfrastructure.ps1 -OutFile .\AzMigrateReplicationinfrastructure.ps1

# Run the script for initializing replication infrastructure for the current Migrate project
.\AzMigrateReplicationInfrastructure.ps1 -ResourceGroupName $ResourceGroup.ResourceGroupName -ProjectName $MigrateProject.Name -Scenario agentlessVMware -TargetRegion "CentralUS" 
```


## <a name="replicate-vms"></a>Replicación de máquinas virtuales

Una vez terminadas la detección y la inicialización de la infraestructura de replicación, podrá empezar la replicación de máquinas virtuales de VMware en Azure. Puede ejecutar hasta 300 replicaciones simultáneamente.

Puede especificar las propiedades de replicación como se indica a continuación.

- **Suscripción y grupo de recursos de destino**: especifique la suscripción y el grupo de recursos donde migrar la máquina virtual; para ello, proporcione el identificador del grupo de recursos con el parámetro `TargetResourceGroupId`. 
- **Red y subred virtuales de destino**: especifique el identificador de la red virtual de Azure y el nombre de la subred donde debe migrarse la máquina virtual mediante los parámetros `TargetNetworkId` y `TargetSubnetName` respectivamente. 
- **Nombre de máquina virtual de destino**: especifique el nombre de la máquina virtual de Azure que se va a crear mediante el parámetro `TargetVMName`.
- **Tamaño de máquina virtual de destino**: especifique el tamaño de máquina virtual de Azure que se usará para la replicación con el parámetro `TargetVMSize`. Por ejemplo, para migrar una máquina virtual a una máquina virtual D2_v2 de Azure, especifique el valor de `TargetVMSize` como "Standard_D2_v2".  
- **Licencia**: para usar Ventaja híbrida de Azure para las máquinas con Windows Server que se incluyen en las suscripciones activas de Software Assurance o Windows Server, especifique el valor del parámetro `LicenseType` como "AHUB". En otros casos, especifique el valor del parámetro `LicenseType` como "NoLicenseType".
- **Disco de sistema operativo**: especifique el identificador único del disco que tiene el cargador de arranque y el instalador del sistema operativo. El identificador de disco que se va a usar es la propiedad de identificador único (UUID) del disco recuperado mediante el cmdlet `Get-AzMigrateServer`.
- **Tipo de disco**: especifique el valor para el parámetro `DiskType` como se indica a continuación.
    - Para usar discos administrados Premium, especifique "Premium_LRS" como valor del parámetro `DiskType`. 
    - Para usar discos SSD estándar, especifique "StandardSSD_LRS" como valor del parámetro `DiskType`. 
    - Para usar discos HDD estándar, especifique "Standard_LRS" como valor del parámetro `DiskType`. 
- **Redundancia de la infraestructura**: especifique la opción de redundancia de la infraestructura como se indica a continuación. 
    - La zona de disponibilidad para anclar la máquina migrada a una zona de disponibilidad específica de la región. Use esta opción para distribuir los servidores que forman una capa de aplicación de varios nodos en Availability Zones. Esta opción solo está disponible si la región de destino seleccionada para la migración admite Availability Zones. Para usar zonas de disponibilidad, especifique el valor de zona de disponibilidad para el parámetro `TargetAvailabilityZone`.
    - El conjunto de disponibilidad para colocar la máquina migrada en un conjunto de disponibilidad. Para usar esta opción, el grupo de recursos de destino seleccionado debe tener uno o varios conjuntos de disponibilidad. Para usar el conjunto de disponibilidad, especifique el identificador del conjunto de disponibilidad para el parámetro `TargetAvailabilitySet`. 

En este tutorial se replicarán todos los discos de la máquina virtual detectada y se especificará un nombre nuevo para la máquina virtual en Azure. Especificamos el primer disco del servidor detectado como disco del sistema operativo y migraremos todos los discos como HDD estándar. Este es el disco que tiene el cargador de arranque y el instalador del sistema operativo.

```azurepowershell
# Retrieve the resource group that you want to migrate to
$TargetResourceGroup = Get-AzResourceGroup -Name "MyTargetResourceGroup"

# Retrieve the Azure virtual network and subnet that you want to migrate to
$TargetVirtualNetwork = Get-AzVirtualNetwork -Name "MyVirtualNetwork"

# Start replication for a discovered VM in an Azure Migrate project
$MigrateJob =  New-AzMigrateServerReplication -InputObject $DiscoveredServer -TargetResourceGroupId $TargetResourceGroup.ResourceId -TargetNetworkId $TargetVirtualNetwork.Id -LicenseType "NoLicenseType" -OSDiskID $DiscoveredServer.Disk[0].Uuid -TargetSubnetName $TargetVirtualNetwork.Subnets[0].Name -DiskType "Standard_LRS" -TargetVMName "MyMigratedTestVM" -TargetVMSize "Standard_DS2_v2"

# Track job status to check for completion
while (($MigrateJob.State -eq "InProgress") -or ($MigrateJob.State -eq "NotStarted")){
        #If the job hasn't completed, sleep for 10 seconds before checking the job status again
        sleep 10;
        $MigrateJob = Get-AzMigrateJob -InputObject $MigrateJob
}
#Check if the Job completed successfully. The updated job state of a successfully completed job should be "Succeeded"
Write-Output $MigrateJob.State
```

También puede replicar discos seleccionados de la máquina virtual detectada mediante el cmdlet `New-AzMigrateDiskMapping` y proporcionar esto como entrada para el parámetro `DiskToInclude` en el cmdlet `New-AzMigrateServerReplication`. También puede usar el cmdlet `New-AzMigrateDiskMapping` para especificar diferentes tipos de discos de destino para cada disco que se va a replicar. 

Especifique valores para los siguientes parámetros del cmdlet `New-AzMigrateDiskMapping`.

- **DiskId**: especifique el identificador único para el disco que se va a migrar. El identificador de disco que se va a usar es la propiedad de identificador único (UUID) del disco recuperado mediante el cmdlet `Get-AzMigrateServer`.  
- **IsOSDisk**: especifique "true" si el disco que se va a migrar es el disco del sistema operativo de la máquina virtual, de lo contrario, elija "false".
- **DiskType**: especifique el tipo de disco que se va a usar en Azure. 

En el ejemplo siguiente replicaremos solo dos discos de la máquina virtual detectada. Especificaremos el disco del sistema operativo y usaremos diferentes tipos de disco para cada replicación.

```azurepowershell
# View disk details of the discovered server
$DiscoveredServer.Disk | ConvertTo-JSON

# Create a new disk mapping for the disks to be replicated
$DisksToReplicate = @()
$OSDisk = New-AzMigrateDiskMapping -DiskID $DiscoveredServer.Disk[0].Uuid -DiskType "StandardSSD_LRS" -IsOSDisk "true"
$DataDisk = New-AzMigrateDiskMapping -DiskID $DiscoveredServer.Disk[1].Uuid -DiskType "Premium_LRS" -IsOSDisk "false"

$DisksToReplicate += $OSDisk
$DisksToReplicate += $DataDisk 

# Retrieve the resource group that you want to migrate to
$TargetResourceGroup = Get-AzResourceGroup -Name "MyTargetResourceGroup"

# Retrieve the Azure virtual network and subnet that you want to migrate to
$TargetVirtualNetwork = Get-AzVirtualNetwork -Name "MyVirtualNetwork"

# Start replication for the VM
$MigrateJob =  New-AzMigrateServerReplication -InputObject $DiscoveredServer -TargetResourceGroupId $TargetResourceGroup.ResourceId -TargetNetworkId $TargetVirtualNetwork.Id -LicenseType "NoLicenseType" -DiskToInclude $DisksToReplicate -TargetSubnetName $TargetVirtualNetwork.Subnets[0].Name -TargetVMName "MyMigratedTestVM" -TargetVMSize "Standard_DS2_v2"

# Track job status to check for completion
while (($MigrateJob.State -eq "InProgress") -or ($MigrateJob.State -eq "NotStarted")){
        #If the job hasn't completed, sleep for 10 seconds before checking the job status again
        sleep 10;
        $MigrateJob = Get-AzMigrateJob -InputObject $MigrateJob
}
#Check if the Job completed successfully. The updated job state of a successfully completed job should be "Succeeded"
Write-Output $MigrateJob.State
```

## <a name="monitor-replication"></a>Supervisión de la replicación 

La replicación se produce de la manera siguiente:

- Cuando el trabajo de inicio de replicación finaliza correctamente, las máquinas comienzan su replicación inicial en Azure.
- Durante la replicación inicial, se crea una instantánea de la máquina virtual. Los datos de disco de la instantánea se replican en los discos administrados de réplica en Azure.
- Cuando finaliza esta replicación inicial, comienza la replicación diferencial. Los cambios incrementales de los discos locales se replican periódicamente en los discos de réplica de Azure.

Realice el seguimiento del estado de la replicación con el cmdlet `Get-AzMigrateServerReplication`. 

> [!NOTE]
> El identificador de la máquina virtual detectada y el de máquina virtual de replicación son dos identificadores únicos diferentes. Ambos sirven para recuperar detalles de un servidor de replicación.  

### <a name="monitor-replication-using-discovered-vm-identifier"></a>Supervisión de la replicación con el identificador de la máquina virtual detectada
```azurepowershell
# Retrieve the replicating VM details by using the discovered VM identifier
$ReplicatingServer = Get-AzMigrateServerReplication -DiscoveredMachineId $DiscoveredServer.ID
```

### <a name="monitor-replication-using-replicating-vm-identifier"></a>Supervisión de la replicación con el identificador de la máquina virtual de replicación

```azurepowershell
# List all replicating VMs in an Azure Migrate project and filter the result for selecting the replication VM. This cmdlet will not return all properties of the replicating VM.
$ReplicatingServer = Get-AzMigrateServerReplication -ProjectName $MigrateProject.Name -ResourceGroupName $ResourceGroup.ResourceGroupName | where MachineName -eq $DiscoveredServer.DisplayName

# Retrieve replicating VM details using replicating VM identifier
$ReplicatingServer = Get-AzMigrateServerReplication -TargetObjectID $ReplicatingServer.Id 
```

Puede realizar un seguimiento de las propiedades "Migration State" y "Migration State Description" en la salida. 
- En la replicación inicial, los valores de las propiedades Migration State y Migration State Description serán "InitialSeedingInProgress" e "Initial replication" respectivamente. 
- En la replicación diferencial, los valores de las propiedades Migration State y Migration State Description serán "Replicating" y "Ready to migrate" respectivamente.
- Una vez terminada la migración, los valores de las propiedades Migration State y Migration State Description serán "Migration succeeded" y "Migrated" respectivamente.

```Output
AllowedOperation            : {DisableMigration, TestMigrate, Migrate}
CurrentJobId                : /Subscriptions/xxx/resourceGroups/xxx/providers/Micr
                              osoft.RecoveryServices/vaults/xxx/replicationJobs/None
CurrentJobName              : None
CurrentJobStartTime         : 1/1/1753 1:01:01 AM
EventCorrelationId          : 9d435c55-4660-41a5-a8ed-dd74213d85fa
Health                      : Normal
HealthError                 : {}
Id                          : /Subscriptions/xxx/resourceGroups/xxx/providers/Micr
                              osoft.RecoveryServices/vaults/xxx/replicationFabrics/xxx/replicationProtectionContainers/xxx/
                              replicationMigrationItems/10-150-8-52-b090bef3-b733-5e34-bc8f-eb6f2701432a_5009e941-3e40-
                              39b2-1e14-f90584522703
LastTestMigrationStatus     :
LastTestMigrationTime       :
Location                    :
MachineName                 : MyTestVM
MigrationState              : InitialSeedingInProgress
MigrationStateDescription   : Initial replication
Name                        : 10-150-8-52-b090bef3-b733-5e34-bc8f-eb6f2701432a_5009e941-3e40-39b2-1e14-f90584522703
PolicyFriendlyName          : xxx
PolicyId                    : /Subscriptions/xxx/resourceGroups/xxx/providers/Micr
                              osoft.RecoveryServices/vaults/xxx/replicationPolicies/xxx
ProviderSpecificDetail      : Microsoft.Azure.PowerShell.Cmdlets.Migrate.Models.Api20180110.VMwareCbtMigrationDetails
TestMigrateState            : None
TestMigrateStateDescription : None
Type                        : Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationMigrationItems
```

Para más información sobre el progreso de la replicación, ejecute el siguiente cmdlet.

```azurepowershell
$replicatingserver.ProviderSpecificDetail | convertto-json
```
Puede realizar un seguimiento del progreso de la replicación inicial mediante las propiedades "Initial Seeding Progress Percentage" en la salida.

```output
    "DataMoverRunAsAccountId": "/subscriptions/xxx/resourceGroups/xxx/providers/Microsoft.OffAzure/VMwareSites/xxx/runasaccounts/xxx",
    "FirmwareType":  "BIOS",
    "InitialSeedingProgressPercentage": 20,
    "InstanceType":  "VMwareCbt",
    "LastRecoveryPointReceived":  "\/Date(1601733591427)\/",
    "LicenseType":  "NoLicenseType",
    "MigrationProgressPercentage":  null,
    "MigrationRecoveryPointId":  null,
    "OSType":  "Windows",
    "PerformAutoResync":  "true",
```

La replicación se produce de la manera siguiente:

- Cuando el trabajo de inicio de replicación finaliza correctamente, las máquinas comienzan su replicación inicial en Azure.
- Durante la replicación inicial, se crea una instantánea de la máquina virtual. Los datos de disco de la instantánea se replican en los discos administrados de réplica en Azure.
- Cuando finaliza esta replicación inicial, comienza la replicación diferencial. Los cambios incrementales de los discos locales se replican periódicamente en los discos de réplica de Azure.

## <a name="update-properties-of-a-replicating-vm"></a>Actualización de las propiedades de una máquina virtual de replicación

[Azure Migrate:Server Migration](migrate-services-overview.md#azure-migrate-server-migration-tool) permite cambiar las propiedades de destino, como el nombre, el tamaño, el grupo de recursos, la configuración de NIC, etc., para una máquina virtual de replicación. 

```azurepowershell
# Retrieve the replicating VM details by using the discovered VM identifier
$ReplicatingServer = Get-AzMigrateServerReplication -DiscoveredMachineId $DiscoveredServer.ID

# View NIC details of the replicating server
Write-Output $ReplicatingServer.ProviderSpecificDetail.VMNic 
```
Para una máquina virtual se pueden actualizar las propiedades siguientes.

- **VM Name**: especifique el nombre de la máquina virtual de Azure que se va a crear mediante el parámetro `TargetVMName`.
- **VM size**: especifique el tamaño de máquina virtual de Azure que se usará para la replicación con el parámetro `TargetVMSize`. Por ejemplo, para migrar una máquina virtual a una máquina virtual D2_v2 de Azure, especifique el valor de `TargetVMSize` como "Standard_D2_v2".  
- **Virtual Network**: especifique el identificador de la red virtual de Azure donde debe migrar la máquina virtual mediante el parámetro `TargetNetworkId`. 
- **Resource Group**: especifique el identificador del grupo de recursos donde migrar la máquina virtual; proporciónelo con el parámetro `TargetResourceGroupId`.
- **Network Interface**: la configuración de NIC se especifica mediante el cmdlet `New-AzMigrateNicMapping`. A continuación, se pasa al objeto una entrada al parámetro `NicToUpdate` del cmdlet `Set-AzMigrateServerReplication`. 

    - **Change IP allocation**: para especificar una dirección IP estática para una NIC, proporcione la dirección IPv4 que se usará como IP estática para la máquina virtual mediante el parámetro `TargetNicIP`. Para asignar dinámicamente una dirección IP para una NIC, proporcione "auto" como valor del parámetro `TargetNicIP`.
    - Use los valores "Primary", "Secondary" o "DoNotCreate" para el parámetro `TargetNicSelectionType` con el fin de especificar si la NIC debe ser principal, secundaria o no debe crearse en la máquina virtual migrada. Solo se puede especificar una NIC como principal de la máquina virtual. 
    - Para que una NIC sea principal, también tendrá que especificar las otras NIC que se deben convertir en secundarias o que no se crearán en la máquina virtual migrada.  
    - Para cambiar la subred de la NIC, especifique el nombre de esta mediante el parámetro `TargetNicSubnet`.

 - **Availability Zone**: para usar zonas de disponibilidad, especifique el valor de zona de disponibilidad para el parámetro `TargetAvailabilityZone`.
 - **Availability Set**: para usar el conjunto de disponibilidad, especifique el identificador de este para el parámetro `TargetAvailabilitySet`.

En el ejemplo siguiente, actualizaremos la configuración de la NIC; convertiremos la primera en principal y le asignaremos una dirección IP estática. Descartaremos la segunda NIC de la migración y actualizaremos el nombre y el tamaño de la máquina virtual de destino. 

```azurepowershell
# Specify the NIC properties to be updated for a replicating VM. 
$NicMapping = @()
$NicMapping1 = New-AzMigrateNicMapping -NicId $ReplicatingServer.ProviderSpecificDetail.VMNic[0].NicId -TargetNicIP "xxx.xxx.xxx.xxx" -TargetNicSelectionType "Primary"
$NicMapping2 = New-AzMigrateNicMapping -NicId $ReplicatingServer.ProviderSpecificDetail.VMNic[1].NicId -TargetNicSelectionType "DoNotCreate"

$NicMapping += $NicMapping1
$NicMapping += $NicMapping2

# Update the name, size and NIC configuration of a replicating server
$UpdateJob = Set-AzMigrateServerReplication -InputObject $ReplicatingServer -TargetVMSize "Standard_DS13_v2" -TargetVMName "MyMigratedVM" -NicToUpdate $NicMapping
```

También puede enumerar todos los servidores de replicación en un proyecto de Azure Migrate y usar el identificador de la máquina virtual de replicación para actualizar las propiedades de la máquina virtual.

```azurepowershell
# List all replicating VMs in an Azure Migrate project and filter the result for selecting the replication VM. This cmdlet will not return all properties of the replicating VM.
$ReplicatingServer = Get-AzMigrateServerReplication -ProjectName $MigrateProject.Name -ResourceGroupName $ResourceGroup.ResourceGroupName | where MachineName -eq $DiscoveredServer.DisplayName

# Retrieve replicating VM details using replicating VM identifier
$ReplicatingServer = Get-AzMigrateServerReplication -TargetObjectID $ReplicatingServer.Id 
```


## <a name="run-a-test-migration"></a>Ejecutar una migración de prueba

Cuando comienza la replicación diferencial, puede ejecutar una migración de prueba para las máquinas virtuales antes de ejecutar una migración completa a Azure. Se recomienda encarecidamente al menos una migración de prueba en cada máquina antes de migrarla.

- Durante la ejecución de la migración de prueba se comprueba que funciona según lo previsto. La migración de prueba no afecta a la máquina local, que sigue operativa y continúa la replicación. 
- Para simular la migración, la migración de prueba crea una máquina virtual de Azure usando datos replicados (normalmente, con una migración a una red virtual que no es de producción en la suscripción a Azure).
- Puede usar la máquina virtual de Azure de prueba replicada para validar la migración, realizar pruebas de aplicaciones y resolver los problemas antes de la migración completa.

Seleccione la red virtual de Azure que se va a usar para las pruebas; para ello, especifique el identificador de la red virtual mediante el parámetro `TestNetworkID`.

```azurepowershell
# Retrieve the Azure virtual network created for testing 
$TestVirtualNetwork = Get-AzVirtualNetwork -Name MyTestVirtualNetwork

# Start test migration for a replicating server
$TestMigrationJob = Start-AzMigrateTestMigration -InputObject $ReplicatingServer -TestNetworkID $TestVirtualNetwork.Id
```

Una vez completadas las pruebas, limpie la migración de prueba con el cmdlet `Start-AzMigrateTestMigrationCleanup`.

```azurepowershell
# Clean-up test migration for a replicating server
$CleanupTestMigrationJob = Start-AzMigrateTestMigrationCleanup -InputObject $ReplicatingServer
```

## <a name="migrate-vms"></a>Migración de máquinas virtuales

Después de comprobar que la migración de prueba funciona según lo previsto, puede migrar el servidor de replicación con el cmdlet siguiente.

```azurepowershell
# Start migration for a replicating server and turn off source server as part of migration
$MigrateJob = Start-AzMigrateServerMigration -InputObject $ReplicatingServer -TurnOffSourceServer 
```
Si no desea desactivar el servidor de origen, no utilice el parámetro `TurnOffSourceServer`.

## <a name="complete-the-migration"></a>Completar la migración

1. Una vez finalizada la migración, detenga la replicación de la máquina local y limpie la información del estado de replicación de la máquina virtual con el siguiente cmdlet.

```azurepowershell
# Stop replication for a migrated server
$StopReplicationJob = Remove-AzMigrateServerReplication -InputObject $ReplicatingServer 
```

2. Instale el agente de [Windows](../virtual-machines/extensions/agent-windows.md) o [Linux](../virtual-machines/extensions/agent-linux.md) de la máquina virtual de Azure en las máquinas migradas.
3. Realice los ajustes de la aplicación posteriores a la migración, como actualizar las cadenas de conexión de la base de datos y las configuraciones del servidor web.
4. Realice las pruebas finales de la aplicación y la aceptación de la migración en la aplicación migrada que ahora se ejecuta en Azure.
5. Pase el tráfico a la instancia de máquina virtual de Azure migrada.
6. Quite las máquinas virtuales locales del inventario de máquinas virtuales local.
7. Quite las máquinas virtuales locales de las copias de seguridad locales.
8. Actualice la documentación interna para mostrar la nueva ubicación y la dirección IP las máquinas virtuales de Azure. 

## <a name="post-migration-best-practices"></a>Procedimientos recomendados después de la migración

- Para aumentar la resistencia:
    - Proteja los datos mediante la copia de seguridad de máquinas virtuales de Azure mediante el servicio Azure Backup. [Más información](../backup/quick-backup-vm-portal.md).
    - Mantenga las cargas de trabajo en ejecución y disponibles continuamente mediante la replicación de máquinas virtuales de Azure en una región secundaria con Site Recovery. [Más información](../site-recovery/azure-to-azure-tutorial-enable-replication.md).
- Para aumentar la seguridad:
    - Bloquee y limite el acceso de tráfico de entrada con la [administración Just-In-Time de Azure Security Center](../security-center/security-center-just-in-time.md).
    - Restrinja el tráfico de red a los puntos de conexión de administración con [grupos de seguridad de red](../virtual-network/security-overview.md).
    - Implemente [Azure Disk Encryption](../security/fundamentals/azure-disk-encryption-vms-vmss.md) para ayudar a proteger discos y datos frente al robo y acceso no autorizado.
    - Obtenga más información sobre la [protección de recursos IaaS](https://azure.microsoft.com/services/virtual-machines/secure-well-managed-iaas/) y visite [Azure Security Center](https://azure.microsoft.com/services/security-center/).
- Para supervisión y administración:
-  Considere la posibilidad de implementar [Azure Cost Management](../cost-management-billing/cloudyn/overview.md) para supervisar el gasto y el uso de recursos.




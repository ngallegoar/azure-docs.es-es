---
title: Replicación de máquinas virtuales de Azure que se ejecutan en grupos con ubicación por proximidad
description: Obtenga información sobre cómo replicar máquinas virtuales de Azure mediante la ejecución en grupos con ubicación por proximidad con Azure Site Recovery.
author: Sharmistha-Rai
manager: gaggupta
ms.topic: how-to
ms.date: 05/25/2020
ms.openlocfilehash: 1a9e38d2718643212848105e09c60d93f497a34a
ms.sourcegitcommit: 1f48ad3c83467a6ffac4e23093ef288fea592eb5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/29/2020
ms.locfileid: "84193546"
---
# <a name="replicate-azure-virtual-machines-running-in-proximity-placement-groups-to-another-region"></a>Replicación de máquinas virtuales de Azure que se ejecutan en grupos con ubicación por proximidad en otra región

En este artículo se describe cómo replicar, conmutar por error y por recuperación máquinas virtuales que se ejecutan en un grupo con ubicación por proximidad en una región secundaria.

[Grupos con ubicación por proximidad](https://docs.microsoft.com/azure/virtual-machines/windows/proximity-placement-groups-portal) es una funcionalidad de agrupación lógica de máquinas virtuales de Azure que se puede usar para reducir la latencia de red entre máquinas virtuales asociada a las aplicaciones. Cuando las máquinas virtuales se implementan en el mismo grupo con ubicación por proximidad, se ubican físicamente lo más cerca posible entre sí. Los grupos con ubicación por proximidad son especialmente útiles para satisfacer los requisitos de las cargas de trabajo sensibles a la latencia.

## <a name="disaster-recovery-with-proximity-placement-groups"></a>Recuperación ante desastres con grupos con ubicación por proximidad

En un escenario típico, puede hacer que las máquinas virtuales que se ejecutan en un grupo con ubicación por proximidad eviten la latencia de red entre los distintos niveles de la aplicación. Aunque esto puede proporcionar a la aplicación una latencia de red óptima, le gustaría proteger estas aplicaciones mediante Site Recovery en caso de errores a nivel regional. Site Recovery replica los datos de una región de Azure en otra, y activa las máquinas en la región de recuperación ante desastres en caso de conmutación por error.

## <a name="considerations"></a>Consideraciones

- El mejor esfuerzo consistirá en realizar la conmutación por error o recuperación de las máquinas virtuales en un grupo con ubicación por proximidad. Pero si la máquina virtual no se puede activar dentro de la ubicación de proximidad durante la conmutación por error o por recuperación, se seguirá produciendo la conmutación por error o por recuperación, y las máquinas virtuales se crearán fuera de un grupo con ubicación por proximidad.
-  Si un conjunto de disponibilidad está anclado a un grupo con ubicación por proximidad y durante la conmutación por error o recuperación las máquinas virtuales del conjunto de disponibilidad tienen una restricción de asignación, se crearán fuera del grupo con ubicación por proximidad y del conjunto de disponibilidad.
-  Para los discos administrados no se permite Site Recovery para grupos con ubicación por proximidad.

> [!Note]
> Azure Site Recovery no admite la conmutación por recuperación desde discos administrados para escenarios de Hyper-V a Azure. Por tanto, no se admite la conmutación por recuperación desde grupos con ubicación por proximidad de Azure a Hyper-V.

## <a name="prerequisites"></a>Requisitos previos

1. Asegúrese de que tiene del módulo Az de Azure PowerShell. Si necesita instalar o actualizar Azure PowerShell, siga esta [Guía para instalar y configurar Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-az-ps).

## <a name="set-up-site-recovery-for-virtual-machines-in-proximity-placement-group"></a>Configuración de Site Recovery para Virtual Machines en un grupo con ubicación por proximidad

### <a name="azure-to-azure"></a>De Azure a Azure

1. [Inicie sesión](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-powershell#sign-in-to-your-microsoft-azure-subscription) en la cuenta y establezca la suscripción.
2. Obtenga los detalles de la máquina virtual que va a replicar tal y como se ha mencionado [aquí](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-powershell#get-details-of-the-virtual-machine-to-be-replicated).
3. [Cree](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-powershell#create-a-recovery-services-vault) el almacén de Recovery Services y [establezca](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-powershell#set-the-vault-context) su contexto.
4. Prepare el almacén para iniciar la replicación de máquinas virtuales. Esto implica la creación de un [objeto de Service Fabric](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-powershell#create-a-site-recovery-fabric-object-to-represent-the-primary-source-region) para la región principal y la de recuperación.
5. [Cree](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-powershell#create-a-site-recovery-protection-container-in-the-primary-fabric) un contenedor de protección de Site Recovery, para el tejido principal y el de recuperación.
6. [Cree](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-powershell#create-a-replication-policy) una directiva de replicación.
7. Cree una asignación de contenedor de protección entre el contenedor de protección principal y el de recuperación mediante [estos](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-powershell#create-a-protection-container-mapping-between-the-primary-and-recovery-protection-container) pasos y una asignación de contenedor de protección para la conmutación por recuperación, como se ha mencionado [aquí](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-powershell#create-a-protection-container-mapping-for-failback-reverse-replication-after-a-failover).
8. Siga [estos](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-powershell#create-cache-storage-account-and-target-storage-account) pasos para crear una cuenta de almacenamiento en caché.
9. Cree las asignaciones de red necesarias como se ha mencionado [aquí](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-powershell#create-network-mappings).
10. Para replicar la máquina virtual de Azure con discos administrados, use el siguiente cmdlet de PowerShell: 

```azurepowershell
#Get the resource group that the virtual machine must be created in when failed over.
$RecoveryRG = Get-AzResourceGroup -Name "a2ademorecoveryrg" -Location "West US 2"

#Specify replication properties for each disk of the VM that is to be replicated (create disk replication configuration)

#OsDisk
$OSdiskId = $vm.StorageProfile.OsDisk.ManagedDisk.Id
$RecoveryOSDiskAccountType = $vm.StorageProfile.OsDisk.ManagedDisk.StorageAccountType
$RecoveryReplicaDiskAccountType = $vm.StorageProfile.OsDisk.ManagedDisk.StorageAccountType

$OSDiskReplicationConfig = New-AzRecoveryServicesAsrAzureToAzureDiskReplicationConfig -ManagedDisk -LogStorageAccountId $EastUSCacheStorageAccount.Id ` -DiskId $OSdiskId -RecoveryResourceGroupId  $RecoveryRG.ResourceId -RecoveryReplicaDiskAccountType  $RecoveryReplicaDiskAccountType ` -RecoveryTargetDiskAccountType $RecoveryOSDiskAccountType -RecoveryProximityPlacementGroupId $recPpg.Id

# Data disk
$datadiskId1 = $vm.StorageProfile.DataDisks[0].ManagedDisk.Id
$RecoveryReplicaDiskAccountType = $vm.StorageProfile.DataDisks[0].ManagedDisk.StorageAccountType
$RecoveryTargetDiskAccountType = $vm.StorageProfile.DataDisks[0].ManagedDisk.StorageAccountType

$DataDisk1ReplicationConfig  = New-AzRecoveryServicesAsrAzureToAzureDiskReplicationConfig -ManagedDisk -LogStorageAccountId $CacheStorageAccount.Id ` -DiskId $datadiskId1 -RecoveryResourceGroupId $RecoveryRG.ResourceId -RecoveryReplicaDiskAccountType $RecoveryReplicaDiskAccountType ` -RecoveryTargetDiskAccountType $RecoveryTargetDiskAccountType

#Create a list of disk replication configuration objects for the disks of the virtual machine that are to be replicated.

$diskconfigs = @()
$diskconfigs += $OSDiskReplicationConfig, $DataDisk1ReplicationConfig

#Start replication by creating replication protected item. Using a GUID for the name of the replication protected item to ensure uniqueness of name.

$TempASRJob = New-AzRecoveryServicesAsrReplicationProtectedItem -AzureToAzure -AzureVmId $VM.Id -Name (New-Guid).Guid -ProtectionContainerMapping $EusToWusPCMapping -AzureToAzureDiskReplicationConfiguration $diskconfigs -RecoveryResourceGroupId $RecoveryRG.ResourceId -RecoveryProximityPlacementGroupId $recPpg.Id
```
Una vez que la operación de replicación inicial se realiza correctamente, los datos de la máquina virtual se replican en la región de recuperación.

El proceso de replicación comienza con la propagación inicial de una copia de los discos de replicación de la máquina virtual en la región de recuperación. Esta fase se conoce como fase de replicación inicial.

Después de que se complete la replicación inicial, la replicación pasa a la fase de sincronización diferencial. En este momento, la máquina virtual está protegida y se puede realizar una operación de conmutación por error de prueba en ella. El estado de replicación del elemento replicado que representa la máquina virtual pasa al estado Protegido después de que finalice la replicación inicial.

Para supervisar el estado y el mantenimiento de la replicación en la máquina virtual, puede obtener los detalles correspondientes al elemento protegido de la replicación. 

```azurepowershell
Get-AzRecoveryServicesAsrReplicationProtectedItem -ProtectionContainer $PrimaryProtContainer | Select FriendlyName, ProtectionState, ReplicationHealth
```

11. Para realizar una conmutación por error de prueba, una validación y limpiar la conmutación por error de prueba, siga [estos](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-powershell#do-a-test-failover-validate-and-cleanup-test-failover) pasos.
12. Para la conmutación por error, siga los pasos descritos [aquí](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-powershell#fail-over-to-azure).
13. Para volver a proteger y realizar la conmutación por recuperación en la región de origen, use el siguiente cmdlet de PowerShell:

```azurepowershell
#Create Cache storage account for replication logs in the primary region
$WestUSCacheStorageAccount = New-AzStorageAccount -Name "a2acachestoragewestus" -ResourceGroupName "A2AdemoRG" -Location 'West US' -SkuName Standard_LRS -Kind Storage


#Use the recovery protection container, new cache storage account in West US and the source region VM resource group 
Update-AzRecoveryServicesAsrProtectionDirection -ReplicationProtectedItem $ReplicationProtectedItem -AzureToAzure -ProtectionContainerMapping $WusToEusPCMapping -LogStorageAccountId $WestUSCacheStorageAccount.Id -RecoveryResourceGroupID $sourceVMResourcegroup.ResourceId -RecoveryProximityPlacementGroupId $vm.ProximityPlacementGroup.Id
```
14. Para deshabilitar la replicación, siga [estos](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-powershell#disable-replication) pasos.

### <a name="vmware-to-azure"></a>De VMware a Azure

1. Asegúrese de [preparar los servidores de VMware locales](https://docs.microsoft.com/azure/site-recovery/vmware-azure-tutorial-prepare-on-premises) para la recuperación ante desastres en Azure.
2. Inicie sesión en la cuenta y establezca la suscripción como se ha especificado [aquí](https://docs.microsoft.com/azure/site-recovery/vmware-azure-disaster-recovery-powershell#log-into-azure).
3. [Configure](https://docs.microsoft.com/azure/site-recovery/vmware-azure-disaster-recovery-powershell#set-up-a-recovery-services-vault) un almacén de Recovery Services y [establezca su contexto](https://docs.microsoft.com/azure/site-recovery/vmware-azure-disaster-recovery-powershell#set-the-vault-context).
4. [Valide](https://docs.microsoft.com/azure/site-recovery/vmware-azure-disaster-recovery-powershell#validate-vault-registration) el registro del almacén.
5. [Cree](https://docs.microsoft.com/azure/site-recovery/vmware-azure-disaster-recovery-powershell#create-a-replication-policy) una directiva de replicación.
6. [Agregue](https://docs.microsoft.com/azure/site-recovery/vmware-azure-disaster-recovery-powershell#add-a-vcenter-server-and-discover-vms) un servidor vCenter y detecte máquinas virtuales, y [cree](https://docs.microsoft.com/azure/site-recovery/vmware-azure-disaster-recovery-powershell#create-storage-accounts-for-replication) cuentas de almacenamiento para la replicación.
7. Para replicar máquinas virtuales de VMware, consulte los detalles aquí y siga el siguiente cmdlet de PowerShell:

```azurepowershell
#Get the target resource group to be used
$ResourceGroup = Get-AzResourceGroup -Name "VMwareToAzureDrPs"

#Get the target virtual network to be used
$RecoveryVnet = Get-AzVirtualNetwork -Name "ASR-vnet" -ResourceGroupName "asrrg" 

#Get the protection container mapping for replication policy named ReplicationPolicy
$PolicyMap = Get-AzRecoveryServicesAsrProtectionContainerMapping -ProtectionContainer $ProtectionContainer | where PolicyFriendlyName -eq "ReplicationPolicy"

#Get the protectable item corresponding to the virtual machine CentOSVM1
$VM1 = Get-AzRecoveryServicesAsrProtectableItem -ProtectionContainer $ProtectionContainer -FriendlyName "CentOSVM1"

# Enable replication for virtual machine CentOSVM1 using the Az.RecoveryServices module 2.0.0 onwards to replicate to managed disks
# The name specified for the replicated item needs to be unique within the protection container. Using a random GUID to ensure uniqueness
$Job_EnableReplication1 = New-AzRecoveryServicesAsrReplicationProtectedItem -VMwareToAzure -ProtectableItem $VM1 -Name (New-Guid).Guid -ProtectionContainerMapping $PolicyMap -ProcessServer $ProcessServers[1] -Account $AccountHandles[2] -RecoveryResourceGroupId $ResourceGroup.ResourceId -logStorageAccountId $LogStorageAccount.Id -RecoveryAzureNetworkId $RecoveryVnet.Id -RecoveryAzureSubnetName "Subnet-1" -RecoveryProximityPlacementGroupId $recPpg.Id
```
8. Puede comprobar el estado de replicación y el mantenimiento de replicación de la máquina virtual con el cmdlet Get-ASRReplicationProtectedItem.

```azurepowershell
Get-AzRecoveryServicesAsrReplicationProtectedItem -ProtectionContainer $ProtectionContainer | Select FriendlyName, ProtectionState, ReplicationHealth
```
9. Para configurar los valores de conmutación por error, siga [estos](https://docs.microsoft.com/azure/site-recovery/vmware-azure-disaster-recovery-powershell#configure-failover-settings) pasos.
10. [Ejecute](https://docs.microsoft.com/azure/site-recovery/vmware-azure-disaster-recovery-powershell#run-a-test-failover) una conmutación por error de prueba. 
11. Siga [estos](https://docs.microsoft.com/azure/site-recovery/vmware-azure-disaster-recovery-powershell#fail-over-to-azure) pasos para conmutar por error a Azure.

### <a name="hyper-v-to-azure"></a>De Hyper-V a Azure

1. Asegúrese de [preparar los servidores de Hyper-V locales](https://docs.microsoft.com/azure/site-recovery/hyper-v-prepare-on-premises-tutorial) para la recuperación ante desastres en Azure.
2. [Inicie sesión](https://docs.microsoft.com/azure/site-recovery/hyper-v-azure-powershell-resource-manager#step-1-sign-in-to-your-azure-account) en Azure.
3. [Configure](https://docs.microsoft.com/azure/site-recovery/hyper-v-azure-powershell-resource-manager#step-2-set-up-the-vault) el almacén de Recovery Services y [establezca](https://docs.microsoft.com/azure/site-recovery/hyper-v-azure-powershell-resource-manager#step-3-set-the-recovery-services-vault-context) su contexto.
4. [Cree](https://docs.microsoft.com/azure/site-recovery/hyper-v-azure-powershell-resource-manager#step-4-create-a-hyper-v-site) un sitio de Hyper-V.
5. [Instale](https://docs.microsoft.com/azure/site-recovery/hyper-v-azure-powershell-resource-manager#step-5-install-the-provider-and-agent) el proveedor y el agente.
6. [Cree](https://docs.microsoft.com/azure/site-recovery/hyper-v-azure-powershell-resource-manager#step-6-create-a-replication-policy) una directiva de replicación.
7. Siga estos pasos para habilitar la replicación: 
    
    a. Recupere el elemento protegible correspondiente a la máquina virtual que quiere proteger, como se indica a continuación:

    ```azurepowershell
    $VMFriendlyName = "Fabrikam-app"          #Name of the VM
    $ProtectableItem = Get-AzRecoveryServicesAsrProtectableItem -ProtectionContainer $protectionContainer -FriendlyName $VMFriendlyName
    ```
    b. Proteja la máquina virtual. Si la VM que se va a proteger tiene más de un disco conectado, especifique el disco del sistema operativo con el parámetro OSDiskName.
    
    ```azurepowershell
    $OSType = "Windows"          # "Windows" or "Linux"
    $DRjob = New-AzRecoveryServicesAsrReplicationProtectedItem -ProtectableItem $VM -Name $VM.Name -ProtectionContainerMapping $ProtectionContainerMapping -RecoveryAzureStorageAccountId   $StorageAccountID -OSDiskName $OSDiskNameList[$i] -OS $OSType -RecoveryResourceGroupId $ResourceGroupID -RecoveryProximityPlacementGroupId $recPpg.Id
    ```
    c. Espere a que las máquinas virtuales lleguen a un estado protegido después de la replicación inicial. Esta operación puede tardar unos minutos en función de factores como el volumen de datos que se van a replicar y el ancho de banda del canal de subida disponible en Azure. Cuando se alcanza un estado protegido, los valores State y StateDescription del trabajo se actualizan de esta forma: 
    
    ```azurepowershell
    $DRjob = Get-AzRecoveryServicesAsrJob -Job $DRjob
    $DRjob | Select-Object -ExpandProperty State

    $DRjob | Select-Object -ExpandProperty StateDescription
    ```
    d. Actualice las propiedades de recuperación (como el tamaño de rol de la VM) y la red de Azure a la que se va a asociar el adaptador de red de la VM tras la conmutación por error.

    ```azurepowershell
    $nw1 = Get-AzVirtualNetwork -Name "FailoverNw" -ResourceGroupName "MyRG"

    $VMFriendlyName = "Fabrikam-App"

    $rpi = Get-AzRecoveryServicesAsrReplicationProtectedItem -ProtectionContainer $protectionContainer -FriendlyName $VMFriendlyName

    $UpdateJob = Set-AzRecoveryServicesAsrReplicationProtectedItem -InputObject $rpi -PrimaryNic $VM.NicDetailsList[0].NicId -RecoveryNetworkId $nw1.Id -RecoveryNicSubnetName $nw1.Subnets[0].Name

    $UpdateJob = Get-AzRecoveryServicesAsrJob -Job $UpdateJob

    $UpdateJob | Select-Object -ExpandProperty state

    Get-AzRecoveryServicesAsrJob -Job $job | Select-Object -ExpandProperty state
    ```
8. Ejecute una [conmutación por error](https://docs.microsoft.com/azure/site-recovery/hyper-v-azure-powershell-resource-manager#step-8-run-a-test-failover) de prueba.


## <a name="next-steps"></a>Pasos siguientes

Para realizar la reprotección y la conmutación por recuperación de VMware en Azure, siga [estos](https://docs.microsoft.com/azure/site-recovery/vmware-azure-prepare-failback) pasos.

Para realizar la conmutación por error de Hyper-V a Azure, siga [estos](https://docs.microsoft.com/azure/site-recovery/site-recovery-failover) pasos y, para la conmutación por recuperación, [estos](https://docs.microsoft.com/azure/site-recovery/hyper-v-azure-failback).

Para obtener más información, vea [Conmutación por error en Site Recovery](site-recovery-failover.md).

---
title: archivo de inclusión
description: archivo de inclusión
services: virtual-machines
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 11/14/2019
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: 0d081a8cec088f4743bd0dc7d3cc37a9fade61d1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "80117145"
---
Los discos Ultra de Azure ofrecen un alto rendimiento, IOPS elevadas y un almacenamiento en disco coherente y de baja latencia para máquinas virtuales IaaS de Azure. En esta nueva oferta se proporciona un rendimiento exclusivo que se encuentra en los mismos niveles de disponibilidad que nuestras ofertas de discos existentes. Una ventaja importante de los discos Ultra es la posibilidad de cambiar dinámicamente el rendimiento del disco SSD junto con sus cargas de trabajo sin tener que reiniciar las máquinas virtuales. Además, los discos Ultra son adecuados para cargas de trabajo con grandes cantidades de datos, como SAP HANA, bases de datos de nivel superior y cargas de trabajo que admitan muchas transacciones.

## <a name="ga-scope-and-limitations"></a>Ámbito y limitaciones de la disponibilidad general

[!INCLUDE [managed-disks-ultra-disks-GA-scope-and-limitations](managed-disks-ultra-disks-GA-scope-and-limitations.md)]

## <a name="determine-vm-size-and-region-availability"></a>Determinar el tamaño de la máquina virtual y la disponibilidad por región

Para aprovechar los discos Ultra, debe determinar en qué zona de disponibilidad se encuentra. No todas las regiones admiten todos los tamaños de máquina virtual con discos Ultra. Para determinar si la región, la zona y el tamaño de la máquina virtual admiten discos Ultra, ejecute cualquiera de los siguientes comandos y asegúrese de reemplazar primero los valores **region**, **vmSize** y **subscription**:

CLI:

```azurecli
$subscription = "<yourSubID>"
# example value is southeastasia
$region = "<yourLocation>"
# example value is Standard_E64s_v3
$vmSize = "<yourVMSize>"

az vm list-skus --resource-type virtualMachines  --location $region --query "[?name=='$vmSize'].locationInfo[0].zoneDetails[0].Name" --subscription $subscription
```

PowerShell:

```powershell
$region = "southeastasia"
$vmSize = "Standard_E64s_v3"
(Get-AzComputeResourceSku | where {$_.Locations.Contains($region) -and ($_.Name -eq $vmSize) -and $_.LocationInfo[0].ZoneDetails.Count -gt 0})[0].LocationInfo[0].ZoneDetails
```

La respuesta será similar al formulario siguiente, donde X es la zona que se utilizará para la implementación en la región elegida. X podría ser 1, 2 o 3.

Conserve el valor de **Zones**, ya que representa la zona de disponibilidad y la necesitará para implementar un disco Ultra.

|ResourceType  |Nombre  |Location  |Zones  |Restricción  |Capacidad  |Value  |
|---------|---------|---------|---------|---------|---------|---------|
|disks     |UltraSSD_LRS         |eastus2         |X         |         |         |         |

> [!NOTE]
> Si no ha habido respuesta del comando, el tamaño de máquina virtual seleccionado no es compatible con los discos Ultra en la región seleccionada.

Ahora que sabe en qué zona se va a realizar la implementación, siga los pasos de implementación de este artículo para implementar una máquina virtual con un disco Ultra conectado o conectar un disco Ultra a una máquina virtual existente.

## <a name="deploy-an-ultra-disk-using-azure-resource-manager"></a>Implementación de un disco Ultra con Azure Resource Manager

En primer lugar, determine el tamaño de la máquina virtual que se va a implementar. Para obtener una lista de los tamaños de máquina virtual admitidos, vea la sección [Ámbito y limitaciones de la disponibilidad general](#ga-scope-and-limitations).

Si quiere crear una máquina virtual con varios discos Ultra, vea el ejemplo [Creación de una máquina virtual con varios discos Ultra](https://aka.ms/ultradiskArmTemplate).

Si piensa usar su propia plantilla, asegúrese de que **apiVersion** para `Microsoft.Compute/virtualMachines` y `Microsoft.Compute/Disks` se establece como `2018-06-01` (o posterior).

Establezca la SKU de disco en **UltraSSD_LRS**, luego establezca la capacidad de disco, IOPS, zona de disponibilidad y rendimiento en MBps para crear un disco ultra.

Cuando se aprovisiona la máquina virtual, puede realizar una partición y dar formato a los discos de datos y configurarlos para las cargas de trabajo.


## <a name="deploy-an-ultra-disk-using-the-azure-portal"></a>Implementación de disco Ultra mediante Azure Portal

En esta sección se describe la implementación de una máquina virtual equipada con un disco Ultra como disco de datos. Se supone que está familiarizado con la implementación de una máquina virtual; si no lo está, consulte [Inicio rápido: crear una máquina virtual de Windows en Azure Portal](../articles/virtual-machines/windows/quick-create-portal.md).

- Inicie sesión en [Azure Portal](https://portal.azure.com/) y navegue hasta Implementar una máquina virtual (VM).
- Asegúrese de elegir un [tamaño y región compatibles de máquina virtual](#ga-scope-and-limitations).
- Seleccione **Zona de disponibilidad** en **Opciones de disponibilidad**.
- Rellene las entradas restantes con las selecciones de su elección.
- Seleccione **Discos**.

![create-ultra-disk-enabled-vm.png](media/virtual-machines-disks-getting-started-ultra-ssd/create-ultra-disk-enabled-vm.png)

- En la hoja Discos, seleccione **Sí** para **Habilitar compatibilidad con Ultra Disks**.
- Seleccione **Crear y adjuntar un nuevo disco** para conectar un disco Ultra ahora.

![enable-and-attach-ultra-disk.png](media/virtual-machines-disks-getting-started-ultra-ssd/enable-and-attach-ultra-disk.png)

- En la hoja **Crear un disco**, escriba un nombre y, a continuación, seleccione **Cambiar tamaño**.
- Cambie **Tipo de cuenta** a **Disco Ultra**.
- Cambie los valores de **Tamaño de disco personalizado (GiB)** , **IOPS del disco** y **Rendimiento de disco** por los de su elección.
- Seleccione **Aceptar** en ambas hojas.
- Continúe con la implementación de la máquina virtual, será igual a cómo implementaría cualquier otra máquina virtual.

![create-ultra-disk.png](media/virtual-machines-disks-getting-started-ultra-ssd/create-ultra-disk.png)

## <a name="attach-an-ultra-disk-using-the-azure-portal"></a>Adjuntar un disco Ultra mediante Azure Portal

Como alternativa, si la máquina virtual existente se encuentra en una región o zona de disponibilidad que puede usar discos Ultra, puede usar este tipo de discos sin tener que crear una nueva máquina virtual. Para ello, habilite los discos Ultra en la máquina virtual existente y, a continuación, asócielos como discos de datos.

- Navegue hasta la máquina virtual y seleccione **Discos**.
- Seleccione **Editar**.

![options-selector-ultra-disks.png](media/virtual-machines-disks-getting-started-ultra-ssd/options-selector-ultra-disks.png)

- Seleccione **Sí** para **Habilitar compatibilidad con Ultra Disks**.

![ultra-options-yes-enable.png](media/virtual-machines-disks-getting-started-ultra-ssd/ultra-options-yes-enable.png)

- Seleccione **Guardar**.
- Seleccione **Agregar disco de datos** y, en la lista desplegable, para **Nombre** seleccione **Crear disco**.

![create-and-attach-new-ultra-disk.png](media/virtual-machines-disks-getting-started-ultra-ssd/create-and-attach-new-ultra-disk.png)

- Escriba un nombre para el nuevo disco y, a continuación, seleccione **Cambiar el tamaño**.
- Cambie **Tipo de cuenta** a **Disco Ultra**.
- Cambie los valores de **Tamaño de disco personalizado (GiB)** , **IOPS del disco** y **Rendimiento de disco** por los de su elección.
- Seleccione **Aceptar** y después **Crear**.

![making-a-new-ultra-disk.png](media/virtual-machines-disks-getting-started-ultra-ssd/making-a-new-ultra-disk.png)

- Cuando vuelva a la hoja del disco, seleccione **Guardar**.

![saving-and-attaching-new-ultra-disk.png](media/virtual-machines-disks-getting-started-ultra-ssd/saving-and-attaching-new-ultra-disk.png)

### <a name="adjust-the-performance-of-an-ultra-disk-using-the-azure-portal"></a>Ajustar del rendimiento de un disco Ultra mediante Azure Portal

Los discos Ultra ofrecen una capacidad única que permite ajustar su rendimiento. Puede realizar estos ajustes desde Azure Portal, en los propios discos.

- Navegue hasta la máquina virtual y seleccione **Discos**.
- Seleccione el disco Ultra en el que desea modificar el rendimiento.

![selecting-ultra-disk-to-modify.png](media/virtual-machines-disks-getting-started-ultra-ssd/selecting-ultra-disk-to-modify.png)

- Seleccione **Configuración** y, a continuación, realice las modificaciones.
- Seleccione **Guardar**.

![configuring-ultra-disk-performance-and-size.png](media/virtual-machines-disks-getting-started-ultra-ssd/configuring-ultra-disk-performance-and-size.png)

## <a name="deploy-an-ultra-disk-using-cli"></a>Implementación de un disco Ultra mediante la CLI

En primer lugar, determine el tamaño de la máquina virtual que se va a implementar. Vea la sección [Ámbito y limitaciones de la disponibilidad general](#ga-scope-and-limitations) para obtener una lista de los tamaños de máquina virtual admitidos.

Para conectar un disco Ultra, debe crear una máquina virtual que pueda usar discos Ultra.

Reemplace o establezca las variables **$vmname**, **$rgname**, **$diskname**, **$location**, **$password** y **$user** con sus propios valores. Establezca **$zone** en el valor de la zona de disponibilidad que ha obtenido al [inicio de este artículo](#determine-vm-size-and-region-availability). Después, ejecute el siguiente comando de la CLI para crear una máquina virtual habilitada para ultra:

```azurecli-interactive
az vm create --subscription $subscription -n $vmname -g $rgname --image Win2016Datacenter --ultra-ssd-enabled true --zone $zone --authentication-type password --admin-password $password --admin-username $user --size Standard_D4s_v3 --location $location
```

### <a name="create-an-ultra-disk-using-cli"></a>Creación de un disco Ultra mediante la CLI

Ahora que ya tiene una máquina virtual en la que se pueden colocar discos Ultra, puede crear un disco Ultra y conectarlo a dicha máquina virtual.

```azurecli-interactive
$location="eastus2"
$subscription="xxx"
$rgname="ultraRG"
$diskname="ssd1"
$vmname="ultravm1"
$zone=123

#create an ultra disk
az disk create `
--subscription $subscription `
-n $diskname `
-g $rgname `
--size-gb 4 `
--location $location `
--zone $zone `
--sku UltraSSD_LRS `
--disk-iops-read-write 1000 `
--disk-mbps-read-write 50
```

## <a name="attach-an-ultra-disk-to-a-vm-using-cli"></a>Conexión de un disco Ultra a una máquina virtual mediante la CLI

Como alternativa, si la máquina virtual existente se encuentra en una región o zona de disponibilidad que puede usar discos Ultra, puede usar este tipo de discos sin tener que crear una nueva máquina virtual.

```azurecli
$rgName = "<yourResourceGroupName>"
$vmName = "<yourVMName>"
$diskName = "<yourDiskName>"
$subscriptionId = "<yourSubscriptionID>"

az vm disk attach -g $rgName --vm-name $vmName --disk $diskName --subscription $subscriptionId
```

### <a name="adjust-the-performance-of-an-ultra-disk-using-cli"></a>Ajuste del rendimiento de un disco Ultra mediante la CLI

Los discos Ultra ofrecen una capacidad única que permite ajustar su rendimiento. El comando siguiente describe cómo se usa esta característica:

```azurecli-interactive
az disk update `
--subscription $subscription `
--resource-group $rgname `
--name $diskName `
--set diskIopsReadWrite=80000 `
--set diskMbpsReadWrite=800
```

## <a name="deploy-an-ultra-disk-using-powershell"></a>Implementación de un disco Ultra mediante PowerShell

En primer lugar, determine el tamaño de la máquina virtual que se va a implementar. Vea la sección [Ámbito y limitaciones de la disponibilidad general](#ga-scope-and-limitations) para obtener una lista de los tamaños de máquina virtual admitidos.

Para usar discos Ultra, debe crear una máquina virtual que sea capaz de usar discos Ultra. Reemplace o establezca las variables **$resourcegroup** y **$vmName** con sus propios valores. Establezca **$zone** en el valor de la zona de disponibilidad que ha obtenido al [inicio de este artículo](#determine-vm-size-and-region-availability). Después, ejecute el siguiente comando de [New-AzVm](/powershell/module/az.compute/new-azvm) para crear una máquina virtual ultra habilitada:

```powershell
New-AzVm `
    -ResourceGroupName $resourcegroup `
    -Name $vmName `
    -Location "eastus2" `
    -Image "Win2016Datacenter" `
    -EnableUltraSSD `
    -size "Standard_D4s_v3" `
    -zone $zone
```

### <a name="create-an-ultra-disk-using-powershell"></a>Creación de un disco Ultra mediante PowerShell

Ahora que ya tiene una máquina virtual que puede usar discos Ultra, puede crear un disco Ultra y conectarlo a dicha máquina virtual:

```powershell
$diskconfig = New-AzDiskConfig `
-Location 'EastUS2' `
-DiskSizeGB 8 `
-DiskIOPSReadWrite 1000 `
-DiskMBpsReadWrite 100 `
-AccountType UltraSSD_LRS `
-CreateOption Empty `
-zone $zone;

New-AzDisk `
-ResourceGroupName $resourceGroup `
-DiskName 'Disk02' `
-Disk $diskconfig;
```

## <a name="attach-an-ultra-disk-to-a-vm-using-powershell"></a>Conexión de un disco Ultra a una máquina virtual mediante PowerShell

Como alternativa, si la máquina virtual existente se encuentra en una región o zona de disponibilidad que puede usar discos Ultra, puede usar este tipo de discos sin tener que crear una nueva máquina virtual.

```powershell
# add disk to VM
$subscription = "<yourSubscriptionID>"
$resourceGroup = "<yourResourceGroup>"
$vmName = "<yourVMName>"
$diskName = "<yourDiskName>"
$lun = 1
Login-AzureRMAccount -SubscriptionId $subscription
$vm = Get-AzVM -ResourceGroupName $resourceGroup -Name $vmName
$disk = Get-AzDisk -ResourceGroupName $resourceGroup -Name $diskName
$vm = Add-AzVMDataDisk -VM $vm -Name $diskName -CreateOption Attach -ManagedDiskId $disk.Id -Lun $lun
Update-AzVM -VM $vm -ResourceGroupName $resourceGroup
```

### <a name="adjust-the-performance-of-an-ultra-disk-using-powershell"></a>Ajuste del rendimiento de un disco Ultra mediante PowerShell

Los discos Ultra tienen una capacidad única que permite ajustar su rendimiento. El comando siguiente es un ejemplo que ajusta el rendimiento sin tener que desasociar el disco:

```powershell
$diskupdateconfig = New-AzDiskUpdateConfig -DiskMBpsReadWrite 2000
Update-AzDisk -ResourceGroupName $resourceGroup -DiskName $diskName -DiskUpdate $diskupdateconfig
```

## <a name="next-steps"></a>Pasos siguientes

Si quiere probar el nuevo tipo de disco, [solicite acceso con esta encuesta](https://aka.ms/UltraDiskSignup).

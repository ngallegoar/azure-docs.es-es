---
title: Uso de PowerShell para implementar máquinas virtuales de Azure Spot
description: Aprenda a usar Azure PowerShell para implementar máquinas virtuales de Spot con el fin de ahorrar costos.
services: virtual-machines-windows
author: cynthn
manager: gwallace
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.topic: article
ms.date: 02/11/2020
ms.author: cynthn
ms.openlocfilehash: 17186d1d7b50ea872dc47eca8c2c4491787d2a38
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "77158951"
---
# <a name="preview-deploy-spot-vms-using-azure-powershell"></a>Vista previa: Implementación de máquinas virtuales de Spot mediante Azure PowerShell


El uso de [máquinas virtuales de Spot](spot-vms.md) permite aprovechar las ventajas de nuestra capacidad no utilizada con un importante ahorro en los costos. Siempre que Azure necesite recuperar la capacidad, su infraestructura expulsará las máquinas virtuales de Spot. Por lo tanto, estas son excelentes para cargas de trabajo que soportan interrupciones, como los trabajos de procesamiento por lotes, los entornos de desarrollo y pruebas, las grandes cargas de trabajo de proceso, etc.

Los precios de las máquinas virtuales de Spot varían en función de la región y la SKU. Para más información, consulte precios de las máquinas virtuales para [Linux](https://azure.microsoft.com/pricing/details/virtual-machines/linux/) y [Windows](https://azure.microsoft.com/pricing/details/virtual-machines/windows/). Para más información sobre la configuración del precio máximo, consulte [Máquinas virtuales de Spot - Precios](spot-vms.md#pricing).

Puede establecer el precio máximo por hora que esté dispuesto por la máquina virtual. El precio máximo de una máquina virtual de Spot se puede establecer en dólares estadounidenses (USD), con un máximo de 5 decimales. Por ejemplo, el valor `0.98765` correspondería a un precio máximo de 0,98765 USD por hora. Si establece el precio máximo en `-1`, la máquina virtual no se expulsará por precio. El precio de la máquina virtual será el actual de Spot o el de una máquina virtual estándar, el menor de los dos, siempre que haya capacidad y cuota disponibles.

> [!IMPORTANT]
> Las instancias de Spot se encuentran actualmente en versión preliminar pública,
> la cual no se recomienda para las cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas. Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).
>



## <a name="create-the-vm"></a>Creación de la máquina virtual

Cree una máquina virtual de Spot y sírvase de [New-AzVmConfig](/powershell/module/az.compute/new-azvmconfig) para crear la configuración. Incluya `-Priority Spot` y establezca `-MaxPrice` en:
- `-1` para que la máquina virtual no se expulse en función del precio
- o en una cantidad de dólares, de hasta 5 dígitos. Por ejemplo, `-MaxPrice .98765` significa que la máquina virtual se desasignará cuando el precio de una máquina virtual de Spot sea de aproximadamente 0,98765 USD por hora.


En este ejemplo se crea una máquina virtual de Spot que no se desasigna por precio (solo cuando Azure requiera la capacidad).

```azurepowershell-interactive
$resourceGroup = "mySpotRG"
$location = "eastus"
$vmName = "mySpotVM"
$cred = Get-Credential -Message "Enter a username and password for the virtual machine."
New-AzResourceGroup -Name $resourceGroup -Location $location
$subnetConfig = New-AzVirtualNetworkSubnetConfig `
   -Name mySubnet -AddressPrefix 192.168.1.0/24
$vnet = New-AzVirtualNetwork -ResourceGroupName $resourceGroup `
   -Location $location -Name MYvNET -AddressPrefix 192.168.0.0/16 `
   -Subnet $subnetConfig
$pip = New-AzPublicIpAddress -ResourceGroupName $resourceGroup -Location $location `
  -Name "mypublicdns$(Get-Random)" -AllocationMethod Static -IdleTimeoutInMinutes 4
$nsgRuleRDP = New-AzNetworkSecurityRuleConfig -Name myNetworkSecurityGroupRuleRDP  -Protocol Tcp `
  -Direction Inbound -Priority 1000 -SourceAddressPrefix * -SourcePortRange * -DestinationAddressPrefix * `
  -DestinationPortRange 3389 -Access Allow
$nsg = New-AzNetworkSecurityGroup -ResourceGroupName $resourceGroup -Location $location `
  -Name myNetworkSecurityGroup -SecurityRules $nsgRuleRDP
$nic = New-AzNetworkInterface -Name myNic -ResourceGroupName $resourceGroup -Location $location `
  -SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $pip.Id -NetworkSecurityGroupId $nsg.Id

# Create a virtual machine configuration and set this to be a Spot VM

$vmConfig = New-AzVMConfig -VMName $vmName -VMSize Standard_D1 -Priority "Spot" -MaxPrice -1| `
Set-AzVMOperatingSystem -Windows -ComputerName $vmName -Credential $cred | `
Set-AzVMSourceImage -PublisherName MicrosoftWindowsServer -Offer WindowsServer -Skus 2016-Datacenter -Version latest | `
Add-AzVMNetworkInterface -Id $nic.Id

New-AzVM -ResourceGroupName $resourceGroup -Location $location -VM $vmConfig
```

Una vez creada la máquina virtual, puede realizar una consulta para ver el precio máximo de todas las máquinas virtuales del grupo de recursos.

```azurepowershell-interactive
Get-AzVM -ResourceGroupName $resourceGroup | `
   Select-Object Name,@{Name="maxPrice"; Expression={$_.BillingProfile.MaxPrice}}
```

## <a name="next-steps"></a>Pasos siguientes

También puede crear una máquina virtual de Spot mediante la [CLI de Azure](../linux/spot-cli.md) o una [plantilla](../linux/spot-template.md).

Si se produce un error, consulte [Códigos de error](../error-codes-spot.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
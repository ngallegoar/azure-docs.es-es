---
title: Crear una máquina virtual Azure con Accelerated Networking - Azure PowerShell
description: Aprenda a crear una máquina virtual Linux con Accelerated Networking.
services: virtual-network
documentationcenter: ''
author: gsilva5
manager: gedegrac
editor: ''
ms.assetid: ''
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 01/04/2018
ms.author: gsilva
ms.openlocfilehash: 16837782af2f08e27363091dc21587a100194cd8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "79225188"
---
# <a name="create-a-windows-virtual-machine-with-accelerated-networking-using-azure-powershell"></a>Creación de una máquina virtual Windows con Accelerated Networking mediante Azure PowerShell

En este tutorial, aprenderá a crear una máquina virtual Windows con Accelerated Networking. Para crear una máquina virtual Linux con redes aceleradas, consulte [Creación de una máquina virtual Linux con redes aceleradas](create-vm-accelerated-networking-cli.md). Accelerated Networking habilita la virtualización de E/S de raíz única (SR-IOV) en una máquina virtual (VM), lo que mejora significativamente su rendimiento en la red. Este método de alto rendimiento omite el host de la ruta de acceso de datos, lo que reduce la latencia, la inestabilidad y la utilización de la CPU para usarse con las cargas de trabajo de red más exigentes en los tipos de máquina virtual admitidos. En la siguiente imagen, se muestra la comunicación entre dos máquinas virtuales (VM) con y sin Accelerated Networking:

![De comparación](./media/create-vm-accelerated-networking/accelerated-networking.png)

Sin Accelerated Networking, todo el tráfico de red de entrada y salida de la máquina virtual tiene que atravesar el host y el conmutador virtual. El conmutador virtual se encarga de toda la aplicación de directivas, como grupos de seguridad de red, listas de control de acceso, aislamiento y otros servicios virtualizados de red, al tráfico de red. Para obtener más información sobre los conmutadores virtuales, vea [Hyper-V network virtualization and virtual switch](https://technet.microsoft.com/library/jj945275.aspx) (Virtualización de red de Hyper-V y conmutador virtual).

Con Accelerated Networking, el tráfico de red llega a la interfaz de red (NIC) de la máquina virtual y se reenvía después a la máquina virtual. Todas las directivas de red que el conmutador virtual aplica se descargan y aplican en el hardware. La aplicación de directivas en hardware permite que la NIC reenvíe el tráfico de red directamente a la máquina virtual, pasando por alto el host y el conmutador virtual, al mismo tiempo que se mantienen todas las directivas aplicadas en el host.

Las ventajas de Accelerated Networking solo se aplican a la máquina virtual donde esté habilitado. Para obtener resultados óptimos, lo ideal es habilitar esta característica en al menos dos máquinas virtuales conectadas a la misma instancia de Azure Virtual Network (VNet). Al comunicarse entre redes virtuales o conectarse de forma local, esta característica tiene un efecto mínimo sobre la latencia total.

## <a name="benefits"></a>Ventajas
* **Menor latencia/Más paquetes por segundo (pps):** al quitarse el conmutador virtual de la ruta de acceso de datos, se elimina el tiempo que los paquetes pasan en el host para el procesamiento de las directivas y se aumenta el número de paquetes que se pueden procesar dentro de la máquina virtual.
* **Inestabilidad reducida:** el procesamiento del conmutador virtual depende de la cantidad de directivas que deben aplicarse y la carga de trabajo de la CPU que se encarga del procesamiento. Al descargarse la aplicación de directivas en el hardware, se elimina esa variabilidad, ya que los paquetes se entregan directamente a la máquina virtual y se elimina el host de la comunicación de la máquina virtual, así como todas las interrupciones de software y los cambios de contexto.
* **Disminución de la utilización de la CPU:** el pasar por alto el conmutador virtual en el host conlleva una disminución de la utilización de la CPU para procesar el tráfico de red.

## <a name="limitations-and-constraints"></a>Limitaciones y restricciones

### <a name="supported-operating-systems"></a>Sistemas operativos admitidos
Se admiten las siguientes distribuciones de fábrica desde la galería de Azure:
* **Windows Server 2016 Datacenter** 
* **Windows Server 2012 R2 Datacenter**
* **Windows Server 2019 Datacenter**

### <a name="supported-vm-instances"></a>Instancias de máquina virtual admitidas
Accelerated Networking se admite con la mayoría de los tamaños de instancia de uso general y optimizados para procesos de dos o más vCPU.  Estas series admitidas son: D/DSv2 y F/Fs

En instancias que admiten hyperthreading, las redes aceleradas se admiten en instancias de máquina virtual con cuatro o más vCPU. Las series admitidas son: D/Dsv3, E/Esv3, Fsv2, Lsv2, Ms/Mms y Ms/Mmsv2.

Para más información sobre las instancias de máquinas virtuales, consulte [Tamaños de las máquinas virtuales con Windows](../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

### <a name="regions"></a>Regions
Está disponible en todas las regiones públicas de Azure y la nube de Azure Government.

### <a name="enabling-accelerated-networking-on-a-running-vm"></a>Habilitación de Accelerated Networking en una máquina virtual en ejecución
Un tamaño de máquina virtual admitido sin tener habilitado Accelerated Networking solo puede tener la característica habilitada cuando se detiene o se desasigna la máquina virtual.

### <a name="deployment-through-azure-resource-manager"></a>Implementación mediante Azure Resource Manager
Las máquinas virtuales (clásicas) no se pueden implementar con Accelerated Networking.

## <a name="create-a-windows-vm-with-azure-accelerated-networking"></a>Creación de una máquina virtual Windows con redes aceleradas de Azure
## <a name="portal-creation"></a>Creación del portal
Aunque en este artículo se explica cómo crear una máquina virtual con redes aceleradas mediante Azure PowerShell, también puede [crear una máquina virtual con redes aceleradas mediante Azure Portal](../virtual-machines/linux/quick-create-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json). Al crear una máquina virtual en el portal, en la hoja **Crear una máquina virtual**, seleccione la pestaña **Redes**.  En esta pestaña hay una opción para **Redes aceleradas**.  Si ha seleccionado un [sistema operativo compatible](#supported-operating-systems) y un [tamaño de máquina virtual](#supported-vm-instances), esta opción se rellena automáticamente como "Activado".  Si no, se rellena la opción "Desactivado" para Redes aceleradas y se proporciona al usuario un motivo de por qué no están habilitadas.   
* *Nota:* Solo los sistemas operativos compatibles se pueden habilitar a través del portal.  Si usa una imagen personalizada y esta es compatible con Redes aceleradas, cree la máquina virtual mediante la CLI o PowerShell. 

Una vez creada la máquina virtual, puede confirmar que la opción Redes aceleradas está habilitada si sigue las instrucciones que se indican en Confirmar que las redes aceleradas están habilitadas.

## <a name="powershell-creation"></a>Creación de PowerShell
## <a name="create-a-virtual-network"></a>Creación de una red virtual

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Instale [Azure PowerShell](/powershell/azure/install-az-ps) versión 1.0.0 o posterior. Para encontrar la versión instalada actualmente, ejecute `Get-Module -ListAvailable Az`. Si necesita instalar o actualizar, instale la versión más reciente del módulo Az desde la [Galería de PowerShell](https://www.powershellgallery.com/packages/Az). En una sesión de PowerShell, inicie sesión en una cuenta de Azure con [Connect-AzAccount](/powershell/module/az.accounts/connect-azaccount).

En los ejemplos siguientes, reemplace los nombres de parámetros de ejemplo por los suyos propios. Los nombres de parámetro de ejemplo incluían *myResourceGroup*, *myNic* y *myVM*.

Cree un grupo de recursos con [New-AzResourceGroup](/powershell/module/az.Resources/New-azResourceGroup). En el ejemplo siguiente, se crea un grupo de recursos denominado *myResourceGroup* en la ubicación *centralus*:

```powershell
New-AzResourceGroup -Name "myResourceGroup" -Location "centralus"
```

Primero, cree una configuración de subred con [New-AzVirtualNetworkSubnetConfig](/powershell/module/az.Network/New-azVirtualNetworkSubnetConfig). En el ejemplo siguiente se crea una subred denominada *mySubnet*:

```powershell
$subnet = New-AzVirtualNetworkSubnetConfig `
    -Name "mySubnet" `
    -AddressPrefix "192.168.1.0/24"
```

Cree una red virtual con [New-AzVirtualNetwork](/powershell/module/az.Network/New-azVirtualNetwork), con la subred *mySubnet*.

```powershell
$vnet = New-AzVirtualNetwork -ResourceGroupName "myResourceGroup" `
    -Location "centralus" `
    -Name "myVnet" `
    -AddressPrefix "192.168.0.0/16" `
    -Subnet $Subnet
```

## <a name="create-a-network-security-group"></a>Crear un grupo de seguridad de red

En primer lugar, cree una regla de grupo de seguridad de red con [New-AzNetworkSecurityRuleConfig](/powershell/module/az.Network/New-azNetworkSecurityRuleConfig).

```powershell
$rdp = New-AzNetworkSecurityRuleConfig `
    -Name 'Allow-RDP-All' `
    -Description 'Allow RDP' `
    -Access Allow `
    -Protocol Tcp `
    -Direction Inbound `
    -Priority 100 `
    -SourceAddressPrefix * `
    -SourcePortRange * `
    -DestinationAddressPrefix * `
    -DestinationPortRange 3389
```

Cree un grupo de seguridad de red con [New-AzNetworkSecurityGroup](/powershell/module/az.Network/New-azNetworkSecurityGroup) y asígnele la regla de seguridad *Allow-RDP-All*. Además de la regla *Allow-RDP-All*, el grupo de seguridad de red contiene varias reglas predeterminadas. Una regla predeterminada deshabilita todo el acceso entrante desde Internet, lo que constituye la razón por la que la regla *Allow-RDP-All* se asigna al grupo de seguridad de red para que pueda conectarla remotamente a la máquina virtual, una vez que se crea.

```powershell
$nsg = New-AzNetworkSecurityGroup `
    -ResourceGroupName myResourceGroup `
    -Location centralus `
    -Name "myNsg" `
    -SecurityRules $rdp
```

Asocie el grupo de seguridad de red a la subred *mySubnet* con [Set-AzVirtualNetworkSubnetConfig](/powershell/module/az.Network/Set-azVirtualNetworkSubnetConfig). La regla en el grupo de seguridad de red es eficaz para todos los recursos implementados en la subred.

```powershell
Set-AzVirtualNetworkSubnetConfig `
    -VirtualNetwork $vnet `
    -Name 'mySubnet' `
    -AddressPrefix "192.168.1.0/24" `
    -NetworkSecurityGroup $nsg
```

## <a name="create-a-network-interface-with-accelerated-networking"></a>Creación de una interfaz de red con Accelerated Networking
Cree una dirección IP pública con [New-AzPublicIpAddress](/powershell/module/az.Network/New-azPublicIpAddress). No se requiere una dirección IP pública si no tiene pensado acceder a la máquina virtual desde Internet, pero, para completar los pasos de este artículo, sí es necesaria.

```powershell
$publicIp = New-AzPublicIpAddress `
    -ResourceGroupName myResourceGroup `
    -Name 'myPublicIp' `
    -location centralus `
    -AllocationMethod Dynamic
```

Cree una interfaz de red con [New-AzNetworkInterface](/powershell/module/az.Network/New-azNetworkInterface) con la redes aceleradas habilitadas y asígnele la dirección IP pública. En el ejemplo siguiente se crea una interfaz de red denominada *myNic* en *mySubnet* de la red virtual *myVnet* y asigna la dirección IP pública *myPublicIp* :

```powershell
$nic = New-AzNetworkInterface `
    -ResourceGroupName "myResourceGroup" `
    -Name "myNic" `
    -Location "centralus" `
    -SubnetId $vnet.Subnets[0].Id `
    -PublicIpAddressId $publicIp.Id `
    -EnableAcceleratedNetworking
```

## <a name="create-the-virtual-machine"></a>Creación de la máquina virtual

Establezca las credenciales de la máquina virtual en la variable `$cred` con [Get-Credential](/powershell/module/microsoft.powershell.security/get-credential):

```powershell
$cred = Get-Credential
```

En primer lugar, defina la máquina virtual con [New-AzVMConfig](/powershell/module/az.compute/new-azvmconfig). En el ejemplo siguiente se define una máquina virtual denominada *myVM* con un tamaño que admite Accelerated Neworking (*Standard_DS4_v2*):

```powershell
$vmConfig = New-AzVMConfig -VMName "myVm" -VMSize "Standard_DS4_v2"
```

Para obtener una lista de todos los tamaños de máquinas virtuales y las características, consulte [tamaños de máquina virtual de Windows](../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

Cree el resto de la configuración de VM con [Set-AzVMOperatingSystem](/powershell/module/az.compute/set-azvmoperatingsystem) y [Set-AzVMSourceImage](/powershell/module/az.compute/set-azvmsourceimage). En el ejemplo siguiente se crea una máquina virtual con Windows Server 2016:

```powershell
$vmConfig = Set-AzVMOperatingSystem -VM $vmConfig `
    -Windows `
    -ComputerName "myVM" `
    -Credential $cred `
    -ProvisionVMAgent `
    -EnableAutoUpdate
$vmConfig = Set-AzVMSourceImage -VM $vmConfig `
    -PublisherName "MicrosoftWindowsServer" `
    -Offer "WindowsServer" `
    -Skus "2016-Datacenter" `
    -Version "latest"
```

Adjunte la interfaz de red creada anteriormente con [Add-AzVMNetworkInterface](/powershell/module/az.compute/add-azvmnetworkinterface):

```powershell
$vmConfig = Add-AzVMNetworkInterface -VM $vmConfig -Id $nic.Id
```

Por último, cree la máquina virtual con [New-AzVM](/powershell/module/az.compute/new-azvm):

```powershell
New-AzVM -VM $vmConfig -ResourceGroupName "myResourceGroup" -Location "centralus"
```

## <a name="confirm-the-driver-is-installed-in-the-operating-system"></a>Confirmación de que el controlador está instalado en el sistema operativo

Una vez creada la máquina virtual en Azure, conéctese a ella y confirme que el controlador está instalado en Windows.

1. Desde un explorador de Internet, abra[Azure Portal](https://portal.azure.com) e inicie sesión con su cuenta de Azure.
2. En el cuadro que contiene el texto *Buscar recursos*, en la parte superior de Azure Portal, escriba *myVm*. Haga clic en **MyVm** cuando aparezca en los resultados de búsqueda. Si **Creando** está visible en el botón **Conectar**, Azure aún no ha terminado de crear la máquina virtual. Haga clic en **Conectar** en la esquina superior izquierda de la información general, pero solo después de que ya no se vea **Creando** en el botón **Conectar**.
3. Escriba el nombre de usuario y la contraseña que escribió en [Creación de la máquina virtual](#create-the-virtual-machine). Si nunca se ha conectado a una máquina virtual Windows en Azure, consulte [Conexión a la máquina virtual](../virtual-machines/windows/quick-create-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json#connect-to-virtual-machine).
4. Haga clic con el botón derecho en el botón Inicio de Windows y haga clic en **Administrador de dispositivos**. Expanda el nodo **Adaptadores de red**. Compruebe que el **Adaptador Ethernet de función virtual ConnectX-3 de Mellanox** aparezca como se muestra en la siguiente imagen:

    ![Administrador de dispositivos](./media/create-vm-accelerated-networking/device-manager.png)

Las redes aceleradas ya están habilitadas para su máquina virtual.

## <a name="enable-accelerated-networking-on-existing-vms"></a>Habilitación de Accelerated Networking en máquinas virtuales existentes
Si ha creado una VM sin Accelerated Networking, es posible habilitar esta característica en una VM existente.  Para admitir Accelerated Networking, la VM debe cumplir los siguientes requisitos previos que también se han descrito anteriormente:

* La VM debe tener un tamaño admitido para Accelerated Networking.
* La VM debe ser una imagen admitida de la galería de Azure (y la versión de kernel de Linux).
* Todas las máquinas virtuales de un conjunto de disponibilidad o VMSS se deben detener o desasignar antes de habilitar Accelerated Networking en cualquier NIC.

### <a name="individual-vms--vms-in-an-availability-set"></a>VM individuales y VM de un conjunto de disponibilidad
En primer lugar, detenga o desasigne la VM o, en el caso de un conjunto de disponibilidad, todas las VM del conjunto:

```azurepowershell
Stop-AzVM -ResourceGroup "myResourceGroup" `
    -Name "myVM"
```

Es importante tener en cuenta que, si la máquina virtual se creó de forma individual, sin un conjunto de disponibilidad, solo es necesario detener o desasignar esa máquina virtual individual para habilitar Accelerated Networking.  Si la máquina virtual se creó con un conjunto de disponibilidad, será necesario detener o desasignar todas las máquinas virtuales contenidas en el conjunto de disponibilidad antes de habilitar Accelerated Networking en cualquiera de las NIC. 

Una vez detenida, habilite Accelerated Networking en la NIC de la máquina virtual:

```azurepowershell
$nic = Get-AzNetworkInterface -ResourceGroupName "myResourceGroup" `
    -Name "myNic"

$nic.EnableAcceleratedNetworking = $true

$nic | Set-AzNetworkInterface
```

Reinicie la máquina virtual o, en el caso de un conjunto de disponibilidad, todas las máquinas virtuales del conjunto, y confirme que las redes aceleradas están habilitadas:

```azurepowershell
Start-AzVM -ResourceGroup "myResourceGroup" `
    -Name "myVM"
```

### <a name="vmss"></a>VMSS
VMSS es ligeramente diferente, pero sigue el mismo flujo de trabajo.  En primer lugar, detenga las VM:

```azurepowershell
Stop-AzVmss -ResourceGroupName "myResourceGroup" `
    -VMScaleSetName "myScaleSet"
```

Una vez detenidas las VM, actualice la propiedad Accelerated Networking bajo la interfaz de red:

```azurepowershell
$vmss = Get-AzVmss -ResourceGroupName "myResourceGroup" `
    -VMScaleSetName "myScaleSet"

$vmss.VirtualMachineProfile.NetworkProfile.NetworkInterfaceConfigurations[0].EnableAcceleratedNetworking = $true

Update-AzVmss -ResourceGroupName "myResourceGroup" `
    -VMScaleSetName "myScaleSet" `
    -VirtualMachineScaleSet $vmss
```

Tenga en cuenta que un VMSS tiene actualizaciones de VM que aplican actualizaciones mediante tres valores diferentes: automático, gradual y manual.  En estas instrucciones, la directiva se establece en automático para que el VMSS recoja los cambios inmediatamente después de reiniciarse.  Para establecerla en automático para que los cambios se recojan inmediatamente:

```azurepowershell
$vmss.UpgradePolicy.AutomaticOSUpgrade = $true

Update-AzVmss -ResourceGroupName "myResourceGroup" `
    -VMScaleSetName "myScaleSet" `
    -VirtualMachineScaleSet $vmss
```

Por último, reinicie el VMSS:

```azurepowershell
Start-AzVmss -ResourceGroupName "myResourceGroup" `
    -VMScaleSetName "myScaleSet"
```

Después de reiniciar, espere a que las actualizaciones finalicen y la función virtual aparecerá dentro de la VM.  (Asegúrese de que usa un tamaño admitido de sistema operativo y máquina virtual).

### <a name="resizing-existing-vms-with-accelerated-networking"></a>Cambio del tamaño de las VM existentes con Accelerated Networking

Las VM con Accelerated Networking habilitado solo pueden cambiar al tamaño de VM que admitan Accelerated Networking.  

Una VM con Accelerated Networking habilitado no puede cambiar al tamaño de una instancia de VM que no admita Accelerated Networking mediante la operación de cambio de tamaño.  Para cambiar el tamaño de una de estas VM, debe hacer lo siguiente:

* Detener o desasignar la VM o, si se trata de un conjunto de disponibilidad o VMSS, detener o desasignar todas las VM del conjunto o VMSS.
* Accelerated Networking debe estar deshabilitado en la NIC de la VM o, si está en un conjunto de disponibilidad o VMSS, de todas las VM del conjunto o VMSS.
* Una vez deshabilitadas, la máquina virtual, el conjunto de disponibilidad o el VMSS se pueden mover a un nuevo tamaño que no admita redes aceleradas y reiniciarse.

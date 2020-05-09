---
title: Creación de un punto de conexión privado de Azure mediante Azure PowerShell | Microsoft Docs
description: Información acerca de Azure Private Link
services: private-link
author: malopMSFT
ms.service: private-link
ms.topic: article
ms.date: 09/16/2019
ms.author: allensu
ms.openlocfilehash: 8af33e95c92cf51bdabe3325bd9249b4662b7d28
ms.sourcegitcommit: b9d4b8ace55818fcb8e3aa58d193c03c7f6aa4f1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/29/2020
ms.locfileid: "82583759"
---
# <a name="create-a-private-endpoint-using-azure-powershell"></a>Creación de un punto de conexión privado mediante Azure PowerShell
Un punto de conexión privado es el bloque de creación fundamental para el vínculo privado en Azure. Permite que los recursos de Azure, como las máquinas virtuales, se comuniquen de manera privada con recursos de vínculos privados. 

En este inicio rápido, obtendrá información sobre cómo crear una máquina virtual en una instancia de Azure Virtual Network, un servidor de SQL Database con un punto de conexión privado de Azure mediante Azure PowerShell. Después, puede acceder de forma segura al servidor de SQL Database desde la máquina virtual.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="create-a-resource-group"></a>Crear un grupo de recursos

Para crear los recursos, debe crear un grupo de recursos que hospede la máquina virtual y el punto de conexión privado con [New-AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup). En el ejemplo siguiente se crea un grupo de recursos denominado *myResourceGroup* en la ubicación *WestUS*:

```azurepowershell

New-AzResourceGroup `
  -ResourceGroupName myResourceGroup `
  -Location westcentralus
```

## <a name="create-a-virtual-network"></a>Creación de una red virtual
En esta sección, creará una red virtual y una subred. Después, asociará la subred a la red virtual.

### <a name="create-a-virtual-network"></a>Creación de una red virtual

Cree una red virtual para su punto de conexión privado con [New-AzVirtualNetwork](/powershell/module/az.network/new-azvirtualnetwork). En el ejemplo siguiente se crea una red virtual llamada *MyVirtualNetwork*:
 
```azurepowershell

$virtualNetwork = New-AzVirtualNetwork `
  -ResourceGroupName myResourceGroup `
  -Location westcentralus `
  -Name myVirtualNetwork `
  -AddressPrefix 10.0.0.0/16
```

### <a name="add-a-subnet"></a>Adición de una subred

Azure implementa recursos en una subred dentro de una red virtual, por lo que es necesario crear una subred. Cree una configuración de subred denominada *mySubnet* con [Add-AzVirtualNetworkSubnetConfig](/powershell/module/az.network/add-azvirtualnetworksubnetconfig). En el ejemplo siguiente se crea una subred denominada *mySubnet* con la marca de directiva de la red del punto de conexión privado establecida en **Deshabilitado**.

```azurepowershell
$subnetConfig = Add-AzVirtualNetworkSubnetConfig `
  -Name mySubnet `
  -AddressPrefix 10.0.0.0/24 `
  -PrivateEndpointNetworkPoliciesFlag "Disabled" `
  -VirtualNetwork $virtualNetwork
```

> [!CAUTION]
> Es fácil confundir el parámetro `PrivateEndpointNetworkPoliciesFlag` con otra marca disponible, `PrivateLinkServiceNetworkPoliciesFlag`, porque son palabras largas y tienen una apariencia similar.  Asegúrese de que usa el adecuado, `PrivateEndpointNetworkPoliciesFlag`.

### <a name="associate-the-subnet-to-the-virtual-network"></a>Asociación de la subred a la red virtual

Puede escribir la configuración de la subred en la red virtual con [Set-AzVirtualNetwork](/powershell/module/az.network/Set-azVirtualNetwork). Este comando crea la subred:

```azurepowershell
$virtualNetwork | Set-AzVirtualNetwork
```

## <a name="create-a-virtual-machine"></a>Creación de una máquina virtual

Cree una máquina virtual en la red virtual con [New-AzVM](/powershell/module/az.compute/new-azvm). Cuando ejecute el comando siguiente, se le pedirán las credenciales. Escriba un nombre de usuario y una contraseña para la máquina virtual:

```azurepowershell-interactive
New-AzVm `
    -ResourceGroupName "myResourceGroup" `
    -Name "myVm" `
    -Location "westcentralus" `
    -VirtualNetworkName "MyVirtualNetwork" `
    -SubnetName "mySubnet" `
    -SecurityGroupName "myNetworkSecurityGroup" `
    -PublicIpAddressName "myPublicIpAddress" `
    -OpenPorts 80,3389 `
    -AsJob  
```

La opción `-AsJob` crea la máquina virtual en segundo plano. Puede continuar al paso siguiente.

Cuando Azure empieza a crear la máquina virtual en segundo plano, verá algo como esto:

```powershell
Id     Name            PSJobTypeName   State         HasMoreData     Location             Command
--     ----            -------------   -----         -----------     --------             -------
1      Long Running... AzureLongRun... Running       True            localhost            New-AzVM
```

## <a name="create-a-sql-database-server"></a>Creación de un servidor de SQL Database 

Cree un servidor de SQL Database con el comando New-AzSqlServer. Recuerde que el nombre del servidor de SQL Database debe ser único en Azure, así que reemplace el valor de marcador de posición entre corchetes por su propio valor único:

```azurepowershell-interactive
$adminSqlLogin = "SqlAdmin"
$password = "ChangeYourAdminPassword1"

$server = New-AzSqlServer -ResourceGroupName "myResourceGroup" `
    -ServerName "myserver" `
    -Location "WestCentralUS" `
    -SqlAdministratorCredentials $(New-Object -TypeName System.Management.Automation.PSCredential -ArgumentList $adminSqlLogin, $(ConvertTo-SecureString -String $password -AsPlainText -Force))

New-AzSqlDatabase  -ResourceGroupName "myResourceGroup" `
    -ServerName "myserver"`
    -DatabaseName "myda"`
    -RequestedServiceObjectiveName "S0" `
    -SampleName "AdventureWorksLT"
```

## <a name="create-a-private-endpoint"></a>Creación de un punto de conexión privado

Punto de conexión privado para el servidor de SQL Database en la red virtual con [New-AzPrivateLinkServiceConnection](/powershell/module/az.network/New-AzPrivateLinkServiceConnection): 

```azurepowershell

$privateEndpointConnection = New-AzPrivateLinkServiceConnection -Name "myConnection" `
  -PrivateLinkServiceId $server.ResourceId `
  -GroupId "sqlServer" 
 
$virtualNetwork = Get-AzVirtualNetwork -ResourceGroupName  "myResourceGroup" -Name "MyVirtualNetwork"  
 
$subnet = $virtualNetwork `
  | Select -ExpandProperty subnets `
  | Where-Object  {$_.Name -eq 'mysubnet'}  
 
$privateEndpoint = New-AzPrivateEndpoint -ResourceGroupName "myResourceGroup" `
  -Name "myPrivateEndpoint" `
  -Location "westcentralus" `
  -Subnet  $subnet `
  -PrivateLinkServiceConnection $privateEndpointConnection
``` 

## <a name="configure-the-private-dns-zone"></a>Configuración de la zona DNS privada 
Cree una zona DNS privada para el dominio del servidor de SQL Database y cree un vínculo de asociación con la red virtual: 

```azurepowershell

$zone = New-AzPrivateDnsZone -ResourceGroupName "myResourceGroup" `
  -Name "privatelink.database.windows.net" 
 
$link  = New-AzPrivateDnsVirtualNetworkLink -ResourceGroupName "myResourceGroup" `
  -ZoneName "privatelink.database.windows.net"`
  -Name "mylink" `
  -VirtualNetworkId $virtualNetwork.Id  
 
$networkInterface = Get-AzResource -ResourceId $privateEndpoint.NetworkInterfaces[0].Id -ApiVersion "2019-04-01" 
 
foreach ($ipconfig in $networkInterface.properties.ipConfigurations) { 
foreach ($fqdn in $ipconfig.properties.privateLinkConnectionProperties.fqdns) { 
Write-Host "$($ipconfig.properties.privateIPAddress) $($fqdn)"  
$recordName = $fqdn.split('.',2)[0] 
$dnsZone = $fqdn.split('.',2)[1] 
New-AzPrivateDnsRecordSet -Name $recordName -RecordType A -ZoneName "privatelink.database.windows.net"  `
-ResourceGroupName "myResourceGroup" -Ttl 600 `
-PrivateDnsRecords (New-AzPrivateDnsRecordConfig -IPv4Address $ipconfig.properties.privateIPAddress)  
} 
} 
``` 
  
## <a name="connect-to-a-vm-from-the-internet"></a>Conexión a una máquina virtual desde Internet

Use [Get-AzPublicIpAddress](/powershell/module/az.network/Get-AzPublicIpAddress) para devolver la dirección IP pública de una máquina virtual. Este ejemplo devuelve la dirección IP pública de la máquina virtual *myVM*:

```azurepowershell
Get-AzPublicIpAddress `
  -Name myPublicIpAddress `
  -ResourceGroupName myResourceGroup `
  | Select IpAddress 
```  
Abra un símbolo del sistema en el equipo local. Ejecute el comando mstsc. Reemplace <publicIpAddress> por la dirección IP pública que se devolvió en el último paso: 


> [!NOTE]
> Si ejecutó estos comandos desde un símbolo del sistema de PowerShell en el equipo local y usa la versión 1.0 o posterior del módulo Az de PowerShell, puede seguir en esa interfaz.
```
mstsc /v:<publicIpAddress>
```

1. Cuando se le pida, seleccione **Conectar**. 
2. Escriba el nombre de usuario y la contraseña que especificó al crear la máquina virtual.
  > [!NOTE]
  > Es posible que tenga que seleccionar Más opciones > Usar otra cuenta para especificar las credenciales que escribió al crear la máquina virtual. 
  
3. Seleccione **Aceptar**. 
4. Puede que reciba una advertencia de certificado. Si la recibe, seleccione **Sí** o **Continuar**. 

## <a name="access-sql-database-server-privately-from-the-vm"></a>Acceso al servidor de SQL Database de forma privada desde la máquina virtual

1. En el Escritorio remoto de myVm, abra PowerShell.
2. Escriba `nslookup myserver.database.windows.net`. No olvide reemplazar `myserver` por el nombre de su instancia de SQL Server.

    Recibirá un mensaje similar a este:
    
    ```azurepowershell
    Server:  UnKnown
    Address:  168.63.129.16
    Non-authoritative answer:
    Name:    myserver.privatelink.database.windows.net
    Address:  10.0.0.5
    Aliases:   myserver.database.windows.net
    ```
    
3. Instale SQL Server Management Studio.
4. En **Conectar con el servidor**, escriba o seleccione esta información:

    | Configuración | Value |
    | --- | --- |
    | Tipo de servidor | Motor de base de datos |
    | Nombre de servidor | myserver.database.windows.net |
    | Nombre de usuario | Escribir el nombre de usuario proporcionado durante la creación |
    | Contraseña | Escribir la contraseña proporcionada durante la creación |
    | Recordar contraseña | Sí |
    
5. Seleccione **Conectar**.
6. Examine las **bases de datos** en el menú izquierdo. 
7. (Opcionalmente) Cree o consulte la información de mydatabase.
8. Cierre la conexión de Escritorio remoto a *myVM*. 

## <a name="clean-up-resources"></a>Limpieza de recursos 
Cuando haya terminado con el punto de conexión privado, el servidor de SQL Database y la máquina virtual, use [Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup) para quitar el grupo de recursos y todos los recursos que tiene:

```azurepowershell-interactive
Remove-AzResourceGroup -Name myResourceGroup -Force
```

## <a name="next-steps"></a>Pasos siguientes
- Más información sobre [Azure Private Link](private-link-overview.md).

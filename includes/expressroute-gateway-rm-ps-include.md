---
title: archivo de inclusión
description: archivo de inclusión
services: expressroute
author: cherylmc
ms.service: expressroute
ms.topic: include
ms.date: 02/21/2019
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 922ac7eb6cb9676af65700a6a2fe7fbae35a0dc5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "67186848"
---
Los pasos de esta tarea usan una red virtual que se basa en los valores de la siguiente lista de referencia de configuración. En esta lista también se enumeran nombres y valores de configuración adicionales. No se utiliza esta lista directamente en ninguno de los pasos, aunque se agregan variables basadas en los valores que aparecen en ella. Puede copiar la lista para utilizarla como referencia y reemplazar los valores por los suyos propios.

* Nombre de red virtual = "TestVNet"
* Espacio de direcciones de red virtual = 192.168.0.0/16
* Grupo de recursos: "TestRG"
* Nombre de subred 1 = "FrontEnd" 
* Espacio de direcciones de subred 1 = "192.168.1.0/24"
* Nombre de subred de puerta de enlace: "GatewaySubnet" (siempre debe asignar a las subredes de puerta de enlace el nombre *GatewaySubnet*).
* Espacio de direcciones de subred de puerta de enlace = "192.168.200.0/26"
* Región = "Este de EE. UU."
* Nombre de puerta de enlace = "GW"
* Nombre de IP de puerta de enlace = "GWIP"
* Nombre de configuración de IP de puerta de enlace = "gwipconf"
* Tipo = "ExpressRoute" (este tipo es obligatorio para una configuración de ExpressRoute).
* Nombre de IP pública de puerta de enlace = "gwpip"

## <a name="add-a-gateway"></a>Adición de una puerta de enlace
1. Conéctese a su suscripción de Azure.

   [!INCLUDE [Sign in](expressroute-cloud-shell-connect.md)]
2. Declare las variables para este ejercicio. No olvide editar el ejemplo para reflejar la configuración que desea utilizar.

   ```azurepowershell-interactive 
   $RG = "TestRG"
   $Location = "East US"
   $GWName = "GW"
   $GWIPName = "GWIP"
   $GWIPconfName = "gwipconf"
   $VNetName = "TestVNet"
   ```
3. Almacene el objeto de red virtual como una variable.

   ```azurepowershell-interactive
   $vnet = Get-AzVirtualNetwork -Name $VNetName -ResourceGroupName $RG
   ```
4. Agregue una subred de puerta de enlace a su red virtual. A la subred de puerta de enlace debe asignarle el nombre "GatewaySubnet". Se recomienda que cree una subred de puerta de enlace con un valor /27 o mayor (/26, /25, etc.).

   ```azurepowershell-interactive
   Add-AzVirtualNetworkSubnetConfig -Name GatewaySubnet -VirtualNetwork $vnet -AddressPrefix 192.168.200.0/26
   ```
5. Establezca la configuración.

   ```azurepowershell-interactive
   $vnet = Set-AzVirtualNetwork -VirtualNetwork $vnet
   ```
6. Almacene la subred de puerta de enlace como una variable.

   ```azurepowershell-interactive
   $subnet = Get-AzVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -VirtualNetwork $vnet
   ```
7. Pida una dirección IP pública. La dirección IP se solicita antes de crear la puerta de enlace. No puede especificar la dirección IP que desea usar; se asigna una dinámicamente. Utilizará esta dirección IP en la siguiente sección de configuración. El valor de AllocationMethod debe ser Dynamic.

   ```azurepowershell-interactive
   $pip = New-AzPublicIpAddress -Name $GWIPName  -ResourceGroupName $RG -Location $Location -AllocationMethod Dynamic
   ```
8. Cree la configuración para su puerta de enlace. La configuración de puerta de enlace define la subred y la dirección IP pública. En este paso, se especifica la configuración que se utilizará al crear la puerta de enlace, pero no se crea realmente el objeto de puerta de enlace. Use el ejemplo siguiente para crear la configuración de la puerta de enlace.

   ```azurepowershell-interactive
   $ipconf = New-AzVirtualNetworkGatewayIpConfig -Name $GWIPconfName -Subnet $subnet -PublicIpAddress $pip
   ```
9. Cree la puerta de enlace. En este paso, el elemento **-GatewayType** resulta especialmente importante. Debe utilizar el valor **ExpressRoute**. Después de ejecutar estos cmdlets, la puerta de enlace puede tardar 45 minutos o más en crearse.

   ```azurepowershell-interactive
   New-AzVirtualNetworkGateway -Name $GWName -ResourceGroupName $RG -Location $Location -IpConfigurations $ipconf -GatewayType Expressroute -GatewaySku Standard
   ```

## <a name="verify-the-gateway-was-created"></a>Comprobación de la creación de la puerta de enlace
Utilice los siguientes comandos para comprobar si se ha creado la puerta de enlace:

```azurepowershell-interactive
Get-AzVirtualNetworkGateway -ResourceGroupName $RG
```

## <a name="resize-a-gateway"></a>Cambio del tamaño de una puerta de enlace
Hay varias [SKU de puerta de enlace](../articles/expressroute/expressroute-about-virtual-network-gateways.md). Puede utilizar el siguiente comando para cambiar en cualquier momento la SKU de puerta de enlace.

> [!IMPORTANT]
> Este comando no funciona para la puerta de enlace de UltraPerformance. Para cambiar la puerta de enlace a una puerta de enlace de UltraPerformance, quite primero la puerta de enlace de ExpressRoute existente y, después, cree una nueva puerta de enlace de UltraPerformance. Para degradar la puerta de enlace desde una puerta de enlace de UltraPerformance, quite primero la puerta de enlace de UltraPerformance existente y, después, cree una nueva puerta de enlace.
> 
> 

```azurepowershell-interactive
$gw = Get-AzVirtualNetworkGateway -Name $GWName -ResourceGroupName $RG
Resize-AzVirtualNetworkGateway -VirtualNetworkGateway $gw -GatewaySku HighPerformance
```

## <a name="remove-a-gateway"></a>Eliminación de una puerta de enlace
Para quitar una puerta de enlace, use el siguiente comando:

```azurepowershell-interactive
Remove-AzVirtualNetworkGateway -Name $GWName -ResourceGroupName $RG
```

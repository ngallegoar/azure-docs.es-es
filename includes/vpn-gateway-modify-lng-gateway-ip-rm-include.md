---
title: archivo de inclusión
description: archivo de inclusión
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 01/15/2020
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 6a90b23c10e08e8b14a18f9619cff5aaeb003cab
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "76045682"
---
### <a name="to-modify-the-local-network-gateway-gatewayipaddress---no-gateway-connection"></a><a name="gwipnoconnection"></a>Para modificar la puerta de enlace de red local "GatewayIpAddress": no hay ninguna conexión de puerta de enlace

Si el dispositivo VPN al que desea conectarse ha cambiado su dirección IP pública, debe modificar la puerta de enlace de red local para reflejar ese cambio. Use el ejemplo para modificar una puerta de enlace de red local que no tenga una conexión de puerta de enlace.

Al modificar este valor, también puede modificar al mismo tiempo los prefijos de dirección. Asegúrese de usar el nombre existente de la puerta de enlace de la red local para sobrescribir la configuración actual. Si usa otro nombre, creará una nueva puerta de enlace de red local, en lugar de sobrescribir la existente.

```azurepowershell-interactive
New-AzLocalNetworkGateway -Name Site1 `
-Location "East US" -AddressPrefix @('10.101.0.0/24','10.101.1.0/24') `
-GatewayIpAddress "5.4.3.2" -ResourceGroupName TestRG1
```

### <a name="to-modify-the-local-network-gateway-gatewayipaddress---existing-gateway-connection"></a><a name="gwipwithconnection"></a>Para modificar la puerta de enlace de red local "GatewayIpAddress": conexión de puerta de enlace existente

Si el dispositivo VPN al que desea conectarse ha cambiado su dirección IP pública, debe modificar la puerta de enlace de red local para reflejar ese cambio. Si ya existe una conexión de puerta de enlace, primero deberá quitar esa conexión. Después de quitar la conexión, puede modificar la dirección IP de la puerta de enlace y volver a crear una nueva conexión. También puede modificar los prefijos de dirección al mismo tiempo. Esto tendrá como resultado un tiempo de inactividad para la conexión VPN. Al modificar la dirección IP de puerta de enlace, no es necesario eliminar la puerta de enlace VPN. Basta con quitar la conexión.
 

1. Cierre la conexión. Puede encontrar el nombre de la conexión mediante el cmdlet "Get-AzVirtualNetworkGatewayConnection".

   ```azurepowershell-interactive
   Remove-AzVirtualNetworkGatewayConnection -Name VNet1toSite1 `
   -ResourceGroupName TestRG1
   ```
2. Modifique el valor de 'GatewayIpAddress'. También puede modificar los prefijos de dirección al mismo tiempo. Asegúrese de utilizar el nombre de la puerta de enlace de la red local existente para sobrescribir la configuración actual. Si no lo hace, creará una nueva puerta de enlace de la red local, en lugar de sobrescribir la existente.

   ```azurepowershell-interactive
   New-AzLocalNetworkGateway -Name Site1 `
   -Location "East US" -AddressPrefix @('10.101.0.0/24','10.101.1.0/24') `
   -GatewayIpAddress "104.40.81.124" -ResourceGroupName TestRG1
   ```
3. Cree la conexión. En este ejemplo, vamos a configurar un tipo de conexión de IPsec. Cuando se vuelva a crear la conexión, use el tipo de conexión que se especifica para la configuración. Para otros tipos de conexión, consulte la página de [cmdlets de PowerShell](https://msdn.microsoft.com/library/mt603611.aspx) .  Para obtener el nombre de VirtualNetworkGateway, puede ejecutar el cmdlet "Get-AzVirtualNetworkGateway".
   
    Establezca las variables.

   ```azurepowershell-interactive
   $local = Get-AzLocalNetworkGateway -Name Site1 -ResourceGroupName TestRG1

   $vnetgw = Get-AzVirtualNetworkGateway -Name VNet1GW -ResourceGroupName TestRG1
   ```
   
    Cree la conexión.

   ```azurepowershell-interactive 
   New-AzVirtualNetworkGatewayConnection -Name VNet1Site1 -ResourceGroupName TestRG1 `
   -Location "East US" `
   -VirtualNetworkGateway1 $vnetgw `
   -LocalNetworkGateway2 $local `
   -ConnectionType IPsec -RoutingWeight 10 -SharedKey 'abc123'
   ```
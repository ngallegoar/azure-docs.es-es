---
title: archivo de inclusión
description: archivo de inclusión
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 11/04/2019
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 4842c55b2b1fd23f4d6b7996ccf02e7141504836
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "73495762"
---
Puede usar el cmdlet de PowerShell `Resize-AzVirtualNetworkGateway` para actualizar o cambiar a una versión anterior una SKU de Generation1 o Generation2 (se puede cambiar el tamaño de todas las SKU de VpnGw excepto las SKU básicas). Si usa la SKU de puerta de enlace Basic, [siga estas instrucciones](../articles/vpn-gateway/vpn-gateway-about-skus-legacy.md#resize) para cambiar el tamaño de la puerta de enlace.

En el siguiente ejemplo de PowerShell se muestra una SKU de puerta de enlace cuyo tamaño se ha cambiado a VpnGw2.

```azurepowershell-interactive
$gw = Get-AzVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg
Resize-AzVirtualNetworkGateway -VirtualNetworkGateway $gw -GatewaySku VpnGw2
```

También se puede cambiar el tamaño de una puerta de enlace en Azure Portal desde la página **Configuración** para la puerta de enlace de red virtual. Para hacerlo, seleccione otra SKU en la lista desplegable.

---
title: archivo de inclusión
titleSuffix: Azure
description: archivo de inclusión
services: internet-peering
author: prmitiki
ms.service: internet-peering
ms.topic: include
ms.date: 11/27/2019
ms.author: prmitiki
ms.openlocfilehash: 86e1a9cce1864ce259fe07b6949be2e32be242a8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "75773703"
---
La siguiente operación de modificación es compatible con el emparejamiento de Exchange
1. Adición de conexiones de emparejamiento de Exchange
1. Eliminación de conexiones de emparejamiento de Exchange
1. Adición de sesión IPv4/IPv6 en conexiones activas
1. Eliminación de la sesión IPv4/IPv6 en conexiones activas


### <a name="add-exchange-peering-connections"></a>Adición de conexiones de emparejamiento de Exchange

En el ejemplo siguiente se describe cómo agregar conexiones a un emparejamiento de Exchange existente.

```powershell

$exchangePeering = Get-AzPeering -Name "SeattleExchangePeering" -ResourceGroupName "PeeringResourceGroup"

$connection = New-AzPeeringExchangeConnectionObject `
    -PeeringDBFacilityId $exchangeLocation[1].PeeringDBFacilityId `
    -PeerSessionIPv4Address 198.32.134.22 `
    -PeerSessionIPv6Address  2001:504:12::22 `
    -MaxPrefixesAdvertisedIPv4 2000 `
    -MaxPrefixesAdvertisedIPv6 2000 `

$exchangePeering.Connections.Add($connection)

$exchangePeering | Update-AzPeering

```

### <a name="remove-exchange-peering-connections"></a>Eliminación de conexiones de emparejamiento de Exchange

En el ejemplo siguiente se describe cómo eliminar conexiones a un emparejamiento de Exchange existente.

```powershell

$exchangePeering = Get-AzPeering -Name "SeattleExchangePeering" -ResourceGroupName "PeeringResourceGroup"

```

Vea todas las conexiones y seleccione la conexión que desea quitar. 

```powershell

$exchangePeering

Name              : SeattleExchangePeering
Sku.Name          : Basic_Exchange_Free
Kind              : Exchange
Connections       : {11}
PeerAsn.Id        : /subscriptions/{subscriptionId}/providers/Microsoft.Peering/peerAsns/{peerAsnName}
PeeringLocation   : Seattle
ProvisioningState : Succeeded
Location          : West US 2
Id                : /subscriptions/{subscriptionId}/resourceGroups/PeeringResourceGroup/providers/Microsoft.Peering/peerings/SeattleExchangePeering
Type              : Microsoft.Peering/peerings
Tags              : {}

```

En el siguiente comando, en lugar de 0, escriba el número de índice de la conexión que desea quitar.

```powershell

$connection = $exchangePeering.Connections[0]

$exchangePeering.Connections.Remove($connection)

$exchangePeering | Update-AzPeering

```

### <a name="add-ipv4ipv6-session-on-active-connections"></a>Adición de sesión IPv4/IPv6 en conexiones activas

En el ejemplo siguiente se describe cómo agregar una sesión IPv6 a una conexión de Exchange existente.

```powershell

$exchangePeering = Get-AzPeering -Name "SeattleExchangePeering" -ResourceGroupName "PeeringResourceGroup"

$exchangePeering.Connections[0].BgpSession.PeerSessionIPv6Address = "2001:504:12::34"

$exchangePeering | Update-AzPeering

```

### <a name="remove-ipv4ipv6-session-on-active-connections"></a>Eliminación de la sesión IPv4/IPv6 en conexiones activas

La eliminación de una sesión IPv4/IPv6 desde una conexión existente no se admite actualmente en PowerShell. Póngase en contacto con el equipo de [emparejamiento de Microsoft](mailto:peeringexperience@microsoft.com).
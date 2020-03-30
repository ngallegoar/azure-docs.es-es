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
ms.openlocfilehash: 33ae3c3f9a2a213bd8135af846b4e72901b3df0d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "75773711"
---
Para obtener la lista de emparejamientos, ejecute el comando `Get-AzPeering`:

```powershell
Get-AzPeering ResourceGroupName "PeeringResourceGroup" -Name "SeattleExchangePeering"
```

A continuación se muestra una respuesta de ejemplo en que el aprovisionamiento de un extremo a otro se completó correctamente:

```powershell
    Name                     : SeattleExchangePeering
    Sku                      : Basic_Exchange_Free
    Kind                     : Exchange
    PeeringLocation          : Seattle
    ProvisioningState        : Succeeded
    PeerAsn                  : 65000
    Connection               : ------------------------
    PeerSessionIPv4Address   : 10.21.31.100
    MicrosoftIPv4Address     : 10.21.31.50
    SessionStateV4           : Established
    MaxPrefixesAdvertisedV4  : 20000
    PeerSessionIPv6Address   : fe01::3e:100
    MicrosoftIPv6Address     : fe01::3e:50
    SessionStateV6           : Established
    MaxPrefixesAdvertisedV6  : 2000
    ConnectionState          : Active
    Connection               : ------------------------
    PeerSessionIPv4Address   : 10.21.31.101
    MicrosoftIPv4Address     : 10.21.31.51
    SessionStateV4           : Established
    MaxPrefixesAdvertisedV4  : 20000
    PeerSessionIPv6Address   : fe01::3e:101
    MicrosoftIPv6Address     : fe01::3e:51
    SessionStateV6           : Established
    MaxPrefixesAdvertisedV6  : 2000
    ConnectionState          : Active
```
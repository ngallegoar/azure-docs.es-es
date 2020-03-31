---
title: Conversión de un emparejamiento de Exchange heredado en un recurso de Azure mediante PowerShell
titleSuffix: Azure
description: Conversión de un emparejamiento de Exchange heredado en un recurso de Azure mediante PowerShell
services: internet-peering
author: prmitiki
ms.service: internet-peering
ms.topic: article
ms.date: 11/27/2019
ms.author: prmitiki
ms.openlocfilehash: aa11f329cf0a0cb27d58b940b42731a2ec41c272
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "75774003"
---
# <a name="convert-a-legacy-exchange-peering-to-azure-resource-using-powershell"></a>Conversión de un emparejamiento de Exchange heredado en un recurso de Azure mediante PowerShell

En este artículo se describe cómo convertir un emparejamiento de Exchange heredado existente en un recurso de Azure mediante cmdlets de PowerShell.

Si lo prefiere, puede completar esta guía mediante el [portal](howto-legacy-exchange-portal.md).

## <a name="before-you-begin"></a>Antes de empezar
* Revise la sección [Requisitos previos](prerequisites.md) y el [Tutorial del emparejamiento de Exchange](walkthrough-exchange-all.md) antes de comenzar la configuración.

### <a name="working-with-azure-powershell"></a>Trabajo con Azure PowerShell
[!INCLUDE [CloudShell](./includes/cloudshell-powershell-about.md)]

## <a name="convert-a-legacy-exchange-peering-to-azure-resource"></a>Conversión de un emparejamiento de Exchange heredado en un recurso de Azure

### <a name="sign-in-to-your-azure-account-and-select-your-subscription"></a>Iniciar sesión en la cuenta de Azure y seleccione la suscripción
[!INCLUDE [Account](./includes/account-powershell.md)]

### <a name="get-legacy-exchange-peering-for-conversion"></a><a name= get></a>Obtener el emparejamiento de Exchange heredado para la conversión
A continuación se muestra un ejemplo para obtener el emparejamiento de Exchange heredado en la ubicación de emparejamiento de Seattle:

```powershell
$legacyPeering = Get-AzLegacyPeering -Kind Exchange -PeeringLocation "Seattle"
$legacyPeering
```

La respuesta será similar al siguiente ejemplo:
```powershell
    Kind                     : Exchange
    PeeringLocation          : Seattle
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
```

### <a name="convert-legacy-peering"></a>Conversión del emparejamiento heredado
El comando siguiente se puede usar para convertir el emparejamiento de Exchange heredado en un recurso de Azure:

```powershell
$legacyPeering[0] | New-AzPeering `
    -Name "SeattleExchangePeering" `
    -ResourceGroupName "PeeringResourceGroup"

```

&nbsp;
> [!IMPORTANT] 
> Tenga en cuenta que, al convertir el emparejamiento heredado en un recurso de Azure, no se admiten modificaciones.&nbsp;

A continuación se muestra una respuesta de ejemplo en que el aprovisionamiento de un extremo a otro se completó correctamente:

```powershell
    Name                     : SeattleExchangePeering
    Kind                     : Exchange
    Sku                      : Basic_Exchange_Free
    PeeringLocation          : Seattle
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
```
## <a name="additional-resources"></a>Recursos adicionales
Puede obtener una descripción detallada de todos los parámetros ejecutando el siguiente comando:

```powershell
Get-Help Get-AzPeering -detailed
```
Para obtener más información, consulte las [preguntas más frecuentes sobre el emparejamiento de Internet](faqs.md).

## <a name="next-steps"></a>Pasos siguientes

* [Creación o modificación de un emparejamiento de Exchange mediante PowerShell](howto-exchange-powershell.md)

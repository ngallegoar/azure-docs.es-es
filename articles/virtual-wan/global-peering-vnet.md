---
title: Configuración del emparejamiento de VNet global para Azure Virtual WAN | Microsoft Docs
description: Conecte una red virtual en una región distinta a la del centro de Virtual WAN.
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: conceptual
ms.date: 11/04/2019
ms.author: cherylmc
ms.openlocfilehash: 340472f84d2dd2c4f46d180992745a57e8ad1884
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "73587069"
---
# <a name="configure-global-vnet-peering-cross-region-vnet-for-virtual-wan"></a>Configuración del emparejamiento de VNet global (VNet entre regiones) para Azure Virtual WAN

Puede conectar una red virtual en una región distinta a la del centro de Virtual WAN.

## <a name="before-you-begin"></a>Antes de empezar

Compruebe que se cumplen los criterios siguientes:

* La red virtual entre regiones (radio) no está conectada a otro centro de Virtual WAN. Un radio solo puede estar conectado a un centro virtual.
* La red virtual (radio) no contiene una puerta de enlace de red virtual (por ejemplo, una puerta de enlace de red virtual de Azure VPN Gateway o ExpressRoute). Si la red virtual contiene una puerta de enlace de red virtual, debe quitar la puerta de enlace antes de conectar la red virtual radial al centro.

## <a name="register-this-feature"></a><a name="register"></a>Registro de esta característica

Puede registrarse para esta característica mediante PowerShell. Si selecciona "Pruébelo" en el ejemplo siguiente, se abrirá Azure Cloud Shell y no tendrá que instalar los cmdlets de PowerShell localmente en el equipo. Si es necesario, puede cambiar las suscripciones mediante el cmdlet "Select-AzSubscription -SubscriptionId <subid>".

```azurepowershell-interactive
Register-AzProviderFeature -FeatureName AllowCortexGlobalVnetPeering -ProviderNamespace Microsoft.Network
Register-AzResourceProvider -ProviderNamespace 'Microsoft.Network'
```

## <a name="verify-registration"></a><a name="verify"></a>Comprobación del registro

```azurepowershell-interactive
Get-AzProviderFeature -FeatureName AllowCortexGlobalVnetPeering -ProviderNamespace Microsoft.Network
```

## <a name="connect-a-vnet-to-the-hub"></a><a name="hub"></a>Conexión de una red virtual a un centro

En este paso, creará la conexión de emparejamiento entre el centro y la red virtual de varias regiones. Repita estos pasos para cada red virtual que desee conectar.

1. En la página de la red WAN virtual, haga clic en **Conexiones de red virtual**.
2. En la página de conexión de red virtual, haga clic en **+ Agregar conexión**.
3. En la página **Agregar conexión**, rellene los campos siguientes:

    * **Nombre de la conexión**: asigne un nombre a la conexión.
    * **Centros**: seleccione el concentrador que desea asociar a esta conexión.
    * **Suscripción**: compruebe la suscripción.
    * **Red virtual**: seleccione la red virtual que quiere conectar con este concentrador. La red virtual no puede tener una puerta de enlace de red virtual ya existente.
4. Haga clic en **Aceptar** para crear la conexión de emparejamiento.

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre Virtual WAN, consulte [Acerca de Azure Virtual WAN](virtual-wan-about.md).
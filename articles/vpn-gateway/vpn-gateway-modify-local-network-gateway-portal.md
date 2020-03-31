---
title: 'VPN Gateway: modificar la configuración de la dirección IP de puerta de enlace: Portal de Azure'
description: Este artículo explica paso a paso cómo cambiar los prefijos de direcciones IP de la puerta de enlace de red local con Azure Portal.
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: article
ms.date: 06/19/2017
ms.author: cherylmc
ms.openlocfilehash: fdb98242cede36f818604a7a5d713f5f7c75daad
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "75864033"
---
# <a name="modify-local-network-gateway-settings-using-the-azure-portal"></a>Modificación de la configuración de la puerta de enlace de red local mediante Azure Portal

A veces, cambia la configuración de la puerta de enlace de red local AddressPrefix o GatewayIPAddress. Este artículo muestra cómo modificar la configuración de puerta de enlace de red local. También puede modificar estas configuraciones mediante un método diferente, si selecciona una opción diferente de la lista siguiente:

Antes de eliminar la conexión, es posible que quiera descargar la configuración para los dispositivos de conexión con el fin de obtener la PSK definida. De este modo, no tendrá que volver a definirla en el otro lado.

> [!div class="op_single_selector"]
> * [Azure Portal](vpn-gateway-modify-local-network-gateway-portal.md)
> * [PowerShell](vpn-gateway-modify-local-network-gateway.md)
> * [CLI de Azure](vpn-gateway-modify-local-network-gateway-cli.md)
>
>


## <a name="modify-ip-address-prefixes"></a><a name="ipaddprefix"></a>Modificación de los prefijos de direcciones IP

Cuando modifique prefijos de la dirección IP, los pasos que seguir dependen de si la puerta de enlace de red local dispone de una conexión.

[!INCLUDE [modify prefix](../../includes/vpn-gateway-modify-ip-prefix-portal-include.md)]

## <a name="modify-the-gateway-ip-address"></a><a name="gwip"></a>Modificación de la dirección IP de la puerta de enlace

Si el dispositivo VPN al que desea conectarse ha cambiado su dirección IP pública, debe modificar la puerta de enlace de red local para reflejar ese cambio. Cuando cambie la dirección IP pública, los pasos que seguir dependen de si la puerta de enlace de red local dispone de una conexión.

[!INCLUDE [modify gateway IP](../../includes/vpn-gateway-modify-lng-gateway-ip-portal-include.md)]

## <a name="next-steps"></a>Pasos siguientes

Puede comprobar la conexión de la puerta de enlace. Consulte [Comprobación de una conexión de puerta de enlace](vpn-gateway-verify-connection-resource-manager.md).

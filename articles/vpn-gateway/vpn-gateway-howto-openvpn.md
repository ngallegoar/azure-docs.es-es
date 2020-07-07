---
title: 'Cómo configurar OpenVPN en Azure VPN Gateway: PowerShell'
description: Pasos para configurar OpenVPN de Azure VPN Gateway
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: how-to
ms.date: 05/21/2019
ms.author: cherylmc
ms.openlocfilehash: de8d03467b5e44df1b9069c6db31d496785ff32e
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "84983858"
---
# <a name="configure-openvpn-for-azure-point-to-site-vpn-gateway"></a>Configurar OpenVPN para VPN Gateway de punto a sitio de Azure

En este artículo le ayudamos a configurar el **protocolo OpenVPN®** en Azure VPN Gateway. Se supone que ya tiene un entorno de trabajo punto a sitio. En caso contrario, utilice las instrucciones del paso 1 para crear una VPN de punto a sitio.



## <a name="1-create-a-point-to-site-vpn"></a><a name="vnet"></a>1. Configuración de una VPN de punto a sitio

Si no dispone de un entorno de punto a sitio funcionando, siga las instrucciones para crear uno. Consulte [Creación de una VPN de punto a sitio](vpn-gateway-howto-point-to-site-resource-manager-portal.md) para crear y configurar una puerta de enlace de VPN de punto a sitio con autenticación de certificados nativos de Azure. 

> [!IMPORTANT]
> La SKU de nivel Básico no es compatible con OpenVPN.

## <a name="2-enable-openvpn-on-the-gateway"></a><a name="enable"></a>2. Habilitación de OpenVPN en la puerta de enlace

Habilite OpenVPN en la puerta de enlace. Asegúrese de que la puerta de enlace ya está configurada para conexiones de punto a sitio (IKEv2 o SSTP) antes de ejecutar los comandos siguientes:

```azurepowershell-interactive
$gw = Get-AzVirtualNetworkGateway -ResourceGroupName $rgname -name $name
Set-AzVirtualNetworkGateway -VirtualNetworkGateway $gw -VpnClientProtocol OpenVPN
```

## <a name="next-steps"></a>Pasos siguientes

Para configurar clientes para OpenVPN, consulte [Configuración de clientes de OpenVPN](vpn-gateway-howto-openvpn-clients.md).

**"OpenVPN" es una marca comercial de OpenVPN Inc.**

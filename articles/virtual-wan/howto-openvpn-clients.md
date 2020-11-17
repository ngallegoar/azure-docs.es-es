---
title: Configuración de clientes de OpenVPN para Azure Virtual WAN
description: Pasos para configurar clientes de OpenVPN para Azure VPN Gateway
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: how-to
ms.date: 09/22/2020
ms.author: cherylmc
ms.openlocfilehash: 05502b344b1224dff5d12f95b96b05baace98970
ms.sourcegitcommit: b4880683d23f5c91e9901eac22ea31f50a0f116f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/11/2020
ms.locfileid: "94491008"
---
# <a name="configure-an-openvpn-client-for-azure-virtual-wan"></a>Configuración de un cliente de OpenVPN para Azure virtual WAN

Este artículo le ayuda a configurar clientes del **protocolo &reg;OpenVPN**. También puede usar el cliente VPN de Azure para Windows 10 para conectarse a través del protocolo OpenVPN. Si necesita más instrucciones, las encontrará [aquí](openvpn-azure-ad-client.md).

## <a name="before-you-begin"></a>Antes de empezar

Cree una configuración de VPN de usuario (de punto a sitio). Asegúrese de seleccionar "OpenVPN" como el tipo de túnel. Para conocer los pasos, consulte [Creación de una configuración de P2S para Azure Virtual WAN](virtual-wan-point-to-site-portal.md#p2sconfig).

[!INCLUDE [configuration steps](../../includes/vpn-gateway-vwan-config-openvpn-clients.md)]

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre VPN de usuario (de punto a sitio), consulte [Creación de conexiones VPN de usuario](virtual-wan-point-to-site-portal.md).

**"OpenVPN" es una marca comercial de OpenVPN Inc.**

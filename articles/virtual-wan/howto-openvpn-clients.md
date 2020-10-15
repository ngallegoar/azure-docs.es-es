---
title: Configuración de clientes de OpenVPN para Azure Virtual WAN
description: Pasos para configurar clientes de OpenVPN para Azure VPN Gateway
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: how-to
ms.date: 09/22/2020
ms.author: cherylmc
ms.openlocfilehash: 68238a084684aeda557690812403995a4dc1e8c9
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "91317674"
---
# <a name="configure-an-openvpn-client-for-azure-virtual-wan"></a>Configuración de un cliente de OpenVPN para Azure virtual WAN

Este artículo le ayuda a configurar clientes del **protocolo &reg;OpenVPN**.

## <a name="before-you-begin"></a>Antes de empezar

Cree una configuración de VPN de usuario (de punto a sitio). Asegúrese de seleccionar "OpenVPN" como el tipo de túnel. Para conocer los pasos, consulte [Creación de una configuración de P2S para Azure Virtual WAN](virtual-wan-point-to-site-portal.md#p2sconfig).

[!INCLUDE [configuration steps](../../includes/vpn-gateway-vwan-config-openvpn-clients.md)]

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre VPN de usuario (de punto a sitio), consulte [Creación de conexiones VPN de usuario](virtual-wan-point-to-site-portal.md).

**"OpenVPN" es una marca comercial de OpenVPN Inc.**

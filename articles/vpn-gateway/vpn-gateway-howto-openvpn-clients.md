---
title: Cómo configurar los clientes OpenVPN de Azure VPN Gateway | Microsoft Docs
description: Pasos para configurar los clientes OpenVPN de Azure VPN Gateway
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: how-to
ms.date: 12/12/2019
ms.author: cherylmc
ms.openlocfilehash: bdddf097265e7af688175688b6f3214413a90fdc
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "84984091"
---
# <a name="configure-openvpn-clients-for-azure-vpn-gateway"></a>Configuración de los clientes OpenVPN de Azure VPN Gateway

Este artículo le ayuda a configurar clientes del **protocolo &reg;OpenVPN**.

## <a name="before-you-begin"></a>Antes de empezar

Compruebe que ha completado los pasos para configurar OpenVPN para VPN Gateway. Para obtener más información, consulte [Configuración de los clientes OpenVPN de Azure VPN Gateway](vpn-gateway-howto-openvpn.md).

[!INCLUDE [configuration steps](../../includes/vpn-gateway-vwan-config-openvpn-clients.md)]

## <a name="next-steps"></a>Pasos siguientes

Si quiere que los clientes VPN tengan acceso a recursos de otra red virtual, siga las instrucciones del artículo [De red virtual a red virtual](vpn-gateway-howto-vnet-vnet-resource-manager-portal.md) para configurar una conexión de red virtual a red virtual. Asegúrese de habilitar BGP en las puertas de enlace y las conexiones; en caso contrario, el tráfico no fluirá.

**"OpenVPN" es una marca comercial de OpenVPN Inc.**

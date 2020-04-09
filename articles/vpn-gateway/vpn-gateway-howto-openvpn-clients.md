---
title: Cómo configurar los clientes OpenVPN de Azure VPN Gateway | Microsoft Docs
description: Pasos para configurar los clientes OpenVPN de Azure VPN Gateway
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: conceptual
ms.date: 12/12/2019
ms.author: cherylmc
ms.openlocfilehash: 09ff3ccebad0baa4148e68995254c818a29d7bd4
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "80066062"
---
# <a name="configure-openvpn-clients-for-azure-vpn-gateway"></a>Configuración de los clientes OpenVPN de Azure VPN Gateway

Este artículo le ayuda a configurar clientes del **protocolo &reg;OpenVPN**.

## <a name="before-you-begin"></a>Antes de empezar

Compruebe que ha completado los pasos para configurar OpenVPN para VPN Gateway. Para obtener más información, consulte [Configuración de los clientes OpenVPN de Azure VPN Gateway](vpn-gateway-howto-openvpn.md).

[!INCLUDE [configuration steps](../../includes/vpn-gateway-vwan-config-openvpn-clients.md)]

## <a name="next-steps"></a>Pasos siguientes

Si quiere que los clientes VPN tengan acceso a recursos de otra red virtual, siga las instrucciones del artículo [De red virtual a red virtual](vpn-gateway-howto-vnet-vnet-resource-manager-portal.md) para configurar una conexión de red virtual a red virtual. Asegúrese de habilitar BGP en las puertas de enlace y las conexiones; en caso contrario, el tráfico no fluirá.

**"OpenVPN" es una marca comercial de OpenVPN Inc.**

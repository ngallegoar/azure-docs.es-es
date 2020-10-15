---
title: 'Azure Virtual WAN: Perfiles de cliente de VPN de usuario'
description: Esto le ayuda a trabajar con el archivo de perfil del cliente
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: how-to
ms.date: 09/22/2020
ms.author: cherylmc
ms.openlocfilehash: 116ec518f7788e620fc6bd177e535c8f6af99d10
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "91267778"
---
# <a name="working-with-user-vpn-client-profiles"></a>Trabajo con perfiles de cliente de VPN de usuario

El archivo de perfil descargado contiene información necesaria para configurar una conexión VPN. Este artículo le ayudará a obtener y comprender la información necesaria para un perfil de cliente de VPN de usuario.

[!INCLUDE [client profiles](../../includes/vpn-gateway-vwan-vpn-profile-download.md)]

* La **carpeta OpenVPN** contiene el perfil de *ovpn* que debe modificarse para incluir la clave y el certificado. Para más información, consulte [Configuración de clientes de OpenVPN](../virtual-wan/howto-openvpn-clients.md#windows).

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre la VPN de usuario de Virtual WAN, consulte [Creación de una conexión VPN de usuario](virtual-wan-point-to-site-portal.md).

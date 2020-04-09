---
title: 'Azure Virtual WAN: Perfiles de cliente de VPN de usuario'
description: Esto le ayuda a trabajar con el archivo de perfil del cliente
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: article
ms.date: 03/18/2020
ms.author: cherylmc
ms.openlocfilehash: b88be5cccdaeb452288a2cb2f1dd4b4690a8b274
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "80066204"
---
# <a name="working-with-user-vpn-client-profiles"></a>Trabajo con perfiles de cliente de VPN de usuario

El archivo de perfil descargado contiene información necesaria para configurar una conexión VPN. Este artículo le ayudará a obtener y comprender la información necesaria para un perfil de cliente de VPN de usuario.

[!INCLUDE [client profiles](../../includes/vpn-gateway-vwan-vpn-profile-download.md)]

* La **carpeta OpenVPN** contiene el perfil de *ovpn* que debe modificarse para incluir la clave y el certificado. Para más información, consulte [Configuración de clientes de OpenVPN](../virtual-wan/howto-openvpn-clients.md#windows).

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre la VPN de usuario de Virtual WAN, consulte [Creación de una conexión VPN de usuario](virtual-wan-point-to-site-portal.md).

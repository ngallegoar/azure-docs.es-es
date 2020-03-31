---
title: 'Azure VPN Gateway: Sobre los perfiles de cliente de P2S VPN'
description: Esto le ayuda a trabajar con el archivo de perfil del cliente
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: article
ms.date: 03/17/2020
ms.author: cherylmc
ms.openlocfilehash: b78d32dbb7b4e0d9a3b13ff741e6e38c12be0e62
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "79528513"
---
# <a name="about-p2s-vpn-client-profiles"></a>Sobre los perfiles de cliente de P2S VPN

El archivo de perfil descargado contiene información necesaria para configurar una conexión VPN. Este artículo le ayudará a obtener y comprender la información necesaria para un perfil de cliente de VPN.

[!INCLUDE [client profiles](../../includes/vpn-gateway-vwan-vpn-profile-download.md)]

* La **carpeta OpenVPN** contiene el perfil de *ovpn* que debe modificarse para incluir la clave y el certificado. Para más información, consulte [Configurar clientes de OpenVPN para Azure VPN Gateway](vpn-gateway-howto-openvpn-clients.md#windows). Esta carpeta no estará presente en el archivo zip si se selecciona la autenticación de Azure AD en la puerta de enlace de la VPN. En su lugar, azurevpnconfig. XML estará en la carpeta AzureVPN.

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre la ubicación de punto a sitio, consulte [Sobre ubicación de punto a sitio](point-to-site-about.md).

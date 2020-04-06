---
title: Configuración de un túnel de usuario de VPN Always On
titleSuffix: Azure VPN Gateway
description: En este artículo se describe cómo configurar un túnel de usuario de VPN para Grupos de disponibilidad AlwaysOn en VPN Gateway
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: conceptual
ms.date: 03/12/2020
ms.author: cherylmc
ms.openlocfilehash: 56934dd13661d8f623e673e2817e87618675c7ed
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "79502260"
---
# <a name="configure-an-always-on-vpn-user-tunnel"></a>Configuración de un túnel de usuario de VPN para Grupos de disponibilidad AlwaysOn

[!INCLUDE [intro](../../includes/vpn-gateway-vwan-always-on-intro.md)]

## <a name="configure-the-gateway"></a>Configuración de la puerta de enlace

 Use las instrucciones del artículo [Configuración de una conexión VPN de punto a sitio](vpn-gateway-howto-point-to-site-resource-manager-portal.md) para configurar la puerta de enlace de VPN para que use IKEv2 y la autenticación basada en certificados.

## <a name="configure-a-user-tunnel"></a>Configuración de un túnel de usuario

[!INCLUDE [user configuration](../../includes/vpn-gateway-vwan-always-on-user.md)]

## <a name="to-remove-a-profile"></a>Eliminación de un perfil

Para quitar un perfil, use estos pasos:

1. Ejecute el siguiente comando:

   ```powershell
   C:\> Remove-VpnConnection UserTest  
   ```

1. Desconecte la conexión y desactive la opción **Conectar automáticamente**.

   ![Limpieza](./media/vpn-gateway-howto-always-on-user-tunnel/disconnect.jpg)

## <a name="next-steps"></a>Pasos siguientes

Para solucionar los problemas de conexión que puedan producirse, consulte [Problemas de conexión de punto a sitio de Azure](vpn-gateway-troubleshoot-vpn-point-to-site-connection-problems.md).

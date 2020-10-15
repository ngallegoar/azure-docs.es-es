---
title: Configuración de un túnel VPN Always On
titleSuffix: Azure Virtual WAN
description: Pasos para configurar un túnel del dispositivo VPN Always On en Virtual WAN
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: how-to
ms.date: 09/22/2020
ms.author: cherylmc
ms.openlocfilehash: e814487cb4dab9c8c19daab2ea3bb81391d4a98f
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "90983688"
---
# <a name="configure-an-always-on-vpn-device-tunnel-for-virtual-wan"></a>Configuración de un túnel de dispositivo VPN Always On para Virtual WAN

[!INCLUDE [intro](../../includes/vpn-gateway-vwan-always-on-intro.md)]

## <a name="prerequisites"></a>Requisitos previos

Debe crear una configuración de punto a sitio y editar la asignación del concentrador virtual. Consulte las secciones siguientes para ver instrucciones:

* [Creación de una configuración de P2S](virtual-wan-point-to-site-portal.md#p2sconfig)
* [Creación de un centro de conectividad con puerta de enlace P2S](virtual-wan-point-to-site-portal.md#hub)

## <a name="configure-the-device-tunnel"></a>Configuración del túnel de dispositivo

[!INCLUDE [device tunnel](../../includes/vpn-gateway-vwan-always-on-device.md)]

## <a name="to-remove-a-profile"></a>Eliminación de un perfil

Para quitar el perfil, ejecute el siguiente comando:

![La captura de pantalla muestra una ventana de PowerShell en la que se ejecuta el comando Remove-VpnConnection -Name MachineCertTest.](./media/howto-always-on-device-tunnel/cleanup.png)

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre Virtual WAN, consulte las [preguntas más frecuentes](virtual-wan-faq.md).
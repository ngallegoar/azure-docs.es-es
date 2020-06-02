---
title: Asociados y ubicaciones de Azure Virtual WAN | Microsoft Docs
description: En este artículo se incluyen listas de ubicaciones de concentradores y de asociados de Azure Virtual WAN.
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: conceptual
ms.date: 02/12/2020
ms.author: cherylmc
Customer intent: As someone with a networking background, I want to find a Virtual WAN partner
ms.openlocfilehash: e56da52823de169ccb1fc28a0bd7f3ac7efacf49
ms.sourcegitcommit: b396c674aa8f66597fa2dd6d6ed200dd7f409915
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/07/2020
ms.locfileid: "82891035"
---
# <a name="virtual-wan-partners-and-virtual-hub-locations"></a>Ubicaciones de concentradores virtuales y asociados de Azure Virtual WAN

En este artículo se proporciona información sobre las regiones admitidas de Virtual WAN y los asociados de conectividad en concentradores virtuales.

Azure Virtual WAN es un servicio de redes que ofrece conectividad de rama a rama automatizada y optimizada mediante Azure. Azure Virtual WAN permite conectar y configurar los dispositivos de rama para comunicarse con Azure. Esta acción puede realizarse manualmente o con dispositivos de proveedores mediante un asociado de Virtual WAN. El uso de dispositivos de asociados permite la administración de la configuración, facilita el uso y simplifica la conectividad.

La conectividad desde el dispositivo local se establece de forma automática para el concentrador virtual. Un concentrador virtual es una red virtual administrada por Microsoft. El concentrador contiene varios puntos de conexión de servicio para habilitar la conectividad de la red local (vpnsite). Solo se puede tener un concentrador por región.

## <a name="automation-from-connectivity-partners"></a><a name="automation"></a>Automatización de asociados de conectividad

Los dispositivos que se conectan a Azure Virtual WAN tienen automatización integrada para conectarse. Normalmente, esto se configura en la interfaz de usuario de administración de dispositivos (o equivalente), donde se configura la administración de configuración y de conectividad entre el dispositivo de la rama VPN a un punto de conexión de VPN de concentrador virtual de Azure (puerta de enlace VPN).

La automatización de alto nivel siguiente se configura en la consola del dispositivo/centro de administración:

* Permisos adecuados para que el dispositivo acceda al grupo de recursos de Azure Virtual WAN
* Carga del dispositivo de la rama en Azure Virtual WAN
* Descarga automática de la información de conectividad de Azure
* Configuración del dispositivo de rama local 

Algunos asociados de conectividad pueden extender la automatización para incluir la creación de la red virtual del concentrador virtual de Azure y VPN Gateway. Si quiere obtener más información acerca de la automatización, consulte [Directrices de automatización para asociados de Azure Virtual WAN](virtual-wan-configure-automation-providers.md).

## <a name="connectivity-through-partners"></a><a name="partners"></a>Conectividad mediante asociados

[!INCLUDE [partners](../../includes/virtual-wan-partners-include.md)]

En nuestro plan de desarrollo para un futuro próximo están programados los siguientes asociados: 128 Technologies, Arista, Aruba HPE, Cisco Systems, F5 Networks, Open Systems, Oracle SD-WAN y SharpLink.

## <a name="locations"></a><a name="locations"></a>Ubicaciones

[!INCLUDE [regions](../../includes/virtual-wan-regions-include.md)]

## <a name="next-steps"></a>Pasos siguientes

* Para más información sobre Virtual WAN, consulte [Preguntas más frecuentes sobre Virtual WAN](virtual-wan-faq.md).

* Para más información sobre cómo automatizar la conectividad a Azure Virtual WAN, consulte [Directrices de automatización de asociados de Azure Virtual WAN](virtual-wan-configure-automation-providers.md).

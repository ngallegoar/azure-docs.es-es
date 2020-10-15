---
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 11/09/2018
ms.author: cherylmc
ms.openlocfilehash: 622a2f69c2e7b82f2df10d6120ee2dd466961915
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "77211387"
---
Las SKU de puertas de enlace de VPN heredadas (antiguas) son:

* Predeterminado (básico)
* Estándar
* HighPerformance

La VPN Gateway no utiliza la SKU de puerta de enlace de UltraPerformance. Para obtener información acerca de la SKU de UltraPerformance, consulte la documentación de [ExpressRoute](../articles/expressroute/expressroute-about-virtual-network-gateways.md).

Cuando trabaje con las SKU heredadas, tenga en cuenta lo siguiente:

* Si desea utilizar un tipo de VPN PolicyBased, debe utilizar la SKU de nivel Básico. Las VPN PolicyBased (que anteriormente se denominaba enrutamiento estático) no se admiten en otra SKU.
* BGP no es compatible con la SKU de nivel Básico.
* Las configuraciones de la coexistencia de ExpressRoute-VPN Gateway no se admiten en la SKU de nivel Básico.
* Las conexiones de VPN Gateway S2S activo/activo solo pueden configurarse en la SKU HighPerformance.
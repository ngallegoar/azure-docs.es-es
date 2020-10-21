---
title: 'Azure ExpressRoute: Modelos de conectividad'
description: Revise la conectividad entre la red del cliente, Microsoft Azure y los servicios de Microsoft 365. Los clientes pueden usar proveedores MPLS, intercambios de nube y Ethernet.
services: expressroute
author: duongau
ms.service: expressroute
ms.topic: conceptual
ms.date: 10/07/2020
ms.author: duau
ms.openlocfilehash: fb35a03b5dd8780445eb27f485966e3c3452feea
ms.sourcegitcommit: d103a93e7ef2dde1298f04e307920378a87e982a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/13/2020
ms.locfileid: "91978768"
---
# <a name="expressroute-connectivity-models"></a>Modelos de conectividad de ExpressRoute
Puede crear una conexión entre su red local y la nube de Microsoft de cuatro maneras diferentes: [colocalización de CloudExchange](#CloudExchange), [conexión Ethernet de punto a punto](#Ethernet), [conexión universal (IPVPN)](#IPVPN) y [ExpressRoute Direct](#Direct). Los proveedores de conectividad pueden ofrecer uno o varios modelos de conectividad. Puede trabajar en conjunción con su proveedor de conectividad para elegir el modelo que mejor le convenga.
<br><br>

:::image type="content" source="./media/expressroute-connectivity-models/expressroute-connectivity-models-diagram.png" alt-text="Diagrama de modelo de conectividad de ExpressRoute":::

## <a name="co-located-at-a-cloud-exchange"></a><a name="CloudExchange"></a>Ubicación compartida en un intercambio en la nube
Si está colocado en la misma instalación con un intercambio en la nube, puede solicitar conexiones cruzadas virtuales a Microsoft Cloud a través del intercambio de Ethernet del proveedor de la ubicación compartida. Los proveedores de ubicación compartida pueden ofrecer conexiones cruzadas de nivel 2, o conexiones cruzadas de nivel 3 administradas entre la infraestructura en la instalación de ubicación compartida y Microsoft Cloud.

## <a name="point-to-point-ethernet-connections"></a><a name="Ethernet"></a>Conexiones Ethernet de punto a punto
Puede conectar sus centros de datos locales u oficinas a Microsoft Cloud a través de vínculos de Ethernet de punto a punto. Los proveedores de Ethernet de punto a punto pueden ofrecer conexiones de nivel 2 o de nivel 3 administradas entre el sitio y Microsoft Cloud.

## <a name="any-to-any-ipvpn-networks"></a><a name="IPVPN"></a>Redes de conexión universal (IPVPN)
Puede integrar la WAN con Microsoft Cloud. Los proveedores IPVPN (normalmente VPN MPLS) ofrecen conectividad universal entre los centros de datos y las sucursales. Microsoft Cloud puede estar conectada a la WAN de forma que parezca otra sucursal más. Los proveedores de WAN normalmente ofrecen una conectividad de nivel 3 administrada. Las características y capacidades de ExpressRoute son todas idénticas en todos los modelos de conectividad anteriores.

## <a name="direct-from-expressroute-sites"></a><a name="Direct"></a>Directo desde sitios de ExpressRoute
Puede conectarse directamente a la red global de Microsoft en una ubicación de emparejamiento distribuida estratégicamente por todo el mundo. [ExpressRoute Direct](expressroute-erdirect-about.md) proporciona conectividad dual de 100 Gbps o 10 Gbps, que es compatible con la conectividad activa/activa a escala.

## <a name="next-steps"></a>Pasos siguientes
* Obtenga información acerca de las conexiones ExpressRoute y dominios de enrutamiento. Consulte [Circuitos y dominios de enrutamiento de ExpressRoute](expressroute-circuit-peerings.md).
* Información acerca de las características de ExpressRoute. Consulte [Información técnica de ExpressRoute](expressroute-introduction.md)
* Busque un proveedor de servicios. Consulte [Asociados de ExpressRoute de Azure y ubicaciones de emparejamiento](expressroute-locations.md).
* Asegúrese de que se cumplen todos los requisitos previos. Consulte [Requisitos previos de ExpressRoute](expressroute-prerequisites.md).
* Consulte los requisitos de [enrutamiento](expressroute-routing.md), [NAT](expressroute-nat.md) y [QoS](expressroute-qos.md).
* Configure su conexión ExpressRoute.
  * [Creación de un circuito ExpressRoute](expressroute-howto-circuit-portal-resource-manager.md)
  * [Configuración del enrutamiento](expressroute-howto-routing-portal-resource-manager.md)
  * [Vinculación de redes virtuales a circuitos ExpressRoute](expressroute-howto-linkvnet-portal-resource-manager.md)
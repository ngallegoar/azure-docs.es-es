---
title: 'Escenario: universal'
titleSuffix: Azure Virtual WAN
description: 'Escenarios de enrutamiento: universal'
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: conceptual
ms.date: 06/29/2020
ms.author: cherylmc
ms.openlocfilehash: ecc2b3cf236cb2a78fd595189649e7f6b176d709
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85568097"
---
# <a name="scenario-any-to-any"></a>Escenario: Universal

Al trabajar con el enrutamiento de centros virtuales de Virtual WAN, hay bastantes escenarios disponibles. En un escenario universal, cualquier radio puede comunicarse con otro radio. Cuando existen varios centros, el enrutamiento de centro a centro (también conocido como hub-to-hub o interhub) está habilitado de forma predeterminada en Virtual WAN Estándar. 

En este escenario, las conexiones VPN, ExpressRoute y VPN de usuario están asociadas a la misma tabla de rutas. Todas las conexiones VPN, ExpressRoute y VPN de usuario propagan rutas al mismo conjunto de tablas de rutas. Para obtener más información sobre el enrutamiento de centros virtuales, consulte [Acerca del enrutamiento de centros virtuales](about-virtual-hub-routing.md).

## <a name="scenario-architecture"></a><a name="architecture"></a>Arquitectura del escenario

En la **Ilustración 1**, todas las redes virtuales y ramas (VPN, ExpressRoute, P2S) pueden comunicarse entre sí. En un centro virtual, las conexiones funcionan de la siguiente manera:

* Una conexión VPN conecta un sitio VPN a una puerta de enlace de VPN.
* La conexión de red virtual conecta una red virtual a un centro virtual. El enrutador del centro virtual proporciona la funcionalidad de tránsito entre las redes virtuales.
* Una conexión ExpressRoute conecta un circuito de ExpressRoute a una puerta de enlace de ExpressRoute.

Estas conexiones (de forma predeterminada en la creación) están asociadas a la tabla de rutas predeterminada, a menos que defina la configuración de enrutamiento de la conexión en **Ninguno** o en una tabla de rutas personalizada. De forma predeterminada, estas conexiones también se propagan a la tabla de rutas predeterminada. Esto es lo que permite un escenario universal, en el que cualquier radio (red virtual, VPN, ER, P2S) puede comunicarse con cualquier otro.

**Ilustración 1**

:::image type="content" source="./media/routing-scenarios/any-any/figure-1.png" alt-text="ilustración 1":::

## <a name="scenario-workflow"></a><a name="workflow"></a>Flujo de trabajo del escenario

Este escenario está habilitado de forma predeterminada para Virtual WAN Estándar. Si la configuración de rama a rama está deshabilitada en la configuración de WAN, no se permitirá la conectividad entre los radios de las ramas. VPN/ExpressRoute/VPN de usuario se consideran radios de ramas en Virtual WAN.

## <a name="next-steps"></a>Pasos siguientes

* Para más información sobre Virtual WAN, consulte las [preguntas más frecuentes](virtual-wan-faq.md).
* Para obtener más información sobre el enrutamiento de centros virtuales, consulte [Acerca del enrutamiento de centros virtuales](about-virtual-hub-routing.md).

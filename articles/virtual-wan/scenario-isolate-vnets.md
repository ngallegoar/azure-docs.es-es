---
title: 'Escenario: Aislamiento de redes virtuales'
titleSuffix: Azure Virtual WAN
description: Escenarios para enrutamiento - aislamiento de redes virtuales
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: conceptual
ms.date: 06/29/2020
ms.author: cherylmc
ms.openlocfilehash: f43f17a0f3742831920836e448de3ef757f2dfa6
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85568082"
---
# <a name="scenario-isolating-vnets"></a>Escenario: Aislamiento de redes virtuales

Al trabajar con el enrutamiento de centros virtuales de Virtual WAN, hay bastantes escenarios disponibles. En este escenario, el objetivo es evitar que las redes virtuales puedan comunicarse entre sí. Esto se conoce como aislamiento de las redes virtuales. La carga de trabajo dentro de la red virtual permanece aislada y no es capaz de comunicarse con otras redes virtuales, como en un escenario universal. Sin embargo, es necesario que las redes virtuales tengan acceso a todas las ramas (VPN, ER y VPN de usuario). En este escenario, todas las conexiones VPN, ExpressRoute y VPN de usuario están asociadas a la misma tabla de rutas única. Todas las conexiones VPN, ExpressRoute y VPN de usuario propagan rutas al mismo conjunto de tablas de rutas. Para obtener información sobre el enrutamiento de centros virtuales, consulte [Acerca del enrutamiento de centros virtuales](about-virtual-hub-routing.md).

## <a name="scenario-workflow"></a><a name="workflow"></a>Flujo de trabajo del escenario

Para poder configurar este escenario, tenga en cuenta los siguientes pasos:

1. Cree una tabla de rutas personalizada. En el ejemplo, la tabla de ruta es **RT_VNet**. Para crear una tabla de rutas, consulte [Configuración del enrutamiento de centro virtual](how-to-virtual-hub-routing.md). Para obtener más información sobre las tablas de rutas, consulte [Acerca del enrutamiento de centros virtuales](about-virtual-hub-routing.md).
2. Cuando cree la tabla de rutas **RT_VNet**, configure las siguientes opciones:

   * **Asociación**: Seleccione las redes virtuales que desea aislar.
   * **Propagación**: Seleccione la opción para las ramas, e indique que las conexiones de rama (VPN/ER/P2S) propagarán las rutas a esta tabla de rutas.

:::image type="content" source="./media/routing-scenarios/isolated/isolated-vnets.png" alt-text="Redes virtuales aisladas":::

## <a name="next-steps"></a>Pasos siguientes

* Para más información sobre Virtual WAN, consulte las [preguntas más frecuentes](virtual-wan-faq.md).
* Para obtener más información sobre el enrutamiento de centros virtuales, consulte [Acerca del enrutamiento de centros virtuales](about-virtual-hub-routing.md).

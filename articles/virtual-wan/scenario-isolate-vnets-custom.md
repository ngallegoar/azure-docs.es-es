---
title: 'Escenario: Aislamiento personalizado para redes virtuales'
titleSuffix: Azure Virtual WAN
description: 'Escenarios de enrutamiento: impedir que las redes virtuales seleccionadas puedan comunicarse entre sí'
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: conceptual
ms.date: 06/29/2020
ms.author: cherylmc
ms.openlocfilehash: 3719956df0dce62ee69d8e306ff2cad27197616d
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85568088"
---
# <a name="scenario-custom-isolation-for-vnets"></a>Escenario: Aislamiento personalizado para redes virtuales

Al trabajar con el enrutamiento de centros virtuales de Virtual WAN, hay bastantes escenarios disponibles. En un escenario de aislamiento personalizado para redes virtuales, el objetivo es evitar que un conjunto específico de redes virtuales pueda comunicarse con otro conjunto específico de redes virtuales. Sin embargo, es necesario que las redes virtuales tengan acceso a todas las ramas (VPN, ER y VPN de usuario).

En este escenario, las conexiones VPN, ExpressRoute y VPN de usuario (colectivamente se denominan ramas) están asociadas a la misma tabla de rutas (tabla de rutas predeterminada). Todas las conexiones VPN, ExpressRoute y VPN de usuario propagan rutas al mismo conjunto de tablas de rutas. Para obtener más información sobre el enrutamiento de centros virtuales, consulte [Acerca del enrutamiento de centros virtuales](about-virtual-hub-routing.md).

## <a name="scenario-workflow"></a><a name="architecture"></a>Flujo de trabajo del escenario

En la **ilustración 1**, hay conexiones de red virtual azules y rojas.

* Las redes virtuales conectadas en azul pueden comunicarse entre sí, así como conectarse con todas las ramas (VPN/ER/P2S).
* Las redes virtuales rojas pueden comunicarse entre sí, así como comunicarse con todas las ramas (VPN/ER/P2S).

Tenga en cuenta los siguientes pasos al configurar el enrutamiento.

1. Cree dos tablas de rutas personalizadas en Azure Portal, **RT_BLUE** y **RT_RED**.
2. En la tabla de rutas **RT_BLUE**, en:
   * **Asociación**: Seleccione todas las redes virtuales azules.
   * **Propagación**: Para las ramas, seleccione la opción para las ramas, e indique que las conexiones de rama (VPN/ER/P2S) propagarán las rutas a esta tabla de rutas.
3. Repita los mismos pasos para la tabla de rutas **RT_RED** para las redes virtuales rojas y ramas (VPN/ER/P2S).

Esto hará que la configuración de enrutamiento cambie como se muestra en la figura siguiente.

**Ilustración 1**

:::image type="content" source="./media/routing-scenarios/custom-isolation/custom.png" alt-text="ilustración 1":::

## <a name="next-steps"></a>Pasos siguientes

* Para más información sobre Virtual WAN, consulte las [preguntas más frecuentes](virtual-wan-faq.md).
* Para obtener más información sobre el enrutamiento de centros virtuales, consulte [Acerca del enrutamiento de centros virtuales](about-virtual-hub-routing.md).

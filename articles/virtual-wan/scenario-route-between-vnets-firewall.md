---
title: 'Escenario: Enrutamiento personalizado de Azure Firewall para Virtual WAN'
titleSuffix: Azure Virtual WAN
description: 'Escenarios de enrutamiento: enrutar el tráfico entre redes virtuales directamente, pero usar Azure Firewall para flujos de tráfico de redes virtuales a Internet o rama y de rama a redes virtuales'
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: conceptual
ms.date: 06/29/2020
ms.author: cherylmc
ms.openlocfilehash: cdaa594a87d960688638591e6edd525aa3b048f4
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85568325"
---
# <a name="scenario-azure-firewall---custom"></a>Escenario: Azure Firewall: personalizado

Al trabajar con el enrutamiento de centros virtuales de Virtual WAN, hay bastantes escenarios disponibles. En este escenario, el objetivo es enrutar el tráfico entre redes virtuales directamente, pero usar Azure Firewall para flujos de tráfico de redes virtuales a Internet o rama y de rama a redes virtuales. Para obtener información sobre el enrutamiento de centros virtuales, vea [Acerca del enrutamiento de centros virtuales](about-virtual-hub-routing.md).

## <a name="scenario-workflow"></a><a name="workflow"></a>Flujo de trabajo del escenario

En este escenario, desea enrutar el tráfico a través de Azure Firewall para el tráfico de red virtual a Internet, de red virtual a rama o de rama a red virtual, pero desearía pasar directamente al tráfico de red virtual a red virtual. Si usó Azure Firewall Manager, la configuración de enrutamiento se rellena automáticamente en la **Tabla de enrutamiento predeterminada**. El tráfico privado se aplica a la red virtual y las ramas, el tráfico de Internet se aplica a 0.0.0.0/0.

Las conexiones VPN, ExpressRoute y VPN de usuario se denominan colectivamente ramas y están asociadas a la misma tabla de rutas (predeterminada). Todas las conexiones VPN, ExpressRoute y VPN de usuario propagan rutas al mismo conjunto de tablas de rutas. Para poder configurar este escenario, tenga en cuenta los siguientes pasos:

1. Cree una tabla de enrutamiento personalizada **RT_VNET**.
1. Cree una ruta para activar el tráfico de red virtual a Internet y de red virtual a rama: 0.0.0.0/0 con el próximo salto que apunta a Azure Firewall. En la sección Propagación, se asegurará de que las redes virtuales estén seleccionadas, lo que garantizará rutas más específicas y permitirá el flujo de tráfico directo de red virtual a red virtual.

   * En **Asociación:** Seleccione redes virtuales que impliquen que las redes virtuales llegarán al destino según las rutas de esta tabla de enrutamiento.
   * En **Propagación:** Seleccione redes virtuales que impliquen que las redes virtuales se propagan a esta tabla de enrutamiento. En otras palabras, las rutas más específicas se propagarán a esta tabla de enrutamiento, lo que garantiza un flujo de tráfico directo de red virtual a red virtual.

1. Agregue una ruta estática agregada para redes virtuales en la **tabla de enrutamiento predeterminada** para activar el flujo de rama a red virtual a través de Azure Firewall. 

   * Recuerde que las ramas están asociadas y se propagan a la tabla de enrutamiento predeterminada.
   * Las ramas no se propagan a la tabla de enrutamiento RT_VNET. Esto garantiza el flujo de tráfico de red virtual a rama a través de Azure Firewall.

Esto hará que la configuración de enrutamiento cambie como se muestra en la **Figura 1**.

**Ilustración 1**

:::image type="content" source="./media/routing-scenarios/between-vnets-firewall/routing.png" alt-text="Ilustración 1":::

## <a name="next-steps"></a>Pasos siguientes

* Para más información sobre Virtual WAN, consulte las [preguntas más frecuentes](virtual-wan-faq.md).
* Para obtener más información sobre el enrutamiento de centros virtuales, vea [Acerca del enrutamiento de centros virtuales](about-virtual-hub-routing.md).
* Para obtener más información sobre cómo configurar el enrutamiento de centros virtuales, vea [Cómo configurar el enrutamiento de centros virtuales](how-to-virtual-hub-routing.md).

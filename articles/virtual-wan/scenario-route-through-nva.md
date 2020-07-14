---
title: 'Escenario: Enrutamiento del tráfico a través de una aplicación virtual de red'
titleSuffix: Azure Virtual WAN
description: Enrutamiento del tráfico a través de una aplicación virtual de red
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: conceptual
ms.date: 06/29/2020
ms.author: cherylmc
ms.openlocfilehash: 0716ca8f0457ca801098c97dd7a5e68751822d4d
ms.sourcegitcommit: cec9676ec235ff798d2a5cad6ee45f98a421837b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85848093"
---
# <a name="scenario-route-traffic-through-an-nva"></a>Escenario: Enrutamiento del tráfico a través de una aplicación virtual de red

Al trabajar con el enrutamiento de centros virtuales de Virtual WAN, hay bastantes escenarios disponibles. En este escenario de NVA, el objetivo consiste en enrutar el tráfico a través de una NVA (aplicación virtual de red) para rama a red virtual y red virtual a rama. Para obtener información sobre el enrutamiento de centros virtuales, vea [Acerca del enrutamiento de centros virtuales](about-virtual-hub-routing.md).

## <a name="scenario-architecture"></a><a name="architecture"></a>Arquitectura del escenario

En la **Figura 1**, hay dos centros: **Hub 1** y **Hub 2**.

* **Hub 1** y **Hub 2** están conectados directamente a las redes virtuales de NVA **VNET 2** y **VNET 4**.

* Las redes **VNET 5** y **VNET 6** están emparejadas con **VNET 2**.

* Las redes **VNET 7** y **VNET 8** están emparejadas con **VNET 4**.

* Las redes **VNET 5, 6, 7 y 8** son radios indirectos, ya que no están conectadas directamente a un centro virtual.

**Ilustración 1**

:::image type="content" source="./media/routing-scenarios/nva/nva.png" alt-text="Ilustración 1":::

## <a name="scenario-workflow"></a><a name="workflow"></a>Flujo de trabajo del escenario

Para configurar el enrutamiento a través de NVA, estos son los pasos que debe tener en cuenta:

1. Identifique la conexión de red virtual de radio de NVA. En la **Figura 1**, son la **conexión VNET 2 (eastusconn)** y la **conexión VNET 4 (weconn)** .

   Asegúrese de que se han configurado rutas definidas por el usuario:
   * De VNET 5 y 6 a la IP de NVA de VNET 2
   * De VNET 7 y 8 a la IP de NVA de VNET 4 
   
   No es necesario conectar VNET 5, 6, 7 y 8 a los centros virtuales directamente. Asegúrese de que los NSG de VNET 5, 6, 7 y 8 permitan el tráfico para la rama (VPN/ER/P2S) o las VNET conectadas a sus VNET remotas. Por ejemplo, VNET 5 y 6 deben garantizar que los NSG permitan el tráfico para los prefijos de direcciones locales y para VNET 7 y 8, conectadas al centro remoto 2. 

2. Incorpore una entrada de ruta estática agregada para VNET 2, 5 y 6 a la tabla de rutas predeterminada de Hub 1. 

   :::image type="content" source="./media/routing-scenarios/nva/nva-static-expand.png" alt-text="Ejemplo":::

3. Configure una ruta estática para VNET 5 y 6 en la conexión de red virtual de VNET 2. Para establecer la configuración de enrutamiento para una conexión de red virtual, vea [Enrutamiento de centros virtuales](how-to-virtual-hub-routing.md#routing-configuration).

4. Incorpore una entrada de ruta estática agregada para VNET 4, 7 y 8 a la tabla de rutas predeterminada de Hub 1.

5. Repita los pasos 2, 3 y 4 para la tabla de rutas predeterminada de Hub 2.

Esto hará que la configuración de enrutamiento cambie como se muestra en la figura siguiente.

   :::image type="content" source="./media/routing-scenarios/nva/nva-result.png" alt-text="Resultado":::

## <a name="next-steps"></a>Pasos siguientes

* Para más información sobre Virtual WAN, consulte las [preguntas más frecuentes](virtual-wan-faq.md).
* Para obtener más información sobre el enrutamiento de centros virtuales, vea [Acerca del enrutamiento de centros virtuales](about-virtual-hub-routing.md).

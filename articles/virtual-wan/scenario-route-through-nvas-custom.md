---
title: 'Escenario: Enrutamiento del tráfico a través de NVA mediante una configuración personalizada'
titleSuffix: Azure Virtual WAN
description: Este escenario le ayuda a enrutar el tráfico a través de las NVA con una NVA diferente para el tráfico vinculado a Internet.
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: conceptual
ms.date: 06/29/2020
ms.author: cherylmc
ms.openlocfilehash: 3fcad59bcfb75b8de6af14eae7e0bcb4962b17eb
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85568301"
---
# <a name="scenario-route-traffic-through-nvas---custom-preview"></a>Escenario: Enrutamiento del tráfico a través de NVA: personalizado (versión preliminar)

Al trabajar con el enrutamiento de centros virtuales de Virtual WAN, hay bastantes escenarios disponibles. En este escenario de NVA (aplicación virtual de red), el objetivo es enrutar el tráfico a través de una NVA para red virtual < > rama, y usar otra NVA para el tráfico vinculado a Internet. Para obtener información sobre el enrutamiento de centros virtuales, consulte [Acerca del enrutamiento de centros virtuales](about-virtual-hub-routing.md).

## <a name="scenario-architecture"></a><a name="architecture"></a>Arquitectura del escenario

En la **ilustración 1**, hay un centro **Hub 1**.

* **Hub 1** está conectado directamente a las redes virtuales de NVA **VNET 4** y **VNET 5**.

* Se espera que el tráfico entre las redes virtuales 1, 2 y 3 y las ramas (VPN/ER/P2S) pase a través de la **NVA VNET 4** 10.4.0.5.

* Se espera que todo el tráfico vinculado a Internet desde las redes virtuales 1, 2 y 3 pase a través de la **NVA VNET 5** 10.5.0.5.

**Ilustración 1**

:::image type="content" source="./media/routing-scenarios/nva-custom/figure-1.png" alt-text="Ilustración 1":::

## <a name="scenario-workflow"></a><a name="workflow"></a>Flujo de trabajo del escenario

Para configurar el enrutamiento a través de NVA, estos son los pasos que debe tener en cuenta:

1. Para que el tráfico vinculado a Internet pase a través de VNET 5, es necesario que VNET 1, 2 y 3 se conecten directamente a través del emparejamiento de redes virtuales a VNET 5. También necesita un UDR configurado en las redes virtuales para 0.0.0.0/0 y el próximo salto 10.5.0.5. Actualmente, Virtual WAN no permite una NVA de próximo salto en el centro virtual para 0.0.0.0/0.

1. En Azure Portal, navegue hasta el centro virtual y cree una tabla de rutas personalizada **RT_Shared** que aprenderá las rutas mediante la propagación desde todas las redes virtuales y las conexiones de rama. En la **ilustración 2**, esto se representa como una tabla de ruta personalizada **RT_Shared** vacía.

   * **Rutas:** No es necesario agregar ninguna ruta estática.

   * **Asociación:** Seleccione las VNET 4 y 5, lo que significa que las conexiones de VNET 4 y 5 se asociarán a la tabla de rutas **RT_Shared**.

   * **Propagación:** Como desea que todas las ramas y conexiones de red virtual propaguen sus rutas dinámicamente a esta tabla de rutas, seleccione las ramas y todas las redes virtuales.

1. Cree una tabla de rutas **RT_V2B** personalizada para dirigir el tráfico de las VNET 1, 2 y 3 a las ramas.

   * **Rutas:** Agregue una entrada de ruta estática agregada para las ramas (VPN/ER/P2S) (10.2.0.0/16 en la **ilustración 2**) con el próximo salto como conexión de VNET 4. También debe configurar una ruta estática en la conexión de VNET 4 para los prefijos de rama e indicar el próximo salto para que sea la dirección IP específica de la NVA en VNET 4.

   * **Asociación:** Seleccione VNET 1, 2 y 3. Esto hará que las conexiones de red virtual 1, 2 y 3 se asociarán a esta tabla de rutas y podrán aprender las rutas (estáticas y dinámicas mediante propagación) en esta tabla de rutas.

   * **Propagación:** Las conexiones propagan las rutas a las tablas de rutas. Al seleccionar VNET 1, 2 y 3, se habilitará la propagación de rutas desde VNET 1, 2 y 3 a esta tabla de rutas. No es necesario propagar las rutas de las conexiones de rama a RT_V2B, ya que el tráfico de la red virtual de rama pasa por la NVA en VNET 4.
  
1. Edite la tabla de rutas **DefaultRouteTable** predeterminada.

   Todas las conexiones VPN, ExpressRoute y VPN de usuario están asociadas a la tabla de rutas predeterminada. Todas las conexiones VPN, ExpressRoute y VPN de usuario propagan rutas al mismo conjunto de tablas de rutas.

   * **Rutas:** Agregue una entrada de ruta estática agregada para VNET 1, 2 y 3 (10.1.0.0/16 en la **ilustración 2**) con el próximo salto como conexión de VNET 4. También debe configurar una ruta estática en la conexión de VNET 4 para los prefijos agregados de VNET 1, 2 y 3, e indicar el próximo salto para que sea la dirección IP específica de la NVA en VNET 4.

   * **Asociación:** Asegúrese de que la opción para las ramas (VPN/ER/P2S) esté seleccionada, y asegúrese de que las conexiones de rama locales estén asociadas a *defaultroutetable*.

   * **Propagación:** Asegúrese de que la opción para las ramas (VPN/ER/P2S) esté seleccionada y asegúrese de que las conexiones locales propaguen las rutas a *defaultroutetable*.

**Ilustración 2**

:::image type="content" source="./media/routing-scenarios/nva-custom/figure-2.png" alt-text="Ilustración 2":::

## <a name="next-steps"></a>Pasos siguientes

* Para más información sobre Virtual WAN, consulte las [preguntas más frecuentes](virtual-wan-faq.md).
* Para obtener más información sobre el enrutamiento de centros virtuales, consulte [Acerca del enrutamiento de centros virtuales](about-virtual-hub-routing.md).

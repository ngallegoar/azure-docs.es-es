---
title: 'Escenario: Enrutamiento a redes virtuales de servicios compartidos'
titleSuffix: Azure Virtual WAN
description: 'Escenarios de enrutamiento: configure rutas para acceder a una red virtual de servicios compartidos con una carga de trabajo a la que desee que accedan todas las redes virtuales y ramas.'
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: conceptual
ms.date: 06/29/2020
ms.author: cherylmc
ms.openlocfilehash: 64bb4c85399f811c0ab7ff84b297b64734efc491
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85568295"
---
# <a name="scenario-route-to-shared-services-vnets"></a>Escenario: Enrutamiento a redes virtuales de servicios compartidos

Al trabajar con el enrutamiento de centros virtuales de Virtual WAN, hay bastantes escenarios disponibles. En este escenario, el objetivo es configurar rutas para acceder a una red virtual **Servicios compartidos** con la carga de trabajo a la que desee que accedan todas las redes virtuales y ramas (VPN/ER/P2S). Para obtener información sobre el enrutamiento de centros virtuales, consulte [Acerca del enrutamiento de centros virtuales](about-virtual-hub-routing.md).

## <a name="scenario-workflow"></a><a name="workflow"></a>Flujo de trabajo del escenario

Para poder configurar este escenario, tenga en cuenta los siguientes pasos:

1. Identifique la red virtual **Shared Services** (Servicios compartidos).
2. Cree una tabla de rutas personalizada. En el ejemplo, hacemos referencia a esta tabla de rutas como **RT_SHARED**. Para conocer los pasos para crear una tabla de rutas, consulte [Configuración del enrutamiento de centro virtual](how-to-virtual-hub-routing.md). Use los valores siguientes como guía:

   * **Asociación**
     * Para **Redes virtuales, *excepto* la red virtual de servicios compartidos**, seleccione las redes virtuales que desea aislar. Esto hará que todas estas redes virtuales (excepto la red virtual de servicios compartidos) puedan alcanzar el destino en función de las rutas de la tabla de rutas RT_SHARED.

   * **Propagación**
      * En el caso de **Ramas**, propague las rutas a esta tabla de rutas, además de a cualquier otra tabla de rutas que ya haya seleccionado. Debido a este paso, la tabla de rutas RT_SHARED aprenderá las rutas de todas las conexiones de rama (VPN/ER/VPN de usuario).
      * En **Redes virtuales**, seleccione la **red virtual de servicios compartidos**. Debido a este paso, la tabla de rutas RT_SHARED aprenderá las rutas de la conexión de la red virtual de servicios compartidos.

     Esto hará que la configuración de enrutamiento cambie como se muestra en la figura siguiente.

   :::image type="content" source="./media/routing-scenarios/shared-service-vnet/shared-services.png" alt-text="Red virtual de servicios compartidos":::

## <a name="next-steps"></a>Pasos siguientes

* Para más información sobre Virtual WAN, consulte las [preguntas más frecuentes](virtual-wan-faq.md).
* Para obtener más información sobre el enrutamiento de centros virtuales, consulte [Acerca del enrutamiento de centros virtuales](about-virtual-hub-routing.md).

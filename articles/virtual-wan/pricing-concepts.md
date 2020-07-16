---
title: Acerca de los precios de Virtual WAN
titleSuffix: Azure Virtual WAN
description: En este artículo se describen las preguntas más frecuentes sobre los precios de Virtual WAN
services: virtual-wan
author: reyandap
ms.service: virtual-wan
ms.topic: conceptual
ms.date: 06/29/2020
ms.author: reyandap
ms.custom: references_pricing
ms.openlocfilehash: 2d2234ec333746c6f1da59346bdb74247deb616c
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85568100"
---
# <a name="about-virtual-wan-pricing"></a>Acerca de los precios de Virtual WAN

Azure Virtual WAN ofrece varios servicios de red y seguridad juntos en un marco unificado. Se basa en una arquitectura radial, donde los centros están administrados por Microsoft con varios servicios que se proporcionan en el centro, como VPN, ExpressRoute, VPN de usuario (punto a sitio), firewall, enrutamiento, etc.

El precio de cada servicio en Virtual WAN está establecido. Por lo tanto, no se aplica la sugerencia de un precio único a Virtual WAN. La [calculadora de precios de Azure](https://azure.microsoft.com/pricing/calculator/) proporciona un mecanismo para obtener el costo, que se basa en los servicios aprovisionados en una instancia de Virtual WAN. En este artículo se describen las preguntas más frecuentes sobre los precios de Virtual WAN.

>[!NOTE]
>Para más información sobre los precios actuales, consulte [Precios de Virtual WAN](https://azure.microsoft.com/pricing/details/virtual-wan/).
>

## <a name="common-pricing-questions"></a><a name="questions">Preguntas habituales sobre precios</a>

### <a name="what-is-a-scale-unit"></a><a name="scale-unit"></a>¿Qué es una unidad de escalado?

Una **unidad de escalado** proporciona la unidad de la capacidad agregada de sitio a sitio (S2S), punto a sitio (P2S) y ExpressRoute (ER) en un centro virtual. Por ejemplo:

* **Una unidad de escalado de VPN S2S** implica una capacidad total de la puerta de enlace VPN de 500 Mbps (se implementan dos instancias para lograr resistencia) en un centro virtual que cuesta más de 0,361 USD por hora.
* **Una unidad de escalado de ER** implica un total de puerta de enlace de ER de 2 Gbps en el centro virtual que cuesta 0,42 USD por hora.
* **Cinco unidades de escalado de ER** implicaría un total de puerta de enlace de ER de 10 Gbps en una red virtual del centro virtual con un precio de 0,42 USD*5 por hora. ExpressRoute se incrementa en 0,25 USD por hora de la unidad de escalado 6 a la 10.

### <a name="what-is-a-connection-unit"></a><a name="connection-unit">¿Qué es una unidad de conexión?</a>

Una **unidad de conexión** se aplica a cualquier punto de conexión local o que no sea de Microsoft que se conecte a las puertas de enlace de Azure. Para la VPN de sitio a sitio, esto implica ramas. Para la VPN de usuario (de punto a sitio), esto implica a los usuarios remotos. Para ExpressRoute, esto implica conexiones del circuito ExpressRoute.<br>Por ejemplo:

* Una conexión de rama que se conecta a la VPN de Azure en un centro virtual cuesta de 0,05 USD por hora. Por lo tanto, 100 conexiones de rama que se conectan a un centro virtual de Azure costarían 0,05 USD*100 por hora.

* Dos conexiones del circuito ExpressRoute que se conectan a un centro virtual costarían 0,05 USD*2 por hora.

* Tres conexiones de usuario remoto que se conectan a la puerta de enlace P2S del centro virtual de Azure costarían 0,03 USD*3 por hora.

### <a name="how-are-data-transfer-charges-calculated"></a><a name="data-transfer"></a>¿Cómo se calculan los cargos de transferencia de datos?

* No se cobra ningún tráfico entrante de Azure. El tráfico que sale de Azure (a través de VPN, ExpressRoute o conexiones VPN de usuario de punto a sitio) está sujeto a los [cargos de transferencia de datos estándar de Azure](https://azure.microsoft.com/pricing/details/bandwidth/).

* En el caso de los cargos de transferencia de datos entre un centro de Virtual WAN y un centro de Virtual WAN remoto o una red virtual en una región distinta a la del centro de origen, se aplican cargos por transferencia de datos para el tráfico que sale de un centro. Ejemplo: Al tráfico que sale de un centro de Este de EE. UU. se le cobrarán 0,02 USD por GB al centro de Oeste de EE. UU. No se aplica ningún cargo por el tráfico que entra en el centro de Oeste de EE. UU. Las siguientes tablas muestran los cargos.

En las tablas siguientes se usan las siguientes abreviaturas: {NAM: Norteamérica}, {EU: Europa}, {MEA: Oriente Medio y África}, {OC: Oceanía (Centro de Australia y Centro de Australia 2)}, {LATAM: América Latina} 

**Precios dentro del continente (*)**

| Dentro del continente| Precio (USD/GB)|
|---|---|
| NAM a NAM|0,02 USD |
| EU a EU |0,02 USD |
| ASIA-ASIA (excepto China)|0,10 USD |
| MEA a MEA|0,16 USD |
| LATAM-LATAM |0,16 USD |
| OC-OC|0,12 USD |

**Precios entre continentes (*)**

| Entre continentes| Precio (USD/GB)|
|---|---|
| De NAM a EU o EU a NAM |0,07 USD |
| De LATAM a cualquier lugar |0,17 USD |
| De MEA a cualquier lugar |0,17 USD |
| De Oceanía a cualquier lugar |0,12 USD |
| De ASIA (excepto CHINA) a cualquier lugar |0,12 USD |

(*) Pueden aplicarse algunos cargos a partir del 1 de agosto de 2020.

### <a name="what-is-the-difference-between-a-standard-hub-fee-and-a-standard-hub-processing-fee"></a><a name="fee"></a>¿Cuál es la diferencia entre una tarifa de centro estándar y una tarifa de procesamiento de centro estándar?

Virtual WAN se ofrece en dos variedades:

* Una **WAN virtual básica**, donde los usuarios pueden implementar varios centros y disfrutar de la conectividad VPN de sitio a sitio. La WAN virtual básica no tiene funcionalidades avanzadas, como los centros totalmente en malla, la conectividad de ExpressRoute, la conectividad de VPN de punto a sitio y VPN de usuario, la conectividad transitiva de red virtual a red virtual, la conectividad de tránsito de VPN y ExpressRoute, o Azure Firewall, etc. No hay ninguna cuota de procesamiento de datos o cuota base para los centros en una WAN virtual básica.

* Una **WAN virtual estándar** proporciona funcionalidades avanzadas, como centros totalmente en malla, conectividad ExpressRoute, VPN de usuario y conectividad VPN de punto a sitio, conectividad transitiva de red virtual a red virtual, conectividad de tránsito de ExpressRoute y VPN, Azure Firewall, etc. Todo el enrutamiento del centro virtual lo proporciona un enrutador que habilita varios servicios en un centro virtual. Hay una cuota base para el centro, con un precio de 0,25 USD por hora. También hay un cargo por el procesamiento de datos en el enrutador del centro virtual para la conectividad de tránsito de red virtual a red virtual. Consulte la siguiente figura.

 En la ilustración del **ejemplo** siguiente hay dos redes virtuales, VNET 1 y VNET 2. Supongamos que se envían datos de 1 Gbps desde una máquina virtual en VNET 1 a otra máquina virtual de VNET 2. Se aplican los siguientes cargos:

* Cuota base del centro virtual 0,25 USD por hora

* Cuota de procesamiento de datos de centro virtual de 0,02 GB para 1 Gbps

**Ejemplo**

   :::image type="content" source="./media/pricing-concepts/figure-1.png" alt-text="Ejemplo":::

## <a name="next-steps"></a>Pasos siguientes

* Para más información sobre Virtual WAN, consulte las [preguntas más frecuentes](virtual-wan-faq.md).

* Para obtener los precios actuales, consulte [Precios de Virtual WAN](https://azure.microsoft.com/pricing/details/virtual-wan/).

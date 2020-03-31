---
title: 'Azure ExpressRoute: Conexión a Microsoft Cloud mediante Global Reach'
description: En este artículo se explica Global Reach de ExpressRoute.
services: expressroute
author: cherylmc
ms.service: expressroute
ms.topic: conceptual
ms.date: 02/20/2020
ms.author: cherylmc
ms.openlocfilehash: f574576044b7e4ddd34289b5cc45fe5ca353f180
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "77538511"
---
# <a name="expressroute-global-reach"></a>Global Reach de ExpressRoute
ExpressRoute es una forma privada y resistente de conectar las redes locales a Microsoft Cloud. Puede acceder a muchos servicios en la nube de Microsoft, como Azure y Office 365, desde su centro de datos privado o la red corporativa. Por ejemplo, puede tener una sucursal en San Francisco con un circuito ExpressRoute en Silicon Valley y otra sucursal en Londres con un circuito ExpressRoute en la misma ciudad. Ambas sucursales pueden tener conectividad de alta velocidad a recursos de Azure en el sur de Reino Unido y el oeste de EE. UU. Sin embargo, las sucursales no pueden intercambiar datos directamente entre sí. En otras palabras, 10.0.1.0/24 puede enviar datos a 10.0.3.0/24 y 10.0.4.0/24, pero NO a 10.0.2.0/24.

![sin][1]

Con **Global Reach de ExpressRoute**, puede vincular los circuitos ExpressRoute para crear una red privada entre las redes locales. En el ejemplo anterior, con la adición de Global Reach de ExpressRoute, la oficina de San Francisco (10.0.1.0/24) puede intercambiar datos directamente con la oficina de Londres (10.0.2.0/24) mediante los circuitos ExpressRoute existentes y a través de la red global de Microsoft. 

![con][2]

## <a name="use-case"></a>Caso de uso
Global Reach de ExpressRoute está diseñado para complementar la implementación de WAN de su proveedor de servicios y conectar sucursales en todo el mundo. Por ejemplo, si su proveedor de servicios opera principalmente en los Estados Unidos y ha vinculado todas sus sucursales en Estados Unidos, pero el proveedor no opera en Japón ni Hong Kong, con Global Reach de ExpressRoute puede trabajar con un proveedor de servicios local y Microsoft conectará sus sucursales a las de los Estados Unidos mediante ExpressRoute y nuestra red global.

![caso de uso][3]

## <a name="availability"></a>Disponibilidad 
Actualmente, Global Reach de ExpressRoute se admite en los siguientes lugares.

* Australia
* Canadá
* Francia
* Alemania
* RAE de Hong Kong
* Irlanda
* Japón
* Corea
* Países Bajos
* Nueva Zelanda
* Singapur
* Suiza
* Reino Unido
* Estados Unidos

Los circuitos ExpressRoute deben crearse en las [ubicaciones de emparejamiento de ExpressRoute](expressroute-locations.md) de estos países o región. Para habilitar Global Reach de ExpressRoute entre [diferentes regiones geopolíticas](expressroute-locations.md), los circuitos deben ser SKU Premium.

## <a name="next-steps"></a>Pasos siguientes
1. [Más información acerca de Global Reach de ExpressRoute](expressroute-faqs.md)
2. [Cómo habilitar Global Reach de ExpressRoute](expressroute-howto-set-global-reach.md)
3. [Vinculación de un circuito ExpressRoute a una red virtual de Azure](expressroute-howto-linkvnet-arm.md)


<!--Image References-->
[1]: ./media/expressroute-global-reach/1.png "diagrama sin Global Reach"
[2]: ./media/expressroute-global-reach/2.png "diagrama con Global Reach"
[3]: ./media/expressroute-global-reach/3.png "caso de uso de global reach"

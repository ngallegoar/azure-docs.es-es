---
title: 'Interoperabilidad en Azure: configuración de prueba | Microsoft Docs'
description: En este artículo se describe una configuración de prueba que puede usar para analizar la interoperabilidad entre ExpressRoute, una VPN de sitio a sitio y el emparejamiento de redes virtuales de Azure.
documentationcenter: na
services: networking
author: rambk
manager: tracsman
ms.service: virtual-network
ms.topic: article
ms.workload: infrastructure-services
ms.date: 10/18/2018
ms.author: rambala
ms.openlocfilehash: 3aec41a145d2c94a45a453393831902069b9c41b
ms.sourcegitcommit: b0ff9c9d760a0426fd1226b909ab943e13ade330
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/01/2020
ms.locfileid: "80518186"
---
# <a name="interoperability-in-azure--test-setup"></a>Interoperabilidad en Azure: Prueba de configuración

En este artículo se describe una configuración de prueba que permite analizar cómo los servicios de redes de Azure interactúan en el nivel del plano de control y en el nivel del plano de datos. Echemos un vistazo a los componentes de red de Azure:

-   **Azure ExpressRoute**: use el emparejamiento privado de Azure ExpressRoute para conectar directamente espacios IP privados de la red local a las implementaciones de la red virtual de Azure. ExpressRoute puede ayudarle a lograr un ancho de banda mayor y una conexión privada. Muchos asociados de ExpressRoute ofrecen conectividad de ExpressRoute con contratos de nivel de servicio. Para más información acerca de ExpressRoute y su configuración, consulte [Información general sobre ExpressRoute][ExpressRoute].
-   **VPN de sitio a sitio**: puede usar Azure VPN Gateway como una VPN de sitio a sitio para conectar de forma segura una red local a Azure a través de Internet o ExpressRoute. Para más información acerca de cómo configurar una VPN de sitio a sitio para conectarse a Azure, consulte [Configurar VPN Gateway][VPN].
-   **Emparejamiento de VNET**: use el emparejamiento de red virtual (VNet) para establecer conectividad entre las redes virtuales de Azure Virtual Network. Para más información sobre el emparejamiento de VNET, consulte el [tutorial acerca del emparejamiento de redes virtuales][VNet].

## <a name="test-setup"></a>Prueba de configuración

En la siguiente ilustración se muestra la configuración de prueba:

![1][1]

La pieza central de la configuración de prueba es la red virtual del concentrador en la región 1 de Azure. La red virtual del concentrador se conecta a otras redes de las siguientes maneras:

-   La red virtual del concentrador se conecta a la red virtual de radio mediante el uso del emparejamiento de redes virtuales. La red virtual de radio tiene acceso remoto a las dos puertas de enlace de la red virtual del concentrador.
-   La red virtual del concentrador está conectada a la red virtual de sucursal mediante una VPN de sitio a sitio. La conectividad usa EBGP para intercambiar las rutas.
-   La red virtual del concentrador se conecta a la red local Ubicación 1 mediante el uso del emparejamiento privado de ExpressRoute1 como la ruta de acceso principal. Asimismo, usa la conectividad VPN de sitio a sitio como ruta de acceso de respaldo. En el resto de este artículo, se hará referencia a este circuito de ExpressRoute como ExpressRoute 1. De forma predeterminada, los circuitos de ExpressRoute proporcionan conectividad redundante para alta disponibilidad. En ExpressRoute1, la subinterfaz del enrutador perimetral del cliente (CE) secundario que accede al Microsoft Enterprise Edge Router (MSEE) secundario está deshabilitada. Esto se indica mediante una línea de color rojo sobre la flecha de doble línea en el diagrama anterior.
-   La red virtual del concentrador se conecta a la red local Ubicación 2 mediante el uso de otro emparejamiento privado de ExpressRoute. En el resto de este artículo, se hará referencia a este segundo circuito de ExpressRoute como ExpressRoute 2.
-   ExpressRoute1 también conecta la red virtual del concentrador y la red local Ubicación 1 a una red virtual remota en la región 2 de Azure.

## <a name="expressroute-and-site-to-site-vpn-connectivity-in-tandem"></a>Uso simultáneo de ExpressRoute y la conectividad VPN de sitio a sitio

###  <a name="site-to-site-vpn-over-expressroute"></a>VPN de sitio a sitio con ExpressRoute

Puede configurar una VPN de sitio a sitio mediante el emparejamiento de Microsoft con ExpressRoute para intercambiar datos de forma privada entre la red local y las redes virtuales de Azure. Esta configuración le asegura un intercambio de datos con confidencialidad, autenticidad, integridad y antirreproducción. Para más información acerca de cómo configurar una VPN IPsec de sitio a sitio en modo de túnel con emparejamiento de Microsoft de ExpressRoute, consulte [Configuración de una VPN de sitio a sitio a través del emparejamiento de Microsoft de ExpressRoute][S2S-Over-ExR]. 

La limitación principal de la configuración de una VPN de sitio a sitio que use el emparejamiento de Microsoft es el rendimiento. El rendimiento a través del túnel IPsec está limitado por la capacidad de la puerta de enlace de VPN. El rendimiento de la puerta de enlace de VPN es menor que el de ExpressRoute. En este escenario, el uso del túnel IPsec para el tráfico de seguridad alta y con emparejamiento privado para el tráfico restante ayuda a optimizar el uso de ancho de banda de ExpressRoute.

### <a name="site-to-site-vpn-as-a-secure-failover-path-for-expressroute"></a>VPN de sitio a sitio como ruta de acceso de conmutación por error para ExpressRoute

ExpressRoute sirve como par de circuito redundante para garantizar la alta disponibilidad. Puede configurar la conectividad de ExpressRoute con redundancia geográfica en diferentes regiones de Azure. Además, como se ha demostrado en la configuración de prueba, dentro de una región de Azure puede usar una VPN de sitio a sitio para crear una ruta de acceso de conmutación por error para la conectividad de ExpressRoute. Cuando se anuncian los mismos prefijos a través de ExpressRoute y VPN de sitio a sitio, Azure da prioridad a ExpressRoute. Para evitar el enrutamiento asimétrico entre ExpressRoute y VPN de sitio a sitio, la configuración de la red local también debe tener prioridad mediante el uso de la conectividad ExpressRoute antes de VPN de sitio a sitio.

Para más información acerca de cómo configurar conexiones coexistentes de ExpressRoute y una VPN de sitio a sitio, consulte el artículo sobre la [coexistencia de conexiones de ExpressRoute y de sitio a sitio][ExR-S2S-CoEx].

## <a name="extend-back-end-connectivity-to-spoke-vnets-and-branch-locations"></a>Ampliación de la conectividad de back-end a redes virtuales de radio y ubicaciones de sucursal

### <a name="spoke-vnet-connectivity-by-using-vnet-peering"></a>Conectividad de red virtual de radio mediante el emparejamiento de redes virtuales

La arquitectura de red virtual de tipo hub-and-spoke se usa ampliamente. El centro de conectividad es una red virtual de Azure que actúa como punto central de conectividad entre las redes virtuales de radio y la red local. Los radios son redes virtuales que se emparejan con el centro de conectividad y sirven para aislar las cargas de trabajo. El tráfico fluye entre el centro de datos local y el concentrador a través de una conexión de ExpressRoute o VPN. Para más información acerca de la arquitectura, consulte [Implementación de una topología de red en estrella tipo hub-and-spoke en Azure][Hub-n-Spoke].

En el emparejamiento de redes virtuales dentro de una región, las redes virtuales de radio pueden usar las puertas de enlace del centro de conectividad (tanto de VPN como de ExpressRoute) para comunicarse con las redes remotas.

### <a name="branch-vnet-connectivity-by-using-site-to-site-vpn"></a>Conectividad de red virtual de sucursal mediante VPN de sitio a sitio

Es posible que quiera redes virtuales en rama, que están en regiones diferentes, y redes locales para comunicarse entre sí a través de una red virtual de centro de conectividad. La solución de Azure nativa para esta configuración es la conectividad VPN de sitio a sitio mediante una VPN. Una alternativa es usar una aplicación virtual de red (NVA) para el enrutamiento en el centro de conectividad.

Para más información, consulte [¿Qué es VPN Gateway?][VPN] e [Implementación de aplicaciones virtuales de red de alta disponibilidad][Deploy-NVA].

## <a name="next-steps"></a>Pasos siguientes

Obtenga información acerca de los [detalles de configuración][Configuration] de la topología de prueba.

Más información acerca del [análisis del plano de control][Control-Analysis] de la configuración de prueba y de las vistas de otras redes virtuales y VLAN de la topología.

Obtenga información acerca del [análisis del plano de datos][Data-Analysis] de la configuración de prueba y de las vistas de las características de supervisión de red de Azure.

Consulte las [preguntas más frecuentes de ExpressRoute][ExR-FAQ] para información acerca de:
-   Cuántos circuitos de ExpressRoute puede conectar a una puerta de enlace de ExpressRoute.
-   Cuántas puertas de enlace de ExpressRoute puede conectar a un circuito de ExpressRoute.
-   Los límites de escala de ExpressRoute.


<!--Image References-->
[1]: ./media/backend-interoperability/TestSetup.png "Diagrama de la topología de prueba"

<!--Link References-->
[ExpressRoute]: https://docs.microsoft.com/azure/expressroute/expressroute-introduction
[VPN]: https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways
[VNet]: https://docs.microsoft.com/azure/virtual-network/tutorial-connect-virtual-networks-portal
[Configuration]: connectivty-interoperability-configuration.md
[Control-Analysis]: connectivty-interoperability-control-plane.md
[Data-Analysis]: connectivty-interoperability-data-plane.md
[ExR-FAQ]: https://docs.microsoft.com/azure/expressroute/expressroute-faqs
[S2S-Over-ExR]: https://docs.microsoft.com/azure/expressroute/site-to-site-vpn-over-microsoft-peering
[ExR-S2S-CoEx]: https://docs.microsoft.com/azure/expressroute/expressroute-howto-coexist-resource-manager
[Hub-n-Spoke]: https://docs.microsoft.com/azure/architecture/reference-architectures/hybrid-networking/hub-spoke
[Deploy-NVA]: https://docs.microsoft.com/azure/architecture/reference-architectures/dmz/nva-ha



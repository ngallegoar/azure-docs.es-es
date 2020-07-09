---
title: 'Interoperabilidad de las características de conectividad del back-end de Azure: Detalles de configuración | Microsoft Docs'
description: En este artículo se describen los detalles de configuración que puede usar para analizar la interoperabilidad entre ExpressRoute, una VPN de sitio a sitio y el emparejamiento de redes virtuales en Azure.
documentationcenter: na
services: networking
author: rambk
manager: tracsman
ms.service: virtual-network
ms.topic: article
ms.workload: infrastructure-services
ms.date: 10/18/2018
ms.author: rambala
ms.openlocfilehash: 7be326e0f01ed6a00244c0f5b9ed6a960b2b6e0b
ms.sourcegitcommit: 1e6c13dc1917f85983772812a3c62c265150d1e7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/09/2020
ms.locfileid: "86171863"
---
# <a name="interoperability-in-azure-back-end-connectivity-features-test-configuration-details"></a>Interoperabilidad de las características de conectividad del back-end de Azure: Detalles de configuración de prueba

En este artículo se describen los detalles de la [configuración de la prueba][Setup]. La configuración de prueba ayuda a analizar cómo los servicios de redes de Azure interactúan en el nivel del plano de control y en el nivel del plano de datos.

## <a name="spoke-vnet-connectivity-by-using-vnet-peering"></a>Conectividad de red virtual de radio mediante el emparejamiento de redes virtuales

En la siguiente ilustración se muestran los detalles del emparejamiento de redes virtuales de Azure de una red virtual de radio. Para información sobre cómo configurar el emparejamiento entre dos redes virtuales, consulte el artículo sobre la [administración del emparejamiento de redes virtuales][VNet-Config]. Si quiere que la red virtual de radio use las puertas de enlace conectadas a la red virtual del centro de conectividad, seleccione **Usar puertas de enlace remotas**.

[![1]][1]

En la siguiente ilustración se muestran los detalles del emparejamiento de redes virtuales del centro de conectividad. Si quiere que la red virtual del centro permita que la red virtual de radio use las puertas de enlace del centro, seleccione **Allow gateway transit** (Permitir tránsito de puerta de enlace).

[![2]][2]

## <a name="branch-vnet-connectivity-by-using-a-site-to-site-vpn"></a>Conectividad de red virtual de sucursal mediante una VPN de sitio a sitio

Configure la conectividad VPN de sitio a sitio entre las redes virtuales del centro de conectividad y de sucursal mediante las puertas de enlace VPN de Azure VPN Gateway. De forma predeterminada, las puertas de enlace VPN y de Azure ExpressRoute usan el número de sistema autónomo (ASN) privado **65515**. Este valor de ASN se puede cambiar en VPN Gateway. En la configuración de prueba, el valor ASN de la puerta de enlace VPN de red virtual de sucursal se cambia a **65516** para habilitar el enrutamiento EBGP entre el centro de conectividad y la red virtual de sucursal.


[![3]][3]


## <a name="on-premises-location-1-connectivity-by-using-expressroute-and-a-site-to-site-vpn"></a>Conectividad local de la ubicación 1 con ExpressRoute y VPN de sitio a sitio

### <a name="expressroute-1-configuration-details"></a>Detalles de configuración de la ubicación 1 (ExpressRoute)

En la siguiente ilustración se muestra la configuración del circuito ExpressRoute en la región 1 de Azure hacia los enrutadores del lado del cliente (CE) locales de la ubicación 1:

[![4]][4]

En la siguiente ilustración se muestra la configuración de conexión entre el circuito ExpressRoute 1 y el centro de conectividad:

[![5]][5]

En la siguiente lista se muestra la configuración del enrutador del lado del cliente principal para la conectividad de emparejamiento privado de ExpressRoute. (En esta configuración de prueba se utilizan enrutadores Cisco ASR1000 en el lado del cliente). Cuando los circuitos de VPN de sitio a sitio y ExpressRoute se configuran en paralelo para conectar una red local con Azure, Azure da prioridad al circuito de ExpressRoute de forma predeterminada. Para evitar el enrutamiento asimétrico, la red local también debe dar prioridad a la conectividad de ExpressRoute sobre VPN de sitio a sitio. La siguiente configuración establece prioridades mediante el atributo BGP **local-preference**:

```config
interface TenGigabitEthernet0/0/0.300
 description Customer 30 private peering to Azure
 encapsulation dot1Q 30 second-dot1q 300
 ip vrf forwarding 30
 ip address 192.168.30.17 255.255.255.252
!
interface TenGigabitEthernet1/0/0.30
 description Customer 30 to south bound LAN switch
 encapsulation dot1Q 30
 ip vrf forwarding 30
 ip address 192.168.30.0 255.255.255.254
 ip ospf network point-to-point
!
router ospf 30 vrf 30
 router-id 10.2.30.253
 redistribute bgp 65021 subnets route-map BGP2OSPF
 network 192.168.30.0 0.0.0.1 area 0.0.0.0
default-information originate always
 default-metric 10
!
router bgp 65021
 !
 address-family ipv4 vrf 30
  network 10.2.30.0 mask 255.255.255.128
  neighbor 192.168.30.18 remote-as 12076
  neighbor 192.168.30.18 activate
  neighbor 192.168.30.18 next-hop-self
  neighbor 192.168.30.18 soft-reconfiguration inbound
  neighbor 192.168.30.18 route-map prefer-ER-over-VPN in
  neighbor 192.168.30.18 prefix-list Cust30_to_Private out
 exit-address-family
!
route-map prefer-ER-over-VPN permit 10
 set local-preference 200
!
ip prefix-list Cust30_to_Private seq 10 permit 10.2.30.0/25
!
```

### <a name="site-to-site-vpn-configuration-details"></a>Detalles de configuración de VPN de sitio a sitio

En la siguiente lista se muestra la configuración del enrutador del lado del cliente principal para la conectividad VPN de sitio a sitio:

```config
crypto ikev2 proposal Cust30-azure-proposal
 encryption aes-cbc-256 aes-cbc-128 3des
 integrity sha1
 group 2
!
crypto ikev2 policy Cust30-azure-policy
 match address local 66.198.12.106
 proposal Cust30-azure-proposal
!
crypto ikev2 keyring Cust30-azure-keyring
 peer azure
  address 52.168.162.84
  pre-shared-key local IamSecure123
  pre-shared-key remote IamSecure123
!
crypto ikev2 profile Cust30-azure-profile
 match identity remote address 52.168.162.84 255.255.255.255
 identity local address 66.198.12.106
 authentication local pre-share
 authentication remote pre-share
 keyring local Cust30-azure-keyring
!
crypto ipsec transform-set Cust30-azure-ipsec-proposal-set esp-aes 256 esp-sha-hmac
 mode tunnel
!
crypto ipsec profile Cust30-azure-ipsec-profile
 set transform-set Cust30-azure-ipsec-proposal-set
 set ikev2-profile Cust30-azure-profile
!
interface Loopback30
 ip address 66.198.12.106 255.255.255.255
!
interface Tunnel30
 ip vrf forwarding 30
 ip address 10.2.30.125 255.255.255.255
 tunnel source Loopback30
 tunnel mode ipsec ipv4
 tunnel destination 52.168.162.84
 tunnel protection ipsec profile Cust30-azure-ipsec-profile
!
router bgp 65021
 !
 address-family ipv4 vrf 30
  network 10.2.30.0 mask 255.255.255.128
  neighbor 10.10.30.254 remote-as 65515
  neighbor 10.10.30.254 ebgp-multihop 5
  neighbor 10.10.30.254 update-source Tunnel30
  neighbor 10.10.30.254 activate
  neighbor 10.10.30.254 soft-reconfiguration inbound
 exit-address-family
!
ip route vrf 30 10.10.30.254 255.255.255.255 Tunnel30
```

## <a name="on-premises-location-2-connectivity-by-using-expressroute"></a>Conectividad local de la ubicación 2 con ExpressRoute

Un segundo circuito de ExpressRoute, más próximo a la ubicación 2 local, conecta la ubicación 2 local al centro de conectividad. En la siguiente ilustración se muestra la segunda configuración de ExpressRoute:

[![6]][6]

En la siguiente ilustración se muestra la configuración de la conexión entre el segundo circuito de ExpressRoute y el centro de conectividad:

[![7]][7]

ExpressRoute 1 conecta el centro de conectividad y la ubicación 1 local con una red virtual remota de otra región de Azure:

[![8]][8]

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

Más información acerca del [análisis del plano de control][Control-Analysis] de la configuración de prueba y de las vistas de otras redes virtuales y VLAN de la topología.

Más información acerca del [análisis del plano de datos][Data-Analysis] de la configuración de prueba y de las vistas de las características de supervisión de red de Azure.

Consulte las [preguntas más frecuentes de ExpressRoute][ExR-FAQ] para información acerca de:
-   Cuántos circuitos de ExpressRoute puede conectar a una puerta de enlace de ExpressRoute.
-   Cuántas puertas de enlace de ExpressRoute puede conectar a un circuito de ExpressRoute.
-   Los límites de escala de ExpressRoute.


<!--Image References-->
[1]: ./media/backend-interoperability/SpokeVNet_peering.png "Emparejamiento de VNET de la red virtual de radio"
[2]: ./media/backend-interoperability/HubVNet-peering.png "Emparejamiento de VNET de la red virtual del concentrador"
[3]: ./media/backend-interoperability/BranchVNet-VPNGW.png "Configuración de la puerta de enlace de VPN de una red virtual de sucursal"
[4]: ./media/backend-interoperability/ExR1.png "Configuración de ExpressRoute 1"
[5]: ./media/backend-interoperability/ExR1-Hub-Connection.png "Configuración de la conexión de ExpressRoute 1 a la puerta de enlace ExR del centro de conectividad"
[6]: ./media/backend-interoperability/ExR2.png "Configuración de ExpressRoute 2"
[7]: ./media/backend-interoperability/ExR2-Hub-Connection.png "Configuración de la conexión de ExpressRoute 2 a la puerta de enlace ExR del centro de conectividad"
[8]: ./media/backend-interoperability/ExR2-Remote-Connection.png "Configuración de la conexión de ExpressRoute 2 a la puerta de enlace ExR de la red virtual remota"

<!--Link References-->
[Setup]: https://docs.microsoft.com/azure/networking/connectivty-interoperability-preface
[ExpressRoute]: https://docs.microsoft.com/azure/expressroute/expressroute-introduction
[VPN]: https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways
[VNet]: https://docs.microsoft.com/azure/virtual-network/tutorial-connect-virtual-networks-portal
[Configuration]: https://docs.microsoft.com/azure/networking/connectivty-interoperability-configuration
[Control-Analysis]:https://docs.microsoft.com/azure/networking/connectivty-interoperability-control-plane
[Data-Analysis]: https://docs.microsoft.com/azure/networking/connectivty-interoperability-data-plane
[ExR-FAQ]: https://docs.microsoft.com/azure/expressroute/expressroute-faqs
[S2S-Over-ExR]: https://docs.microsoft.com/azure/expressroute/site-to-site-vpn-over-microsoft-peering
[ExR-S2S-CoEx]: https://docs.microsoft.com/azure/expressroute/expressroute-howto-coexist-resource-manager
[Hub-n-Spoke]: https://docs.microsoft.com/azure/architecture/reference-architectures/hybrid-networking/hub-spoke
[Deploy-NVA]: https://docs.microsoft.com/azure/architecture/reference-architectures/dmz/nva-ha
[VNet-Config]: https://docs.microsoft.com/azure/virtual-network/virtual-network-manage-peering



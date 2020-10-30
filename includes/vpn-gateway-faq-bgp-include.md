---
title: archivo de inclusión
description: archivo de inclusión
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 09/17/2020
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 649c5805c600b6282be6d05fefb59cecaf249f4f
ms.sourcegitcommit: 59f506857abb1ed3328fda34d37800b55159c91d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/24/2020
ms.locfileid: "92526084"
---
### <a name="is-bgp-supported-on-all-azure-vpn-gateway-skus"></a>¿Se admite BGP en todas las SKU de Azure VPN Gateway?
El protocolo de puerta de enlace de borde se admite en todas las SKU de Azure VPN Gateway, excepto en la SKU básica.

### <a name="can-i-use-bgp-with-azure-policy-vpn-gateways"></a>¿Se puede usar BGP con puertas de enlace de VPN de Azure Policy?
No, BGP solo es compatible con puertas de enlace de VPN basadas en rutas.

### <a name="what-asns-autonomous-system-numbers-can-i-use"></a>¿Qué ASN (números de sistema autónomo) se pueden usar?
Se pueden usar los ASN públicos propios o los ASN privados tanto para las redes locales como para las redes virtuales de Azure. No se pueden usar los rangos reservados por Azure o IANA.

Los siguientes ASN están reservados por Azure o IANA:
* ASN reservados por Azure:
  * ASN públicos: 8074, 8075, 12076
  * ASN privados: 65515, 65517, 65518, 65519, 65520
* ASN [reservados por IANA](http://www.iana.org/assignments/iana-as-numbers-special-registry/iana-as-numbers-special-registry.xhtml):
   * 23456, 64496-64511, 65535-65551 y 429496729

Al conectarse a las puertas de enlace de VPN de Azure, no puede especificar estos ASN para los dispositivos VPN locales.

### <a name="can-i-use-32-bit-4-byte-asns"></a>¿Se pueden usar ASN de 32 bits (4 bytes)?
Sí, VPN Gateway ahora admite ASN de 32 bits (4 bytes). Para usar ASN en formato decimal en la configuración, use PowerShell, la CLI de Azure o el SDK de Azure.

### <a name="what-private-asns-can-i-use"></a>¿Qué ASN privados se pueden usar?
Estos son los rangos de ASN privados que se pueden usar:

* 64512-65514 y 65521-65534

Ni IANA ni Azure han reservado estos ASN para su uso, por lo que se pueden utilizar para asignarlos a una puerta de enlace de VPN de Azure.

### <a name="what-address-does-vpn-gateway-use-for-bgp-peer-ip"></a>¿Qué dirección utiliza VPN Gateway para la IP del par de BGP?

De manera predeterminada, VPN Gateway asigna una única dirección IP del rango de *GatewaySubnet* para las puertas de enlace de VPN en modo activo/en espera, o bien dos direcciones IP para puertas de enlace de VPN en modo activo/activo. Estas direcciones se asignan automáticamente al crear la puerta de enlace de VPN. Para obtener la dirección IP de BGP real asignada, utilice PowerShell, o bien búsquela en Azure Portal. En PowerShell, use **Get-AzVirtualNetworkGateway** y busque la propiedad **bgpPeeringAddress** . En Azure Portal, en la página **Configuración de puerta de enlace** , fíjese en la propiedad **Configurar ASN BGP** .

Si los enrutadores locales de la red privada virtual utilizan las direcciones IP (169.254.x.x) de **APIPA** como direcciones IP del protocolo de puerta de enlace de borde, es preciso especificar una **dirección IP de BGP de APIPA de Azure** adicional en la puerta de enlace de VPN de Azure. Azure VPN Gateway selecciona la dirección de APIPA que se va a usar con el par BGP de APIPA especificado de la puerta de enlace de red local, o bien una dirección IP privada, en el caso del par BGP local que no sea APIPA. Para más información, consulte el artículo sobre la [configuración del protocolo de puerta de enlace de borde](../articles/vpn-gateway/bgp-howto.md).

### <a name="what-are-the-requirements-for-the-bgp-peer-ip-addresses-on-my-vpn-device"></a>¿Cuáles son los requisitos de las direcciones IP del par BGP en mi dispositivo VPN?
La dirección del par BGP local no debe coincidir con la dirección IP pública del dispositivo VPN ni con el espacio de direcciones de red virtual de la puerta de enlace de red virtual. Use otra dirección IP en el dispositivo VPN para la dirección IP del par BGP. Puede ser una dirección asignada a la interfaz de bucle invertido del dispositivo (puede ser una dirección IP normal o una dirección de APIPA). Si el dispositivo usa una dirección de APIPA para el protocolo de puerta de enlace de borde, es preciso especificar una dirección IP de APIPA BGP en una puerta de enlace de VPN de Azure, como se describe en el artículo sobre la [configuración del protocolo de puerta de enlace de borde](../articles/vpn-gateway/bgp-howto.md). Especifique esta dirección en la puerta de enlace de red local correspondiente que representa la ubicación.

### <a name="what-should-i-specify-as-my-address-prefixes-for-the-local-network-gateway-when-i-use-bgp"></a>¿Qué debo especificar como mis prefijos de dirección para la puerta de enlace de red local al utilizar BGP?

> [!IMPORTANT]
>
>Aquí se ha producido un cambio, con respecto al requisito que estaba documentado. Si usa el protocolo de puerta de enlace de borde para una conexión, deje vacío el campo **Espacio de direcciones** del recurso de la puerta de enlace de red local correspondiente. Azure VPN Gateway agrega internamente una ruta de host a la IP del par BGP local a través del túnel IPsec. No agregue la ruta /32 en el campo **Espacio de direcciones** . Es redundante y si usa una dirección de APIPA como la IP del protocolo de puerta de enlace de borde del dispositivo VPN local, no podrá agregarla a este campo. Si agrega otros prefijos en el campo **Espacio de direcciones** , se agregan como rutas estáticas en la puerta de enlace de VPN de Azure, además de las rutas aprendidas a través del protocolo de puerta de enlace de borde.
>

### <a name="can-i-use-the-same-asn-for-both-on-premises-vpn-networks-and-azure-virtual-networks"></a>¿Se puede usar el mismo ASN para redes VPN locales y redes virtuales de Azure?
No, si va a conectar redes locales y redes virtuales de Azure con el protocolo de puerta de enlace de borde, debe asignar ASN diferentes a cada una de ellas. Las puertas de enlace de VPN de Azure tienen un ASN predeterminado de 65515 asignado, independientemente de que BGP esté habilitado, o no, para la conectividad entre locales. Para invalidar este valor predeterminado, asigne otro ASN al crear la puerta de enlace de VPN o cambie el ASN después de crearla. Tendrá que asignar los ASN locales a las puertas de enlace de red local de Azure correspondientes.

### <a name="what-address-prefixes-will-azure-vpn-gateways-advertise-to-me"></a>¿Qué prefijos de direcciones de puertas de enlace de VPN de Azure se me anunciarán?
Las puertas de enlace anuncian las siguientes rutas a los dispositivos BGP locales:

* Los prefijos de direcciones de red virtual.
* Los prefijos de dirección de las puertas de enlace de red local conectadas a la puerta de enlace de VPN de Azure.
* Las rutas aprendidas de otras sesiones de emparejamiento de BGP conectadas a la puerta de enlace de VPN de Azure, excepto la ruta predeterminada o las rutas que se superpongan con cualquier prefijo de red virtual.

### <a name="how-many-prefixes-can-i-advertise-to-azure-vpn-gateway"></a>¿Cuántos prefijos se pueden anunciar en Azure VPN Gateway?
Azure VPN Gateway admite hasta 4000 prefijos. Si el número de prefijos supera el límite, se elimina la sesión BGP.

### <a name="can-i-advertise-default-route-00000-to-azure-vpn-gateways"></a>¿Puedo anunciar la ruta predeterminada (0.0.0.0/0) para puertas de enlace de VPN de Azure?
Sí. Tenga en cuenta que esto obliga a que todo el tráfico de salida de red virtual se dirija hacia el sitio local. También impide que las máquinas virtuales de la red virtual acepten directamente la comunicación pública desde Internet, como RDP o SSH desde Internet a las máquinas virtuales.

### <a name="can-i-advertise-the-exact-prefixes-as-my-virtual-network-prefixes"></a>¿Puedo anunciar los mismos prefijos que mis prefijos de red virtual?

No, Azure bloqueará o filtrará el anuncio de los mismos prefijos que los de cualquiera de sus otros prefijos de dirección de red virtual. Sin embargo, se puede anunciar un prefijo que sea un superconjunto de lo que contenga su red virtual. 

Por ejemplo, si una red virtual ha usado el espacio de direcciones 10.0.0.0/16, se puede anunciar el 10.0.0.0/8. Sin embargo, no se pueden anunciar el 10.0.0.0/16 ni el 10.0.0.0/24.

### <a name="can-i-use-bgp-with-my-connections-between-virtual-networks"></a>¿Se puede usar el protocolo de puerta de enlace de borde con las conexiones entre redes virtuales?
Sí, BGP se puede usar tanto para conexiones entre locales como para conexiones entre redes virtuales.

### <a name="can-i-mix-bgp-with-non-bgp-connections-for-my-azure-vpn-gateways"></a>¿Puedo combinar BGP con conexiones no de BGP para mi puertas de enlace de VPN de Azure?
Sí, puede mezclar conexiones de BGP y no de BGP para la misma puerta de enlace de VPN de Azure.

### <a name="does-azure-vpn-gateway-support-bgp-transit-routing"></a>¿Admite Azure VPN Gateway el enrutamiento del tránsito de BGP?
Sí, se admite el enrutamiento del tránsito de BGP, pero las puertas de enlace de VPN de Azure no anuncian las rutas predeterminadas a otros pares de BGP. Para habilitar el enrutamiento del tránsito a través de varias puertas de enlace de VPN de Azure, es preciso habilitar el protocolo de puerta de enlace de borde en todas las conexiones intermedias entre las redes virtuales. Para más información, consulte [Acerca de BGP](../articles/vpn-gateway/vpn-gateway-bgp-overview.md).

### <a name="can-i-have-more-than-one-tunnel-between-an-azure-vpn-gateway-and-my-on-premises-network"></a>¿Puedo tener más de un túnel entre una puerta de enlace de VPN de Azure y mi red local?
Sí, puede establecer varios túneles VPN de sitio a sitio (S2S) entre una puerta de enlace de VPN de Azure y la red local. Tenga en cuenta que todos estos túneles se incluyen en el recuento total de túneles de las puertas de enlace de la VPN de Azure y el protocolo de puerta de enlace de borde debe habilitarse en los dos túneles.

Por ejemplo, si tiene dos túneles redundantes entre una puerta de enlace de VPN de Azure y una de las redes locales, consumen dos de los túneles de la cuota total para la puerta de enlace de VPN de Azure.

### <a name="can-i-have-multiple-tunnels-between-two-azure-virtual-networks-with-bgp"></a>¿Puedo tener varios túneles entre dos redes virtuales de Azure con BGP?
Sí, pero al menos una de las puertas de enlace de la red virtual debe estar en una configuración activa-activa.

### <a name="can-i-use-bgp-for-s2s-vpn-in-an-azure-expressroute-and-s2s-vpn-coexistence-configuration"></a>¿Se puede usar BGP para VPN de sitio a sitio en una configuración en que coexisten una VPN de sitio a sitio y ExpressRoute?
Sí. 

### <a name="what-should-i-add-to-my-on-premises-vpn-device-for-the-bgp-peering-session"></a>¿Qué debo agregar a mi dispositivo VPN local para la sesión de emparejamiento BGP?
Agregue una ruta de host de la dirección IP del par BGP de Azure al dispositivo VPN. Esta ruta apunta al túnel VPN de sitio a sitio de IPsec. Por ejemplo, si la dirección IP del par VPN de Azure es 10.12.255.30, debe agregar una ruta de host para 10.12.255.30 con una interfaz de próximo salto de la interfaz de túnel IPsec coincidente en el dispositivo VPN.

### <a name="does-the-virtual-network-gateway-support-bfd-for-s2s-connections-with-bgp"></a>¿Admite la puerta de enlace de red virtual BFD para las conexiones S2S con BGP?
No. Detección de reenvío bidireccional (BFD) es un protocolo que se puede usar con el protocolo de puerta de enlace de borde para detectar el tiempo de inactividad del vecino más rápidamente que si se usan conexiones persistentes BGP estándar. BFD usa temporizadores de subsegundo diseñados para funcionar en entornos de LAN, pero no en las conexiones de red de área extensa o de la red pública de Internet.

En el caso de las conexiones a través de la red pública de Internet, no es inusual que algunos paquetes se retrasen, o incluso se eliminen, por lo que la introducción de estos agresivos temporizadores puede agregar inestabilidad, y dicha inestabilidad podría provocar que BGP amortiguara las rutas. Como alternativa, puede configurar un dispositivo local con temporizadores inferiores al intervalo de conexión persistente predeterminado de 60 segundos el temporizador de retención de 180 segundos, con lo que se obtiene un menor tiempo de convergencia.

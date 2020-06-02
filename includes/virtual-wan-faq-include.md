---
title: archivo de inclusión
description: archivo de inclusión
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: include
ms.date: 03/24/2020
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 827a2d6dc8a3622c17cdbcdfb179a3ea0f434f6f
ms.sourcegitcommit: ac4a365a6c6ffa6b6a5fbca1b8f17fde87b4c05e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/10/2020
ms.locfileid: "83006461"
---
### <a name="does-the-user-need-to-have-hub-and-spoke-with-sd-wanvpn-devices-to-use-azure-virtual-wan"></a>¿El usuario necesita disponer de una topología radial con los dispositivos SD-WAN/VPN para usar Azure Virtual WAN?

Virtual WAN proporciona muchas funcionalidades integradas en un único panel, como la conectividad VPN de sitio a sitio, la conectividad de usuario y P2S, la conectividad de ExpressRoute, la conectividad de red virtual, la interconectividad de ExpressRoute de VPN, la conectividad transitiva de red virtual a red virtual, el enrutamiento centralizado, el firewall de Azure y la seguridad del administrador del firewall, la supervisión, el cifrado de ExpressRoute y muchas otras funcionalidades. No es necesario disponer de todos estos casos de uso para empezar a usar Virtual WAN. Simplemente puede empezar a trabajar con uno de ellos. La arquitectura de Virtual WAN es una arquitectura radial con escalado y rendimiento integrados, donde las ramas (dispositivos VPN/SD-WAN), los usuarios (clientes de VPN de Azure, clientes de openVPN o IKEv2), los circuitos de ExpressRoute y las redes virtuales sirven como radios para los centros de conectividad virtuales. Todos los centros de conectividad están conectados en una malla completa en una red WAN virtual estándar, lo que facilita al usuario el uso de la red troncal de Microsoft para la conectividad de cualquier tipo (cualquier radio). En el caso de la topología radial de los dispositivos SD-WAN/VPN, los usuarios pueden configurarla manualmente en el portal de Azure Virtual WAN o usar el CPE del asociado de Virtual WAN (SD-WAN/VPN) para configurar la conectividad con Azure. Los asociados de Virtual WAN proporcionan automatización para la conectividad, que es la capacidad de exportar la información del dispositivo a Azure, descargar la configuración de Azure y establecer conectividad con el centro de conectividad de Azure Virtual WAN. Para la conectividad de punto a sitio/VPN de usuario, se admiten el [cliente de VPN de Azure](https://go.microsoft.com/fwlink/?linkid=2117554), OpenVPN o el cliente de IKEv2. 

### <a name="what-client-does-the-azure-virtual-wan-user-vpn-point-to-site-support"></a>¿Qué cliente admite la red privada virtual de usuario (de punto a sitio) de Azure Virtual WAN?

Virtual WAN admite el [cliente de VPN de Azure](https://go.microsoft.com/fwlink/?linkid=2117554), el cliente de OpenVPN o cualquier cliente de IKEv2. La autenticación de Azure AD se admite con el cliente de VPN de Azure. Como mínimo, se requiere la versión 17763.0 o superior del sistema operativo de cliente de Windows 10.  Los clientes de OpenVPN pueden admitir la autenticación basada en certificados. Una vez que se ha seleccionado la autenticación basada en certificados en la puerta de enlace, verá el archivo .ovpn que se va a descargar en el dispositivo. Con IKEv2, se admiten tanto la autenticación basada en certificados como la de RADIUS. 

### <a name="for-user-vpn-point-to-site--why-is-the-p2s-client-pool-split-into-two-routes"></a>En el caso de la red privada virtual de usuario (de punto a sitio), ¿por qué el grupo de clientes de P2S se divide en dos rutas?

Cada puerta de enlace tiene dos instancias; la división se produce para que cada instancia de la puerta de enlace pueda asignar de forma independiente las direcciones IP de cliente para los clientes conectados, y el tráfico de la red virtual se redirija a la instancia de puerta de enlace correcta para evitar el salto de instancias entre puertas de enlace.

### <a name="how-do-i-add-dns-servers-for-p2s-clients"></a>¿Cómo puedo agregar servidores DNS para los clientes de P2S?

Hay dos opciones para agregar servidores DNS para los clientes de P2S.

1. Abra una incidencia de soporte técnico con Microsoft para pedir que se agreguen los servidores DNS al concentrador.
2. O bien, si usa el cliente de VPN de Azure para Windows 10, puede modificar el archivo XML de perfil descargado y agregar las etiquetas **\<dnsservers>\<dnsserver> \</dnsserver>\</dnsservers>** antes de importarlo.

```
<azvpnprofile>
<clientconfig>

    <dnsservers>
        <dnsserver>x.x.x.x</dnsserver>
        <dnsserver>y.y.y.y</dnsserver>
    </dnsservers>
    
</clientconfig>
</azvpnprofile>
```

### <a name="for-user-vpn-point-to-site--how-many-clients-are-supported"></a>En el caso de la red privada virtual de usuario (de punto a sitio), ¿cuántos clientes se admiten?

Cada puerta de enlace de red privada virtual de usuario P2S tiene dos instancias, y cada instancia admite hasta un número determinado de usuarios a medida que cambia la unidad de escalado. Las unidades de escalado de 1 a 3 admiten 500 conexiones, las unidades de escalado de 4 a 6 admiten 1000 conexiones, las unidades de escalado de 7 a 12 admiten 5000 conexiones y las unidades de escalado de 13 a 20 admiten hasta 10 000 conexiones. Como ejemplo, supongamos que el usuario elige 1 unidad de escalado. Cada unidad de escalado implica una puerta de enlace activo-activo implementada, y cada una de las instancias (2, en este caso) admitiría hasta 500 conexiones. Aunque se pueden tener 500 conexiones*2 por cada puerta de enlace, no significa que se pueda planear para 1 000 en lugar de para 500 para esta unidad de escalado, ya que las instancias podrían necesitar mantenimiento y, durante este tiempo, la conectividad de las 500 adicionales se vería interrumpida si se sobrepasa el número recomendado de conexiones. Además, asegúrese de planear el tiempo de inactividad en caso de que decida escalar o reducir verticalmente en la unidad de escalado o cambiar la configuración de punto a sitio en la puerta de enlace de VPN.

### <a name="what-is-the-difference-between-an-azure-virtual-network-gateway-vpn-gateway-and-an-azure-virtual-wan-vpn-gateway"></a>¿Cuál es la diferencia entre una puerta de enlace de Azure Virtual Network (VPN Gateway) y una instancia de VPN Gateway de Azure Virtual WAN?

Virtual WAN proporciona conectividad de sitio a sitio y a gran escala, y se ha creado para mejorar el rendimiento, la escalabilidad y la facilidad de uso. Cuando conecta un sitio a una instancia de VPN Gateway de Virtual WAN, esto es diferente a una puerta de enlace de red virtual normal que usa un tipo de puerta de enlace "VPN". Del mismo modo, cuando se conecta un circuito ExpressRoute a un centro de conectividad de Virtual WAN, se usa un recurso para la puerta de enlace de ExpressRoute que es distinto al de la puerta de enlace de red virtual normal que usa el tipo de puerta de enlace "ExpressRoute". Virtual WAN admite un rendimiento agregado de hasta 20 Gbps para VPN y ExpressRoute. Virtual WAN también tiene automatización para la conectividad con un ecosistema de asociados de dispositivos de la rama CPE. Los dispositivos de la rama CPE tienen una automatización integrada que se aprovisiona automáticamente y se conecta a Azure Virtual WAN. Estos dispositivos están disponibles en un creciente ecosistema de asociados de VPN y SD-WAN. Consulte la [lista de asociados preferidos](../articles/virtual-wan/virtual-wan-locations-partners.md).

### <a name="how-is-virtual-wan-different-from-an-azure-virtual-network-gateway"></a>¿En qué difiere Virtual WAN de una puerta de enlace de Azure Virtual Network?

Una VPN de puerta de enlace de red virtual se limita a 30 túneles. En las conexiones, debe usar Virtual WAN para VPN a gran escala. Puede conectar hasta 1000 conexiones de rama por región (centro de conectividad virtual) con agregados de 20 Gbps por centro. Una conexión es un túnel de activo a activo del dispositivo VPN local al concentrador virtual. Puede tener un único concentrador por región, lo que significa que puede conectar más de 1000 ramas en total con todos los concentradores.

### <a name="what-is-a-virtual-wan-gateway-scale-unit"></a>¿Qué es una unidad de escalado de puerta de enlace de Virtual WAN?
Una unidad de escalado es una unidad definida para obtener un rendimiento agregado de una puerta de enlace de un centro virtual. 1 unidad de escalado de VPN = 500 Mbps. 1 unidad de escalado de ExpressRoute = 2 Gbps. Ejemplo: 10 unidades de escalado de VPN significarían 500 Mbps * 10 = 5 Gbps

### <a name="which-device-providers-virtual-wan-partners-are-supported"></a>¿Qué proveedores de dispositivos (asociados de Virtual WAN) se admiten?

En este momento, muchos asociados admiten la experiencia de Virtual WAN totalmente automatizada. Para más información, consulte [Asociados de Virtual WAN](../articles/virtual-wan/virtual-wan-locations-partners.md). 

### <a name="what-are-the-virtual-wan-partner-automation-steps"></a>¿Cuáles son los pasos de automatización de asociados de Virtual WAN?

Para conocer estos pasos, consulte [Automatización de asociados de Virtual WAN](../articles/virtual-wan/virtual-wan-configure-automation-providers.md).

### <a name="am-i-required-to-use-a-preferred-partner-device"></a>¿Debo usar un dispositivo de asociado preferido?

No. Puede usar cualquier dispositivo compatible con VPN que cumpla los requisitos de Azure para la compatibilidad con IPsec de IKEv1 o IKEv2.

### <a name="how-do-virtual-wan-partners-automate-connectivity-with-azure-virtual-wan"></a>¿Cómo automatizan la conectividad con Azure Virtual WAN los asociados de Virtual WAN?

Las soluciones de conectividad definidas por software suelen administrar sus dispositivos de rama con un controlador o un centro de aprovisionamiento de dispositivos. El controlador puede usar las API de Azure para automatizar la conectividad a Azure Virtual WAN. La automatización incluye la carga de información de la rama, la descarga de la configuración de Azure, la configuración de túneles IPSec en los centros de conectividad virtuales de Azure y la configuración automática de la conectividad desde el dispositivo de rama a Azure Virtual WAN. Cuando tiene cientos de ramas, la conexión mediante asociados de CPE de Virtual WAN es fácil ya que la experiencia de incorporación elimina la necesidad de configurar y administrar la conectividad de IPsec a gran escala. Para más información, consulte [Automatización de asociados de Virtual WAN](../articles/virtual-wan/virtual-wan-configure-automation-providers.md).


### <a name="how-is-virtual-wan-supporting-sd-wan-devices"></a>¿Cómo es que Virtual WAN admite dispositivos SD-WAN?

Los asociados de Virtual WAN automatizan la conectividad IPsec a los puntos de conexión de VPN de Azure. Si el asociado de Virtual WAN es un proveedor de SD-WAN, se supone que el controlador de SD-WAN administra la automatización y la conectividad IPsec a los puntos de conexión de VPN de Azure. Si el dispositivo SD-WAN necesita su propio punto de conexión en lugar de la VPN de Azure para obtener funcionalidad de SD-WAN propietaria, puede implementar el punto de conexión de SD-WAN en una red virtual de Azure y coexistir con Azure Virtual WAN.

### <a name="does-virtual-wan-change-any-existing-connectivity-features"></a>¿Cambia Virtual WAN alguna de las características de conectividad existentes?

No hay ningún cambio en las características de conectividad de Azure.

### <a name="are-there-new-resource-manager-resources-available-for-virtual-wan"></a>¿Hay disponibles nuevos recursos de Resource Manager para Virtual WAN?
  
Sí, Virtual WAN introduce nuevos recursos de Resource Manager. Para más información, consulte la [Información general](../articles/virtual-wan/virtual-wan-about.md).

### <a name="how-many-vpn-devices-can-connect-to-a-single-hub"></a>¿Cuántos dispositivos VPN pueden conectarse a un único centro?

Se admiten hasta 1000 conexiones por concentrador virtual. Cada conexión consta de cuatro vínculos y cada conexión de vínculo admite dos túneles que están en una configuración activo-activo. Los túneles terminan en un elemento vpngateway del centro de conectividad virtual de Azure.

### <a name="can-the-on-premises-vpn-device-connect-to-multiple-hubs"></a>¿El dispositivo VPN local puede conectarse a varios concentradores?

Sí. Al principio, el flujo de tráfico sería del dispositivo local al perímetro de red de Microsoft más cercano y, luego, al centro de conectividad virtual.

### <a name="can-i-deploy-and-use-my-favorite-network-virtual-appliance-in-an-nva-vnet-with-azure-virtual-wan"></a>¿Puedo implementar y usar mi dispositivo virtual de red favorito (en una red virtual de NVA) con Azure Virtual WAN?

Sí, puede conectar su aplicación virtual de red (NVA) favorita a Azure Virtual WAN. En primer lugar, conecte la red virtual de la aplicación virtual de red virtual al concentrador con una conexión de red virtual de concentrador. A continuación, cree una ruta de centro de conectividad virtual con un próximo salto que apunte a la aplicación virtual. Puede aplicar varias rutas a la tabla de rutas del centro de conectividad virtual. Además, los radios conectados a la red virtual del dispositivo virtual deben estar conectados al concentrador virtual para garantizar que las rutas de la red virtual del radio se propagan a los sistemas locales.

### <a name="can-i-create-a-network-virtual-appliance-inside-the-virtual-hub"></a>¿Puedo crear una aplicación virtual de red dentro del centro de conectividad virtual?

Una aplicación virtual de red (NVA) no se puede implementar en un centro de conectividad virtual. Sin embargo, puede crearla en una red virtual de radio que esté conectada al centro de conectividad virtual y habilitar una ruta en el centro para dirigir el tráfico de la red virtual de destino a través de la dirección IP de la aplicación virtual de red (de la NIC).

### <a name="can-a-spoke-vnet-have-a-virtual-network-gateway"></a>¿Puede una red virtual de radio tener una puerta de enlace de red virtual?

No. La red virtual de radio no puede tener una puerta de enlace de red virtual si está conectada al centro de conectividad virtual.

### <a name="is-there-support-for-bgp"></a>¿Hay compatibilidad con BGP?

Sí, BGP se admite. Al crear una VPN de sitio, puede proporcionar en ella los parámetros de BGP. Como resultado, las conexiones creadas en Azure para ese sitio estarán habilitadas para BGP. Además, si tuviera una red virtual con una NVA, y si esta red estuviera asociada a un centro Virtual WAN, para garantizar que las rutas de una de estas redes se anuncian correctamente, los radios asociados a esta red deben deshabilitar BGP. Asimismo, conecte estas redes virtuales de radio a la red virtual del concentrador virtual para garantizar que las rutas de dichas redes se propagan a los sistemas locales.

### <a name="can-i-direct-traffic-using-udr-in-the-virtual-hub"></a>¿Se puede dirigir el tráfico mediante UDR en el concentrador virtual?

Sí, puede dirigir el tráfico a una red virtual mediante una tabla de rutas del centro de conectividad virtual. Esto le permite establecer rutas para redes virtuales de destino en Azure a través de una dirección IP específica (normalmente de la NIC de la aplicación virtual de red).

### <a name="is-there-any-licensing-or-pricing-information-for-virtual-wan"></a>¿Hay información de precios o licencias para Virtual WAN?

Sí. Consulte la página [Precios](https://azure.microsoft.com/pricing/details/virtual-wan/).

### <a name="how-do-i-calculate-price-of-a-hub"></a>¿Cómo se calcula el precio de un centro?

* Tendría que pagar por los servicios del centro. Por ejemplo, supongamos que tiene diez ramas o dispositivos locales que necesitan conectarse a Azure Virtual WAN. Eso implicaría la conexión a los puntos de conexión de la VPN del centro. Supongamos que esta VPN es de 1 unidad de escala = 500 Mbps, esto se cobraría a 0,361 USD/h. Cada conexión se cobra a 0,05 USD/h. Para 10 conexiones, el costo total de servicio/h sería de 0,361 USD + 0,05 USD/h. Se aplican cargos de datos por el tráfico que sale de Azure.

* Hay un cargo adicional por centro. Consulte la página [Precios](https://azure.microsoft.com/pricing/details/virtual-wan/).

* Si tuviera una puerta de enlace de ExpressRoute debido a la existencia de circuitos ExpressRoute que se conectan a un centro de conectividad virtual, pagaría por el precio de la unidad de escalado. Cada unidad de escalado en ER es de 2 Gbps y cada unidad de conexión se cobra a la misma tarifa que la unidad de conexión VPN.

* Si tiene redes virtuales de radio conectadas al centro, se seguirán aplicando cargos de emparejamiento en las redes virtuales de radio. 

### <a name="how-do-new-partners-that-are-not-listed-in-your-launch-partner-list-get-onboarded"></a>¿Cómo consiguen los nuevos asociados que no aparecen en la lista de asociados de partida ser incluidos en ella?

Todas las API de Virtual WAN son API abiertas. Puede consultar la documentación para valorar la viabilidad técnica. Si tiene alguna pregunta, envíe un correo electrónico a azurevirtualwan@microsoft.com. Un asociado ideal es aquel que tiene un dispositivo que se puede aprovisionar para la conectividad IPsec de IKEv1 o IKEv2.

### <a name="what-if-a-device-i-am-using-is-not-in-the-virtual-wan-partner-list-can-i-still-use-it-to-connect-to-azure-virtual-wan-vpn"></a>¿Qué ocurre si el dispositivo que uso no está en la lista de partners de Virtual WAN? ¿Lo puedo usar a pesar de todo para conectarme a una VPN de Azure Virtual WAN?

Sí, siempre que el dispositivo admita IPsec IKEv1 o IKEv2. Los asociados de Virtual WAN automatizan la conectividad desde el dispositivo hasta los puntos de conexión de VPN de Azure. Esto implica la automatización de pasos como la carga de la información de rama, IPsec y configuración y conectividad. Puesto que el dispositivo no procede del ecosistema de un asociado de Virtual WAN, deberá realizar el trabajo pesado de tomar manualmente la configuración de Azure y actualizar el dispositivo para configurar la conectividad IPsec.

### <a name="is-it-possible-to-construct-azure-virtual-wan-with-a-resource-manager-template"></a>¿Es posible construir Azure Virtual WAN con una plantilla de Resource Manager?

Se puede crear una configuración simple de una instancia de Virtual WAN con un centro y un sitio VPN mediante una [plantilla de inicio rápido](https://azure.microsoft.com/resources/templates/?resourceType=Microsoft.Network). Virtual WAN es principalmente un servicio REST o basado en el portal.

### <a name="is-global-vnet-peering-supported-with-azure-virtual-wan"></a>¿Se admite el emparejamiento de VNet global con Azure Virtual WAN? 

Puede conectar una red virtual en una región distinta a la de Virtual WAN.

### <a name="can-spoke-vnets-connected-to-a-virtual-hub-communicate-with-each-other-v2v-transit"></a>¿Pueden las redes virtuales radiales conectadas a un centro virtual comunicarse entre sí (tránsito V2V)?

Sí. Virtual WAN Estándar admite la conectividad transitiva entre redes virtuales mediante el centro de conectividad de Virtual WAN al que están conectadas las redes virtuales. En la terminología de Virtual WAN, llamamos a estas rutas de acceso "tránsito local de red virtual de Virtual WAN" para las redes virtuales conectadas a un centro de conectividad de Virtual WAN dentro de una única región, y "tránsito global de red virtual de Virtual WAN" en el caso de las redes virtuales conectadas mediante varios centros de conectividad de Virtual WAN en dos o más zonas. El tránsito de red virtual tiene una capacidad de procesamiento máxima de 3 Gbps durante la versión preliminar pública. Esta capacidad aumentará cuando el tránsito global pase a estar disponible de manera general.

NOTA:  Actualmente, la versión preliminar del tránsito V2V requiere la implementación de una puerta de enlace de VPN en un centro virtual para desencadenar los elementos de enrutamiento que se van a iniciar. Esta puerta de enlace de VPN no se utiliza para la ruta de acceso del tránsito V2V. Se trata de una limitación conocida que se solucionará cuando se publique la versión con disponibilidad general de V2V. Puede eliminar la puerta de enlace de VPN de los centros una vez que se haya iniciado totalmente, ya que no es necesaria para la funcionalidad de tránsito V2V. 

En algunos escenarios, las redes virtuales radiales también se pueden emparejar directamente entre sí mediante el [emparejamiento de red virtual](../articles/virtual-network/virtual-network-peering-overview.md), además del tránsito local o global de red virtual de Virtual WAN. En este caso, el emparejamiento de red virtual tiene prioridad sobre la conexión transitiva del centro de conectividad de Virtual WAN. 

### <a name="what-is-a-branch-connection-to-azure-virtual-wan"></a>¿Qué es una conexión de rama a Azure Virtual WAN?

Una conexión desde un dispositivo de rama a Azure Virtual WAN admite hasta cuatro vínculos. Un vínculo es el vínculo de conectividad física de la ubicación de la rama (por ejemplo: ATT, Verizon, etc.). Cada conexión de vínculo se compone de dos túneles IPsec activos/activos.

### <a name="is-branch-to-branch-connectivity-allowed-in-virtual-wan"></a>¿Se permite la conectividad de rama a rama en Virtual WAN?

Sí, este tipo de conectividad está disponible en Virtual WAN para conexiones VPN y VPN a ExpressRoute.

### <a name="does-branch-to-branch-traffic-traverse-through-the-azure-virtual-wan"></a>¿Atraviesa el tráfico de rama a rama Azure Virtual WAN?

Sí.

### <a name="does-virtual-wan-require-expressroute-from-each-site"></a>¿La instancia de Virtual WAN requiere ExpressRoute desde cada sitio?

No. Usa la conectividad de sitio a sitio IPsec estándar a través de vínculos de Internet desde el dispositivo a un centro de conectividad de Azure Virtual WAN. Los sitios pueden estar conectados a una red de proveedor mediante un circuito ExpressRoute. Los sitios conectados mediante ExpressRoute en el centro de conectividad virtual pueden tener un flujo de tráfico de rama a rama entre ExpressRoute y VPN.

### <a name="is-there-a-network-throughput-limit-when-using-azure-virtual-wan"></a>¿Hay un límite en el rendimiento de la red al utilizar Azure Virtual WAN?

El número de ramas se limita a 1000 conexiones por cada región o concentrador y a un total de 20 Gbps en el concentrador. Puede tener 1 centro por región.

### <a name="how-many-vpn-connections-does-a-virtual-wan-hub-support"></a>¿Cuántas conexiones VPN admite un concentrador de Virtual WAN?

Un centro de Azure Virtual WAN puede admitir hasta 1000 conexiones S2S, 10 000 conexiones de P2S y 4 conexiones de ExpressRoute simultáneamente.

### <a name="what-is-the-total-vpn-throughput-of-a-vpn-tunnel-and-a-connection"></a>¿Qué es el rendimiento total de VPN de un túnel y una conexión VPN?

El rendimiento total de VPN de un concentrador es de hasta 20 Gbps en función de la unidad de escalado elegida. El rendimiento se comparte entre todas las conexiones existentes. Cada túnel de una conexión puede admitir hasta 1 Gbps.

### <a name="i-dont-see-the-20-gbps-setting-for-the-virtual-hub-in-the-portal-how-do-i-configure-that"></a>No veo la configuración de 20 Gbps para el centro virtual en el portal. ¿Cómo puedo configurarlo?

Vaya a instancia de VPN Gateway de un centro del portal y haga clic en la unidad de escalado para cambiarla a la configuración adecuada.

### <a name="does-virtual-wan-allow-the-on-premises-device-to-utilize-multiple-isps-in-parallel-or-is-it-always-a-single-vpn-tunnel"></a>¿Azure Virtual WAN permite al dispositivo local usar varias ISP en paralelo o es siempre un solo túnel VPN?
Las soluciones en dispositivos locales pueden aplicar directivas de tráfico para dirigir el tráfico entre varios túneles en Azure.


### <a name="what-is-global-transit-architecture"></a>¿Qué es la arquitectura de tránsito global?

Para más información acerca de la arquitectura de tránsito global, consulte [Arquitectura de red de tránsito global y Virtual WAN](../articles/virtual-wan/virtual-wan-global-transit-network-architecture.md).

### <a name="how-is-traffic-routed-on-the-azure-backbone"></a>¿Cómo se enruta el tráfico en la red troncal de Azure?

El tráfico sigue el patrón: dispositivo de rama -> ISP -> perímetro de red de Microsoft -> controlador de dominio de Microsoft (red virtual de centro) -> perímetro de red de Microsoft -> ISP -> dispositivo de rama.

### <a name="in-this-model-what-do-you-need-at-each-site-just-an-internet-connection"></a>En este modelo, ¿qué es necesario en cada sitio? ¿Solo una conexión a Internet?

Sí. Una conexión a Internet y un dispositivo físico que admita IPsec, preferiblemente de nuestros [asociados de Virtual WAN](../articles/virtual-wan/virtual-wan-locations-partners.md) integrados. De forma opcional, puede administrar manualmente la configuración y la conectividad a Azure desde su dispositivo preferido.

### <a name="how-do-i-enable-default-route-00000-in-a-connection-vpn-expressroute-or-virtual-network"></a>¿Cómo habilito la ruta predeterminada (0.0.0.0/0) de una conexión? (VPN, ExpressRoute o Virtual Network):

Un centro de conectividad virtual puede propagar una ruta predeterminada aprendida en una red virtual, una VPN de sitio a sitio y una conexión ExpressRoute si la marca está "habilitada" en la conexión. Esta marca está visible cuando el usuario edita una conexión de red virtual, una conexión VPN o una conexión ExpressRoute. De forma predeterminada, esta marca está deshabilitada cuando un sitio o circuito de ExpressRoute se conecta a un centro. Está habilitada de forma predeterminada cuando se agrega una conexión de red virtual para conectar una red virtual a un centro de conectividad virtual. La ruta predeterminada no se origina en el centro de conectividad de Virtual WAN; la ruta se propaga si ya la ha aprendido el centro de Virtual WAN como resultado de la implementación de un firewall en el centro o en caso de que otro sitio conectado tenga habilitada la tunelización forzada.

### <a name="how-does-the-virtual-hub-in-a-virtual-wan-select-the-best-path-for-a-route-from-multiple-hubs"></a>Cómo selecciona el centro virtual de una instancia de Virtual WAN la mejor ruta de varios centros

Si un centro virtual aprende la misma ruta de varios centros remotos, el orden en el que decide es el siguiente:
1) Origen de la ruta: a) rutas de red (prefijos de red virtual aprendidos directamente por las puertas de enlace del centro virtual; b) RouteTable del centro (rutas configuradas estáticamente); c) BGP; d) rutas de InterHub.
2)  Métrica de ruta: Virtual WAN prefiere ExpressRoute a VPN. El emparejamiento de ExpressRoute tiene una ponderación mayor en comparación con el emparejamiento de VPN
3)  Longitud de la ruta de acceso del sistema autónomo

### <a name="is-there-support-for-ipv6-in-virtual-wan"></a>¿Hay compatibilidad con IPv6 en un Virtual WAN?

IPv6 no se admite en el concentrador de Virtual WAN y sus puertas de enlace. Si tiene una red virtual que tiene compatibilidad con IPv6 y quiere conectarla a Virtual WAN, este escenario tampoco se admite. 

### <a name="what-are-the-differences-between-the-virtual-wan-types-basic-and-standard"></a>¿Cuáles son las diferencias entre los tipos de instancias de Virtual WAN (básico y estándar)?

El tipo básico de Virtual WAN le permite crear un centro básico (SKU = básico). Un tipo estándar de Virtual WAN permite crear un centro estándar (SKU = estándar). Los centros básicos se limitan a la funcionalidad de VPN de sitio a sitio. Los centros estándar permiten tener ExpressRoute, VPN de usuario (P2S), centro de malla completo y tránsito de red virtual a red virtual mediante los centros. Pagará un cargo básico de 0,25 USD/h por los centros estándar y una tarifa de procesamiento de datos por el tránsito entre los centros durante la conectividad de red virtual a red virtual, así como por el procesamiento de datos del tráfico de centro a centro. Para más información, consulte [Basic and Standard virtual WANs](../articles/virtual-wan/virtual-wan-about.md#basicstandard) (Instancias básicas y estándar de Virtual WAN). Para ver los precios, consulte la página de [precios](https://azure.microsoft.com/pricing/details/virtual-wan/).

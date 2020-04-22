---
title: 'Arquitectura: Arquitectura de red de tránsito global'
titleSuffix: Azure Virtual WAN
description: Obtenga información sobre la arquitectura de red de tránsito global de Virtual WAN.
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: article
ms.date: 02/06/2020
ms.author: cherylmc
ms.openlocfilehash: 9515058bc78a2d56dc1734c046dac5d5b04f68d9
ms.sourcegitcommit: fb23286d4769442631079c7ed5da1ed14afdd5fc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/10/2020
ms.locfileid: "81113174"
---
# <a name="global-transit-network-architecture-and-virtual-wan"></a>Arquitectura de red de tránsito global y Virtual WAN

Las empresas modernas requieren de una conectividad ubicua entre las aplicaciones, los datos y los usuarios hiperdistribuidos en la nube y en el entorno local. Las empresas están adoptando la arquitectura de red de tránsito global para consolidar, conectar y controlar la huella de una TI empresarial moderna y global centrada en la nube.

La arquitectura de red de tránsito global se basa en un modelo de conectividad radial clásica donde el "centro" de la red que se hospeda en la nube permite la conectividad transitiva entre puntos de conexión que pueden estar distribuidos en distintos tipos de "radios".

En este modelo, una radio puede ser:
* Red virtual (redes virtuales)
* Sitio de sucursal física
* Usuario remoto
* Internet

![centro y radio](./media/virtual-wan-global-transit-network-architecture/figure1.png)

**Figura 1: red radial de tránsito global**

En la figura 1 se muestra la vista lógica de la red de tránsito global donde los usuarios, los sitios físicos y las redes virtuales distribuidos geográficamente están interconectados a través de un centro de redes hospedado en la nube. Esta arquitectura permite la conectividad de tránsito de un salto lógico entre los puntos de conexión de red.

## <a name="global-transit-network-with-virtual-wan"></a><a name="globalnetworktransit"></a>Red de tránsito global con Virtual WAN

Azure Virtual WAN es un servicio de redes en la nube administrado de Microsoft. Microsoft hospeda y administra todos los componentes de red que conforman este servicio. Para más información sobre Virtual WAN, consulte el artículo de [información general sobre Virtual WAN](virtual-wan-about.md).

Azure Virtual WAN permite una arquitectura de red de tránsito global al habilitar una conectividad ubicua universal entre conjuntos distribuidos globalmente de cargas de trabajo en la nube de redes virtuales, sitios de sucursales, aplicaciones SaaS y PaaS, y usuarios.

![Azure Virtual WAN](./media/virtual-wan-global-transit-network-architecture/figure2.png)

**Figura 2: Red de tránsito global y Virtual WAN**

En la arquitectura de Azure Virtual WAN, los centros WAN virtuales se aprovisionan en regiones de Azure, y puede elegir conectar sus sucursales, redes virtuales y usuarios remotos a ellas. Los sitios de sucursales físicas están conectados al concentrador mediante VPN de ExpressRoute de nivel Premium o VPN de sitio a sitio; las redes virtuales están conectadas al concentrador mediante conexiones de red virtual; y los usuarios remotos pueden conectarse directamente al concentrador mediante una VPN de usuario (VPN de punto a sitio). Virtual WAN también admite la conexión de red virtual entre regiones, donde una red virtual de una región puede estar conectada a un centro WAN virtual en una región diferente.

Puede establecer una WAN virtual mediante la creación de un centro único de Virtual WAN en la región con el mayor número de radios (sucursales, redes virtuales, usuarios) y, luego, la conexión con el centro de radios que están en otras regiones. Esta es una buena opción cuando la superficie de una empresa está principalmente en una región con algunos radios remotos.  
  
## <a name="hub-to-hub-connectivity-preview"></a><a name="hubtohub"></a>Conectividad de centro a centro (versión preliminar)

La superficie en la nube empresarial puede abarcar varias regiones en la nube y resulta óptimo (en términos de latencia) tener acceso a la nube desde una región más cercana a su sitio físico y a los usuarios. Uno de los principios clave de la arquitectura de red de tránsito global es habilitar la conectividad entre regiones entre todos los puntos de conexión de red locales y en la nube. Esto significa que el tráfico de una sucursal que está conectada a la nube en una región puede llegar a otra sucursal o a una red virtual en otra región mediante la conectividad de centro a centro que permite la [red global de Azure](https://azure.microsoft.com/global-infrastructure/global-network/).

![varias regiones](./media/virtual-wan-global-transit-network-architecture/figure3.png)

**Figura 3: Conectividad entre regiones de Virtual WAN**

Cuando se habilitan varios centros en una sola WAN virtual, estos se interconectan automáticamente mediante vínculos de centro a centro, lo que habilita la conectividad global entre las sucursales y las redes virtuales distribuidas en varias regiones. 

Además, todos los centros que son parte de la misma WAN virtual se pueden asociar con distintas directivas de seguridad y acceso regionales. Para obtener más información, consulte [control de seguridad y directivas](#security) más adelante en este artículo.

## <a name="any-to-any-connectivity"></a><a name="anytoany"></a>Conectividad universal

La arquitectura de red de tránsito global permite la conectividad universal a través de centros de WAN virtuales. Esta arquitectura elimina o disminuye la necesidad de la conectividad de malla parcial o de malla completa entre radios, que son más complejos de crear y mantener. Además, el enrutamiento de control entre las redes radiales y las redes de malla es más fácil de configurar y mantener.

La conectividad universal, en el contexto de una arquitectura global, permite que una empresa con usuarios, sucursales, centros de datos, redes virtuales y aplicaciones distribuidos globalmente se conecten entre sí a través de los centros de "tránsito". Azure Virtual WAN actúa como el sistema de tránsito global.

![universal](./media/virtual-wan-global-transit-network-architecture/figure4.png)

**Ilustración 4: Rutas de tráfico de Virtual WAN**

Azure Virtual WAN admite las siguientes rutas de conectividad de tránsito global. Las letras entre paréntesis hacen referencia a la figura 4.

* Sucursal a red virtual (a)
* Sucursal a sucursal (b)
  * Global Reach de ExpressRoute y Virtual WAN
* Usuario remoto a red virtual (c)
* Usuario remoto a sucursal (d)
* De red virtual a red virtual (e)
* De sucursal a centro y de centro a sucursal (f)
* De sucursal a centro y de centro a red virtual (g)
* De red virtual a centro y de centro a red virtual (h)

### <a name="branch-to-vnet-a-and-branch-to-vnet-cross-region-g"></a>De sucursal a red virtual (a) y de sucursal a red virtual entre regiones (g)

Sucursal a red virtual es la ruta principal admitida por Azure Virtual WAN. Esta ruta le permite conectar sucursales a las cargas de trabajo empresariales de IaaS de Azure que están implementadas en redes virtuales de Azure. Las sucursales se pueden conectar a la WAN virtual a través de ExpressRoute o de VPN de sitio a sitio. El tráfico transita a las redes virtuales que están conectadas a los centros de Virtual WAN a través de conexiones de red virtual. El [tránsito de puerta de enlace](../virtual-network/virtual-network-peering-overview.md#gateways-and-on-premises-connectivity) explícito no es necesario para Virtual WAN, porque Virtual WAN habilita automáticamente el tránsito de puerta de enlace al sitio de sucursal. Consulte el artículo [Asociados de WAN virtual](virtual-wan-configure-automation-providers.md) sobre cómo conectar un SD-WAN CPE a Virtual WAN.

### <a name="expressroute-global-reach-and-virtual-wan"></a>Global Reach de ExpressRoute y Virtual WAN

ExpressRoute es una forma privada y resistente de conectar las redes locales a la nube de Microsoft. Virtual WAN admite conexiones de circuito de ExpressRoute. La conexión de un sitio de sucursal a Virtual WAN con ExpressRoute requiere 1) un circuito Premium 2) un circuito para que esté en una ubicación habilitada para Global Reach.

Global Reach ExpressRoute es una característica de complemento para ExpressRoute. Con Global Reach, puede vincular los circuitos de ExpressRoute para crear una red privada entre las redes locales. Las sucursales que se conectan a Azure Virtual WAN a través de ExpressRoute requieren que Global Reach de ExpressRoute se comuniquen entre sí.

En este modelo, cada sucursal que se conecta al centro de Virtual WAN mediante ExpressRoute se puede conectar a las redes virtuales mediante la ruta de sucursal a red virtual. El tráfico de sucursal a sucursal no atravesará el centro, porque Global Reach de ExpressRoute permite una ruta más óptima a través de Azure WAN.

### <a name="branch-to-branch-b-and-branch-to-branch-cross-region-f"></a>Sucursal a sucursal (b) y sucursal a sucursal entre regiones (f)

Las sucursales se pueden conectar a un centro de Azure Virtual WAN mediante circuitos de ExpressRoute o conexiones de VPN de sitio a sitio. Puede conectar las sucursales al centro de Virtual WAN que se encuentra en la región más cercana a la sucursal.

Esta opción permite que las empresas usen la red troncal de Azure para conectar las sucursales. Sin embargo, aunque esta funcionalidad esté disponible, debe ponderar los beneficios de conectar las sucursales a través de Azure Virtual WAN en comparación con usar una WAN privada.  

### <a name="remote-user-to-vnet-c"></a>Usuario remoto a red virtual (c)

Puede habilitar el acceso remoto seguro y directo a Azure mediante una conexión de punto a sitio desde un cliente de usuario remoto a una WAN virtual. Los usuarios empresariales remotos ya no tienen que conectarse a la nube a través de una VPN corporativa.

### <a name="remote-user-to-branch-d"></a>Usuario remoto a sucursal (d)

La ruta de usuario remoto a sucursal permite que los usuarios remotos que usan una conexión de punto a sitio a Azure accedan a aplicaciones y cargas de trabajo locales mediante el tránsito a través de la nube. Esta ruta brinda a los usuarios remotos la flexibilidad para acceder a las cargas de trabajo que están implementadas en Azure y en el entorno local. Las empresas pueden habilitar el servicio central de acceso remoto seguro basado en la nube en Azure Virtual WAN.

### <a name="vnet-to-vnet-transit-e-and-vnet-to-vnet-cross-region-h"></a>Tránsito de red virtual a red virtual (e) y de red virtual a red virtual entre (h)

El tránsito de red virtual a red virtual permite que las redes virtuales se conecten entre sí para interconectar aplicaciones de varios niveles implementadas en varias redes virtuales. Opcionalmente, puede conectar redes virtuales entre sí mediante el emparejamiento de red virtual. Esta opción puede ser adecuada para algunos escenarios en los que no es necesario el tránsito a través del centro de WAN virtual.

## <a name="security-and-policy-control"></a><a name="security"></a>Seguridad y control de directivas

Los centros de Azure Virtual WAN interconectan todos los puntos de conexión de red a través de la red híbrida y pueden ver todo el tráfico de red de tránsito. Los centros de Virtual WAN se pueden convertir en centros virtuales protegidos si implementa Azure Firewall dentro de los centros de Virtual WAN para habilitar la seguridad, el acceso y el control de directivas basados en la nube. Azure Firewall Manager puede realizar la orquestación de Azure Firewall en los centros de WAN virtual.

[Azure Firewall Manager](https://go.microsoft.com/fwlink/?linkid=2107683) proporciona las funcionalidades para administrar y escalar la seguridad de las redes de tránsito global. Azure Firewall Manager ofrece la posibilidad de administrar de forma centralizada el enrutamiento, la administración global de directivas y los servicios de seguridad de Internet avanzados a través de terceros junto con Azure Firewall.

![centro virtual protegido con Azure Firewall](./media/virtual-wan-global-transit-network-architecture/figure5.png)

**Ilustración 5: Centro virtual protegido con Azure Firewall**

Azure Firewall a la WAN virtual admite las siguientes rutas de conectividad de tránsito seguro global. Las letras entre paréntesis hacen referencia a la figura 5.

* Tránsito seguro de red virtual a red virtual (e)
* Servicio de seguridad de red virtual a Internet o a terceros (i)
* Servicio de seguridad de sucursal a Internet o a terceros (i)

### <a name="vnet-to-vnet-secured-transit-e"></a>Tránsito seguro de red virtual a red virtual (e)

El tránsito seguro de red virtual a red virtual permite que las redes virtuales se conecten entre sí a través de Azure Firewall en el centro de WAN virtual.

### <a name="vnet-to-internet-or-third-party-security-service-i"></a>Servicio de seguridad de red virtual a Internet o a terceros (i)

El tránsito seguro de red virtual a Internet o a terceros permite que las redes virtuales se conecten a Internet o a servicios de seguridad de terceros admitidos a través de Azure Firewall en el centro de WAN virtual.

### <a name="branch-to-internet-or-third-party-security-service-j"></a>Servicio de seguridad de sucursal a Internet o a terceros (i)
El tránsito seguro de sucursal a Internet o a terceros permite que las sucursales se conecten a Internet o a servicios de seguridad de terceros admitidos a través de Azure Firewall en el centro de WAN virtual.

## <a name="next-steps"></a>Pasos siguientes

Cree una conexión mediante Virtual WAN e implemente Azure Firewall en los centros de Virtual WAN.

* [Creación de una conexión de sitio a sitio mediante Azure Virtual WAN](virtual-wan-site-to-site-portal.md)
* [Conexiones de ExpressRoute con Virtual WAN](virtual-wan-expressroute-portal.md)
* [Azure Firewall Manager para implementar Azure Firewall en WAN virtual](https://go.microsoft.com/fwlink/?linkid=2107683)

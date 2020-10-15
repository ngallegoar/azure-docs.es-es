---
title: Configuración y validación de conexiones de red virtual o de VPN
description: Guía paso a paso para configurar y validar diversas implementaciones de red virtual y de VPN de Azure
services: virtual-network
documentationcenter: na
author: v-miegge
manager: dcscontentpm
editor: ''
ms.assetid: 0433a4f4-b5a0-476d-b398-1506c57eafa2
ms.service: virtual-network
ms.devlang: na
ms.topic: troubleshooting
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/28/2019
ms.author: kaushika
ms.openlocfilehash: f25e42b1785f83e0b93c346e260055247a4ab29d
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "91400713"
---
# <a name="configure-and-validate-virtual-network-or-vpn-connections"></a>Configuración y validación de conexiones de red virtual o de VPN

Este tutorial proporciona una guía paso a paso para configurar y validar diversas implementaciones de red virtual y de VPN de Azure. Entre los escenarios se incluyen el enrutamiento de tránsito, las conexiones entre redes, el Protocolo de puerta de enlace de borde (BGP), las conexiones multisitio y las conexiones de punto a sitio.

Las puertas de enlace VPN de Azure proporcionan flexibilidad al organizar prácticamente cualquier tipo de topología de red virtual conectada de Azure. Por ejemplo, puede conectar redes virtuales:

- Entre regiones.
- Entre los tipos de red virtual (Azure Resource Manager frente a clásica).
- En Azure o en un entorno híbrido local.
- En distintas suscripciones. 

## <a name="network-to-network-vpn-connection"></a>Conexión VPN de red a red

La conexión de una red virtual a otra (red a red) a través de VPN es similar a la conexión de una red virtual a la ubicación de un sitio local. Ambos tipos de conectividad usan una puerta de enlace de VPN para proporcionar un túnel seguro a través de IPsec e IKE. Las redes virtuales pueden estar en la misma región o en distintas, así como pertenecer a una única suscripción o a varias.

![Conexión de red a red con IPsec](./media/virtual-network-configure-vnet-connections/4034386_en_2.png)
 
Si las redes virtuales están en la misma región, también existe la posibilidad de conectarlas mediante el emparejamiento de red virtual. El emparejamiento de red virtual no usa puerta de enlace de VPN. Aumenta el rendimiento y reduce la latencia. Para configurar una conexión de emparejamiento de red virtual, seleccione **Configure and validate VNet Peering** (Configurar y validar emparejamiento de VNet).

Si las redes virtuales se han creado mediante el modelo de implementación con Azure Resource Manager, seleccione **Configure and validate a Resource Manager VNet to a Resource Manager VNet connection** (Configurar y validar una red virtual de Resource Manager en una conexión de red virtual de Resource Manager) para configurar una conexión VPN.

Si una de las redes virtuales se ha creado mediante el modelo de implementación clásica de Azure y la otra mediante Resource Manager, seleccione **Configure and validate a classic VNet to a Resource Manager VNet connection** (Configurar y validar una red virtual clásica en una conexión de red virtual de Resource Manager) para configurar una conexión VPN.

### <a name="configure-virtual-network-peering-for-two-virtual-networks-in-the-same-region"></a>Configuración del emparejamiento de red virtual para dos redes virtuales de la misma región

Antes de empezar a implementar y configurar el emparejamiento de red virtual de Azure, asegúrese de que se cumplen los siguientes requisitos previos:

* Las redes virtuales emparejadas deben existir en la misma región de Azure.
* Las redes virtuales emparejadas deben tener espacios de direcciones IP que no se solapen.
* El emparejamiento de red virtual se realiza entre dos redes virtuales. No hay ninguna relación transitiva derivada entre emparejamientos. Por ejemplo, si VNetA está emparejada con VNetB y VNetB está emparejada con VNetC, VNetA *no* está emparejada con VNetC.

Cuando cumpla los requisitos, puede empezar a leer el artículo [Tutorial: Conexión de redes virtuales con emparejamiento de red virtual desde Azure Portal](https://docs.microsoft.com/azure/virtual-network/virtual-network-create-peering) para crear y configurar el emparejamiento.

Para comprobar la configuración del emparejamiento, use el siguiente método:

1. Inicie sesión en [Azure Portal](https://portal.azure.com/) con una cuenta que tenga los [roles y permisos](virtual-network-manage-peering.md#permissions) necesarios.
2. En el cuadro que contiene el texto **Buscar recursos**, en la parte superior del portal, escriba **redes virtuales**. Cuando aparezca la opción **Redes virtuales** en los resultados de búsqueda, selecciónela.
3. En la hoja **Redes virtuales** que aparece, haga clic en la red virtual para la que desea crear un emparejamiento.
4. En el panel que aparece para la red virtual, seleccione **Emparejamientos** en la sección **Configuración**.
5. Seleccione un emparejamiento y vea los resultados de la configuración.

![Elementos que se seleccionan para comprobar la configuración del emparejamiento de red virtual](./media/virtual-network-configure-vnet-connections/4034496_en_1.png)
 
En el caso de Azure PowerShell, ejecute el comando [Get-AzureRmVirtualNetworkPeering](https://docs.microsoft.com/powershell/module/azurerm.network/get-azurermvirtualnetworkpeering?view=azurermps-4.1.0) para obtener el emparejamiento de red virtual. Este es un ejemplo:

```
PS C:\Users\User1> Get-AzureRmVirtualNetworkPeering -VirtualNetworkName Vnet10-01 -ResourceGroupName dev-vnets
Name                             : LinkToVNET10-02
Id                               : /subscriptions/GUID/resourceGroups/dev-vnets/providers/Microsoft.Network/virtualNetworks/VNET10-01/virtualNetworkPeerings/LinkToVNET10-0
2
Etag                             : W/"GUID"
ResourceGroupName                : dev-vnets
VirtualNetworkName               : vnet10-01
ProvisioningState                : Succeeded
RemoteVirtualNetwork             : {
                                  "Id": "/subscriptions/GUID/resourceGroups/DEV-VNET
                                   s/providers/Microsoft.Network/virtualNetworks/VNET10-02"
                                   }
AllowVirtualNetworkAccess        : True
AllowForwardedTraffic            : False
AllowGatewayTransit              : False
UseRemoteGateways                : False
RemoteGateways                   : null
RemoteVirtualNetworkAddressSpace : null
```

### <a name="connect-a-resource-manager-virtual-network-to-another-resource-manager-virtual-network"></a>Conexión de dos redes virtuales de Resource Manager

Puede configurar una conexión de una red virtual de Resource Manager a otra directamente. O bien, se puede configurar la conexión mediante IPsec.

### <a name="configure-a-vpn-connection-between-resource-manager-virtual-networks"></a>Configuración de una conexión VPN entre redes virtuales de Resource Manager

Para configurar una conexión entre redes virtuales de Resource Manager sin IPsec, consulte [Configuración de una conexión de VPN Gateway de red a red mediante Azure Portal](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-vnet-vnet-resource-manager-portal).

Para configurar una conexión con IPsec entre dos redes virtuales de Resource Manager, siga los pasos del 1 a 5 del artículo [Creación de una conexión de sitio a sitio mediante Azure Portal](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal) en todas las redes virtuales.

> [!Note]
> Estos pasos solo funcionan para redes virtuales que están en la misma suscripción. Si las redes virtuales se encuentran en suscripciones distintas, es preciso usar PowerShell para realizar la conexión. Vea el artículo [PowerShell](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-vnet-vnet-rm-ps).

### <a name="validate-the-vpn-connection-between-resource-manager-virtual-networks"></a>Validación de la conexión VPN entre redes virtuales de Resource Manager

![Conexión de una red virtual clásica a una red virtual de Azure Resource Manager](./media/virtual-network-configure-vnet-connections/4034493_en_2.png)

Para comprobar que la conexión VPN está configurada correctamente, siga estas instrucciones.

> [!Note] 
> Los números que aparecen después de los componentes de la red virtual en estos pasos se corresponden con los números del diagrama anterior.

1. Asegúrese de que no se solapen espacios de direcciones en las redes virtuales conectadas.
2. Compruebe que el intervalo de direcciones de la red virtual de Azure Resource Manager (1) está definido correctamente en la instancia del **objeto de conexión** (4).
3. Compruebe que el intervalo de direcciones de la red virtual de Azure Resource Manager (6) está definido correctamente en la instancia del **objeto de conexión** (3).
4. Compruebe que las claves precompartidas coinciden con los objetos de conexión.
5. Compruebe que la IP virtual de la puerta de enlace de la red virtual de Azure Resource Manager (2) está definida correctamente en la instancia del **objeto de conexión** (4).
6. Compruebe que la IP virtual de la puerta de enlace de la red virtual de Azure Resource Manager (5) está definida correctamente en la instancia del **objeto de conexión** (3).

### <a name="connect-a-classic-virtual-network-to-a-resource-manager-virtual-network"></a>Conexión de una red virtual clásica a una red virtual de Resource Manager

Puede crear una conexión entre redes virtuales que estén en diferentes suscripciones y regiones. También puede conectar redes virtuales que ya tengan conexiones a redes locales, siempre que el tipo de puerta de enlace se haya configurado como basada en ruta.

Para configurar una conexión entre una red virtual clásica y una red virtual de Resource Manager, consulte [Conexión de redes virtuales a partir de diferentes modelos de implementación desde Azure Portal](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-connect-different-deployment-models-portal).

![Conexión de una red virtual clásica a una red virtual de Azure Resource Manager](./media/virtual-network-configure-vnet-connections/4034389_en_2.png)

Para comprobar la configuración cuando conecte una red virtual clásica a una red virtual de Azure Resource Manager, siga estas instrucciones.

> [!Note] 
> Los números que aparecen después de los componentes de la red virtual en estos pasos se corresponden con los números del diagrama anterior. 

1. Asegúrese de que no se solapen espacios de direcciones en las redes virtuales conectadas.
2. Compruebe que el intervalo de direcciones de la red virtual de Azure Resource Manager (6) se ha incluido correctamente en la definición de la red local clásica (3).
3. Compruebe que el intervalo de direcciones de la red virtual clásica (1) se ha definido correctamente en la instancia del **objeto de conexión** (4) de Azure Resource Manager.
4. Compruebe que la IP virtual de la puerta de enlace de la red virtual clásica (2) se ha definido correctamente en la instancia del **objeto de conexión** (4) de Azure Resource Manager.
5. Compruebe que la puerta de enlace de la red virtual de Azure Resource Manager (5) se ha definido correctamente en la instancia de la **definición de la red local** (3).
6. Compruebe que las claves precompartidas coinciden en las dos redes virtuales conectadas:
   - Red virtual clásica: **definición de red local** (3)
   - Red virtual de Azure Resource Manager: **objeto de conexión** (4)

## <a name="create-a-point-to-site-vpn-connection"></a>Creación de una conexión VPN de punto a sitio

Las configuraciones de punto a sitio (*P2S*) permiten crear una conexión segura entre un equipo cliente individual y una red virtual. Las conexiones de punto a sitio resultan útiles cuando se desea establecer conexión con una red virtual desde una ubicación remota; por ejemplo, desde casa o desde una conferencia. También son útiles cuando hay pocos clientes que necesiten conectarse a una red virtual. 

La conexión VPN de punto a sitio se inicia desde el equipo cliente mediante el cliente VPN de Windows nativo. En la conexión de clientes, se usan certificados para la autenticación.

![Conexión de punto a sitio](./media/virtual-network-configure-vnet-connections/4034387_en_3.png)

Las conexiones de punto a sitio no requieren ningún dispositivo VPN. Crean la conexión VPN a través del Protocolo de túnel de sockets seguros (SSTP). Para establecer una conexión de punto a sitio con una red virtual se pueden usar varias herramientas y modelos de implementación:

* [Configuración de una conexión punto a sitio a una red virtual mediante Azure Portal](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal)
* [Configuración de una conexión punto a sitio a una red virtual mediante Azure Portal (clásico)](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-point-to-site-classic-azure-portal)
* [Configuración de una conexión punto a sitio a una red virtual mediante PowerShell](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-point-to-site-rm-ps)

### <a name="validate-your-point-to-site-connection"></a>Validación de una conexión de punto a sitio

En el artículo [Solución de problemas: Problemas de conexión de punto a sitio de Azure](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-troubleshoot-vpn-point-to-site-connection-problems) se describen problemas comunes con las conexiones de punto a sitio.

## <a name="create-a-multisite-vpn-connection"></a>Creación de una conexión VPN multisitio

Puede agregar una conexión de sitio a sitio (*S2S* en el diagrama siguiente) con una red virtual que ya tenga una conexión de sitio a sitio, una conexión de punto a sitio o una conexión de red a red. Con frecuencia, a este tipo de conexión se le denomina configuración *multisitio*. 

![Conexión multisitio](./media/virtual-network-configure-vnet-connections/4034497_en_2.png)

Azure actualmente funciona con dos modelos de implementación: el de Resource Manager y el clásico. Los dos modelos no son totalmente compatibles entre sí. Para configurar una conexión multisitio con distintos modelos, consulte los siguientes artículos:

* [Agregar una conexión de sitio a sitio a una red virtual con una conexión de VPN Gateway existente](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-multi-site-to-site-resource-manager-portal)
* [Agregar una conexión de sitio a sitio a una red virtual con una conexión de VPN Gateway existente (clásico)](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-multi-site)

> [!Note]
> Los pasos que se indican en estos artículos no se aplican a las configuraciones de conexión coexistentes de ExpressRoute y sitio a sitio. Para más información, consulte [Configuración de conexiones ExpressRoute y de sitio a sitio coexistentes](https://docs.microsoft.com/azure/expressroute/expressroute-howto-coexist-resource-manager).

## <a name="configure-transit-routing"></a>Configuración del enrutamiento de tránsito

El enrutamiento de tránsito es un escenario de enrutamiento concreto en el que se conectan varias redes en una topología de cadena de margarita. Este enrutamiento permite a los recursos de las redes virtuales de cualquiera de los extremos de la cadena comunicarse entre sí a través de redes virtuales. Sin el enrutamiento de tránsito, las redes o los dispositivos emparejados a través de un centro de conectividad no se pueden conectar entre sí.

### <a name="configure-transit-routing-in-a-point-to-site-connection"></a>Configuración del enrutamiento de tránsito en una conexión de punto a sitio

Imagine un escenario en el que desea configurar una conexión VPN de sitio a sitio entre VNetA y VNetB. También desea configurar una VPN de punto a sitio para que el cliente se conecte a la puerta de enlace de VNetA. Luego, desea habilitar el enrutamiento de tránsito para que los clientes de punto a sitio se conecten a VNetB, que pasa a través de VNetA. 

Este escenario se admite cuando BGP está habilitado en la VPN de sitio a sitio entre VNetA y VNetB. Para más información, consulte [Información sobre el enrutamiento de VPN de punto a sitio](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-point-to-site-routing).

### <a name="configure-transit-routing-in-an-expressroute-connection"></a>Configuración del enrutamiento de tránsito en una conexión de ExpressRoute

Azure ExpressRoute le permite ampliar sus redes locales a la nube de Microsoft a través de una conexión privada y dedicada que facilita un proveedor de conectividad. Con ExpressRoute, se pueden establecer conexiones con servicios en la nube de Microsoft, como Microsoft Azure, Microsoft 365 y Dynamics 365. Para más información, consulte [Información general sobre ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction).

![Conexión de emparejamiento privado de ExpressRoute a redes virtuales de Azure](./media/virtual-network-configure-vnet-connections/4034395_en_1.png)

> [!Note]
> Si VNetA y VNetB se encuentran en la misma región geográfica, se recomienda [vincular ambas redes virtuales al circuito de ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-howto-linkvnet-arm), en lugar de configurar el enrutamiento de tránsito. Si las redes virtuales se encuentran en regiones geopolíticas distintas, también puede vincularlas directamente al circuito si tiene [ExpressRoute Premium](https://docs.microsoft.com/azure/expressroute/expressroute-faqs#expressroute-premium). 

Si coexisten conexiones ExpressRoute y de sitio a sitio, no se admite el enrutamiento de tránsito. Para más información, consulte [Configuración de conexiones ExpressRoute y de sitio a sitio coexistentes con PowerShell](https://docs.microsoft.com/azure/expressroute/expressroute-howto-coexist-resource-manager).

Si ha habilitado ExpressRoute para conectar las redes locales a una red virtual de Azure, puede habilitar el emparejamiento entre las redes virtuales en las que desea tener enrutamiento de tránsito. Para que las redes locales se conecten a la red virtual remota, debe configurar el [emparejamiento de red virtual](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview#gateways-and-on-premises-connectivity). 

> [!Note]
> El emparejamiento de red virtual solo está disponible para redes virtuales de la misma región.

Para comprobar si ha configurado el enrutamiento de tránsito para el emparejamiento de red virtual, siga estas instrucciones:

1. Inicie sesión en [Azure Portal](https://portal.azure.com/) con una cuenta que tenga los [roles y permisos](virtual-network-manage-peering.md#permissions) necesarios.
2. [Cree un emparejamiento entre VNetA y VNetB](https://docs.microsoft.com/azure/virtual-network/virtual-network-create-peering) como se muestra en el diagrama anterior. 
3. En el panel que aparece para la red virtual, seleccione **Emparejamientos** en la sección **Configuración**.
4. Seleccione el emparejamiento que desea ver. Luego, seleccione **Configuración** para validar que ha habilitado **Permitir tránsito de puerta de enlace** en la red VNetA conectada al circuito ExpressRoute y **Usar puerta de enlace remota** en la red VNetB remota no conectada al circuito ExpressRoute.

### <a name="configure-transit-routing-in-a-virtual-network-peering-connection"></a>Configuración del enrutamiento de tránsito en una conexión de emparejamiento de red virtual

Cuando las redes virtuales están emparejadas, también puede configurar la puerta de enlace de la red virtual emparejada como punto de tránsito a una red local. Para configurar la ruta de tránsito en el emparejamiento de red virtual, consulte [Conexiones de red a red](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-vnet-vnet-rm-ps?toc=/azure/virtual-network/toc.json).

> [!Note]
> No se admite el tránsito de puerta de enlace en la relación de emparejamiento entre las redes virtuales creadas mediante modelos de implementación diferentes. Para que funcione el tránsito de puerta de enlace, las dos redes virtuales de la relación de emparejamiento se deben haber creado mediante Resource Manager.

Para comprobar si ha configurado una ruta de tránsito para el emparejamiento de red virtual, siga estas instrucciones:

1. Inicie sesión en [Azure Portal](https://portal.azure.com/) con una cuenta que tenga los [roles y permisos](virtual-network-manage-peering.md#permissions) necesarios.
2. En el cuadro que contiene el texto **Buscar recursos**, en la parte superior del portal, escriba **redes virtuales**. Cuando aparezca la opción **Redes virtuales** en los resultados de búsqueda, selecciónela.
3. En la hoja **Redes virtuales** que aparece, seleccione la red virtual cuyo valor de emparejamiento desea comprobar.
4. En el panel de la red virtual que ha seleccionado, seleccione **Emparejamientos** en la sección **Configuración**.
5. Seleccione el emparejamiento que desea ver. Compruebe que ha habilitado **Permitir tránsito de puerta de enlace** y **Usar puerta de enlace remota** en **Configuración**.

![Elementos seleccionados para comprobar que ha configurado una ruta de tránsito para el emparejamiento de red virtual](./media/virtual-network-configure-vnet-connections/4035414_en_1.png)

### <a name="configure-transit-routing-in-a-network-to-network-connection"></a>Configuración del enrutamiento de tránsito en una conexión de red a red

Para configurar el enrutamiento de tránsito entre redes virtuales, es preciso habilitar BGP en todas las conexiones de red a red intermedias mediante el modelo de implementación de Resource Manager y PowerShell. Para conocer las instrucciones, consulte [Configuración de BGP para Azure VPN Gateway con PowerShell](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-bgp-resource-manager-ps).

El tráfico en tránsito a través de Azure VPN Gateway es posible mediante el modelo de implementación clásica, pero se basa en espacios de direcciones definidos estáticamente en el archivo de configuración de red. BGP aún no es compatible con las instancias de Azure Virtual Network y VPN Gateway mediante el modelo de implementación clásica. Sin BGP, se suelen cometer errores al definir manualmente los espacios de direcciones de tránsito, por lo que no se recomienda.

> [!Note]
> Las conexiones de red a red clásicas se configuran mediante el Portal de Azure clásico o mediante un archivo de configuración de red en el portal clásico. No se pueden crear ni modificar redes virtuales clásicas mediante el modelo de implementación de Azure Resource Manager ni mediante Azure Portal. Para más información sobre el enrutamiento de tránsito para redes virtuales clásicas, consulte el [blog de desarrolladores de Microsoft](https://blogs.msdn.microsoft.com/igorpag/2015/10/01/hubspoke-daisy-chain-and-full-mesh-vnet-topologies-in-azure-arm-using-vpn-v1/).

### <a name="configure-transit-routing-in-a-site-to-site-connection"></a>Configuración del enrutamiento de tránsito en una conexión de sitio a sitio

Para configurar el enrutamiento de tránsito entre una red local y una red virtual con una conexión de sitio a sitio, debe habilitar BGP en todas las conexiones de sitio a sitio intermedias mediante el modelo de implementación de Resource Manager y PowerShell. Para obtener instrucciones al respecto, consulte [Configuración de BGP para Azure VPN Gateway con PowerShell](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-bgp-resource-manager-ps).

El tráfico en tránsito a través de Azure VPN Gateway es posible mediante el modelo de implementación clásica, pero se basa en espacios de direcciones definidos estáticamente en el archivo de configuración de red. BGP aún no es compatible con las instancias de Azure Virtual Network y VPN Gateway mediante el modelo de implementación clásica. Sin BGP, se suelen cometer errores al definir manualmente los espacios de direcciones de tránsito, por lo que no se recomienda.

> [!Note]
> Las conexiones de sitio a sitio clásicas se configuran mediante el Portal de Azure clásico o mediante un archivo de configuración de red en el portal clásico. No se pueden crear ni modificar redes virtuales clásicas mediante el modelo de implementación de Azure Resource Manager ni mediante Azure Portal. Para más información sobre el enrutamiento de tránsito para redes virtuales clásicas, consulte el [blog de desarrolladores de Microsoft](https://blogs.msdn.microsoft.com/igorpag/2015/10/01/hubspoke-daisy-chain-and-full-mesh-vnet-topologies-in-azure-arm-using-vpn-v1/).

## <a name="configure-bgp-for-a-vpn-gateway"></a>Configuración de BGP para una instancia de VPN Gateway

BGP es el protocolo de enrutamiento estándar que se usa en Internet para intercambiar información de enrutamiento y disponibilidad entre dos o más redes. Cuando BGP se usa en el contexto de redes virtuales de Azure, habilita las puertas de enlace VPN y los dispositivos VPN locales, conocidos como vecinos o pares BGP. Intercambian "rutas" que informan a ambas puertas de enlace sobre la disponibilidad y accesibilidad de los prefijos para pasar por las puertas de enlace o los enrutadores pertinentes. 

BGP también puede habilitar el enrutamiento de tránsito entre varias redes mediante la propagación de rutas que una puerta de enlace de BGP aprende de un par BGP a los restantes pares BGP. Para más información, consulte [Introducción a BGP con Azure VPN Gateway](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-bgp-overview).

### <a name="configure-bgp-for-a-vpn-connection"></a>Configuración de BGP para una conexión VPN

Para configurar una conexión VPN que usa BGP, consulte [Configuración de BGP para Azure VPN Gateway con PowerShell](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-bgp-resource-manager-ps).

Habilite BGP en la puerta de enlace de red virtual mediante la creación de un número de sistema autónomo (AS) para él. Las puertas de enlace básicas no admiten BGP. Para comprobar la SKU de la puerta de enlace, vaya a la sección **Información general** de la hoja **VPN Gateway** en Azure Portal. Si la SKU es **básica**, tendrá que cambiarla (consulte el artículo en el que se indica cómo se [cambia el tamaño de la puerta de enlace](https://docs.microsoft.com/powershell/module/azurerm.network/resize-azurermvirtualnetworkgateway?view=azurermps-4.1.0&viewFallbackFrom=azurermps-4.0.0)) a **VpnGw1**. 

La comprobación de la SKU provocará un tiempo de inactividad que oscila entre 20 y 30 minutos. En cuanto la puerta de enlace tenga la SKU correcta, se puede agregar el número de sistema autónomo (AS) mediante el cmdlet [Set-AzureRmVirtualNetworkGateway](https://docs.microsoft.com/powershell/module/azurerm.network/set-azurermvirtualnetworkgateway?view=azurermps-3.8.0) de PowerShell. Después de configurar el número de AS, se proporcionará automáticamente una dirección IP del par BGP para la puerta de enlace.

Debe especificar manualmente un número de sistema autónomo (AS) y una dirección de par BGP para `LocalNetworkGateway`. Puede establecer los valores `ASN` y `-BgpPeeringAddress` mediante los cmdlets [New-AzureRmLocalNetworkGateway](https://docs.microsoft.com/powershell/module/azurerm.network/new-azurermlocalnetworkgateway?view=azurermps-4.1.0) o [Set-AzureRmLocalNetworkGateway](https://docs.microsoft.com/powershell/module/azurerm.network/set-azurermlocalnetworkgateway?view=azurermps-4.1.0) de PowerShell. Algunos números de sistema autónomo (AS) están reservados para Azure y no se pueden usar, como se describe en [Acerca de BGP con Azure VPN Gateway](../vpn-gateway/vpn-gateway-bgp-overview.md#faq).

El objeto de conexión debe tener BGP habilitado. Puede establecer el valor `-EnableBGP` en `$True` mediante [New-AzureRmVirtualNetworkGatewayConnection](https://docs.microsoft.com/powershell/module/azurerm.network/new-azurermvirtualnetworkgatewayconnection?view=azurermps-4.1.0) o [Set-AzureRmVirtualNetworkGatewayConnection](https://docs.microsoft.com/powershell/module/azurerm.network/set-azurermvirtualnetworkgatewayconnection?view=azurermps-4.1.0).

### <a name="validate-the-bgp-configuration"></a>Validación de la configuración de BGP

Para comprobar si BGP está configurado correctamente, puede ejecutar los cmdlets `get-AzureRmVirtualNetworkGateway` y `get-AzureRmLocalNetworkGateway`. Luego, observará la salida relacionada con BGP en la parte de `BgpSettingsText`. Por ejemplo:

```
{

"Asn": AsnNumber,

"BgpPeeringAddress": "IP address",

"PeerWeight": 0

}
```

## <a name="create-a-highly-available-activeactive-vpn-connection"></a>Creación de una conexión VPN activo-activo de alta disponibilidad

Estas son las principales diferencias entre las puertas de enlace activo-activo y activo-en espera:

* Debe crear dos configuraciones de la IP de la puerta de enlace con dos direcciones IP públicas.
* Debe establecer la marca **EnableActiveActiveFeature**.
* La SKU de la puerta de enlace debe ser **VpnGw1**, **VpnGw2** o **VpnGw3**.

Para lograr una alta disponibilidad en la conectividad de red a red y entre entornos locales, debe implementar varias puertas de enlace VPN y establecer varias conexiones en paralelo entre sus redes y Azure. Para obtener información general de las opciones y topología de conectividad, consulte [Conectividad de alta disponibilidad entre locales y de red virtual a red virtual](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-highlyavailable).

Para crear conexiones activo-activo entre locales y de red a red, siga las instrucciones de [Configuración de conexiones VPN S2S activo-activo con Azure VPN Gateway](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-activeactive-rm-powershell) para configurar Azure VPN Gateway en modo activo/activo.

> [!Note]  
> * Al agregar direcciones a la puerta de enlace de red local para el modo activo-activo habilitado para BGP, *agregue solo las direcciones /32 de los pares BGP*. Si agrega más direcciones, se considerarán rutas estáticas y tendrán prioridad sobre las rutas BGP.
> * Debe usar números de sistema autónomo (AS) de BGP diferentes para las redes locales que se conectan a Azure. (Si son iguales, tiene que cambiar el número de sistema autónomo (AS) de la red virtual si el dispositivo VPN local ya utiliza el ASN para emparejarse con otros vecinos de BGP).

## <a name="change-an-azure-vpn-gateway-type-after-deployment"></a>Cambio de un tipo de instancia de Azure VPN Gateway tras la implementación

No se puede cambiar un tipo de puerta de enlace de red virtual de Azure de basada en directivas a basada en el enrutamiento, o viceversa, directamente. Primero es preciso eliminar la puerta de enlace. Cuando se haga, la dirección IP y la clave precompartida no se conservarán. Luego, se puede crear una nueva puerta de enlace del tipo deseado. 

Para eliminar y crear una puerta de enlace, siga estos pasos:

1. Elimine también las conexiones asociadas a la puerta de enlace original.
2. Elimine la puerta de enlace mediante Azure Portal, PowerShell o PowerShell clásico: 
   * [Eliminación de una puerta de enlace de red virtual mediante Azure Portal](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-delete-vnet-gateway-portal)
   * [Eliminación de una puerta de enlace de red virtual mediante PowerShell](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-delete-vnet-gateway-powershell)
   * [Eliminación de una puerta de enlace de red virtual mediante PowerShell (clásico)](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-delete-vnet-gateway-classic-powershell)
3. Siga los pasos que se describen en [Creación de la puerta de enlace VPN](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md#VNetGateway) para crear la nueva puerta de enlace del tipo deseado y completar la configuración de VPN.

> [!Note]
> Este proceso tardará unos 60 minutos.

## <a name="next-steps"></a>Pasos siguientes

* [Solución de problemas de conectividad entre máquinas virtuales de Azure](https://docs.microsoft.com/azure/virtual-network/virtual-network-troubleshoot-connectivity-problem-between-vms)


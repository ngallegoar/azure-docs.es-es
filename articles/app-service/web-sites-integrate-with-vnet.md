---
title: Integración de aplicaciones con Azure Virtual Network
description: Integre aplicaciones en Azure App Service con Azure Virtual Network.
author: ccompy
ms.assetid: 90bc6ec6-133d-4d87-a867-fcf77da75f5a
ms.topic: article
ms.date: 02/27/2020
ms.author: ccompy
ms.custom: seodec18
ms.openlocfilehash: 89aa78e0d26598eacf436ca88cc6c5549f91d2fc
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "78673228"
---
# <a name="integrate-your-app-with-an-azure-virtual-network"></a>Integración de su aplicación con una instancia de Azure Virtual Network
En este documento, se describe la característica Integración con red virtual de Azure App Service y se explica cómo configurarla con aplicaciones en [Azure App Service](https://go.microsoft.com/fwlink/?LinkId=529714). [Azure Virtual Network][VNETOverview] (redes virtuales) le permite colocar cualquier recurso de Azure en una red que se pueda enrutar distinta de Internet.  

Azure App Service adopta dos variantes.

[!INCLUDE [app-service-web-vnet-types](../../includes/app-service-web-vnet-types.md)]

## <a name="enable-vnet-integration"></a>Habilitación de Integración con red virtual 

1. Vaya a la interfaz de usuario de Redes en el portal de App Service. En Integración con red virtual, seleccione *"Haga clic aquí para configurar"* . 

1. Seleccione **Agregar VNET**.  

   ![Selección de Integración con red virtual][1]

1. La lista desplegable contendrá todas las redes virtuales de Resource Manager de la suscripción que están en la misma región y, a continuación, una lista de todas las redes virtuales de Resource Manager que están en las demás regiones. Seleccione la red virtual con la que desea realizar la integración.

   ![Seleccione la red virtual.][2]

   * Si la red virtual está en la misma región, cree una nueva subred o seleccione una subred vacía existente. 

   * Para seleccionar una red virtual de otra región, necesita una puerta de enlace de Virtual Network aprovisionada con la conectividad de punto a sitio habilitada.

   * Si desea realizar la integración con una red virtual clásica, en lugar de hacer clic en la lista desplegable de redes virtuales, seleccione **Haga clic aquí para conectarse a una red virtual clásica**. Seleccione la red virtual clásica que desee. Para seleccionar una red virtual de destino, debe tener una puerta de enlace de Virtual Network aprovisionada con la conectividad de punto a sitio habilitada.

    ![Seleccione una red virtual clásica.][3]
    
Durante la integración, la aplicación se reinicia.  Una vez que se complete la integración, podrá ver los detalles en la red virtual con la que ha realizado la integración. 

Cuando la aplicación esté integrada con la red virtual, usará el mismo servidor DNS que está configurado en la red virtual, a menos que pertenezca a Azure DNS Private Zones. En la actualidad, Integración con red virtual no puede utilizarse con Azure DNS Private Zones.

## <a name="regional-vnet-integration"></a>Integración con red virtual regional

[!INCLUDE [app-service-web-vnet-types](../../includes/app-service-web-vnet-regional.md)]

### <a name="how-regional-vnet-integration-works"></a>Funcionamiento de la versión regional de Integración con red virtual

Las aplicaciones en App Service se hospedan en roles de trabajo. Los planes de precios básico y más elevados son planes de hospedaje dedicados donde no hay ninguna otra carga de trabajo de cliente ejecutándose en los mismos trabajos. La versión regional de Integración con red virtual funciona montando interfaces virtuales con las direcciones de la subred delegada. Como la dirección de origen está en la red virtual, tiene acceso a la mayoría de las cosas que están en esa red virtual o a las que se puede acceder a través de ella, igual que cualquier máquina virtual que estuviera en la red virtual. La implementación de red no es lo mismo que ejecutar una máquina virtual en la red virtual, y ese es el motivo por el que algunas características de red todavía no están disponibles al usar esta característica.

![Funcionamiento de la versión regional de Integración con red virtual][5]

Cuando se habilita la versión regional de Integración con red virtual, la aplicación sigue realizando llamadas salientes a Internet utilizando los mismos canales, como hace habitualmente. Las direcciones salientes que se muestran en el portal de propiedades de la aplicación siguen siendo las direcciones usadas por la aplicación. Lo que es distinto para la aplicación es que las llamadas a servicios protegidos de punto de conexión de servicio o direcciones de RFC 1918 tienen lugar en la red virtual. Si WEBSITE_VNET_ROUTE_ALL está establecido en 1, todo el tráfico de salida puede enviarse a la red virtual. 

La característica solo admite una interfaz virtual por trabajo.  Una interfaz virtual por trabajo significa una característica Integración con red virtual regional por plan de App Service. Todas las aplicaciones en el mismo plan de App Service pueden usar la misma característica Integración con red virtual, pero si necesita que una aplicación se conecte a otra red virtual, deberá crear otro plan de App Service. La interfaz virtual utilizada no es un recurso al que los clientes tengan acceso directo.

Dada la forma en que esta tecnología funciona, el tráfico que se usa con Integración con red virtual no se muestra en los registros de flujos de NSG o Network Watcher.  

## <a name="gateway-required-vnet-integration"></a>Integración con red virtual con requisito de puerta de enlace

La versión de Integración con red virtual que necesita una puerta de enlace permite conectarse a una red virtual de otra región o a una red virtual clásica. Versión de Integración con red virtual que necesita una puerta de enlace: 

* Permite que una aplicación se conecte solo a una red virtual a la vez.
* Permite la integración con hasta cinco redes virtuales dentro de un plan de App Service. 
* Permite que varias aplicaciones de un mismo plan de App Service usen la misma red virtual sin que ello afecte al número total que se puede utilizar en un plan de App Service.  Si tiene seis aplicaciones que usan la misma red virtual en el mismo plan de App Service, solo se contabilizará una sola red virtual en uso. 
* Admite un SLA del 99,9 % debido al SLA de la puerta de enlace.
* Permite que las aplicaciones utilicen el DNS con el que se ha configurado la red virtual.
* Para poder conectarse a la aplicación, necesita una puerta de enlace basada en rutas de Virtual Network que esté configurada con una VPN de punto a sitio SSTP. 

La versión de Integración con red virtual que necesita una puerta de enlace no puede utilizarse en las siguientes circunstancias:

* Con aplicaciones de Linux
* Con una red virtual conectada a ExpressRoute 
* Para acceder a recursos protegidos por puntos de conexión de servicio
* Con una puerta de enlace coexistente que admita tanto VPN de ExpressRoute como VPN de punto a sitio o de sitio a sitio

### <a name="set-up-a-gateway-in-your-vnet"></a>Configuración de una puerta de enlace en la red virtual ###

Para crear una puerta de enlace:

1. [Cree una subred de puerta de enlace][creategatewaysubnet] en su red virtual.  

1. [Cree la instancia de VPN Gateway][creategateway]. Seleccione un tipo de VPN basada en rutas.

1. [Establezca las direcciones de punto a sitio][setp2saddresses]. Si la puerta de enlace no está en la SKU básica, IKEV2 debe estar deshabilitado en la configuración de punto a sitio, mientras que SSTP debe estar seleccionado. El espacio de direcciones de punto a sitio debe especificarse en bloques de direcciones de RFC 1918 (10.0.0.0/8, 172.16.0.0/12 y 192.168.0.0/16).

Si solo está creando la puerta de enlace para su uso con Integración con red virtual de App Service, no es necesario cargar un certificado. La creación de la puerta de enlace puede tardar 30 minutos. No podrá integrar su aplicación con la red virtual hasta que la puerta de enlace esté aprovisionada. 

### <a name="how-gateway-required-vnet-integration-works"></a>Funcionamiento de la versión de Integración con red virtual que necesita una puerta de enlace

La versión de Integración con red virtual que necesita una puerta de enlace se basa en la tecnología VPN de punto a sitio. Las VPN de punto a sitio limitan el acceso de red únicamente a la máquina virtual que hospeda la aplicación. Las aplicaciones están restringidas para enviar solo tráfico a internet a través de conexiones híbridas o de la integración con red virtual. Cuando la aplicación se ha configurado con el portal para que utilice la versión de Integración con red virtual que necesita una puerta de enlace, se entabla una compleja negociación en su nombre para crear y asignar certificados en la puerta de enlace y en la aplicación. El resultado final es que los trabajos que se usan para hospedar las aplicaciones pueden conectarse directamente a la puerta de enlace de la red virtual seleccionada. 

![Funcionamiento de la versión de Integración con red virtual que necesita una puerta de enlace][6]

### <a name="accessing-on-premises-resources"></a>Obtener acceso a recursos locales

Las aplicaciones pueden acceder a los recursos locales mediante la integración con redes virtuales que tengan conexiones de sitio a sitio. Si usa la Integración con red virtual con requisito de puerta de enlace, debe actualizar las rutas de puerta de enlace de red virtual locales con los bloques de direcciones de punto a sitio. En la configuración inicial de la VPN de sitio a sitio, los scripts que se usan para configurarla deben configurar las rutas correctamente. Si agrega las direcciones de punto a sitio después de crear la VPN de sitio a sitio, deberá actualizar las rutas manualmente. El procedimiento varía según la puerta de enlace y no se describe aquí. No se puede tener BGP configurado con una conexión VPN de sitio a sitio.

No hay ninguna configuración adicional necesaria para que la característica Integración con red virtual regional acceda a la red virtual y al entorno local. Basta con conectar la red virtual al entorno local mediante ExpressRoute o una VPN de sitio a sitio. 

> [!NOTE]
> La característica Integración con red virtual con requisito de puerta de enlace no integra una aplicación con una red virtual que tiene una puerta de enlace de ExpressRoute. Aunque la puerta de enlace de ExpressRoute esté configurada en [modo de coexistencia][VPNERCoex], la integración con red virtual no funcionará. Si necesita acceder a los recursos mediante una conexión de ExpressRoute, puede utilizar la característica Integración con red virtual regional o un entorno [App Service Environment][ASE] que se ejecute en la red virtual. 
> 
> 

### <a name="peering"></a>Emparejamiento

Si se usa el emparejamiento con Integración con red virtual regional, no es necesario configurar nada más. 

Si usa la Integración con red virtual con requisito de puerta de enlace con el emparejamiento, deberá configurar algunos elementos más. Para configurar el emparejamiento para que funcione con su aplicación:

1. Agregue una conexión de emparejamiento en la red virtual a la que se conecta su aplicación. Al agregar la conexión de emparejamiento, habilite **Permitir acceso a red virtual** y active **Permitir tráfico reenviado** y **Permitir tránsito de puerta de enlace**.
1. Agregue una conexión de emparejamiento en la red virtual que se está emparejando con la red virtual a la que está conectado. Al agregar la conexión de emparejamiento en la red virtual de destino, habilite **Permitir acceso a red virtual** y active **Permitir tráfico reenviado** y **Allow remote gateways** (Permitir puertas de enlace remotas).
1. Vaya a Plan de App Service > Redes > UI de Integración con red virtual en el portal.  Seleccione la red virtual a la que está conectada su aplicación. En la sección de enrutamiento, agregue el intervalo de direcciones de la red virtual que está emparejada con la red virtual a la que está conectada su aplicación.  

## <a name="managing-vnet-integration"></a>Administración de Integración con red virtual 

La conexión y la desconexión con una red virtual tiene lugar en el nivel de la aplicación. Las operaciones que pueden afectar a la integración con red virtual en varias aplicaciones se encuentran en un nivel del plan de App Service. En la aplicación > Redes > Portal de Integración con red virtual, puede obtener detalles sobre la red virtual. Puede encontrar información similar en el nivel de ASP, en el portal ASP > Redes > Integración con red virtual.

La única operación que puede realizar en la vista de aplicación de Integración con red virtual es desconectar la aplicación de la red virtual si está conectada a ella. Para desconectar la aplicación de una red virtual, seleccione **Desconectar**. La aplicación se reiniciará cuando se desconecte de una red virtual. La desconexión no cambia la red virtual. La subred o la puerta de enlace no se quitan. Si después desea eliminar la red virtual, primero debe desconectar la aplicación de la red virtual y eliminar los recursos que hay en ella, como las puertas de enlace. 

En la interfaz de usuario de Integración con red virtual de ASP, aparecerán todas las integraciones de red virtual que se utilizan en las aplicaciones de la instancia de ASP. Para ver más detalles sobre cada red virtual, haga clic en la red virtual que le interese. Hay dos acciones que puede realizar aquí en relación con la versión de Integración con red virtual que necesita una puerta de enlace.

* **Sincronizar red**. La operación de sincronización de red solo es posible con la característica Integración con red virtual con requisito de puerta de enlace. Realizar una operación de sincronización de red garantiza que los certificados y la información de red estén sincronizados. Si agrega o cambia el DNS de la red virtual, debe ejecutar la operación **Sincronizar red**. Esta operación reiniciará todas las aplicaciones con esta red virtual.
* **Agregar rutas** La adición de rutas dirigirá el tráfico saliente hacia la red virtual. 

**Enrutamiento de la versión de Integración con red virtual que necesita una puerta de enlace**: las rutas definidas en la red virtual se utilizan para dirigir el tráfico a la red virtual desde la aplicación. Si necesita enviar más tráfico saliente a la red virtual, puede agregar aquí los bloques de direcciones. Esta capacidad solo funciona con la Integración con red virtual con requisito de puerta de enlace. Las tablas de rutas no afectan del mismo modo al tráfico de la aplicación cuando se utiliza la versión de Integración con red virtual que necesita una puerta de enlace del mismo modo que cuando se utiliza la versión regional.

**Certificados en la versión de Integración con red virtual que necesita una puerta de enlace**: cuando se habilita la versión de Integración con red virtual que necesita una puerta de enlace, es preciso intercambiar los certificados para garantizar la seguridad de la conexión. Junto con los certificados, se incluyen la configuración de DNS, las rutas y otros elementos similares que describen la red.
Si los certificados o la información de red cambian, es necesario hacer clic en "Sincronizar red". Al hacer clic en "Sincronizar red", se producirá una breve interrupción en la conectividad entre la aplicación y la red virtual. Aunque no se reinicia la aplicación, la pérdida de conectividad podría hacer que su sitio no funcione correctamente. 

## <a name="pricing-details"></a>Detalles de precios
La característica Integración con red virtual regional no tiene ningún cargo extra por uso más allá de los cargos del plan de tarifa de ASP.

Hay tres cargos relacionados con el uso de la característica Integración con red virtual con requisito de puerta de enlace:

* Cargos del plan de tarifa de ASP: las aplicaciones deben estar en un plan de App Service Estándar, Premium o PremiumV2. Puede ver más detalles sobre esos costos aquí: [Precios de App Service][ASPricing] 
* Costos de transferencias de datos: se aplica un cargo de salida de datos, aunque la red virtual esté en el mismo centro de datos. Estos cargos se describen en la página de [detalles de precios de Transferencia de datos][DataPricing]. 
* Costos de puerta de enlace de red virtual: existe un costo para la puerta de enlace de red virtual necesaria para la VPN de punto a sitio. Encontrará los detalles en la página [Precios de VPN Gateway][VNETPricing].

## <a name="troubleshooting"></a>Solución de problemas

[!INCLUDE [app-service-web-vnet-troubleshooting](../../includes/app-service-web-vnet-troubleshooting.md)]

## <a name="automation"></a>Automation

La CLI es compatible con la versión regional de Integración con red virtual. Para acceder a los comandos siguientes, [instale la CLI de Azure][installCLI]. 

        az webapp vnet-integration --help

        Group
            az webapp vnet-integration : Methods that list, add, and remove virtual network integrations
            from a webapp.
                This command group is in preview. It may be changed/removed in a future release.
        Commands:
            add    : Add a regional virtual network integration to a webapp.
            list   : List the virtual network integrations on a webapp.
            remove : Remove a regional virtual network integration from webapp.

        az appservice vnet-integration --help

        Group
            az appservice vnet-integration : A method that lists the virtual network integrations used in an
            appservice plan.
                This command group is in preview. It may be changed/removed in a future release.
        Commands:
            list : List the virtual network integrations used in an appservice plan.

En la versión de Integración con red virtual que necesita una puerta de enlace, puede integrar App Service con una instancia de Azure Virtual Network utilizando PowerShell. Para obtener un script que esté listo para ejecutarse, consulte [Connect an app in Azure App Service to an Azure Virtual Network](https://gallery.technet.microsoft.com/scriptcenter/Connect-an-app-in-Azure-ab7527e3) (Conectar una aplicación de Azure App Service a una red de Azure Virtual Network).


<!--Image references-->
[1]: ./media/web-sites-integrate-with-vnet/vnetint-app.png
[2]: ./media/web-sites-integrate-with-vnet/vnetint-addvnet.png
[3]: ./media/web-sites-integrate-with-vnet/vnetint-classic.png
[5]: ./media/web-sites-integrate-with-vnet/vnetint-regionalworks.png
[6]: ./media/web-sites-integrate-with-vnet/vnetint-gwworks.png


<!--Links-->
[VNETOverview]: https://azure.microsoft.com/documentation/articles/virtual-networks-overview/ 
[AzurePortal]: https://portal.azure.com/
[ASPricing]: https://azure.microsoft.com/pricing/details/app-service/
[VNETPricing]: https://azure.microsoft.com/pricing/details/vpn-gateway/
[DataPricing]: https://azure.microsoft.com/pricing/details/data-transfers/
[V2VNETP2S]: https://azure.microsoft.com/documentation/articles/vpn-gateway-howto-point-to-site-rm-ps/
[ILBASE]: environment/create-ilb-ase.md
[V2VNETPortal]: ../vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal.md
[VPNERCoex]: ../expressroute/expressroute-howto-coexist-resource-manager.md
[ASE]: environment/intro.md
[creategatewaysubnet]: ../vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal.md#creategw
[creategateway]: https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal#creategw
[setp2saddresses]: https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal#addresspool
[VNETRouteTables]: https://docs.microsoft.com/azure/virtual-network/manage-route-table/
[installCLI]: https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest/

---
author: ccompy
ms.service: app-service-web
ms.topic: include
ms.date: 10/21/2020
ms.author: ccompy
ms.openlocfilehash: 1a9f468b8e2f9fff20b9b26b8890d485e426b691
ms.sourcegitcommit: 4bee52a3601b226cfc4e6eac71c1cb3b4b0eafe2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/11/2020
ms.locfileid: "94523902"
---
Cuando se utiliza la versión regional de Integración con red virtual, la aplicación puede acceder a:

* Recursos en una VNet de la misma región que la aplicación.
* Recursos de las VNet emparejadas con la VNet con la que se integra la aplicación.
* Servicios protegidos mediante puntos de conexión de servicio.
* Recursos de diferentes conexiones de Azure ExpressRoute.
* Recursos de la VNet con la que está integrado.
* Recursos en conexiones emparejadas, lo que incluye conexiones de Azure ExpressRoute.
* Puntos de conexión privados 

Si Integración con red virtual se utiliza con redes virtuales de la misma región, se pueden usar las siguientes características de redes de Azure:

* **Grupos de seguridad de red (NSG)** : el tráfico saliente se puede bloquear con un grupo de seguridad de red que se encuentre en la subred de integración. Las reglas de entrada no se aplican, ya que Integración con red virtual no se puede usar para proporcionar acceso de entrada a la aplicación.
* **Tablas de enrutamiento (UDR)** : puede colocar una tabla de enrutamiento en la subred de integración para enviar el tráfico de salida donde quiera.

De forma predeterminada, su aplicación solo enruta el tráfico de RFC 1918 a su VNet. Si desea enrutar todo el tráfico de salida a la red virtual, aplique el valor WEBSITE_VNET_ROUTE_ALL a la aplicación. Para configurar este valor de la aplicación:

1. Vaya a la interfaz de usuario de **Configuración** en el portal de la aplicación. Seleccione **New application setting** (Nueva configuración de la aplicación).
1. Escriba **WEBSITE_VNET_ROUTE_ALL** en el cuadro **Nombre** y **1** en el cuadro **Valor**.

   ![Configuración del valor de la aplicación][4]

1. Seleccione **Aceptar**.
1. Seleccione **Guardar**.

> [!NOTE]
> Si enruta todo el tráfico de salida a la VNet, este estará sujeto a los grupos de seguridad de red y las rutas definidas por los usuarios que se apliquen a la subred de integración. Si enruta todo el tráfico de salida a la VNet, las direcciones de salida seguirán siendo las que aparecen en las propiedades de la aplicación, a menos que proporcione otras rutas para enviar allí el tráfico.

Existen algunas limitaciones cuando se la característica Integración con red virtual se utiliza con redes virtuales que están en la misma región:

* No se puede acceder a los recursos en las conexiones de emparejamiento global.
* La característica está disponible en todas las unidades de escalado de App Service de nivel Premium V2 y Premium V3. También está disponible en Estándar, pero solo desde las unidades de escalado de App Service más recientes. Si está en una unidad de escalado anterior, solo puede usar la característica desde un plan de App Service Premium V2. Si quiere estar seguro de que puede usar la característica en un plan de App Service Estándar, cree la aplicación en un plan de App Service Premium V3. Estos planes solo se admiten en las unidades de escalado más recientes. Si quiere, después puede reducir verticalmente.  
* La subred de integración solo puede usarla un plan de App Service.
* Las aplicaciones del plan Aislado que estén en una instancia de App Service Environment no pueden usar la característica.
* La característica requiere una subred sin usar que sea /28 o mayor en una red virtual de Azure Resource Manager.
* La aplicación y la VNet deben estar en la misma región.
* No puede eliminar una VNet con una aplicación integrada. Quite la integración antes de eliminar la VNet.
* Solo puede integrar con redes virtuales de la misma suscripción que la aplicación.
* Solo se puede tener una característica Integración con red virtual regional por plan de App Service. Varias aplicaciones en el mismo plan de App Service pueden usar la misma red virtual.
* No se puede cambiar la suscripción de una aplicación o un plan mientras haya una aplicación que use Integración con red virtual regional.
* La aplicación no puede resolver direcciones en Azure DNS Private Zones sin que se realicen cambios en la configuración

La integración con red virtual depende del uso de una subred dedicada.  Al aprovisionar una subred, la subred de Azure pierde 5 direcciones IP desde el inicio. Se usa una dirección de la subred de integración para cada instancia del plan. Si escala la aplicación a cuatro instancias, se usan cuatro direcciones. El débito de 5 direcciones del tamaño de la subred significa que el número máximo de direcciones disponibles por bloque CIDR es:

- /28 tiene 11 direcciones
- /27 tiene 27 direcciones
- /26 tiene 59 direcciones

Si escala o reduce verticalmente el tamaño, necesita doblar la necesidad de la dirección durante un breve período de tiempo. Los límites de tamaño significan que las instancias compatibles reales disponibles por tamaño de subred son:

- Si la subred es /28, la escala horizontal máxima es de 5 instancias
- Si la subred es /27, la escala horizontal máxima es de 13 instancias
- Si la subred es /26, la escala horizontal máxima es de 29 instancias

Los límites indicados en la escala horizontal máxima dan por hecho que va a necesitar escalar o reducir verticalmente el tamaño o la SKU en algún momento. 

Puesto que el tamaño de la subred no se puede cambiar después de la asignación, use una subred lo suficientemente grande como para dar cabida a cualquier escala que pueda alcanzar la aplicación. Para evitar problemas con la capacidad de la subred, el tamaño recomendado es /26 con 64 direcciones.  

Si quiere que las aplicaciones de otro plan lleguen a una VNet a la que ya están conectadas aplicaciones de otro plan, debe seleccionar una subred distinta a la usada por la característica Integración con VNet ya existente.

La característica es totalmente compatible con aplicaciones para Windows y Linux, incluidos los [contenedores personalizados](../articles/app-service/quickstart-custom-container.md). Todos los comportamientos actúan del mismo modo entre aplicaciones para Windows y Linux.

### <a name="service-endpoints"></a>Puntos de conexión del servicio

La versión regional de Integración con red virtual permite utilizar puntos de conexión de servicio. Si desea usar puntos de conexión de servicio con la aplicación, utilice la versión regional de Integración con red virtual para conectarse a una VNet seleccionada y configurar después los puntos de conexión de servicio con el servicio de destino en la subred que utilice para realizar la integración. Si luego desea tener acceso a un servicio a través de puntos de conexión de servicio:

1. Configure la versión regional de Integración con red virtual con su aplicación web.
1. Vaya al servicio de destino y configure los puntos de conexión de servicio en la subred usada para la integración.

### <a name="network-security-groups"></a>Grupos de seguridad de red

Puede usar grupos de seguridad de red para bloquear el tráfico de entrada y salida de los recursos de una VNet. Una aplicación que emplee la versión regional de Integración con red virtual puede usar un [grupo de seguridad de red][VNETnsg] para bloquear el tráfico de salida a los recursos de la VNet o Internet. Para bloquear el tráfico dirigido a direcciones públicas, debe tener el valor WEBSITE_VNET_ROUTE_ALL de la aplicación establecido en 1. Las reglas de entrada de un grupo de seguridad de red no se aplican a la aplicación, ya que Integración con red virtual solo afecta al tráfico saliente de la aplicación.

Para controlar el trafico de entrada a la aplicación, use la característica Restricciones de acceso. Un grupo de seguridad de red que se aplique a la subred de integración está en vigor, con independencia de las rutas aplicadas a la subred de integración. Si WEBSITE_VNET_ROUTE_ALL está establecido en 1 y no tiene ninguna ruta que afecte al tráfico de direcciones públicas en la subred de integración, todo el tráfico de salida sigue estando sujeto a los grupos de seguridad de red asignados a la subred de integración. Si WEBSITE_VNET_ROUTE_ALL no se ha definido, los grupos de seguridad de red solo se aplican al tráfico de RFC 1918.

### <a name="routes"></a>Rutas

Las tablas de enrutamiento se pueden usar para enrutar el tráfico de salida de la aplicación al lugar que se desee. De forma predeterminada, las tablas de enrutamiento solo afectan al tráfico de destino de RFC 1918. Si WEBSITE_VNET_ROUTE_ALL se establece en 1, se verán afectadas todas las llamadas salientes. Las rutas que se establecen en la subred de integración no afectan a las respuestas a las solicitudes de entrada de la aplicación. Los destinos más habituales suelen ser puertas de enlace o dispositivos de firewall.

Si desea enrutar todo el tráfico de salida del entorno local, puede utilizar una tabla de rutas para enviar el tráfico de salida a la puerta de enlace de ExpressRoute. Si no enruta el tráfico a una puerta de enlace, asegúrese de establecer las rutas en la red externa para poder enviar de vuelta las respuestas.

Las rutas del Protocolo de puerta de enlace de borde (BGP) también afectan al tráfico de la aplicación. Si tiene rutas de BGP cuyo origen es algo similar a una puerta de enlace de ExpressRoute, el tráfico de salida de la aplicación se verá afectado. De forma predeterminada, las rutas de BGP solo afectan al tráfico de destino de RFC 1918. Si WEBSITE_VNET_ROUTE_ALL está establecido en 1, todo el tráfico de salida puede verse afectado por las rutas de BGP.

### <a name="azure-dns-private-zones"></a>Azure DNS Private Zones 

Una vez que la aplicación se integra con la red virtual, usa el mismo servidor DNS que el configurado para la red virtual. Para invalidar este comportamiento en la aplicación, configure el valor de la aplicación WEBSITE_DNS_SERVER con la dirección del servidor DNS que quiera. Si tiene un servidor DNS personalizado configurado con la red virtual, pero quiere que la aplicación use zonas privadas de Azure DNS, debe establecer WEBSITE_DNS_SERVER con el valor 168.63.129.16. 

### <a name="private-endpoints"></a>Puntos de conexión privados

Si quiere realizar llamadas a [Puntos de conexión privados][privateendpoints], debe asegurarse de que las búsquedas de DNS se resuelvan en el punto de conexión privado. Para asegurarse de que las búsquedas de DNS de la aplicación apunten a los puntos de conexión privados, puede:

* realizar la integración con Azure DNS Private Zones. Si la red virtual no tiene un servidor DNS personalizado, esto es automático
* administrar el punto de conexión privado en el servidor DNS que usa la aplicación. Para ello, debe conocer la dirección del punto de conexión privado y, a continuación, apuntar el punto de conexión al que está intentando acceder a esa dirección con un registro A
* configurar su propio servidor DNS para reenviarlo a Azure DNS Private Zones

<!--Image references-->
[4]: ../includes/media/web-sites-integrate-with-vnet/vnetint-appsetting.png

<!--Links-->
[VNETnsg]: https://docs.microsoft.com/azure/virtual-network/security-overview/
[privateendpoints]: https://docs.microsoft.com/azure/app-service/networking/private-endpoint

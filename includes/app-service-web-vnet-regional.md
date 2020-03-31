---
author: ccompy
ms.service: app-service-web
ms.topic: include
ms.date: 02/27/2020
ms.author: ccompy
ms.openlocfilehash: c731e2a7d4da1a66aabb50b335d526fbb6a0a302
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "78671516"
---
Cuando se utiliza la versión regional de Integración con red virtual, la aplicación puede acceder a:

* recursos de la red virtual que están en la misma región con la que se está realizando la integración, 
* recursos de redes virtuales emparejadas con la red virtual que se encuentran en la misma región,
* servicios protegidos mediante puntos de conexión de servicio,
* recursos de diferentes conexiones de ExpressRoute,
* recursos de la red virtual a la que está conectado,
* recursos de diferentes conexiones emparejadas, incluidas conexiones de ExpressRoute,
* puntos de conexión privados. 

Si Integración con red virtual se utiliza con redes virtuales de la misma región, se pueden usar las siguientes características de Redes de Azure:

* Grupos de seguridad de red (NSG): puede bloquear el tráfico de salida con un grupo de seguridad de red situado en la subred de integración. Las reglas de entrada no se aplican, ya que no se puede usar Integración con red virtual para proporcionar acceso de entrada a la aplicación.
* Tablas de rutas (UDR): puede colocar una tabla de rutas en la subred de integración para enviar el tráfico de salida donde quiera. 

De forma predeterminada, la aplicación solo enrutará el tráfico de RFC 1918 a la red virtual. Si desea enrutar todo el tráfico de salida a la red virtual, aplique el valor WEBSITE_VNET_ROUTE_ALL a la aplicación. Para configurar este valor de la aplicación:

1. Vaya a la interfaz de usuario de configuración del portal de la aplicación. Haga clic en **Nueva configuración de la aplicación**.
1. Escriba **WEBSITE_VNET_ROUTE_ALL** en el campo Nombre y **1** en el campo Valor.

   ![Configuración del valor de la aplicación][4]

1. Seleccione **Aceptar**.
1. Seleccione **Guardar**.

Si enruta todo el tráfico de salida a la red virtual, dicho tráfico estará sujeto a los grupos de seguridad de red y las rutas definidas por los usuarios que se apliquen a la subred de integración. Si enruta todo el tráfico de salida a la red virtual, las direcciones salientes seguirán siendo las que aparecen en las propiedades de la aplicación, a menos que proporcione otras rutas para enviar allí el tráfico. 

Existen algunas limitaciones cuando se la característica Integración con red virtual se utiliza con redes virtuales que están en la misma región:

* No se puede tener acceso a los recursos en conexiones de emparejamiento global.
* La característica solo está disponible en las unidades de escalado de App Service más recientes que admiten planes PremiumV2 de App Service.
* La subred de integración solo se puede usar en un plan de App Service.
* Las aplicaciones de plan Aislado que estén en un App Service Environment no pueden usar la característica.
* La característica necesita una subred sin usar que sea /27 con un mínimo de 32 direcciones en la red virtual de Resource Manager.
* La aplicación y la red virtual deben estar en la misma región
* No puede eliminar una red virtual con una aplicación integrada. Tiene que quitar la integración antes de eliminar la red virtual. 
* Solo puede integrar con redes virtuales de la misma suscripción que la aplicación.
* Solo se puede tener una característica Integración con red virtual regional por plan de App Service. Varias aplicaciones en el mismo plan de App Service pueden usar la misma red virtual. 
* No se puede cambiar la suscripción de una aplicación o un plan mientras haya una aplicación que use Integración con red virtual regional.

Se usa una dirección para cada instancia del plan. Si escala la aplicación a cinco instancias, se utilizarán cinco direcciones. Puesto que no se puede cambiar el tamaño de la subred después de la asignación, debe usar una subred lo suficientemente grande como para dar cabida a cualquier escala que pueda alcanzar la aplicación. Un tamaño de /26 con 64 direcciones es el recomendado. Una subred /26 con 64 direcciones da cabida a un plan Premium con 30 instancias. Al escalar o reducir verticalmente un plan, necesita el doble de direcciones durante un breve período de tiempo. 

Si quiere que las aplicaciones de otro plan lleguen a una red virtual a la que ya están conectadas aplicaciones de otro plan, debe seleccionar una subred distinta a la usada por la característica Integración con red virtual ya existente.  

Esta característica se encuentra en versión preliminar para Linux. La versión para Linux de esta característica solo permite realizar llamadas a direcciones RFC 1918 (10.0.0.0/8, 172.16.0.0/12 y 192.168.0.0/16).

### <a name="web--function-app-for-containers"></a>Web/Function App for Containers

Si hospeda la aplicación en Linux con las imágenes integradas, Integración con red virtual regional funciona sin necesidad de más cambios. Si usa Web/Function App for Containers, debe modificar la imagen de Docker para usar Integración con red virtual. En la imagen de Docker, use la variable de entorno PORT como puerto de escucha del servidor web principal en lugar de un número de puerto codificado de forma rígida. La variable de entorno PORT la establece automáticamente la plataforma al iniciar el contenedor. Si usa SSH, el demonio de SSH debe estar configurado para escuchar en el número de puerto que especificó la variable de entorno SSH_PORT al usar la integración de red virtual regional.  En Linux, no se puede utilizar la versión de Integración con red virtual que necesita una puerta de enlace. 

### <a name="service-endpoints"></a>Puntos de conexión de servicio

La versión regional de Integración con red virtual permite utilizar puntos de conexión de servicio.  Si desea usar puntos de conexión de servicio con la aplicación, utilice la versión regional de Integración con red virtual para conectarse a una red virtual seleccionada y configurar después los puntos de conexión de servicio en la subred que utilice para realizar la integración. 

### <a name="network-security-groups"></a>Grupos de seguridad de red

Los grupos de seguridad de red permiten bloquear el tráfico de entrada y salida de los recursos de una red virtual. Una aplicación que emplee Integración con red virtual regional puede usar [Grupo de seguridad de red][VNETnsg] para bloquear el tráfico de salida a los recursos de la red virtual o Internet. Para bloquear el tráfico dirigido a direcciones públicas, debe tener el valor WEBSITE_VNET_ROUTE_ALL de la aplicación establecido en 1. Las reglas de entrada de un grupo de seguridad de red no tendrían efecto sobre la aplicación, ya que Integración con red virtual solo afecta al tráfico que sale de la aplicación. Para controlar el trafico de entrada a la aplicación, use la característica Restricciones de acceso. Un grupo de seguridad de red que se aplique a la subred de integración entrará en vigor con independencia de las rutas aplicadas a la subred de integración. Si el valor WEBSITE_VNET_ROUTE_ALL estuviera establecido en 1 y no hubiera ninguna ruta que afectara al tráfico de direcciones públicas en la subred de integración, todo el tráfico de salida seguiría estando sujeto a los grupos de seguridad de red asignados a la subred de integración. Si WEBSITE_VNET_ROUTE_ALL no estuviera definido, los grupos de seguridad de red solo se aplicarían al tráfico de RFC 1918.

### <a name="routes"></a>Rutas

Las tablas de rutas permiten enrutar el tráfico de salida de la aplicación a cualquier sitio que se desee. De forma predeterminada, las tablas de rutas solo afectarán al tráfico de destino de RFC 1918.  Si establece WEBSITE_VNET_ROUTE_ALL en 1, solo se verán afectadas las llamadas salientes. Las rutas establecidas en la subred de integración no afectarán a las respuestas de las solicitudes de entrada de la aplicación. Los destinos más habituales suelen ser puertas de enlace o dispositivos de firewall. Si desea enrutar todo el tráfico de salida del entorno local, puede utilizar una tabla de rutas para enviar el tráfico de salida a la puerta de enlace de ExpressRoute. Si no enruta el tráfico a una puerta de enlace, asegúrese de establecer las rutas en la red externa para poder enviar de vuelta las respuestas.

Las rutas del Protocolo de puerta de enlace de borde (BGP) también tendrán efecto sobre el tráfico de la aplicación. Si tiene rutas de BGP cuyo origen es algo similar a una puerta de enlace de ExpressRoute, el tráfico de salida de la aplicación se verá afectado. De forma predeterminada, las rutas de BGP solo afectan al tráfico de destino de RFC 1918. Si WEBSITE_VNET_ROUTE_ALL está establecido en 1, todo el tráfico de salida puede verse afectado por las rutas de BGP. 


<!--Image references-->
[4]: ../includes/media/web-sites-integrate-with-vnet/vnetint-appsetting.png

<!--Links-->
[VNETnsg]: https://docs.microsoft.com/azure/virtual-network/security-overview/
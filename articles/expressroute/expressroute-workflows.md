---
title: 'Azure ExpressRoute: Flujos de trabajo de configuración de circuito'
description: En esta página se muestran los flujos de trabajo para configurar el circuito ExpressRoute y las configuraciones entre pares
services: expressroute
author: cherylmc
ms.service: expressroute
ms.topic: conceptual
ms.date: 09/18/2018
ms.author: cherylmc
ms.openlocfilehash: 58914709838c72246678ce92005de5ac18695a1f
ms.sourcegitcommit: 3541c9cae8a12bdf457f1383e3557eb85a9b3187
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/09/2020
ms.locfileid: "86204160"
---
# <a name="expressroute-workflows-for-circuit-provisioning-and-circuit-states"></a>Flujos de trabajo de ExpressRoute para aprovisionamiento de circuitos y estados de circuitos de ExpressRoute
Esta página le guiará a través del aprovisionamiento de servicios y de los flujos de trabajo de configuración del enrutamiento a alto nivel.

![flujo de trabajo de circuito](./media/expressroute-workflows/expressroute-circuit-workflow.png)

En la ilustración siguiente y los pasos correspondientes se describen las tareas para aprovisionar un circuito ExpressRoute de un extremo a otro. 

1. Use PowerShell para configurar un circuito ExpressRoute. Para obtener más información, siga las instrucciones del artículo [Creación y modificación de circuitos de ExpressRoute](expressroute-howto-circuit-classic.md) .
2. Solicite conectividad al proveedor de servicios. Este proceso varía. Para obtener más información sobre cómo solicitar conectividad, póngase en contacto con su proveedor de conectividad.
3. Asegúrese de que el circuito se ha aprovisionado correctamente, para lo que debe comprobar el estado de aprovisionamiento del circuito ExpressRoute a través de PowerShell. 
4. Configure los dominios de enrutamiento. Si el proveedor de conectividad administra la configuración de nivel 3, configurará el enrutamiento del circuito. Si el proveedor de conectividad solo ofrece servicios de nivel 2, debe configurar el enrutamiento según las instrucciones que se describen en las páginas de [requisitos de enrutamiento](expressroute-routing.md) y [configuración de enrutamiento](expressroute-howto-routing-classic.md).
   
   * Habilitar el emparejamiento privado en Azure: habilite este emparejamiento para conectarse a VM/servicios en la nube implementados en redes virtuales.

   * Habilitar el emparejamiento de Microsoft: habilítelo para acceder a servicios en línea de Microsoft, como Office 365. Todos los servicios de PaaS de Azure están accesibles a través del emparejamiento de Microsoft.
     
     > [!IMPORTANT]
     > Debe asegurarse de que usa un proxy o borde independientes para conectarse a Microsoft distinto del que usa para Internet. Si usa el mismo borde para ExpressRoute e Internet se producirá un enrutamiento asimétrico y causará interrupciones en la conectividad de la red.
     > 
     > 
     
     ![flujos de trabajo de enrutamiento](./media/expressroute-workflows/routing-workflow.png)
5. Vinculación de redes virtuales a circuitos de ExpressRoute: puede vincular redes virtuales a un circuito ExpressRoute. Siga las instrucciones [para vincular redes virtuales](expressroute-howto-linkvnet-arm.md) al circuito. Dichas redes virtuales pueden estar en la misma suscripción de Azure que el circuito ExpressRoute, o bien pueden estar en una suscripción diferente.

## <a name="expressroute-circuit-provisioning-states"></a>Estados de aprovisionamiento de circuitos de ExpressRoute
Cada circuito ExpressRoute tiene dos estados:

* Estado de aprovisionamiento de proveedor de servicio
* Status

Estado representa el estado de aprovisionamiento de Microsoft. Esta propiedad se establece en Habilitado al crear un circuito Expressroute.

El estado de aprovisionamiento del proveedor de conectividad representa el estado del lado del proveedor de conectividad. Puede ser *No aprovisionado*, *Aprovisionando* o *Aprovisionado*. Para configurar el emparejamiento, el circuito ExpressRoute debe estar en un estado aprovisionado.

### <a name="possible-states-of-an-expressroute-circuit"></a>Posibles estados de un circuito ExpressRoute
En esta sección se enumeran los posibles estados de un circuito ExpressRoute.

**En tiempo de creación**

El circuito ExpressRoute notificará los estados siguientes durante la creación de recursos.

```output
ServiceProviderProvisioningState : NotProvisioned
Status                           : Enabled
```

**Cuando el proveedor de conectividad está en el proceso de aprovisionamiento del circuito**

El circuito ExpressRoute notificará los estados siguientes mientras el proveedor de conectividad trabaja para aprovisionar el circuito.

```output
ServiceProviderProvisioningState : Provisioning
Status                           : Enabled
```

**Cuando el proveedor de conectividad haya completado el proceso de aprovisionamiento**

El circuito ExpressRoute notificará los estados siguientes una vez que el proveedor de conectividad haya aprovisionado correctamente el circuito.

```output
ServiceProviderProvisioningState : Provisioned
Status                           : Enabled
```

**Cuando el proveedor de conectividad está desaprovisionando el circuito**

Si es necesario cancelar el aprovisionamiento del circuito ExpressRoute, el circuito notificará los estados siguientes una vez que el proveedor de servicios haya completado el proceso de desaprovisionamiento.

```output
ServiceProviderProvisioningState : NotProvisioned
Status                           : Enabled
```

Si es necesario, puede volver a habilitarlo, o bien ejecutar los cmdlets de PowerShell para eliminar el circuito.  

> [!IMPORTANT]
> No se puede eliminar un circuito cuando el valor de ServiceProviderProvisioningState sea Aprovisionamiento o Aprovisionado. El proveedor de conectividad debe desaprovisionar el circuito antes de que se pueda eliminar. Microsoft seguirá facturando el circuito hasta que se elimine el recurso del circuito ExpressRoute en Azure.
> 

## <a name="routing-session-configuration-state"></a>Estado de configuración de sesión de enrutamiento
El estado de aprovisionamiento de BGP notifica si la sesión BGP se ha habilitado en el perímetro de Microsoft. El estado debe estar habilitado para usar el emparejamiento privado o de Microsoft.

Es importante comprobar el estado de sesión de BGP, especialmente para la configuración entre pares de Microsoft. Además del estado de aprovisionamiento de BGP, hay otro estado denominado *estado de prefijos públicos anunciados*. El estado de los prefijos públicos anunciados debe estar en *configurado*, tanto para que la sesión BGP esté activa como para que el enrutamiento funcione de un extremo a otro. 

Si el estado de los prefijos públicos anunciados se establece en el estado de *validación necesaria* , la sesión BGP no se habilita, ya que los prefijos anunciados no coincidían con el número AS en ninguno de los registros de enrutamiento. 

> [!IMPORTANT]
> Si el estado de los prefijos públicos anunciados es *validación manual*, tendrá que abrir una incidencia de soporte técnico en el [servicio de soporte técnico de Microsoft](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) y aportar una evidencia de que posee la dirección IP anunciada, junto con el número del sistema autónomo asociado.
> 
> 

## <a name="next-steps"></a>Pasos siguientes
* Configure su conexión ExpressRoute.
  
  * [Creación de un circuito ExpressRoute](expressroute-howto-circuit-arm.md)
  * [Configuración del enrutamiento](expressroute-howto-routing-arm.md)
  * [Vinculación de redes virtuales a circuitos ExpressRoute](expressroute-howto-linkvnet-arm.md)


---
author: ccompy
ms.service: app-service-web
ms.topic: include
ms.date: 02/27/2020
ms.author: ccompy
ms.openlocfilehash: da43e639779dd4946cb014be777e3a6db0366e24
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "78671510"
---
* Los sistemas multiinquilino que admiten la gama completa de planes de precios, excepto Aislado.
* App Service Environment (ASE), que se implementa en la red virtual y admite aplicaciones de planes de precios aislados.

En este documento se explica la característica Integración con red virtual, que está indicada para aplicaciones multiinquilino. Si la aplicación está en [App Service Environment][ASEintro], ya está en una red virtual y no requiere el uso de la característica Integración con red virtual para acceder a recursos en la misma red virtual. Para obtener detalles sobre todas las características de redes, vea [Características de redes de App Service][Networkingfeatures].

Integración con red virtual proporciona a la aplicación acceso a los recursos de la red virtual, pero no concede acceso privado de entrada a la aplicación desde la red virtual. El acceso al sitio privado se refiere a que la aplicación solo es accesible desde una red privada, por ejemplo desde dentro de una red virtual de Azure. Integración con red virtual solo sirve para realizar llamadas salientes desde la aplicación hacia la red virtual. La característica Integración con red virtual se comporta de forma diferente cuando se usa con redes virtuales de la misma región y con redes virtuales de otras regiones. Esta característica tiene dos variantes.

* Versión regional de Integración con red virtual: si se conecta a redes virtuales de Resource Manager de la misma región, debe tener una subred dedicada en la red virtual con la que realizar la integración. 
* Versión de Integración con red virtual que necesita una puerta de enlace: para conectarse a redes virtuales de otras regiones o a una red virtual clásica de la misma región, necesita una puerta de enlace de Virtual Network aprovisionada en la red virtual de destino.

Integración con red virtual tiene las siguientes características:

* Necesita un plan de precios Estándar, Premium, PremiumV2 o Grupo elástico Premium. 
* Es compatible con TCP y UDP.
* Funciona con aplicaciones de App Service y de Functions.

Hay algunos aspectos que Integración con red virtual no admite, como:

* montar una unidad;
* la integración de AD; 
* NetBios;

Cuando Integración con red virtual necesita una puerta de enlace, solo proporciona acceso a los recursos de la red virtual de destino o de las redes conectadas a esta que tienen emparejamiento o redes privadas virtuales. Cuando Integración con red virtual necesita una puerta de enlace, no permite acceder a los recursos disponibles en las conexiones de ExpressRoute ni funciona con puntos de conexión de servicio. 

Independientemente de la versión que se use, Integración con red virtual ofrece a la aplicación acceso a los recursos de la red virtual, pero no concede acceso privado de entrada a la aplicación desde la red virtual. El acceso al sitio privado se refiere a que la aplicación solo es accesible desde una red privada, como desde dentro de una red virtual de Azure. Integración con red virtual solo sirve para realizar llamadas salientes desde la aplicación hacia la red virtual. 

<!--Links-->
[ASEintro]: https://docs.microsoft.com/azure/app-service/environment/intro
[Networkingfeatures]: https://docs.microsoft.com/azure/app-service/networking-features
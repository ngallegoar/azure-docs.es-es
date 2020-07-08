---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 05/06/2020
ms.author: glenga
ms.openlocfilehash: 5e0cff7bde6e80a776d694820ca7b69dafa7c0d9
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "83648825"
---
El acceso privado a sitios se refiere a que la aplicación solo sea accesible desde una red privada, como una red virtual de Azure.

* El acceso privado a sitios está disponible en los planes [Premium](../articles/azure-functions/functions-premium-plan.md), [Consumo](../articles/azure-functions/functions-scale.md#consumption-plan) y [App Service](../articles/azure-functions/functions-scale.md#app-service-plan) si se han configurado puntos de conexión de servicio.
    * Los puntos de conexión de servicio se pueden configurar por aplicación en **Características de la plataforma** > **Redes** > **Configurar restricciones de acceso** > **Agregar regla**. Las redes virtuales ahora se pueden seleccionar como un tipo de regla.
    * Para obtener más información, consulte [puntos de conexión de servicio de red virtual](../articles/virtual-network/virtual-network-service-endpoints-overview.md).
    * Tenga en cuenta que, con los puntos de conexión de servicio, la función todavía tiene acceso de salida completo a Internet, incluso cuando está configurada la integración de red virtual.
* El acceso privado a sitios también está disponible en una instancia de App Service Environment configurada con un equilibrador de carga interno (ILB). Para obtener más información, consulte [Creación y uso de un equilibrador de carga interno con un App Service Environment](../articles/app-service/environment/create-ilb-ase.md).

Para más información sobre cómo configurar el acceso a un sitio privado, consulte [Establecimiento del acceso a un sitio privado de Azure Functions](../articles/azure-functions/functions-create-private-site-access.md).
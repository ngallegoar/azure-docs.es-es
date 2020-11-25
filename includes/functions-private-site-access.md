---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 05/06/2020
ms.author: glenga
ms.openlocfilehash: 2d66e7f497f85141de172c59b67676e1bb93955e
ms.sourcegitcommit: 1d6ec4b6f60b7d9759269ce55b00c5ac5fb57d32
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/13/2020
ms.locfileid: "94578836"
---
Un [punto de conexión privado de Azure](../articles/private-link/private-endpoint-overview.md) es una interfaz de red que nos conecta de forma privada y segura a un servicio por medio de la tecnología Azure Private Link.  Los puntos de conexión privados emplean una dirección IP privada de la red virtual, lo que hace que el servicio se incluya en la red virtual.

Puede usar el punto de conexión privado para las funciones hospedadas en los planes [Premium](../articles/azure-functions/functions-premium-plan.md) y [App Service](../articles/azure-functions/functions-scale.md#app-service-plan).

Al crear una conexión de punto de conexión privado entrante para las funciones, también necesitará un registro DNS para resolver la dirección privada.  De manera predeterminada, se creará automáticamente un registro DNS privado al crear un punto de conexión privado mediante Azure Portal.

Para más información, consulte cómo [usar puntos de conexión privados para aplicaciones web](../articles/app-service/networking/private-endpoint.md).
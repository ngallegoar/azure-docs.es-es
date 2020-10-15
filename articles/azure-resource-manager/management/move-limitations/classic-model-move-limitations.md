---
title: Movimiento de los recursos de implementación la versión clásica de Azure
description: Use Azure Resource Manager para mover los recursos de implementación de la versión clásica a un nuevo grupo de recursos o una nueva suscripción.
ms.topic: conceptual
ms.date: 07/09/2019
ms.openlocfilehash: 78b9769a31fa0c96c12e18d05cb9c484aa52a1d5
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "75476618"
---
# <a name="move-guidance-for-classic-deployment-model-resources"></a>Guía para mover los recursos del modelo de implementación clásica

Los pasos para mover los recursos implementados en el modelo clásico varían en función de si los recursos se trasladan dentro de una misma suscripción o se trasladan a otra nueva.

## <a name="move-in-the-same-subscription"></a>Traslado en la misma suscripción

Al mover recursos de un grupo de recursos a otro dentro de la misma suscripción, se aplican las restricciones siguientes:

* No se pueden mover redes virtuales (clásico).
* Las máquinas virtuales (clásico) se deben mover con el servicio en la nube.
* El servicio en la nube solo se puede mover cuando el traslado incluye todas sus máquinas virtuales.
* Solo se puede mover un servicio en la nube cada vez.
* Solo se puede mover una cuenta de almacenamiento (clásico) cada vez.
* No se puede mover una cuenta de almacenamiento (clásico) con una máquina virtual o un servicio en la nube en la misma operación de traslado.

Para mover los recursos de la versión clásica a un nuevo grupo de recursos de la misma suscripción, utilice las [operaciones de traslado estándar](../move-resource-group-and-subscription.md) en el portal, Azure PowerShell, la CLI de Azure o la API REST. Utilice las mismas operaciones que utiliza para trasladar recursos de Resource Manager.

## <a name="move-across-subscriptions"></a>Traslado entre suscripciones

Al trasladar recursos a una nueva suscripción, se aplican las restricciones siguientes:

* Todos los recursos clásicos de la suscripción se deben mover en la misma operación.
* La suscripción de destino no debe contener otros recursos clásicos.
* El traslado solo puede solicitarse a través de una API de REST independiente para el traslado de recursos clásicos. Los comandos de movimiento estándar de Resource Manager no funcionan para mover recursos clásicos a una nueva suscripción.

Para trasladar recursos clásicos a una nueva suscripción, use operaciones REST específicas para recursos clásicos. Para usar REST, siga estos pasos:

1. Compruebe si la suscripción de origen puede participar en un movimiento entre suscripciones. Utilice la siguiente operación:

   ```HTTP
   POST https://management.azure.com/subscriptions/{sourceSubscriptionId}/providers/Microsoft.ClassicCompute/validateSubscriptionMoveAvailability?api-version=2016-04-01
   ```

     En el cuerpo de la solicitud, incluya:

   ```json
   {
    "role": "source"
   }
   ```

     La respuesta para la operación de validación está en el formato siguiente:

   ```json
   {
    "status": "{status}",
    "reasons": [
      "reason1",
      "reason2"
    ]
   }
   ```

1. Compruebe si la suscripción de destino puede participar en un movimiento entre suscripciones. Utilice la siguiente operación:

   ```HTTP
   POST https://management.azure.com/subscriptions/{destinationSubscriptionId}/providers/Microsoft.ClassicCompute/validateSubscriptionMoveAvailability?api-version=2016-04-01
   ```

     En el cuerpo de la solicitud, incluya:

   ```json
   {
    "role": "target"
   }
   ```

     La respuesta está en el mismo formato que la validación de la suscripción de origen.
1. Si ambas suscripciones superan la validación, traslade todos los recursos clásicos de una suscripción a otra con la siguiente operación:

   ```HTTP
   POST https://management.azure.com/subscriptions/{subscription-id}/providers/Microsoft.ClassicCompute/moveSubscriptionResources?api-version=2016-04-01
   ```

    En el cuerpo de la solicitud, incluya:

   ```json
   {
    "target": "/subscriptions/{target-subscription-id}"
   }
   ```

Es posible que esta operación tarde varios minutos.

## <a name="next-steps"></a>Pasos siguientes

Si tiene problemas para mover los recursos de la versión clásica, póngase en contacto con el servicio de [soporte técnico](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview).

Para ver los comandos para trasladar recursos, vea [Traslado de los recursos a un nuevo grupo de recursos o a una nueva suscripción](../move-resource-group-and-subscription.md).

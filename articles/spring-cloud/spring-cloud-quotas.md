---
title: Planes de servicio y cuotas para Azure Spring Cloud
description: Más información sobre las cuotas y los planes de servicio de Azure Spring Cloud
author: bmitchell287
ms.service: spring-cloud
ms.topic: conceptual
ms.date: 11/04/2019
ms.author: brendm
ms.openlocfilehash: 8a7ba3c3b9c19b2084b6892b55ac417da38ab047
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "76278886"
---
# <a name="quotas-and-service-plans-for-azure-spring-cloud"></a>Cuotas y planes de servicio de Azure Spring Cloud

Todos los servicios de Azure establecen límites y cuotas predeterminados para recursos y características.  Durante el período de versión preliminar, Azure Spring Cloud ofrece solo un plan de servicio.

En este artículo se detallan las cuotas de servicio que se ofrecen durante el período de versión preliminar actual.

## <a name="azure-spring-cloud-service-tiers-and-quotas"></a>Niveles y cuotas de servicio en Azure Spring Cloud

Durante el período de versión preliminar, Azure Spring Cloud ofrece solo un nivel de servicio.

Resource | Importe
------- | -------
vCPU | 4 por instancia de servicio
Memoria | 8 GBytes por instancia de servicio
Instancias del servicio Azure Spring Cloud por región y suscripción | 10
Instancias de aplicación totales por instancia del servicio Azure Spring Cloud | 500
Instancias de aplicación totales por aplicación Spring | 20
Volúmenes persistentes | 10 x 50 GBytes

Cuando alcance una cuota, recibirá un error 400 que indica: "Quota exceeds limit for subscription *your subscription* in region *region where your Azure Spring Cloud service is created*" (La cuota supera el límite de la suscripción <su suscripción> en la región <región en la que se ha creado el servicio de Azure Spring Cloud>).

## <a name="next-steps"></a>Pasos siguientes

Pueden aumentarse ciertas cuotas y límites predeterminados. Si el recurso requiere un aumento, [cree una solicitud de soporte técnico](https://docs.microsoft.com/azure/azure-portal/supportability/how-to-create-azure-support-request).

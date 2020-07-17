---
title: Planes de servicio y cuotas para Azure Spring Cloud
description: Más información sobre las cuotas y los planes de servicio de Azure Spring Cloud
author: bmitchell287
ms.service: spring-cloud
ms.topic: conceptual
ms.date: 11/04/2019
ms.author: brendm
ms.openlocfilehash: dc4426fc08efff44bc46ba37eb6fbc6fe31f356b
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "84735445"
---
# <a name="quotas-and-service-plans-for-azure-spring-cloud"></a>Cuotas y planes de servicio de Azure Spring Cloud

Todos los servicios de Azure establecen límites y cuotas predeterminados para recursos y características.   Azure Spring Cloud ofrece dos planes de tarifa: Básico y Estándar. En este artículo se detallan los límites de ambos niveles.

## <a name="azure-spring-cloud-service-tiers-and-limits"></a>Límites y niveles de servicio de Azure Spring Cloud

| Recurso | Básico | Estándar
------- | ------- | -------
vCPU | 1 por instancia de servicio | 4 por instancia de servicio
Memoria | 2 GB por instancia de servicio | 8 GB por instancia de servicio
Instancias del servicio Azure Spring Cloud por región y suscripción | 10 | 10
Instancias de aplicación totales por instancia del servicio Azure Spring Cloud | 25 | 500
Volúmenes persistentes | 1 GB/aplicación x 10 aplicaciones | 50 GB/aplicación x 10 aplicaciones


Durante el período de versión preliminar, Azure Spring Cloud ofrece solo un nivel de servicio. Cuando alcance un límite, recibirá un error 400 que indica lo siguiente: "Quota exceeds limit for subscription *your subscription* in region *region where your Azure Spring Cloud service is created*" (La cuota supera el límite de la suscripción <su suscripción> en la región <región en la que se ha creado el servicio de Azure Spring Cloud>).

## <a name="next-steps"></a>Pasos siguientes

Algunos límites predeterminados se pueden aumentar. Si su configuración requiere un aumento, [cree una solicitud de soporte técnico](https://docs.microsoft.com/azure/azure-portal/supportability/how-to-create-azure-support-request).

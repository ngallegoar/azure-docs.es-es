---
title: Información general sobre Resource Health de Azure Application Gateway
description: En este artículo se ofrece información general de la característica Resource Health de Azure Application Gateway
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: article
ms.date: 7/9/2019
ms.author: victorh
ms.openlocfilehash: 7e30a93f8270cfaf8910130cc1e2633bb80c2b8e
ms.sourcegitcommit: 0ce1ccdb34ad60321a647c691b0cff3b9d7a39c8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/05/2020
ms.locfileid: "93397179"
---
# <a name="azure-application-gateway-resource-health-overview"></a>Información general sobre Resource Health de Azure Application Gateway

[Azure Resource Health](../service-health/resource-health-overview.md) ayuda a diagnosticar los problemas en los servicios de Azure que afectan a los recursos y a obtener soporte técnico para resolverlos. Informa sobre el mantenimiento actual y pasado de los recursos. Y proporciona soporte técnico para ayudar a mitigar los problemas.

En Application Gateway, Resource Health se basa en las señales que emite la puerta de enlace para evaluar si su estado es correcto o no. Si el estado de la puerta de enlace no es correcto, Resource Health analiza información adicional para determinar el origen del problema. También identifica las acciones que Microsoft está llevando a cabo o lo que el usuario puede hacer para solucionar el problema.

Para más información sobre cómo se evalúa el mantenimiento, revise la lista completa de tipos de recurso y comprobaciones de mantenimiento en [Azure Resource Health](../service-health/resource-health-checks-resource-types.md#microsoftnetworkapplicationgateways).


El estado de Application Gateway se muestra como uno de los siguientes:

## <a name="available"></a>Disponible

Un estado **Disponible** significa que el servicio no ha detectado ningún evento que afecte al estado del recurso. En aquellos casos en los que la puerta de enlace se haya recuperado de un tiempo de inactividad no planeado en las últimas 24 horas, se ve la notificación **Resuelto recientemente**.

![Estado Disponible](media/resource-health-overview/available-full.png)

## <a name="unavailable"></a>No disponible

Un estado **No disponible** significa que el servicio ha detectado un evento en curso relacionado o no con la plataforma que afecta al estado de la puerta de enlace.

### <a name="platform-events"></a>Eventos de plataforma

Varios componentes de la infraestructura de Azure desencadenan estos eventos. Por ejemplo, acciones programadas (como mantenimiento planeado) e incidentes inesperados (como reinicio del host no planeado).

Resource Health proporciona detalles adicionales sobre el evento y el proceso de recuperación. También permite ponerse en contacto con soporte técnico, incluso si no tiene un contrato de soporte técnico de Microsoft activo.

![Estado No disponible](media/resource-health-overview/unavailable.png)

## <a name="unknown"></a>Desconocido

El estado **Desconocido** indica que Resource Health no ha recibido información sobre la puerta de enlace desde hace más de 10 minutos. Este estado no es una indicación definitiva del estado de la puerta de enlace, aunque es un punto de datos importante en el proceso de solución de problemas.

Si la puerta de enlace se ejecuta según lo esperado, el estado cambia a **Disponible** al cabo de unos minutos.

Si tiene problemas, el estado **Desconocido** podría sugerir que un evento de la plataforma afecta a la puerta de enlace.

![Estado Desconocido](media/resource-health-overview/unknown.png)

## <a name="degraded"></a>Degradado

El estado **Degradado** indica que la puerta de enlace ha detectado una pérdida de rendimiento, aunque sigue estando disponible para su uso.

![Estado Degradado](media/resource-health-overview/degraded.png)

## <a name="next-steps"></a>Pasos siguientes

Para obtener información sobre cómo solucionar problemas del firewall de aplicaciones web (WAF) de Application Gateway, vea [Solución de problemas del firewall de aplicaciones web (WAF) de Azure Application Gateway](../web-application-firewall/ag/web-application-firewall-troubleshoot.md).
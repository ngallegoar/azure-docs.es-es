---
title: 'Azure Front Door: protección contra DDoS'
description: En esta página se proporciona información sobre cómo Azure Front Door ayuda para proteger contra los ataques DDoS
services: frontdoor
documentationcenter: ''
author: johndowns
ms.service: frontdoor
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/28/2020
ms.author: jodowns
ms.openlocfilehash: 58efeaebcc9f643c725cde54fcbda6f65c4bd700
ms.sourcegitcommit: 17b36b13857f573639d19d2afb6f2aca74ae56c1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2020
ms.locfileid: "94413805"
---
# <a name="ddos-protection-on-front-door"></a>Protección contra DDoS en Front Door

Azure Front Door tiene varias características que pueden ayudar a evitar ataques de denegación de servicio distribuido (DDoS). Estas características pueden impedir que los atacantes lleguen a la aplicación y afecten a la disponibilidad y el rendimiento de la misma.

## <a name="integration-with-azure-ddos-protection-basic"></a>Integración con el nivel básico de Azure DDoS Protection

Front Door está protegido con el nivel básico de Azure DDoS Protection. Se integra en la plataforma de Front Door de forma predeterminada y sin costo adicional. En su conjunto, el tamaño y la capacidad de la red de implementación global de Front Door proporcionan una defensa contra los ataques al nivel de red más comunes mediante la supervisión constante del tráfico y la mitigación en tiempo real. La Protección contra DDoS básica defiende también contra los ataques "flood" de congestión del servidor de consulta de DNS de capa 7 y volumétricos de capas 3 y 4 más comunes y frecuentes que se dirigen a los puntos de conexión públicos. Este servicio también tiene un registro de seguimiento comprobado en la protección de los servicios empresariales y de consumo de Microsoft contra ataques a gran escala. Para más información, consulte [Azure DDoS Protection](../security/fundamentals/ddos-best-practices.md).

## <a name="protocol-blocking"></a>Bloqueo de protocolo

Front Door solo acepta el tráfico en los protocolos HTTP y HTTPS, y solo procesará solicitudes válidas con un encabezado `Host` conocido. Este comportamiento ayuda a mitigar algunos tipos de ataque DDoS comunes, incluidos los ataques volumétricos que se reparten entre una gama de protocolos y puertos, los ataques de amplificación de DNS y los ataques de envenenamiento de TCP.

## <a name="capacity-absorption"></a>Absorción de capacidad

Front Door es un servicio escalado a gran escala y distribuido globalmente. Tenemos muchos clientes, incluidos los propios productos en la nube a gran escala de Microsoft, que reciben cientos de miles de solicitudes cada segundo. Front Door se encuentra en el perímetro de la red de Azure, absorbiendo y aislando geográficamente ataques de gran volumen. Esto puede impedir que el tráfico malintencionado vaya más allá del perímetro de la red de Azure.

## <a name="caching"></a>Almacenamiento en memoria caché

Las [funcionalidades de almacenamiento en caché de Front Door](./front-door-caching.md) se pueden usar para proteger los servidores back-end de grandes volúmenes de tráfico generados por un ataque. Los recursos almacenados en caché se devolverán desde los nodos perimetrales de Front Door para que no se reenvíen al back-end. Incluso los tiempos de expiración de caché breves (segundos o minutos) en las respuestas dinámicas pueden reducir en gran medida la carga en los servicios de back-end. Para obtener más información sobre el almacenamiento en caché de conceptos y patrones, consulte [Almacenamiento en memoria caché](https://docs.microsoft.com/azure/architecture/best-practices/caching) y [Patrón Cache-Aside](https://docs.microsoft.com/azure/architecture/patterns/cache-aside).

## <a name="web-application-firewall-waf"></a>Firewall de aplicaciones web (WAF)

[Web Application Firewall (WAF) de Front Door](../web-application-firewall/afds/afds-overview.md) se puede usar para mitigar una serie de diversos tipos de ataques:

* El uso del conjunto de reglas administradas proporciona protección contra una serie de ataques comunes.
* El tráfico dentro o fuera de una región geográfica definida se puede bloquear o redirigir a una página web estática. Para más información, consulte [Filtro geográfico](../web-application-firewall/afds/waf-front-door-geo-filtering.md).
* Las direcciones IP y los intervalos que identifique como malintencionados se pueden bloquear.
* La limitación de frecuencia se puede aplicar para impedir que las direcciones IP llamen a su servicio con demasiada frecuencia.
* Puede crear [reglas personalizadas de WAF](../web-application-firewall/afds/waf-front-door-custom-rules.md) para bloquear automáticamente los ataques HTTP o HTTPS que tienen firmas conocidas, así como para limitar el volumen de estos.

## <a name="for-further-protection"></a>Para una mayor protección

Si requiere una mayor protección, puede habilitar [Azure DDoS Protection estándar](../security/fundamentals/ddos-best-practices.md#ddos-protection-standard) en la red virtual donde se implementan sus servidores back-end. Los clientes de DDoS Protection estándar reciben ventajas adicionales, como protección de los costos, garantía del SLA y acceso a expertos del equipo de respuesta rápida de DDoS para obtener ayuda inmediata durante un ataque.

## <a name="next-steps"></a>Pasos siguientes

- Aprenda a configurar un [perfil WAF en Front Door](front-door-waf.md). 
- Aprenda a [crear una instancia de Front Door](quickstart-create-front-door.md).
- Más información acerca de cómo [funciona Front Door](front-door-routing-architecture.md).

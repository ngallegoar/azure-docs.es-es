---
title: Procedimientos recomendados fundamentales de Azure DDoS Protection
description: Obtenga información sobre los procedimientos recomendados de seguridad con DDoS Protection.
services: ddos-protection
documentationcenter: na
author: yitoh
ms.service: ddos-protection
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/08/2020
ms.author: yitoh
ms.openlocfilehash: 594a7e2a6977cc2a7052a15e1a007c68c08da259
ms.sourcegitcommit: 693df7d78dfd5393a28bf1508e3e7487e2132293
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/28/2020
ms.locfileid: "92904965"
---
# <a name="fundamental-best-practices"></a>Procedimientos recomendados fundamentales

En las siguientes secciones se proporcionan instrucciones preceptivas para crear servicios resistentes a los ataques de DDoS en Azure.

## <a name="design-for-security"></a>Diseño para seguridad

Asegúrese de que la seguridad es prioritaria durante todo el ciclo de vida de una aplicación, desde su diseño e implementación hasta la implementación y las operaciones. Las aplicaciones pueden tener errores que permiten que un volumen relativamente bajo de solicitudes usen una cantidad excesiva de recursos y produzcan una interrupción del servicio. 

Para proteger un servicio que se ejecuta en Microsoft Azure, debe conocer bien la arquitectura de su aplicación y centrarse en los [cinco pilares de la calidad del software](/azure/architecture/guide/pillars).
Debe conocer los volúmenes de tráfico típicos, el modelo de conectividad entre la aplicación y otras aplicaciones, y los puntos de conexión del servicio expuestos a la red pública de Internet.

Es de vital importancia garantizar que una aplicación sea lo suficientemente resistente para tratar con un ataque de denegación de servicio dirigido a la propia aplicación. La seguridad y la privacidad están integradas en la plataforma Azure, comenzando por el [ciclo de vida del desarrollo de la seguridad (SDL)](https://www.microsoft.com/sdl/default.aspx). El SDL aborda la seguridad en cada fase de desarrollo y se asegura de que Azure se actualice continuamente para que sea aún más seguro.

## <a name="design-for-scalability"></a>Diseño para escalabilidad

La escalabilidad representa el grado en que un sistema puede controlar el aumento de la carga. Debe diseñar sus aplicaciones de modo que se puedan [escalar horizontalmente](/azure/architecture/guide/design-principles/scale-out) para satisfacer la demanda de una carga mayor, específicamente en caso de un ataque de DDoS. Si la aplicación depende de una única instancia de un servicio, crea un único punto de error. El aprovisionamiento de varias instancias hace que el sistema sea más resistente y más escalable.

Para [Azure App Service](/azure/app-service/app-service-value-prop-what-is), seleccione un [Plan de App Service](/azure/app-service/overview-hosting-plans) que ofrezca varias instancias. Para Azure Cloud Services, configure cada uno de los roles para utilizar [varias instancias](/azure/cloud-services/cloud-services-choose-me). En el caso de [Azure Virtual Machines](../virtual-machines/index.yml), asegúrese de que la arquitectura de las máquinas virtuales incluya más de una máquina virtual y que cada una de ellas se incluya en un [conjunto de disponibilidad](../virtual-machines/windows/tutorial-availability-sets.md). Se recomienda usar [conjuntos de escalado de máquinas virtuales](../virtual-machine-scale-sets/overview.md) para contar con funcionalidades de escalado automático.

## <a name="defense-in-depth"></a>Defensa en profundidad

La idea que subyace a la defensa en profundidad es administrar los riesgos con diversas estrategias defensivas. Disponer en niveles la defensa de la seguridad en una aplicación reduce las probabilidades de éxito de un ataque. Se recomienda que implemente diseños seguros para sus aplicaciones con las funcionalidades integradas de la plataforma Azure.

Por ejemplo, el riesgo de ataques aumenta con el tamaño ( *área expuesta* ) de la aplicación. Puede reducir el área expuesta mediante una lista de aprobación para cerrar el espacio de direcciones IP expuesto y los puertos de escucha que no sean necesarios en los equilibradores de carga ([Azure Load Balancer](/azure/load-balancer/load-balancer-get-started-internet-portal) y [Azure Application Gateway](/azure/application-gateway/application-gateway-create-probe-portal)). Los [grupos de seguridad de red (NSG)](/azure/virtual-network/security-overview) constituyen otra manera de reducir el área expuesta a ataques.
Puede usar [etiquetas de servicio](/azure/virtual-network/security-overview#service-tags) y [grupos de seguridad de la aplicación](/azure/virtual-network/security-overview#application-security-groups) para minimizar la complejidad de la creación de reglas de seguridad y configurar la seguridad de la red como una extensión natural de la estructura de una aplicación.

Debe implementar los servicios de Azure en una [red virtual](/azure/virtual-network/virtual-networks-overview) siempre que sea posible. Este procedimiento permite que los recursos del servicio se comuniquen mediante direcciones IP privadas. De forma predeterminada, el tráfico de los servicios Azure desde una red virtual usa direcciones IP públicas como direcciones IP de origen. Con los [puntos de conexión de servicio](/azure/virtual-network/virtual-network-service-endpoints-overview), el tráfico del servicio cambia para usar direcciones privadas de red virtual como direcciones IP de origen al acceder al servicio de Azure desde una red virtual.

Con frecuencia vemos ataques a los recursos locales de un cliente, además de a los recursos en Azure. Si conecta un entorno local a Azure, se recomienda que reduzca al mínimo la exposición de los recursos locales a la red pública de Internet. Para usar las funcionalidades de escalado y protección contra DDoS de Azure puede implementar entidades públicas bien conocidas en Azure. Como estas entidades de acceso público suelen ser destinatarias de ataques de DDoS, al colocarlas en Azure se reduce el impacto en los recursos locales.

## <a name="next-steps"></a>Pasos siguientes

- Aprenda a [crear un plan de protección contra DDoS](manage-ddos-protection.md).
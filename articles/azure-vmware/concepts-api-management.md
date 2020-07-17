---
title: 'Conceptos: API Management'
description: Obtenga información sobre la manera en que API Management protege las API que se ejecutan en máquinas virtuales (VM) de Azure VMware Solution (AVS)
ms.topic: conceptual
ms.date: 06/23/2020
ms.openlocfilehash: 62112bf3c0bf551232e09e5910e3eaae228dc202
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85306812"
---
# <a name="api-management-to-publish-and-protect-apis-running-on-avs-based-vms"></a>Publicación y protección con API Management de las API que se ejecutan en máquinas virtuales basadas en AVS

Microsoft Azure [API Management](https://azure.microsoft.com/services/api-management/) permite a los desarrolladores y a los equipos de DevOps hacer publicaciones de forma segura para consumidores internos o externos.

Aunque se ofrecen en varias SKU, solo las SKU de desarrollador y premium permiten la integración de Azure Virtual Network para publicar las API que se ejecutan en cargas de trabajo de Azure VMware Solution (AVS). Estas dos SKU habilitan de forma segura la conectividad entre el servicio de API Management y el back-end. La SKU de desarrollador está pensada para desarrollo y pruebas, mientras que la SKU premium es para implementaciones de producción.

En el caso de los servicios de back-end que se ejecutan sobre máquinas virtuales de AVS, la configuración de API Management es, de forma predeterminada, la misma que la de los servicios de back-end locales. Tanto para las implementaciones internas como para las externas, API Management configura la IP virtual (VIP) del equilibrador de carga como el punto de conexión de back-end cuando el servidor back-end se coloca detrás de un equilibrador de carga NSX en el lado de AVS.

## <a name="external-deployment"></a>Implementación externa

Una implementación externa publica las API que utilizan los usuarios externos mediante un punto de conexión público. Los desarrolladores y los ingenieros de DevOps pueden administrar las API a través de Azure Portal o PowerShell, y desde el portal para desarrolladores de API Management.

El diagrama de implementación externa muestra todo el proceso y los actores implicados (mostrados en la parte superior). Los actores son:

- **Administradores:** representa el equipo de administradores o DevOps, que administra AVS a través de Azure Portal y de mecanismos de automatización como PowerShell o Azure DevOps.

- **Usuarios:**  representa los consumidores de las API expuestas, y pueden ser los usuarios y los servicios que consumen las API.

El flujo de tráfico pasa por la instancia de API Management, que abstrae los servicios de back-end, conectados a la red virtual del centro. La puerta de enlace de ExpressRoute enruta el tráfico al canal Global Reach de ExpressRoute. Luego, el tráfico llega a un equilibrador de carga NSX, que distribuye el tráfico entrante a las distintas instancias de servicios de back-end.

API Management tiene una API pública de Azure, y se recomienda activar el servicio Azure DDOS Protection. 

:::image type="content" source="media/api-management/external-deployment.png" alt-text="Implementación externa: API Management para AVS":::


## <a name="internal-deployment"></a>Implementación interna

Una implementación interna publica las API que consumen los usuarios o sistemas internos. El equipo de DevOps y los desarrolladores de API usan las mismas herramientas de administración y el mismo portal para desarrolladores que en la implementación externa.

Las implementaciones internas pueden hacerse con [Azure Application Gateway](../api-management/api-management-howto-integrate-internal-vnet-appgateway.md) para crear un punto de conexión público y seguro de modo que la API aproveche sus capacidades y cree una implementación híbrida que permita escenarios diferentes.  La API aprovecha sus capacidades y crea una implementación híbrida que habilita distintos escenarios.

* Utilizar el mismo recurso de API Management para su uso por parte de usuarios internos y externos.

* Tener un único recurso de API Management con un subconjunto de API definido y disponible para los consumidores externos.

* Proporcionar una solución fácil para activar y desactivar el acceso a API Management desde la red Internet pública.

En el diagrama de implementación siguiente se muestran los consumidores, que pueden ser internos o externos, y cada tipo tiene acceso a las mismas API o a API diferentes.

En una implementación interna, las API se exponen a la misma instancia de API Management. Delante de API Management, Application Gateway se implementa con la funcionalidad de Firewall de aplicaciones web (WAF) de Azure activada y un conjunto de agentes de escucha HTTP y reglas para filtrar el tráfico. Se expone solo un subconjunto de los servicios back-end que se ejecutan en AVS.

* El tráfico interno se enruta a través de la puerta de enlace de ExpressRoute a Azure Firewall y, después, a API Management si se establecen reglas de tráfico, o bien directamente a API Management.  

* El tráfico externo entra en Azure a través de Application Gateway, que usa el nivel de protección externa para API Management.


:::image type="content" source="media/api-management/internal-deployment.png" alt-text="Implementación interna: API Management para AVS":::
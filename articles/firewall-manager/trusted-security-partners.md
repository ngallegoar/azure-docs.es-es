---
title: Definición de los asociados de seguridad de confianza de Azure Firewall Manager (versión preliminar)
description: Información sobre los asociados de seguridad de confianza de Azure Firewall Manager
author: vhorne
ms.service: firewall-manager
services: firewall-manager
ms.topic: conceptual
ms.date: 10/30/2019
ms.author: victorh
ms.openlocfilehash: b92242ce9086579d0397f78853402cfc08453f68
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "75436774"
---
# <a name="what-are-trusted-security-partners-preview"></a>¿Qué son los asociados de seguridad de confianza (versión preliminar)?

> [!IMPORTANT]
> Esta versión preliminar pública se proporciona sin un acuerdo de nivel de servicio y no debe usarse para cargas de trabajo de producción. Puede que algunas características no se admitan, que tengan funcionalidades limitadas o que no estén disponibles en todas las ubicaciones de Azure. Para más información, consulte [Términos de uso complementarios de las versiones preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Los *asociados de seguridad de confianza (versión preliminar)* de Azure Firewall Manager le permiten usar las mejores ofertas de seguridad como servicio (SECaaS) de terceros que ya conoce para proteger el acceso a Internet de sus usuarios.

Con una configuración rápida, puede proteger un centro de conectividad con un asociado de seguridad compatible, así como enrutar y filtrar el tráfico de Internet de sus redes virtuales (VNet) o ubicaciones de rama en una región. Esto se hace mediante la administración de rutas automatizada, sin configurar ni administrar rutas definidas por el usuario (UDR).

Puede implementar centros de conectividad protegidos configurados con el asociado de seguridad que prefiera en varias regiones de Azure para obtener conectividad y seguridad para los usuarios en cualquier lugar del mundo en dichas regiones. Con la capacidad de usar la oferta del asociado de seguridad para el tráfico de la aplicación de Internet/SaaS y Azure Firewall para el tráfico privado en los centros de conectividad de protegidos, ahora puede empezar a crear el perímetro de seguridad en Azure próximo a las aplicaciones y los usuarios distribuidos globalmente.

Para esta versión preliminar, los asociados de seguridad admitidos son **ZScaler** e **iboss**. Las regiones admitidas son WestCentralUS, NorthCentralUS, Westus, WestUS2 y EastUS.

![Asociados de seguridad de confianza](media/trusted-security-partners/trusted-security-partners.png)

## <a name="key-scenarios"></a>Escenarios principales

Puede usar los asociados de seguridad para filtrar el tráfico de Internet en los siguientes escenarios:

- De red virtual (VNet) a Internet

   Aproveche la protección de Internet avanzada con reconocimiento del usuario para sus cargas de trabajo en la nube que se ejecutan en Azure.

- De rama a Internet

   Aproveche la conectividad de Azure y la distribución global para agregar fácilmente el filtrado de NSaaS de terceros para escenarios de rama a Internet. Puede crear la red de tránsito global y el perímetro de seguridad mediante Azure Virtual WAN.

Se admiten los escenarios siguientes:
-   De red virtual a Internet a través de una oferta de asociado de terceros.
-   De rama a Internet a través de una oferta de asociado de terceros.
-   De rama a Internet a través de una oferta de asociado de terceros; el resto del tráfico privado (de radio a radio, de radio a ramas, de rama a radio) a través de Azure Firewall.

El escenario siguiente no se admite:

- La oferta de red virtual a Internet a través de un asociado no se puede combinar con Azure Firewall para el tráfico privado. Consulte las siguientes limitaciones.

## <a name="current-limitations"></a>Limitaciones actuales

- Para la oferta de red virtual a Internet, no se puede combinar la adición de Azure Firewall para el tráfico privado con una oferta de asociado para el tráfico de Internet. Puede enviar tráfico de Internet a Azure Firewall o a una oferta de asociado de seguridad de terceros en el centro virtual protegido, pero no a ambos. 
- Puede implementar como máximo un asociado de seguridad por centro virtual. Si necesita cambiar el proveedor, debe quitar el asociado existente y agregar uno nuevo.

## <a name="best-practices-for-internet-traffic-filtering-in-secured-virtual-hubs"></a>Procedimientos recomendados para el filtrado de tráfico de Internet en los centros virtuales protegidos

El tráfico de Internet suele incluir tráfico web. No obstante, también incluye tráfico destinado a aplicaciones SaaS como Office 365 (O365) y servicios PaaS públicos de Azure, como Azure Storage, Azure SQL, etc. A continuación se muestran los procedimientos recomendados para controlar el tráfico a estos servicios:

### <a name="handling-azure-paas-traffic"></a>Control del tráfico de PaaS de Azure
 
- Use Azure Firewall para la protección si el tráfico es principalmente de PaaS de Azure y el acceso a los recursos para las aplicaciones se puede filtrar mediante direcciones IP, FQDN, etiquetas de servicio o etiquetas FQDN.

- Use una solución de asociados de terceros en sus centros de conectividad si el tráfico es de acceso a las aplicaciones SaaS, si necesita filtrado que tenga en cuenta al usuario [por ejemplo, para cargas de trabajo de infraestructura de escritorio virtual (VDI)] o si necesita capacidades de filtrado de Internet avanzadas.

![Todos los escenarios de Azure Firewall Manager](media/trusted-security-partners/all-scenarios.png)

## <a name="handling-office-365-o365-traffic"></a>Control del tráfico de Office 365 (O365)

En escenarios de ubicación de rama distribuida globalmente, debe redirigir el tráfico de Office 365 directamente en la rama antes de enviar el tráfico de Internet restante a su centro de conectividad protegido de Azure.

Para Office 365, la latencia y el rendimiento de la red son fundamentales para lograr una experiencia del usuario satisfactoria. Para lograr estos objetivos en torno al rendimiento óptimo y la experiencia del usuario, los clientes deben implementar el escape de Office 365 directo y local antes de considerar el enrutamiento del resto del tráfico de Internet a través de Azure.

Los [principios de conectividad de red de Office 365](https://docs.microsoft.com/office365/enterprise/office-365-network-connectivity-principles) requieren que las principales conexiones de red de Office 365 se enruten localmente desde la rama del usuario o el dispositivo móvil y directamente a través de Internet en el punto de presencia de la red de Microsoft más cercano.

Además, las conexiones de Office 365 están fuertemente cifradas para proteger la privacidad, y usan protocolos propietarios eficaces por motivos de rendimiento. En consecuencia, el hecho de someter estas conexiones a las soluciones de seguridad de nivel de red tradicionales puede ser poco práctico y tener un gran impacto. Por estos motivos, se recomienda encarecidamente que los clientes envíen el tráfico de Office 365 directamente desde las ramas, antes de enviar el resto del tráfico a través de Azure. Microsoft se ha asociado con varios proveedores de soluciones SD-WAN, que se integran con Azure y Office 365, y que permiten a los clientes habilitar fácilmente el desglose de Internet de Office 365 directo y local. Para obtener más información, consulte [¿Cómo configuro mis directivas de Office 365 a través de Virtual WAN?](https://docs.microsoft.com/azure/virtual-wan/virtual-wan-office365-overview)


## <a name="next-steps"></a>Pasos siguientes

[Implementación de una oferta de seguridad de confianza en un centro de conectividad protegido mediante Azure Firewall Manager](deploy-trusted-security-partner.md).

---
title: Acerca de Azure Edge Zone (versión preliminar)
description: 'Más información sobre las ofertas de proceso perimetral de Microsoft: Azure Edge Zone.'
services: vnf-manager
author: ganesr
ms.service: vnf-manager
ms.topic: article
ms.date: 04/02/2020
ms.author: ganesr
ms.openlocfilehash: 90e796c244950d6d374a02757b608099c229c1ea
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/28/2020
ms.locfileid: "82146945"
---
# <a name="about-azure-edge-zone-preview"></a>Acerca de Azure Edge Zone (versión preliminar)

Azure Edge Zone es una familia de ofertas de Microsoft Azure que permite el procesamiento de datos cerca del usuario. Puede implementar máquinas virtuales, contenedores y otros servicios de Azure seleccionados en Azure Edge Zone para satisfacer los requisitos de baja latencia y alto rendimiento de las aplicaciones.

Los escenarios de casos de uso típicos de las zonas perimetrales incluyen:

- Comando y control en tiempo real en robótica.
- Análisis e inferencia en tiempo real mediante inteligencia artificial y aprendizaje automático.
- Visión por ordenador.
- Representación remota para escenarios de realidad mixta y VDI.
- Juegos de inmersión de varios jugadores.
- Streaming multimedia y entrega de contenido.
- Vigilancia y seguridad.

Hay tres tipos de instancias de Azure Edge Zone:

- Azure Edge Zones
- Azure Edge Zones with Carrier
- Azure Private Edge Zones

## <a name="azure-edge-zones"></a><a name="edge-zones"></a>Azure Edge Zones

![Instancias de Azure Edge Zone](./media/edge-zones-overview/edge-zones.png "Azure Edge Zones")

Las instancias de Azure Edge Zone son pequeñas extensiones de superficie de Azure ubicadas en centros de población que están lejos de las regiones de Azure. Azure Edge Zone admite VM, contenedores y un conjunto seleccionado de servicios de Azure que permite ejecutar aplicaciones sensibles a la latencia y de alto rendimiento cerca de los usuarios finales. Las instancias de Azure Edge Zone forman parte de la red global de Microsoft. Proporcionan conectividad segura, confiable y de alto ancho de banda entre las aplicaciones que se ejecutan en la instancia de Edge Zone próxima al usuario. Además, ofrecen el conjunto completo de servicios de Azure que se ejecutan en las regiones de Azure. Microsoft posee y opera las instancias de Azure Edge Zone. Puede usar el mismo conjunto de herramientas de Azure y el mismo portal para administrar e implementar servicios en instancias de Edge Zone.

Entre los casos de uso comunes se incluyen:

- Juegos y streaming de juegos.
- Streaming multimedia y entrega de contenido.
- Análisis e inferencia en tiempo real mediante inteligencia artificial y aprendizaje automático.
- Representación para realidad mixta.

Azure Edge Zone estará disponible en las siguientes áreas metropolitanas:

- Nueva York, NY
- Los Angeles, CA
- Miami, FL

[Póngase en contacto con el equipo de Edge Zone](https://aka.ms/EdgeZones) para más información.

## <a name="azure-edge-zones-with-carrier"></a><a name="carrier"></a>Azure Edge Zones with Carrier

![Edge Zones with Carrier](./media/edge-zones-overview/edge-carrier.png "Edge Zones with Carrier")

Las instancias de Azure Edge Zone with Carrier son pequeñas extensiones de superficie de Azure que están ubicadas en centros de datos de operadores móviles de centros de población. La infraestructura de Azure Edge Zone with Carrier se coloca ligeramente apartada de la red 5G del operador móvil. Esta selección de ubicación ofrece una latencia de menos de 10 milisegundos a las aplicaciones de dispositivos móviles.

Las instancias de Azure Edge Zone with Carrier se implementan en los centros de datos de los operadores móviles y están conectadas a la red global de Microsoft. Proporcionan conectividad segura, confiable y de alto ancho de banda entre las aplicaciones que se ejecutan cerca del usuario. Además, ofrecen el conjunto completo de servicios de Azure que se ejecutan en las regiones de Azure. Los desarrolladores pueden usar el mismo conjunto de herramientas conocidas para crear e implementar servicios en Edge Zones.

Entre los casos de uso comunes se incluyen:

- Juegos y streaming de juegos.
- Streaming multimedia y entrega de contenido.
- Análisis e inferencia en tiempo real mediante inteligencia artificial y aprendizaje automático.
- Representación para realidad mixta.
- Automóviles conectados.
- Telemedicina.

Edge Zones se ofrecerá en colaboración con los operadores siguientes:

- AT&T (Atlanta, Dallas y Los Ángeles)

## <a name="azure-private-edge-zones"></a><a name="private-edge-zones"></a>Azure Private Edge Zones

![Private Edge Zones](./media/edge-zones-overview/private-edge.png "Private Edge Zones")

Las instancias privadas de Azure Edge Zone son pequeñas extensiones de superficie de Azure que están ubicadas en el entorno local. Azure Private Edge Zone se basa en la plataforma de [Azure Stack Edge](https://azure.microsoft.com/products/azure-stack/edge/). Permite el acceso de baja latencia a los servicios informáticos y de almacenamiento implementados en el entorno local. Private Edge Zone también le permite implementar aplicaciones de ISV y funciones de red virtualizadas (VNFs) como [aplicaciones administradas de Azure](https://azure.microsoft.com/services/managed-applications/), junto con máquinas virtuales y contenedores en el entorno local. Estos VNF pueden incluir núcleos de paquetes móviles, enrutadores, firewalls y dispositivos SD-WAN. Azure Private Edge Zone incluye una solución de orquestación nativa en la nube que le permite administrar el ciclo de vida de las VNF y las aplicaciones desde Azure Portal.

Azure Private Edge Zone le permite desarrollar e implementar aplicaciones de forma local con las mismas herramientas conocidas utilizadas para crear e implementar aplicaciones en Azure. 

También le permite lo siguiente: 

- Ejecutar redes móviles privadas (LTE o 5G privadas).
- Implementar funciones de seguridad como firewalls.
- Ampliar sus redes locales en varias ramas y Azure mediante el uso de dispositivos SD-WAN en los mismos dispositivos de Private Edge Zone y administrarlas desde Azure.

Entre los casos de uso comunes se incluyen:

- Comando y control en tiempo real en robótica.
- Análisis e inferencia en tiempo real con inteligencia artificial y aprendizaje automático.
- Visión por ordenador.
- Representación remota para escenarios de realidad mixta y VDI.
- Vigilancia y seguridad.

Disponemos de un amplio ecosistema de proveedores de VNF, ISV y asociados de MSP para habilitar soluciones de un extremo a otro que usan instancias Private Edge Zone. [Póngase en contacto con el equipo de Private Edge Zone](https://aka.ms/EdgeZonesPartner) para obtener más información.

### <a name="private-edge-zone-partners"></a><a name="private-edge-partners"></a>Asociados de Private Edge Zone

![Asociados de Private Edge Zone](./media/edge-zones-overview/partners.png "Asociados de Private Edge Zone")

#### <a name="virtualized-network-functions-vnfs"></a><a name="vnf"></a>Funciones de red virtualizadas (VNF)

##### <a name="virtualized-evolved-packet-core-vepc-for-mobile-networks"></a><a name="vEPC"></a>Virtualized Evolved Packet Core (vEPC) para redes móviles

- [Affirmed Networks](https://www.affirmednetworks.com/)
- [Druid Software](https://www.druidsoftware.com/)
- [Expeto](https://www.expeto.io/)
- [Mavenir](https://mavenir.com/)
- [Metaswitch](https://www.metaswitch.com/)
- [Nokia Digital Automation Cloud](https://www.dac.nokia.com/)

##### <a name="mobile-radio-partners"></a><a name="mobile-radio"></a>Asociados de radio móvil

- [Commscope Ruckus](https://support.ruckuswireless.com/)

##### <a name="sd-wan-vendors"></a><a name="sdwan-vendors"></a>Proveedores de SD-WAN

- [NetFoundry](https://netfoundry.io/)
- [Nuage Networks de Nokia](https://www.nuagenetworks.net/)
- [VMware SD-WAN de Velocloud](https://www.velocloud.com/)

##### <a name="router-vendors"></a><a name="router-vendors"></a>Proveedores de routers

- [Arista](https://www.arista.com/)

##### <a name="firewall-vendors"></a><a name="firewall-vendors"></a>Proveedores de firewalls

- [Palo Alto Networks](https://www.paloaltonetworks.com/)

##### <a name="managed-solutions-providers-mobile-operators-and-global-system-integrators-gsis"></a><a name="msp-mobile"></a>Proveedores de soluciones administradas: Operadores móviles e integradores de sistemas globales (GSI)

| GSI y operadores | Operadores móviles |
| --- | --- |
| Amdocs                       | Etisalat             |
| American Tower               | NTT Communications   |
| CenturyLink                  | Proximus             |
| Expeto                       | Rogers               |
| Federated Wireless           | SK Telecom           |
| Infosys                      | Telefónica           |
| Tech Mahindra                | Telstra              |
|                              | Vodafone             |

[Póngase en contacto con el equipo de Private Edge Zone](https://aka.ms/EdgeZonesPartner) para más información sobre cómo convertirse en asociado.

### <a name="private-edge-zone-solutions"></a><a name="solutions-private-edge"></a>Soluciones de Private Edge Zone

#### <a name="private-mobile-network-on-private-edge-zones"></a><a name="private-mobile-private-edge"></a>Private Mobile Network en Private Edge Zone

![Private Mobile Network en Private Edge Zone](./media/edge-zones-overview/mobile-networks.png "Private Mobile Network en Private Edge Zone")

Ahora puede implementar Private Mobile Network en Private Edge Zone. Private Mobile Network tiene una latencia muy baja, gran capacidad y la red inalámbrica confiable y segura que es necesaria para las aplicaciones críticas de la empresa. 

Private Mobile Network puede habilitar escenarios como: 
- Comando y control de vehículos con guía automatizada (AGV) en almacenes. 
- Comunicación en tiempo real entre robots en fábricas inteligentes.
- Aplicaciones perimetrales de realidad aumentada y realidad virtual.

La función de red Virtualized Evolved Packet Core (vEPC) es el cerebro de Private Mobile Network. Ahora puede implementar un vEPC en Private Edge Zones. Para obtener una lista de los asociados de vEPC que están disponibles en Private Edge Zone, consulte [ISV de vEPC](#vEPC).

La implementación de una solución Private Mobile Network en Private Edge Zone requiere otros componentes; por ejemplo, puntos de acceso móvil, tarjetas SIM y otras VNF, como los enrutadores. El acceso a un espectro con licencia o sin licencia es fundamental para configurar una red móvil privada. Además, puede que necesite ayuda con la planeación de RF, el diseño físico, la instalación y el soporte técnico. Para obtener una lista de asociados, consulte [Asociados de radio móvil](#mobile-radio).

Microsoft proporciona un ecosistema de partners que puede ayudar en todos los aspectos de este proceso. Los asociados pueden ayudar en el planeamiento de la red, la compra de los dispositivos necesarios, la configuración del hardware y la administración de la configuración desde Azure. Con un conjunto de asociados validados que están estrechamente integrados con Microsoft, puede estar seguro de que la solución será confiable y fácil de usar. Puede centrarse en sus escenarios principales y confiar en Microsoft y sus asociados para que le ayuden con el resto.

#### <a name="sd-wan-on-private-edge-zones"></a><a name="sdwan-private-edge"></a>SD-WAN en Private Edge Zones

![SD-WAN en Private Edge Zones](./media/edge-zones-overview/sd-wan.png "SD-WAN en Private Edge Zones")
 
SD-WAN le permite crear redes de área extensa (WAN) de nivel empresarial que tienen estas ventajas:

- Mayor ancho de banda
- Acceso de alto rendimiento a la nube
- Inserción de servicio
- Confiabilidad
- Administración de directivas
- Amplia visibilidad de red
    
SD-WAN proporciona conectividad sin problemas para las sucursales organizada desde controladores centrales redundantes, con un costo menor de propiedad.
SD-WAN en Private Edge Zone permite pasar de un modelo centrado en gastos de capital a un modelo de software como servicio (SaaS) para reducir los presupuestos de TI. Puede usar la opción del orquestador o el controlador de asociados de SD-WAN para habilitar nuevos servicios y propagarlos a toda la red de forma inmediata.

## <a name="next-steps"></a>Pasos siguientes

Para más información, póngase en contacto con los siguientes equipos:

* [Equipo de Edge Zone](https://aka.ms/EdgeZones)
* [Equipo de Private Edge Zone, para convertirse en asociado](https://aka.ms/EdgeZonesPartner)

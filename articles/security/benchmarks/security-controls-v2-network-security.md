---
title: 'Azure Security Benchmark v2: seguridad de red'
description: Seguridad de red de Azure Security Benchmark v2
author: msmbaldwin
ms.service: security
ms.topic: conceptual
ms.date: 09/20/2020
ms.author: mbaldwin
ms.custom: security-benchmark
ms.openlocfilehash: b6aaad9cce330d755fb69f8be48737f9a8f6c4a7
ms.sourcegitcommit: d60976768dec91724d94430fb6fc9498fdc1db37
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/02/2020
ms.locfileid: "96487752"
---
# <a name="security-control-v2-network-security"></a>Control de seguridad V2: Seguridad de redes

La seguridad de red abarca los controles para proteger y asegurar las redes de Azure. Esto incluye la protección de redes virtuales, el establecimiento de conexiones privadas, la prevención y la mitigación de ataques externos, y la protección de DNS.

## <a name="ns-1-implement-security-for-internal-traffic"></a>NS-1: implementación de la seguridad para el tráfico interno

| Identificador de Azure | Identificadores de CIS Controls v7.1 | Identificadores de NIST SP800-53 r4 |
|--|--|--|--|
| NS-1 | 9.2, 9.4, 14.1, 14.2, 14.3 | AC-4, CA-3, SC-7 |

Asegúrese de que todas las redes virtuales de Azure siguen un principio de segmentación empresarial que se adapte a los riesgos empresariales. Todos los sistemas que podrían suponer un riesgo mayor para la organización deben aislarse en su propia red virtual y estar lo suficientemente protegidos con Azure Firewall o un grupo de seguridad de red (NSG). 

En función de las aplicaciones y la estrategia de segmentación empresarial, restrinja o permita el tráfico entre los recursos internos en función de las reglas del grupo de seguridad de red. En el caso de aplicaciones específicas bien definidas (como una aplicación de tres niveles), puede ser un enfoque de tipo "denegar de manera predeterminada, permitir por excepción" altamente seguro. Es posible que esta estrategia no escale bien si tiene muchas aplicaciones y puntos de conexión que interactúan entre sí. También puede usar Azure Firewall en circunstancias en las que se requiere la administración central a través de un gran número de segmentos o radios de empresa (en una topología tipo hub-and-spoke). 

Use la protección de red adaptable de Azure Security Center para recomendar configuraciones de grupo de seguridad de red que limiten los puertos y las direcciones IP en función de las reglas de tráfico externo.

Use Azure Sentinel para detectar el uso de protocolos heredados poco seguros como SSL/TLSv1, SMBv1, LM/NTLMv1, wDigest, enlaces LDAP sin firmar y códigos débiles de Kerberos.

- [Creación de un grupo de seguridad de red con reglas de seguridad](../../virtual-network/tutorial-filter-network-traffic.md)

- [Implementación y configuración de Azure Firewall](../../firewall/tutorial-firewall-deploy-portal.md)

- [Protección de red adaptable en Azure Security Center](../../security-center/security-center-adaptive-network-hardening.md)

- [Libro de protocolos poco seguros de Azure Sentinel](../../sentinel/quickstart-get-visibility.md#use-built-in-workbooks)

**Responsabilidad**: Customer

**Partes interesadas de seguridad del cliente** ([Más información](/azure/cloud-adoption-framework/organize/cloud-security#security-functions)):

- [Arquitectura de seguridad](/azure/cloud-adoption-framework/organize/cloud-security-architecture) 

- [Administración de la posición](/azure/cloud-adoption-framework/organize/cloud-security-posture-management)     

- [Seguridad de las aplicaciones y DevOps](/azure/cloud-adoption-framework/organize/cloud-security-application-security-devsecops)

## <a name="ns-2-connect-private-networks-together"></a>NS-2: Conexión conjunta de redes privadas

| Identificador de Azure | Identificadores de CIS Controls v7.1 | Identificadores de NIST SP800-53 r4 |
|--|--|--|--|
| NS-2 | N/D | CA-3, AC-17, MA-4 |

Use Azure ExpressRoute o la red privada virtual (VPN) de Azure para crear conexiones privadas entre centros de datos de Azure y una infraestructura local en un entorno de coubicación. Las conexiones ExpressRoute no usan la Internet pública y ofrecen más confiabilidad, velocidad, seguridad y una menor latencia que las conexiones a Internet habituales. Para las conexiones de punto a sitio y de sitio a sitio, puede conectar dispositivos o redes locales a una red virtual mediante cualquier combinación de estas opciones de VPN y Azure ExpressRoute.

Para conectar entre sí dos o más redes virtuales en Azure, use el emparejamiento de redes virtuales o Private Link. El tráfico entre redes virtuales emparejadas es privado y se mantiene en la red troncal de Azure. 

- [Qué son los modelos de conectividad de ExpressRoute](../../expressroute/expressroute-connectivity-models.md) 

- [Información general sobre la red privada virtual de Azure](../../vpn-gateway/vpn-gateway-about-vpngateways.md)

- [Interconexión de red virtual](../../virtual-network/virtual-network-peering-overview.md)

- [Azure Private Link](../../private-link/private-link-service-overview.md)

**Responsabilidad**: Customer

**Partes interesadas de seguridad del cliente** ([Más información](/azure/cloud-adoption-framework/organize/cloud-security#security-functions)):

- [Arquitectura de seguridad](/azure/cloud-adoption-framework/organize/cloud-security-architecture) 

- [Administración de la posición](/azure/cloud-adoption-framework/organize/cloud-security-posture-management)    

- [Seguridad de las aplicaciones y DevOps](/azure/cloud-adoption-framework/organize/cloud-security-application-security-devsecops) 

## <a name="ns-3-establish-private-network-access-to-azure-services"></a>NS-3: establecimiento del acceso de red privada a los servicios de Azure

| Identificador de Azure | Identificadores de CIS Controls v7.1 | Identificadores de NIST SP800-53 r4 |
|--|--|--|--|
| NS-3 | 14,1 | AC-4, CA-3, SC-7 |

Use Azure Private Link para habilitar el acceso privado a los servicios de Azure desde sus redes virtuales sin usar la Internet. En situaciones en las que todavía no está disponible Azure Private Link, use los puntos de conexión de servicio de red virtual de Azure.  Los puntos de conexión de servicio de red virtual de Azure proporcionan acceso seguro a los servicios a través de una ruta optimizada que usa la red troncal de Azure.  

El acceso privado es una medida de defensa exhaustiva adicional a la seguridad para la autenticación y el tráfico que ofrecen los servicios de Azure. 

- [¿Qué es Azure Private Link?](../../private-link/private-link-overview.md)

- [Introducción a los puntos de conexión de servicio de red virtual](../../virtual-network/virtual-network-service-endpoints-overview.md)

**Responsabilidad**: Customer

**Partes interesadas de seguridad del cliente** ([Más información](/azure/cloud-adoption-framework/organize/cloud-security#security-functions)):

- [Arquitectura de seguridad](/azure/cloud-adoption-framework/organize/cloud-security-architecture) 

- [Administración de la posición](/azure/cloud-adoption-framework/organize/cloud-security-posture-management)   

- [Seguridad de las aplicaciones y DevOps](/azure/cloud-adoption-framework/organize/cloud-security-application-security-devsecops) 

## <a name="ns-4-protect-applications-and-services-from-external-network-attacks"></a>NS-4: protección de las aplicaciones y servicios de ataques de redes externas

| Identificador de Azure | Identificadores de CIS Controls v7.1 | Identificadores de NIST SP800-53 r4 |
|--|--|--|--|
| NS-4 | 9.5, 12.3, 12.9 | SC-5, SC-7 |

Proteja los recursos de Azure de ataques de redes externas, incluidos los ataques de denegación de servicio distribuido (DDoS), los ataques específicos de la aplicación y el tráfico de Internet no solicitado y potencialmente malintencionado. Azure incluye funcionalidades nativas para estas acciones:
-   Use Azure Firewall para proteger las aplicaciones y los servicios contra el tráfico potencialmente malintencionado de Internet y otras ubicaciones externas. 

-   Use las funciones de Web Application Firewall (WAF) en Azure Application Gateway, Azure Front Door y Azure Content Delivery Network (CDN) para proteger sus aplicaciones, servicios y API frente a ataques del nivel de aplicación. 

-   Proteja sus recursos de ataques DDoS al habilitar la protección contra DDoS estándar en las redes virtuales de Azure. 
-   Use Azure Security Center para detectar posibles riesgos de configuración incorrecta relacionados con lo anterior. 

- [Documentación sobre Azure Firewall](../../firewall/index.yml)

- [Implementación de WAF de Azure](../../web-application-firewall/overview.md)

- [Administración de Azure DDoS Protection estándar mediante Azure Portal](../../ddos-protection/manage-ddos-protection.md)

**Responsabilidad**: Customer

**Partes interesadas de seguridad del cliente** ([Más información](/azure/cloud-adoption-framework/organize/cloud-security#security-functions)):

None

## <a name="ns-5-deploy-intrusion-detectionintrusion-prevention-systems-idsips"></a>NS-5: implementación de sistemas de prevención de intrusiones y detección de intrusiones (IDS/IPS)

| Identificador de Azure | Identificadores de CIS Controls v7.1 | Identificadores de NIST SP800-53 r4 |
|--|--|--|--|
| NS-5 | 12.6, 12.7 | SI-4 |

Use el filtrado basado en inteligencia sobre amenazas de Azure Firewall para alertar o bloquear el tráfico desde y hacia dominios y direcciones IP malintencionados conocidos. La direcciones IP y los dominios proceden de la fuente Inteligencia sobre amenazas de Microsoft. Cuando se requiera la inspección de las cargas, puede implementar un sistema de prevención de intrusiones y detección de intrusiones (IDS/IPS) desde Azure Marketplace con funcionalidades de inspección de cargas. También puede usar IDS/IPS basado en host o una solución de detección y respuesta de punto de conexión (EDR) basada en host junto un IDS/IPS basado en redes, o en lugar de este.  

Nota: Si tiene un requisito normativo o de otro tipo para el uso de IDS/IPS, asegúrese de que siempre esté optimizado para proporcionar alertas de alta calidad a la solución de SIEM. 

- [Implementación de Azure Firewall](../../firewall/tutorial-firewall-deploy-portal.md)

- [Azure Marketplace incluye funcionalidades de IDS de terceros](https://azuremarketplace.microsoft.com/marketplace?search=IDS)

- [Funcionalidad de detección y respuesta de la protección contra amenazas avanzada de Microsoft Defender](/windows/security/threat-protection/microsoft-defender-atp/overview-endpoint-detection-response)

**Responsabilidad**: Customer

**Partes interesadas de seguridad del cliente** ([Más información](/azure/cloud-adoption-framework/organize/cloud-security#security-functions)):

- [Arquitectura de seguridad](/azure/cloud-adoption-framework/organize/cloud-security-architecture) 

- [Administración de la posición](/azure/cloud-adoption-framework/organize/cloud-security-posture-management)   

- [Seguridad de las aplicaciones y DevOps](/azure/cloud-adoption-framework/organize/cloud-security-application-security-devsecops) 

## <a name="ns-6-simplify-network-security-rules"></a>NS-6: simplificación de las reglas de seguridad de red

| Identificador de Azure | Identificadores de CIS Controls v7.1 | Identificadores de NIST SP800-53 r4 |
|--|--|--|--|
| NS-6 | 1.5 | IA-4 |

Simplifique las reglas de seguridad de red al aprovechar las etiquetas de servicio y los grupos de seguridad de aplicaciones (ASG). 

Puede usar etiquetas de servicio de red virtual para definir controles de acceso a la red en los grupos de seguridad de red o Azure Firewall. Puede utilizar etiquetas de servicio en lugar de direcciones IP específicas al crear reglas de seguridad. Al especificar el nombre de la etiqueta de servicio en el campo de origen o destino de una regla, puede permitir o denegar el tráfico para el servicio correspondiente. Microsoft administra los prefijos de direcciones que la etiqueta de servicio incluye y actualiza automáticamente dicha etiqueta a medida que las direcciones cambian.

También puede usar grupos de seguridad de aplicaciones para simplificar una configuración de seguridad compleja. En lugar de definir directivas en función de las direcciones IP explícitas de los grupos de seguridad, los grupos de seguridad de aplicaciones le permiten configurar la seguridad de red como una extensión natural de la estructura de una aplicación, lo que le permite agrupar máquinas virtuales y directivas de seguridad de red basadas en esos grupos.

- [Descripción y uso de las etiquetas de servicio](../../virtual-network/service-tags-overview.md)

- [Descripción y uso de los grupos de seguridad de aplicaciones](../../virtual-network/network-security-groups-overview.md#application-security-groups)

**Responsabilidad**: Customer

**Partes interesadas de seguridad del cliente** ([Más información](/azure/cloud-adoption-framework/organize/cloud-security#security-functions)):

- [Arquitectura de seguridad](/azure/cloud-adoption-framework/organize/cloud-security-architecture) 

- [Administración de la posición](/azure/cloud-adoption-framework/organize/cloud-security-posture-management)    

- [Seguridad de las aplicaciones y DevOps](/azure/cloud-adoption-framework/organize/cloud-security-application-security-devsecops)

## <a name="ns-7-secure-domain-name-service-dns"></a>NS-7: servicio de nombres de dominio (DNS) seguro

| Identificador de Azure | Identificadores de CIS Controls v7.1 | Identificadores de NIST SP800-53 r4 |
|--|--|--|--|
| NS-7 | N/D | SC-20, SC-21 |

Siga los procedimientos recomendados de la seguridad de DNS con el fin de mitigar los ataques comunes, como el DNS pendiente, los ataques de amplificación de DNS, "poisoning" y suplantación de DNS, etc. 

Cuando se usa Azure DNS como servicio DNS autoritativo, asegúrese de que los registros y las zonas DNS están protegidos contra modificaciones accidentales o malintencionadas mediante Azure RBAC y bloqueos de recursos. 

- [Introducción a Azure DNS](../../dns/dns-overview.md)

- [Guía de implementación del sistema de nombres de dominio (DNS) seguro](https://csrc.nist.gov/publications/detail/sp/800-81/2/final)

- [Evitar las entradas DNS pendientes y la adquisición de subdominios](../fundamentals/subdomain-takeover.md)

**Responsabilidad**: Customer

**Partes interesadas de seguridad del cliente** ([Más información](/azure/cloud-adoption-framework/organize/cloud-security#security-functions)):

- [Arquitectura de seguridad](/azure/cloud-adoption-framework/organize/cloud-security-architecture) 

- [Administración de la posición](/azure/cloud-adoption-framework/organize/cloud-security-posture-management)   

- [Seguridad de las aplicaciones y DevOps](/azure/cloud-adoption-framework/organize/cloud-security-application-security-devsecops)
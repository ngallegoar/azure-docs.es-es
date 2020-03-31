---
title: 'Control de seguridad de Azure: seguridad de redes'
description: Seguridad de redes del control de seguridad
author: msmbaldwin
manager: rkarlin
ms.service: security
ms.topic: conceptual
ms.date: 12/30/2019
ms.author: mbaldwin
ms.custom: security-recommendations
ms.openlocfilehash: 7916bbb28602d64e0916fce7badf16a65c242227
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "77251879"
---
# <a name="security-control-network-security"></a>Control de seguridad: Seguridad de redes

Las recomendaciones de seguridad de redes se centran en especificar a qué protocolos de red, puertos TCP/UDP y servicios conectados a la red se les permite o deniega el acceso a los servicios de Azure.

## <a name="11-protect-resources-using-network-security-groups-or-azure-firewall-on-your-virtual-network"></a>1.1: Proteja los recursos mediante grupos de seguridad de red o Azure Firewall en su red virtual

| Id. de Azure | Id. de CIS | Responsabilidad |
|--|--|--|
| 1.1 | 9.2, 9.4, 14.1-14.3 | Customer |

Asegúrese de que todas las implementaciones de subred de Virtual Network tienen un grupo de seguridad de red aplicado con controles de acceso a la red específicos para los puertos y orígenes de confianza de su aplicación. Use los servicios de Azure con Private Link habilitado, implemente el servicio dentro de la red virtual o conéctese de forma privada con puntos de conexión privados. Para conocer los requisitos específicos de un servicio, consulte la recomendación de seguridad para ese servicio concreto.

Como alternativa, si tiene un caso de uso específico, se pueden cumplir los requisitos implementando Azure Firewall.

Información general sobre Private Link:

https://docs.microsoft.com/azure/private-link/private-link-overview

Cómo crear una red virtual:

https://docs.microsoft.com/azure/virtual-network/quick-create-portal

Cómo crear un NSG con una configuración de seguridad:

https://docs.microsoft.com/azure/virtual-network/tutorial-filter-network-traffic

Cómo implementar y configurar Azure Firewall:

https://docs.microsoft.com/azure/firewall/tutorial-firewall-deploy-portal

## <a name="12-monitor-and-log-the-configuration-and-traffic-of-vnets-subnets-and-nics"></a>1.2: Supervise y registre la configuración y el tráfico de redes virtuales, subredes y NIC

| Id. de Azure | Id. de CIS | Responsabilidad |
|--|--|--|
| 1.2 | 9.3, 12.2 | Customer |

Use Azure Security Center y siga las recomendaciones de protección de redes para ayudar a proteger sus recursos de red en Azure. Habilite los registros de flujo de NSG y envíe registros a una cuenta de almacenamiento para la auditoría del tráfico.

Cómo habilitar los registros de flujo de NSG:

https://docs.microsoft.com/azure/network-watcher/network-watcher-nsg-flow-logging-portal

Descripción de la seguridad de red proporcionada por Azure Security Center:

https://docs.microsoft.com/azure/security-center/security-center-network-recommendations

## <a name="13-protect-critical-web-applications"></a>1.3: Proteja las aplicaciones web críticas

| Id. de Azure | Id. de CIS | Responsabilidad |
|--|--|--|
| 1.3 | 9.5 | Customer |

Implemente el firewall de aplicaciones web (WAF) de Azure delante de las aplicaciones web críticas para la inspección adicional del tráfico entrante. Habilite la configuración de diagnóstico para WAF e ingiera los registros en una cuenta de almacenamiento, un centro de eventos o un área de trabajo de análisis de registros.

Cómo implementar WAF de Azure:

https://docs.microsoft.com/azure/web-application-firewall/ag/create-waf-policy-ag

## <a name="14-deny-communications-with-known-malicious-ip-addresses"></a>1.4: Deniegue las comunicaciones con direcciones IP malintencionadas conocidas

| Id. de Azure | Id. de CIS | Responsabilidad |
|--|--|--|
| 1.4 | 12.3 | Customer |

Habilite la protección contra DDoS estándar en las redes virtuales de Azure para protegerse frente a ataques DDoS. Use la inteligencia sobre amenazas integrada en Azure Security Center para denegar las comunicaciones con direcciones IP malintencionadas conocidas.

Implemente Azure Firewall en cada uno de los límites de red de la organización con la inteligencia de amenazas habilitada y configurada para &quot;alertar y denegar&quot; para el tráfico de red malintencionado.

Use el acceso de red Just-in-Time de Azure Security Center para configurar los NSG para limitar la exposición de los puntos de conexión a las direcciones IP aprobadas durante un período limitado.

Use la protección de redes adaptativa de Azure Security Center para recomendar configuraciones de NSG que limiten los puertos y las direcciones IP de origen según el tráfico real y la inteligencia sobre amenazas.

Cómo configurar la protección contra DDoS:

https://docs.microsoft.com/azure/virtual-network/manage-ddos-protection

Cómo implementar Azure Firewall:

https://docs.microsoft.com/azure/firewall/tutorial-firewall-deploy-portal

Descripción del la inteligencia sobre amenazas integrada de Azure Security Center:

https://docs.microsoft.com/azure/security-center/security-center-alerts-service-layer

Descripción de la protección de red adaptable de Azure Security Center:

https://docs.microsoft.com/azure/security-center/security-center-adaptive-network-hardening

Descripción del control de acceso de red Just-in-Time de Azure Security Center:

https://docs.microsoft.com/azure/security-center/security-center-just-in-time

## <a name="15-record-network-packets-and-flow-logs"></a>1.5: Registre los paquetes de red y registros de flujo

| Id. de Azure | Id. de CIS | Responsabilidad |
|--|--|--|
| 1.5 | 12.5, 15.8 | Customer |

Registre los registros de flujo de NSG en una cuenta de almacenamiento para generar registros de flujo. Si es necesario para investigar actividades anómalas, habilite la captura de paquetes de Network Watcher.

Cómo habilitar los registros de flujo de NSG:

https://docs.microsoft.com/azure/network-watcher/network-watcher-nsg-flow-logging-portal

Cómo habilitar Network Watcher:

https://docs.microsoft.com/azure/network-watcher/network-watcher-create

## <a name="16-deploy-network-based-intrusion-detectionintrusion-prevention-systems-idsips"></a>1.6: Implemente sistemas de prevención de intrusiones y detección de intrusiones (IDS/IPS) basados en la red

| Id. de Azure | Id. de CIS | Responsabilidad |
|--|--|--|
| 1.6 | 12.6, 12.7 | Customer |

Implemente Azure Firewall en cada uno de los límites de red de la organización con la inteligencia de amenazas habilitada y configurada para &quot;alertar y denegar&quot; para el tráfico de red malintencionado.

Cómo implementar Azure Firewall: https://docs.microsoft.com/azure/firewall/tutorial-firewall-deploy-portal

Cómo configurar las alertas con Azure Firewall: https://docs.microsoft.com/azure/firewall/threat-intel

## <a name="17-manage-traffic-to-web-applications"></a>1.7: Administre el tráfico a las aplicaciones web

| Id. de Azure | Id. de CIS | Responsabilidad |
|--|--|--|
| 1.7 | 12.9, 12.10 | Customer |

Implemente Application Gateway de Azure para las aplicaciones web con HTTPS/SSL habilitado para los certificados de confianza.

Cómo implementar Application Gateway:

https://docs.microsoft.com/azure/application-gateway/quick-create-portal

Cómo configurar Application Gateway para usar HTTPS:

https://docs.microsoft.com/azure/application-gateway/create-ssl-portal

Descripción del equilibrio de carga de capa 7 con las puertas de enlace de aplicaciones web de Azure:

https://docs.microsoft.com/azure/application-gateway/overview

## <a name="18-minimize-complexity-and-administrative-overhead-of-network-security-rules"></a>1.8: Minimice la complejidad y la sobrecarga administrativa de las reglas de seguridad de red

| Id. de Azure | Id. de CIS | Responsabilidad |
|--|--|--|
| 1.8 | 1.5 | Customer |

Puede usar etiquetas de servicio de Virtual Network para definir controles de acceso a la red en grupos de seguridad de red o Azure Firewall. Puede utilizar etiquetas de servicio en lugar de direcciones IP específicas al crear reglas de seguridad. Al especificar el nombre de la etiqueta de servicio (por ejemplo, ApiManagement) en el campo de origen o destino apropiado de una regla, puede permitir o denegar el tráfico para el servicio correspondiente. Microsoft administra los prefijos de direcciones que la etiqueta de servicio incluye y actualiza automáticamente dicha etiqueta a medida que las direcciones cambian.

Descripción y uso de las etiquetas de servicio:

https://docs.microsoft.com/azure/virtual-network/service-tags-overview

## <a name="19-maintain-standard-security-configurations-for-network-devices"></a>1.9: Mantenga las configuraciones de seguridad estándar para dispositivos de red

| Id. de Azure | Id. de CIS | Responsabilidad |
|--|--|--|
| 1.9 | 11,1 | Customer |

Defina e implemente configuraciones de seguridad estándar para los recursos de red con Azure Policy.

También puede usar Azure Blueprints para simplificar las implementaciones de Azure a gran escala mediante el empaquetado de artefactos de entorno clave, como plantillas de Azure Resource Manager, controles de RBAC y directivas, en una única definición de plano técnico. Puede aplicar el plano técnico a nuevas suscripciones y ajustar el control y la administración a través del control de versiones.

Configuración y administración de Azure Policy:

https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage

Ejemplos de Azure Policy para redes:

https://docs.microsoft.com/azure/governance/policy/samples/built-in-policies#network

Cómo crear un plano técnico de Azure:

https://docs.microsoft.com/azure/governance/blueprints/create-blueprint-portal

## <a name="110-document-traffic-configuration-rules"></a>1.10: Documente las reglas de configuración de tráfico

| Id. de Azure | Id. de CIS | Responsabilidad |
|--|--|--|
| 1.1 | 11.2 | Customer |

Use etiquetas para NSG y otros recursos relacionados con la seguridad de red y el flujo de tráfico. En el caso de las reglas de NSG individuales, use el campo &quot;Descripción&quot; para especificar las necesidades y/o la duración de la empresa (etc.) para todas las reglas que permiten el tráfico hacia y desde una red.

Cómo crear y usar etiquetas:

https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags

Cómo crear una red virtual:

https://docs.microsoft.com/azure/virtual-network/quick-create-portal

Cómo crear un NSG con una configuración de seguridad:

https://docs.microsoft.com/azure/virtual-network/tutorial-filter-network-traffic

## <a name="111-use-automated-tools-to-monitor-network-resource-configurations-and-detect-changes"></a>1.11: Use herramientas automatizadas para supervisar las configuraciones de recursos de red y detectar cambios

| Id. de Azure | Id. de CIS | Responsabilidad |
|--|--|--|
| 1.11 | 11.3 | Customer |

Use Azure Policy para validar (y/o corregir) la configuración de los recursos de red.

Cómo configurar y administrar Azure Policy:

https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage

Ejemplos de Azure Policy para redes:

https://docs.microsoft.com/azure/governance/policy/samples/built-in-policies#network

## <a name="next-steps"></a>Pasos siguientes

- Vea el siguiente control de seguridad: [Registro y supervisión](security-control-logging-monitoring.md)

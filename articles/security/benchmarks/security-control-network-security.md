---
title: 'Control de seguridad de Azure: seguridad de redes'
description: Seguridad de redes del control de seguridad de Azure
author: msmbaldwin
ms.service: security
ms.topic: conceptual
ms.date: 04/14/2020
ms.author: mbaldwin
ms.custom: security-benchmark
ms.openlocfilehash: dad01212be3589af7167082ff22c624fa776772a
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "82193129"
---
# <a name="security-control-network-security"></a>Control de seguridad: Seguridad de redes

Las recomendaciones de seguridad de redes se centran en especificar a qué protocolos de red, puertos TCP/UDP y servicios conectados a la red se les permite o deniega el acceso a los servicios de Azure.

## <a name="11-protect-azure-resources-within-virtual-networks"></a>1.1: Protección de los recursos de Azure dentro de las redes virtuales

| Id. de Azure | Id. de CIS | Responsabilidad |
|--|--|--|
| 1.1 | 9.2, 9.4, 14.1, 14.2, 14.3 | Customer |

Asegúrese de que todas las implementaciones de subred de Virtual Network tienen un grupo de seguridad de red aplicado con controles de acceso a la red específicos para los puertos y orígenes de confianza de su aplicación. Cuando estén disponibles, use los puntos de conexión privados con Private Link para proteger los recursos de servicio de Azure en la red virtual mediante la extensión de la identidad de la red virtual al servicio. Cuando los puntos de conexión privados y Private Link no estén disponibles, utilice los puntos de conexión de servicio. Para conocer los requisitos específicos de un servicio, consulte la recomendación de seguridad para ese servicio concreto. 

Como alternativa, si tiene un caso de uso específico, se puede cumplir el requisito si implementa Azure Firewall.

- [Introducción a los puntos de conexión de servicio de red virtual](https://docs.microsoft.com/azure/virtual-network/virtual-network-service-endpoints-overview)

- [¿Qué es Azure Private Link?](https://docs.microsoft.com/azure/private-link/private-link-overview)

- [Creación de una red virtual](https://docs.microsoft.com/azure/virtual-network/quick-create-portal)

- [Creación de un NSG con una configuración de seguridad](https://docs.microsoft.com/azure/virtual-network/tutorial-filter-network-traffic)

- [Implementación y configuración de Azure Firewall](https://docs.microsoft.com/azure/firewall/tutorial-firewall-deploy-portal)

## <a name="12-monitor-and-log-the-configuration-and-traffic-of-virtual-networks-subnets-and-nics"></a>1.2: Supervisión y registro de la configuración y el tráfico de redes virtuales, subredes y NIC

| Id. de Azure | Id. de CIS | Responsabilidad |
|--|--|--|
| 1.2 | 9.3, 12.2, 12.8 | Customer |

Use Azure Security Center y siga las recomendaciones de protección de redes para ayudar a proteger sus recursos de red en Azure. Habilite los registros de flujo de NSG y envíe registros a una cuenta de almacenamiento para la auditoría del tráfico. También puede enviar registros de flujo de grupo de seguridad de red a un área de trabajo de Log Analytics y usar el Análisis de tráfico para proporcionar información detallada sobre el flujo de tráfico en la nube de Azure. Algunas de las ventajas del Análisis de tráfico son la capacidad de visualizar la actividad de la red e identificar las zonas activas, identificar las amenazas de seguridad, comprender los patrones de flujo de tráfico y detectar configuraciones de red incorrectas.

- [Habilitación de los registros de flujo de NSG](https://docs.microsoft.com/azure/network-watcher/network-watcher-nsg-flow-logging-portal)

- [Habilitación y uso del Análisis de tráfico](https://docs.microsoft.com/azure/network-watcher/traffic-analytics)

- [Descripción de la seguridad de red proporcionada por Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-network-recommendations)

## <a name="13-protect-critical-web-applications"></a>1.3: Proteja las aplicaciones web críticas

| Id. de Azure | Id. de CIS | Responsabilidad |
|--|--|--|
| 1.3 | 9.5 | Customer |

Implemente el firewall de aplicaciones web (WAF) de Azure delante de las aplicaciones web críticas para la inspección adicional del tráfico entrante. Habilite la configuración de diagnóstico para WAF e ingiera los registros en una cuenta de almacenamiento, un centro de eventos o un área de trabajo de análisis de registros.

- [Implementación de WAF de Azure](https://docs.microsoft.com/azure/web-application-firewall/ag/create-waf-policy-ag)

## <a name="14-deny-communications-with-known-malicious-ip-addresses"></a>1.4: Deniegue las comunicaciones con direcciones IP malintencionadas conocidas

| Id. de Azure | Id. de CIS | Responsabilidad |
|--|--|--|
| 1.4 | 12.3 | Customer |

Habilite la protección contra DDoS estándar en las redes virtuales de Azure para protegerse frente a ataques DDoS. Use la inteligencia sobre amenazas integrada en Azure Security Center para denegar las comunicaciones con direcciones IP malintencionadas conocidas.

Implemente Azure Firewall en cada uno de los límites de red de la organización con la inteligencia de amenazas habilitada y configurada para alertar y denegar el tráfico de red malintencionado.

Use el acceso de red Just-in-Time de Azure Security Center para configurar los NSG para limitar la exposición de los puntos de conexión a las direcciones IP aprobadas durante un período limitado.

Use la protección de redes adaptativa de Azure Security Center para recomendar configuraciones de NSG que limiten los puertos y las direcciones IP de origen según el tráfico real y la inteligencia sobre amenazas.

- [Configuración de la protección contra DDoS](https://docs.microsoft.com/azure/virtual-network/manage-ddos-protection)

- [Implementación de Azure Firewall](https://docs.microsoft.com/azure/firewall/tutorial-firewall-deploy-portal)

- [Descripción de la inteligencia sobre amenazas integrada de Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-alerts-service-layer)

- [Descripción de la protección de red adaptable de Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-adaptive-network-hardening)

- [Descripción del control de acceso de red Just-in-Time de Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-just-in-time)

## <a name="15-record-network-packets"></a>1.5: Registro de los paquetes de red

| Id. de Azure | Id. de CIS | Responsabilidad |
|--|--|--|
| 1.5 | 12.5 | Customer |

Habilite la captura de paquetes de Network Watcher para investigar las actividades anómalas.

- [Habilitación de Network Watcher](https://docs.microsoft.com/azure/network-watcher/network-watcher-create)

## <a name="16-deploy-network-based-intrusion-detectionintrusion-prevention-systems-idsips"></a>1.6: Implemente sistemas de prevención de intrusiones y detección de intrusiones (IDS/IPS) basados en la red

| Id. de Azure | Id. de CIS | Responsabilidad |
|--|--|--|
| 1.6 | 12.6, 12.7 | Customer |

Seleccione una oferta de Azure Marketplace que admita la funcionalidad de IDS/IPS con funcionalidades de inspección de carga.  Si la detección y/o la prevención de intrusiones basadas en la inspección de carga no es un requisito, se puede usar Azure Firewall con la inteligencia sobre amenazas. El filtrado basado en inteligencia sobre amenazas de Azure Firewall puede alertar y denegar el tráfico desde y hacia los dominios y las direcciones IP malintencionados conocidos. La direcciones IP y los dominios proceden de la fuente Inteligencia sobre amenazas de Microsoft.

Implemente la solución de firewall que prefiera en cada uno de los límites de red de su organización para detectar y/o denegar el tráfico malintencionado.

- [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/?term=Firewall)

- [Implementación de Azure Firewall](https://docs.microsoft.com/azure/firewall/tutorial-firewall-deploy-portal)

- [Configuración de alertas con Azure Firewall](https://docs.microsoft.com/azure/firewall/threat-intel)

## <a name="17-manage-traffic-to-web-applications"></a>1.7: Administre el tráfico a las aplicaciones web

| Id. de Azure | Id. de CIS | Responsabilidad |
|--|--|--|
| 1.7 | 12.9, 12.10 | Customer |

Implemente Application Gateway de Azure para las aplicaciones web con HTTPS/TLS habilitado para los certificados de confianza.

- [Implementación de Application Gateway](https://docs.microsoft.com/azure/application-gateway/quick-create-portal)

- [Configuración de Application Gateway para usar HTTPS](https://docs.microsoft.com/azure/application-gateway/create-ssl-portal)

- [Descripción del equilibrio de carga de capa 7 con las puertas de enlace de aplicaciones web de Azure](https://docs.microsoft.com/azure/application-gateway/overview)

## <a name="18-minimize-complexity-and-administrative-overhead-of-network-security-rules"></a>1.8: Minimice la complejidad y la sobrecarga administrativa de las reglas de seguridad de red

| Id. de Azure | Id. de CIS | Responsabilidad |
|--|--|--|
| 1.8 | 1.5 | Customer |

Puede usar etiquetas de servicio de Virtual Network para definir controles de acceso a la red en grupos de seguridad de red o Azure Firewall. Puede utilizar etiquetas de servicio en lugar de direcciones IP específicas al crear reglas de seguridad. Al especificar el nombre de la etiqueta de servicio (por ejemplo, ApiManagement) en el campo de origen o destino apropiado de una regla, puede permitir o denegar el tráfico para el servicio correspondiente. Microsoft administra los prefijos de direcciones que la etiqueta de servicio incluye y actualiza automáticamente dicha etiqueta a medida que las direcciones cambian.

También puede usar grupos de seguridad de aplicaciones para simplificar una configuración de seguridad compleja. Los grupos de seguridad de aplicaciones le permiten configurar la seguridad de red como una extensión natural de la estructura de una aplicación, lo que le permite agrupar máquinas virtuales y directivas de seguridad de red basadas en esos grupos.

- [Descripción y uso de las etiquetas de servicio](https://docs.microsoft.com/azure/virtual-network/service-tags-overview)

- [Descripción y uso de los grupos de seguridad de aplicaciones](https://docs.microsoft.com/azure/virtual-network/security-overview#application-security-groups)

## <a name="19-maintain-standard-security-configurations-for-network-devices"></a>1.9: Mantenga las configuraciones de seguridad estándar para dispositivos de red

| Id. de Azure | Id. de CIS | Responsabilidad |
|--|--|--|
| 1.9 | 11,1 | Customer |

Defina e implemente configuraciones de seguridad estándar para los recursos de red con Azure Policy.

También puede usar Azure Blueprints para simplificar las implementaciones de Azure a gran escala mediante el empaquetado de artefactos de entorno clave, como plantillas de Azure Resource Manager, controles de RBAC y directivas, en una única definición de plano técnico. Puede aplicar el plano técnico a nuevas suscripciones y ajustar el control y la administración a través del control de versiones.

- [Configuración y administración de Azure Policy](https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage)

- [Ejemplos de Azure Policy para redes](https://docs.microsoft.com/azure/governance/policy/samples/#network)

- [Creación de un plano técnico de Azure](https://docs.microsoft.com/azure/governance/blueprints/create-blueprint-portal)

## <a name="110-document-traffic-configuration-rules"></a>1.10: Documente las reglas de configuración de tráfico

| Id. de Azure | Id. de CIS | Responsabilidad |
|--|--|--|
| 1.10 | 11.2 | Customer |

Use etiquetas para NSG y otros recursos relacionados con la seguridad de red y el flujo de tráfico. En el caso de las reglas de NSG individuales, use el campo "Descripción" para especificar las necesidades empresariales o la duración (etc.) de las reglas que permiten que entre o salga el tráfico en una red.

Use cualquiera de las definiciones de Azure Policy integradas relacionadas con el etiquetado, como "Requerir etiqueta y su valor", para asegurarse de que todos los recursos se creen con etiquetas y para notificarle los recursos no etiquetados existentes.

Puede usar Azure PowerShell o la CLI de Azure para buscar o realizar acciones en los recursos en función de sus etiquetas.

- [Creación y uso de etiquetas](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags)

- [Creación de una red virtual](https://docs.microsoft.com/azure/virtual-network/quick-create-portal)

- [Creación de un grupo de seguridad de red con una configuración de seguridad](https://docs.microsoft.com/azure/virtual-network/tutorial-filter-network-traffic)

## <a name="111-use-automated-tools-to-monitor-network-resource-configurations-and-detect-changes"></a>1.11: Use herramientas automatizadas para supervisar las configuraciones de recursos de red y detectar cambios

| Id. de Azure | Id. de CIS | Responsabilidad |
|--|--|--|
| 1.11 | 11.3 | Customer |

Use el registro de actividad de Azure para supervisar las configuraciones de recursos y detectar cambios en los recursos de Azure. Cree alertas en Azure Monitor que se desencadenarán cuando se produzcan cambios en los recursos críticos.

- [Visualización y recuperación de eventos del registro de actividad de Azure](https://docs.microsoft.com/azure/azure-monitor/platform/activity-log-view)

- [Creación de alertas en Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-activity-log)

## <a name="next-steps"></a>Pasos siguientes

- Consulte el control de seguridad siguiente: [Registro y supervisión](security-control-logging-monitoring.md)

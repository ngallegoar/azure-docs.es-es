---
title: Características de Azure Firewall
description: Conozca las características de Azure Firewall
services: firewall
author: vhorne
ms.service: firewall
ms.topic: conceptual
ms.date: 10/08/2020
ms.author: victorh
ms.openlocfilehash: 7429be4430b2b520fb2a66b6b2c0dd138af8e501
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "91850598"
---
# <a name="azure-firewall-features"></a>Características de Azure Firewall

[Azure Firewall](overview.md) es un servicio de seguridad de red administrado y basado en la nube que protege los recursos de red virtual de Azure.

![Información general de firewalls](media/overview/firewall-threat.png)

Azure Firewall incluye las siguientes características:

- [Alta disponibilidad integrada](#built-in-high-availability)
- [Zonas de disponibilidad](#availability-zones)
- [Escalabilidad a la nube sin restricciones](#unrestricted-cloud-scalability)
- [Reglas de filtrado de FQDN de aplicación](#application-fqdn-filtering-rules)
- [Reglas de filtrado de tráfico de red](#network-traffic-filtering-rules)
- [Etiquetas de nombre de dominio completo](#fqdn-tags)
- [Etiquetas de servicio](#service-tags)
- [Información sobre amenazas](#threat-intelligence)
- [Compatibilidad con SNAT saliente](#outbound-snat-support)
- [Compatibilidad con DNAT entrante](#inbound-dnat-support)
- [Varias direcciones IP públicas](#multiple-public-ip-addresses)
- [Registro de Azure Monitor](#azure-monitor-logging)
- [Tunelización forzada](#forced-tunneling)
- [Certificaciones](#certifications)

## <a name="built-in-high-availability"></a>Alta disponibilidad integrada

Gracias a la alta disponibilidad integrada, no se necesita ningún equilibrador de carga adicional y no es necesario configurar nada.

## <a name="availability-zones"></a>Zonas de disponibilidad

Azure Firewall se puede configurar durante la implementación para abarcar varias zonas de disponibilidad y aumentar la disponibilidad. Con Availability Zones, la disponibilidad aumenta a un tiempo de actividad del 99,99 %. Para más información, consulte el [Acuerdo de Nivel de Servicio (SLA)](https://azure.microsoft.com/support/legal/sla/azure-firewall/v1_0/) de Azure Firewall. El SLA de tiempo de actividad del 99,99 % se ofrece cuando se seleccionan dos o más zonas de Availability Zones.

También puede asociar Azure Firewall a una zona específica solo por motivos de proximidad, mediante el SLA del 99,95 % estándar del servicio.

No hay ningún costo adicional por el firewall implementado en una zona de disponibilidad. Sin embargo, hay costos adicionales para las transferencias de datos entrantes y salientes asociadas con Availability Zones. Para más información, consulte [Detalles de precios de ancho de banda](https://azure.microsoft.com/pricing/details/bandwidth/).

Availability Zones de Azure Firewall está disponible en las regiones que lo admiten. Para más información, consulte [Regiones que admiten Availability Zones en Azure](../availability-zones/az-region.md).

> [!NOTE]
> Availability Zones solo se puede configurar durante la implementación. No se puede configurar un firewall existente para incluirlo.

Para más información sobre Availability Zones, consulte [Regiones y Availability Zones en Azure](../availability-zones/az-overview.md)

## <a name="unrestricted-cloud-scalability"></a>Escalabilidad a la nube sin restricciones

Azure Firewall puede escalarse verticalmente todo lo que sea necesario para acoger los flujos de tráfico de red cambiantes, por lo que no es necesario elaborar un presupuesto para el tráfico en su momento álgido.

## <a name="application-fqdn-filtering-rules"></a>Reglas de filtrado de FQDN de aplicación

Puede limitar el tráfico HTTP/S o el tráfico de Azure SQL saliente a una lista especificada de nombres de dominio completos (FQDN) que incluye caracteres comodín. Esta característica no requiere terminación de TLS.

## <a name="network-traffic-filtering-rules"></a>Reglas de filtrado de tráfico de red

Puede crear reglas de filtrado de red para *permitir* o *denegar* por dirección IP de origen y destino, puerto y protocolo. Azure Firewall tiene estado completo, de modo que puede distinguir los paquetes legítimos de diferentes tipos de conexiones. Las reglas se aplican y se registran en varias suscripciones y redes virtuales.

## <a name="fqdn-tags"></a>Etiquetas FQDN

Con las [etiquetas FQDN](fqdn-tags.md), puede permitir fácilmente que el tráfico de red conocido del servicio de Azure atraviese el firewall. Por ejemplo, supongamos que quiere permitir el tráfico de red de Windows Update a través del firewall. Puede crear una regla de aplicación e incluir la etiqueta de Windows Update. Ahora, el tráfico de red de Windows Update puede fluir a través del firewall.

## <a name="service-tags"></a>Etiquetas de servicio

Una [etiqueta de servicio](service-tags.md) representa un grupo de prefijos de direcciones IP que ayudan a reducir la complejidad de la creación de reglas de seguridad. No puede crear su propia etiqueta de servicio ni especificar qué direcciones IP se incluyen dentro de una etiqueta. Microsoft administra los prefijos de direcciones que incluye la etiqueta de servicio y actualiza automáticamente esta a medida que las direcciones cambian.

## <a name="threat-intelligence"></a>Información sobre amenazas

El filtrado basado en [inteligencia sobre amenazas](threat-intel.md) puede habilitarse para que el firewall alerte y deniegue el tráfico desde y hacia los dominios y las direcciones IP malintencionados. La direcciones IP y los dominios proceden de la fuente Inteligencia sobre amenazas de Microsoft.

## <a name="outbound-snat-support"></a>Compatibilidad con SNAT saliente

Todas las direcciones IP de tráfico de red virtual salientes se convierten a direcciones IP públicas de Azure Firewall (traducción de direcciones de red de origen). Puede identificar y permitir el tráfico que se origina en la red virtual y se dirige a los destinos de Internet remotos. Azure Firewall no aplica SNAT cuando la dirección IP de destino es un intervalo de direcciones IP privadas por [IANA RFC 1918](https://tools.ietf.org/html/rfc1918). 

Si su organización usa un intervalo de direcciones IP públicas para las redes privadas, Azure Firewall aplicará SNAT al tráfico para una de las direcciones IP privadas de firewall en AzureFirewallSubnet. Puede configurar Azure Firewall de modo que **no** aplique SNAT al intervalo de direcciones IP públicas. Para más información, consulte [Aplicación de SNAT por parte de Azure Firewall a intervalos de direcciones IP privadas](snat-private-range.md).

## <a name="inbound-dnat-support"></a>Compatibilidad con DNAT entrante

El tráfico entrante de Internet a la dirección IP pública de firewall se traduce (traducción de direcciones de red de destino) y se filtra para las direcciones IP privadas en las redes virtuales.

## <a name="multiple-public-ip-addresses"></a>Varias direcciones IP públicas

Puede asociar [varias direcciones IP públicas](deploy-multi-public-ip-powershell.md) (hasta 250) con el firewall.

Esto admite los siguientes escenarios:

- **DNAT**: puede traducir varias instancias de puerto estándar para los servidores back-end. Por ejemplo, si tiene dos direcciones IP públicas, puede traducir el puerto TCP 3389 (RDP) para ambas direcciones IP.
- **SNAT**: hay disponibles puertos adicionales para las conexiones SNAT salientes, lo que reduce la posibilidad de que se agoten los puertos SNAT. En este momento, Azure Firewall selecciona aleatoriamente la dirección IP pública de origen que se usará para una conexión. Si dispone de algún filtro de nivel inferior de la red, deberá permitir todas las direcciones IP públicas asociadas con el firewall. Considere la posibilidad de usar un [prefijo de dirección IP pública](../virtual-network/public-ip-address-prefix.md) para simplificar esta configuración.

## <a name="azure-monitor-logging"></a>Registro de Azure Monitor

Todos los eventos se integran en Azure Monitor, lo que permite archivar registros en una cuenta de almacenamiento, transmitir eventos al centro de eventos o enviarlos a los registros de Azure Monitor. Para obtener ejemplos de registro de Azure Monitor, consulte [Registros de Azure Monitor para Azure Firewall](log-analytics-samples.md).

Para más información, consulte el [Tutorial: Supervisión de métricas y registros de Azure Firewall](tutorial-diagnostics.md). 

El libro de Azure Firewall proporciona un lienzo flexible para el análisis de datos de Azure Firewall. Puede utilizarlo para crear informes visuales completos en Azure Portal. Para más información, consulte [Supervisión de registros mediante el libro de Azure Firewall](firewall-workbook.md).

## <a name="forced-tunneling"></a>Tunelización forzada

Puede configurar Azure Firewall para enrutar todo el tráfico vinculado a Internet a un próximo salto designado, en lugar de ir directamente a Internet. Por ejemplo, puede tener un servidor perimetral local u otra aplicación virtual de red (NVA) para procesar el tráfico de red antes de que pase a Internet. Para más información, consulte [Tunelización forzada de Azure Firewall](forced-tunneling.md).

## <a name="certifications"></a>Certificaciones

Azure Firewall es compatible con la industria de tarjetas de pago (PCI), los controles de organización de servicio (SOC), la Organización internacional de normalización (ISO) y con ICSA Labs. Para más información, consulte [Certificaciones de cumplimiento de Azure Firewall](compliance-certifications.md).

## <a name="next-steps"></a>Pasos siguientes

- [Lógica de procesamiento de reglas de Azure Firewall](rule-processing.md).
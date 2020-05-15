---
title: ¿Qué es Azure Firewall?
description: Azure Firewall es un servicio de seguridad de red administrado y basado en la nube que protege los recursos de Azure Virtual Network.
author: vhorne
ms.service: firewall
services: firewall
ms.topic: overview
ms.custom: mvc
ms.date: 05/11/2020
ms.author: victorh
Customer intent: As an administrator, I want to evaluate Azure Firewall so I can determine if I want to use it.
ms.openlocfilehash: 928e0cec1cad5d6fe8b70b728cd86a41577ce797
ms.sourcegitcommit: a8ee9717531050115916dfe427f84bd531a92341
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/12/2020
ms.locfileid: "83195358"
---
# <a name="what-is-azure-firewall"></a>¿Qué es Azure Firewall?

![Certificación ICSA](media/overview/icsa-cert-firewall-small.png)

Azure Firewall es un servicio de seguridad de red administrado y basado en la nube que protege los recursos de Azure Virtual Network. Se trata de un firewall como servicio con estado completo que incorpora alta disponibilidad y escalabilidad a la nube sin restricciones.

![Información general de firewalls](media/overview/firewall-threat.png)

Puede crear, aplicar y registrar directivas de aplicaciones y de conectividad de red a nivel central en suscripciones y redes virtuales. Azure Firewall usa una dirección IP pública estática para los recursos de red virtual, que permite que los firewall externos identifiquen el tráfico procedente de la red virtual.  El servicio está totalmente integrado con Azure Monitor para los registros y análisis.

Azure Firewall ofrece las siguientes características:

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

Puede limitar el tráfico HTTP/S o el tráfico de Azure SQL (versión preliminar) saliente a una lista especificada de nombres de dominio completos (FQDN) que incluye caracteres comodín. Esta característica no requiere terminación de TLS.

## <a name="network-traffic-filtering-rules"></a>Reglas de filtrado de tráfico de red

Puede crear reglas de filtrado de red para *permitir* o *denegar* por dirección IP de origen y destino, puerto y protocolo. Azure Firewall tiene estado completo, de modo que puede distinguir los paquetes legítimos de diferentes tipos de conexiones. Las reglas se aplican y se registran en varias suscripciones y redes virtuales.

## <a name="fqdn-tags"></a>Etiquetas FQDN

Con las etiquetas FQDN, puede permitir fácilmente el tráfico de red conocido del servicio de Azure a través del firewall. Por ejemplo, supongamos que quiere permitir el tráfico de red de Windows Update a través del firewall. Puede crear una regla de aplicación e incluir la etiqueta de Windows Update. Ahora, el tráfico de red de Windows Update puede fluir a través del firewall.

## <a name="service-tags"></a>Etiquetas de servicio

Una etiqueta de servicio representa un grupo de prefijos de direcciones IP que ayudan a reducir la complejidad de la creación de reglas de seguridad. No puede crear su propia etiqueta de servicio ni especificar qué direcciones IP se incluyen dentro de una etiqueta. Microsoft administra los prefijos de direcciones que incluye la etiqueta de servicio y actualiza automáticamente esta a medida que las direcciones cambian.

## <a name="threat-intelligence"></a>Información sobre amenazas

El filtrado basado en inteligencia sobre amenazas puede habilitarse para que el firewall alerte y deniegue el tráfico desde y hacia los dominios y las direcciones IP malintencionados. La direcciones IP y los dominios proceden de la fuente Inteligencia sobre amenazas de Microsoft.

## <a name="outbound-snat-support"></a>Compatibilidad con SNAT saliente

Todas las direcciones IP de tráfico de red virtual salientes se convierten a direcciones IP públicas de Azure Firewall (traducción de direcciones de red de origen). Puede identificar y permitir el tráfico que se origina en la red virtual y se dirige a los destinos de Internet remotos. Azure Firewall no aplica SNAT cuando la dirección IP de destino es un intervalo de direcciones IP privadas por [IANA RFC 1918](https://tools.ietf.org/html/rfc1918). 

Si su organización usa un intervalo de direcciones IP públicas para las redes privadas, Azure Firewall aplicará SNAT al tráfico para una de las direcciones IP privadas de firewall en AzureFirewallSubnet. Puede configurar Azure Firewall de modo que **no** aplique SNAT al intervalo de direcciones IP públicas. Para más información, consulte [Aplicación de SNAT por parte de Azure Firewall a intervalos de direcciones IP privadas](snat-private-range.md).

## <a name="inbound-dnat-support"></a>Compatibilidad con DNAT entrante

El tráfico entrante de Internet a la dirección IP pública de firewall se traduce (traducción de direcciones de red de destino) y se filtra para las direcciones IP privadas en las redes virtuales.

## <a name="multiple-public-ip-addresses"></a>Varias direcciones IP públicas

Puede asociar varias direcciones IP públicas (hasta 250) con el firewall.

Esto admite los siguientes escenarios:

- **DNAT**: puede traducir varias instancias de puerto estándar para los servidores back-end. Por ejemplo, si tiene dos direcciones IP públicas, puede traducir el puerto TCP 3389 (RDP) para ambas direcciones IP.
- **SNAT**: hay disponibles puertos adicionales para las conexiones SNAT salientes, lo que reduce la posibilidad de que se agoten los puertos SNAT. En este momento, Azure Firewall selecciona aleatoriamente la dirección IP pública de origen que se usará para una conexión. Si dispone de algún filtro de nivel inferior de la red, deberá permitir todas las direcciones IP públicas asociadas con el firewall. Considere la posibilidad de usar un [prefijo de dirección IP pública](../virtual-network/public-ip-address-prefix.md) para simplificar esta configuración.

## <a name="azure-monitor-logging"></a>Registro de Azure Monitor

Todos los eventos se integran en Azure Monitor, lo que permite archivar registros en una cuenta de almacenamiento, transmitir eventos al centro de eventos o enviarlos a los registros de Azure Monitor.

## <a name="forced-tunneling"></a>Tunelización forzada

Puede configurar Azure Firewall para enrutar todo el tráfico vinculado a Internet a un próximo salto designado, en lugar de ir directamente a Internet. Por ejemplo, puede tener un servidor perimetral local u otra aplicación virtual de red (NVA) para procesar el tráfico de red antes de que pase a Internet. Para más información, consulte [Tunelización forzada de Azure Firewall](forced-tunneling.md).

## <a name="certifications"></a>Certificaciones

Azure Firewall es compatible con la industria de tarjetas de pago (PCI), los controles de organización de servicio (SOC), la Organización internacional de normalización (ISO) y con ICSA Labs. Para más información, consulte [Certificaciones de cumplimiento de Azure Firewall](compliance-certifications.md).


## <a name="known-issues"></a>Problemas conocidos

Azure Firewall presenta los siguientes problemas conocidos:

|Problema  |Descripción  |Mitigación  |
|---------|---------|---------|
Las reglas de filtrado de red para protocolos que no son TCP/UDP (por ejemplo, ICMP) no funcionan con el tráfico enlazado a Internet|Las reglas de filtrado de red de protocolos que no son TCP/UDP no funcionan con la traducción SNAT a la dirección IP pública. Los protocolos que no son TCP/UDP no se admiten entre subredes de radio y redes virtuales.|Azure Firewall usa Standard Load Balancer, [que actualmente no admite SNAT para los protocolos IP](https://docs.microsoft.com/azure/load-balancer/load-balancer-overview). Se están examinando opciones para admitir este escenario en una versión futura.|
|Falta de compatibilidad entre PowerShell y CLI con ICMP|Azure PowerShell y la CLI no admiten ICMP como protocolo válido en las reglas de red.|Aun así se puede usar ICMP como protocolo a través del portal y la API REST. Estamos trabajando para agregar pronto ICMP a PowerShell y la CLI.|
|Las etiquetas FQDN requieren que se establezca una combinación protocolo: puerto|Las reglas de aplicaciones con las etiquetas FQDN requieren la definición de puerto: protocolo.|Puede usar **https** como valor de puerto: protocolo. Estamos trabajando para que este campo sea opcional cuando se usen etiquetas FQDN.|
|No se admite la posibilidad de mover un firewall a otro grupo de recursos o suscripción.|No se admite la posibilidad de mover un firewall a otro grupo de recursos o suscripción.|La compatibilidad con esta funcionalidad está en nuestro mapa de ruta. Para mover un firewall a otro un grupo de recursos o suscripción, debe eliminar la instancia actual y volver a crearla en el nuevo grupo de recursos o suscripción.|
|Las alertas de inteligencia sobre amenazas pueden estar enmascaradas|Reglas de red con destino 80/443 para las alertas de inteligencia sobre amenazas de las máscaras de filtrado de salida cuando la configuración está establecida en el modo de solo alerta.|Cree filtrado de salida para 80/443 mediante reglas de aplicaciones. También puede cambiar el modo de inteligencia sobre amenazas a **Alertar y denegar**.|
|Azure Firewall usa Azure DNS para la resolución de nombres|Azure Firewall resuelve los FQDN solo mediante Azure DNS. No se admite un servidor DNS personalizado. No hay ningún efecto sobre la resolución de DNS en otras subredes.|Estamos trabajando para reducir esta limitación.|
|DNAT de Azure Firewall no funciona para destinos de IP privadas|La compatibilidad con DNAT de Azure Firewall se limita a la entrada/salida de Internet. DNAT no funciona actualmente con destinos de IP privada. Por ejemplo, radio a radio.|Se trata de una limitación actual.|
|No se puede eliminar la primera configuración de IP pública|Cada IP pública de Azure Firewall se asigna a una *configuración de IP*.  La primera configuración de IP se asigna durante la implementación del firewall y, normalmente, también contiene una referencia a la subred del firewall (a menos que se configure explícitamente de forma diferente mediante una implementación de plantilla). No se puede eliminar esta configuración de IP porque podría anular la asignación del firewall. Aún podrá cambiar o eliminar la dirección IP pública asociada con esta configuración de IP si el firewall tiene al menos otra dirección IP pública disponible para su uso.|es así por diseño.|
|Availability Zones solo se puede configurar durante la implementación.|Availability Zones solo se puede configurar durante la implementación. No se puede configurar una vez implementado un firewall.|es así por diseño.|
|SNAT en conexiones entrantes|Además de DNAT, en las conexiones mediante la dirección IP pública del firewall (entrante) se aplica SNAT en una de las direcciones IP privadas del firewall. Este requisito hoy en día (también para aplicaciones virtuales de red activa/activa) es para garantizar el enrutamiento simétrico.|Para conservar el código fuente original para HTTP/S, considere el uso de encabezados [XFF](https://en.wikipedia.org/wiki/X-Forwarded-For). Por ejemplo, use un servicio como [Azure Front Door](../frontdoor/front-door-http-headers-protocol.md#front-door-to-backend) o [Azure Application Gateway](../application-gateway/rewrite-http-headers.md) delante del firewall. También puede agregar WAF como parte de Azure Front Door Service y la cadena al firewall.
|El filtrado por nombre de dominio completo de SQL se admite solo en modo de proxy (puerto 1433)|Para Azure SQL Database, Azure SQL Data Warehouse e Instancia administrada de SQL Database:<br><br>Durante la versión preliminar, el filtrado por nombre de dominio completo de SQL se admite solo en modo de proxy (puerto 1433).<br><br>Para IaaS de Azure SQL:<br><br>Si va a usar puertos que no son los estándar, puede especificar esos puertos en las reglas de la aplicación.|Para SQL en modo de redirección (el valor predeterminado si se conecta desde dentro de Azure), puede filtrar en su lugar el acceso mediante la etiqueta de servicio de SQL como parte de las reglas de red de Azure Firewall.
|No se permite tráfico de salida en el puerto TCP 25.| Las conexiones SMTP salientes que usan el puerto TCP 25 se bloquearon. El puerto 25 se usa principalmente para la entrega de correo electrónico sin autenticación. Éste es el comportamiento predeterminado de la plataforma para las máquinas virtuales. Para más información, consulte [Solución de problemas de conectividad SMTP saliente en Azure](../virtual-network/troubleshoot-outbound-smtp-connectivity.md). Sin embargo, a diferencia de las máquinas virtuales, actualmente no es posible habilitar esta funcionalidad en Azure Firewall. Nota: Para permitir SMTP autenticado (puerto 587) o SMTP a través de un puerto distinto de 25, asegúrese de configurar una regla de red y no una regla de aplicación, ya que la inspección de SMTP no se admite en este momento.|Siga el método recomendado para enviar correo electrónico, como se documenta en el artículo de solución de problemas de SMTP. O bien, excluya la máquina virtual que necesita acceso SMTP saliente desde la ruta predeterminada hasta el firewall. En su lugar, configure el acceso saliente directamente a Internet.
|No se admite el FTP activo|El FTP activo está deshabilitado en Azure Firewall para protegerse frente a ataques de devolución FTP mediante el comando PORT de FTP.|En su lugar, puede usar el FTP pasivo. Todavía debe abrir explícitamente los puertos TCP 20 y 21 en el firewall.
|La métrica de uso de puertos SNAT muestra un 0 %|La métrica de uso de puertos SNAT de Azure Firewall puede mostrar un uso del 0 % incluso cuando se usan puertos SNAT. En este caso, el uso de la métrica como parte de la métrica de estado de mantenimiento del firewall proporciona un resultado incorrecto.|Este problema se ha corregido y su implementación en producción está prevista para mayo de 2020. En algunos casos, la reimplementación del firewall resuelve el problema, pero no es coherente. Como solución intermedia, use solo el estado de mantenimiento del firewall para buscar *estado=degradado*, no para *estado=incorrecto*. El agotamiento de puertos se muestra como *degradado*. *Incorrecto* se reserva para un uso futuro cuando haya más métricas que afecten al estado de mantenimiento del firewall.
|DNAT no es compatible con la tunelización forzada habilitada|Los firewalls implementados con la tunelización forzada habilitada no admiten el acceso entrante desde Internet debido al enrutamiento asimétrico.|Esto es así por diseño. La ruta de acceso de retorno de las conexiones entrantes pasa por el firewall local, que no ha visto la conexión establecida.
|FTP pasivo saliente no funciona en el caso de los firewalls con varias direcciones IP públicas.|FTP pasivo establece distintas conexiones para los canales de control y datos. Cuando un firewall con varias direcciones IP públicas envía datos de salida, selecciona de manera aleatoria una de sus direcciones IP públicas como dirección IP de origen. FTP produce un error cuando los canales de control y datos usan direcciones IP de origen diferentes.|Está previsto una configuración de SNAT explícita. Mientras tanto, considere la posibilidad de usar una única dirección IP en esta situación.|
|Falta una dimensión de protocolo en la métrica NetworkRuleHit|La métrica ApplicationRuleHit permite el filtrado basado en el protocolo, pero esta funcionalidad falta en la métrica NetworkRuleHit correspondiente.|Se está investigando una solución.|
|No se admiten las reglas NAT con puertos entre el 64000 y el 65535|Azure Firewall permite cualquier puerto en el rango 1-65535 en reglas de red y de aplicación; sin embargo, las reglas NAT solo admiten los puertos del rango 1-63999.|Se trata de una limitación actual.
|Las actualizaciones de configuración pueden tardar cinco minutos por término medio.|Una actualización de la configuración de Azure Firewall puede tardar entre tres y cinco minutos por término medio, y no se admiten actualizaciones en paralelo.|Se está investigando una solución.

## <a name="next-steps"></a>Pasos siguientes

- [Tutorial: Implementación y configuración de Azure Firewall mediante Azure Portal](tutorial-firewall-deploy-portal.md)
- [Implementación de Azure Firewall mediante una plantilla](deploy-template.md)
- [Creación de un entorno de prueba de Azure Firewall](scripts/sample-create-firewall-test.md)

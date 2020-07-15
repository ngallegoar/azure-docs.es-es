---
title: Preguntas frecuentes sobre Azure Load Balancer
description: Respuestas a las preguntas frecuentes sobre Azure Load Balancer.
services: load-balancer
author: erichrt
ms.service: load-balancer
ms.topic: article
ms.date: 04/22/2020
ms.author: errobin
ms.openlocfilehash: 205a4bd119a7324c4e6524a0e29d432aa57bf315
ms.sourcegitcommit: cec9676ec235ff798d2a5cad6ee45f98a421837b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85848223"
---
# <a name="load-balancer-frequently-asked-questions"></a>Preguntas frecuentes sobre Load Balancer

## <a name="what-types-of-load-balancer-exist"></a>¿Qué tipos de Load Balancer existen?
Equilibradores de carga internos que equilibran el tráfico dentro de una red virtual y equilibradores de carga externos que equilibran el tráfico hacia y desde un punto de conexión conectado a Internet. Para más información, consulte [Tipos de Load Balancer](components.md#frontend-ip-configurations). 

Para ambos tipos, Azure ofrece una SKU básica y una SKU estándar que tienen diferentes capacidades funcionales, de rendimiento, de seguridad y de seguimiento de estado. Estas diferencias se explican en el artículo [Comparación de SKU](skus.md).

 ## <a name="how-can-i-upgrade-from-a-basic-to-a-standard-load-balancer"></a>¿Cómo puedo actualizar de la versión Basic de Load Balancer a la versión Standard?
Consulte el artículo [Actualización de Basic a Standard](upgrade-basic-standard.md) para obtener una secuencia de comandos automatizada e instrucciones sobre la actualización de una SKU de Load Balancer.

 ## <a name="what-are-the-different-load-balancing-options-in-azure"></a>¿Cuáles son las diferentes opciones de equilibrio de carga en Azure?
Consulte la [Guía de tecnología de Load Balancer](https://docs.microsoft.com/azure/architecture/guide/technology-choices/load-balancing-overview) para conocer los servicios de equilibrio de carga disponibles y los usos recomendados para cada uno.

## <a name="where-can-i-find-load-balancer-arm-templates"></a>¿Dónde puedo encontrar plantillas de Resource Manager para Load Balancer?
Consulte la [lista de plantillas de inicio rápido de Azure Load Balancer](https://docs.microsoft.com/azure/templates/microsoft.network/loadbalancers#quickstart-templates) para conocer las plantillas de Resource Manager de las implementaciones comunes.

## <a name="how-are-inbound-nat-rules-different-from-load-balancing-rules"></a>¿En qué se diferencian las reglas NAT de entrada de las reglas de equilibrio de carga?
Las reglas NAT se usan para especificar un recurso de back-end al que enrutar el tráfico. Por ejemplo, la configuración de un puerto de equilibrador de carga específico para enviar el tráfico RDP a una máquina virtual específica. Las reglas de equilibrio de carga se usan para especificar un grupo de recursos de back-end para enrutar el tráfico y equilibrar la carga en cada instancia. Por ejemplo, una regla de equilibrador de carga puede enrutar los paquetes TCP en el puerto 80 del equilibrador de carga en un grupo de servidores web.

## <a name="what-is-ip-1686312916"></a>¿Qué es la IP 168.63.129.16?
La dirección IP virtual para el host etiquetada como equilibrador de carga de la infraestructura de Azure en la que se originan los sondeos del estado de Azure. Al configurar las instancias de back-end, deben permitir que el tráfico procedente de esta dirección IP responda correctamente a los sondeos de estado. Esta regla no interactúa con el acceso al front-end de Load Balancer. Si no usa Azure Load Balancer, puede anular esta regla. [Aquí](https://docs.microsoft.com/azure/virtual-network/service-tags-overview#available-service-tags) encontrará más información sobre las etiquetas de servicio.

## <a name="can-i-use-global-vnet-peering-with-basic-load-balancer"></a>¿Puedo usar el emparejamiento de VNET global con Load Balancer básico?
No. Load Balancer básico no admite el emparejamiento de VNET global. En su lugar, puede usar Standard Load Balancer. Consulte el artículo de [actualización de básico a Standard](upgrade-basic-standard.md) para una actualización sin problemas.

## <a name="how-can-i-discover-the-public-ip-that-an-azure-vm-uses"></a>¿Cómo puedo descubrir la dirección IP pública que usa una máquina virtual de Azure?

Hay muchas maneras de determinar la dirección IP de origen público de una conexión saliente. OpenDNS proporciona un servicio que puede mostrar la dirección IP pública de la máquina virtual.
Mediante el comando nslookup, puede enviar una consulta DNS del nombre myip.opendns.com para la resolución de OpenDNS. El servicio devuelve la dirección IP de origen que se usó para enviar la consulta. Si ejecuta la siguiente consulta desde la máquina virtual, la respuesta es la dirección IP pública que se usa para esa máquina virtual:

 ```nslookup myip.opendns.com resolver1.opendns.com```

## <a name="how-do-connections-to-azure-storage-in-the-same-region-work"></a>¿Cómo funcionan las conexiones a Azure Storage en la misma región?
La conectividad saliente a través de los escenarios anteriores no es necesaria para conectarse al almacenamiento en la misma región que la máquina virtual. Si no quiere que pase esto, use grupos de seguridad de red (NSG) como se explicó anteriormente. Para la conectividad con el almacenamiento en otras regiones, se requiere conectividad de salida. Tenga en cuenta que, al conectarse al almacenamiento desde una máquina virtual en la misma región, la dirección IP de origen en los registros de diagnóstico de almacenamiento será una dirección de proveedor interna y no la dirección IP pública de la máquina virtual. Si desea restringir el acceso a la cuenta de almacenamiento a las máquinas virtuales de una o varias subredes de Virtual Network en la misma región, use [Virtual Network los puntos de conexión de servicio](../virtual-network/virtual-network-service-endpoints-overview.md) y no la dirección IP pública al configurar el firewall de la cuenta de almacenamiento. Una vez configurados los puntos de conexión de servicio, verá la dirección IP privada de su instancia de Virtual Network en los registros de diagnóstico de almacenamiento y no la dirección interna del proveedor.

## <a name="next-steps"></a>Pasos siguientes
Si su pregunta no aparece en la lista anterior, envíe sus comentarios sobre esta página junto con su pregunta. Esto creará un problema de GitHub para que el equipo del producto se asegure de que las preguntas de todos nuestros valiosos clientes se respondan.

---
title: Componentes de Azure Load Balancer
description: Información general acerca de los componentes de Azure Load Balancer
services: load-balancer
documentationcenter: na
author: asudbring
ms.service: load-balancer
ms.devlang: na
ms.topic: overview
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/30/2020
ms.author: allensu
ms.openlocfilehash: 4a84c43b57ec4f632a2bfabb10d112e4975249bf
ms.sourcegitcommit: 4499035f03e7a8fb40f5cff616eb01753b986278
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/03/2020
ms.locfileid: "82733114"
---
# <a name="azure-load-balancer-components"></a>Componentes de Azure Load Balancer

Azure Load Balancer contiene varios componentes clave para su funcionamiento. Estos componentes se pueden configurar en la suscripción mediante Azure Portal, la CLI de Azure, Azure PowerShell o plantillas.

## <a name="frontend-ip-configurations"></a>Configuraciones de direcciones IP de front-end

la dirección IP del equilibrador de carga. Es el punto de contacto de los clientes. Estas direcciones pueden ser:

- **Dirección IP pública**
- **Dirección IP privada**

La selección de la dirección IP determina el **tipo** de equilibrador de carga creado. Si se selecciona una dirección IP privada, se crea un equilibrador de carga interno. Si se selecciona una dirección IP pública, se crea un equilibrador de carga público.

|  | Equilibrador de carga público  | Equilibrador de carga interno |
| ---------- | ---------- | ---------- |
| Configuración de direcciones IP de front-end| Dirección IP pública | Dirección IP privada|
| Descripción | Los equilibradores de carga públicos asignan la IP pública y el puerto de tráfico entrante a la IP privada y al puerto de la máquina virtual. El equilibrador de carga asigna el tráfico de la otra manera en torno al tráfico de respuesta de la máquina virtual. Puede distribuir determinados tipos de tráfico entre varias máquinas virtuales o servicios aplicando reglas de equilibrio de carga. Por ejemplo, puede distribuir la carga del tráfico de solicitudes web entre varios servidores web.| Un equilibrador de carga interno distribuye el tráfico a los recursos que se encuentran en una red virtual. Azure restringe el acceso a las direcciones IP de front-end de una red virtual que tienen equilibrio de carga. Las direcciones IP de front-end y las redes virtuales no se exponen nunca directamente a un punto de conexión de Internet. Las aplicaciones de línea de negocio internas se ejecutan en Azure y se accede a ellas desde Azure o desde recursos locales. |
| SKU compatibles | Básico y Estándar | Básico y Estándar |

![Ejemplo de equilibrador de carga con niveles](./media/load-balancer-overview/load-balancer.png)

## <a name="backend-pool"></a>Grupo back-end

El grupo de máquinas virtuales o instancias de un conjunto de escalado de máquinas virtuales que van a atender la solicitud entrante. Para escalar de forma rentable, con el fin de satisfacer grandes volúmenes de instrucciones para el procesamiento de tráfico entrante, generalmente se recomienda agregar más instancias al grupo de back-end. 

El equilibrador de carga se reconfigura al instante de forma automática mediante el escalado o la reducción vertical de instancias. Si se agregan o quitan máquinas virtuales del grupo de servidores back-end, el equilibrador de carga se vuelve a configurar sin operaciones adicionales. El ámbito del grupo de back-end es cualquier máquina virtual de la red virtual. 

A la hora de considerar cómo diseñar el grupo de back-end, diseñe el menor número de recursos individuales del grupo de back-end para optimizar la duración de las operaciones de administración. No hay ninguna diferencia en el rendimiento o la escala del plano de los datos.

## <a name="health-probes"></a>Sondeos de estado

los sondeos de estado se usan para determinar el estado de las instancias del grupo de back-end. Puede definir el umbral incorrecto de los sondeos de estado. Cuando un sondeo no responde, el equilibrador de carga deja de enviar nuevas conexiones a las instancias incorrectas. Un error de sondeo no afecta a las conexiones existentes. La conexión continúa hasta que la aplicación:

- Finaliza el flujo.
- Se produce el tiempo de espera de inactividad.
- La máquina virtual se apaga.

Load Balancer proporciona diferentes tipos de sondeo de estado para los puntos de conexión:

- TCP
- HTTP
- HTTPS

La instancia de Load Balancer Básico no admite sondeos HTTPS. Además, cierra todas las conexiones TCP (incluidas las conexiones establecidas).

## <a name="load-balancing-rules"></a>Reglas de equilibrio de carga

Las reglas de equilibrio de carga son las que indican al equilibrador de carga lo que debe hacer. Una regla de equilibrio de carga asigna una configuración de IP de front-end y un puerto determinados a varios puertos y direcciones IP de back-end.

## <a name="inbound-nat-rules"></a>Reglas NAT de entrada

Las reglas NAT de entrada reenvían el tráfico desde la dirección IP de front-end a una instancia de back-end en la red virtual. El reenvío de puertos se realiza mediante la misma distribución basada en hash que el equilibrio de carga. 

Un ejemplo de uso son las sesiones de Secure Shell (SSH) o del Protocolo de escritorio remoto (RDP) para separar las instancias de máquina virtual en una red virtual. Se pueden asignar varios puntos de conexión internos a puertos de la misma dirección IP de front-end. Las direcciones IP de front-end se pueden usar para administrar de forma remota máquinas virtuales sin un jumpbox adicional.

## <a name="outbound-rules"></a>Reglas de salida

Una regla de salida configura una traducción de direcciones de red (NAT) de salida para todas las máquinas virtuales o instancias identificadas por el grupo de back-end.

Load Balancer Básico no admite reglas de salida.
![Equilibrador de carga de Azure](./media/load-balancer-overview/load-balancer-overview.png)

## <a name="next-steps"></a>Pasos siguientes

- Consulte el artículo sobre cómo [crear una instancia de Standard Load Balancer pública](quickstart-load-balancer-standard-public-portal.md) para empezar a usar un equilibrador de carga.
- Más información sobre [Azure Load Balancer](load-balancer-overview.md).
- Información sobre las [direcciones IP públicas](https://docs.microsoft.com/azure/virtual-network/virtual-network-public-ip-address)
- Información sobre las [direcciones IP privadas](https://docs.microsoft.com/azure/virtual-network/virtual-network-ip-addresses-overview-arm#private-ip-addresses)
- Más información en [Standard Load Balancer y Availability Zones](load-balancer-standard-availability-zones.md).
- Más información acerca de los [diagnósticos de Load Balancer Estándar](load-balancer-standard-diagnostics.md).
- Obtenga información sobre el [restablecimiento de TCP en estado inactivo](load-balancer-tcp-reset.md).
- Más información acerca de [Standard Load Balancer con reglas de equilibrio de carga para puertos HA](load-balancer-ha-ports-overview.md).
- Más información acerca de cómo usar [Load Balancer con varios front-ends](load-balancer-multivip-overview.md).
- Más información sobre los [grupos de seguridad de red](../virtual-network/security-overview.md).
- Información sobre los [tipos de sondeo](load-balancer-custom-probe-overview.md#types).
- Más información sobre los [límites de Load Balancer](https://docs.microsoft.com/azure/azure-resource-manager/management/azure-subscription-service-limits#load-balancer).
- Información sobre el uso del [reenvío de puertos](https://docs.microsoft.com/azure/load-balancer/tutorial-load-balancer-port-forwarding-portal).
- Más información sobre las [reglas de salida de Load Balancer](https://docs.microsoft.com/azure/load-balancer/load-balancer-outbound-rules-overview).

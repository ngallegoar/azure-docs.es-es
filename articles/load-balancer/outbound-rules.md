---
title: Reglas de salida en Azure Load Balancer
description: En este artículo se explica cómo configurar reglas de salida para controlar el tráfico de salida de Internet con Azure Load Balancer.
services: load-balancer
author: asudbring
ms.service: load-balancer
ms.topic: conceptual
ms.custom: contperfq1
ms.date: 10/13/2020
ms.author: allensu
ms.openlocfilehash: 51810876e3636b7023ce9c9318a071636bb00c4c
ms.sourcegitcommit: 090ea6e8811663941827d1104b4593e29774fa19
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/13/2020
ms.locfileid: "92002612"
---
# <a name="outbound-rules-azure-load-balancer"></a><a name="outboundrules"></a>Reglas de salida en Azure Load Balancer

Las reglas de salida le permiten configurar la traducción de direcciones de red de origen (SNAT) públicas de salida de Standard Load Balancer. Esta configuración le permite usar las direcciones IP públicas de su equilibrador de carga como proxy.

Esta configuración habilita:

* El enmascaramiento de IP
* La simplificación de las listas de permitidos
* La reducción del número de recursos de IP pública para la implementación

Con las reglas de salida, tiene un control declarativo completo sobre la conectividad saliente de Internet. Las reglas de salida le permiten escalar y ajustar esta capacidad a sus necesidades específicas. 

Solo se seguirán las reglas de salida si la VM de back-end no tiene una dirección IP pública de nivel de instancia (ILPIP).

![Reglas de salida de Load Balancer](media/load-balancer-outbound-rules-overview/load-balancer-outbound-rules.png)

Con las reglas de salida, puede definir explícitamente un comportamiento **SNAT**.

Las reglas de salida permiten controlar:

* **Qué máquinas virtuales se deben traducir a qué direcciones IP públicas.**
     * Dos reglas, donde el grupo de back-end A usa las direcciones IP A y B, el grupo de back-end B usa las direcciones IP C y D.
* **Cómo se conceden los puertos SNAT salientes.**
     * El grupo de back-end B es el único grupo que realiza conexiones salientes, concede todos los puertos SNAT al grupo de back-end B y ninguno al grupo de back-end A.
* **A qué protocolos se proporciona traducción de salida.**
     * El grupo de back-end B necesita puertos UDP para la salida. El grupo de back-end A necesita TCP. Asigne los puertos TCP a A y los puertos UDP a B.
* **Qué duración se debe usar para el tiempo de espera de inactividad de conexión (de 4 a 120 minutos).**
     * Si hay conexiones de larga ejecución con Keepalives, reserve puertos inactivos para las conexiones de larga ejecución durante un máximo de 120 minutos. Suponga que las conexiones obsoletas se han abandonado y libere los puertos en 4 minutos para las conexiones nuevas. 
* **Si quiere enviar un restablecimiento de TCP en caso de tiempo de espera de inactividad.**
     * Al agotar el tiempo de espera de las conexiones inactivas, ¿se envía un RST de TCP al cliente y al servidor para que sepan que se ha abandonado el flujo?

## <a name="outbound-rule-definition"></a>Definición de la regla de salida

Las reglas de salida siguen la misma sintaxis conocida que las reglas de NAT de entrada y de equilibrio de carga: **front-end** + **parámetros** + **grupo de back-end**. 

Una regla de salida configura una NAT de salida para _todas las máquinas virtuales identificadas por el grupo de back-end_ que se deben traducir para el _front-end_.  

Los _parámetros_ proporcionan un control más preciso sobre el algoritmo de NAT de salida.

## <a name="scale-outbound-nat-with-multiple-ip-addresses"></a><a name="scale"></a> NAT de salida de escala con varias direcciones IP

Cada dirección IP adicional que proporciona un front-end ofrece 64 000 puertos efímeros adicionales que el equilibrador de carga puede usar como puertos SNAT. 

Use varias direcciones IP para planear escenarios a gran escala. Use reglas de salida para mitigar el [agotamiento de SNAT](troubleshoot-outbound-connection.md#snatexhaust). 

También puede usar un [prefijo de IP pública](https://aka.ms/lbpublicipprefix) directamente con una regla de salida. 

Un prefijo de dirección IP pública aumenta el escalado de la implementación. El prefijo se puede agregar a la lista de permitidos de flujos procedentes de los recursos de Azure. Puede definir una configuración de IP de front-end en el equilibrador de carga para hacer referencia a un prefijo de dirección IP pública.  

El equilibrador de carga tiene control sobre el prefijo de IP pública. La regla de salida usará automáticamente todas las direcciones IP públicas que contiene el prefijo de IP pública para las conexiones de salida. 

Cada una de las direcciones IP dentro del prefijo de IP pública proporciona 64 000 puertos efímeros adicionales por dirección IP que el equilibrador de carga puede usar como puertos SNAT.

## <a name="outbound-flow-idle-timeout-and-tcp-reset"></a><a name="idletimeout"></a> Tiempo de espera de inactividad de flujo de salida y restablecimiento de TCP

Las reglas de salida proporcionan un parámetro de configuración para controlar el tiempo de espera de inactividad del flujo de salida y que se corresponda con las necesidades de su aplicación. El tiempo de espera de inactividad de salida predeterminado es de 4 minutos. Para obtener más información, consulte la [configuración de tiempos de espera de inactividad](load-balancer-tcp-idle-timeout.md). 

El comportamiento predeterminado del equilibrador de carga es anular el flujo en silencio cuando se alcanza el tiempo de espera de inactividad de salida. El parámetro `enableTCPReset` habilita un control y comportamiento predecibles de la aplicación. El parámetro indica si se debe enviar el restablecimiento de TCP (TCP RST) bidireccional en el tiempo de espera del tiempo de espera de inactividad de salida. 

Revise [Restablecimiento de TCP al agotarse el tiempo de espera de inactividad](https://aka.ms/lbtcpreset) para obtener detalles como la disponibilidad por regiones.

## <a name="securing-and-controlling-outbound-connectivity-explicitly"></a><a name="preventoutbound"></a>Protección y control de la conectividad de salida de manera explícita

Las reglas de equilibrio de carga proporcionan programación automática de NAT de salida. Algunos escenarios requieren que deshabilite la programación automática de NAT de salida por parte de la regla de equilibrio de carga. Deshabilitar a través de la regla le permite controlar o perfeccionar el comportamiento.  

Puede usar este parámetro de dos maneras:

1. Prevención de la dirección IP de entrada para SNAT de salida. Deshabilite la SNAT de salida en la regla de equilibrio de carga.
  
2. Ajuste los parámetros de **SNAT** de salida de una dirección IP que se usa para el tráfico de entrada y salida al mismo tiempo. La NAT de salida automática debe deshabilitarse para permitir que una regla de salida tome el control. Para cambiar la asignación de puertos SNAT de una dirección que también se usa para la entrada, el parámetro `disableOutboundSnat` se debe establecer en true. 

Se producirá un error en la operación de configuración de una regla de salida si intenta redefinir una dirección IP que se usa para la entrada.  Deshabilite primero la NAT de salida de la regla de equilibrio de carga.

>[!IMPORTANT]
> La máquina virtual no tendrá conectividad de salida si este parámetro se establece en true y no tiene una regla de salida para definir la conectividad de salida.  Algunas operaciones de la VM o la aplicación pueden depender de la conectividad de salida disponible. Asegúrese de comprender las dependencias del escenario y de haber considerado el impacto de hacer este cambio.

En ocasiones, no es aconsejable que una VM cree un flujo de salida. Puede que exista un requisito para administrar qué destinos reciben flujos de salida o qué destinos comienzan los flujos de entrada. Use los [grupos de seguridad de red](../virtual-network/security-overview.md) para administrar los destinos a los que se conecta la VM. Use los NSG para administrar qué destinos públicos inician los flujos de entrada.

Cuando aplique un grupo de seguridad de red a una máquina virtual de carga equilibrada, preste atención a las [etiquetas de servicio](../virtual-network/security-overview.md#service-tags) y a las [reglas de seguridad predeterminadas](../virtual-network/security-overview.md#default-security-rules). 

Asegúrese de que la VM pueda recibir solicitudes de sondeo de mantenimiento desde Azure Load Balancer.

Si un grupo de seguridad de red bloquea las solicitudes de sondeo de mantenimiento de la etiqueta predeterminada AZURE_LOADBALANCER, se producirá un error en el sondeo de mantenimiento de la máquina virtual y esta se marca como no disponible. El equilibrador de carga dejará de enviar nuevos flujos a esa VM.

## <a name="limitations"></a>Limitaciones

- El número máximo de puertos efímeros posibles por dirección IP de front-end es 64 000.
- El intervalo de tiempo de espera de inactividad de salida que puede configurar oscila entre 4 y 120 minutos (de 240 a 7200 segundos).
- El equilibrador de carga no es compatible con ICMP para NAT de salida.
- Las reglas de salida solo se pueden aplicar a la configuración de IP principal de una NIC.  No se puede crear una regla de salida para la dirección IP secundaria de una VM o NVA. Se admiten varias NIC.
- Todas las máquinas virtuales de un **conjunto de disponibilidad** se deben agregar al grupo de back-end para la conectividad de salida. 
- Todas las máquinas virtuales de un **conjunto de escalado de máquinas virtuales** se deben agregar al grupo de back-end para la conectividad de salida.

## <a name="next-steps"></a>Pasos siguientes

- Más información sobre [Azure Standard Load Balancer](load-balancer-overview.md).
- Consulte las [preguntas más frecuentes sobre Azure Load Balancer](load-balancer-faqs.md)


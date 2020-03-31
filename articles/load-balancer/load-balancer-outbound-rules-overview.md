---
title: 'Reglas de salida: Azure Load Balancer'
description: Con esta ruta de aprendizaje, empiece a usar reglas de salida para definir las traducciones de las direcciones de red de salida.
services: load-balancer
documentationcenter: na
author: asudbring
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.custom: seodec18
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 7/17/2019
ms.author: allensu
ms.openlocfilehash: d419c213b3bcfef3631d68eb9d4cb485291bed31
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "78304198"
---
# <a name="load-balancer-outbound-rules"></a>Reglas de salida de Load Balancer

Azure Load Balancer proporciona conectividad de salida desde una red virtual, además de conectividad de entrada.  Las reglas de salida simplifican la configuración de la traducción de direcciones de red públicas de salida de [Standard Load Balancer](load-balancer-standard-overview.md).  Tiene control declarativo completo sobre la conectividad de salida para escalar y ajustar esta conectividad según sus necesidades específicas.

![Reglas de salida de Load Balancer](media/load-balancer-outbound-rules-overview/load-balancer-outbound-rules.png)

Con las reglas de salida, puede usar Load Balancer para: 
- definir la NAT de salida desde cero.
- escalar y ajustar el comportamiento de la NAT de salida existente. 

Las reglas de salida permiten controlar:
- qué máquinas virtuales se deben traducir para qué direcciones IP públicas. 
- cómo se deben asignar los [puertos de SNAT de salida](load-balancer-outbound-connections.md#snat).
- para qué protocolos se proporciona traducción de salida.
- qué duración se debe usar para el tiempo de espera de inactividad de conexión (de 4 a 120 minutos).
- si quiere enviar un restablecimiento de TCP en caso de tiempo de espera de inactividad.

Las reglas de salida expanden el [escenario 2](load-balancer-outbound-connections.md#lb) que se describe en el artículo [Conexiones salientes en Azure](load-balancer-outbound-connections.md) y la precedencia del escenario permanece igual.

## <a name="outbound-rule"></a>Regla de salida

Al igual que el resto de reglas de Load Balancer, las reglas de salida siguen la misma sintaxis conocida que las reglas de NAT de entrada y el equilibrio de carga:

**front-end** + **parámetros** + **grupo de back-end**

Una regla de salida configura una NAT de salida para _todas las máquinas virtuales identificadas por el grupo de back-end_ que se deben traducir para el _front-end_.  Y los _parámetros_ proporcionan un control más preciso sobre el algoritmo de NAT de salida.

La versión de la API "2018-07-01" permite una definición de regla de salida que se estructura como sigue:

```json
      "outboundRules": [
        {
          "frontendIPConfigurations": [ list_of_frontend_ip_configuations ],
          "allocatedOutboundPorts": number_of_SNAT_ports,
          "idleTimeoutInMinutes": 4 through 66,
          "enableTcpReset": true | false,
          "protocol": "Tcp" | "Udp" | "All",
          "backendAddressPool": backend_pool_reference,
        }
      ]
```

>[!NOTE]
>La configuración de NAT de salida eficaz es una composición de todas las reglas de salida y de equilibrio de carga. Las reglas de salida son incrementales para las reglas de equilibrio de carga. Revise [Deshabilitar la NAT de salida para una regla de equilibrio de carga](#disablesnat) para administrar la traducción de NAT de salida eficaz cuando se aplican varias reglas a una VM. Debe [deshabilitar la SNAT de salida](#disablesnat) al definir una regla de salida que usa la misma dirección IP pública que una regla de equilibrio de carga.

### <a name="scale-outbound-nat-with-multiple-ip-addresses"></a><a name="scale"></a> NAT de salida de escala con varias direcciones IP

Mientras una regla de salida se puede usar con una única dirección IP pública, las reglas de salida alivian la carga de configuración para escalar la NAT de salida. Puede usar varias direcciones IP para planear escenarios de gran escala y puede usar reglas de salida para mitigar los patrones con tendencia a [agotamiento de SNAT](load-balancer-outbound-connections.md#snatexhaust).  

Cada dirección IP adicional que proporciona un front-end ofrece 64 000 puertos efímeros que Load Balancer puede usar como puertos SNAT. Mientras las reglas de equilibrio de carga o NAT de entrada tienen un único front-end, la regla de salida amplía la noción de front-end y permite varios front-ends por regla.  Con varios front-ends por regla, se multiplica la cantidad de puertos SNAT disponibles con cada dirección IP pública y se pueden admitir escenarios grandes.

Además, puede usar un [prefijo de IP pública](https://aka.ms/lbpublicipprefix) directamente con una regla de salida.  El uso de un prefijo de dirección IP pública proporciona un escalado más fácil y una creación simplificada de listas de permitidos para los flujos que se originan en la implementación de Azure. Puede configurar una configuración de IP de front-end en el recurso de Load Balancer para hacer referencia directamente a un prefijo de dirección IP pública.  Esto permite el control exclusivo de Load Balancer sobre el prefijo IP público y la regla de salida usará automáticamente todas las direcciones IP públicas que contiene el prefijo de IP pública para las conexiones de salida.  Cada una de las direcciones IP dentro del intervalo del prefijo de IP pública proporciona 64 000 puertos efímeros por dirección IP que Load Balancer puede usar como puertos SNAT.   

No se pueden crear recursos de dirección IP pública individuales a partir del prefijo de IP pública cuando se usa esta opción, ya que la regla de salida debe tener control total sobre el prefijo de IP pública.  Si necesita un control más específico, puede crear un recurso de dirección IP pública individual a partir del prefijo de IP pública y asignar varias direcciones IP públicas individualmente al front-end de una regla de salida.

### <a name="tune-snat-port-allocation"></a><a name="snatports"></a> Ajustar la asignación de puertos SNAT

Puede usar reglas de salida para ajustar la [asignación de puertos SNAT automática según el tamaño del grupo de back-end](load-balancer-outbound-connections.md#preallocatedports) y asignar más o menos que la asignación automática de puertos SNAT.

Use el siguiente parámetro para asignar 10 000 puertos SNAT por VM (configuración IP del NIC).
 

          "allocatedOutboundPorts": 10000

Cada dirección IP pública de todos los servidores front-end de una regla de salida aporta hasta 64 000 puertos efímeros para su uso como puertos SNAT.  Load Balancer asigna puertos SNAT en múltiplos de 8. Si proporciona un valor que no se puede dividir por 8, se rechaza la operación de configuración.  Si intenta asignar más puertos SNAT de los disponibles en función del número de direcciones IP públicas, se rechaza la operación de configuración.  Por ejemplo, si asigna 10 000 puertos por VM y 7 VM de un grupo de back-end tienen que compartir una única dirección IP pública, la configuración se rechaza (7 x 10 000 puertos SNAT > 64 000 puertos SNAT).  Puede agregar más direcciones IP públicas al front-end de la regla de salida para habilitar el escenario.

Puede volver a la [asignación de puertos SNAT automática basada en el tamaño del grupo de back-end](load-balancer-outbound-connections.md#preallocatedports). Para hacerlo, especifique 0 como número de puertos. En ese caso, las primeras 50 instancias de máquina virtual obtendrán 1024 puertos, las instancias de máquina virtual 51 a 100 obtendrán 512 y así sucesivamente según la tabla.

### <a name="control-outbound-flow-idle-timeout"></a><a name="idletimeout"></a> Tiempo de espera de inactividad de flujo de salida de control

Las reglas de salida proporcionan un parámetro de configuración para controlar el tiempo de espera de inactividad del flujo de salida y que se corresponda con las necesidades de su aplicación.  El tiempo de espera de inactividad de salida predeterminado es de 4 minutos.  El parámetro acepta un valor de 4 a 120 para especificar el número de minutos del tiempo de espera de inactividad para los flujos que coincidan con esta regla concreta.

Use el parámetro siguiente para establecer el tiempo de espera de inactividad de salida como 1 hora:

          "idleTimeoutInMinutes": 60

### <a name="enable-tcp-reset-on-idle-timeout"></a><a name="tcprst"></a> <a name="tcpreset"></a> Habilitar el restablecimiento de TCP al agotarse el tiempo de espera de inactividad

De forma predeterminada, Load Balancer anula el flujo en silencio cuando se alcanza el tiempo de espera de inactividad de salida.  Con el parámetro enableTCPReset, puede habilitar un comportamiento de la aplicación más predecible y controlar si se debe enviar un restablecimiento de TCP bidireccional (TCP RST) al agotarse del tiempo de espera de inactividad de salida. 

Para habilitar el restablecimiento de TCP en una regla de salida, use el siguiente parámetro:

           "enableTcpReset": true

Revise [Restablecimiento de TCP al agotarse el tiempo de espera de inactividad](https://aka.ms/lbtcpreset) para obtener detalles como la disponibilidad por regiones.

### <a name="support-both-tcp-and-udp-transport-protocols-with-a-single-rule"></a><a name="proto"></a> Compatibilidad con los protocolos de transporte TCP y UDP con una sola regla

Probablemente, querrá usar "All" para el protocolo de transporte de la regla de salida, pero también puede aplicar la regla de salida a un protocolo de transporte específico si necesita hacerlo.

Para definir el protocolo TCP y UDP, use el siguiente parámetro:

          "protocol": "All"

### <a name="disable-outbound-nat-for-a-load-balancing-rule"></a><a name="disablesnat"></a> Deshabilitar NAT de salida para una regla de equilibrio de carga

Como se ha indicado anteriormente, las reglas de equilibrio de carga proporcionan programación automática de NAT de salida. Sin embargo, algunos escenarios requieren que deshabilite la programación automática de NAT de salida por parte de la regla de equilibrio de carga para permitirle controlar o refinar el comportamiento, o bien mejoran al hacerlo.  Las reglas de salida tienen escenarios donde es importante detener la programación de NAT de salida automática.

Puede usar este parámetro de dos maneras:
- Supresión opcional del uso de la dirección IP de entrada para NAT de salida.  Las reglas de salida son incrementales para las reglas de equilibrio de carga y, con parámetro definido, la regla de salida tiene el control.
  
- Ajuste los parámetros de NAT de salida de una dirección IP que se usa para el tráfico de entrada y salida al mismo tiempo.  La programación de NAT de salida automática debe deshabilitarse para permitir que una regla de salida tome el control.  Por ejemplo, para cambiar la asignación de puertos SNAT de una dirección que también se usa para la entrada, este parámetro debe establecerse en true.  Si intenta usar una regla de salida para volver a definir los parámetros de una dirección IP que también se usa para la entrada y no ha liberado la programación de NAT saliente de la regla de equilibrio de carga, se producirá un error en la operación para configurar una regla de salida.

>[!IMPORTANT]
> La máquina virtual no tendrá conectividad de salida si se establece este parámetro en true y no tiene una regla de salida (o [escenario de IP pública de nivel de instancia](load-balancer-outbound-connections.md#ilpip) para definir la conectividad de salida.  Algunas operaciones de la VM o la aplicación pueden depender de la conectividad de salida disponible. Asegúrese de comprender las dependencias del escenario y de haber considerado el impacto de hacer este cambio.

Puede deshabilitar la SNAT de salida en la regla de equilibrio de carga con este parámetro de configuración:

```json
      "loadBalancingRules": [
        {
          "disableOutboundSnat": true
        }
      ]
```

El valor predeterminado del parámetro disableOutboundSNAT es false, lo que significa que la regla de equilibrio de carga **sí** que proporciona NAT de salida automática como imagen reflejada de la configuración de la regla de equilibrio de carga.  

Si establece disableOutboundSnat en true en la regla de equilibrio de carga, la regla de equilibrio de carga cede el control de la programación de NAT de salida, que, en caso contrario, es automática.  La SNAT de salida como resultado de la regla de equilibrio de carga está deshabilitada.

### <a name="reuse-existing-or-define-new-backend-pools"></a>Reutilizar los grupos de back-end existentes o definir otros nuevos

Las reglas de salida no introducen ningún concepto nuevo para definir el grupo de VM al que se debería aplicar la regla.  En su lugar, reutilizan el concepto de grupo de back-end, que también se usa para las reglas de equilibrio de carga. Puede usar esto para simplificar la configuración reutilizando un grupo de back-end existente o creando uno específicamente para una regla de salida.

## <a name="scenarios"></a>Escenarios

### <a name="groom-outbound-connections-to-a-specific-set-of-public-ip-addresses"></a><a name="groom"></a> Limpiar las conexiones de salida en un conjunto específico de direcciones IP públicas

Puede usar una regla de salida para limpiar las conexiones de salida para que parezca que se originan en un conjunto concreto de direcciones IP públicas a fin de facilitar los escenarios de creación de listas de permitidos.  Esta dirección IP pública de origen puede ser la misma que usa una regla de equilibrio de carga u otro conjunto de direcciones IP públicas distinto del que usa la regla de equilibrio de carga.  

1. Cree un [prefijo IP público](https://aka.ms/lbpublicipprefix) (o direcciones IP públicas de un prefijo de IP pública)
2. Creación de una instancia de Load Balancer Estándar pública
3. Cree servidores front-end que hagan referencia al prefijo de IP pública (o direcciones IP públicas) que quiera usar
4. Reutilice un grupo de back-end o cree uno nuevo y coloque las VM en un grupo de back-end de la instancia pública de Load Balancer
5. Configure una regla de salida en la instancia pública de Load Balancer para programar una NAT de salid para estas VM mediante front-end
   
Si no quiere usar la regla de equilibrio de carga para la salida, debe [deshabilitar la SNAT de salida](#disablesnat) en la regla de equilibrio de carga.

### <a name="modify-snat-port-allocation"></a><a name="modifysnat"></a> Modificar la asignación de puertos SNAT

Puede utilizar reglas de salida para ajustar la [asignación de puertos de SNAT automática basada en el tamaño del grupo de back-end](load-balancer-outbound-connections.md#preallocatedports).

Por ejemplo, si tiene dos máquinas virtuales que comparten una única dirección IP pública de NAT de salida, puede aumentar el número de puertos SNAT asignados a de los 1024 puertos predeterminados si experimenta agotamiento de SNAT. Cada dirección IP pública puede contribuir hasta a 64 000 puertos efímeros.  Si configura una regla de salida con un único front-end de dirección IP pública, puede distribuir un total de 64 000 puertos SNAT a VM del grupo de back-end.  Para dos VM, se puede asignar un máximo de 32 000 puertos SNAT con una regla de salida (2 x 32 000 = 64 000).

Consulte las [conexiones de salida](load-balancer-outbound-connections.md) y los detalles sobre cómo se asignan y usan los puertos [SNAT](load-balancer-outbound-connections.md#snat).

### <a name="enable-outbound-only"></a><a name="outboundonly"></a> Habilitar solo la salida

Puede usar una instancia pública de Standard Load Balancer para proporcionar NAT de salida para un grupo de VM. En este escenario, puede usar una sola regla de salida, sin necesidad de reglas adicionales.

#### <a name="outbound-nat-for-vms-only-no-inbound"></a>NAT de salida solo para VM (no de entrada)

Defina una instancia pública de Standard Load Balancer, coloque las VM en el grupo de back-end y configure una regla de salida para programar la NAT de salida y limpiar las conexiones de salida para que se originen desde una dirección IP pública específica. También puede usar un prefijo IP público para simplificar la inclusión del origen de las conexiones de salida en una lista blanca.

1. Cree una instancia pública de Standard Load Balancer.
2. Cree un grupo de back-end y coloque las VM en un grupo de back-end de la instancia pública de Load Balancer.
3. Configure una regla de salida en la instancia pública de Load Balancer para programar una NAT de salid para estas VM.

#### <a name="outbound-nat-for-internal-standard-load-balancer-scenarios"></a>NAT de salida para escenarios internos de Standard Load Balancer

Cuando se usa una instancia de Standard Load Balancer, la NAT de salida no está disponible hasta que la conectividad de salida se haya declarado explícitamente. Puede definir la conectividad de salida utilizando una regla de salida para crear conectividad de salida para las máquinas virtuales detrás de una instancia interna de Standard Load Balancer con los pasos siguientes:

1. Cree una instancia pública de Standard Load Balancer.
2. Cree un grupo de back-end y coloque las máquinas virtuales en un grupo de back-end de la instancia pública de Load Balancer además de la instancia interna de Load Balancer.
3. Configure una regla de salida en la instancia pública de Load Balancer para programar una NAT de salid para estas VM.

#### <a name="enable-both-tcp--udp-protocols-for-outbound-nat-with-a-public-standard-load-balancer"></a>Habilitar los protocolos TCP y UDP para NAT de salida con una instancia pública de Standard Load Balancer

- Cuando se usa una instancia pública de Standard Load Balancer, la programación de NAT de salida automática proporcionada coincide con el protocolo de transporte de la regla de equilibrio de carga.  

   1. Deshabilite la SNAT de salida en la regla de equilibrio de carga.
   2. Configure una regla de salida en la misma instancia de Load Balancer.
   3. Vuelva a usar el grupo de back-end que usó para sus VM.
   4. Especifique "protocol": "All" como parte de la regla de salida.

- Si solo se usan reglas de NAT entrantes, no se proporciona ninguna NAT de salida.

   1. Coloque las VM en un grupo de back-end.
   2. Defina una o varias configuraciones de IP de front-end con direcciones IP públicas o un prefijo de IP pública.
   3. Configure una regla de salida en la misma instancia de Load Balancer.
   4. Especifique "protocol": "All" como parte de la regla de salida.

## <a name="limitations"></a>Limitaciones

- El número máximo de puertos efímeros posibles por dirección IP de front-end es 64 000.
- El intervalo de tiempo de espera de inactividad de salida que puede configurar oscila entre 4 y 120 minutos (de 240 a 7200 segundos).
- Load Balancer no es compatible con ICMP para NAT de salida.
- Las reglas de salida solo se pueden aplicar a la configuración de IP principal de una NIC.  Se admiten varias NIC.

## <a name="next-steps"></a>Pasos siguientes

- Más información acerca de cómo usar [Load Balancer para conexiones salientes](load-balancer-outbound-connections.md).
- Más información sobre [Standard Load Balancer](load-balancer-standard-overview.md).
- Obtenga información sobre el [restablecimiento de TCP bidireccional al agotarse el tiempo de espera de inactividad](load-balancer-tcp-reset.md).
- [Configurar reglas de salida con la CLI de Azure 2.0](configure-load-balancer-outbound-cli.md).

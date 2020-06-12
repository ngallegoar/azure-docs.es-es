---
title: Conexiones salientes en Azure
titleSuffix: Azure Load Balancer
description: En este artículo se explica cómo Azure permite que las máquinas virtuales puedan comunicarse con servicios de la red Internet pública.
services: load-balancer
documentationcenter: na
author: asudbring
ms.service: load-balancer
ms.custom: seodec18
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/07/2019
ms.author: allensu
ms.openlocfilehash: 0e46905ad280cd76d66befb1156e428b23f35664
ms.sourcegitcommit: f1132db5c8ad5a0f2193d751e341e1cd31989854
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2020
ms.locfileid: "84235640"
---
# <a name="outbound-connections-in-azure"></a>Conexiones salientes en Azure

Azure Load Balancer proporciona conectividad saliente para implementaciones de cliente gracias a varios mecanismos diferentes. En este artículo se describen cuáles son los escenarios, cuándo se aplican, cómo funcionan y cómo administrarlos. Si tiene problemas con la conectividad saliente mediante Azure Load Balancer, consulte la [guía de solución de problemas para conexiones salientes] (../load-balancer/troubleshoot-outbound-connection.md).

>[!NOTE] 
>Este artículo trata solo sobre las implementaciones de Resource Manager. Revise [Conexiones salientes (clásico)](load-balancer-outbound-connections-classic.md) para todos los escenarios de implementación clásicos de Azure.

En una implementación de Azure es posible comunicarse con puntos de conexión externos a Azure en el espacio de direcciones IP públicas. Cuando una instancia inicia un flujo de salida a un destino del espacio de direcciones IP públicas, Azure asigna dinámicamente la dirección IP privada a una dirección IP pública. Una vez creada esta asignación, el tráfico de retorno de este flujo originado de salida también puede comunicarse con la dirección IP privada donde se originó el flujo.

Azure usa la traducción de direcciones de red de origen (SNAT) para realizar esta función. Cuando se enmascaran varias direcciones IP privadas detrás de una única dirección IP pública, Azure usa la [traducción de direcciones de puerto (PAT)](#pat) para enmascarar las direcciones IP privadas. En PAT se usan puertos efímeros y se [asignan previamente](#preallocatedports) en función del tamaño del grupo.

Son varios los [escenarios de salida](#scenarios). Puede combinar estos escenarios según sea necesario. Revíselos cuidadosamente para comprender las funcionalidades, las restricciones y los patrones que se aplican a su modelo de implementación y escenario de aplicación. Revise las instrucciones para [administrar estos escenarios](../load-balancer/troubleshoot-outbound-connection.md#snatexhaust).

>[!IMPORTANT] 
>Standard Load Balancer y la IP pública estándar presentan capacidades nuevas y comportamientos diferentes en la conectividad saliente.  No son lo mismo que las SKU de nivel Básico.  Si quiere conectividad saliente al trabajar con las SKU de nivel Estándar, debe definirlas con las direcciones IP públicas estándar o con la instancia pública de Load Balancer estándar.  Esto incluye establecer conectividad saliente cuando se usa una instancia interna de Standard Load Balancer.  Se recomienda que use siempre las reglas de salida en una instancia pública de Load Balancer estándar.  El [escenario 3](#defaultsnat) no está disponible con la SKU de nivel Estándar.  Esto significa que cuando se usa una instancia interna de Standard Load Balancer, es necesario seguir los pasos para establecer la conectividad saliente para las máquinas virtuales en el grupo de back-end si se quiere contar con conectividad saliente.  En el contexto de la conectividad saliente, una máquina virtual independiente, todas las máquinas virtuales en un conjunto de disponibilidad y todas las instancias de VMSS se comportan como un grupo. Es decir que, si una máquina virtual en un conjunto de disponibilidad está asociada con una SKU de nivel Estándar, todas las instancias de máquina virtual dentro de dicho conjunto de disponibilidad ahora siguen las mismas reglas que están asociadas con la SKU de nivel Estándar, incluso si una instancia individual no está directamente asociada con ella. Este comportamiento también se observa en el caso de una máquina virtual independiente con varias tarjetas de interfaz de red conectadas a un equilibrador de carga. Si una NIC se agrega como independiente, tendrá el mismo comportamiento. Revise cuidadosamente todo el documento para entender los conceptos generales, consulte [Standard Load Balancer](load-balancer-standard-overview.md) para conocer las diferencias entre las SKU y consulte las [reglas de salida](load-balancer-outbound-rules-overview.md).  Al usar las reglas de salida, obtiene un control avanzado de todos los aspectos de la conectividad saliente.

## <a name="scenario-overview"></a><a name="scenarios"></a>Información general de los escenarios

Cuando se usan [recursos de Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview), se definen explícitamente Azure Load Balancer y los recursos relacionados.  Actualmente, Azure proporciona tres métodos diferentes para lograr la conectividad saliente con los recursos de Azure Resource Manager. 

| SKU | Escenario | Método | Protocolos IP | Descripción |
| --- | --- | --- | --- | --- |
| Nivel Estándar o Básico | [1. Máquina virtual con una dirección IP pública en el nivel de instancia (con o sin Load Balancer)](#ilpip) | SNAT, no se usa el enmascaramiento de puertos | TCP, UDP, ICMP, ESP | Azure usa la dirección IP pública asignada a la configuración IP de NIC de. la instancia. La instancia tiene disponibles todos los puertos efímeros. Cuando se usa Standard Load Balancer, no se admiten las [reglas de salida](load-balancer-outbound-rules-overview.md) si se asigna una dirección IP pública a la máquina virtual. |
| Nivel Estándar o Básico | [2. Load Balancer público asociado a una máquina virtual (ninguna dirección IP pública en la instancia)](#lb) | SNAT con enmascaramiento de puertos (PAT) mediante los servidores front-end de Load Balancer | TCP, UDP |Azure comparte la dirección IP pública de los servidores front-end de Load Balancer público con varias direcciones IP privadas. Azure usa puertos efímeros de los servidores front-end para PAT. Cuando se usa Standard Load Balancer, también deben usarse las [reglas de salida](load-balancer-outbound-rules-overview.md) para definir explícitamente la conectividad saliente. |
| Ninguna o nivel Básico | [3. Máquina virtual independiente (sin Load Balancer, sin dirección IP pública)](#defaultsnat) | SNAT con enmascaramiento de puertos (PAT) | TCP, UDP | Azure designa automáticamente una dirección IP pública para SNAT, la comparte con varias direcciones IP privadas del conjunto de disponibilidad y usa los puertos efímeros de esta dirección IP pública. Este es un escenario de reserva para los escenarios anteriores. No es aconsejable si necesita visibilidad y control. |

Si no quiere que una máquina virtual se comunique con puntos de conexión situados fuera de Azure en el espacio de direcciones IP públicas, puede usar grupos de seguridad de red (NSG) para bloquear el acceso según sea necesario. En la sección [Impedir la conectividad saliente](#preventoutbound) se proporciona información sobre los grupos de seguridad de red. Las instrucciones sobre cómo diseñar, implementar y administrar una red virtual sin acceso de salida escapan del ámbito de este artículo.

### <a name="scenario-1-vm-with-public-ip-address"></a><a name="ilpip"></a>Escenario 1: Máquina virtual con dirección IP pública

En este escenario, la máquina virtual tiene una dirección IP pública asignada. En lo que se refiere a las conexiones salientes, no importa si la máquina virtual tiene la carga equilibrada o no. Este escenario tiene prioridad sobre las demás. Cuando se usa una dirección IP pública, la máquina virtual usa la dirección IP pública para todos los flujos salientes.  

Una dirección IP pública asignada a una máquina virtual es una relación 1:1 (en lugar de 1:muchos) y se implementa como NAT de 1:1 sin estado.  No se usa el enmascaramiento de puertos (PAT) y la máquina dispone de todos los puertos efímeros para su uso.

Si la aplicación inicia muchos flujos de salida y experimenta el agotamiento de puertos SNAT, considere la posibilidad de asignar una [dirección IP pública para mitigar las restricciones de SNAT](../load-balancer/troubleshoot-outbound-connection.md#assignilpip). Revise la sección [Administración de agotamiento de SNAT](../load-balancer/troubleshoot-outbound-connection.md#snatexhaust) en su totalidad.

### <a name="scenario-2-load-balanced-vm-without-a-public-ip-address"></a><a name="lb"></a>Escenario 2: máquina virtual de carga equilibrada sin dirección IP pública

En este escenario, la máquina virtual forma parte de un grupo de servidores back-end de Load Balancer. La máquina virtual no tiene ninguna dirección IP pública asignada. El recurso Load Balancer debe configurarse con una regla de equilibrador de carga que cree un vínculo entre el front-end de dirección IP pública y el grupo de back-end.

Si no lleva a cabo esta configuración de reglas, el comportamiento es tal y como se describe en la sección para [Máquina virtual independiente sin dirección IP pública](#defaultsnat). No es necesario que la regla tenga un agente de escucha en funcionamiento en el grupo de servidores back-end o que el sondeo de mantenimiento sea correcto.

Cuando la máquina virtual de carga equilibrada crea un flujo de salida, Azure traduce la dirección IP de origen privado del flujo de salida en una dirección IP pública del front-end público de Load Balancer. Azure usa SNAT para realizar esta función. También usa [PAT](#pat) para enmascarar varias direcciones IP privadas detrás de una dirección IP pública. 

Para distinguir los flujos individuales que se originan en la máquina virtual, se emplean puertos efímeros del servidor front-end de la dirección IP pública de Load Balancer. Cuando se crean flujos de salida, SNAT usa dinámicamente los [puertos efímeros asignados previamente](#preallocatedports). En este contexto, los puertos efímeros usados para SNAT se conocen como puertos SNAT.

Los puertos SNAT se asignan previamente, como se describe en la sección [Descripción de SNAT y PAT](#snat). Estos puertos son un recurso finito que puede agotarse. Es importante entender cómo se [consumen](#pat). Para comprender cómo realizar el diseño de cara a este consumo y solucionar los posibles problemas, revise la sección [Administración de agotamiento de SNAT](../load-balancer/troubleshoot-outbound-connection.md#snatexhaust).

Cuando hay [varias direcciones IP públicas asociadas a Load Balancer básico](load-balancer-multivip-overview.md), cualquiera de estas direcciones IP públicas son candidatas para los flujos de salida y se selecciona una de forma aleatoria.  

Para supervisar el mantenimiento de las conexiones salientes con Load Balancer básico, puede usar los [logs de Azure Monitor para Load Balancer](load-balancer-monitor-log.md) y [registros de eventos de alerta](load-balancer-monitor-log.md#alert-event-log).

### <a name="scenario-3-standalone-vm-without-a-public-ip-address"></a><a name="defaultsnat"></a>Escenario 3: Máquina virtual independiente sin dirección IP pública

En este escenario, la máquina virtual no forma parte de un grupo público de un grupo de Load Balancer (ni tampoco de un grupo de Standard Load Balancer interno) y no tiene asignada una dirección IP pública. Cuando la máquina virtual crea un flujo de salida, Azure traduce la dirección IP de origen privado del flujo de salida en una dirección IP de origen público. La dirección IP pública usada para este flujo de salida no es configurable y no cuenta para el límite de recursos de IP pública de la suscripción. Esta dirección IP pública no le pertenece y no se puede reservar. Si vuelve a implementar la máquina virtual, el conjunto de disponibilidad o el conjunto de escalado de máquinas virtuales, esta dirección IP pública se publicará y se solicitará una nueva dirección IP pública. No use este escenario para direcciones IP permitidas. En su lugar, utilice uno de los otros dos escenarios en el que declare explícitamente el escenario de salida y la dirección IP pública que se utilizará para la conectividad de salida.

>[!IMPORTANT] 
>Este escenario también se aplica cuando __solo__ se asocia una instancia de Load Balancer básico interna. El escenario 3 __no está disponible__ cuando se asocia una instancia de Load Balancer estándar interna a una máquina virtual.  Debe crear explícitamente el [escenario 1](#ilpip) o el [escenario 2](#lb) además de usar una instancia de Load Balancer estándar interna.

Para realizar esta función, Azure usa SNAT con enmascaramiento de puertos ([PAT](#pat)). Este escenario es similar al [escenario 2](#lb), excepto en que no hay ningún control sobre la dirección IP usada. Se trata de un escenario de reserva para cuando no existan los escenarios 1 y 2. No se recomienda este escenario si desea tener control sobre la dirección de salida. Si las conexiones salientes son una parte fundamental de la aplicación, debe elegir otro escenario.

Los puertos SNAT se asignan previamente, como se describe en la sección [Descripción de SNAT y PAT](#snat).  El número de máquinas virtuales que comparten un conjunto de disponibilidad determina qué nivel preasignación se aplica.  Una máquina virtual independiente sin un conjunto de disponibilidad es en efecto un grupo de uno, cuando se trata de determinar la preasignación (1024 puertos SNAT). Los puertos SNAT son un recurso finito que puede agotarse. Es importante entender cómo se [consumen](#pat). Para comprender cómo realizar el diseño de cara a este consumo y solucionar los posibles problemas, revise la sección [Administración de agotamiento de SNAT](../load-balancer/troubleshoot-outbound-connection.md#snatexhaust).

### <a name="multiple-combined-scenarios"></a><a name="combinations"></a>Combinación de varios escenarios

Los escenarios descritos en las secciones anteriores se pueden combinar para conseguir un resultado concreto. Cuando hay varios escenarios, se aplica un orden de prioridad: el [escenario 1](#ilpip) tiene prioridad sobre el [escenario 2](#lb) y el [3](#defaultsnat). [El escenario 2](#lb) invalida el [escenario 3](#defaultsnat).

Un ejemplo sería una implementación de Azure Resource Manager donde la aplicación depende en gran medida de las conexiones salientes a un número limitado de destinos, peor también recibe flujos de entrada a través de un servidor front-end de Load Balancer. En este caso, puede combinar los escenarios 1 y 2 como ayuda. Para ver patrones adicionales, revise [Administración de agotamiento de SNAT](../load-balancer/troubleshoot-outbound-connection.md#snatexhaust).

### <a name="multiple-frontends-for-outbound-flows"></a><a name="multife"></a> Varios servidores front-end para flujos de salida

#### <a name="standard-load-balancer"></a>Standard Load Balancer

El servicio Standard Load Balancer usa todos los candidatos para los flujos de salida al mismo tiempo que están presentes [varios servidores front-end de direcciones IP (públicos)](load-balancer-multivip-overview.md). Cada servidor front-end multiplica el número de puertos SNAT preasignados disponibles si se habilita una regla de equilibrio de carga para las conexiones salientes.

Puede optar por suprimir una dirección IP de servidor front-end para que no se use en las conexiones salientes con una nueva opción de regla de equilibrio de carga:

```json    
      "loadBalancingRules": [
        {
          "disableOutboundSnat": false
        }
      ]
```

Normalmente, la opción `disableOutboundSnat` tiene como valor predeterminado _false_, lo que significa que esta regla programa SNAT saliente para las máquinas virtuales asociadas en el grupo de servidores back-end de la regla de equilibrio de carga. `disableOutboundSnat` puede cambiarse a _true_ para impedir que Load Balancer use la dirección IP de servidor front-end asociada en las conexiones salientes para la máquina virtual en el grupo de servidores back-end de esta regla de equilibrio de carga.  Asimismo, puede designar una dirección IP específica para los flujos de salida, como se describe también en [Varios escenarios combinados](#combinations).

#### <a name="load-balancer-basic"></a>Load Balancer básico

Load Balancer básico elige un único servidor front-end para usarlo con los flujos de salida cuando hay [varios servidores front-end de direcciones IP (públicas)](load-balancer-multivip-overview.md) candidatos para los flujos de salida. Esta selección no es configurable y el algoritmo de selección se debe considerar aleatorio. Puede designar una dirección IP específica para los flujos de salida, como se describe en [Combinación de varios escenarios](#combinations).

### <a name="availability-zones"></a><a name="az"></a>Zonas de disponibilidad

Cuando se usa [Load Balancer estándar con zonas de disponibilidad](load-balancer-standard-availability-zones.md), los servidores front-end con redundancia de zona pueden proporcionar conexiones SNAT salientes con redundancia de zona y la programación de SNAT sobrevive a los errores en la zona.  Cuando se utilizan servidores front-end de zona, las conexiones SNAT salientes comparten su destino con la zona a la que pertenecen.

## <a name="understanding-snat-and-pat"></a><a name="snat"></a>Descripción de SNAT y PAT

### <a name="port-masquerading-snat-pat"></a><a name="pat"></a>SNAT de enmascaramiento de puertos (PAT)

Cuando un recurso de Load Balancer público está asociado con instancias de máquina virtual, que no tienen direcciones IP públicas dedicadas, se reescribe cada origen de conexión saliente. El origen se reescribe del espacio de direcciones IP privadas de la red virtual a la dirección IP pública de servidor front-end del equilibrador de carga. En el espacio de direcciones IP públicas, la tupla de cinco elementos del flujo (dirección IP de origen, puerto de origen, protocolo de transporte IP, dirección IP de destino, puerto de destino) debe ser única. SNAT de enmascaramiento de puertos se puede usar con los protocolos IP UDP o TCP.

Para conseguir esto, se usan puertos efímeros (puertos SNAT) después de volver a escribir la dirección IP de origen privada, dado que varios flujos se originan desde una única dirección IP pública. El algoritmo SNAT de enmascaramiento de puertos asigna los puertos SNAT de forma diferente para UDP que para TCP.

#### <a name="tcp-snat-ports"></a><a name="tcp"></a>Puertos TCP SNAT

Se consume un puerto SNAT por cada flujo a una combinación única de dirección IP y puerto de destino. En el caso de varios flujos TCP a la misma dirección IP, puerto y protocolo de destino, cada flujo TCP consume un solo puerto SNAT. Esto garantiza que los flujos son únicos si se han originado desde la misma dirección IP pública y se dirigen a la misma dirección IP, puerto y protocolo de destino. 

Si hay varios flujos, cada uno de ellos dirigido a una dirección IP, un puerto y un protocolo de destino diferentes, se comparte un solo puerto SNAT. La dirección IP, el puerto y el protocolo de destino hacen que los flujos sean únicos, sin necesidad de usar puertos de origen adicionales para distinguirlos en el espacio de direcciones IP públicas.

#### <a name="udp-snat-ports"></a><a name="udp"></a> Puertos UDP SNAT

Los puertos UDP SNAT se administran mediante un algoritmo diferente que los puertos TCP SNAT.  Load Balancer utiliza un algoritmo que se conoce como "NAT de cono restringido de puertos" para UDP.  Se consume un puerto SNAT por cada flujo, con independencia de la combinación de dirección IP y puerto de destino.

#### <a name="snat-port-reuse"></a>Reutilización del puerto SNAT

Una vez que se ha liberado un puerto, está disponible para reutilizarse según sea necesario.  Puede pensar en los puertos SNAT como una secuencia del menor al mayor disponibles para un escenario determinado. El primer puerto SNAT disponible se usa para las nuevas conexiones. 
 
#### <a name="exhaustion"></a>Agotamiento

Cuando se agotan los recursos de los puertos SNAT, los flujos de salida generan errores hasta que los flujos ya existentes liberan puertos SNAT. Load Balancer reclama puertos SNAT cuando el flujo se cierra y usa un [tiempo de espera de inactividad de 4 minutos](../load-balancer/troubleshoot-outbound-connection.md#idletimeout) para reclamar puertos SNAT de los flujos inactivos.

Los puertos UDP SNAT suelen agotarse mucho más rápidamente que los puertos TCP SNAT debido a la diferencia en el algoritmo utilizado. Debe tener presente esta diferencia al realizar pruebas de diseño y escalado.

Revise la sección [Administración de agotamiento de SNAT](../load-balancer/troubleshoot-outbound-connection.md#snatexhaust) para conocer los patrones de mitigación de las condiciones que suelen provocar el agotamiento de puertos SNAT.

### <a name="ephemeral-port-preallocation-for-port-masquerading-snat-pat"></a><a name="preallocatedports"></a>Preasignación de puertos efímeros para SNAT de enmascaramiento de puertos (PAT)

Para determinar el número de puertos SNAT asignados previamente disponibles, Azure usa un algoritmo basado en el tamaño del grupo de servidores back-end cuando se usa SNAT de enmascaramiento de puertos ([PAT](#pat)). Los puertos SNAT son puertos efímeros disponibles para una determinada dirección IP de origen pública. Para cada dirección IP pública asociada a un equilibrador de carga, hay 64 000 puertos disponibles como puertos SNAT para cada protocolo de transporte IP.

Está preasignado el mismo número de puertos SNAT para UDP y TCP respectivamente, y se consumen independiente para cada protocolo de transporte de IP.  Aunque la utilización del puerto SNAT varía en función de que el flujo sea UDP o TCP.

>[!IMPORTANT]
>La programación SNAT de SKU estándar corresponde a cada protocolo de transporte IP y se deriva de la regla del equilibrio de carga.  Si solo existe una regla de equilibrio de carga de TCP, SNAT solo está disponible para TCP. Si tiene solo una regla de equilibrio de carga de TCP y necesita SNAT saliente para UDP, cree una regla de equilibrio de carga desde el mismo front-end al mismo grupo de back-end.  Con esto se desencadenará la programación de SNAT para UDP.  No se necesita una regla de trabajo ni un sondeo de estado.  El SNAT de SKU básico programa siempre SNAT para el protocolo de transporte de IP, con independencia del protocolo de transporte especificado en la regla del equilibrio de carga.

Azure asigna previamente puertos SNAT a la configuración de IP de la NIC de cada máquina virtual. Cuando se agrega una configuración IP al grupo, los puertos SNAT se asignan previamente a esta configuración IP en función del tamaño del grupo de servidores back-end. Cuando se crean flujos de salida, [PAT](#pat) consume dinámicamente (hasta el límite asignado previamente) y libera estos puertos cuando el flujo se cierra o se [agota el tiempo de espera de inactividad](../load-balancer/troubleshoot-outbound-connection.md#idletimeout).

En la tabla siguiente se muestran las asignaciones previas de puertos SNAT para los niveles de tamaño de grupo de servidores back-end:

| Tamaño del grupo (instancias de máquina virtual) | Puertos SNAT asignados previamente por configuración IP|
| --- | --- |
| 1-50 | 1024 |
| 51-100 | 512 |
| 101-200 | 256 |
| 201-400 | 128 |
| 401-800 | 64 |
| 801-1000 | 32 |

>[!NOTE]
> Al usar Standard Load Balancer con [varios servidores front-end](load-balancer-multivip-overview.md), cada dirección IP de servidor front-end multiplica el número de puertos SNAT disponibles en la tabla anterior. Por ejemplo, un grupo de servidores back-end de 50 máquinas virtuales con dos reglas de equilibrio de carga, cada uno con una dirección IP de servidor front-end independiente, usará 2048 (2 x 1024) puertos SNAT por cada regla. Consulte los detalles para [varios servidores front-end](#multife).

Recuerde que el número de puertos SNAT disponibles no equivale directamente al número de flujos. Un solo puerto SNAT se puede reutilizar con varios destinos únicos. Solo se consumen puertos si es necesario que los flujos sean únicos. Para instrucciones sobre diseño y mitigación, consulte [cómo administrar este recurso agotable](../load-balancer/troubleshoot-outbound-connection.md#snatexhaust), así como la sección en la que se describe [PAT](#pat).

Cambiar el tamaño del grupo de servidores back-end puede afectar a algunos de los flujos establecidos. Si el tamaño del grupo de servidores back-end aumenta y pasa al siguiente nivel, la mitad de los puertos SNAT previamente asignados se reclaman durante la transición al siguiente nivel más grande del grupo de servidores back-end. Los flujos que están asociados a un puerto SNAT reclamado agotan el tiempo de espera y deben restablecerse. Si se intenta un nuevo flujo, el flujo se inicia inmediatamente siempre y cuando los puertos asignados previamente estén disponibles.

Si el tamaño del grupo de servidores back-end se reduce y pasa a un nivel inferior, aumenta el número de puertos SNAT disponibles. En este caso, los puertos SNAT asignados existentes y sus flujos respectivos no se ven afectados.

Las asignaciones de puertos SNAT son específicas del protocolo de transporte IP (TCP y UDP se mantienen por separado) y se liberan en las siguientes condiciones:

### <a name="tcp-snat-port-release"></a>Liberación del puerto TCP SNAT

- Si tanto el cliente como el servidor envían FIN/ACK, el puerto SNAT se liberará después de 240 segundos.
- Si se ve un RST, el puerto SNAT se liberará después de 15 segundos.
- Si se agotó el tiempo de expiración, el puerto se libera.

### <a name="udp-snat-port-release"></a>Liberación del puerto UDP SNAT

- Si se agotó el tiempo de expiración, el puerto se libera.

## <a name="discovering-the-public-ip-that-a-vm-uses"></a><a name="discoveroutbound"></a>Detección de la dirección IP pública que usa una máquina virtual
Hay muchas maneras de determinar la dirección IP de origen público de una conexión saliente. OpenDNS proporciona un servicio que puede mostrar la dirección IP pública de la máquina virtual. 

Mediante el comando nslookup, puede enviar una consulta DNS del nombre myip.opendns.com para la resolución de OpenDNS. El servicio devuelve la dirección IP de origen que se usó para enviar la consulta. Si ejecuta la siguiente consulta desde la máquina virtual, la respuesta es la dirección IP pública que se usa para esa máquina virtual:

    nslookup myip.opendns.com resolver1.opendns.com

## <a name="preventing-outbound-connectivity"></a><a name="preventoutbound"></a>Impedir la conectividad saliente
En ocasiones, no es aconsejable permitir que una máquina virtual cree un flujo de salida. O bien, puede que exista un requisito para administrar a qué destinos se puede llegar con los flujos de salida o qué destinos pueden comenzar los flujos de entrada. En este caso, puede usar los [grupos de seguridad de red](../virtual-network/security-overview.md) para administrar los destinos a los que puede llegar la máquina virtual. También puede usar los NSG para administrar qué destino público puede iniciar los flujos de entrada.

Cuando aplique un grupo de seguridad de red a una máquina virtual de carga equilibrada, preste atención a las [etiquetas de servicio](../virtual-network/security-overview.md#service-tags) y a las [reglas de seguridad predeterminadas](../virtual-network/security-overview.md#default-security-rules). Debe asegurarse de que la máquina virtual puede recibir solicitudes de sondeo de mantenimiento desde Azure Load Balancer. 

Si un grupo de seguridad de red bloquea las solicitudes de sondeo de mantenimiento de la etiqueta predeterminada AZURE_LOADBALANCER, se producirá un error en el sondeo de mantenimiento de la máquina virtual y esta se marca como inactiva. Load Balancer dejará de enviar nuevos flujos a esa máquina virtual.

## <a name="connections-to-azure-storage-in-the-same-region"></a>Conexiones a Azure Storage en la misma región

La conectividad saliente a través de los escenarios anteriores no es necesaria para conectarse al almacenamiento en la misma región que la máquina virtual. Si no quiere que pase esto, use grupos de seguridad de red (NSG) como se explicó anteriormente. Para la conectividad con el almacenamiento en otras regiones, se requiere conectividad de salida. Tenga en cuenta que, al conectarse al almacenamiento desde una máquina virtual en la misma región, la dirección IP de origen en los registros de diagnóstico de almacenamiento será una dirección de proveedor interna y no la dirección IP pública de la máquina virtual. Si desea restringir el acceso a la cuenta de almacenamiento a las máquinas virtuales de una o varias subredes de Virtual Network en la misma región, use [Virtual Network los puntos de conexión de servicio](../virtual-network/virtual-network-service-endpoints-overview.md) y no la dirección IP pública al configurar el firewall de la cuenta de almacenamiento. Una vez configurados los puntos de conexión de servicio, verá la dirección IP privada de su instancia de Virtual Network en los registros de diagnóstico de almacenamiento y no la dirección interna del proveedor.

## <a name="azure-load-balancer-outbound-rules"></a><a name="outboundrules"></a>Reglas de salida de Azure Load Balancer

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

### <a name="outbound-rule"></a>Regla de salida

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

#### <a name="scale-outbound-nat-with-multiple-ip-addresses"></a><a name="scale"></a> NAT de salida de escala con varias direcciones IP

Mientras una regla de salida se puede usar con una única dirección IP pública, las reglas de salida alivian la carga de configuración para escalar la NAT de salida. Puede usar varias direcciones IP para planear escenarios de gran escala y puede usar reglas de salida para mitigar los patrones con tendencia a [agotamiento de SNAT](troubleshoot-outbound-connection.md#snatexhaust).  

Cada dirección IP adicional que proporciona un front-end ofrece 64 000 puertos efímeros que Load Balancer puede usar como puertos SNAT. Mientras las reglas de equilibrio de carga o NAT de entrada tienen un único front-end, la regla de salida amplía la noción de front-end y permite varios front-ends por regla.  Con varios front-ends por regla, se multiplica la cantidad de puertos SNAT disponibles con cada dirección IP pública y se pueden admitir escenarios grandes.

Además, puede usar un [prefijo de IP pública](https://aka.ms/lbpublicipprefix) directamente con una regla de salida.  El uso de un prefijo de dirección IP pública proporciona un escalado más fácil y una creación simplificada de listas de permitidos para los flujos que se originan en la implementación de Azure. Puede configurar una configuración de IP de front-end en el recurso de Load Balancer para hacer referencia directamente a un prefijo de dirección IP pública.  Esto permite el control exclusivo de Load Balancer sobre el prefijo IP público y la regla de salida usará automáticamente todas las direcciones IP públicas que contiene el prefijo de IP pública para las conexiones de salida.  Cada una de las direcciones IP dentro del intervalo del prefijo de IP pública proporciona 64 000 puertos efímeros por dirección IP que Load Balancer puede usar como puertos SNAT.   

No se pueden crear recursos de dirección IP pública individuales a partir del prefijo de IP pública cuando se usa esta opción, ya que la regla de salida debe tener control total sobre el prefijo de IP pública.  Si necesita un control más específico, puede crear un recurso de dirección IP pública individual a partir del prefijo de IP pública y asignar varias direcciones IP públicas individualmente al front-end de una regla de salida.

#### <a name="tune-snat-port-allocation"></a><a name="snatports"></a> Ajustar la asignación de puertos SNAT

Puede usar reglas de salida para ajustar la [asignación de puertos SNAT automática según el tamaño del grupo de back-end](load-balancer-outbound-connections.md#preallocatedports) y asignar más o menos que la asignación automática de puertos SNAT.

Use el siguiente parámetro para asignar 10 000 puertos SNAT por VM (configuración IP del NIC).
 

          "allocatedOutboundPorts": 10000

Cada dirección IP pública de todos los servidores front-end de una regla de salida aporta hasta 64 000 puertos efímeros para su uso como puertos SNAT.  Load Balancer asigna puertos SNAT en múltiplos de 8. Si proporciona un valor que no se puede dividir por 8, se rechaza la operación de configuración.  Si intenta asignar más puertos SNAT de los disponibles en función del número de direcciones IP públicas, se rechaza la operación de configuración.  Por ejemplo, si asigna 10 000 puertos por VM y 7 VM de un grupo de back-end tienen que compartir una única dirección IP pública, la configuración se rechaza (7 x 10 000 puertos SNAT > 64 000 puertos SNAT).  Puede agregar más direcciones IP públicas al front-end de la regla de salida para habilitar el escenario.

Puede volver a la [asignación de puertos SNAT automática basada en el tamaño del grupo de back-end](load-balancer-outbound-connections.md#preallocatedports). Para hacerlo, especifique 0 como número de puertos. En ese caso, las primeras 50 instancias de máquina virtual obtendrán 1024 puertos, las instancias de máquina virtual 51 a 100 obtendrán 512 y así sucesivamente según la tabla.

#### <a name="control-outbound-flow-idle-timeout"></a><a name="idletimeout"></a> Tiempo de espera de inactividad de flujo de salida de control

Las reglas de salida proporcionan un parámetro de configuración para controlar el tiempo de espera de inactividad del flujo de salida y que se corresponda con las necesidades de su aplicación.  El tiempo de espera de inactividad de salida predeterminado es de 4 minutos.  El parámetro acepta un valor de 4 a 120 para especificar el número de minutos del tiempo de espera de inactividad para los flujos que coincidan con esta regla concreta.

Use el parámetro siguiente para establecer el tiempo de espera de inactividad de salida como 1 hora:

          "idleTimeoutInMinutes": 60

#### <a name="enable-tcp-reset-on-idle-timeout"></a><a name="tcprst"></a> <a name="tcpreset"></a> Habilitar el restablecimiento de TCP al agotarse el tiempo de espera de inactividad

De forma predeterminada, Load Balancer anula el flujo en silencio cuando se alcanza el tiempo de espera de inactividad de salida.  Con el parámetro enableTCPReset, puede habilitar un comportamiento de la aplicación más predecible y controlar si se debe enviar un restablecimiento de TCP bidireccional (TCP RST) al agotarse del tiempo de espera de inactividad de salida. 

Para habilitar el restablecimiento de TCP en una regla de salida, use el siguiente parámetro:

           "enableTcpReset": true

Revise [Restablecimiento de TCP al agotarse el tiempo de espera de inactividad](https://aka.ms/lbtcpreset) para obtener detalles como la disponibilidad por regiones.

#### <a name="support-both-tcp-and-udp-transport-protocols-with-a-single-rule"></a><a name="proto"></a> Compatibilidad con los protocolos de transporte TCP y UDP con una sola regla

Probablemente, querrá usar "All" para el protocolo de transporte de la regla de salida, pero también puede aplicar la regla de salida a un protocolo de transporte específico si necesita hacerlo.

Para definir el protocolo TCP y UDP, use el siguiente parámetro:

          "protocol": "All"

#### <a name="disable-outbound-nat-for-a-load-balancing-rule"></a><a name="disablesnat"></a> Deshabilitar NAT de salida para una regla de equilibrio de carga

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

#### <a name="reuse-existing-or-define-new-backend-pools"></a>Reutilizar los grupos de back-end existentes o definir otros nuevos

Las reglas de salida no introducen ningún concepto nuevo para definir el grupo de VM al que se debería aplicar la regla.  En su lugar, reutilizan el concepto de grupo de back-end, que también se usa para las reglas de equilibrio de carga. Puede usar esto para simplificar la configuración reutilizando un grupo de back-end existente o creando uno específicamente para una regla de salida.

### <a name="scenarios"></a>Escenarios

#### <a name="groom-outbound-connections-to-a-specific-set-of-public-ip-addresses"></a><a name="groom"></a> Limpiar las conexiones de salida en un conjunto específico de direcciones IP públicas

Puede usar una regla de salida para limpiar las conexiones de salida para que parezca que se originan en un conjunto concreto de direcciones IP públicas a fin de facilitar los escenarios de creación de listas de permitidos.  Esta dirección IP pública de origen puede ser la misma que usa una regla de equilibrio de carga u otro conjunto de direcciones IP públicas distinto del que usa la regla de equilibrio de carga.  

1. Cree un [prefijo IP público](https://aka.ms/lbpublicipprefix) (o direcciones IP públicas de un prefijo de IP pública)
2. Creación de una instancia de Load Balancer Estándar pública
3. Cree servidores front-end que hagan referencia al prefijo de IP pública (o direcciones IP públicas) que quiera usar
4. Reutilice un grupo de back-end o cree uno nuevo y coloque las VM en un grupo de back-end de la instancia pública de Load Balancer
5. Configure una regla de salida en la instancia pública de Load Balancer para programar una NAT de salid para estas VM mediante front-end
   
Si no quiere usar la regla de equilibrio de carga para la salida, debe [deshabilitar la SNAT de salida](#disablesnat) en la regla de equilibrio de carga.

#### <a name="modify-snat-port-allocation"></a><a name="modifysnat"></a> Modificar la asignación de puertos SNAT

Puede utilizar reglas de salida para ajustar la [asignación de puertos de SNAT automática basada en el tamaño del grupo de back-end](load-balancer-outbound-connections.md#preallocatedports).

Por ejemplo, si tiene dos máquinas virtuales que comparten una única dirección IP pública de NAT de salida, puede aumentar el número de puertos SNAT asignados a de los 1024 puertos predeterminados si experimenta agotamiento de SNAT. Cada dirección IP pública puede contribuir hasta a 64 000 puertos efímeros.  Si configura una regla de salida con un único front-end de dirección IP pública, puede distribuir un total de 64 000 puertos SNAT a VM del grupo de back-end.  Para dos VM, se puede asignar un máximo de 32 000 puertos SNAT con una regla de salida (2 x 32 000 = 64 000).

Consulte las [conexiones de salida](load-balancer-outbound-connections.md) y los detalles sobre cómo se asignan y usan los puertos [SNAT](load-balancer-outbound-connections.md#snat).

#### <a name="enable-outbound-only"></a><a name="outboundonly"></a> Habilitar solo la salida

Puede usar una instancia pública de Standard Load Balancer para proporcionar NAT de salida para un grupo de VM. En este escenario, puede usar una sola regla de salida, sin necesidad de reglas adicionales.

##### <a name="outbound-nat-for-vms-only-no-inbound"></a>NAT de salida solo para VM (no de entrada)

Defina una instancia pública de Standard Load Balancer, coloque las VM en el grupo de back-end y configure una regla de salida para programar la NAT de salida y limpiar las conexiones de salida para que se originen desde una dirección IP pública específica. También puede usar un prefijo IP público para simplificar la inclusión del origen de las conexiones de salida en una lista blanca.

1. Cree una instancia pública de Standard Load Balancer.
2. Cree un grupo de back-end y coloque las VM en un grupo de back-end de la instancia pública de Load Balancer.
3. Configure una regla de salida en la instancia pública de Load Balancer para programar una NAT de salid para estas VM.

##### <a name="outbound-nat-for-internal-standard-load-balancer-scenarios"></a>NAT de salida para escenarios internos de Standard Load Balancer

Cuando se usa una instancia de Standard Load Balancer, la NAT de salida no está disponible hasta que la conectividad de salida se haya declarado explícitamente. Puede definir la conectividad de salida utilizando una regla de salida para crear conectividad de salida para las máquinas virtuales detrás de una instancia interna de Standard Load Balancer con los pasos siguientes:

1. Cree una instancia pública de Standard Load Balancer.
2. Cree un grupo de back-end y coloque las máquinas virtuales en un grupo de back-end de la instancia pública de Load Balancer además de la instancia interna de Load Balancer.
3. Configure una regla de salida en la instancia pública de Load Balancer para programar una NAT de salid para estas VM.

##### <a name="enable-both-tcp--udp-protocols-for-outbound-nat-with-a-public-standard-load-balancer"></a>Habilitar los protocolos TCP y UDP para NAT de salida con una instancia pública de Standard Load Balancer

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
- Solo se puede acceder a los roles de trabajo web sin una red virtual y otros servicios de plataforma de Microsoft si se usa un equilibrador de carga estándar debido a un efecto secundario del funcionamiento de los servicios previos a la red virtual y otros servicios de plataforma. No dependa de este efecto secundario, porque el propio servicio o la plataforma subyacente pueden cambiar sin previo aviso. Siempre debe pensar que necesita crear conectividad de salida de manera explícita si lo desea al usar solo un equilibrador de carga estándar interno. El escenario 3 del [SNAT predeterminado](#defaultsnat) que se describe en este artículo no está disponible.

## <a name="next-steps"></a>Pasos siguientes

- Más información acerca de [Load Balancer Estándar](load-balancer-standard-overview.md).
- Obtenga más información sobre las [reglas de salida](load-balancer-outbound-rules-overview.md) para la instancia pública de Load Balancer estándar.
- Más información acerca de [Load Balancer](load-balancer-overview.md).
- Más información sobre los [grupos de seguridad de red](../virtual-network/security-overview.md).
- Aprenda sobre las demás [funcionalidades de red](../networking/networking-overview.md) clave en Azure.

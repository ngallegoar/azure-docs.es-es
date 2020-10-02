---
title: Configuración del portal para Azure Load Balancer
description: Introducción a la configuración del portal para Azure Load Balancer
services: load-balancer
documentationcenter: na
author: asudbring
ms.service: load-balancer
ms.devlang: NA
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/8/2020
ms.author: allensu
ms.openlocfilehash: e1080aea12e70f4312fbee07b063d5a5cfbd1201
ms.sourcegitcommit: 1b320bc7863707a07e98644fbaed9faa0108da97
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/09/2020
ms.locfileid: "89596238"
---
# <a name="azure-load-balancer-portal-settings"></a>Configuración del portal para Azure Load Balancer

Al crear una instancia de Azure Load Balancer, la información de este artículo lo ayudará a obtener más información sobre los valores de configuración y a saber cuál es la configuración adecuada.

## <a name="create-load-balancer"></a>Creación de un equilibrador de carga

Azure Load Balancer es un equilibrador de carga de red que distribuye el tráfico entre las instancias de máquina virtual del grupo de back-end.

### <a name="basics"></a>Aspectos básicos

En la pestaña **Conceptos básicos** de la página del portal Crear equilibrador de carga, verá la siguiente información:



| Configuración |  Detalles |
| ---------- | ---------- |
| Subscription  | Seleccione su suscripción. Esta selección es la suscripción en la que desea implementar el equilibrador de carga. |
| Resource group | Seleccione **Crear nuevo** y escriba el nombre del grupo de recursos en el cuadro de texto. Si tiene un grupo de recursos, selecciónelo. |
| Nombre | Este valor es el nombre de la instancia de Azure Load Balancer. |
| Region | Seleccione una región de Azure en la que desee implementar el equilibrador de carga. |
| Tipo | Un equilibrador de carga tiene dos tipos: </br> **Interno (privado)** </br> **Público (externo)** .</br> Un equilibrador de carga interno (ILB) enruta el tráfico a los miembros del grupo de back-end a través de una dirección IP privada.</br> Un equilibrador de carga público dirige las solicitudes de los clientes por Internet al grupo de back-end.</br> Aprenda más sobre los [tipos de equilibradores de carga](components.md#frontend-ip-configuration-).|
| SKU  | Seleccione **Estándar**. </br> Un equilibrador de carga tiene dos SKU: **Básico** y **Estándar**. </br> Básico tiene una funcionalidad limitada. </br> Se recomienda usar **Estándar** para cargas de trabajo de producción. </br> Obtenga más información sobre las [SKU](skus.md). |

Si selecciona **Público** como tipo, verá la siguiente información:

| Configuración |  Detalles |
| ---------- | ---------- |
| Dirección IP pública | Seleccione **Crear nuevo** con el fin de crear una dirección IP pública para el equilibrador de carga público. </br> Si tiene una dirección IP pública, seleccione **Usar existente**.  |
| Nombre de la dirección IP pública | El nombre de la dirección IP pública.|
| SKU de la dirección IP pública | Las direcciones IP públicas tienen dos SKU: **Básico** y **Estándar**. </br> Básico no admite atributos de zona ni resistencia de zona. </br> Se recomienda usar **Estándar** para cargas de trabajo de producción. </br> Las SKU del equilibrador de carga y de la dirección IP pública **deben coincidir**. |
| Asignación | **Estática** se selecciona automáticamente para Estándar. </br> Las direcciones IP públicas básicas tienen dos tipos: **Dinámica** y **Estática**. </br> Las direcciones IP públicas dinámicas no se asignan hasta la creación. </br> Las direcciones IP se pueden perder si se elimina el recurso. </br> Se recomiendan las direcciones IP estáticas. |
| Zona de disponibilidad | Seleccione **Con redundancia de zona** para crear un equilibrador de carga resistente. </br> Para crear un equilibrador de carga de zona, seleccione una zona específica entre **1**, **2** o **3**. </br> El equilibrador de carga estándar y las direcciones IP públicas admiten zonas. </br> Obtenga más información sobre los [equilibradores de carga y las zonas de disponibilidad](load-balancer-standard-availability-zones.md). </br> No verá la selección de zona para los equilibradores de carga básicos. Basic Load Balancer no admite zonas. |
| Adición de una dirección IPv6 pública | Un equilibrador de carga admite direcciones **IPv6** para el front-end. </br> Obtenga más información sobre las [direcciones IPv6 y los equilibradores de carga](load-balancer-ipv6-overview.md).
| Preferencia de enrutamiento | Seleccione **Microsoft Network**. </br> Con Microsoft Network, el tráfico se enruta a través de la red global de Microsoft. </br> Con Internet, el tráfico se enruta a través de la red del proveedor de servicios de Internet. </br> Obtenga más información sobre las [preferencias de enrutamiento](../virtual-network/routing-preference-overview.md).|

:::image type="content" source="./media/manage/create-public-load-balancer-basics.png" alt-text="Creación de un equilibrador de carga público" border="true":::

Si selecciona **Interno** en Tipo, verá la siguiente información:

| Configuración |  Detalles |
| ---------- | ---------- |
| Virtual network | La red virtual que quiere para el equilibrador de carga interno. </br> La dirección IP de front-end privada que seleccione para el equilibrador de carga interno será de esta red virtual. |
| Asignación de dirección IP | Las opciones son **Estática** o **Dinámica**. </br> Estática garantiza que la dirección IP no cambie. Mientras que una dirección IP dinámica podría cambiar. |
| Zona de disponibilidad | Las opciones son: </br> **Con redundancia de zona** </br> **Zona 1** </br> **Zona 2** </br> **Zona 3** </br> Para crear un equilibrador de carga de alta disponibilidad y resistente a errores de zona de disponibilidad, seleccione una dirección IP **con redundancia de zona**. |

:::image type="content" source="./media/manage/create-internal-load-balancer-basics.png" alt-text="Creación de un equilibrador de carga público" border="true":::

## <a name="frontend-ip-configuration"></a>Configuración de direcciones IP de front-end

La dirección IP de Azure Load Balancer. Es el punto de contacto de los clientes. 

Puede tener una o varias configuraciones de direcciones IP de front-end. Si completó la sección de conceptos básicos anterior, ya habrá creado un front-end para el equilibrador de carga. 

Si desea agregar una configuración de IP de front-end al equilibrador de carga, vaya a su equilibrador de carga en Azure Portal, seleccione **Configuración de IP de front-end** y, a continuación, seleccione **+ Agregar**.

| Configuración |  Detalles |
| ---------- | ---------- |
| Nombre | El nombre de la configuración de IP de front-end. |
| Versión de la dirección IP | La versión de la dirección IP que desea que tenga el front-end. </br> Un equilibrador de carga admite configuraciones IP de front-end IPv4 e IPv6. |
| Tipo de IP | Tipo de IP determina si una única dirección IP está asociada a su front-end o a un intervalo de direcciones IP mediante un prefijo IP. </br> Un [prefijo de IP pública](../virtual-network/public-ip-address-prefix.md) sirve de ayuda para cuando necesita conectarse al mismo punto de conexión repetidamente. El prefijo garantiza que se proporcionan suficientes puertos para ayudar a solucionar los problemas del puerto SNAT. |
| Dirección IP pública (o prefijo si eligió un prefijo antes) | Seleccione o cree una dirección IP pública (o prefijo) para el front-end del equilibrador de carga. |

:::image type="content" source="./media/manage/frontend.png" alt-text="Creación de un equilibrador de carga público" border="true":::

## <a name="backend-pools"></a>Grupos back-end

Un grupo de direcciones de back-end contiene las direcciones IP de las interfaces de red virtuales del grupo de back-end. 

Si desea agregar un grupo de back-end al equilibrador de carga, vaya a su equilibrador de carga en Azure Portal, seleccione **Grupos de back-end** y, a continuación, seleccione **+ Agregar**.

| Configuración | Detalles |
| ---------- |  ---------- |
| Nombre | Nombre del grupo de back-end. |
| Virtual network | La red virtual en la que se encuentran las instancias de back-end. |
| Versión de la dirección IP | Las opciones son **IPv4** o **IPv6**. |

Puede agregar máquinas virtuales o conjuntos de escalado de máquinas virtuales al grupo de back-end de su instancia de Azure Load Balancer. Cree primero las máquinas virtuales o los conjuntos de escalado de máquinas virtuales. A continuación, agréguelos al equilibrador de carga en el portal.

:::image type="content" source="./media/manage/backend.png" alt-text="Creación de un equilibrador de carga público" border="true":::

## <a name="health-probes"></a>Sondeos de estado

Un sondeo de estado se usa para supervisar el estado de las instancias o máquinas virtuales de back-end. El estado de sondeo determina cuándo se envían nuevas conexiones a una instancia en función de las comprobaciones de estado. 

Si desea agregar un sondeo de estado al equilibrador de carga, vaya a su equilibrador de carga en Azure Portal, seleccione **Sondeos de estado** y, a continuación, seleccione **+ Agregar**.

| Configuración | Detalles |
| ---------- | ---------- |
| Nombre | El nombre del sondeo de estado. |
| Protocolo | El protocolo que seleccione determina el tipo de comprobación que se usa para determinar si las instancias de back-end son correctas. </br> Las opciones son: </br> **TCP** </br> **HTTPS** </br> **HTTP** </br> Asegúrese de que esté utilizando la dirección URL correcta. Esta selección dependerá de la naturaleza de la aplicación. </br> La configuración del sondeo de estado y las respuestas de sondeo determinan qué instancias del grupo de back-end recibirán nuevos flujos. </br> Puede usar los sondeos de estado para detectar el error de una aplicación en un punto de conexión de back-end. </br> Obtenga más información sobre los [sondeos de estado](load-balancer-custom-probe-overview.md). |
| Port | El puerto de destino del sondeo de estado. </br> Este valor es el puerto de la instancia de back-end que el sondeo de estado usará para determinar el estado de la instancia. |
| Intervalo | El número de segundos entre los intentos de sondeo. </br> El intervalo determinará la frecuencia con la que el sondeo de estado intentará comunicarse con la instancia de back-end. </br> Si selecciona 5, el segundo intento de sondeo se realizará después de 5 segundos, y así sucesivamente. |
| Umbral incorrecto | El número de errores de sondeo consecutivos que tienen que producirse para que se considere que una máquina virtual no funciona correctamente.</br> Si selecciona 2, no se establecerán nuevos flujos en esta instancia de back-end después de 2 errores consecutivos. |

:::image type="content" source="./media/manage/health-probe.png" alt-text="Creación de un equilibrador de carga público" border="true":::

## <a name="load-balancing-rules"></a>Reglas de equilibrio de carga.

Define cómo se distribuye el tráfico entrante a todas las instancias del grupo de back-end. Las reglas de equilibrio de carga asignan una configuración de IP de front-end y un puerto determinados a varios puertos y direcciones IP de back-end.

Si desea agregar una regla de equilibrio de carga al equilibrador de carga, vaya a su equilibrador de carga en Azure Portal, seleccione **Reglas de equilibrio de carga** y, a continuación, seleccione **+ Agregar**.
    
| Configuración | Detalles |
| ---------- | ---------- |
| Nombre | El nombre de la regla de equilibrio de carga. |
| Versión de la dirección IP | Las opciones son **IPv4** o **IPv6**.  |
| Dirección IP del front-end | Seleccione la dirección IP de front-end. </br> La dirección IP de front-end del equilibrador de carga al que desea asociar la regla de equilibrio de carga.|
| Protocolo | Azure Load Balancer es un equilibrador de carga de nivel 4. </br> Tendrá las siguientes opciones: **TCP** o **UDP**. |
| Port | Este valor es el puerto asociado a la dirección IP de front-end en la que quiere que se distribuya el tráfico en función de esta regla de equilibrio de carga. |
| Puerto back-end | Este valor es el puerto de las instancias del grupo de back-end al que desea que el equilibrador de carga envíe el tráfico. El valor puede ser el mismo que el del puerto de front-end o diferente si necesita flexibilidad para su aplicación. |
| Grupo back-end | El grupo de back-end en el que desea aplicar esta regla de equilibrio de carga. |
| Sondeo de mantenimiento | El sondeo de estado que creó para comprobar el estado de las instancias del grupo de back-end. </br> Solo las instancias correctas recibirán nuevo tráfico. |
| Persistencia de la sesión |  Las opciones son: </br> **None** </br> **IP de cliente** </br> **IP y protocolo del cliente**</br> </br> Mantenga el tráfico de un cliente a la misma máquina virtual del grupo de back-end. Este tráfico se mantendrá mientras dure la sesión. </br> **Ninguno**: especifica que cualquier máquina virtual puede controlar las solicitudes sucesivas del mismo cliente. </br> **IP de cliente**: especifica que la misma máquina virtual controlará las solicitudes sucesivas de la misma dirección IP de cliente. </br> **IP y protocolo del cliente**: especifica que la misma máquina virtual controlará las solicitudes sucesivas de la misma dirección IP de cliente y protocolo. </br> Obtenga más información sobre los [modos de distribución](load-balancer-distribution-mode.md). |
| Tiempo de espera de inactividad (minutos) | Mantenga una conexión **TCP** o **HTTP** abierta sin depender de los clientes para enviar mensajes persistentes. |  
| Restablecimiento de TCP | Un equilibrador de carga puede enviar **restablecimientos de TCP** para ayudar a crear un comportamiento de aplicación más predecible en el momento en que la conexión está inactiva. </br> Obtenga más información sobre los [restablecimientos de TCP](load-balancer-tcp-reset.md).|
| Dirección IP flotante | La dirección IP flotante es el término de Azure para referirse a una parte de lo que se conoce como **Direct Server Return (DSR)** . </br> DSR consta de dos partes: <br> 1. Topología de flujo </br> 2. Un esquema de asignación de direcciones IP en el nivel de plataforma. </br></br> Azure Load Balancer siempre funciona en una topología de flujo DSR, independientemente de si la dirección IP flotante está habilitada o no. </br> Esto significa que la parte de salida de un flujo siempre se reescribe correctamente para que se dirija de nuevo al origen. </br> Sin una dirección IP flotante, Azure expone un esquema de asignación de direcciones IP de equilibrio (la IP de las instancias de máquina virtual). </br> Habilitar la dirección IP flotante cambia el esquema de asignación de direcciones IP a la IP de servidor front-end del equilibrador de carga para permitir más flexibilidad. </br> Para obtener más información, consulte [Varios front-ends para Azure Load Balancer](load-balancer-multivip-overview.md).|
| Creación de reglas de salida implícitas | así que seleccione **No**. </br> Valor predeterminado: **disableOutboundSnat = false**  </br> En este caso, la salida se produce a través de la misma dirección IP de front-end. </br></br> **disableOutboundSnat = true** </br>En este caso, se necesitan reglas de salida para la salida. |

:::image type="content" source="./media/manage/load-balancing-rule.png" alt-text="Creación de un equilibrador de carga público" border="true":::

## <a name="inbound-nat-rules"></a>Reglas NAT de entrada

Una regla NAT de entrada reenvía el tráfico entrante enviado a la combinación de dirección IP y puerto de front-end. 

El tráfico se envía a una máquina virtual o instancia específica en el grupo de back-end. El reenvío de puertos se realiza mediante la misma distribución basada en hash que el equilibrio de carga.

Por ejemplo, si desea que las sesiones de Secure Shell (SSH) o del Protocolo de escritorio remoto (RDP) separen las instancias de máquina virtual de un grupo de back-end. Se pueden asignar varios puntos de conexión internos a puertos de la misma dirección IP de front-end. 

Las direcciones IP de front-end se pueden usar para administrar de forma remota máquinas virtuales sin un jumpbox adicional.

Si desea agregar una regla NAT de entrada a su equilibrador de carga, vaya a su equilibrador de carga en Azure Portal, seleccione **Reglas NAT de entrada** y, a continuación, seleccione **+ Agregar**.

| Configuración | Detalles |
| ---------- | ---------- |
| Nombre | El nombre de la regla NAT de entrada. |
| Dirección IP del front-end | Seleccione la dirección IP de front-end. </br> La dirección IP de front-end del equilibrador de carga al que desea que se asocie la regla NAT de entrada. |
| Versión de la dirección IP | Las opciones son IPv4 e IPv6. |
| Servicio | El tipo de servicio en el que se va a ejecutar Azure Load Balancer. </br> Según lo que seleccione aquí, actualizará la información de puertos adecuadamente. |
| Protocolo | Azure Load Balancer es un equilibrador de carga de nivel 4. </br> Tendrá las siguientes opciones: TCP o UDP. |
| Tiempo de espera de inactividad (minutos) | Mantenga una conexión TCP o HTTP abierta sin depender de los clientes para enviar mensajes persistentes. |
| Restablecimiento de TCP | Un equilibrador de carga puede enviar restablecimientos de TCP para ayudar a crear un comportamiento de aplicación más predecible en el momento en que la conexión está inactiva. </br> Obtenga más información sobre los [restablecimientos de TCP](load-balancer-tcp-reset.md). |
| Port | Este valor es el puerto asociado a la dirección IP de front-end en la que quiere que se distribuya el tráfico en función de esta regla NAT de entrada. |
| Máquina virtual de destino | La parte de la máquina virtual del grupo de back-end al que le gustaría asociar esta regla. |
| Asignación de puertos | Este valor puede ser el predeterminado o personalizada según las preferencias de la aplicación. |

:::image type="content" source="./media/manage/inbound-nat-rule.png" alt-text="Creación de un equilibrador de carga público" border="true":::

## <a name="outbound-rules"></a>Reglas de salida

Las reglas de salida del equilibrador de carga configuran SNAT saliente para las máquinas virtuales del grupo de back-end.

Si desea agregar una regla de salida al equilibrador de carga, vaya a su equilibrador de carga en Azure Portal, seleccione **Reglas de salida** y, a continuación, seleccione **+ Agregar**.

| Configuración | Detalles |
| ------- | ------ |
| Nombre | El nombre de la regla de salida. |
| Dirección IP del front-end | Seleccione la dirección IP de front-end. </br> La dirección IP de front-end del equilibrador de carga al que desea que se asocie la regla de salida. |
| Protocolo | Azure Load Balancer es un equilibrador de carga de nivel 4. </br> Tendrá las siguientes opciones: **Todos**, **TCP** o **UDP**. |
| Tiempo de espera de inactividad (minutos) | Mantenga una conexión **TCP** o **HTTP** abierta sin depender de los clientes para enviar mensajes persistentes. |
| Restablecimiento de TCP | Un equilibrador de carga puede enviar **restablecimientos de TCP** para ayudar a crear un comportamiento de aplicación más predecible en el momento en que la conexión está inactiva. </br> Obtenga más información sobre los [restablecimientos de TCP](load-balancer-tcp-reset.md). |
| Grupo back-end | El grupo de back-end en el que desea aplicar esta regla de salida. |

### <a name="port-allocation"></a>Asignación de puertos

| Configuración | Detalles |
| ------- | ------ |
| Asignación de puertos | Se recomienda seleccionar **Elegir manualmente el número de puertos de salida**.|

### <a name="outbound-ports"></a>Puertos de salida

| Configuración | Detalles |
| ------- | ------ |
| Elegir por | Seleccione **Puertos por instancia**. |
| Puertos por instancia | Escriba **10 000**. |

:::image type="content" source="./media/manage/outbound-rule.png" alt-text="Creación de un equilibrador de carga público" border="true":::

## <a name="next-steps"></a>Pasos siguientes

En este artículo, ha aprendido los distintos términos y configuraciones de Azure Portal para Azure Load Balancer.

* Obtenga [más información](./load-balancer-overview.md) sobre Azure Load Balancer.
* [Preguntas más frecuentes](./load-balancer-faqs.md) sobre Azure Load Balancer.
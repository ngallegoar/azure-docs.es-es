---
title: Planeación de capacidad del clúster de Service Fabric
description: Consideraciones de planeación de capacidad del clúster de Service Fabric Tipos de nodos, operaciones, durabilidad y niveles de confiabilidad
ms.topic: conceptual
ms.date: 07/09/2019
ms.author: pepogors
ms.openlocfilehash: 6e60fc10dd7e0eec24de4a089d09d914624dcfbc
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "79229452"
---
# <a name="service-fabric-cluster-capacity-planning-considerations"></a>Consideraciones de planeación de capacidad del clúster de Service Fabric
En cualquier implementación de producción, la planeación de capacidad es un paso importante. Estos son algunos de los elementos que se deben tener en cuenta como parte de ese proceso.

* Número de tipos de nodos con los que el clúster tiene que empezar
* Propiedades de cada tipo de nodo (tamaño, principal, accesible desde Internet, número de VM, etc.)
* Características de confiabilidad y durabilidad del clúster

> [!NOTE]
> Como mínimo, debe revisar todos los valores **no permitidos** de la directiva de actualización durante la planeación. Esto es para asegurarse de que se establezcan correctamente los valores y para mitigar el consumo del clúster más adelante debido a un conjunto de configuración del sistema que no se puede cambiar. 
> 

Repasemos brevemente cada uno de estos elementos.

## <a name="the-number-of-node-types-your-cluster-needs-to-start-out-with"></a>Número de tipos de nodos con los que el clúster tiene que empezar
En primer lugar, debe decidir para qué va a servir el clúster que va a crear.  ¿Qué tipos de aplicaciones planea que se van a implementar en este clúster? Si no tiene clara la finalidad del clúster, es muy probable que todavía no está listo para especificar el proceso de planeación de capacidad.

Establezca el número de tipos de nodos con los que el clúster tiene que empezar.  Cada tipo de nodo se asigna a un conjunto de escalado de máquinas virtuales. Cada tipo de nodo se puede escalar o reducir verticalmente de forma independiente. Cada uno tiene diferentes conjuntos de puertos abiertos y puede tener distintas métricas de capacidad. Por lo que la decisión del número de tipos de nodo depende esencialmente de las consideraciones siguientes:

* ¿Tiene la aplicación varios servicios y alguno de ellos ha de ser público o accesible desde Internet? Las aplicaciones típicas contienen un servicio front-end de puerta de enlace que recibe entradas de un cliente, así como uno o varios servicios back-end que se comunican con los servicios front-end. Por lo que en este caso, acaba teniendo al menos dos tipos de nodo.
* ¿Tienen los servicios (que componen la aplicación) distintos requisitos de infraestructura, como, por ejemplo, una RAM mayor o un número más alto de ciclos de CPU? Por ejemplo, supongamos que la aplicación que desea implementar contiene un servicio front-end y un servicio back-end. El servicio front-end se puede ejecutar en máquinas virtuales más pequeñas (tamaños de VM como D2) que tienen puertos abiertos a Internet.  Pero el servicio back-end es de cálculo intensivo y tiene que ejecutarse en máquinas virtuales más grandes (con tamaños de VM como D4, D6 o D15) que no son accesibles desde Internet.
  
  En este ejemplo, aunque puede optar por colocar todos los servicios en un tipo de nodo, se recomienda colocarlos en un clúster con dos tipos de nodos.  Esto permite que cada tipo de nodo pueda tener distintas propiedades, por ejemplo, conectividad a Internet o tamaño de máquina virtual. El número de máquinas virtuales también se puede escalar de forma independiente.  
* Como no puede predecir el futuro, parta de hechos que conozca y elija el número de tipos de nodo que con el que deben empezar las aplicaciones. Siempre puede agregar o quitar tipos de nodos más adelante. Un clúster de Service Fabric debe tener como mínimo un tipo de nodo.

## <a name="the-properties-of-each-node-type"></a>Las propiedades de cada tipo de nodo.
El **tipo de nodo** puede considerarse similar a los roles de Cloud Services. Los tipos de nodos definen los tamaños de máquina virtual, el número de máquinas virtuales y sus propiedades. Cada tipo de nodo que se define en un clúster de Service Fabric se asigna a con un [conjunto de escalado de máquinas virtuales](https://docs.microsoft.com/azure/virtual-machine-scale-sets/overview).  
Cada tipo de nodo es un conjunto de escalado diferente que se puede escalar o reducir verticalmente de forma independiente. Cada uno tiene diferentes conjuntos de puertos abiertos y distintas métricas de capacidad. Para más información acerca de las relaciones entre los tipos de nodos y los conjuntos de escalado de máquinas virtuales, cómo conectarse con RDP a una de las instancias, cómo abrir nuevos puertos, etc., consulte [Tipos de nodos de clúster de Service Fabric](service-fabric-cluster-nodetypes.md).

Un clúster de Service Fabric puede constar de más de un tipo de nodo. En ese caso, el clúster consta de un tipo de nodo principal y uno o más tipos de nodo no principales.

Un solo tipo de nodo no puede escalar de manera confiable más allá de 100 nodos por cada conjunto de escalado de máquinas virtuales para aplicaciones SF; para lograr más de 100 nodos de manera confiable, tendrá que agregar más conjuntos de escalado de máquinas virtuales.

### <a name="primary-node-type"></a>Tipo de nodo principal

Los servicios de sistema de Service Fabric (por ejemplo, Administrador de clústeres o Almacén de imágenes) se colocan en el tipo de nodo principal. 

![Captura de pantalla de un clúster con dos tipos de nodo][SystemServices]

* El **tamaño mínimo de las máquinas virtuales** del tipo de nodo principal se determina mediante el **nivel de durabilidad** que se elija. El nivel de durabilidad predeterminado es Bronze. Consulte [Características de durabilidad del clúster](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-capacity#the-durability-characteristics-of-the-cluster) para más información.  
* El **número mínimo de máquinas virtuales** del tipo de nodo principal se determina mediante el **nivel de confiabilidad** que se elija. El nivel de confiabilidad predeterminado es Silver. Consulte [Características de confiabilidad del clúster](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-capacity#the-reliability-characteristics-of-the-cluster) para más información.  

En la plantilla de Azure Resource Manager, el tipo de nodo principal se configura con el atributo `isPrimary` en la [definición de tipo de nodo](https://docs.microsoft.com/azure/templates/microsoft.servicefabric/clusters#nodetypedescription-object).

### <a name="non-primary-node-type"></a>Tipo de nodo no principal

En un clúster con varios tipos de nodos, hay un tipo de nodo principal y los demás son de tipo no principal.

* El **tamaño mínimo de las máquinas virtuales** de los tipos de nodo no principales se determina mediante el **nivel de durabilidad** que se elija. El nivel de durabilidad predeterminado es Bronze. Para obtener más información, vea [Características de durabilidad del clúster](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-capacity#the-durability-characteristics-of-the-cluster).  
* El **número mínimo de máquinas virtuales** para los tipos de nodo no principales es uno. Sin embargo, este número se debe elegir en función del número de réplicas de la aplicación o los servicios que desee ejecutar en este tipo de nodo. Después de implementar el clúster, se puede aumentar el número de máquinas virtuales de un tipo de nodo.

## <a name="the-durability-characteristics-of-the-cluster"></a>Características de durabilidad del clúster
El nivel de durabilidad se usa para indicar al sistema los privilegios que tienen las máquinas virtuales con la infraestructura subyacente de Azure. En el tipo de nodo principal, este privilegio permite que Service Fabric pause cualquier solicitud de la infraestructura de nivel de máquina virtual (por ejemplo, reinicio de máquina virtual, restablecimiento de la imagen inicial de máquina virtual o migración de máquina virtual) que afecte a los requisitos de quórum de los servicios del sistema y los servicios con estado. En los tipos de nodo distintos del principal, este privilegio permite que Service Fabric pause cualquier solicitud de la infraestructura de nivel de máquina virtual (por ejemplo, el reinicio de máquina virtual, el restablecimiento de la imagen inicial de máquina virtual o la migración de máquinas virtuales) que afecte a los requisitos de cuórum de los servicios con estado.

| Nivel de durabilidad  | Número mínimo de máquinas virtuales | SKU de VM admitidas                                                                  | Actualizaciones realizadas en el conjunto de escalado de máquinas virtuales                               | Actualizaciones y mantenimiento iniciados por Azure                                                              | 
| ---------------- |  ----------------------------  | ---------------------------------------------------------------------------------- | ----------------------------------------------------------- | ------------------------------------------------------------------------------------------------------- |
| Oro             | 5                              | SKU de nodo completo dedicadas a un solo cliente (por ejemplo, L32s, GS5, G5, DS15_v2, D15_v2) | Se puede retrasar hasta que lo apruebe el clúster de Service Fabric | Puede poner en pausa durante 2 horas por UD para dar más tiempo a las réplicas para que se recuperen de errores anteriores |
| Plata           | 5                              | Máquinas virtuales de un solo núcleo o varios con al menos 50 GB de SSD local                      | Se puede retrasar hasta que lo apruebe el clúster de Service Fabric | No se puede retrasar una cantidad de tiempo significativa                                                    |
| Bronce           | 1                              | Máquinas virtuales con al menos 50 GB de SSD local                                              | El clúster de Service Fabric no lo retrasará           | No se puede retrasar una cantidad de tiempo significativa                                                    |

> [!WARNING]
> Los tipos de nodos que se ejecutan con la durabilidad Bronze _no obtienen privilegios_. Es decir, los trabajos de infraestructura que afecten a las cargas de trabajo con estado no se detendrán ni retrasarán, lo que podría afectar a las cargas de trabajo. Utilice el nivel Bronze en los tipos de nodos que ejecutan únicamente cargas de trabajo sin estado. Para las cargas de trabajo de producción, se recomienda utilizar el nivel Silver o superiores. 
> 
> Con independencia de cualquier nivel de durabilidad, la operación de [desasignación](https://docs.microsoft.com/rest/api/compute/virtualmachinescalesets/deallocate) en el conjunto de escalado de máquina virtual destruirá el clúster

**Ventajas del uso de los niveles de durabilidad Silver o Gold**
 
- Reduce el número de pasos necesarios en una operación de reducción horizontal (es decir, se llama automáticamente a la desactivación del nodo y a Remove-ServiceFabricNodeState).
- Reduce el riesgo de pérdida de datos debido a una operación de cambio de SKU en máquinas virtuales locales iniciada por el cliente o a operaciones en la infraestructura de Azure.

**Desventajas del uso de los niveles de durabilidad Silver o Gold**
 
- Las implementaciones tanto en el conjunto de escalado de máquinas virtuales como en otros recursos de Azure relacionados se pueden retrasar, pueden agotar el tiempo de espera o se pueden bloquear completamente por problemas en el clúster o en la infraestructura. 
- Aumenta el número de [eventos de los ciclos de vida de las réplicas](service-fabric-reliable-services-lifecycle.md) (p. ej. intercambios principales) debidos a las desactivaciones automáticas de nodos durante las operaciones en la infraestructura de Azure.
- Deja los nodos fuera de servicio durante los períodos de tiempo en los que tienen lugar las actualizaciones de software de la plataforma de Azure o las actividades de mantenimiento de hardware. Durante estas actividades, puede ver los nodos con un estado que indica que se están deshabilitando o que están deshabilitados. Esto reduce la capacidad del clúster temporalmente, pero no debería afectar a la disponibilidad de dicho clúster o de las aplicaciones.

### <a name="recommendations-for-when-to-use-silver-or-gold-durability-levels"></a>Recomendaciones acerca de cuándo usar los niveles de durabilidad Silver o Gold

Utilice las durabilidades Silver o Gold para todos los tipos de nodo que hospedan servicios con estado que espera reducir horizontalmente (reducir el número de instancias de máquina virtual) con frecuencia, y preferiría que las operaciones de implementación se retrasasen y la capacidad se redujera en favor de la simplificación de estas operaciones de reducción horizontal. Los escenarios de escalabilidad horizontal (adición de instancias de máquinas virtuales) no se tienen en cuenta al elegir el nivel de durabilidad, solo se tiene en cuenta la reducción horizontal.

### <a name="changing-durability-levels"></a>Cambio de los niveles de durabilidad
- Los tipos de nodo con niveles de durabilidad plata u oro no se pueden cambiar a bronce.
- El cambio de bronce a plata u oro tarda unas horas.
- Al cambiar el nivel de durabilidad, asegúrese de actualizarlo tanto en la configuración de la extensión de Service Fabric del recurso del conjunto de escalado de máquinas virtuales como en la definición del tipo de nodo del recurso de clúster de Service Fabric. Estos valores deben coincidir.

### <a name="operational-recommendations-for-the-node-type-that-you-have-set-to-silver-or-gold-durability-level"></a>Recomendaciones operativas para el tipo de nodo en el que ha elegido los niveles de durabilidad Silver o Gold.

- Mantenga el clúster y las aplicaciones en buen estado en todo momento, y asegúrese de que las aplicaciones responden a todos los [eventos de los ciclos de vida de las réplicas del servicio](service-fabric-reliable-services-lifecycle.md) (como que la réplica en compilación está bloqueada) en el momento adecuado.
- Adopte formas más seguras de cambiar una SKU de una máquina virtual (escalar verticalmente/reducir verticalmente): El cambio de la SKU de la máquina virtual de un conjunto de escalado de máquinas virtuales requiere determinados pasos y consideraciones. Este es el proceso que se puede seguir para evitar problemas comunes.
    - **Para los tipos de nodos no principales:** le recomendamos crear un conjunto de escalado de máquinas virtuales, modificar la restricción de la ubicación de servicio para incluir el nuevo conjunto de escalado de máquinas virtuales o tipo de nodo y, después, reducir el recuento de instancias del conjunto de escalado de máquinas virtuales antiguo a cero, nodo a nodo (de esta forma, se asegurará de que la eliminación de los nodos no afecte a la confiabilidad del clúster).
    - **En el caso de tipo de nodo principal:** Si la SKU de la máquina virtual que ha seleccionado está al máximo de su capacidad y desea cambiar a una SKU de mayor tamaño, siga las instrucciones de [Escalado vertical del tipo de nodo principal](https://docs.microsoft.com/azure/service-fabric/service-fabric-scale-up-node-type). 

- Mantenga un mínimo de cinco nodos en todos los conjuntos de escalado de máquinas virtuales que tengan habilitados los niveles de durabilidad Gold o Silver.
- Cada conjunto de escalado de máquinas virtuales con el nivel de durabilidad Silver o Gold tiene que asignarse a su propio tipo de nodo en el clúster de Service Fabric. La asignación de varios conjuntos de escalado de máquinas virtuales a un único tipo de nodo impedirá que la coordinación entre el clúster de Service Fabric y la infraestructura de Azure funcione correctamente.
- No elimine instancias de máquina virtual aleatorias, use siempre la característica de reducción vertical del conjunto de escalado de máquinas virtuales. La eliminación de instancias de máquina virtual aleatorias tiene el potencial de crear desequilibrios en la instancia de máquina virtual repartidos entre UD y FD. Este desequilibrio puede afectar negativamente a la capacidad de los sistemas para realizar un correcto equilibrio de la carga entre las instancias del servicio o réplicas del servicio.
- Si se usa el escalado automático, establezca las reglas de forma que la reducción horizontal (eliminación de instancias de máquina virtual) no se realiza en varios nodos simultáneamente. No es seguro reducir verticalmente más de una instancia a la vez.
- Si va a eliminar o cancelar la asignación de máquinas virtuales en el tipo de nodo principal, no reduzca nunca el número de máquinas virtuales asignadas por debajo de lo que requiere el nivel de confiabilidad. Estas operaciones se bloquearán indefinidamente en un conjunto de escalado con un nivel de durabilidad Silver o Gold.

## <a name="the-reliability-characteristics-of-the-cluster"></a>Características de confiabilidad del clúster
El nivel de confiabilidad se usa para establecer el número de réplicas de los servicios del sistema que se desea ejecutar en el tipo de nodo principal de este clúster. Cuanto mayor sea el número de réplicas, más confiables son los servicios del sistema en el clúster.  

El nivel de confiabilidad puede adoptar los siguientes valores:

* Platinum: los servicios del sistema se ejecutan con un recuento de conjunto de réplicas de nueve
* Gold: los servicios del sistema se ejecutan con un recuento de conjunto de réplicas de siete
* Silver: los servicios del sistema se ejecutan con un recuento de conjunto de réplicas de cinco 
* Bronze: los servicios del sistema se ejecutan con un recuento de conjunto de réplicas de tres

> [!NOTE]
> El nivel de confiabilidad que elija determina el número mínimo de nodos que debe tener el tipo de nodo principal. 
> 
> 

### <a name="recommendations-for-the-reliability-tier"></a>Recomendaciones para el nivel de confiabilidad

Al aumentar o disminuir el tamaño del clúster (la suma de las instancias de máquinas virtuales en todos los tipos de nodo), debe actualizar la confiabilidad del clúster desde un nivel a otro. Esto desencadena las actualizaciones de clúster necesarias para cambiar el recuento de conjunto de réplicas de los servicios del sistema. Espere a que finalice la actualización en curso antes de realizar otros cambios en el clúster, como agregar nodos.  Puede supervisar el progreso de la actualización en Service Fabric Explorer o puede ejecutar [Get-ServiceFabricClusterUpgrade](/powershell/module/servicefabric/get-servicefabricclusterupgrade?view=azureservicefabricps).

Esta es la recomendación sobre cómo elegir el nivel de confiabilidad.  El número de nodos raíz también se establece en el número mínimo de nodos para un nivel de confiabilidad.  Por ejemplo, para un clúster con una confiabilidad Gold, hay siete nodos raíz.

| **Número de nodos del clúster** | **Nivel de confiabilidad** |
| --- | --- |
| 1 |No especifique el parámetro nivel de confiabilidad, el sistema lo calcula |
| 3 |Bronce |
| 5 o 6|Plata |
| 7 u 8 |Oro |
| 9 y superiores |Platinum |

## <a name="primary-node-type---capacity-guidance"></a>Tipo de nodo principal: guía de capacidad

Esta es la guía para planear la capacidad del tipo de nodo principal:

- **Número de instancias de máquina virtual para ejecutar cargas de trabajo de producción en Azure:** Debe especificar un tamaño mínimo de tipo de nodo principal de 5 y un nivel de confiabilidad Silver.  
- **Número de instancias de máquina virtual para ejecutar cargas de trabajo de prueba en Azure** Puede especificar un tamaño mínimo de tipo de nodo principal de 1 o 3. El clúster de 1 nodo uno funciona con una configuración especial y, por tanto, no se admite el escalado horizontal de ese clúster. El clúster de 1 nodo no tiene ninguna confiabilidad y, por tanto, en la plantilla de Resource Manager, tiene que quitar/no especificar esa configuración (no basta con no definir el valor de configuración). Si configura el clúster del nodo uno configurado mediante el portal, la configuración se controla automáticamente. Los clústeres de uno y tres nodos no se admiten en la ejecución de cargas de trabajo de producción. 
- **SKU de máquina virtual:** el tipo de nodo principal es donde se ejecutan los servicios del sistema, así que la SKU de máquina virtual que elija, debe tener en cuenta la carga máxima global que planea colocar en el clúster. En esta analogía se ilustra el significado de esto: considere que el tipo de nodo principal son sus "pulmones", es lo que lleva oxígeno a su cerebro, de modo que si no se obtiene oxígeno suficiente, el cuerpo sufre. 

Las necesidades de capacidad de un clúster las determina la carga de trabajo que planea ejecutar en el clúster. Por consiguiente, no le podemos proporcionar una guía cualitativa de la carga de trabajo específica, aunque sí una amplia guía que le ayudará a comenzar

Para cargas de trabajo de producción: 

- Se recomienda dedicar el NodeType principal de los clústeres a los servicios del sistema, y usar las restricciones de selección de ubicación para implementar la aplicación en los NodeTypes secundarios.
- La SKU de máquina virtual recomendada es Standard V2_V2 o equivalente con un SSD local de 50 GB como mínimo.
- La SKU de máquina virtual mínima admitida es Standard_D2_V3 o Standard D1_V2 o equivalente con un SSD local de 50 GB como mínimo. 
- Nuestra recomendación es un mínimo de 50 GB. Para las cargas de trabajo, especialmente cuando se ejecutan contenedores de Windows, se necesitan discos más grandes. 
- Las SKU de máquina virtual de núcleo parcial, como Standard A0, no se admiten en cargas de trabajo de producción.
- Las SKU de máquinas virtuales de serie no se admiten en cargas de trabajo de producción por motivos de rendimiento.
- No se admiten máquinas virtuales de prioridad baja.

> [!WARNING]
> Cambiar el tamaño de SKU de la máquina virtual del nodo principal en un clúster en ejecución es una operación de escalado, y se documenta en [Escalado horizontal de un conjunto de escalado de máquinas virtuales](virtual-machine-scale-set-scale-node-type-scale-out.md).

## <a name="non-primary-node-type---capacity-guidance-for-stateful-workloads"></a>Tipo de nodo distinto del principal: guía de capacidad para las cargas de trabajo con estado

Esta guía está dirigida a cargas de trabajo con estado con [colecciones confiables o actores confiables](service-fabric-choose-framework.md) de Service Fabric en ejecución en el tipo de nodo no principal.

**Número de instancias de máquina virtual:** para cargas de trabajo de producción con estado, se recomienda ejecutarlas con un número mínimo de réplicas de destino de 5. Esto significa que en estado estable acabará con una réplica (de un conjunto de réplicas) en cada dominio de error y dominio de actualización. El concepto de nivel de confiabilidad completo para el tipo de nodo principal es una manera de especificar esta configuración para los servicios del sistema. Por lo que la misma consideración se aplica a los servicios con estado también.

Por lo tanto, para cargas de trabajo de producción, el tamaño mínimo recomendado de tipo de nodo no principal es 5, si ejecuta en él cargas de trabajo con estado.

**SKU de máquina virtual:** es el tipo de nodo donde se ejecutan los servicios de aplicación, de modo que la SKU de máquina virtual que elija para él debe tener en cuenta la carga máxima que planea colocar en cada nodo. Como las necesidades de capacidad del tipo de nodo se determinan por la carga de trabajo que planea ejecutar en el clúster, no podemos proporcionarle una guía cualitativa de su carga de trabajo específica, aunque le ofrecemos una guía general para ayudarle a empezar.

Para cargas de trabajo de producción 

- La SKU de máquina virtual recomendada es Standard V2_V2 o equivalente con un SSD local de 50 GB como mínimo.
- La SKU de máquina virtual mínima admitida es Standard_D2_V3 o Standard D1_V2 o equivalente con un SSD local de 50 GB como mínimo. 
- Las SKU de máquina virtual de núcleo parcial, como Standard A0, no se admiten en cargas de trabajo de producción.
- Las SKU de máquinas virtuales de serie no se admiten en cargas de trabajo de producción por motivos de rendimiento.

## <a name="non-primary-node-type---capacity-guidance-for-stateless-workloads"></a>Tipo de nodo distinto del principal: guía de capacidad para las cargas de trabajo sin estado

Esta guía es válida para las cargas de trabajo sin estado en ejecución en el tipo de nodo no principal.

**Número de instancias de máquina virtual:** en el caso de las cargas de trabajo de producción sin estado, el tamaño mínimo admitido del tipo de nodo no principal es dos. Esto le permite ejecutar dos instancias sin estado de la aplicación y permite que el servicio para sobreviva a la pérdida de una instancia de máquina virtual. 

**SKU de máquina virtual:** es el tipo de nodo donde se ejecutan los servicios de aplicación, de modo que la SKU de máquina virtual que elija para él debe tener en cuenta la carga máxima que planea colocar en cada nodo. Las necesidades de capacidad del tipo de nodo se determinan mediante la carga de trabajo que tenga previsto ejecutar en el clúster. No podemos proporcionarle una guía cualitativa para su carga de trabajo específica.  Pero le ofrecemos una guía general para ayudarle a empezar.

Para cargas de trabajo de producción 

- La SKU de máquina virtual recomendada es Standard D2_V2 o equivalente. 
- La SKU de máquina virtual mínima admitida es Standard D1 o Standard D1_V2 o equivalente. 
- Las SKU de máquina virtual de núcleo parcial, como Standard A0, no se admiten en cargas de trabajo de producción.
- Las SKU de máquinas virtuales de serie no se admiten en cargas de trabajo de producción por motivos de rendimiento.

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->

## <a name="next-steps"></a>Pasos siguientes
Después de finalizar la planeación de capacidad y configurar un clúster, puede consultar lo siguiente:

* [Seguridad de los clústeres de Service Fabric](service-fabric-cluster-security.md)
* [Escalado de los clústeres de Service Fabric](service-fabric-cluster-scaling.md)
* [Planeamiento de recuperación ante desastres](service-fabric-disaster-recovery.md)
* [Relación entre los tipos de nodo y los conjuntos de escalado de máquinas virtuales](service-fabric-cluster-nodetypes.md)

<!--Image references-->
[SystemServices]: ./media/service-fabric-cluster-capacity/SystemServices.png

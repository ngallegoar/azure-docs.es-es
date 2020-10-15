---
title: Equilibrio de métricas agrupadas en subclústeres
description: Efecto de las restricciones de selección de ubicación en el equilibrio y cómo controlarlo
author: nipavlo
ms.topic: conceptual
ms.date: 03/15/2020
ms.author: nipavlo
ms.openlocfilehash: 7f571a851e4da147240c524b742bcd652bc54181
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "82183130"
---
# <a name="balancing-of-subclustered-metrics"></a>Equilibrio de métricas agrupadas en subclústeres

## <a name="what-is-subclustering"></a>Agrupación en subclústeres

La agrupación en subclústeres se produce cuando dos servicios con restricciones de selección de ubicación diferentes tienen una métrica común y ambos notifican la carga de esta. Si la carga que notifican los servicios difiere significativamente, la carga total de los nodos tendrá una gran desviación estándar y parecerá que el clúster está desequilibrado, incluso aunque tenga el mejor equilibrio posible.

## <a name="how-subclustering-affects-load-balancing"></a>Efecto de la agrupación en subclústeres en el equilibrio de carga

Si la carga que los servicios notifican en diferentes nodos difiere significativamente, puede parecer que hay un gran desequilibrio cuando realmente no hay ninguno. Además, si el falso desequilibrio causado por una agrupación en subclústeres es mayor que el desequilibrio real, existe la posibilidad de confundir al algoritmo de equilibrio de Resource Manager y producir un equilibrio no óptimo en el clúster.

Por ejemplo, supongamos que tenemos cuatro servicios y todos ellos notifican una carga para la métrica Metric1:

* Servicio A: tiene una restricción de colocación "NodeType==Frontend" y notifica una carga de 10
* Servicio B: tiene una restricción de colocación "NodeType==Frontend" y notifica una carga de 10
* Servicio C: tiene una restricción de colocación "NodeType==Backend" y notifica una carga de 100
* Servicio D: tiene una restricción de colocación "NodeType==Backend" y notifica una carga de 100
* Y tenemos cuatro nodos. En dos de ellos, NodeType está establecido en "Frontend", mientras que en los otros dos está en "Backend"

Y tenemos la siguiente selección de ubicación:

<center>

![Ejemplo de selección de ubicación agrupada en subclústeres][Image1]
</center>

El clúster puede parecer desequilibrado, tenemos una carga grande en los nodos 3 y 4, pero esta selección de ubicación crea el mejor equilibrio posible en esta situación.

Resource Manager puede reconocer las situaciones de agrupación en subclústeres y en casi todos los casos puede generar el equilibrio óptimo para una situación determinada.

En algunas situaciones excepcionales en las que Resource Manager no puede equilibrar de manera óptima una métrica subagrupada, seguirá detectando la agrupación en subclústeres y se generará un informe de mantenimiento que le aconsejará que solucione el problema.

## <a name="types-of-subclustering-and-how-they-are-handled"></a>Tipos de agrupación en subclústeres y cómo se controlan

Las situaciones de agrupación en subclústeres se pueden clasificar en tres categorías diferentes. La categoría de una situación específica de agrupación en subclústeres determina cómo la controlará Resource Manager.

### <a name="first-category--flat-subclustering-with-disjoint-node-groups"></a>Primera categoría: agrupación plana en subclústeres con grupos de nodos no contiguos

Esta categoría tiene la forma más sencilla de agrupación en subclústeres en la que los nodos se pueden dividir en grupos diferentes y cada servicio solo se puede colocar en nodos de uno de esos grupos. Cada nodo pertenece a un grupo únicamente. La situación descrita anteriormente pertenece a esta categoría al igual que la mayoría de las situaciones de agrupación en subclústeres. 

Para las situaciones de esta categoría, Resource Manager puede producir el equilibrio óptimo y no se necesita ninguna intervención más.

### <a name="second-category--subclustering-with-hierarchical-node-groups"></a>Segunda categoría: agrupación en subclústeres con grupos de nodos jerárquicos

Esta situación se produce cuando un grupo de nodos permitidos para un servicio es un subconjunto del grupo de nodos permitidos para otro servicio. El ejemplo más habitual de esta situación se produce cuando algún servicio tiene una restricción de selección de ubicación definida y otro servicio no tiene ninguna restricción de este tipo y se puede colocar en cualquier nodo.

Ejemplo:

* Servicio A: no hay ninguna restricción de selección de ubicación
* Servicio B: restricción de colocación "NodeType==Frontend"
* Servicio C: restricción de colocación "NodeType==Backend"

Esta configuración crea una relación de subconjunto-supraconjunto entre grupos de nodos de diferentes servicios.

<center>

![Subclústeres de supraconjuntos de subconjuntos][Image2]
</center>

En esta situación, existe la posibilidad de que se produzca un equilibrio no óptimo.

Resource Manager reconocerá esta situación y generará un informe de estado en el que se aconseja dividir el servicio A en dos servicios: el servicio A1, que se puede colocar en Frontend, y el servicio A2 que se puede colocar en los nodos Backend. Esto nos llevará de nuevo a la situación de la primera categoría que se puede equilibrar de manera óptima.

### <a name="third-category--subclustering-with-partial-overlap-between-node-sets"></a>Tercera categoría: agrupación en subclústeres con superposición parcial entre conjuntos de nodos

Esta situación se produce cuando hay una superposición parcial entre conjuntos de nodos en los que se pueden colocar algunos servicios.

Por ejemplo, si tenemos una propiedad de nodo denominada NodeColor y tenemos tres nodos:

* Nodo 1: NodeColor=Red
* Nodo 2: NodeColor=Blue
* Nodo 2: NodeColor=Green

Y tenemos dos servicios:

* Servicio A: con restricción de selección de ubicación "Color==Red || Color==Blue"
* Servicio B: con restricción de selección de ubicación "Color==Blue || Color==Green"

Por este motivo, el servicio A se puede colocar en los nodos 1 y 2 y el servicio B se puede colocar en los nodos 2 y 3.

En esta situación, existe la posibilidad de que se produzca un equilibrio no óptimo.

Resource Manager reconocerá esta situación y creará un informe de mantenimiento que le aconseja dividir algunos de los servicios.

En esta situación, Resource Manager no puede dar una propuesta sobre cómo dividir los servicios, ya que se pueden realizar varias divisiones y no hay ninguna manera de estimar qué manera sería la óptima para dividir los servicios.

## <a name="configuring-subclustering"></a>Configuración de la agrupación en subclústeres

El comportamiento de Resource Manager con respecto a la agrupación en subclústeres se puede cambiar modificando los siguientes parámetros de configuración:
* SubclusteringEnabled: este parámetro determina si Resource Manager tendrá en cuenta la agrupación en subclústeres al equilibrar la carga. Si este parámetro está desactivado, Resource Manager omitirá la agrupación en subclústeres e intentará lograr un equilibrio óptimo en un nivel global. El valor predeterminado de este parámetro es false.
* SubclusteringReportingPolicy: determina el modo en que Resource Manager emitirá informes de mantenimiento para la agrupación en subclústeres jerárquica y la de superposición parcial. Un valor de cero significa que los informes de mantenimiento sobre la agrupación en subclústeres están desactivados, "1" significa que se producirán informes de mantenimiento de advertencia para las situaciones que no sean óptimas y un valor de "2" producirá informes de mantenimiento con el estado "correcto". El valor predeterminado para este parámetro es "1".

ClusterManifest.xml:

``` xml
        <Section Name="PlacementAndLoadBalancing">
            <Parameter Name="SubclusteringEnabled" Value="true" />
            <Parameter Name="SubclusteringReportingPolicy" Value="1" />
        </Section>
```

a través de ClusterConfig.json para las implementaciones independientes o Template.json para los clústeres hospedados en Azure:

```json
"fabricSettings": [
  {
    "name": "PlacementAndLoadBalancing",
    "parameters": [
      {
          "name": "SubclusteringEnabled",
          "value": "true"
      },
      {
          "name": "SubclusteringReportingPolicy",
          "value": "1"
      },
    ]
  }
]
```

## <a name="next-steps"></a>Pasos siguientes
* Para más información sobre cómo Cluster Resource Manager administra y equilibra la carga en el clúster, consulte el artículo sobre el [equilibrio de carga](service-fabric-cluster-resource-manager-balancing.md)
* Para más información sobre cómo se pueden restringir los servicios para que solo se coloquen en determinados nodos, consulte [Restricciones de ubicación y propiedades de nodo](service-fabric-cluster-resource-manager-cluster-description.md#node-properties-and-placement-constraints).

[Image1]:./media/cluster-resource-manager-subclustering/subclustered-placement.png
[Image2]:./media/cluster-resource-manager-subclustering/subset-superset-nodes.png

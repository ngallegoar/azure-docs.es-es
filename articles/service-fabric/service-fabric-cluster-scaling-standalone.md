---
title: Escalado de un clúster de Azure Service Fabric independiente
description: Obtenga información acerca de cómo escalar horizontal y verticalmente clústeres independientes de Service Fabric.
author: dkkapur
ms.topic: conceptual
ms.date: 11/13/2018
ms.author: dekapur
ms.openlocfilehash: 3195f1f409ab5cb87cd0520192a3dd362e188a3f
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85610241"
---
# <a name="scaling-service-fabric-standalone-clusters"></a>Escalado de clústeres independientes de Service Fabric
Un clúster de Service Fabric es un conjunto de máquinas físicas o virtuales conectadas a la red, en las que se implementan y administran los microservicios. Un equipo o máquina virtual que forma parte de un clúster se denomina nodo. Los clústeres pueden contener potencialmente miles de nodos. Después de crear un clúster de Service Fabric, puede escalar el clúster horizontalmente (cambiar el número de nodos) o verticalmente (cambiar los recursos de los nodos).  Puede escalar el clúster en cualquier momento, incluso con cargas de trabajo en ejecución en el clúster.  Según se escala el clúster, las aplicaciones se escalan automáticamente.

¿Por qué escalar el clúster? Las necesidades de las aplicaciones cambian a lo largo del tiempo.  Puede necesitar aumentar los recursos del clúster para cumplir con aumentos del tráfico de red o de la carga de trabajo de la aplicación o reducir los recursos del clúster cuando la demanda baja.

## <a name="scaling-in-and-out-or-horizontal-scaling"></a>Escalado horizontal
Cambia el número de nodos del clúster.  Una vez que los nuevos nodos se unen al clúster, el [Administrador de recursos del clúster](service-fabric-cluster-resource-manager-introduction.md) mueve servicios a ellos, lo que reduce la carga en los nodos existentes.  Si los nodos del clúster no se usan de forma eficaz, puede reducir el número de nodos.  Cuando los nodos abandonan el clúster, los servicios son trasladados de dichos nodos y la carga aumenta en los nodos restantes.  La reducción del número de nodos en un clúster que se ejecuta en Azure puede ahorrarle dinero, ya que se paga por el número de máquinas virtuales utilizadas y no por la carga de trabajo en esas máquinas virtuales.  

- Ventajas: escala infinita, en teoría.  Si la aplicación está diseñada para ofrecer escalabilidad, puede habilitar un crecimiento ilimitado agregando más nodos.  Las herramientas de los entornos de nube permiten agregar o quitar nodos fácilmente, por lo que resulta sencillo ajustar la capacidad y solo se paga por los recursos que utiliza.  
- Desventajas: las aplicaciones deben [diseñarse para ofrecer escalabilidad](service-fabric-concepts-scalability.md).  Las bases de datos y la persistencia de la aplicación pueden requerir un trabajo de arquitectura adicional para el escalado.  Las[colecciones confiables](service-fabric-reliable-services-reliable-collections.md) de los servicios con estado de Service Fabric, sin embargo, hacen mucho más fácil el escalado de los datos de la aplicación.

Los clústeres independientes le permiten implementar el clúster de Service Fabric de modo local o en el proveedor en la nube de su elección.  Los tipos de nodo están formados por máquinas físicas o virtuales, según la implementación. En comparación con los clústeres que se ejecutan en Azure, el proceso de escalado de un clúster independiente es un poco más complicado.  Debe cambiar manualmente el número de nodos del clúster y, a continuación, ejecutar una actualización de la configuración de clúster.

La eliminación de nodos puede iniciar varias actualizaciones. Algunos nodos están marcados con la etiqueta `IsSeedNode=”true”` y se pueden identificar consultando el manifiesto del clúster mediante [Get-ServiceFabricClusterManifest](/powershell/module/servicefabric/get-servicefabricclustermanifest). La eliminación de estos nodos puede tardar más que la de otros, ya que se tendrán que mover los nodos de inicialización en estos escenarios. El clúster debe tener un mínimo de tres nodos del tipo de nodo principal.

> [!WARNING]
> Se recomienda no reducir el número de nodos por debajo del [tamaño del clúster del nivel de confiabilidad](service-fabric-cluster-capacity.md#reliability-characteristics-of-the-cluster) del clúster. Esto interferirá con la capacidad de los servicios del sistema de Service Fabric para replicarse en el clúster y llegar a desestabilizar o, posiblemente, destruir el clúster.
>

Al escalar un clúster independiente, tenga en cuenta las directrices siguientes:
- Los nodos principales se deben sustituir uno por uno, en lugar de quitarlos y agregarlos de forma masiva.
- Antes de eliminar un tipo de nodo, compruebe que no hay algún nodo que haga referencia a dicho tipo. Elimine estos nodos antes de quitar el tipo de nodo correspondiente. Una vez se han eliminado todos los nodos correspondientes, puede eliminar el valor de NodeType de la configuración del clúster y comenzar una actualización de la configuración mediante [Start-ServiceFabricClusterConfigurationUpgrade](/powershell/module/servicefabric/start-servicefabricclusterconfigurationupgrade).

Para más información, consulte [Escalado de un clúster independiente](service-fabric-cluster-windows-server-add-remove-nodes.md).

## <a name="scaling-up-and-down-or-vertical-scaling"></a>Escalado vertical 
Cambia los recursos (CPU, memoria o almacenamiento) de los nodos del clúster.
- Ventajas: el software y la arquitectura de la aplicación siguen siendo los mismos.
- Desventajas: escala finita, ya que hay un límite respecto a cuánto puede aumentar los recursos en los nodos individuales. Tiempo de inactividad, ya que tendrá máquinas físicas o virtuales sin conexión con el fin de agregar o quitar recursos.

## <a name="next-steps"></a>Pasos siguientes
* Obtenga información sobre [escalabilidad de aplicaciones](service-fabric-concepts-scalability.md).
* [Escalado horizontal de un clúster de Azure](service-fabric-tutorial-scale-cluster.md).
* [Escalado de un clúster de Azure mediante programación](service-fabric-cluster-programmatic-scaling.md) con el SDK de proceso de Azure.
* [Escalado o reducción horizontal de un clúster independiente](service-fabric-cluster-windows-server-add-remove-nodes.md)


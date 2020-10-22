---
title: Instancias de contenedor y orquestación de contenedores
description: Descripción de cómo Azure Container Instances interactúa con los orquestadores de contenedores.
ms.topic: article
ms.date: 04/15/2019
ms.custom: mvc
ms.openlocfilehash: c7faeecfcc3a1d006cc923b48339a1242a0b9e9b
ms.sourcegitcommit: dbe434f45f9d0f9d298076bf8c08672ceca416c6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/17/2020
ms.locfileid: "92148604"
---
# <a name="azure-container-instances-and-container-orchestrators"></a>Azure Container Instances y orquestadores de contenedores

Los contenedores, debido a su tamaño pequeño y a la orientación de aplicaciones, se adaptan perfectamente a entornos de entrega ágil y arquitecturas basadas en microservicios. La tarea de automatizar y administrar una gran cantidad de contenedores y cómo interactúan se conoce como *orquestación*. Entre los orquestadores de contenedores más populares se encuentran Kubernetes, DC/OS y Docker Swarm.

Azure Container Instances proporciona algunas de las funcionalidades básicas de programación de plataformas de orquestación. Y mientras no se cubran los servicios de más valor que esas plataformas proporcionan, Azure Container Instances puede complementarlos. En este artículo se describe el ámbito de los aspectos que controla Azure Container Instances y cómo los orquestadores de contenedores completos pueden interactuar con él.

## <a name="traditional-orchestration"></a>Orquestación tradicional

La definición estándar de orquestación incluye las tareas siguientes:

- **Programación**: en una solicitud de recurso e imagen de contenedor, busque una máquina adecuada en la cual ejecutar el contenedor.
- **Afinidad/antiafinidad**: especifique que un conjunto de contenedores se deben ejecutar de manera cercana entre sí (para mejorar el rendimiento) o con la distancia suficiente como para mejorar la disponibilidad.
- **Seguimiento del estado**: inspeccione los contenedores por si existen errores y vuelva a programarlos.
- **Conmutación por error**: realice un seguimiento de qué se ejecuta en cada máquina y vuelva a programar los contenedores desde máquinas con error a nodos en estado correcto.
- **Escalado**: agregue o quite instancias de contenedores para coincidir con la demanda, ya sea manual o automáticamente.
- **Redes**: proporcione una red superpuesta para coordinar los contenedores a fin de que se comuniquen entre varios equipos host.
- **Detección de servicios**: permita que los contenedores se localicen entre sí de manera automática incluso cuando se mueven entre equipos host y cambian las direcciones IP.
- **Actualizaciones de aplicaciones coordinadas**: administre las actualizaciones de contenedores para evitar el tiempo de inactividad de las aplicaciones y permitir la reversión si algo sale mal.

## <a name="orchestration-with-azure-container-instances-a-layered-approach"></a>Orquestación con Azure Container Instances: un enfoque por niveles

Azure Container Instances permite un enfoque por niveles a la orquestación, lo que proporciona todas las funcionalidades de programación y administración que se requieren para ejecutar un solo contenedor mientras se permite que las plataformas de orquestadores administren tareas de varios contenedores sobre él.

Dado que Azure administra la infraestructura subyacente de Container Instances, una plataforma de orquestadores no necesita ocuparse de encontrar un equipo host adecuado en el cual ejecutar un solo contenedor. La elasticidad de la nube garantiza que siempre haya uno disponible. En lugar de eso, el orquestador se puede centrar en las tareas que simplifican la implementación de arquitecturas con varios contenedores, incluidas actualizaciones coordinadas y escalado.

## <a name="scenarios"></a>Escenarios

Si bien la integración de los orquestadores con Azure Container Instances todavía es incipiente, podemos prever que surgirán algunos entornos distintos:

### <a name="orchestration-of-container-instances-exclusively"></a>Orquestación exclusiva de Container Instances

Dado que se inician rápidamente y se facturan por segundo, un entorno basado exclusivamente en Azure Container Instances ofrece la forma más rápida de comenzar usar y trabajar con cargas de trabajo muy variables.

### <a name="combination-of-container-instances-and-containers-in-virtual-machines"></a>Combinación de Container Instances y contenedores en Virtual Machines

En el caso de cargas de trabajo estables y de ejecución prolongada, orquestar contenedores en un clúster de máquinas virtuales dedicadas habitualmente es más barato que ejecutar los mismos contenedores con Azure Container Instances. Sin embargo, Container Instances ofrece una solución excelente para expandir y contraer rápidamente la capacidad total para enfrentar las subidas inesperadas o breves en el uso.

En lugar de escalar horizontalmente el número de máquinas virtuales en el clúster y luego implementar contenedores adicionales en esas máquinas, el orquestador puede simplemente programar los contenedores adicionales con Azure Container Instances y eliminarlos una vez que deje de necesitarlos.

## <a name="sample-implementation-virtual-nodes-for-azure-kubernetes-service-aks"></a>Implementación de ejemplo: nodos virtuales para Azure Kubernetes Service (AKS)

Para escalar rápidamente las cargas de trabajo de aplicación en un clúster de [Azure Kubernetes Service](../aks/intro-kubernetes.md), puede usar *nodos virtuales* creados de manera dinámica en Azure Container Instances. Los nodos virtuales permiten la comunicación de red entre los pods que se ejecutan en ACI y el clúster de AKS. 

Actualmente, estos nodos virtuales admiten instancias de contenedor de Linux. Empiece a trabajar con los nodos virtuales mediante la [CLI de Azure](../aks/virtual-nodes-cli.md) o [Azure Portal](../aks/virtual-nodes-portal.md).

Los nodos virtuales utilizan el código abierto [Virtual Kubelet][aci-connector-k8s] para imitar al [kubelet][kubelet-doc] de Kubernetes al registrarse como un nodo con capacidad ilimitada. Virtual Kubelet envía la creación de [pods][pod-doc] como grupos de contenedores en Azure Container Instances.

Consulte el proyecto [Virtual Kubelet](https://github.com/virtual-kubelet/virtual-kubelet) para obtener ejemplos adicionales de la ampliación de la API de Kubernetes en plataformas de contenedores sin servidor.

## <a name="next-steps"></a>Pasos siguientes

Use la [guía de inicio rápido](container-instances-quickstart.md) para crear el primer contenedor con Azure Container Instances.

<!-- IMAGES -->

<!-- LINKS -->
[aci-connector-k8s]: https://github.com/virtual-kubelet/azure-aci
[kubelet-doc]: https://kubernetes.io/docs/admin/kubelet/
[pod-doc]: https://kubernetes.io/docs/concepts/workloads/pods/pod/
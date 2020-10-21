---
title: Información general de clústeres de Service Fabric en Azure e independientes
description: Puede crear clústeres de Service Fabric en cualquier VM o equipo con Windows Server o Linux. Esto significa que podrá implementar y ejecutar aplicaciones de Service Fabric en cualquier entorno donde haya un conjunto de equipos de Windows Server o Linux que estén conectados entre sí, ya sea de manera local, en Microsoft Azure o con algún proveedor en la nube.
ms.topic: conceptual
ms.date: 01/07/2020
ms.custom: sfrev
ms.openlocfilehash: 8313d75bb6398bbe614c6f56b494079548c99954
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "91842604"
---
# <a name="comparing-azure-and-standalone-service-fabric-clusters-on-windows-server-and-linux"></a>Comparación de clústeres de Service Fabric en Azure e independientes en Windows Server y Linux

Un clúster de Service Fabric es un conjunto de máquinas físicas o virtuales conectadas a la red, en las que se implementan y administran los microservicios. Una máquina física o virtual que forma parte de un clúster se denomina nodo del clúster. Los clústeres pueden escalarse a miles de nodos. Si agrega nuevos nodos al clúster, Service Fabric reequilibra las réplicas e instancias de la partición del servicio en el número aumentado de nodos. El rendimiento general de la aplicación mejora y se reduce la contención para el acceso a la memoria. Si los nodos del clúster no se usan de forma eficaz, puede reducir su número de nodos. Service Fabric vuelve a reequilibrar las réplicas e instancias de la partición en el número reducido de nodos para aprovechar mejor el hardware de cada nodo.

Service Fabric permite la creación de clústeres de Service Fabric en cualquier máquina virtual o equipo con Windows Server o Linux. Esto significa que podrá implementar y ejecutar aplicaciones de Service Fabric en cualquier entorno donde haya un conjunto de equipos de Windows Server o Linux que estén conectados entre sí, ya sea de manera local, en Microsoft Azure o con algún proveedor en la nube.

## <a name="benefits-of-clusters-on-azure"></a>Ventajas de clústeres en Azure

En Azure se proporciona integración con otras características y servicios de Azure, lo que facilita las operaciones y la administración del clúster y hace que sea más confiable.

* **Azure Portal:** Azure Portal facilita la creación y la administración de clústeres.
* **Azure Resource Manager:** el uso de Azure Resource Manager permite una administración sencilla de todos los recursos que usa el clúster como una unidad y simplifica el costo del seguimiento y la facturación.
* **Clúster de Service Fabric como recurso de Azure** Un clúster de Service Fabric es un recurso de Azure, de modo que puede ajustarlo como hace con otros recursos en Azure.
* **Integración con la infraestructura de Azure** : Service Fabric se coordina con la infraestructura de Azure subyacente para el SO, la red y otras actualizaciones con el fin de mejorar la disponibilidad y la confiabilidad de las aplicaciones.  
* **Diagnósticos:** en Azure se proporciona la integración con Azure Diagnostics y los registros de Azure Monitor.
* **Escalabilidad automática:** para los clústeres hospedados en Azure, proporcionamos una funcionalidad integrada de escalabilidad automática a través de conjuntos de escalas de la máquina virtual. En los entornos locales y otros entornos en la nube, hay que crear una característica propia de escalabilidad automática o realizar el escalado manualmente mediante las API que ofrece Service Fabric para escalar los clústeres.

## <a name="benefits-of-standalone-clusters"></a>Ventajas de los clústeres independientes

* Es libre de elegir cualquier proveedor en la nube para hospedar el clúster.
* Las aplicaciones de Service Fabric, una vez escritas, se pueden ejecutar en varios entornos de hospedaje con pocos cambios (o ninguno).
* El proceso de creación de aplicaciones de Service Fabric se aplica de un entorno de hospedaje a otro.
* La experiencia operativa que se adquiere al ejecutar y administrar clústeres de Service Fabric se puede aplicar de un entorno a otro.
* Existe una amplia cobertura de clientes sin limitaciones derivadas de las restricciones del entorno de hospedaje.
* Hay un nivel adicional de confiabilidad y protección contra interrupciones generalizadas al poder mover los servicios a otro entorno de implementación en caso de que un centro de datos o un proveedor de nube sufran una interrupción.

## <a name="next-steps"></a>Pasos siguientes

* Lea la introducción a [clústeres de Service Fabric en Azure](service-fabric-azure-clusters-overview.md)
* Lea la introducción a [clústeres de Service Fabric independientes](service-fabric-standalone-clusters-overview.md)
* Más información sobre las [opciones de soporte técnico de Service Fabric](service-fabric-support.md)
---
title: Clústeres administrados de Service Fabric (versión preliminar)
description: Los clústeres administrados de Service Fabric son una evolución del modelo de recursos de clúster de Azure Service Fabric que agiliza la implementación y la administración de clústeres.
ms.topic: overview
ms.date: 09/28/2020
ms.openlocfilehash: 3d26a92126491662d5c51b3c4e8900ffa547f830
ms.sourcegitcommit: b48e8a62a63a6ea99812e0a2279b83102e082b61
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/28/2020
ms.locfileid: "91410402"
---
# <a name="service-fabric-managed-clusters-preview"></a>Clústeres administrados de Service Fabric (versión preliminar)

Los clústeres administrados de Service Fabric son una evolución del modelo de recursos de clúster de Azure Service Fabric que agiliza la experiencia de implementación y administración de clústeres.

La plantilla del modelo de recursos de Azure (ARM) para los clústeres de Service Fabric tradicionales requiere la definición de un recurso de clúster junto con una serie de recursos de soporte técnico. Todos ellos deben estar "conectados" correctamente (durante la implementación y a lo largo del ciclo de vida del clúster) para que el clúster y los servicios funcionen correctamente. Por el contrario, el modelo de encapsulación para clústeres administrados de Service Fabric consta de un único recurso de *clúster administrado de Service Fabric*. Todos los recursos subyacentes para el clúster se abstraen y se administran mediante Azure en su nombre.

**Modelo de clúster tradicional de Service Fabric**
![Modelo de clúster tradicional de Service Fabric][sf-composition]

**Modelo de clúster administrado de Service Fabric**
![Modelo de clúster encapsulado de Service Fabric][sf-encapsulation]

En lo que se refiere al tamaño y la complejidad, la plantilla de ARM para un clúster administrado de Service Fabric consiste en alrededor de 100 líneas de JSON, frente a las 1000 líneas necesarias aproximadamente para definir un clúster de Service Fabric típico:

| Recursos de Service Fabric | Recursos de clúster administrado de Service Fabric |
|----------|-----------|
| Clúster de Service Fabric | Clúster administrado de Service Fabric |
| Conjuntos de escalado de máquinas virtuales | |
| Equilibrador de carga | |
| Dirección IP pública | |
| Cuentas de almacenamiento | |
| Virtual network | |

Los clústeres administrados de Service Fabric proporcionan una serie de ventajas respecto a los clústeres tradicionales:

**Implementación y administración simplificadas del clúster**
- Implementación y administración de un único recurso de Azure
- Administración y rotación automática de certificados
- Operaciones de escalado simplificadas

**Prevención de errores operativos**
- Prevención de errores de coincidencia de configuración con recursos subyacentes
- Bloqueo de operaciones no seguras (como la eliminación de un nodo de inicialización)

**Procedimientos recomendados de manera predeterminada**
- Configuración simplificada de la confiabilidad y la durabilidad

No hay ningún costo adicional por los clústeres administrados de Service Fabric más allá del costo de los recursos subyacentes necesarios para el clúster.

## <a name="service-fabric-managed-cluster-skus"></a>SKU de clúster administrado de Service Fabric

Los clústeres administrados de Service Fabric están disponibles en las SKU de nivel básico y estándar.

| Característica | Básico | Estándar |
| ------- | ----- | -------- |
| Recurso de red (SKU para [Load Balancer](../load-balancer/skus.md), [IP pública](../virtual-network/public-ip-addresses.md)) | Básico | Estándar |
| Número mínimo de nodos (instancia de máquina virtual) | 3 | 5 |
| Número máximo de nodos por tipo de nodo | 100 | 100 |
| Número máximo de tipos de nodos | 1 | 20 |
| Adición o eliminación de tipos de nodos | No | Sí |
| Redundancia de zona | No | Sí |

## <a name="service-fabric-managed-cluster-feature-roadmap"></a>Hoja de ruta de características de un clúster administrado de Service Fabric
Como se trata de una versión preliminar anticipada de los clústeres administrados de Service Fabric, hay algunas lagunas que se deben tener en cuenta. Estas características estarán disponibles en futuras versiones. 

* Publicación de aplicaciones en clústeres directamente desde Visual Studio
* Identidades administradas 
* Implementaciones de la aplicación de ARM 
* Zonas de disponibilidad 
* Proxy inverso 
* Ajuste de escala automático 
* Actualización de reglas de grupo de seguridad de red 
* Actualizaciones automáticas del sistema operativo

## <a name="next-steps"></a>Pasos siguientes

Para empezar a trabajar con los clústeres administrados de Service Fabric, pruebe el inicio rápido:

> [!div class="nextstepaction"]
> [Creación de un clúster administrado de Service Fabric (versión preliminar)](quickstart-managed-cluster-template.md)


[sf-composition]: ./media/overview-managed-cluster/sfrp-composition-resource.png
[sf-encapsulation]: ./media/overview-managed-cluster/sfrp-encapsulated-resource.png
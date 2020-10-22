---
title: Preguntas frecuentes sobre los clústeres administrados de Service Fabric
description: Preguntas frecuentes sobre los clústeres administrados de Service Fabric, incluidas las funcionalidades, los casos de uso y los escenarios comunes.
ms.topic: troubleshooting
ms.author: pepogors
author: peterpogorski
ms.date: 09/28/2020
ms.custom: references_regions
ms.openlocfilehash: 4dc41d2c13c834657534971041440bb744cfca38
ms.sourcegitcommit: ce8eecb3e966c08ae368fafb69eaeb00e76da57e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/21/2020
ms.locfileid: "92319820"
---
# <a name="service-fabric-managed-clusters-frequently-asked-questions"></a>Preguntas más frecuentes sobre los clústeres administrados de Service Fabric

Hay muchas preguntas comunes sobre lo que los clústeres administrados de Service Fabric (versión preliminar) pueden hacer y cómo deben usarse. En este documento se incluyen muchas de esas preguntas y sus respuestas.

## <a name="general"></a>General

### <a name="what-are-service-fabric-managed-clusters"></a>¿Qué son los clústeres administrados de Service Fabric?

Los clústeres administrados de Service Fabric son una evolución del modelo de recursos de clúster de Service Fabric diseñado para facilitar la implementación y administración de los clústeres. Un clúster administrado de Service Fabric usa el modelo de encapsulación de Azure Resource Manager para que un usuario solo necesite definir e implementar un único recurso de clúster en comparación con los muchos recursos independientes que deben implementar hoy (conjunto de escalado de máquinas virtuales, Load Balancer, IP, etc.).

### <a name="what-regions-are-supported-in-the-preview"></a>¿Qué regiones se admiten en la versión preliminar?

Entre las regiones admitidas para la versión preliminar de clústeres administrados de Service Fabric se incluyen `centraluseuap`, `eastus2euap`, `eastasia`, `northeurope`, `westcentralus` y `eastus2`.

### <a name="can-i-do-an-in-place-migration-of-my-existing-service-fabric-cluster-to-a-managed-cluster-resource"></a>¿Puedo realizar una migración en contexto del clúster de Service Fabric existente a un recurso de clúster administrado?

En este momento, necesitaría crear un recurso de clúster de Service Fabric para usar el nuevo tipo de recurso de clúster administrado de Service Fabric.

### <a name="is-there-an-additional-cost-for-service-fabric-managed-clusters"></a>¿Hay un costo adicional para los clústeres administrados de Service Fabric?

No, no se aplica ningún costo adicional asociado con un clúster administrado de Service Fabric aparte del costo de los recursos de red, almacenamiento y proceso subyacentes necesarios para el clúster.

### <a name="is-there-a-new-sla-introduced-by-the-service-fabric-managed-cluster-resource"></a>¿El recurso de clúster administrado de Service Fabric incluye algún Acuerdo de Nivel de Servicio nuevo?

El Acuerdo de Nivel de Servicio no cambia respecto al modelo de recursos de Service Fabric actual.

### <a name="what-is-the-difference-between-a-basic-and-standard-sku-cluster"></a>¿Cuál es la diferencia entre un clúster de SKU Básico y Estándar?

Un clúster de SKU Básico supone que la mayoría de las configuraciones las proporciona el proveedor de recursos de Service Fabric. Los clústeres de SKU Básico están diseñados para usarse en entornos de prueba y preproducción. Un clúster de SKU Estándar permite a los usuarios configurar el clúster para satisfacer sus necesidades de forma específica. Para más información, vea [SKU de clúster administrado de Service Fabric](./overview-managed-cluster.md#service-fabric-managed-cluster-skus).

## <a name="cluster-deployment-and-management"></a>Implementación y administración de clústeres

### <a name="i-run-custom-script-extensions-on-my-virtual-machine-scale-set-can-i-continue-to-do-that-with-a-managed-service-fabric-resource"></a>Ejecuto extensiones de script personalizadas en mi conjunto de escalado de máquinas virtuales, ¿puedo continuar haciendo eso con un recurso de Service Fabric administrado?

Sí, todavía puede especificar las extensiones de máquina virtual en un tipo de nodo. Para más información, vea el ejemplo de extensión de tipo de nodo.

### <a name="i-want-to-have-an-internal-only-load-balancer-is-that-possible"></a>Quiero tener un equilibrador de carga solo interno, ¿es posible?

Actualmente no es posible tener un equilibrador de carga solo interno. Se recomienda consolidar las reglas del grupo de seguridad de red para bloquear el tráfico entrante o saliente no deseado.

### <a name="can-i-autoscale-my-cluster"></a>¿Puedo escalar mi clúster automáticamente? 
La escalabilidad automática no está disponible actualmente en la versión preliminar.

### <a name="can-i-deploy-my-cluster-across-availability-zones"></a>¿Puedo implementar mi clúster en zonas de disponibilidad? 
Los clústeres de zonas de disponibilidad cruzadas no están disponibles actualmente en la versión preliminar.

### <a name="can-i-select-between-automatic-and-manual-upgrades-for-my-cluster-runtime"></a>¿Puedo seleccionar entre actualizaciones automáticas y manuales del clúster en tiempo de ejecución? 
En la versión preliminar, todas las actualizaciones en tiempo de ejecución se completarán automáticamente.

## <a name="applications"></a>Aplicaciones

### <a name="is-there-a-local-development-experience-for-service-fabric-managed-clusters"></a>¿Hay alguna experiencia de desarrollo local para los clústeres administrados de Service Fabric?

La experiencia de desarrollo local no cambia respecto a los clústeres de Service Fabric existentes. Para más información, vea [Creación de una aplicación .NET](./service-fabric-quickstart-dotnet.md) para obtener más información sobre la experiencia de desarrollo local.

### <a name="can-i-deploy-my-applications-as-an-azure-resource-manager-resource"></a>¿Puedo implementar mis aplicaciones como un recurso de Azure Resource Manager?

En la versión preliminar, no puede implementar aplicaciones como un recurso de Azure Resource Manager. Las aplicaciones deben implementarse conectándose directamente al clúster mediante PowerShell o la CLI. Esta funcionalidad se agregará antes de que los clústeres de Service Fabric administrados se agreguen a la disponibilidad general.
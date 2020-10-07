---
title: Escalado horizontal de un clúster administrado de Service Fabric (versión preliminar)
description: En este tutorial aprenderá a escalar horizontalmente un tipo de nodo de un clúster administrado de Service Fabric.
ms.topic: tutorial
ms.date: 09/28/2020
ms.openlocfilehash: a6777743932f3cc73db973c4d9935d4b5827700b
ms.sourcegitcommit: b48e8a62a63a6ea99812e0a2279b83102e082b61
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/28/2020
ms.locfileid: "91410241"
---
# <a name="tutorial-scale-out-a-service-fabric-managed-cluster-preview"></a>Tutorial: Escalado horizontal de un clúster administrado de Service Fabric (versión preliminar)

En esta serie de tutoriales, trataremos de lo siguiente:

> [!div class="checklist"]
> * [Implementación de un clúster administrado de Service Fabric.](tutorial-managed-cluster-deploy.md)
> * Escalado horizontal de un clúster administrado de Service Fabric
> * [Incorporación y eliminación de tipos de nodo en un clúster administrado de Service Fabric](tutorial-managed-cluster-add-remove-node-type.md)
> * [Implementación de una aplicación en un clúster administrado de Service Fabric](tutorial-managed-cluster-deploy-app.md)

En esta parte de la serie se explica lo siguiente:

> [!div class="checklist"]
> * Escalado de un nodo de clúster administrado de Service Fabric

## <a name="prerequisites"></a>Requisitos previos

* Un clúster administrado de Service Fabric (consulte [*Implementación de un clúster administrado*](tutorial-managed-cluster-deploy.md)).
* [Azure PowerShell 4.7.0](https://docs.microsoft.com/powershell/azure/release-notes-azureps?view=azps-4.7.0&preserve-view=true#azservicefabric) o posterior (consulte [*Instalación de Azure PowerShell*](https://docs.microsoft.com/powershell/azure/install-az-ps?view=azps-4.7.0&preserve-view=true)).

## <a name="scale-a-service-fabric-managed-cluster"></a>Escalado de un clúster administrado de Service Fabric
Cambie el recuento de instancias para aumentar o disminuir el número de nodos del tipo de nodo que desea escalar. Puede buscar los nombres de tipo de nodo en la plantilla de Azure Resource Manager (plantilla de ARM) de la implementación del clúster o en Service Fabric Explorer.  

> [!NOTE]
> Si el tipo de nodo es principal, no podrá desplazarse por debajo de 3 nodos para un clúster de SKU básico y de 5 nodos, para un clúster de SKU estándar.

```powershell
$resourceGroup = "myResourceGroup"
$clusterName = "mysfcluster"
$nodeTypeName = "FE"
$instanceCount = "7"

Set-AzServiceFabricManagedNodeType -ResourceGroupName $resourceGroup -ClusterName $clusterName -name $nodeTypeName -InstanceCount $instanceCount -Verbose
```

El clúster comenzará a actualizarse automáticamente y, después de unos minutos, verá los nodos adicionales.

## <a name="next-steps"></a>Pasos siguientes

En este paso, hemos escalado un tipo de nodo en un clúster administrado de Service Fabric. Para más información sobre cómo agregar y eliminar tipos de nodo, consulte:

> [!div class="nextstepaction"]
> [Incorporación y eliminación de tipos de nodo en un clúster administrado de Service Fabric](tutorial-managed-cluster-add-remove-node-type.md)

---
title: Incorporación y eliminación de tipos de nodos en un clúster administrado de Service Fabric (versión preliminar)
description: En este tutorial aprenderá a agregar y quitar tipos de nodos de un clúster administrado de Service Fabric.
ms.topic: tutorial
ms.date: 09/28/2020
ms.openlocfilehash: 860345f5b297edaeea9d099ac392243176dfaca7
ms.sourcegitcommit: b48e8a62a63a6ea99812e0a2279b83102e082b61
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/28/2020
ms.locfileid: "91410253"
---
# <a name="tutorial-add-and-remove-node-types-from-a-service-fabric-managed-cluster-preview"></a>Tutorial: Incorporación y eliminación de tipos de nodos en un clúster administrado de Service Fabric (versión preliminar)

En este tutorial, trataremos de lo siguiente:

> [!div class="checklist"]
> * [Implementación de un clúster administrado de Service Fabric](tutorial-managed-cluster-deploy.md)
> * [Escalado horizontal de un clúster administrado de Service Fabric](tutorial-managed-cluster-scale.md)
> * Incorporación y eliminación de nodos de un clúster administrado de Service Fabric
> * [Implementación de una aplicación en un clúster administrado de Service Fabric](tutorial-managed-cluster-deploy-app.md)

En esta parte de la serie se explica lo siguiente:

> [!div class="checklist"]
> * Adición de un tipo de nodo a un clúster administrado de Azure Service Fabric
> * Eliminación de un tipo de nodo de un clúster administrado de Azure Service Fabric

## <a name="prerequisites"></a>Requisitos previos

* Un clúster administrado de Service Fabric (consulte [*Implementación de un clúster administrado*](tutorial-managed-cluster-deploy.md)).
* [Azure PowerShell 4.7.0](https://docs.microsoft.com/powershell/azure/release-notes-azureps?view=azps-4.7.0&preserve-view=true#azservicefabric) o posterior (consulte [*Instalación de Azure PowerShell*](https://docs.microsoft.com/powershell/azure/install-az-ps?view=azps-4.7.0&preserve-view=true)).

## <a name="add-a-node-type-to-a-service-fabric-managed-cluster"></a>Adición de un tipo de nodo a un clúster administrado de Azure Service Fabric

Puede agregar un tipo de nodo a un clúster administrado de Service Fabric a través de una plantilla de Azure Resource Manager, PowerShell o la CLI. En este tutorial, se va a agregar un tipo de nodo mediante Azure PowerShell.

Para crear un nuevo tipo de nodo, deberá definir tres propiedades:

* **Nombre del tipo de nodo**: nombre único de los tipos de nodos existentes en el clúster.
* **Recuento de instancias**: número inicial de nodos del nuevo tipo de nodo.
* **Tamaño de VM**: SKU de VM para los nodos. Si no se especifica, se usa el valor predeterminado *Standard_D2*.

> [!NOTE]
> Si el tipo de nodo que se va a agregar es el primero o el único tipo de nodo del clúster, se debe usar la propiedad principal.

```powershell
$resourceGroup = "myResourceGroup"
$clusterName = "mysfcluster"
$nodeTypeName = "nt2"
$vmSize = "Standard_D2_v2"

New-AzServiceFabricManagedNodeType -ResourceGroupName $resourceGroup -ClusterName $clusterName -Name $nodeTypeName -InstanceCount 3 -vmSize $vmSize
```

## <a name="remove-a-node-type-from-a-service-fabric-managed-cluster"></a>Eliminación de un tipo de nodo de un clúster administrado de Azure Service Fabric

Para quitar un tipo de nodo de un clúster administrado de Service Fabric, debe usar PowerShell o la CLI. En este tutorial, se va a quitar un tipo de nodo mediante Azure PowerShell.

> [!NOTE]
> No es posible quitar un tipo de nodo principal si es el único tipo de nodo principal del clúster.  

Para quitar un tipo de nodo:

```powershell
$resourceGroup = "myResourceGroup"
$clusterName = "myCluster"
$nodeTypeName = "nt2"

Remove-AzServiceFabricManagedNodeType -ResourceGroupName $resourceGroup -ClusterName $clusterName  -Name $nodeTypeName
```

## <a name="next-steps"></a>Pasos siguientes

 En esta sección, hemos agregado y eliminado tipos de nodos. Para obtener más información sobre cómo implementar una aplicación en un clúster administrado de Service Fabric, consulte:

> [!div class="nextstepaction"]
> [Implementación de una aplicación en un clúster administrado de Service Fabric](tutorial-managed-cluster-deploy-app.md)

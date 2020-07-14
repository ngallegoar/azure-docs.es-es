---
title: Uso de grupos de nodos del sistema en Azure Kubernetes Service (AKS)
description: Aprenda a crear y administrar grupos de nodos del sistema en Azure Kubernetes Service (AKS).
services: container-service
ms.topic: article
ms.date: 06/18/2020
ms.author: mlearned
ms.openlocfilehash: 9b6270f81e7af8bd508d29510698e6cf9a5a2010
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85052648"
---
# <a name="manage-system-node-pools-in-azure-kubernetes-service-aks"></a>Administración de grupos de nodos del sistema en Azure Kubernetes Service (AKS)

En Azure Kubernetes Service, los nodos de la misma configuración se agrupan en *grupos de nodos*. Los grupos de nodos contienen las máquinas virtuales subyacentes que ejecutan las aplicaciones. Los grupos de nodos del sistema y los grupos de nodos del usuario son dos modos de grupos de nodos diferentes para los clústeres de AKS. Los grupos de nodos del sistema tienen el propósito principal de hospedar los pods críticos del sistema, como CoreDNS y tunnelfront. Los grupos de nodos de usuario tienen el propósito principal de hospedar los pods de aplicación. Sin embargo, se pueden programar pods de aplicación en grupos de nodos del sistema si quiere tener solo un grupo en el clúster de AKS. Cada clúster de AKS debe contener al menos un grupo de nodos del sistema con al menos un nodo.

> [!Important]
> Si ejecuta un único grupo de nodos del sistema para el clúster de AKS en un entorno de producción, se recomienda usar al menos tres nodos para el grupo de nodos.

## <a name="before-you-begin"></a>Antes de empezar

* Es preciso que esté instalada y configurada la versión 2.3.1 de la CLI de Azure, o cualquier otra posterior. Ejecute `az --version` para encontrar la versión. Si necesita instalarla o actualizarla, vea [Instalación de la CLI de Azure][install-azure-cli].

## <a name="limitations"></a>Limitaciones

Se aplican las siguientes limitaciones cuando crea y administra clústeres de AKS que admiten grupos de nodos del sistema.

* Consulte [Cuotas, restricciones de tamaño de máquinas virtuales y disponibilidad de regiones en Azure Kubernetes Service (AKS)][quotas-skus-regions].
* El clúster de AKS se debe crear con conjuntos de escalado de máquinas virtuales como tipo de máquina virtual.
* El nombre de un grupo de nodos solo puede contener caracteres alfanuméricos en minúsculas y debe comenzar con una letra minúscula. En el caso de los grupos de nodos de Linux, la longitud debe estar comprendida entre 1 y 12 caracteres. En el caso de los grupos de nodos de Windows, la longitud debe estar comprendida entre 1 y 6 caracteres.
* Se debe usar una versión de API de 2020-03-01 o superior para establecer un modo de grupo de nodos. Los clústeres creados con versiones de API anteriores a la 2020-03-01 solo contienen grupos de nodos de usuario, pero se pueden migrar para que contengan grupos de nodos del sistema si sigue las [instrucciones del actualización del modo de grupo](#update-existing-cluster-system-and-user-node-pools).
* El modo de un grupo de nodos es una propiedad obligatoria y se debe establecer explícitamente cuando se usan plantillas de Resource Manager o llamadas API directas.

## <a name="system-and-user-node-pools"></a>Grupos de nodos del sistema y del usuario

Cada nodo del grupo de nodos del sistema tiene la etiqueta **kubernetes.azure.com/mode: system**. Cada clúster de AKS contiene al menos un grupo de nodos del sistema. Los grupos de nodos del sistema tienen las siguientes restricciones:

* El valor de osType para los grupos del sistema debe ser Linux.
* En el caso de los grupos de nodos del usuario puede ser Linux o Windows.
* Los grupos del sistema deben contener al menos un nodo mientras que los grupos de nodos del usuario pueden contener varios nodos o ninguno.
* Los grupos de nodos del sistema requieren una SKU de máquina virtual de 2 vCPUs y 4 GB de memoria como mínimo.
* Los grupos de nodos del sistema deben admitir al menos 30 pods como se indica en la [fórmula del valor mínimo y máximo de los pods][maximum-pods].
* Los grupos de nodos de zona requieren grupos de nodos del usuario.

Puede hacer las siguientes operaciones con los grupos de nodos:

* Cambiar un grupo de nodos del sistema para que sea un grupo de nodos del usuario, siempre que disponga de otro grupo de nodos del sistema que ocupe su lugar en el clúster de AKS.
* Cambiar un grupo de nodos del usuario para que sea un grupo de nodos del sistema.
* Eliminar grupos de nodos del usuario.
* Puede eliminar grupos de nodos del sistema, siempre que disponga de otro grupo de nodos del sistema que ocupe su lugar en el clúster de AKS.
* Un clúster de AKS puede incluir varios grupos de nodos del sistema y requiere uno como mínimo.
* Si quiere cambiar varias opciones de configuración inmutables en los grupos de nodos existentes, puede crear nuevos grupos de nodos para reemplazarlas. Por ejemplo, puede agregar un nuevo grupo de nodos con una nueva opción de configuración maxPods y eliminar el grupo de nodos anterior.

## <a name="create-a-new-aks-cluster-with-a-system-node-pool"></a>Creación de un nuevo clúster de AKS con un grupo de nodos del sistema

Cuando crea un nuevo clúster de AKS, crea automáticamente un grupo de nodos del sistema con un único nodo. De forma predeterminada, el grupo de nodos inicial es del tipo sistema. Cuando crea nuevos grupos de nodos con az aks nodepool add, estos serán grupos de nodos del usuario a menos que especifique explícitamente lo contrario en el parámetro de modo.

En el ejemplo siguiente, se crea un grupo de recursos denominado *myResourceGroup* en la región *eastus*.

```azurecli-interactive
az group create --name myResourceGroup --location eastus
```

Use el comando [az aks create][az-aks-create] para crear un clúster de AKS. En el siguiente ejemplo se crea un clúster denominado *myAKSCluster* con un grupo del sistema que contiene un nodo. Para las cargas de trabajo de producción, asegúrese de que usa grupos de nodos del sistema con al menos tres nodos. Esta operación puede tardar varios minutos en completarse.

```azurecli-interactive
az aks create -g myResourceGroup --name myAKSCluster --node-count 1 --generate-ssh-keys
```

## <a name="add-a-system-node-pool-to-an-existing-aks-cluster"></a>Incorporación de un grupo de nodos del sistema a un clúster de AKS existente

Puede agregar uno o varios grupos de nodos del sistema a los clústeres de AKS existentes. El siguiente comando le permite agregar un grupo de nodos del tipo sistema con un número predeterminado de tres nodos.

```azurecli-interactive
az aks nodepool add -g myResourceGroup --cluster-name myAKSCluster -n mynodepool --mode system
```
## <a name="show-details-for-your-node-pool"></a>Visualización de detalles del grupo de nodos

Puede comprobar los detalles del grupo de nodos con el siguiente comando.  

```azurecli-interactive
az aks nodepool show -g myResourceGroup --cluster-name myAKSCluster -n mynodepool
```

Un modo del tipo **sistema** se define para los grupos de nodos del sistema y uno del tipo **usuario** para los del usuario.

```output
{
  "agentPoolType": "VirtualMachineScaleSets",
  "availabilityZones": null,
  "count": 3,
  "enableAutoScaling": null,
  "enableNodePublicIp": false,
  "id": "/subscriptions/666d66d8-1e43-4136-be25-f25bb5de5883/resourcegroups/myResourceGroup/providers/Microsoft.ContainerService/managedClusters/myAKSCluster/agentPools/mynodepool",
  "maxCount": null,
  "maxPods": 110,
  "minCount": null,
  "mode": "System",
  "name": "mynodepool",
  "nodeLabels": {},
  "nodeTaints": null,
  "orchestratorVersion": "1.15.10",
  "osDiskSizeGb": 100,
  "osType": "Linux",
  "provisioningState": "Succeeded",
  "resourceGroup": "myResourceGroup",
  "scaleSetEvictionPolicy": null,
  "scaleSetPriority": null,
  "spotMaxPrice": null,
  "tags": null,
  "type": "Microsoft.ContainerService/managedClusters/agentPools",
  "vmSize": "Standard_DS2_v2",
  "vnetSubnetId": null
}
```

## <a name="update-existing-cluster-system-and-user-node-pools"></a>Actualización de los grupos de nodos de usuario y del sistema en clústeres existentes

> [!NOTE]
> Se debe usar la versión de API 2020-03-01 o posterior para configurar el modo de grupo de nodos del sistema. Como resultado, los clústeres creados en versiones de API anteriores a la 2020-03-01 solo contienen grupos de nodos de usuario. Para obtener la funcionalidad y las ventajas del grupo de nodos del sistema en clústeres anteriores, actualice el modo de los grupos de nodos existentes con los siguientes comandos en la última versión de la CLI de Azure.

Puede cambiar los modos para los grupos de nodos del sistema y del usuario. Solo puede cambiar un grupo de nodos del sistema para que sea uno del usuario si ya existe otro grupo de nodos del sistema en el clúster de AKS.

Este comando cambia un grupo de nodos del sistema en un grupo de nodos del usuario.

```azurecli-interactive
az aks nodepool update -g myResourceGroup --cluster-name myAKSCluster -n mynodepool --mode user
```

Este comando cambia un grupo de nodos del usuario en un grupo de nodos del sistema.

```azurecli-interactive
az aks nodepool update -g myResourceGroup --cluster-name myAKSCluster -n mynodepool --mode system
```

## <a name="delete-a-system-node-pool"></a>Eliminación de un grupo de nodos del sistema

> [!Note]
> Para usar grupos de nodos del sistema en clústeres de AKS con una versión de API anterior a 2020-03-02, agregue un nuevo grupo de nodos del sistema y, a continuación, elimine el grupo de nodos predeterminado original.

Anteriormente no podía eliminar el grupo de nodos del sistema que constituía el grupo de nodos predeterminado inicial de un clúster de AKS. Ahora dispone de la opción de eliminar cualquier grupo de nodos de los clústeres. Dado que los clústeres de AKS requieren al menos un grupo de nodos del sistema, debe contar con al menos dos grupos de nodos del sistema en el clúster de AKS antes de poder eliminar uno de ellos.

```azurecli-interactive
az aks nodepool delete -g myResourceGroup --cluster-name myAKSCluster -n mynodepool
```

## <a name="next-steps"></a>Pasos siguientes

En este artículo ha aprendido a crear y administrar grupos de nodos del sistema en un clúster de AKS. Para más información acerca de cómo usar varios grupos de nodos, consulte [Uso de grupos de varios nodos][use-multiple-node-pools].

<!-- EXTERNAL LINKS -->
[kubernetes-drain]: https://kubernetes.io/docs/tasks/administer-cluster/safely-drain-node/
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[kubectl-taint]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#taint
[kubectl-describe]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#describe
[kubernetes-labels]: https://kubernetes.io/docs/concepts/overview/working-with-objects/labels/
[kubernetes-label-syntax]: https://kubernetes.io/docs/concepts/overview/working-with-objects/labels/#syntax-and-character-set

<!-- INTERNAL LINKS -->
[aks-windows]: windows-container-cli.md
[az-aks-get-credentials]: /cli/azure/aks#az-aks-get-credentials
[az-aks-create]: /cli/azure/aks#az-aks-create
[az-aks-nodepool-add]: /cli/azure/aks/nodepool?view=azure-cli-latest#az-aks-nodepool-add
[az-aks-nodepool-list]: /cli/azure/aks/nodepool?view=azure-cli-latest#az-aks-nodepool-list
[az-aks-nodepool-update]: /cli/azure/aks/nodepool?view=azure-cli-latest#az-aks-nodepool-update
[az-aks-nodepool-upgrade]: /cli/azure/aks/nodepool?view=azure-cli-latest#az-aks-nodepool-upgrade
[az-aks-nodepool-scale]: /cli/azure/aks/nodepool?view=azure-cli-latest#az-aks-nodepool-scale
[az-aks-nodepool-delete]: /cli/azure/aks/nodepool?view=azure-cli-latest#az-aks-nodepool-delete
[az-extension-add]: /cli/azure/extension#az-extension-add
[az-extension-update]: /cli/azure/extension#az-extension-update
[az-group-create]: /cli/azure/group#az-group-create
[az-group-delete]: /cli/azure/group#az-group-delete
[az-group-deployment-create]: /cli/azure/group/deployment#az-group-deployment-create
[gpu-cluster]: gpu-cluster.md
[install-azure-cli]: /cli/azure/install-azure-cli
[operator-best-practices-advanced-scheduler]: operator-best-practices-advanced-scheduler.md
[quotas-skus-regions]: quotas-skus-regions.md
[supported-versions]: supported-kubernetes-versions.md
[tag-limitation]: ../azure-resource-manager/resource-group-using-tags.md
[taints-tolerations]: operator-best-practices-advanced-scheduler.md#provide-dedicated-nodes-using-taints-and-tolerations
[vm-sizes]: ../virtual-machines/linux/sizes.md
[use-multiple-node-pools]: use-multiple-node-pools.md
[maximum-pods]: configure-azure-cni.md#maximum-pods-per-node
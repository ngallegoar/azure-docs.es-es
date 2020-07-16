---
title: Uso de grupos con ubicación por proximidad para disminuir la latencia de los clústeres de Azure Kubernetes Service (AKS)
description: Aprenda a usar los grupos con ubicación por proximidad para disminuir la latencia de las cargas de trabajo de clústeres de AKS.
services: container-service
manager: gwallace
ms.topic: article
ms.date: 06/22/2020
ms.openlocfilehash: 1bcdfb4bb3c910feeac0521308e1e7d733fbd959
ms.sourcegitcommit: dabd9eb9925308d3c2404c3957e5c921408089da
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/11/2020
ms.locfileid: "86244079"
---
# <a name="reduce-latency-with-proximity-placement-groups-preview"></a>Reducción de la latencia con grupos con ubicación por proximidad (versión preliminar)

> [!Note]
> Cuando se usan los grupos con ubicación por proximidad con AKS, la coubicación solo se aplica a los nodos de agente. Se mejora la latencia nodo a nodo y la latencia pod a pod hospedado correspondiente. La coubicación no afecta la selección de ubicación del plano de control de un clúster.

Al implementar la aplicación en Azure, la propagación de instancias de máquina virtual (VM) entre regiones o zonas de disponibilidad crea una latencia de red, lo que puede afectar al rendimiento general de la aplicación. Un grupo con ubicación por proximidad es una agrupación lógica que se usa para asegurarse de que los recursos de proceso de Azure se encuentran físicamente cercanos entre sí. Algunas aplicaciones como juegos, simulaciones de ingeniería y operaciones bursátiles de alta frecuencia (HFT) requieren una latencia baja y tareas que se completen rápidamente. Para escenarios de informática de alto rendimiento (HPC), como estos, considere la posibilidad de usar [grupos con ubicación por proximidad](../virtual-machines/linux/co-location.md#proximity-placement-groups) para los grupos de nodos del clúster.

## <a name="limitations"></a>Limitaciones

* El grupo con ubicación por proximidad abarca una sola zona de disponibilidad.
* No hay compatibilidad actual con los clústeres de AKS que usan conjuntos de disponibilidad de máquinas virtuales.
* No se pueden modificar los grupos de nodos existentes para usar un grupo con ubicación por proximidad.

> [!IMPORTANT]
> Las características en versión preliminar de AKS están disponibles como opción de participación y autoservicio. Las versiones preliminares se proporcionan "tal cual" y "como están disponibles", y están excluidas de los Acuerdos de Nivel de Servicio y la garantía limitada. Las versiones preliminares de AKS reciben cobertura parcial del soporte al cliente en la medida de lo posible. Por lo tanto, estas características no están diseñadas para usarse en producción. Para más información, consulte los siguientes artículos de soporte:
>
> - [Directivas de soporte técnico para AKS](support-policies.md)
> - [Preguntas más frecuentes de soporte técnico de Azure](faq.md)

## <a name="before-you-begin"></a>Antes de empezar

Debe tener instalados los siguientes recursos:

- La extensión aks-preview 0.4.53

### <a name="set-up-the-preview-feature-for-proximity-placement-groups"></a>Configuración de la característica en vista previa (GB) para los grupos con ubicación por proximidad

> [!IMPORTANT]
> Cuando se usan los grupos con ubicación por proximidad con AKS, la coubicación solo se aplica a los nodos de agente. Se mejora la latencia nodo a nodo y la latencia pod a pod hospedado correspondiente. La coubicación no afecta la selección de ubicación del plano de control de un clúster.

```azurecli-interactive
# register the preview feature
az feature register --namespace "Microsoft.ContainerService" --name "ProximityPlacementGroupPreview"
```

Puede tardar varios minutos en registrarse. Use el siguiente comando para comprobar que la característica está registrada:

```azurecli-interactive
# Verify the feature is registered:
az feature list -o table --query "[?contains(name, 'Microsoft.ContainerService/ProximityPlacementGroupPreview')].{Name:name,State:properties.state}"
```

Durante la versión preliminar, necesita la extensión *aks-preview* de la CLI para usar los grupos con ubicación por proximidad. Use el comando [az extension add][az-extension-add] y, a continuación, busque las actualizaciones disponibles mediante el comando [az extension update][az-extension-update]:

```azurecli-interactive
# Install the aks-preview extension
az extension add --name aks-preview

# Update the extension to make sure you have the latest version installed
az extension update --name aks-preview
```
## <a name="node-pools-and-proximity-placement-groups"></a>Grupos de nodos y grupos con ubicación por proximidad

El primer recurso que se implementa con un grupo con ubicación por proximidad se adjunta a un centro de datos específico. Los recursos adicionales implementados con el mismo grupo con ubicación por proximidad se colocan en el mismo centro de datos. Una vez que todos los recursos que usan el grupo con ubicación por proximidad se han detenido (desasignado) o eliminado, ya no se adjuntan.

* Muchos grupos de nodos pueden asociarse a un solo grupo con ubicación por proximidad.
* Un grupo de nodos solo se puede asociar a un solo grupo con ubicación por proximidad.

## <a name="create-a-new-aks-cluster-with-a-proximity-placement-group"></a>Creación de un clúster de AKS con un grupo con ubicación por proximidad

El ejemplo siguiente usa el comando [az group create][az-group-create] para crear un grupo de recursos denominado *myResourceGroup* en la región *centralus*. Se crea un clúster de AKS denominado *myAKSCluster* mediante el comando [az aks create][az-aks-create]. 

Las redes aceleradas mejoran en gran medida el rendimiento de red de las máquinas virtuales. Idealmente, use grupos con ubicación por proximidad junto con redes aceleradas. De forma predeterminada, AKS usa redes aceleradas en las [instancias de máquinas virtuales admitidas](../virtual-network/create-vm-accelerated-networking-cli.md?toc=/azure/virtual-machines/linux/toc.json#limitations-and-constraints), que incluyen la mayoría de las máquinas virtuales de Azure con dos o más CPU virtuales.

Cree un clúster de AKS con un grupo con ubicación por proximidad:

```azurecli-interactive
# Create an Azure resource group
az group create --name myResourceGroup --location centralus
```
Ejecute el comando siguiente y almacene el id. que se devuelve:

```azurecli-interactive
# Create proximity placement group
az ppg create -n myPPG -g myResourceGroup -l centralus -t standard
```

El comando genera una salida, que incluye el valor *id* que necesita para los comandos siguientes de la CLI:

```output
{
  "availabilitySets": null,
  "colocationStatus": null,
  "id": "/subscriptions/yourSubscriptionID/resourceGroups/myResourceGroup/providers/Microsoft.Compute/proximityPlacementGroups/myPPG",
  "location": "centralus",
  "name": "myPPG",
  "proximityPlacementGroupType": "Standard",
  "resourceGroup": "myResourceGroup",
  "tags": {},
  "type": "Microsoft.Compute/proximityPlacementGroups",
  "virtualMachineScaleSets": null,
  "virtualMachines": null
}
```

Use el id. de recurso del grupo con ubicación por proximidad para el valor *myPPGResourceID* en el comando siguiente:

```azurecli-interactive
# Create an AKS cluster that uses a proximity placement group for the initial node pool
az aks create \
    --resource-group myResourceGroup \
    --name myAKSCluster \
    --ppg myPPGResourceID
```

## <a name="add-a-proximity-placement-group-to-an-existing-cluster"></a>Incorporación de un grupo con ubicación por proximidad a un clúster existente

Puede agregar un grupo con ubicación por proximidad a un clúster existente mediante la creación de un grupo de nodos nuevo. Si lo desea, puede migrar las cargas de trabajo existentes al grupo de nodos nuevo y, a continuación, eliminar el grupo de nodos original.

Use el mismo grupo con ubicación por proximidad que creó anteriormente y esto garantizará que los nodos de agente de ambos grupos de nodos del clúster de AKS estén físicamente ubicados en el mismo centro de datos.

Use el id. de recurso del grupo con ubicación por proximidad que creó anteriormente y agregue un grupo de nodos nuevo con el comando [`az aks nodepool add`][az-aks-nodepool-add]:

```azurecli-interactive
# Add a new node pool that uses a proximity placement group, use a --node-count = 1 for testing
az aks nodepool add \
    --resource-group myResourceGroup \
    --cluster-name myAKSCluster \
    --name mynodepool \
    --node-count 1 \
    --ppg myPPGResourceID
```

## <a name="clean-up"></a>Limpieza

Para eliminar el clúster, use el comando [`az group delete`][az-group-delete] para eliminar el grupo de recursos de AKS:

```azurecli-interactive
az group delete --name myResourceGroup --yes --no-wait
```

## <a name="next-steps"></a>Pasos siguientes

* Más información sobre los [grupos con ubicación por proximidad][proximity-placement-groups].

<!-- LINKS - Internal -->
[azure-ad-rbac]: azure-ad-rbac.md
[aks-tutorial-prepare-app]: ./tutorial-kubernetes-prepare-app.md
[azure-cli-install]: /cli/azure/install-azure-cli
[az-aks-get-upgrades]: /cli/azure/aks#az-aks-get-upgrades
[az-aks-upgrade]: /cli/azure/aks#az-aks-upgrade
[az-aks-show]: /cli/azure/aks#az-aks-show
[nodepool-upgrade]: use-multiple-node-pools.md#upgrade-a-node-pool
[az-extension-add]: /cli/azure/extension#az-extension-add
[az-extension-update]: /cli/azure/extension#az-extension-update
[proximity-placement-groups]: ../virtual-machines/linux/co-location.md#proximity-placement-groups
[az-aks-create]: /cli/azure/aks#az-aks-create
[system-pool]: ./use-system-pools.md
[az-aks-nodepool-add]: /cli/azure/aks/nodepool?view=azure-cli-latest#az-aks-nodepool-add
[az-aks-create]: /cli/azure/aks#az-aks-create
[az-group-create]: /cli/azure/group#az-group-create
[az-group-delete]: /cli/azure/group#az-group-delete

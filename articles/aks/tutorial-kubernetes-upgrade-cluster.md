---
title: 'Tutorial de Kubernetes en Azure: Actualización de un clúster'
description: En este tutorial de Azure Kubernetes Service (AKS) aprenderá a actualizar un clúster de AKS existente a la versión de Kubernetes más reciente disponible.
services: container-service
ms.topic: tutorial
ms.date: 02/25/2020
ms.custom: mvc
ms.openlocfilehash: 22aad0e601c600e582cbea0cea82dd67a20a2c06
ms.sourcegitcommit: d6e4eebf663df8adf8efe07deabdc3586616d1e4
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/15/2020
ms.locfileid: "81392674"
---
# <a name="tutorial-upgrade-kubernetes-in-azure-kubernetes-service-aks"></a>Tutorial: Actualización de Kubernetes en Azure Kubernetes Service (AKS)

Como parte del ciclo de vida de la aplicación y del clúster, es posible que desee actualizar su versión de Kubernetes a la más reciente y usar las nuevas características. Un clúster de Azure Kubernetes Service (AKS) se puede actualizar mediante la CLI de Azure.

En este tutorial, la séptima parte de siete, se actualiza un clúster de Kubernetes. Aprenderá a:

> [!div class="checklist"]
> * Identificación de las versiones de Kubernetes actuales y disponibles
> * Actualización de los nodos de Kubernetes
> * Validación de una actualización correcta

## <a name="before-you-begin"></a>Antes de empezar

En los tutoriales anteriores se empaquetó una aplicación en una imagen de contenedor. Esta imagen se cargó en Azure Container Registry y el usuario creó un clúster de AKS. Luego la aplicación se implementó en el clúster de AKS. Si no ha realizado estos pasos, pero desea continuar, comience con el [Tutorial 1: Creación de imágenes de contenedor][aks-tutorial-prepare-app].

Para realizar este tutorial es necesario disponer de la versión 2.0.53, o superior, de la CLI de Azure. Ejecute `az --version` para encontrar la versión. Si necesita instalarla o actualizarla, vea [Instalación de la CLI de Azure][azure-cli-install].

## <a name="get-available-cluster-versions"></a>Obtención de las versiones disponibles del clúster

Antes de actualizar un clúster, use el comando [az aks get-upgrades][] para comprobar qué versiones de Kubernetes están disponibles para la actualización:

```azurecli
az aks get-upgrades --resource-group myResourceGroup --name myAKSCluster --output table
```

En el siguiente ejemplo, la versión actual es la *1.14.8* y las versiones disponibles se muestran en la columna *Upgrades* (Actualizaciones).

```
Name     ResourceGroup    MasterVersion    NodePoolVersion    Upgrades
-------  ---------------  ---------------  -----------------  --------------
default  myResourceGroup  1.14.8           1.14.8             1.15.5, 1.15.7
```

## <a name="upgrade-a-cluster"></a>Actualizar un clúster

Para minimizar las interrupciones en las aplicaciones en ejecución, los nodos de AKS se acordonan y purgan minuciosamente. En este proceso, se realizan los pasos siguientes:

1. El programador de Kubernetes impide que se programen pods adicionales en un nodo que se va a actualizar.
1. Los pods en ejecución en el nodo se programan en otros nodos del clúster.
1. Se crea un nodo que ejecuta los componentes más recientes de Kubernetes.
1. Cuando el nuevo nodo está listo y se ha unido al clúster, el programador de Kubernetes comienza a ejecutar pods en él.
1. El nodo antiguo se elimina y el siguiente nodo del clúster comienza el proceso de acordonamiento y purga.

Use el comando [az aks upgrade][] para actualizar el clúster de AKS. En el ejemplo siguiente se actualiza el clúster a la versión *1.14.6* de Kubernetes.

> [!NOTE]
> No se pueden actualizar varias versiones secundarias al mismo tiempo. Por ejemplo, puede realizar la actualización de la versión *1.14.x* a la *1.15.x*, pero no de la *1.14.x* a la *1.16.x* directamente. Para actualizar de la versión *1.14.x* a la *1.16.x*, primero actualice de la *1.14.x* a la *1.15.x* y, después, realice otra actualización de la versión *1.15.x* a la *1.16.x*.

```azurecli
az aks upgrade --resource-group myResourceGroup --name myAKSCluster --kubernetes-version 1.15.5
```

La siguiente salida de ejemplo condensada muestra que *kubernetesVersion* es ahora *1.15.5*:

```json
{
  "agentPoolProfiles": [
    {
      "count": 3,
      "maxPods": 110,
      "name": "nodepool1",
      "osType": "Linux",
      "storageProfile": "ManagedDisks",
      "vmSize": "Standard_DS1_v2",
    }
  ],
  "dnsPrefix": "myAKSClust-myResourceGroup-19da35",
  "enableRbac": false,
  "fqdn": "myaksclust-myresourcegroup-19da35-bd54a4be.hcp.eastus.azmk8s.io",
  "id": "/subscriptions/<Subscription ID>/resourcegroups/myResourceGroup/providers/Microsoft.ContainerService/managedClusters/myAKSCluster",
  "kubernetesVersion": "1.15.5",
  "location": "eastus",
  "name": "myAKSCluster",
  "type": "Microsoft.ContainerService/ManagedClusters"
}
```

## <a name="validate-an-upgrade"></a>Validación de una actualización

Confirme que la actualización se realizó correctamente con el comando [az aks show][], tal como se muestra a continuación:

```azurecli
az aks show --resource-group myResourceGroup --name myAKSCluster --output table
```

La salida del ejemplo siguiente muestra que el clúster de AKS ejecuta *KubernetesVersion 1.15.5*:

```
Name          Location    ResourceGroup    KubernetesVersion    ProvisioningState    Fqdn
------------  ----------  ---------------  -------------------  -------------------  ----------------------------------------------------------------
myAKSCluster  eastus      myResourceGroup  1.15.5               Succeeded            myaksclust-myresourcegroup-19da35-bd54a4be.hcp.eastus.azmk8s.io
```

## <a name="delete-the-cluster"></a>Eliminación del clúster

Como este tutorial es la última parte de la serie, puede que quiera eliminar el clúster de AKS. Mientras los nodos de Kubernetes se ejecutan en Azure Virtual Machines (VM), seguirán acumulando cargos, aunque no se use el clúster. Use el comando [az group delete][az-group-delete] para quitar el grupo de recursos, el servicio de contenedor y todos los recursos relacionados.

```azurecli-interactive
az group delete --name myResourceGroup --yes --no-wait
```

> [!NOTE]
> Cuando elimina el clúster, la entidad de servicio Azure Active Directory que utiliza el clúster de AKS no se quita. Para conocer los pasos que hay que realizar para quitar la entidad de servicio, consulte [Consideraciones principales y eliminación de AKS][sp-delete]. Si usó una identidad administrada, esta está administrada por la plataforma y no requiere que aprovisione o rote los secretos.

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, actualizó Kubernetes en un clúster de AKS. Ha aprendido a:

> [!div class="checklist"]
> * Identificación de las versiones de Kubernetes actuales y disponibles
> * Actualización de los nodos de Kubernetes
> * Validación de una actualización correcta

Siga este vínculo para conocer más acerca de AKS.

> [!div class="nextstepaction"]
> [Introducción a AKS][aks-intro]

<!-- LINKS - external -->
[kubernetes-drain]: https://kubernetes.io/docs/tasks/administer-cluster/safely-drain-node/

<!-- LINKS - internal -->
[aks-intro]: ./intro-kubernetes.md
[aks-tutorial-prepare-app]: ./tutorial-kubernetes-prepare-app.md
[az aks show]: /cli/azure/aks#az-aks-show
[az aks get-upgrades]: /cli/azure/aks#az-aks-get-upgrades
[az aks upgrade]: /cli/azure/aks#az-aks-upgrade
[azure-cli-install]: /cli/azure/install-azure-cli
[az-group-delete]: /cli/azure/group#az-group-delete
[sp-delete]: kubernetes-service-principal.md#additional-considerations

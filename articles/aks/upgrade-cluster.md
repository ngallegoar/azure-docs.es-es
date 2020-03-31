---
title: Actualización de un clúster de Azure Kubernetes Service (AKS)
description: Aprenda a actualizar clústeres de Azure Kubernetes Service (AKS)
services: container-service
ms.topic: article
ms.date: 05/31/2019
ms.openlocfilehash: 4520297e83f96f95b10ecafd5af52a913dc5f450
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "77621973"
---
# <a name="upgrade-an-azure-kubernetes-service-aks-cluster"></a>Actualización de un clúster de Azure Kubernetes Service (AKS)

Como parte del ciclo de vida de un clúster de AKS, a menudo es preciso actualizar Kubernetes a su versión más reciente. Es importante que aplique las versiones de seguridad de Kubernetes más recientes o que realice la actualización necesaria para disfrutar de las últimas características. En este artículo se muestra cómo actualizar los componentes principales o un único grupo de nodos predeterminado en un clúster de AKS.

Para los clústeres de AKS que usan varios grupos de nodos o nodos de Windows Server (ambos actualmente en versión preliminar en AKS), consulte [Actualización de un grupo de nodos en AKS][nodepool-upgrade].

## <a name="before-you-begin"></a>Antes de empezar

Para este artículo es preciso usar la versión 2.0.65 de la CLI de Azure, o cualquier versión posterior. Ejecute `az --version` para encontrar la versión. Si necesita instalarla o actualizarla, vea [Instalación de la CLI de Azure][azure-cli-install].

> [!WARNING]
> Una actualización del clúster de AKS desencadena un acordonamiento y purga de los nodos. Si tiene una cuota de proceso baja disponible, se puede producir un error en la actualización. Consulte el [aumento de cuotas](https://docs.microsoft.com/azure/azure-portal/supportability/resource-manager-core-quotas-request) para más información.
> Si está ejecutando su propia implementación del escalador automático del clúster, deshabilítela (puede escalarla a cero réplicas) durante la actualización, ya que existe la posibilidad de que interfiera con el proceso de actualización. El escalador administrado lo controla automáticamente. 

## <a name="check-for-available-aks-cluster-upgrades"></a>Compruebe las actualizaciones disponibles del clúster de AKS

Para comprobar qué versiones de Kubernetes están disponibles para su clúster, use el comando [az aks get-upgrades][az-aks-get-upgrades]. En el ejemplo siguiente se comprueba si hay actualizaciones disponibles para el clúster denominado *myAKSCluster* en el grupo de recursos denominado *myResourceGroup*:

```azurecli-interactive
az aks get-upgrades --resource-group myResourceGroup --name myAKSCluster --output table
```

> [!NOTE]
> Al actualizar un clúster de AKS, no pueden omitirse las versiones secundarias de Kubernetes. Por ejemplo, se permiten las actualizaciones entre *1.12.x* -> *1.13.x* o *1.13.x* -> *1.14.x*, pero no entre *1.12.x* -> *1.14.x*.
>
> Para actualizar de *1.12.x* -> *1.14.x*, la primera actualización sería de *1.12.x* -> *1.13.x* y, después, de *1.13.x* -> *1.14.x*.

La siguiente salida de ejemplo muestra que el clúster puede actualizarse a las versiones *1.13.9* y *1.13.10*:

```console
Name     ResourceGroup     MasterVersion    NodePoolVersion    Upgrades
-------  ----------------  ---------------  -----------------  ---------------
default  myResourceGroup   1.12.8           1.12.8             1.13.9, 1.13.10
```
Si no hay ninguna actualización disponible, obtendrá:
```console
ERROR: Table output unavailable. Use the --query option to specify an appropriate query. Use --debug for more info.
```

## <a name="upgrade-an-aks-cluster"></a>Actualización de un clúster de AKS

Con una lista de las versiones disponibles para el clúster de AKS, use el comando [az aks upgrade][az-aks-upgrade] para realizar la actualización. En el proceso de actualización, AKS agrega un nuevo nodo al clúster que ejecuta la versión de Kubernetes especificada y después [acordona y purga][kubernetes-drain] uno de los nodos antiguos para minimizar las interrupciones de las aplicaciones en ejecución. Cuando se confirma que el nuevo nodo ejecuta pods de aplicación, el anterior se elimina. Este proceso se repite hasta que se hayan actualizado todos los nodos del clúster.

En el ejemplo siguiente se actualiza un clúster a la versión *1.13.10*:

```azurecli-interactive
az aks upgrade --resource-group myResourceGroup --name myAKSCluster --kubernetes-version 1.13.10
```

Dicha actualización tarda varios minutos. El tiempo exacto dependerá del número de nodos que tenga. 

> [!NOTE]
> Hay un tiempo total permitido para que se complete la actualización de un clúster. Este tiempo se calcula tomando el producto de `10 minutes * total number of nodes in the cluster`. Por ejemplo, en un clúster de 20 nodos, las operaciones de actualización deben realizarse correctamente en 200 minutos o AKS producirá un error en la operación para evitar un estado de clúster irrecuperable. Para la recuperación en caso de error de actualización, vuelva a intentar la operación una vez alcanzado el tiempo de espera.

Para confirmar que la actualización se ha realizado correctamente, use el comando [az aks show][az-aks-show]:

```azurecli-interactive
az aks show --resource-group myResourceGroup --name myAKSCluster --output table
```

En la salida de ejemplo siguiente se muestra que el clúster ahora ejecuta la versión *1.13.10*:

```json
Name          Location    ResourceGroup    KubernetesVersion    ProvisioningState    Fqdn
------------  ----------  ---------------  -------------------  -------------------  ---------------------------------------------------------------
myAKSCluster  eastus      myResourceGroup  1.13.10               Succeeded            myaksclust-myresourcegroup-19da35-90efab95.hcp.eastus.azmk8s.io
```

## <a name="next-steps"></a>Pasos siguientes

En este artículo se ha mostrado cómo actualizar un clúster de AKS existente. Para más información acerca de la implementación y administración de clústeres de AKS, consulte el conjunto de tutoriales.

> [!div class="nextstepaction"]
> [Tutoriales de AKS][aks-tutorial-prepare-app]

<!-- LINKS - external -->
[kubernetes-drain]: https://kubernetes.io/docs/tasks/administer-cluster/safely-drain-node/

<!-- LINKS - internal -->
[aks-tutorial-prepare-app]: ./tutorial-kubernetes-prepare-app.md
[azure-cli-install]: /cli/azure/install-azure-cli
[az-aks-get-upgrades]: /cli/azure/aks#az-aks-get-upgrades
[az-aks-upgrade]: /cli/azure/aks#az-aks-upgrade
[az-aks-show]: /cli/azure/aks#az-aks-show
[nodepool-upgrade]: use-multiple-node-pools.md#upgrade-a-node-pool

---
title: Uso de las zonas de disponibilidad en Azure Kubernetes Service (AKS)
description: Aprenda a crear un clúster que distribuya nodos a través de las zonas de disponibilidad en Azure Kubernetes Service (AKS)
services: container-service
ms.custom: fasttrack-edit
ms.topic: article
ms.date: 06/24/2019
ms.openlocfilehash: 5693d9e90de9ba68e7b76e0f2bd5b75141dbda71
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "77596817"
---
# <a name="create-an-azure-kubernetes-service-aks-cluster-that-uses-availability-zones"></a>Creación de un clúster de Azure Kubernetes Service (AKS) que use zonas de disponibilidad

Un clúster de Azure Kubernetes Service (AKS) distribuye recursos como los nodos y el almacenamiento en secciones lógicas de la infraestructura de procesos subyacente de Azure. Este modelo de implementación garantiza que los nodos se ejecuten en dominios de actualización y error separados en un solo centro de datos de Azure. Los clústeres de AKS implementados con este comportamiento predeterminado proporcionan un alto nivel de disponibilidad para protegerle contra un error de hardware o un evento de mantenimiento planeado.

Para proporcionar un mayor nivel de disponibilidad a sus aplicaciones, los clústeres de AKS se pueden distribuir en zonas de disponibilidad. Estas zonas son centros de datos físicamente separados dentro de una región determinada. Cuando los componentes del clúster se distribuyen entre varias zonas, su clúster de AKS puede tolerar un error en una de esas zonas. Sus aplicaciones y operaciones de administración continuarán disponibles incluso si todo un centro de datos tiene problemas.

En este artículo le mostraremos cómo crear un clúster de AKS y distribuir los componentes del nodo en las zonas de disponibilidad.

## <a name="before-you-begin"></a>Antes de empezar

Es preciso que esté instalada y configurada la versión 2.0.76 de la CLI de Azure, o cualquier otra posterior. Ejecute  `az --version` para encontrar la versión. Si necesita instalarla o actualizarla, consulte  [Install Azure CLI][install-azure-cli] (Instalación de la CLI de Azure).

## <a name="limitations-and-region-availability"></a>Limitaciones y disponibilidad de región

Los clústeres de AKS se pueden crear actualmente mediante zonas de disponibilidad en las siguientes regiones:

* Centro de EE. UU.
* Este de EE. UU. 2
* Este de EE. UU.
* Centro de Francia
* Japón Oriental
* Norte de Europa
* Sudeste de Asia
* Sur de Reino Unido 2
* Oeste de Europa
* Oeste de EE. UU. 2

Las siguientes limitaciones se aplican cuando crea un clúster de AKS mediante zonas de disponibilidad:

* Solo puede habilitar las zonas de disponibilidad cuando se crea el clúster.
* La configuración de la zona de disponibilidad no se puede actualizar después de crear el clúster. Tampoco puede actualizar un clúster de zona sin disponibilidad para usar zonas de disponibilidad.
* No puede deshabilitar las zonas de disponibilidad de un clúster de AKS una vez que se haya creado.
* El tamaño del nodo (SKU de VM) seleccionado debe estar disponible en todas las zonas de disponibilidad.
* Los clústeres con zonas de disponibilidad habilitadas requieren el uso de equilibradores de carga estándar de Azure para la distribución entre zonas.
* Debe usar la versión 1.13.5 o superior de Kubernetes para implementar los equilibradores de carga estándar.

Los clústeres de AKS que usan zonas de disponibilidad deben usar la SKU *Estándar* de Azure Load Balancer, que es el valor predeterminado para el tipo de equilibrador de carga. Este tipo de equilibrador de carga solo se puede definir en el momento de la creación del clúster. Para obtener más información y las limitaciones del equilibrador de carga estándar, consulte las [limitaciones de la SKU estándar del equilibrador de carga de Azure][standard-lb-limitations].

### <a name="azure-disks-limitations"></a>Limitaciones de los discos de Azure

Los volúmenes que usan discos administrados de Azure no son recursos de zona actualmente. Los pods reprogramados en una zona diferente a su zona original no pueden volver a conectarse a sus discos anteriores. Es recomendable ejecutar cargas de trabajo sin estado que no requieran un almacenamiento persistente, ya que pueden surgir problemas en las zonas.

Si debe ejecutar cargas de trabajo con estado, use marcas y tolerancias en sus especificaciones de pod para indicarle al programador de Kubernetes que cree pods en la misma zona que sus discos. Asimismo, puede usar el almacenamiento basado en la red, como Azure Files, que se puede conectar a los pods según se programan entre zonas.

## <a name="overview-of-availability-zones-for-aks-clusters"></a>Introducción a las zonas de disponibilidad para los clústeres de AKS

Las zonas de disponibilidad constituyen una oferta de alta disponibilidad que protege las aplicaciones y los datos de los errores en el centro de datos. Las zonas son ubicaciones físicas exclusivas dentro de una región de Azure. Cada zona de disponibilidad consta de uno o varios centros de datos equipados con alimentación, refrigeración y redes independientes. Para garantizar la resistencia, hay tres zonas independientes como mínimo en todas las regiones habilitadas. La separación física de las zonas de disponibilidad dentro de una región protege las aplicaciones y los datos frente a los errores del centro de datos. Los servicios con redundancia de zona replican las aplicaciones y los datos entre zonas de disponibilidad para protegerlos frente a puntos de error únicos.

Para más información, consulte [¿Qué son las zonas de disponibilidad en Azure?][az-overview]

Los clústeres de AKS que se implementan mediante zonas de disponibilidad que pueden distribuir nodos en varias zonas dentro de una sola región. Por ejemplo, un clúster en la región  *Este de EE. UU. 2* puede crear nodos en las tres zonas de disponibilidad del *Este de EE. UU. 2*. Esta distribución de los recursos del clúster de AKS mejora la disponibilidad del clúster, ya que son resistentes a los errores de una zona específica.

![Distribución de nodos de AKS en zonas de disponibilidad](media/availability-zones/aks-availability-zones.png)

Cuando se produce una interrupción en una zona, los nodos se pueden reequilibrar manualmente o con el escalador automático del clúster. Así pues, si una sola zona deja de estar disponible, sus aplicaciones continúan ejecutándose.

## <a name="create-an-aks-cluster-across-availability-zones"></a>Crear un clúster de AKS en zonas de disponibilidad

Cuando crea un clúster con el comando [az aks create][az-aks-create], el parámetro `--zones` define en qué zonas se implementan los nodos de agente. Cuando se define este parámetro `--zones` al crear el clúster, los componentes del plano de control de AKS del clúster también se extienden a través de las zonas, con la máxima configuración disponible.

Si no define ninguna zona para el grupo de agentes predeterminado cuando crea un clúster de AKS, los componentes del plano de control AKS del clúster no usarán las zonas de disponibilidad. Puede agregar grupos de nodos adicionales mediante el comando [az aks nodepool add][az-aks-nodepool-add] y especificar `--zones` para esos nuevos nodos; sin embargo, los componentes del plano de control permanecen sin conocimiento de la zona de disponibilidad. No puede cambiar el reconocimiento de zona para un grupo de nodos o los componentes del plano de control de AKS una vez que se implementan.

En el ejemplo siguiente se crea un clúster de AKS denominado *myAKSCluster* en el grupo de recursos denominado *myResourceGroup*. Se crean un total de *3* nodos: un agente en la zona *1*, uno en la *2* y otro en la *3*. Los componentes del plano de control de AKS también se distribuyen a través de zonas en la configuración más alta disponible, ya que se definen como parte del proceso de creación del clúster.

```azurecli-interactive
az group create --name myResourceGroup --location eastus2

az aks create \
    --resource-group myResourceGroup \
    --name myAKSCluster \
    --generate-ssh-keys \
    --vm-set-type VirtualMachineScaleSets \
    --load-balancer-sku standard \
    --node-count 3 \
    --zones 1 2 3
```

El clúster de AKS tarda unos minutos en crearse.

## <a name="verify-node-distribution-across-zones"></a>Comprobar la distribución de nodos entre zonas

Cuando el clúster esté listo, enumere los nodos de agente en el conjunto de escalado para ver en qué zona de disponibilidad se implementarán.

Primero, obtenga las credenciales del clúster de AKS mediante el comando [az aks get-credentials][az-aks-get-credentials]:

```azurecli-interactive
az aks get-credentials --resource-group myResourceGroup --name myAKSCluster
```

A continuación, use el comando [kubectl describe][kubectl-describe] para enumerar los nodos del clúster. Filtre el valor *failure-domain.beta.kubernetes.io/zone* tal como se muestra en el siguiente ejemplo:

```console
kubectl describe nodes | grep -e "Name:" -e "failure-domain.beta.kubernetes.io/zone"
```

El siguiente resultado de ejemplo muestra los tres nodos distribuidos en la región especificada y las zonas de disponibilidad, como *eastus2-1* para la primera zona de disponibilidad y *eastus2-2* para la segunda zona de disponibilidad:

```console
Name:       aks-nodepool1-28993262-vmss000000
            failure-domain.beta.kubernetes.io/zone=eastus2-1
Name:       aks-nodepool1-28993262-vmss000001
            failure-domain.beta.kubernetes.io/zone=eastus2-2
Name:       aks-nodepool1-28993262-vmss000002
            failure-domain.beta.kubernetes.io/zone=eastus2-3
```

A medida que agrega nodos adicionales a un grupo de agentes, la plataforma Azure distribuye automáticamente las VM subyacentes en las zonas de disponibilidad especificadas.

Tenga en cuenta que en las versiones más recientes de Kubernetes (1.17.0 y versiones posteriores), AKS usa la etiqueta `topology.kubernetes.io/zone` más reciente además de la `failure-domain.beta.kubernetes.io/zone` en desuso.

## <a name="verify-pod-distribution-across-zones"></a>Comprobar la distribución de pods entre zonas

Como se documenta en [Etiquetas, anotaciones y taints ampliamente conocidas][kubectl-well_known_labels], Kubernetes usa la etiqueta `failure-domain.beta.kubernetes.io/zone` para distribuir automáticamente los pods en un controlador o servicio de replicación en las diferentes zonas disponibles. Para probarlo, puede escalar verticalmente el clúster de 3 a 5 nodos para comprobar la propagación correcta del pod:

```azurecli-interactive
az aks scale \
    --resource-group myResourceGroup \
    --name myAKSCluster \
    --node-count 5
```

Cuando la operación de escalado se complete después de unos minutos, el comando `kubectl describe nodes | grep -e "Name:" -e "failure-domain.beta.kubernetes.io/zone"` debe proporcionar una salida similar a la de este ejemplo:

```console
Name:       aks-nodepool1-28993262-vmss000000
            failure-domain.beta.kubernetes.io/zone=eastus2-1
Name:       aks-nodepool1-28993262-vmss000001
            failure-domain.beta.kubernetes.io/zone=eastus2-2
Name:       aks-nodepool1-28993262-vmss000002
            failure-domain.beta.kubernetes.io/zone=eastus2-3
Name:       aks-nodepool1-28993262-vmss000003
            failure-domain.beta.kubernetes.io/zone=eastus2-1
Name:       aks-nodepool1-28993262-vmss000004
            failure-domain.beta.kubernetes.io/zone=eastus2-2
```

Como puede ver, ahora tenemos dos nodos adicionales en las zonas 1 y 2. Puede implementar una aplicación que consta de tres réplicas. Usaremos NGINX como ejemplo:

```console
kubectl run nginx --image=nginx --replicas=3
```

Si comprueba los nodos en los que se ejecutan los pods, verá que estos se ejecutan en los pods correspondientes a tres zonas de disponibilidad diferentes. Por ejemplo, con el comando `kubectl describe pod | grep -e "^Name:" -e "^Node:"` obtendría una salida similar a la siguiente:

```console
Name:         nginx-6db489d4b7-ktdwg
Node:         aks-nodepool1-28993262-vmss000000/10.240.0.4
Name:         nginx-6db489d4b7-v7zvj
Node:         aks-nodepool1-28993262-vmss000002/10.240.0.6
Name:         nginx-6db489d4b7-xz6wj
Node:         aks-nodepool1-28993262-vmss000004/10.240.0.8
```

Como puede ver en la salida anterior, el primer pod se ejecuta en el nodo 0, que se encuentra en la zona de disponibilidad `eastus2-1`. El segundo pod se ejecuta en el nodo 2, que corresponde a `eastus2-3`, y el tercero, en el nodo 4, en `eastus2-2`. Sin necesidad de configuración adicional, Kubernetes extiende los pods correctamente en las tres zonas de disponibilidad.

## <a name="next-steps"></a>Pasos siguientes

En este artículo se detalla cómo crear un clúster de AKS que usa zonas de disponibilidad. Para obtener más información sobre los clústeres de alta disponibilidad, consulte [Best practices for business continuity and disaster recovery in AKS][best-practices-bc-dr] (Procedimientos recomendados para la continuidad empresarial y recuperación ante desastres en AKS).

<!-- LINKS - internal -->
[install-azure-cli]: /cli/azure/install-azure-cli
[az-feature-register]: /cli/azure/feature#az-feature-register
[az-feature-list]: /cli/azure/feature#az-feature-list
[az-provider-register]: /cli/azure/provider#az-provider-register
[az-aks-create]: /cli/azure/aks#az-aks-create
[az-overview]: ../availability-zones/az-overview.md
[best-practices-bc-dr]: operator-best-practices-multi-region.md
[aks-support-policies]: support-policies.md
[aks-faq]: faq.md
[standard-lb-limitations]: load-balancer-standard.md#limitations
[az-extension-add]: /cli/azure/extension#az-extension-add
[az-extension-update]: /cli/azure/extension#az-extension-update
[az-aks-nodepool-add]: /cli/azure/ext/aks-preview/aks/nodepool#ext-aks-preview-az-aks-nodepool-add
[az-aks-get-credentials]: /cli/azure/aks?view=azure-cli-latest#az-aks-get-credentials

<!-- LINKS - external -->
[kubectl-describe]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#describe
[kubectl-well_known_labels]: https://kubernetes.io/docs/reference/kubernetes-api/labels-annotations-taints/

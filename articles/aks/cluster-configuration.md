---
title: Configuración de clúster en Azure Kubernetes Service (AKS)
description: Aprenda a configurar un clúster en Azure Kubernetes Service (AKS).
services: container-service
ms.topic: conceptual
ms.date: 03/12/2020
ms.openlocfilehash: 94f84beee2d7a76e48ac1470a0ce0b387929cc08
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "79474177"
---
# <a name="configure-an-aks-cluster"></a>Configuración de un clúster de AKS

Como parte de la creación de un clúster de AKS, puede que tenga que personalizar la configuración del clúster para que se adapte a sus necesidades. En este artículo se presentan algunas opciones para personalizar el clúster de AKS.

## <a name="os-configuration-preview"></a>Configuración del sistema operativo (versión preliminar)

AKS admite ya Ubuntu 18.04 como sistema operativo de nodo en versión preliminar. Durante el período de versión preliminar, se encuentran disponibles tanto Ubuntu 16.04 como Ubuntu 18.04.

Debe tener instalados los siguientes recursos:

- La CLI de Azure, versión 2.2.0 o cualquier versión posterior
- La extensión aks-preview 0.4.35

Para instalar la extensión aks-preview 0.4.35 o una posterior, use los siguientes comandos de la CLI de Azure:

```azurecli
az extension add --name aks-preview
az extension list
```

Registre la característica `UseCustomizedUbuntuPreview`:

```azurecli
az feature register --name UseCustomizedUbuntuPreview --namespace Microsoft.ContainerService
```

Pueden pasar unos minutos hasta que el estado aparezca como **Registrado**. Puede comprobar el estado del registro con el comando [az feature list](https://docs.microsoft.com/cli/azure/feature?view=azure-cli-latest#az-feature-list):

```azurecli
az feature list -o table --query "[?contains(name, 'Microsoft.ContainerService/UseCustomizedUbuntuPreview')].{Name:name,State:properties.state}"
```

Cuando el estado se muestre como Registrado, actualice el registro del proveedor de recursos `Microsoft.ContainerService` mediante el comando [az provider register](https://docs.microsoft.com/cli/azure/provider?view=azure-cli-latest#az-provider-register):

```azurecli
az provider register --namespace Microsoft.ContainerService
```

Configure el clúster para que use Ubuntu 18.04 cuando se cree el clúster. Use la marca `--aks-custom-headers` para establecer Ubuntu 18.04 como sistema operativo predeterminado.

```azure-cli
az aks create --name myAKSCluster --resource-group myResourceGroup --aks-custom-headers CustomizedUbuntu=aks-ubuntu-1804
```

Si desea crear un clúster de Ubuntu 16.04 normal, puede hacerlo omitiendo la etiqueta `--aks-custom-headers` personalizada.

## <a name="custom-resource-group-name"></a>Nombre del grupo de recursos personalizado

Al implementar un clúster de Azure Kubernetes Service en Azure, se crea un segundo grupo de recursos para los nodos de trabajo. De forma predeterminada, AKS asignará un nombre al grupo de recursos del nodo `MC_resourcegroupname_clustername_location`, pero también puede proporcionar su propio nombre.

Para especificar su propio nombre de grupo de recursos, instale la versión de la extensión de la CLI de Azure aks-preview 0.3.2 o una posterior. Con la CLI de Azure, use el parámetro `--node-resource-group` del comando `az aks create` para especificar un nombre personalizado para el grupo de recursos. Si usa una plantilla de Azure Resource Manager para implementar un clúster de AKS, puede definir el nombre del grupo de recursos mediante la propiedad `nodeResourceGroup`.

```azurecli
az aks create --name myAKSCluster --resource-group myResourceGroup --node-resource-group myNodeResourceGroup
```

El proveedor de recursos de Azure crea automáticamente el grupo de recursos secundario en su propia suscripción. Tenga en cuenta que solo puede especificar el nombre del grupo de recursos personalizado cuando se crea el clúster. 

Mientras trabaje con el grupo de recursos del nodo, tenga en cuenta que no puede realizar lo siguiente:

- Especificar un grupo de recursos existente para el grupo de recursos del nodo.
- Especificar otra suscripción para el grupo de recursos del nodo.
- Cambiar el nombre del grupo de recursos del nodo una vez se haya creado el clúster.
- Especificar los nombres de los recursos administrados del grupo de recursos del nodo.
- Modificar o eliminar las etiquetas creadas en Azure de los recursos administrados del grupo de recursos del nodo.

## <a name="next-steps"></a>Pasos siguientes

- Aprenda a usar `Kured` para [aplicar actualizaciones de seguridad y kernel a los nodos de Linux](node-updates-kured.md) en el clúster.
- Consulte [Actualización de un clúster de Azure Kubernetes Service (AKS)](upgrade-cluster.md) para más información sobre cómo actualizar el clúster a la versión más reciente de Kubernetes.
- Consulte la lista de [Preguntas más frecuentes sobre AKS](faq.md) para encontrar respuestas a algunas preguntas comunes sobre AKS.
---
title: Restablecer las credenciales de un clúster de Azure Kubernetes Service (AKS)
description: Aprenda a actualizar o restablecer las credenciales de la entidad de servicio de la aplicación AAD de un clúster en Azure Kubernetes Service (AKS)
services: container-service
ms.topic: article
ms.date: 03/11/2019
ms.openlocfilehash: b7d652be3733cb130a3973909de59489047efe0a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "79475551"
---
# <a name="update-or-rotate-the-credentials-for-azure-kubernetes-service-aks"></a>Actualización o modificación de las credenciales de un clúster de Azure Kubernetes Service (AKS)

De forma predeterminada, los clústeres de AKS se crean con una entidad de servicio que tiene un período de expiración de un año. A medida que se aproxima a la fecha de expiración, puede restablecer las credenciales para ampliar la entidad de servicio durante un período de tiempo adicional. También puede actualizar o rotar las credenciales como parte de una directiva de seguridad definida. En este artículo se describe cómo actualizar estas credenciales para un clúster de AKS.

Es posible que también haya [integrado el clúster de AKS con Azure Active Directory][aad-integration] y que lo utilice como proveedor de autenticación del clúster. En ese caso, tendrá dos identidades más creadas para el clúster: la aplicación de servidor de AAD y la aplicación cliente de AAD, y también puede restablecer esas credenciales. 

## <a name="before-you-begin"></a>Antes de empezar

Es preciso que esté instalada y configurada la versión 2.0.65 de la CLI de Azure, o cualquier otra posterior. Ejecute  `az --version` para encontrar la versión. Si necesita instalarla o actualizarla, consulte  [Install Azure CLI][install-azure-cli] (Instalación de la CLI de Azure).

## <a name="update-or-create-a-new-service-principal-for-your-aks-cluster"></a>Actualización o creación de una nueva entidad de servicio para el clúster de AKS

Cuando quiera actualizar las credenciales de un clúster de AKS, puede hacer lo siguiente:

* actualizar las credenciales de la entidad de servicio existente que usa el clúster, o
* crear a una entidad de servicio y actualizar el clúster para usar estas nuevas credenciales.

### <a name="reset-existing-service-principal-credential"></a>Restablecimiento de las credenciales de una entidad de servicio existente

Para actualizar las credenciales de la entidad de servicio existente, obtenga el identificador de entidad de servicio del clúster mediante el comando [az aks show][az-aks-show]. En el ejemplo siguiente se obtiene el id. del clúster denominado *myAKSCluster* en el grupo de recursos *myResourceGroup*. El identificador de la entidad de servicio se establece como una variable denominada *SP_ID* para usarlo en un comando adicional.

```azurecli-interactive
SP_ID=$(az aks show --resource-group myResourceGroup --name myAKSCluster \
    --query servicePrincipalProfile.clientId -o tsv)
```

Con un conjunto de variables que contiene el id. de entidad de servicio, debe restablecer las credenciales mediante [az ad sp credential reset][az-ad-sp-credential-reset]. En el ejemplo siguiente se permite a la plataforma Azure generar un nuevo secreto seguro para la entidad de servicio. Este nuevo secreto seguro también se almacena como una variable.

```azurecli-interactive
SP_SECRET=$(az ad sp credential reset --name $SP_ID --query password -o tsv)
```

Ahora, vaya a la sección [Actualización del clúster de AKS con nuevas credenciales de la entidad de servicio](#update-aks-cluster-with-new-service-principal-credentials). Este paso es necesario para que los cambios de la entidad de servicio se reflejen en el clúster de AKS.

### <a name="create-a-new-service-principal"></a>Creación de una nueva entidad de servicio

Si decidió actualizar las credenciales de la entidad de servicio existente en la sección anterior, omita este paso. Vaya a la sección [Actualización del clúster de AKS con nuevas credenciales de la entidad de servicio](#update-aks-cluster-with-new-service-principal-credentials).

Para crear una entidad de servicio y actualizar el clúster de AKS para usar estas nuevas credenciales, use el comando [az ad sp create-for-rbac][az-ad-sp-create]. En el ejemplo siguiente, el parámetro `--skip-assignment` impide la asignación de asignaciones predeterminadas adicionales:

```azurecli-interactive
az ad sp create-for-rbac --skip-assignment
```

La salida será similar al del ejemplo siguiente: Tome nota de sus valores `appId` y `password`. Estos valores se usan en el siguiente paso.

```json
{
  "appId": "7d837646-b1f3-443d-874c-fd83c7c739c5",
  "name": "7d837646-b1f3-443d-874c-fd83c7c739c",
  "password": "a5ce83c9-9186-426d-9183-614597c7f2f7",
  "tenant": "a4342dc8-cd0e-4742-a467-3129c469d0e5"
}
```

A continuación, defina las variables del identificador de la entidad de servicio y el secreto de cliente; para ello, use el resultado del comando [az ad sp create-for-rbac][az-ad-sp-create], tal como se muestra en el ejemplo siguiente. El valor de *SP_ID* es su *id. de aplicación* y el valor de *SP_SECRET* es su *contraseña*:

```console
SP_ID=7d837646-b1f3-443d-874c-fd83c7c739c5
SP_SECRET=a5ce83c9-9186-426d-9183-614597c7f2f7
```

Ahora, vaya a la sección [Actualización del clúster de AKS con nuevas credenciales de la entidad de servicio](#update-aks-cluster-with-new-service-principal-credentials). Este paso es necesario para que los cambios de la entidad de servicio se reflejen en el clúster de AKS.

## <a name="update-aks-cluster-with-new-service-principal-credentials"></a>Actualización del clúster de AKS con nuevas credenciales de la entidad de servicio

Independientemente de si decide actualizar las credenciales de la entidad de servicio existente o de si crea una entidad de servicio, debe actualizar de nuevo el clúster de AKS con sus credenciales mediante el comando [az aks update-credentials][az-aks-update-credentials]. Las variables para los valores de *--service-principal* y *--client-secret* se usan de la siguiente manera:

```azurecli-interactive
az aks update-credentials \
    --resource-group myResourceGroup \
    --name myAKSCluster \
    --reset-service-principal \
    --service-principal $SP_ID \
    --client-secret $SP_SECRET
```

Las credenciales de la entidad de servicio tardan unos minutos en actualizarse en AKS.

## <a name="update-aks-cluster-with-new-aad-application-credentials"></a>Actualización del clúster de AKS con nuevas credenciales de la aplicación AAD

Puede crear una nueva aplicación cliente y una nueva aplicación de servidor de AAD siguiendo los [pasos para integrar AAD][create-aad-app]. También puede restablecer las aplicaciones de AAD existentes siguiendo el [mismo método que para restablecer la entidad de servicio](#reset-existing-service-principal-credential). Una vez hecho esto, solo tendrá que actualizar las credenciales de la aplicación AAD utilizando el mismo comando [az aks update-credentials][az-aks-update-credentials], pero con las variables *--reset-aad*.

```azurecli-interactive
az aks update-credentials \
    --resource-group myResourceGroup \
    --name myAKSCluster \
    --reset-aad \
    --aad-server-app-id <SERVER APPLICATION ID> \
    --aad-server-app-secret <SERVER APPLICATION SECRET> \
    --aad-client-app-id <CLIENT APPLICATION ID>
```


## <a name="next-steps"></a>Pasos siguientes

En este artículo, lo que se ha actualizado es la propia entidad de servicio del clúster de AKS y las propias aplicaciones de integración de AAD. Para más información sobre cómo administrar la identidad de las cargas de trabajo de un clúster, consulte los [procedimientos recomendados para la autenticación y autorización en AKS][best-practices-identity].

<!-- LINKS - internal -->
[install-azure-cli]: /cli/azure/install-azure-cli
[az-aks-show]: /cli/azure/aks#az-aks-show
[az-aks-update-credentials]: /cli/azure/aks#az-aks-update-credentials
[best-practices-identity]: operator-best-practices-identity.md
[aad-integration]: azure-ad-integration.md
[create-aad-app]: azure-ad-integration.md#create-the-server-application
[az-ad-sp-create]: /cli/azure/ad/sp#az-ad-sp-create-for-rbac
[az-ad-sp-credential-reset]: /cli/azure/ad/sp/credential#az-ad-sp-credential-reset

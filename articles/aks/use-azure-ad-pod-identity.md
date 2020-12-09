---
title: Uso de identidades administradas del pod de Azure Active Directory en Azure Kubernetes Service (versión preliminar)
description: Aprenda a utilizar identidades administradas del pod de AAD en Azure Kubernetes Service (AKS)
services: container-service
ms.topic: article
ms.date: 12/01/2020
ms.openlocfilehash: 150e2e71a4db8ab07caad479ae098d5b9eb746da
ms.sourcegitcommit: 6a350f39e2f04500ecb7235f5d88682eb4910ae8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/01/2020
ms.locfileid: "96465883"
---
# <a name="use-azure-active-directory-pod-managed-identities-in-azure-kubernetes-service-preview"></a>Uso de identidades administradas del pod de Azure Active Directory en Azure Kubernetes Service (versión preliminar)

Las identidades administradas del pod de Azure Active Directory usan primitivas de Kubernetes para asociar [identidades administradas para recursos de Azure][az-managed-identities] e identidades en Azure Active Directory (AAD) con pods. Los administradores crean identidades y enlaces como primitivas de Kubernetes que permiten a los pods acceder a los recursos de Azure que dependen de AAD como proveedor de identidades.

[!INCLUDE [preview features callout](./includes/preview/preview-callout.md)]

## <a name="before-you-begin"></a>Antes de empezar

Debe tener instalado el siguiente recurso:

* La CLI de Azure, versión 2.8.0 o posterior
* La versión de la extensión `azure-preview` 0.4.68 o posterior

### <a name="limitations"></a>Limitaciones

* Se permite un máximo de 50 identidades de pod para un clúster.
* Se permite un máximo de 50 excepciones de identidades de pod para un clúster.
* Las identidades administradas del pod solo están disponibles en grupos de nodos de Linux.

### <a name="register-the-enablepodidentitypreview"></a>Registro de `EnablePodIdentityPreview`

Registre la característica `EnablePodIdentityPreview`:

```azurecli
az feature register --name EnablePodIdentityPreview --namespace Microsoft.ContainerService
```

### <a name="install-the-aks-preview-azure-cli"></a>Instalación de la CLI de Azure `aks-preview`

También se necesita la versión 0.4.64 o posterior de la extensión de la CLI de Azure *aks-preview*. Instale la extensión de la CLI de Azure *aks-preview* mediante el comando [az extension add][az-extension-add]. También puede instalar las actualizaciones disponibles mediante el comando [az extension update][az-extension-update].

```azurecli-interactive
# Install the aks-preview extension
az extension add --name aks-preview

# Update the extension to make sure you have the latest version installed
az extension update --name aks-preview
```

## <a name="create-an-aks-cluster-with-managed-identities"></a>Creación de un clúster de AKS con identidades administradas

Cree un clúster de AKS con una identidad administrada y una identidad administrada del pod habilitadas. Los siguientes comandos usan [az group create][az-group-create] para crear un grupo de recursos denominado *myResourceGroup* y el comando [az aks create][az-aks-create] para crear un clúster de AKS denominado *myAKSCluster* en el grupo de recursos *myResourceGroup*.

```azurecli-interactive
az group create --name myResourceGroup --location eastus
az aks create -g myResourceGroup -n myAKSCluster --enable-managed-identity --enable-pod-identity
```

Use [az aks get-credentials][az-aks-get-credentials] para iniciar sesión en el clúster de AKS. Este comando también descarga y configura el certificado de cliente `kubectl` en el equipo de desarrollo.

```azurecli-interactive
az aks get-credentials --resource-group myResourceGroup --name myAKSCluster
```

## <a name="create-an-identity"></a>Creación de una identidad

Cree una identidad mediante [az identity create][az-identity-create] y establezca las variables *IDENTITY_CLIENT_ID* e *IDENTITY_RESOURCE_ID*.

```azurecli-interactive
az group create --name myIdentityResourceGroup --location eastus
export IDENTITY_RESOURCE_GROUP="myIdentityResourceGroup"
export IDENTITY_NAME="application-identity"
az identity create --resource-group ${IDENTITY_RESOURCE_GROUP} --name ${IDENTITY_NAME}
export IDENTITY_CLIENT_ID="$(az identity show -g ${IDENTITY_RESOURCE_GROUP} -n ${IDENTITY_NAME} --query clientId -otsv)"
export IDENTITY_RESOURCE_ID="$(az identity show -g ${IDENTITY_RESOURCE_GROUP} -n ${IDENTITY_NAME} --query id -otsv)"
```

## <a name="create-a-pod-identity"></a>Creación de una identidad de pod

Cree una identidad de pod para el clúster mediante `az aks pod-identity add`.

> [!IMPORTANT]
> Debe tener los permisos adecuados, como `Owner`, en su suscripción para crear la identidad y el enlace de rol.

```azurecli-interactive
export POD_IDENTITY_NAME="my-pod-identity"
export POD_IDENTITY_NAMESPACE="my-app"
az aks pod-identity add --resource-group myResourceGroup --cluster-name myAKSCluster --namespace ${POD_IDENTITY_NAMESPACE}  --name ${POD_IDENTITY_NAME} --identity-resource-id ${IDENTITY_RESOURCE_ID}
```

> [!NOTE]
> Al habilitar la identidad administrada del pod en el clúster de AKS, se agrega una excepción AzurePodIdentityException denominada *aks-addon-exception* al espacio de nombres *kube-system*. Una excepción AzurePodIdentityException permite que los pods con determinadas etiquetas tengan acceso al punto de conexión de Azure Instance Metadata Service (IMDS) sin que el servidor de identidad administrada del nodo (NMI) lo intercepte. *aks-addon-exception* permite que los complementos propios de AKS, como la identidad administrada del pod de AAD, funcionen sin tener que configurar manualmente una excepción AzurePodIdentityException. También puede agregar, quitar y actualizar una excepción AzurePodIdentityException con `az aks pod-identity exception add`, `az aks pod-identity exception delete`, `az aks pod-identity exception update` o `kubectl`.

## <a name="run-a-sample-application"></a>Ejecución de una aplicación de ejemplo

Para que un pod use la identidad administrada del pod de AAD, el pod necesita una etiqueta *aadpodidbinding* con un valor que coincida con un selector de *AzureIdentityBinding*. Para ejecutar una aplicación de ejemplo mediante la identidad administrada del pod de AAD, cree un archivo de `demo.yaml` con el siguiente contenido. Reemplace *POD_IDENTITY_NAME*, *IDENTITY_CLIENT_ID* e *IDENTITY_RESOURCE_GROUP* por los valores de los pasos anteriores. Reemplace *SUBSCRIPTION_ID* por su identificador de suscripción.

> [!NOTE]
> En los pasos anteriores, creó las variables *POD_IDENTITY_NAME*, *IDENTITY_CLIENT_ID* e *IDENTITY_RESOURCE_GROUP*. Puede usar un comando como `echo` para mostrar el valor establecido para las variables; por ejemplo, `echo $IDENTITY_NAME`.

```yml
apiVersion: v1
kind: Pod
metadata:
  name: demo
  labels:
    aadpodidbinding: POD_IDENTITY_NAME
spec:
  containers:
  - name: demo
    image: mcr.microsoft.com/oss/azure/aad-pod-identity/demo:v1.6.3
    args:
      - --subscriptionid=SUBSCRIPTION_ID
      - --clientid=IDENTITY_CLIENT_ID
      - --resourcegroup=IDENTITY_RESOURCE_GROUP
    env:
      - name: MY_POD_NAME
        valueFrom:
          fieldRef:
            fieldPath: metadata.name
      - name: MY_POD_NAMESPACE
        valueFrom:
          fieldRef:
            fieldPath: metadata.namespace
      - name: MY_POD_IP
        valueFrom:
          fieldRef:
            fieldPath: status.podIP
  nodeSelector:
    kubernetes.io/os: linux
```

Observe que la definición del pod tiene una etiqueta *aadpodidbinding* con un valor que coincide con el nombre de la identidad de pod que ejecutó `az aks pod-identity add` en el paso anterior.

Implemente `demo.yaml` en el mismo espacio de nombres que la identidad de pod mediante `kubectl apply`:

```azurecli-interactive
kubectl apply -f demo.yaml --namespace $POD_IDENTITY_NAMESPACE
```

Compruebe que la aplicación de ejemplo se ejecuta correctamente con `kubectl logs`.

```azurecli-interactive
kubectl logs demo --follow --namespace $POD_IDENTITY_NAMESPACE
```

Compruebe que los registros muestran que un token se ha obtenido correctamente y que la operación *GET* es correcta.
 
```output
...
successfully doARMOperations vm count 0
successfully acquired a token using the MSI, msiEndpoint(http://169.254.169.254/metadata/identity/oauth2/token)
successfully acquired a token, userAssignedID MSI, msiEndpoint(http://169.254.169.254/metadata/identity/oauth2/token) clientID(xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx)
successfully made GET on instance metadata
...
```

## <a name="clean-up"></a>Limpieza

Para quitar la identidad administrada del pod de AAD del clúster, quite la aplicación de ejemplo y la identidad de pod del clúster. A continuación, quite la identidad.

```azurecli-interactive
kubectl delete pod demo --namespace $POD_IDENTITY_NAMESPACE
az aks pod-identity delete --name ${POD_IDENTITY_NAME} --namespace ${POD_IDENTITY_NAMESPACE} --resource-group myResourceGroup --cluster-name myAKSCluster
az identity delete -g ${IDENTITY_RESOURCE_GROUP} -n ${IDENTITY_NAME}
```

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre las identidades administradas, consulte [Identidades administradas para recursos de Azure][az-managed-identities].

<!-- LINKS - external -->
[az-aks-create]: /cli/azure/aks?view=azure-cli-latest#az-aks-create
[az-aks-get-credentials]: /cli/azure/aks?view=azure-cli-latest#az-aks-get-credentials
[az-extension-add]: /cli/azure/extension?view=azure-cli-latest#az-extension-add&preserve-view=true
[az-extension-update]: /cli/azure/extension?view=azure-cli-latest#az-extension-update&preserve-view=true
[az-group-create]: /cli/azure/group#az-group-create
[az-identity-create]: /cli/azure/identity?view=azure-cli-latest#az_identity_create
[az-managed-identities]: ../active-directory/managed-identities-azure-resources/overview.md
[az-role-assignment-create]: /cli/azure/role/assignment?view=azure-cli-latest#az_role_assignment_create

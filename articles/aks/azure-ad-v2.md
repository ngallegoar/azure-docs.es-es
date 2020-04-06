---
title: Uso de Azure AD en Azure Kubernetes Service
description: Aprenda a usar Azure AD en Azure Kubernetes Service (AKS).
services: container-service
manager: gwallace
ms.topic: article
ms.date: 03/24/2020
ms.openlocfilehash: 1ca4b70139ed5e0a136f6f5f2b0382b8c1688983
ms.sourcegitcommit: 0553a8b2f255184d544ab231b231f45caf7bbbb0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/30/2020
ms.locfileid: "80389416"
---
# <a name="integrate-azure-ad-in-azure-kubernetes-service-preview"></a>Integración de Azure AD en Azure Kubernetes Service (versión preliminar)

> [!Note]
> Los clústeres de AKS v1 existentes con integración de Azure AD no se ven afectados por la nueva experiencia de AKS v2.

La integración de Azure AD con AKS v2 está diseñada para simplificar la integración de Azure AD con la experiencia de AKS v1, donde los usuarios debían crear una aplicación de cliente y otra de servidor, y donde el inquilino de Azure AD debía conceder permisos de lectura para Active Directory. En la nueva versión, el proveedor de recursos de AKS administra las aplicaciones de cliente y servidor.

## <a name="limitations"></a>Limitaciones

* Actualmente no se puede actualizar un clúster de AKS v1 existente habilitado para Azure AD a la experiencia de v2.

> [!IMPORTANT]
> Las características en versión preliminar de AKS están disponibles como opción de participación y autoservicio. Las versiones preliminares se proporcionan "tal cual" y "como están disponibles", y están excluidas de los Acuerdos de Nivel de Servicio y la garantía limitada. Las versiones preliminares de AKS reciben cobertura parcial del soporte al cliente en la medida de lo posible. Por lo tanto, estas características no están diseñadas para usarse en producción. Para más información, consulte los siguientes artículos de soporte:
>
> - [Directivas de soporte técnico para AKS](support-policies.md)
> - [Preguntas más frecuentes de soporte técnico de Azure](faq.md)

## <a name="before-you-begin"></a>Antes de empezar

Debe tener instalados los siguientes recursos:

- CLI de Azure, versión 2.2.0 o cualquier versión posterior
- Extensión aks-preview 0.4.38
- Kubectl con la versión [beta 1.18](https://github.com/kubernetes/kubernetes/blob/master/CHANGELOG/CHANGELOG-1.18.md#client-binaries) como mínimo

Para instalar o actualizar la extensión aks-preview, use los siguientes comandos de la CLI de Azure:

```azurecli
az extension add --name aks-preview
az extension list
```

```azurecli
az extension update --name aks-preview
az extension list
```

Para instalar kubectl, use lo siguiente:
```azurecli
curl -LO "https://storage.googleapis.com/kubernetes-release/release/v1.18.0-beta.2/bin/linux/amd64/kubectl"
chmod +x ./kubectl
sudo mv ./kubectl /usr/local/bin/kubectl
kubectl version --client
```

Siga [estas instrucciones](https://kubernetes.io/docs/tasks/tools/install-kubectl/) para otros sistemas operativos.

> [!CAUTION]
> Actualmente, después de registrar una característica en una suscripción no se puede anular el registro de esa característica. Al habilitar algunas características en versión preliminar, es posible que se usen valores predeterminados en todos los clústeres de AKS creados más tarde en la suscripción. No habilite características en vista previa en las suscripciones de producción. En su lugar, use una suscripción independiente para probar las características en versión preliminar y recopilar comentarios.

```azurecli-interactive
az feature register --name AAD-V2 --namespace Microsoft.ContainerService
```

Pueden pasar unos minutos hasta que el estado aparezca como **Registrado**. Puede comprobar el estado del registro con el comando [az feature list](https://docs.microsoft.com/cli/azure/feature?view=azure-cli-latest#az-feature-list):

```azurecli-interactive
az feature list -o table --query "[?contains(name, 'Microsoft.ContainerService/AAD-V2')].{Name:name,State:properties.state}"
```

Cuando el estado se muestre como Registrado, actualice el registro del proveedor de recursos `Microsoft.ContainerService` mediante el comando [az provider register](https://docs.microsoft.com/cli/azure/provider?view=azure-cli-latest#az-provider-register):

```azurecli-interactive
az provider register --namespace Microsoft.ContainerService
```

## <a name="create-an-aks-cluster-with-azure-ad-enabled"></a>Creación de un clúster de AKS con Azure AD habilitado

Ahora puede crear un clúster de AKS usando los siguientes comandos de la CLI.

En primer lugar, cree un grupo de recursos de Azure:

```azurecli-interactive
# Create an Azure resource group
az group create --name myResourceGroup --location centralus
```

Después, cree un clúster de AKS:

```azurecli-interactive
az aks create -g MyResourceGroup -n MyManagedCluster --enable-aad
```
El comando anterior crea un clúster de AKS de tres nodos, pero, de forma predeterminada, el usuario que ha creado el clúster no es miembro de un grupo con acceso a este clúster. Este usuario debe crear un grupo de Azure AD, agregarse como miembro y, a continuación, actualizar el clúster como se muestra a continuación. Siga las instrucciones que encontrará [aquí](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-groups-create-azure-portal).

Después de crear un grupo y agregarse usted mismo (y a otros) como miembro, puede actualizar el clúster con el grupo de Azure AD mediante el siguiente comando:

```azurecli-interactive
az aks update -g MyResourceGroup -n MyManagedCluster --enable-aad [--aad-admin-group-object-ids <id1,id2>] [--aad-tenant-id <id>]
```
De forma alternativa, si primero crea un grupo y agrega miembros, puede habilitar el grupo de Azure AD en el momento de la creación con el siguiente comando:

```azurecli-interactive
az aks create -g MyResourceGroup -n MyManagedCluster --enable-aad [--aad-admin-group-object-ids <id1,id2>] [--aad-tenant-id <id>]
```

Un clúster de Azure AD v2 creado correctamente contiene la siguiente sección en el cuerpo de la respuesta:
```
"Azure ADProfile": {
    "adminGroupObjectIds": null,
    "clientAppId": null,
    "managed": true,
    "serverAppId": null,
    "serverAppSecret": null,
    "tenantId": "72f9****-****-****-****-****d011db47"
  }
```

El clúster se crea en pocos minutos.

## <a name="access-an-azure-ad-enabled-cluster"></a>Acceso a un clúster habilitado para Azure AD
Para obtener las credenciales de administrador para acceder al clúster, use el siguiente comando:

```azurecli-interactive
az aks get-credentials --resource-group myResourceGroup --name MyManagedCluster --admin
```
A continuación, use el comando "kubectl get nodes" para ver los nodos del clúster:

```azurecli-interactive
kubectl get nodes

NAME                       STATUS   ROLES   AGE    VERSION
aks-nodepool1-15306047-0   Ready    agent   102m   v1.15.10
aks-nodepool1-15306047-1   Ready    agent   102m   v1.15.10
aks-nodepool1-15306047-2   Ready    agent   102m   v1.15.10
```

Para obtener las credenciales de usuario para acceder al clúster, use el siguiente comando:
 
```azurecli-interactive
 az aks get-credentials --resource-group myResourceGroup --name MyManagedCluster
```
Siga las instrucciones para iniciar sesión.

Recibirá el mensaje: **You must be logged in to the server (Unauthorized)** (Debe iniciar sesión en el servidor [no autorizado]).

El usuario anterior recibe un error porque no forma parte de un grupo con acceso al clúster.

## <a name="next-steps"></a>Pasos siguientes

Obtenga más información sobre el [control de acceso basado en roles de Azure AD][azure-ad-rbac].

<!-- LINKS - Internal -->
[azure-ad-rbac]: azure-ad-rbac.md

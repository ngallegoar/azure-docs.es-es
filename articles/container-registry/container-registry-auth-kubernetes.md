---
title: Autenticación desde un clúster de Kubernetes
description: Obtenga información sobre cómo proporcionar a un clúster de Kubernetes acceso a las imágenes en su instancia de Azure Container Registry al crear un secreto de extracción con una entidad de servicio.
ms.topic: article
author: karolz-ms
ms.author: karolz
ms.reviewer: danlep
ms.date: 05/28/2020
ms.openlocfilehash: fbf5dfd68b823b600b11cad3643e5d4004b85ff5
ms.sourcegitcommit: 69156ae3c1e22cc570dda7f7234145c8226cc162
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/03/2020
ms.locfileid: "84309822"
---
# <a name="pull-images-from-an-azure-container-registry-to-a-kubernetes-cluster"></a>Extracción de imágenes de una instancia de Azure Container Registry a un clúster de Kubernetes

Puede usar una instancia de Azure Container Registry como origen de las imágenes de contenedor con cualquier clúster de Kubernetes, incluidos los clústeres de Kubernetes "locales", como [minikube](https://minikube.sigs.k8s.io/) y [kind](https://kind.sigs.k8s.io/). En este artículo se muestra cómo crear un secreto de extracción de Kubernetes basado en una entidad de servicio de Azure Active Directory. A continuación, use el secreto para extraer imágenes de una instancia de Azure Container Registry en una implementación de Kubernetes.

> [!TIP]
> Si usa la instancia administrada de [Azure Kubernetes Service](../aks/intro-kubernetes.md), también puede [integrar el clúster](../aks/cluster-container-registry-integration.md?toc=/azure/container-registry/toc.json&bc=/azure/container-registry/breadcrumb/toc.json) en una instancia de Azure Container Registry de destino para las extracciones de imágenes. 

En este artículo se supone que ya ha creado una instancia privada de Azure Container Registry. Además, debe tener un clúster de Kubernetes en ejecución y accesible a través de la herramienta de línea de comandos `kubectl`.

[!INCLUDE [container-registry-service-principal](../../includes/container-registry-service-principal.md)]

Si no guarda ni recuerda la contraseña de la entidad de servicio, puede restablecerla con el comando [az ad sp credential reset][az-ad-sp-credential-reset]:

```azurecli
az ad sp credential reset  --name http://<service-principal-name> --query password --output tsv
```

Este comando devuelve una nueva contraseña válida para la entidad de servicio.

## <a name="create-an-image-pull-secret"></a>Creación de un secreto de extracción de imágenes

Kubernetes usa un *secreto de extracción de imágenes* para almacenar la información necesaria para autenticarse en el registro. Para crear el secreto de extracción para una instancia de Azure Container Registry, debe proporcionar el identificador de la entidad de servicio, la contraseña y la dirección URL del registro. 

Cree un secreto de extracción de imágenes con el siguiente comando `kubectl`:

```console
kubectl create secret docker-registry <secret-name> \
    --namespace <namespace> \
    --docker-server=<container-registry-name>.azurecr.io \
    --docker-username=<service-principal-ID> \
    --docker-password=<service-principal-password>
```
donde:

| Value | Descripción |
| :--- | :--- |
| `secret-name` | Nombre del secreto de extracción de imágenes, por ejemplo, *acr-secret* |
| `namespace` | Espacio de nombres de Kubernetes donde colocar el secreto <br/> Solo es necesario si desea colocar el secreto en un espacio de nombres distinto del espacio de nombres predeterminado |
| `container-registry-name` | Nombre de la instancia de Azure Container Registry, por ejemplo, *miregistro*<br/><br/>`--docker-server` es el nombre completo del servidor de inicio de sesión del registro  |
| `service-principal-ID` | Identificador de la entidad de servicio que usará Kubernetes para tener acceso al registro |
| `service-principal-password` | Contraseña de la entidad de servicio |

## <a name="use-the-image-pull-secret"></a>Uso del secreto de extracción de imágenes

Una vez que haya creado el secreto de extracción de imágenes, puede usarlo para crear implementaciones y pods de Kubernetes. Proporcione el nombre del secreto en `imagePullSecrets` en el archivo de implementación. Por ejemplo:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: my-awesome-app-pod
  namespace: awesomeapps
spec:
  containers:
    - name: main-app-container
      image: myregistry.azurecr.io/my-awesome-app:v1
      imagePullPolicy: IfNotPresent
  imagePullSecrets:
    - name: acr-secret
```

En el ejemplo anterior, `my-awesome-app:v1` es el nombre de la imagen que se va a extraer de Azure Container Registry, y `acr-secret` es el nombre del secreto de extracción que creó para tener acceso al registro. Al implementar el pod, Kubernetes extrae automáticamente la imagen del registro, si aún no está presente en el clúster.


## <a name="next-steps"></a>Pasos siguientes

* Para más información sobre cómo trabajar con entidades de servicio y Azure Container Registry, consulte [Autenticación de Azure Container Registry con entidades de servicio](container-registry-auth-service-principal.md).
* Más información sobre los secretos de extracción de imágenes en la [documentación de Kubernetes](https://kubernetes.io/docs/concepts/containers/images/#specifying-imagepullsecrets-on-a-pod)


<!-- IMAGES -->

<!-- LINKS - External -->
[acr-scripts-cli]: https://github.com/Azure/azure-docs-cli-python-samples/tree/master/container-registry
[acr-scripts-psh]: https://github.com/Azure/azure-docs-powershell-samples/tree/master/container-registry

<!-- LINKS - Internal -->
[az-ad-sp-credential-reset]: /cli/azure/ad/sp/credential#az-ad-sp-credential-reset
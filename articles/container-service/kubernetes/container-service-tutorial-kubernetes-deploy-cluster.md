---
title: '(EN DESUSO) Tutorial de Azure Container Service: Implementación de clúster'
description: 'Tutorial de Azure Container Service: Implementación de clúster'
author: iainfoulds
ms.service: container-service
ms.topic: tutorial
ms.date: 09/14/2017
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: b8821f3bb3d48786697cbc4137baf530856774fd
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/24/2020
ms.locfileid: "78274003"
---
# <a name="deprecated-deploy-a-kubernetes-cluster-in-azure-container-service"></a>(EN DESUSO) Implementación de un clúster de Kubernetes en Azure Container Service

> [!TIP]
> Para la versión actualizada de este tutorial que usa Azure Kubernetes Service, consulte [Tutorial: Implementación de un clúster de Azure Kubernetes Service (AKS)](../../aks/tutorial-kubernetes-deploy-cluster.md).

[!INCLUDE [ACS deprecation](../../../includes/container-service-kubernetes-deprecation.md)]

Kubernetes proporciona una plataforma distribuida para aplicaciones en contenedores. Con Azure Container Service, el aprovisionamiento de un clúster de Kubernetes listo para producción se realiza de forma rápida y sencilla. En este tutorial, la tercera parte de siete, se implementa un clúster de Kubernetes de Azure Container Service. Los pasos completados incluyen:

> [!div class="checklist"]
> * Implementación de un clúster de ACS de Kubernetes
> * Instalación de la CLI de Kubernetes (kubectl)
> * Configuración de kubectl

En tutoriales posteriores, la aplicación Azure Vote se implementa en el clúster, se escala y se actualiza, y Log Analytics se configura para supervisar el clúster de Kubernetes.

## <a name="before-you-begin"></a>Antes de empezar

En los tutoriales anteriores, se creó una imagen de contenedor y se actualizó en una instancia de Azure Container Registry. Si no ha realizado estos pasos, pero desea continuar, vuelva al tutorial [Create container images to be used with Azure Container Service](./container-service-tutorial-kubernetes-prepare-app.md) (Creación de las imágenes de contenedor que se usan con Azure Container Service).

## <a name="create-kubernetes-cluster"></a>Creación de un clúster de Kubernetes

Cree un clúster de Kubernetes en Azure Container Service con el comando [az acs create](/cli/azure/acs#az-acs-create). 

En el siguiente ejemplo se crea un clúster denominado `myK8sCluster` en un grupo de recursos llamado `myResourceGroup`. Este grupo de recursos se creó en el [tutorial anterior](./container-service-tutorial-kubernetes-prepare-acr.md).

```azurecli-interactive
az acs create --orchestrator-type kubernetes --resource-group myResourceGroup --name myK8SCluster --generate-ssh-keys 
```

En algunos casos, como con las versiones de evaluación de tiempo limitado, una suscripción a Azure tiene un acceso limitado a los recursos de Azure. Si se produce un error en la implementación debido a que los núcleos disponibles son limitados, reduzca el número de agentes predeterminado agregando `--agent-count 1` al comando [az acs create](/cli/azure/acs#az-acs-create). 

Después de varios minutos, la implementación se completa y devuelve información en formato json sobre la implementación de ACS.

## <a name="install-the-kubectl-cli"></a>Instalación de la CLI de kubectl

Para conectarse al clúster de Kubernetes desde el equipo cliente, use [kubectl](https://kubernetes.io/docs/user-guide/kubectl/), el cliente de la línea de comandos de Kubernetes. 

Si usa Azure Cloud Shell, kubectl ya está instalado. Si desea instalarlo de forma local, use el comando [az acs kubernetes install-cli](/cli/azure/acs/kubernetes).

Si está ejecutando en Linux o macOS, debe ejecutar con sudo. En Windows, asegúrese de que el shell se ha ejecutado como administrador.

```azurecli-interactive
az acs kubernetes install-cli 
```

En Windows, la instalación predeterminada es *c:\program files (x86)\kubectl.exe*. Puede que deba agregar este archivo a la ruta de acceso de Windows. 

## <a name="connect-with-kubectl"></a>Conexión con kubectl

Para configurar kubectl para conectarse al clúster de Kubernetes, ejecute el comando [az acs kubernetes get-credentials](/cli/azure/acs/kubernetes).

```azurecli-interactive
az acs kubernetes get-credentials --resource-group myResourceGroup --name myK8SCluster
```

Para comprobar la conexión al clúster, ejecute el comando [kubectl get nodes](https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get).

```console
kubectl get nodes
```

Salida:

```output
NAME                    STATUS                     AGE       VERSION
k8s-agent-98dc3136-0    Ready                      5m        v1.6.2
k8s-agent-98dc3136-1    Ready                      5m        v1.6.2
k8s-agent-98dc3136-2    Ready                      5m        v1.6.2
k8s-master-98dc3136-0   Ready,SchedulingDisabled   5m        v1.6.2
```

Al finalizar el tutorial, tendrá un clúster de ACS de Kubernetes preparado para cargas de trabajo. En los tutoriales posteriores, una aplicación de contenedores múltiples está implementada en este clúster, escalada horizontalmente, actualizada y supervisada.

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, se implementó un clúster de Kubernetes de Azure Container Service. Se han completado los siguientes pasos:

> [!div class="checklist"]
> * Implementación de un clúster de ACS de Kubernetes
> * Instalación de la CLI de Kubernetes (kubectl)
> * Configuración de kubectl

Avance al siguiente tutorial para aprender a ejecutar la aplicación en el clúster.

> [!div class="nextstepaction"]
> [Implementación de una aplicación en Kubernetes](./container-service-tutorial-kubernetes-deploy-application.md)

---
title: '(EN DESUSO) Tutorial de Azure Container Service: escalado de una aplicación'
description: 'Tutorial de Azure Container Service: escalado de una aplicación'
author: dlepow
ms.service: container-service
ms.topic: tutorial
ms.date: 09/14/2017
ms.author: danlep
ms.custom: mvc
ms.openlocfilehash: 2ea8a5428c1fabdfda4f2298c0559792537df481
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/24/2020
ms.locfileid: "78273985"
---
# <a name="deprecated-scale-kubernetes-pods-and-kubernetes-infrastructure"></a>(EN DESUSO) Escalado de pods de Kubernetes e infraestructura de Kubernetes

> [!TIP]
> Para la versión actualizada de este tutorial que usa Azure Kubernetes Service, consulte [Tutorial: Escalado de aplicaciones en Azure Kubernetes Service (AKS)](../../aks/tutorial-kubernetes-scale.md).

[!INCLUDE [ACS deprecation](../../../includes/container-service-kubernetes-deprecation.md)]

Si ha ido siguiendo los tutoriales, tiene un clúster de Kubernetes en funcionamiento en Azure Container Service y ha implementado la aplicación Azure Voting. 

En este tutorial, la parte cinco de siete, se escalan horizontalmente los pods en la aplicación y se prueba el escalado automático de pods. También se aprende a escalar el número de nodos de agente de VM de Azure para cambiar la capacidad del clúster de hospedar cargas de trabajo. Las tareas completadas incluyen:

> [!div class="checklist"]
> * Escalado manual de pods de Kubernetes
> * Configuración de pods de Autoscale que se ejecuten en el front-end de la aplicación
> * Escalado de los nodos de agente de Azure para Kubernetes

En tutoriales posteriores se actualiza la aplicación Azure Vote y se configura Log Analytics para supervisar el clúster de Kubernetes.

## <a name="before-you-begin"></a>Antes de empezar

En tutoriales anteriores se ha empaquetado una aplicación en una imagen de contenedor, esta imagen se ha cargado en Azure Container Registry y se ha creado un clúster de Kubernetes. La aplicación se ejecutó después en el clúster de Kubernetes. 

Si no ha realizado estos pasos y quiere continuar, vuelva al [paso 1 del tutorial: creación de imágenes de contenedor](./container-service-tutorial-kubernetes-prepare-app.md). 

## <a name="manually-scale-pods"></a>Escalado manual de pods

Hasta el momento se han implementado el front-end de Azure Vote y la instancia de Redis, cada uno con una única réplica. Para comprobarlo, ejecute el comando [kubectl get](https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get).

Vaya a [https://shell.azure.com](https://shell.azure.com) para abrir Cloud Shell en el explorador.

```console
kubectl get pods
```

Salida:

```output
NAME                               READY     STATUS    RESTARTS   AGE
azure-vote-back-2549686872-4d2r5   1/1       Running   0          31m
azure-vote-front-848767080-tf34m   1/1       Running   0          31m
```

Cambie manualmente el número de pods en la implementación de `azure-vote-front` mediante el comando [kubectl scale](https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#scale). En este ejemplo el número se aumenta a cinco.

```console
kubectl scale --replicas=5 deployment/azure-vote-front
```

Ejecute [kubectl get pods](https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get) para comprobar que Kubernetes crea los pods. Tras aproximadamente un minuto, se ejecutan los pods adicionales:

```console
kubectl get pods
```

Salida:

```output
NAME                                READY     STATUS    RESTARTS   AGE
azure-vote-back-2606967446-nmpcf    1/1       Running   0          15m
azure-vote-front-3309479140-2hfh0   1/1       Running   0          3m
azure-vote-front-3309479140-bzt05   1/1       Running   0          3m
azure-vote-front-3309479140-fvcvm   1/1       Running   0          3m
azure-vote-front-3309479140-hrbf2   1/1       Running   0          15m
azure-vote-front-3309479140-qphz8   1/1       Running   0          3m
```

## <a name="autoscale-pods"></a>Escalado automático de pods

Kubernetes admite el [escalado horizontal automático de pods](https://kubernetes.io/docs/tasks/run-application/horizontal-pod-autoscale/) para ajustar el número de pods en una implementación en función del uso de la CPU o de otras métricas de selección. 

Para usar el escalador automático, los pods deben tener solicitudes de la CPU y límites definidos. En la implementación de `azure-vote-front`, el contenedor del front-end solicita 0,25 CPU, con un límite de 0,5 CPU. La configuración tiene el siguiente aspecto:

```yaml
resources:
  requests:
     cpu: 250m
  limits:
     cpu: 500m
```

En el ejemplo siguiente se usa el comando [kubectl autoscale](https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#autoscale) para escalar automáticamente el número de pods en la implementación de `azure-vote-front`. En este contexto, si el uso de la CPU supera el 50 %, el escalador automático aumenta el número de pods a un máximo de 10.


```console
kubectl autoscale deployment azure-vote-front --cpu-percent=50 --min=3 --max=10
```

Para ver el estado del escalador automático, ejecute el siguiente comando.

```console
kubectl get hpa
```

Salida:

```output
NAME               REFERENCE                     TARGETS    MINPODS   MAXPODS   REPLICAS   AGE
azure-vote-front   Deployment/azure-vote-front   0% / 50%   3         10        3          2m
```

Después de unos minutos con carga mínima en la aplicación Azure Vote, el número de réplicas del pod se reduce automáticamente a tres.

## <a name="scale-the-agents"></a>Escalado de los agentes

Si en el tutorial anterior ha creado el clúster de Kubernetes mediante comandos predeterminados, este tiene tres nodos de agente. Puede ajustar el número de pods manualmente si tiene previsto usar más o menos cargas de trabajo en el clúster. Use el comando [az acs scale](/cli/azure/acs#az-acs-scale) y especifique el número de agentes con el parámetro `--new-agent-count`.

En el ejemplo siguiente, el número de nodos de agente se aumenta a 4 en el clúster de Kubernetes denominado *myK8sCluster*. El comando tarda unos minutos en completarse.

```azurecli-interactive
az acs scale --resource-group=myResourceGroup --name=myK8SCluster --new-agent-count 4
```

La salida del comando muestra el número de nodos de agente en el valor de `agentPoolProfiles:count`:

```output
{
  "agentPoolProfiles": [
    {
      "count": 4,
      "dnsPrefix": "myK8SCluster-myK8SCluster-e44f25-k8s-agents",
      "fqdn": "",
      "name": "agentpools",
      "vmSize": "Standard_D2_v2"
    }
  ],
...

```

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, se han usado distintas características de escalado en el clúster de Kubernetes. Estas son las tareas que se han tratado:

> [!div class="checklist"]
> * Escalado manual de pods de Kubernetes
> * Configuración de pods de Autoscale que se ejecuten en el front-end de la aplicación
> * Escalado de los nodos de agente de Azure para Kubernetes

Vaya al siguiente tutorial para aprender a actualizar la aplicación en Kubernetes.

> [!div class="nextstepaction"]
> [Actualización de una aplicación en Kubernetes](./container-service-tutorial-kubernetes-app-update.md)


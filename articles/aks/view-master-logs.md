---
title: Visualización de los registros de controlador de Azure Kubernetes Service (AKS)
description: Aprenda a habilitar y ver los registros del nodo maestro de Kubernetes en Azure Kubernetes Service (AKS).
services: container-service
ms.topic: article
ms.date: 01/03/2019
ms.openlocfilehash: a0207ebbb1596e41ad65e21a769d7041a239f767
ms.sourcegitcommit: 3c66bfd9c36cd204c299ed43b67de0ec08a7b968
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "90004874"
---
# <a name="enable-and-review-kubernetes-master-node-logs-in-azure-kubernetes-service-aks"></a>Habilitación y revisión de los registros del nodo maestro de Kubernetes en Azure Kubernetes Service (AKS)

Con Azure Kubernetes Service (AKS), los componentes principales, como *kube-apiserver* y *kube-controller-manager* se proporcionan como un servicio administrado. Cree y administre los nodos que ejecutan *kubelet* y el entorno de tiempo de ejecución del contenedor, e implemente las aplicaciones mediante el servidor de API de Kubernetes administrado. Para ayudar a solucionar los problemas de sus aplicaciones y servicios, quizás deba ver los registros generados por estos componentes principales. En este artículo se muestra cómo usar registros de Azure Monitor para habilitar y consultar los registros de los componentes principales de Kubernetes.

## <a name="before-you-begin"></a>Antes de empezar

En este artículo se requiere un clúster de AKS existente que se ejecute en su cuenta de Azure. Si todavía no tiene un clúster de AKS, cree uno con la [CLI de Azure][cli-quickstart] o [Azure Portal][portal-quickstart]. Registros de Azure Monitor funciona tanto con clústeres de AKS habilitados para RBAC como no habilitados para RBAC.

## <a name="enable-resource-logs"></a>Habilitación de registros de recursos

Para ayudar a recopilar y revisar los datos de varios orígenes, registros de Azure Monitor proporciona un motor de lenguaje y análisis de consultas que ofrece información detallada sobre su entorno. Un área de trabajo se usa para intercalar y analizar los datos, y se puede integrar con otros servicios de Azure como Application Insights y Security Center. Si desea utilizar una plataforma diferente para analizar los registros, puede decidir en su lugar enviar los registros de recursos a una cuenta de almacenamiento o un centro de eventos de Azure. Para más información, consulte el artículo sobre los [registros de Azure Monitor][log-analytics-overview].

Los registros de Azure Monitor se habilitan y administran en Azure Portal. Para habilitar la recopilación de registros para los componentes principales de Kubernetes en el clúster de AKS, abra Azure Portal en un explorador web y realice los pasos siguientes:

1. Seleccione el grupo de recursos del clúster de AKS, como *myResourceGroup*. No seleccione el grupo de recursos que contiene los recursos del clúster de AKS individuales, como *MC_myResourceGroup_myAKSCluster_eastus*.
1. En la parte izquierda, elija **Configuración de diagnóstico**.
1. Seleccione el clúster de AKS, como *myAKSCluster*, y elija **Agregar configuración de diagnóstico**.
1. Escriba un nombre, como *myAKSClusterLogs* y seleccione la opción **Send to Log Analytics** (Enviar a Log Analytics).
1. Seleccione un área de trabajo existente o cree uno. Al crear un área de trabajo, proporciónele un nombre, un grupo de recursos y una ubicación.
1. En la lista de registros disponibles, seleccione los que desea habilitar. En este ejemplo, habilite los registros de *kube-audit*. Los registros típicos incluyen *kube-apiserver*, *kube-controller-manager* y *kube-scheduler*. Puede volver y cambiar los registros recopilados una vez que las áreas de trabajo de Log Analytics está habilitadas.
1. Cuando esté listo, seleccione **Guardar** para habilitar la recopilación de los registros seleccionados.

## <a name="schedule-a-test-pod-on-the-aks-cluster"></a>Programación de un pod de prueba en el clúster de AKS

Para generar algunos registros, cree un nuevo pod en el clúster de AKS. El siguiente ejemplo de manifiesto de YAML puede usarse para crear una instancia básica de NGINX. Cree un archivo denominado `nginx.yaml` en el editor que prefiera y pegue el contenido siguiente:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx
spec:
  containers:
  - name: mypod
    image: nginx:1.15.5
    resources:
      requests:
        cpu: 100m
        memory: 128Mi
      limits:
        cpu: 250m
        memory: 256Mi
    ports:
    - containerPort: 80
```

Cree el pod con el comando [kubectl create][kubectl-create] y especifique el archivo YAML, como se muestra en el ejemplo siguiente:

```
$ kubectl create -f nginx.yaml

pod/nginx created
```

## <a name="view-collected-logs"></a>Visualización de los datos recopilados

Los registros de diagnóstico pueden tardar unos minutos en habilitarse y aparecer. En Azure Portal, vaya al clúster de AKS y seleccione **Registros** en el lado izquierdo. Si aparece, cierre la ventana *Consultas de ejemplo*.

En el lado izquierdo, elija **Registros**. Para ver los registros de *kube-audit*, escriba la siguiente consulta en el cuadro de texto:

```
KubePodInventory
| where TimeGenerated > ago(1d)
```

Es probable que se devuelvan muchos registros. Para reducir el ámbito de la consulta y ver los registros sobre el pod de NGINX creado en el paso anterior, agregue una instrucción *where* adicional para buscar *nginx*, como se muestra en la consulta de ejemplo siguiente:

```
KubePodInventory
| where TimeGenerated > ago(1d)
| where Name contains "nginx"
```

Para más información acerca de cómo consultar y filtrar datos de registro, consulte el artículo de [Visualización o análisis de los datos recopilados con la búsqueda de registros de Log Analytics][analyze-log-analytics].

## <a name="log-event-schema"></a>Esquema de evento de registro

AKS registra los siguientes eventos:

* [AzureActivity][log-schema-azureactivity]
* [AzureMetrics][log-schema-azuremetrics]
* [ContainerImageInventory][log-schema-containerimageinventory]
* [ContainerInventory][log-schema-containerinventory]
* [ContainerLog][log-schema-containerlog]
* [ContainerNodeInventory][log-schema-containernodeinventory]
* [ContainerServiceLog][log-schema-containerservicelog]
* [Heartbeat][log-schema-heartbeat]
* [InsightsMetrics][log-schema-insightsmetrics]
* [KubeEvents][log-schema-kubeevents]
* [KubeHealth][log-schema-kubehealth]
* [KubeMonAgentEvents][log-schema-kubemonagentevents]
* [KubeNodeInventory][log-schema-kubenodeinventory]
* [KubePodInventory][log-schema-kubepodinventory]
* [KubeServices][log-schema-kubeservices]
* [Perf][log-schema-perf]

## <a name="log-roles"></a>Roles de registro

| Role                     | Descripción |
|--------------------------|-------------|
| *aksService*             | El nombre para mostrar en el registro de auditoría de la operación de plano de control (desde hcpService). |
| *masterclient*           | El nombre para mostrar en el registro de auditoría de MasterClientCertificate, el certificado que obtiene desde az aks get-credentials. |
| *nodeclient*             | El nombre para mostrar de ClientCertificate, que los nodos de agente usan. |

## <a name="next-steps"></a>Pasos siguientes

En este artículo, aprendió a habilitar y revisar los registros de los componentes principales de Kubernetes en el clúster de AKS. Para seguir supervisando y solucionando problemas, también puede [ver los registros de Kubelet][kubelet-logs] y [habilitar el acceso al nodo SSH][aks-ssh].

<!-- LINKS - external -->
[kubectl-create]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#create

<!-- LINKS - internal -->
[cli-quickstart]: kubernetes-walkthrough.md
[portal-quickstart]: kubernetes-walkthrough-portal.md
[log-analytics-overview]: ../azure-monitor/log-query/log-query-overview.md
[analyze-log-analytics]: ../azure-monitor/log-query/get-started-portal.md
[kubelet-logs]: kubelet-logs.md
[aks-ssh]: ssh.md
[az-feature-register]: /cli/azure/feature#az-feature-register
[az-feature-list]: /cli/azure/feature#az-feature-list
[az-provider-register]: /cli/azure/provider#az-provider-register
[log-schema-azureactivity]: /azure/azure-monitor/reference/tables/azureactivity
[log-schema-azuremetrics]: /azure/azure-monitor/reference/tables/azuremetrics
[log-schema-containerimageinventory]: /azure/azure-monitor/reference/tables/containerimageinventory
[log-schema-containerinventory]: /azure/azure-monitor/reference/tables/containerinventory
[log-schema-containerlog]: /azure/azure-monitor/reference/tables/containerlog
[log-schema-containernodeinventory]: /azure/azure-monitor/reference/tables/containernodeinventory
[log-schema-containerservicelog]: /azure/azure-monitor/reference/tables/containerservicelog
[log-schema-heartbeat]: /azure/azure-monitor/reference/tables/heartbeat
[log-schema-insightsmetrics]: /azure/azure-monitor/reference/tables/insightsmetrics
[log-schema-kubeevents]: /azure/azure-monitor/reference/tables/kubeevents
[log-schema-kubehealth]: /azure/azure-monitor/reference/tables/kubehealth
[log-schema-kubemonagentevents]: /azure/azure-monitor/reference/tables/kubemonagentevents
[log-schema-kubenodeinventory]: /azure/azure-monitor/reference/tables/kubenodeinventory
[log-schema-kubepodinventory]: /azure/azure-monitor/reference/tables/kubepodinventory
[log-schema-kubeservices]: /azure/azure-monitor/reference/tables/kubeservices
[log-schema-perf]: /azure/azure-monitor/reference/tables/perf

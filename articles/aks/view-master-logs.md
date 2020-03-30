---
title: Visualización de los registros de controlador de Azure Kubernetes Service (AKS)
description: Aprenda a habilitar y ver los registros del nodo maestro de Kubernetes en Azure Kubernetes Service (AKS).
services: container-service
ms.topic: article
ms.date: 01/03/2019
ms.openlocfilehash: f759f15cf98546cb95ba0adb5890885f85ca6aa1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "79229596"
---
# <a name="enable-and-review-kubernetes-master-node-logs-in-azure-kubernetes-service-aks"></a>Habilitación y revisión de los registros del nodo maestro de Kubernetes en Azure Kubernetes Service (AKS)

Con Azure Kubernetes Service (AKS), los componentes principales, como *kube-apiserver* y *kube-controller-manager* se proporcionan como un servicio administrado. Cree y administre los nodos que ejecutan *kubelet* y el entorno de tiempo de ejecución del contenedor, e implemente las aplicaciones mediante el servidor de API de Kubernetes administrado. Para ayudar a solucionar los problemas de sus aplicaciones y servicios, quizás deba ver los registros generados por estos componentes principales. En este artículo se muestra cómo usar registros de Azure Monitor para habilitar y consultar los registros de los componentes principales de Kubernetes.

## <a name="before-you-begin"></a>Antes de empezar

En este artículo se requiere un clúster de AKS existente que se ejecute en su cuenta de Azure. Si todavía no tiene un clúster de AKS, cree uno con la [CLI de Azure][cli-quickstart] o [Azure Portal][portal-quickstart]. Registros de Azure Monitor funciona tanto con clústeres de AKS habilitados para RBAC como no habilitados para RBAC.

## <a name="enable-diagnostics-logs"></a>Habilitación del registro de diagnósticos

Para ayudar a recopilar y revisar los datos de varios orígenes, registros de Azure Monitor proporciona un motor de lenguaje y análisis de consultas que ofrece información detallada sobre su entorno. Un área de trabajo se usa para intercalar y analizar los datos, y se puede integrar con otros servicios de Azure como Application Insights y Security Center. Para usar una plataforma diferente para analizar los registros, puede decidir en su lugar enviar los registros de diagnóstico a una cuenta de almacenamiento o un centro de eventos de Azure. Para más información, consulte el artículo sobre los [registros de Azure Monitor][log-analytics-overview].

Los registros de Azure Monitor se habilitan y administran en Azure Portal. Para habilitar la recopilación de registros para los componentes principales de Kubernetes en el clúster de AKS, abra Azure Portal en un explorador web y realice los pasos siguientes:

1. Seleccione el grupo de recursos del clúster de AKS, como *myResourceGroup*. No seleccione el grupo de recursos que contiene los recursos del clúster de AKS individuales, como *MC_myResourceGroup_myAKSCluster_eastus*.
1. En la parte izquierda, elija **Configuración de diagnóstico**.
1. Seleccione el clúster de AKS, como *myAKSCluster*, y elija **Agregar configuración de diagnóstico**.
1. Escriba un nombre, como *myAKSClusterLogs* y seleccione la opción **Send to Log Analytics** (Enviar a Log Analytics).
1. Seleccione un área de trabajo existente o cree uno. Al crear un área de trabajo, proporciónele un nombre, un grupo de recursos y una ubicación.
1. En la lista de registros disponibles, seleccione los que desea habilitar. Los registros típicos incluyen *kube-apiserver*, *kube-controller-manager* y *kube-scheduler*. Puede habilitar otros, como *kube-audit* y *cluster-autoscaler*. Puede volver y cambiar los registros recopilados una vez que las áreas de trabajo de Log Analytics está habilitadas.
1. Cuando esté listo, seleccione **Guardar** para habilitar la recopilación de los registros seleccionados.

La siguiente captura de pantalla de ejemplo del portal muestra la ventana *Configuración de diagnóstico*:

![Habilitación de un área de trabajo de Log Analytics para registros de Azure Monitor del clúster de AKS](media/view-master-logs/enable-oms-log-analytics.png)

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

Los registros de diagnóstico pueden tardar unos minutos en habilitarse y aparecer en el área de trabajo de Log Analytics. En Azure Portal, seleccione el grupo de recursos para el área de trabajo de Log Analytics, como *myResourceGroup*, y elija el recurso de Log Analytics, por ejemplo, *myAKSLogs*.

![Selección del área de trabajo de Log Analytics para el clúster de AKS](media/view-master-logs/select-log-analytics-workspace.png)

En el lado izquierdo, elija **Registros**. Para ver *kube-apiserver*, escriba la siguiente consulta en el cuadro de texto:

```
AzureDiagnostics
| where Category == "kube-apiserver"
| project log_s
```

Es probable que se devuelvan muchos registros para el servidor de API. Para reducir el ámbito de la consulta y ver los registros sobre el pod de NGINX creado en el paso anterior, agregue una instrucción *where* adicional para buscar *pods/nginx*, como se muestra en la consulta de ejemplo siguiente:

```
AzureDiagnostics
| where Category == "kube-apiserver"
| where log_s contains "pods/nginx"
| project log_s
```

Se muestran los registros específicos de su pod NGINX, como se ilustra en la siguiente captura de pantalla de ejemplo:

![Resultados de consulta de Log Analytics del pod NGINX de ejemplo](media/view-master-logs/log-analytics-query-results.png)

Para ver registros adicionales, puede actualizar la consulta del nombre *Categoría* a *kube-controller-manager* o *kube-programador*, dependiendo de qué registros adicionales habilite. Luego, se pueden usar instrucciones adicionales *where* para refinar los eventos que busca.

Para más información acerca de cómo consultar y filtrar datos de registro, consulte el artículo de [Visualización o análisis de los datos recopilados con la búsqueda de registros de Log Analytics][analyze-log-analytics].

## <a name="log-event-schema"></a>Esquema de evento de registro

Para ayudar a analizar los datos de registro, en la tabla siguiente se describe el esquema usado para cada evento:

| Nombre del campo               | Descripción |
|--------------------------|-------------|
| *resourceId*             | Recursos de Azure que generó el registro |
| *time*                   | Marca de tiempo de cuando se cargó el registro |
| *category*               | Nombre del contenedor o componente que genera el registro |
| *operationName*          | Siempre *Microsoft.ContainerService/managedClusters/diagnosticLogs/Read* |
| *properties.log*         | Texto completo del registro desde el componente |
| *properties.stream*      | *stderr* o *stdout* |
| *properties.pod*         | Nombre del pod del que procede el registro |
| *properties.containerID* | Identificador del contenedor de Docker del que procede este registro |

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
[log-analytics-overview]: ../log-analytics/log-analytics-overview.md
[analyze-log-analytics]: ../azure-monitor/learn/tutorial-viewdata.md
[kubelet-logs]: kubelet-logs.md
[aks-ssh]: ssh.md
[az-feature-register]: /cli/azure/feature#az-feature-register
[az-feature-list]: /cli/azure/feature#az-feature-list
[az-provider-register]: /cli/azure/provider#az-provider-register

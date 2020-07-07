---
title: Uso de GitOps con Helm para una configuración de clúster habilitada para Azure Arc (versión preliminar)
services: azure-arc
ms.service: azure-arc
ms.date: 05/19/2020
ms.topic: article
author: mlearned
ms.author: mlearned
description: Uso de GitOps con Helm para una configuración de clúster habilitada para Azure Arc (versión preliminar)
keywords: GitOps, Kubernetes, K8s, Azure, Helm, Arc, AKS, Azure Kubernetes Service, contenedores
ms.openlocfilehash: 677c5f2b27794ebea9d38e470b5e1a5ba12bff7e
ms.sourcegitcommit: 9b5c20fb5e904684dc6dd9059d62429b52cb39bc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85857231"
---
# <a name="use-gitops-with-helm-for-an-azure-arc-enabled-cluster-configuration-preview"></a>Uso de GitOps con Helm para una configuración de clúster habilitada para Azure Arc (versión preliminar)

Helm es una herramienta de empaquetado de código abierto que facilita la instalación y administración del ciclo de vida de las aplicaciones de Kubernetes. Helm, que funciona de forma similar a los administradores de paquetes de Linux como APT y Yum, se usa para administrar los gráficos de Kubernetes, que son paquetes de recursos de Kubernetes preconfigurados.

En este artículo se muestra cómo configurar y usar Helm con Kubernetes habilitado para Azure Arc.

## <a name="before-you-begin"></a>Antes de empezar

En este artículo se supone que ya tiene un clúster conectado de Kubernetes habilitado para Azure Arc. Si necesita un clúster conectado, vea el [inicio rápido sobre conexión de un clúster](./connect-cluster.md).

En primer lugar se establecerán las variables de entorno que se usarán en este tutorial. Necesitará el nombre del grupo de recursos y el del clúster para el clúster conectado.

```bash
export RESOURCE_GROUP=<Resource_Group_Name>
export CLUSTER_NAME=<ClusterName>
```

## <a name="verify-your-cluster-is-enabled-with-arc"></a>Comprobación de que el clúster está habilitado con Arc

```bash
az connectedk8s list -g $RESOURCE_GROUP -o table
```

Salida:
```bash
Name           Location    ResourceGroup
-------------  ----------  ---------------
arc-helm-demo  eastus      k8s-clusters
```

## <a name="overview-of-using-helm-with-azure-arc-enabled-kubernetes"></a>Información general sobre el uso de Helm con Kubernetes habilitado para Azure Arc

 El operador Helm proporciona una extensión para Flux que automatiza las versiones Chart de Helm. Una versión Chart se describe a través de un recurso personalizado de Kubernetes denominado HelmRelease. Flux sincroniza estos recursos de Git con el clúster, y el operador de Helm se asegura de que los gráficos de Helm se publiquen como se especifica en los recursos.

 En el ejemplo siguiente se muestra la estructura de repositorios de Git que se usará en este tutorial:

```bash
├── charts
│   └── azure-vote
│       ├── Chart.yaml
│       ├── templates
│       │   ├── NOTES.txt
│       │   ├── deployment.yaml
│       │   └── service.yaml
│       └── values.yaml
└── releases
    └── prod
        └── azure-vote-app.yaml
```

En el repositorio de Git hay dos directorios, uno que contiene un gráfico de Helm y con la configuración de publicaciones. En el directorio `releases/prod`, `azure-vote-app.yaml` contiene la configuración de HelmRelease siguiente:

```bash
 apiVersion: helm.fluxcd.io/v1
 kind: HelmRelease
 metadata:
   name: azure-vote-app
   namespace: prod
   annotations:
 spec:
   releaseName: azure-vote-app
   chart:
     git: https://github.com/Azure/arc-helm-demo
     path: charts/azure-vote
     ref: master
   values:
     image:
       repository: dstrebel/azurevote
       tag: v1
     replicaCount: 3
```

La configuración de versión de Helm contiene los campos siguientes:

- `metadata.name` es obligatorio y debe seguir las convenciones de nomenclatura de Kubernetes
- `metadata.namespace` es opcional y determina dónde se crea la versión
- `spec.releaseName` es opcional y, si no se proporciona, el nombre de la versión será $espacio_de_nombres-$nombre
- `spec.chart.path` es el directorio que contiene el gráfico, en relación con la raíz del repositorio.
- `spec.values` son personalizaciones de usuario de los valores de parámetro predeterminados del propio gráfico

Las opciones especificadas en spec.values de HelmRelease reemplazarán las opciones especificadas en values.yaml del origen del gráfico.

Puede obtener más información sobre HelmRelease en la [documentación oficial del operador Helm](https://docs.fluxcd.io/projects/helm-operator/en/1.0.0-rc9/references/helmrelease-custom-resource.html).

## <a name="create-a-configuration"></a>Creación de una configuración

Con la extensión de la CLI de Azure para `k8sconfiguration`, se vinculará el clúster conectado al repositorio de Git de ejemplo. A esta configuración se le asigna el nombre `azure-voting-app` y el operador de Flux se implementa en el espacio de nombres `prod`.

```bash
az k8sconfiguration create --name azure-voting-app --resource-group  $RESOURCE_GROUP --cluster-name $CLUSTER_NAME --operator-instance-name azure-voting-app --operator-namespace prod --enable-helm-operator --helm-operator-version='0.6.0' --helm-operator-params='--set helm.versions=v3' --repository-url https://github.com/Azure/arc-helm-demo.git --operator-params='--git-readonly --git-path=releases/prod' --scope namespace --cluster-type connectedClusters
```

### <a name="configuration-parameters"></a>Parámetros de configuración

Para personalizar la creación de la configuración, [obtenga información sobre los parámetros adicionales que puede usar](./use-gitops-connected-cluster.md#additional-parameters).


## <a name="validate-the-configuration"></a>Validación de la configuración

Con la CLI de Azure, compruebe que `sourceControlConfiguration` se ha creado correctamente.

```console
az k8sconfiguration show --resource-group $RESOURCE_GROUP --name azure-voting-app --cluster-name $CLUSTER_NAME --cluster-type connectedClusters
```

Tenga en cuenta que el recurso `sourceControlConfiguration` se actualiza con el estado de cumplimiento, los mensajes y la información de depuración.

**Salida:**

```console
Command group 'k8sconfiguration' is in preview. It may be changed/removed in a future release.
{
  "complianceStatus": {
    "complianceState": "Installed",
    "lastConfigApplied": "2019-12-05T05:34:41.481000",
    "message": "...",
    "messageLevel": "3"
  },
  "id": "/subscriptions/57ac26cf-a9f0-4908-b300-9a4e9a0fb205/resourceGroups/AzureArcTest/providers/Microsoft.Kubernetes/connectedClusters/AzureArcTest1/providers/Microsoft.KubernetesConfiguration/sourceControlConfigurations/cluster-config",
  "name": "azure-vote-app",
  "operatorInstanceName": "cluster-config",
  "operatorNamespace": "prod",
  "operatorParams": "--git-readonly --git-path=releases/prod",
  "operatorScope": "namespace",
  "operatorType": "Flux",
  "provisioningState": "Succeeded",
  "repositoryPublicKey": "...",
  "repositoryUrl": "git://github.com/Azure/arc-helm-demo.git",
  "resourceGroup": "AzureArcTest",
  "type": "Microsoft.KubernetesConfiguration/sourceControlConfigurations"
}
```

## <a name="validate-application"></a>Validación de la aplicación

Ahora se comprobará que la aplicación está activa y en ejecución mediante la obtención de la dirección IP del servicio.

```bash
kubectl get svc/azure-vote-front -n prod
```

**Salida:**

```bash
NAME               TYPE           CLUSTER-IP    EXTERNAL-IP      PORT(S)        AGE
azure-vote-front   LoadBalancer   10.0.14.161   52.186.160.216   80:30372/TCP   4d22h
```

Busque la dirección IP externa de la salida anterior y ábrala en un explorador.

## <a name="next-steps"></a>Pasos siguientes

- [Uso de Azure Policy para controlar la configuración del clúster](./use-azure-policy.md)

---
title: Conexión de un clúster de Kubernetes habilitado para Azure Arc (versión preliminar)
services: azure-arc
ms.service: azure-arc
ms.date: 05/19/2020
ms.topic: article
author: mlearned
ms.author: mlearned
description: Conexión de un clúster de Kubernetes habilitado para Azure Arc
keywords: Kubernetes, Arc, Azure, K8s, contenedores
ms.custom: references_regions
ms.openlocfilehash: 868964361e6089eb3417b0f2e2681d82d4aa0b75
ms.sourcegitcommit: d118ad4fb2b66c759b70d4d8a18e6368760da3ad
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/02/2020
ms.locfileid: "84299650"
---
# <a name="connect-an-azure-arc-enabled-kubernetes-cluster-preview"></a>Conexión de un clúster de Kubernetes habilitado para Azure Arc (versión preliminar)

Conecte un clúster de Kubernetes a Azure Arc.

## <a name="before-you-begin"></a>Antes de empezar

Compruebe que tiene listos los requisitos siguientes:

* Un clúster de Kubernetes en funcionamiento
* Necesitará acceso con kubeconfig y acceso de administrador de clústeres.
* El usuario o la entidad de servicio que se usa con los comandos `az login` y `az connectedk8s connect` debe tener los permisos de "Lectura" y "Escritura" en el tipo de recurso "Microsoft.Kubernetes/connectedclusters". El rol "Azure Arc para la incorporación de Kubernetes" que tiene estos permisos se puede usar para las asignaciones de roles en el usuario o la entidad de servicio que se utiliza con la CLI de Azure para la incorporación.
* La versión más reciente de las extensiones *connectedk8s* y *k8sconfiguration*.

## <a name="supported-regions"></a>Regiones admitidas

* Este de EE. UU.
* Oeste de Europa

## <a name="network-requirements"></a>Requisitos de red

Los agentes de Azure Arc requieren que funcionen los siguientes protocolos, puertos y direcciones URL de salida.

* TCP en el puerto 443--> `https://:443`
* TCP en el puerto 9418--> `git://:9418`

| Punto de conexión (DNS)                                                                                               | Descripción                                                                                                                 |
| ------------------------------------------------------------------------------------------------------------ | --------------------------------------------------------------------------------------------------------------------------- |
| `https://management.azure.com`                                                                                 | Necesario para que el agente se conecte a Azure y registre el clúster                                                        |
| `https://eastus.dp.kubernetesconfiguration.azure.com`, `https://westeurope.dp.kubernetesconfiguration.azure.com` | Punto de conexión de plano de datos para que el agente inserte información de la configuración de estado y recuperación de cambios                                      |
| `https://docker.io`                                                                                            | Necesario para extraer imágenes de contenedor                                                                                         |
| `https://github.com`, git://github.com                                                                         | Los repositorios de GitOps de ejemplo se hospedan en GitHub. El agente de configuración requiere conectividad con el punto de conexión de Git que se especifique. |
| `https://login.microsoftonline.com`                                                                            | Necesario para capturar y actualizar tokens de Azure Resource Manager                                                                                    |
| `https://azurearcfork8s.azurecr.io`                                                                            | Necesario para extraer imágenes de contenedor para agentes de Azure Arc                                                                  |

## <a name="register-the-two-providers-for-azure-arc-enabled-kubernetes"></a>Registre los dos proveedores para Kubernetes habilitado para Azure Arc:

```console
az provider register --namespace Microsoft.Kubernetes
Registering is still on-going. You can monitor using 'az provider show -n Microsoft.Kubernetes'

az provider register --namespace Microsoft.KubernetesConfiguration
Registering is still on-going. You can monitor using 'az provider show -n Microsoft.KubernetesConfiguration'
```

El registro es un proceso asincrónico. Puede tardar 10 minutos, aproximadamente. Puede supervisar el proceso de registro con los comandos siguientes:

```console
az provider show -n Microsoft.Kubernetes -o table
```

```console
az provider show -n Microsoft.KubernetesConfiguration -o table
```

## <a name="install-azure-cli-and-arc-enabled-kubernetes-extensions"></a>Instalación de las extensiones de Kubernetes habilitado para Arc y de la CLI de Azure
Se requiere la versión 2.3 o posteriores de la CLI de Azure para instalar las extensiones de la CLI de Kubernetes habilitado para Azure Arc. [Instale la CLI de Azure](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) o actualice a la última versión para asegurarse de que tiene la versión 2.3 o posterior de la CLI de Azure.

Instale la extensión `connectedk8s`, que ayuda a conectar clústeres de Kubernetes a Azure:

```console
az extension add --name connectedk8s
```

Instale la extensión `k8sconfiguration`:

```console
az extension add --name k8sconfiguration
```

Para actualizar las extensiones a las versiones más recientes, ejecute los comandos siguientes.

```console
az extension update --name connectedk8s
az extension update --name k8sconfiguration
```

## <a name="install-helm"></a>Instalación de Helm
Helm 3 es necesario para incorporar el clúster con la extensión connectedk8s. [Instale la última versión de Helm 3](https://helm.sh/docs/intro/install) para cumplir este requisito.

## <a name="create-a-resource-group"></a>Creación de un grupo de recursos

Use un grupo de recursos para almacenar los metadatos del clúster.

En primer lugar, cree un grupo de recursos para almacenar el recurso de clúster conectado.

```console
az group create --name AzureArcTest -l EastUS -o table
```

**Salida:**

```console
Location    Name
----------  ------------
eastus      AzureArcTest
```

## <a name="connect-a-cluster"></a>Conexión de un clúster

A continuación, se conectará el clúster de Kubernetes a Azure. El flujo de trabajo para `az connectedk8s connect` es el siguiente:

1. Compruebe la conectividad con el clúster de Kubernetes: a través de `KUBECONFIG`, `~/.kube/config` o `--kube-config`.
1. Implemente agentes de Azure Arc para Kubernetes mediante Helm 3, en el espacio de nombres `azure-arc`.

```console
az connectedk8s connect --name AzureArcTest1 --resource-group AzureArcTest
```

**Salida:**

```console
Command group 'connectedk8s' is in preview. It may be changed/removed in a future release.
Helm release deployment succeeded

{
  "aadProfile": {
    "clientAppId": "",
    "serverAppId": "",
    "tenantId": ""
  },
  "agentPublicKeyCertificate": "...",
  "agentVersion": "0.1.0",
  "id": "/subscriptions/57ac26cf-a9f0-4908-b300-9a4e9a0fb205/resourceGroups/AzureArcTest/providers/Microsoft.Kubernetes/connectedClusters/AzureArcTest1",
  "identity": {
    "principalId": null,
    "tenantId": null,
    "type": "None"
  },
  "kubernetesVersion": "v1.15.0",
  "location": "eastus",
  "name": "AzureArcTest1",
  "resourceGroup": "AzureArcTest",
  "tags": {},
  "totalNodeCount": 1,
  "type": "Microsoft.Kubernetes/connectedClusters"
}
```

## <a name="verify-connected-cluster"></a>Comprobación del clúster conectado

Enumere los clústeres conectados:

```console
az connectedk8s list -g AzureArcTest -o table
```

**Salida:**

```console
Command group 'connectedk8s' is in preview. It may be changed/removed in a future release.
Name           Location    ResourceGroup
-------------  ----------  ---------------
AzureArcTest1  eastus      AzureArcTest
```

También puede ver este recurso en el [Portal de vista previa de Azure](https://preview.portal.azure.com/). Una vez que tenga el portal abierto en el explorador, navegue hasta el grupo de recursos y el recurso Kubernetes habilitado para Azure Arc según las entradas de nombre de recurso y nombre del grupo de recursos que se usaron anteriormente en el comando `az connectedk8s connect`.

Kubernetes habilitado para Azure Arc implementa algunos operadores en el espacio de nombres `azure-arc`. Puede ver estas implementaciones y pods aquí:

```console
kubectl -n azure-arc get deployments,pods
```

**Salida:**

```console
NAME                                        READY   UP-TO-DATE AVAILABLE AGE
deployment.apps/cluster-metadata-operator   1/1     1           1        16h
deployment.apps/clusteridentityoperator     1/1     1           1        16h
deployment.apps/config-agent                1/1     1           1        16h
deployment.apps/controller-manager          1/1     1           1        16h
deployment.apps/flux-logs-agent             1/1     1           1        16h
deployment.apps/metrics-agent               1/1     1           1        16h
deployment.apps/resource-sync-agent         1/1     1           1        16h

NAME                                            READY   STATUS   RESTART AGE
pod/cluster-metadata-operator-7fb54d9986-g785b  2/2     Running  0       16h
pod/clusteridentityoperator-6d6678ffd4-tx8hr    3/3     Running  0       16h
pod/config-agent-544c4669f9-4th92               3/3     Running  0       16h
pod/controller-manager-fddf5c766-ftd96          3/3     Running  0       16h
pod/flux-logs-agent-7c489f57f4-mwqqv            2/2     Running  0       16h
pod/metrics-agent-58b765c8db-n5l7k              2/2     Running  0       16h
pod/resource-sync-agent-5cf85976c7-522p5        3/3     Running  0       16h
```

## <a name="azure-arc-agents-for-kubernetes"></a>Agentes de Azure Arc para Kubernetes

Kubernetes habilitado para Azure Arc consta de algunos agentes (operadores) que se ejecutan en el clúster implementado en el espacio de nombres `azure-arc`.

* `deployment.apps/config-agent`: busca en el clúster conectado los recursos de configuración de control de código fuente aplicados en el clúster y actualiza el estado de cumplimiento.
* `deployment.apps/controller-manager`: es un operador de operadores y organiza las interacciones entre los componentes de Azure Arc.
* `deployment.apps/metrics-agent`: recopila métricas de otros agentes de Arc para asegurarse de que presentan un rendimiento óptimo.
* `deployment.apps/cluster-metadata-operator`: recopila metadatos del clúster: la versión, el número de nodos y la versión del agente de Arc.
* `deployment.apps/resource-sync-agent`: sincroniza con Azure los metadatos de clúster mencionados anteriormente.
* `deployment.apps/clusteridentityoperator`: mantiene el certificado de Managed Service Identity (MSI) que usan otros agentes para la comunicación con Azure.
* `deployment.apps/flux-logs-agent`: recopila registros de los operadores de Flux implementados como parte de la configuración de control de código fuente.

## <a name="delete-a-connected-cluster"></a>Eliminación de un clúster conectado

Puede eliminar un recurso `Microsoft.Kubernetes/connectedcluster` mediante la CLI de Azure o Azure Portal.


* **Eliminación con la CLI de Azure**: el siguiente comando de la CLI de Azure se puede usar para iniciar la eliminación del recurso Kubernetes habilitado para Azure Arc.
  ```console
  az connectedk8s delete --name AzureArcTest1 --resource-group AzureArcTest
  ```
  Esto quita el recurso `Microsoft.Kubernetes/connectedCluster` y los recursos `sourcecontrolconfiguration` asociados en Azure. En la CLI de Azure se usa helm uninstall para quitar también los agentes que se ejecutan en el clúster.

* **Eliminación en Azure Portal**: con la eliminación del recurso Kubernetes habilitado para Azure Arc desde Azure Portal, se quitan el recurso `Microsoft.Kubernetes/connectedcluster` y los recursos `sourcecontrolconfiguration` asociados en Azure, pero no se eliminan los agentes que se ejecutan en el clúster. Para eliminar los agentes que se ejecutan en el clúster, ejecute el siguiente comando.

  ```console
  az connectedk8s delete --name AzureArcTest1 --resource-group AzureArcTest
  ```

## <a name="next-steps"></a>Pasos siguientes

* [Uso de GitOps en un clúster conectado](./use-gitops-connected-cluster.md)
* [Uso de Azure Policy para controlar la configuración del clúster](./use-azure-policy.md)
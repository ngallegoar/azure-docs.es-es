---
title: Configuración de la versión 4.x de Red Hat OpenShift en Azure con Azure Monitor para contenedores | Microsoft Docs
description: En este artículo se describe cómo configurar la supervisión de un clúster de Kubernetes con Azure Monitor hospedado en la versión 4 o posterior de Red Hat OpenShift en Azure.
ms.topic: conceptual
ms.date: 06/30/2020
ms.openlocfilehash: 49097d96ecf58d7c5bf7d1a60ff01fc7182587c6
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85801485"
---
# <a name="configure-azure-red-hat-openshift-v4x-with-azure-monitor-for-containers"></a>Configuración de la versión 4.x de Red Hat OpenShift en Azure con Azure Monitor para contenedores

Azure Monitor para contenedores proporciona una experiencia de supervisión enriquecida para los clústeres de Azure Kubernetes Service (AKS) y del Motor de AKS. En este artículo se describe cómo lograr una experiencia de supervisión similar mediante la habilitación de la supervisión de los clústeres de Kubernetes hospedados en la versión 4.x de [Red Hat OpenShift en Azure](../../openshift/intro-openshift.md).

>[!NOTE]
>La compatibilidad con Red Hat OpenShift en Azure es una característica que se encuentra en versión preliminar pública en este momento.
>

Azure Monitor para contenedores se puede habilitar para una o más implementaciones existentes de la versión 4.x de Red Hat OpenShift en Azure con los métodos admitidos que se describen en este artículo.

Para un clúster existente, ejecute este [script de Bash en la CLI de Azure](https://docs.microsoft.com/cli/azure/openshift?view=azure-cli-latest#az-openshift-create).

## <a name="supported-and-unsupported-features"></a>Características admitidas y no admitidas

Azure Monitor para contenedores admite la supervisión de la versión 4.x de Red Hat OpenShift en Azure, tal y como se describe en [Introducción a Azure Monitor para contenedores](container-insights-overview.md), excepto para las siguientes características:

- Datos en directo (versión preliminar)
- [Recopilación de métricas](container-insights-update-metrics.md) de nodos y pods del clúster y su almacenamiento en la base de datos de métricas de Azure Monitor

## <a name="prerequisites"></a>Requisitos previos

- La CLI de Azure, versión 2.0.72 o posterior  

- La herramienta de la CLI [Helm 3](https://helm.sh/docs/intro/install/)

- [Bash versión 4](https://www.gnu.org/software/bash/)

- La herramienta de la línea de comandos [Kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl/)

- Un [área de trabajo de Log Analytics.](../platform/design-logs-deployment.md)

    Azure Monitor para contenedores admite un área de trabajo de Log Analytics en las regiones enumeradas en los [productos por región](https://azure.microsoft.com/global-infrastructure/services/?regions=all&products=monitor) de Azure. Para crear el área de trabajo, puede configurarla mediante [Azure Resource Manager](../platform/template-workspace-configuration.md), [PowerShell](../scripts/powershell-sample-create-workspace.md?toc=%2fpowershell%2fmodule%2ftoc.json) o en el [Azure Portal](../learn/quick-create-workspace.md).

- Para habilitar y acceder a las características de Azure Monitor para contenedores debe ser miembro, como mínimo, del rol *Colaborador* de la suscripción de Azure y miembro del rol [*Colaborador de Log Analytics*](../platform/manage-access.md#manage-access-using-azure-permissions) del área de trabajo de Log Analytics configurada con Azure Monitor para contenedores.

- Para ver los datos de supervisión, debe tener el rol [*Lector de Log Analytics*](../platform/manage-access.md#manage-access-using-azure-permissions) en el área de trabajo de Log Analytics configurada con Azure Monitor para contenedores.

## <a name="enable-monitoring-for-an-existing-cluster"></a>Habilitación de la supervisión para un clúster existente

Realice los siguientes pasos para habilitar la supervisión de un clúster de la versión 4 o posterior de Red Hat OpenShift en Azure implementado en Azure mediante el script de Bash proporcionado:

1. Inicie sesión en Azure ejecutando el siguiente comando:

    ```azurecli
    az login
    ```

1. Descargue y guarde en una carpeta local el script que configura el clúster con el complemento de supervisión mediante la ejecución de los siguientes comandos:

    `curl -o enable-monitoring.sh -L https://aka.ms/enable-monitoring-bash-script`

1. Para identificar el valor de *kubeContext* del clúster, ejecute los siguientes comandos:

    ```
    adminUserName=$(az aro list-credentials -g $clusterResourceGroup -n $clusterName --query 'kubeadminUsername' -o tsv)
    adminPassword=$(az aro list-credentials -g $clusterResourceGroup -n $clusterName --query 'kubeadminPassword' -o tsv)
    apiServer=$(az aro show -g $clusterResourceGroup -n $clusterName --query apiserverProfile.url -o tsv)
    oc login $apiServer -u $adminUserName -p $adminPassword
    # openshift project name for azure monitor for containers
    openshiftProjectName="azure-monitor-for-containers"
    oc new-project $openshiftProjectName
    # get the kube config context
    kubeContext=$(oc config current-context)
    ```

1. Copie el valor para su uso posterior.

### <a name="integrate-with-an-existing-workspace"></a>Integración en un área de trabajo existente

En esta sección, se habilita la supervisión del clúster mediante el script de Bash que descargó anteriormente. Para la integración en un área de trabajo de Log Analytics existente, comience por identificar el identificador de recurso completo del área de trabajo de Log Analytics necesario para el parámetro `logAnalyticsWorkspaceResourceId` y, a continuación, ejecute el comando para habilitar el complemento de supervisión en el área de trabajo especificada.

Si no dispone de un área de trabajo para especificar, puede ir a la sección [Integración con el área de trabajo predeterminada](#integrate-with-the-default-workspace) y dejar que el script cree una nueva área de trabajo.

1. Enumere todas las suscripciones a las que tiene acceso mediante la ejecución del siguiente comando:

    ```azurecli
    az account list --all -o table
    ```

    El resultado tendrá un aspecto similar al siguiente:

    ```azurecli
    Name                                  CloudName    SubscriptionId                        State    IsDefault
    ------------------------------------  -----------  ------------------------------------  -------  -----------
    Microsoft Azure                       AzureCloud   0fb60ef2-03cc-4290-b595-e71108e8f4ce  Enabled  True
    ```

1. Copie el valor de **SubscriptionId**.

1. Cambie a la suscripción que hospeda el área de trabajo de Log Analytics mediante la ejecución del siguiente comando:

    ```azurecli
    az account set -s <subscriptionId of the workspace>
    ```

1. Muestre la lista de áreas de trabajo de sus suscripciones en el formato JSON predeterminado mediante la ejecución del siguiente comando:

    ```
    az resource list --resource-type Microsoft.OperationalInsights/workspaces -o json
    ```

1. En la salida, busque el nombre del área de trabajo y copie el identificador de recurso completo de esa área de trabajo de Log Analytics en el campo **identificador**.

1. Ejecute el comando siguiente para habilitar la supervisión. Reemplace los valores de los parámetros `azureAroV4ClusterResourceId`, `logAnalyticsWorkspaceResourceId` y `kubeContext`.

    ```bash
    export azureAroV4ClusterResourceId=“/subscriptions/<subscriptionId>/resourceGroups/<resourceGroupName>/providers/Microsoft.RedHatOpenShift/OpenShiftClusters/<clusterName>”
    export logAnalyticsWorkspaceResourceId=“/subscriptions/<subscriptionId>/resourceGroups/<resourceGroupName>/providers/microsoft.operationalinsights/workspaces/<workspaceName>”
    export kubeContext="<kubeContext name of your ARO v4 cluster>"  
    ```

    Ejemplo:

    `bash enable-monitoring.sh --resource-id $azureAroV4ClusterResourceId --kube-context $kubeContext --workspace-id $logAnalyticsWorkspaceResourceId`

Después de habilitar la supervisión, pueden pasar unos 15 minutos hasta que pueda ver las métricas de estado del clúster.

### <a name="integrate-with-the-default-workspace"></a>Integración con el área de trabajo predeterminada

En esta sección se habilita la supervisión del clúster de la versión 4.x de Red Hat OpenShift en Azure mediante el script de Bash que descargó.

En este ejemplo, no es necesario crear previamente ni especificar un área de trabajo existente. Este comando le simplifica el proceso al crear un área de trabajo predeterminada en el grupo de recursos predeterminado de la suscripción del clúster, si aún no existe en la región.

El área de trabajo predeterminada que se crea tiene el formato *DefaultWorkspace-\<GUID>-\<Region>* .  

Reemplace los valores de los parámetros `azureAroV4ClusterResourceId` y `kubeContext`.

```bash
export azureAroV4ClusterResourceId=“/subscriptions/<subscriptionId>/resourceGroups/<resourceGroupName>/providers/Microsoft.RedHatOpenShift/OpenShiftClusters/<clusterName>”
export kubeContext="<kubeContext name of your ARO v4 cluster>"
```

Por ejemplo:

`bash enable-monitoring.sh --resource-id $azureAroV4ClusterResourceId --kube-context $kubeContext`

Después de habilitar la supervisión, pueden pasar unos 15 minutos hasta que pueda ver la métrica de estado del clúster.

### <a name="enable-monitoring-from-the-azure-portal"></a>Habilitación de la supervisión desde Azure Portal

La vista de varios clústeres de Azure Monitor para contenedores resalta los clústeres de Red Hat OpenShift en Azure que no tienen la supervisión habilitada en la pestaña **Clústeres no supervisados**. La opción **Habilitar** situada junto al clúster no inicia la incorporación de la supervisión desde el portal. Se le redirigirá a este artículo para habilitar manualmente la supervisión según los pasos descritos anteriormente en este artículo.

1. Inicie sesión en [Azure Portal](https://portal.azure.com).

1. En el panel izquierdo o desde la página principal, seleccione **Azure Monitor**.

1. En la sección **Información detallada** , seleccione **Contenedores**.

1. En la página **Supervisión - Contenedores**, seleccione **Clústeres sin supervisar**.

1. En la lista de clústeres no supervisados, seleccione el clúster y, a continuación, seleccione **Habilitar**.

    Puede identificar los resultados en la lista buscando el valor **ARO** en la columna **Tipo de clúster**. Después de seleccionar **Habilitar**, se le redirigirá a este artículo.

## <a name="next-steps"></a>Pasos siguientes

- Ahora que ha habilitado la supervisión para recopilar el estado y la utilización de recursos del clúster de la versión 4.x de Red Hat OpenShift y las cargas de trabajo que se ejecutan en ellos, obtenga información sobre [cómo usar](container-insights-analyze.md) Azure Monitor para contenedores.

- De forma predeterminada, el agente en contenedores recopila los registros de contenedor *stdout* y *stderr* de todos los contenedores que se ejecutan en todos los espacios de nombres excepto kube-system. Para configurar la recopilación de registros de contenedor específica de uno o varios espacios de nombres determinados, consulte [Configuración del agente de Container Insights](container-insights-agent-config.md) para configurar las opciones de recopilación de datos que desee en el archivo de configuraciones *ConfigMap*.

- Para extraer y analizar las métricas de Prometheus desde el clúster, consulte [Configuración de la extracción de métricas de Prometheus](container-insights-prometheus-integration.md).

- Para aprender a detener la supervisión del clúster con Azure Monitor para contenedores, consulte [Cómo detener la supervisión del clúster de Red Hat OpenShift en Azure](container-insights-optout-openshift.md).

---
title: Configuración de la versión 4.x de Red Hat OpenShift en Azure con Azure Monitor para contenedores | Microsoft Docs
description: En este artículo se describe cómo configurar la supervisión de un clúster de Kubernetes con Azure Monitor hospedado en la versión 4 y posteriores de Red Hat OpenShift en Azure.
ms.topic: conceptual
ms.date: 04/22/2020
ms.openlocfilehash: 4b827524845874dabaabe535163d99c408f77a60
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/28/2020
ms.locfileid: "82195751"
---
# <a name="configure-azure-red-hat-openshift-v4x-with-azure-monitor-for-containers"></a>Configuración de la versión 4.x de Red Hat OpenShift en Azure con Azure Monitor para contenedores

Azure Monitor para contenedores proporciona una experiencia de supervisión enriquecida para los clústeres de Azure Kubernetes Service (AKS) y Motor de AKS. En este artículo se describe cómo habilitar la supervisión de clústeres de Kubernetes hospedados en la versión 4.x de [Red Hat OpenShift en Azure](../../openshift/intro-openshift.md) para lograr una experiencia de supervisión similar.

>[!NOTE]
>La compatibilidad con Red Hat OpenShift en Azure es una característica que se encuentra en versión preliminar pública en este momento.
>

Azure Monitor para contenedores se puede habilitar para una o más implementaciones existentes de la versión 4.x de Red Hat OpenShift en Azure con los siguientes métodos admitidos:

- Para un clúster existente, mediante el script de Bash proporcionado, que se ejecuta en la [CLI de Azure](https://docs.microsoft.com/cli/azure/openshift?view=azure-cli-latest#az-openshift-create).

## <a name="supported-and-unsupported-features"></a>Características admitidas y no admitidas

Azure Monitor para contenedores admite la supervisión de la versión 4.x de Red Hat OpenShift en Azure, tal y como se describe en el artículo [introducción](container-insights-overview.md), excepto para las siguientes características:

- Datos en directo (versión preliminar)
- [Recopilación de métricas](container-insights-update-metrics.md) de nodos y pods de clúster, y almacenamiento de estos en la base de datos de métricas de Azure Monitor

## <a name="prerequisites"></a>Prerrequisitos

- CLI de Azure, versión 2.0.72 o posterior

- La herramienta de la CLI [Helm 3](https://helm.sh/docs/intro/install/)

- [Bash versión 4](https://www.gnu.org/software/bash/)

- La herramienta de la línea de comandos [Kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl/)

- Para habilitar y acceder a las características de Azure Monitor para contenedores debe ser miembro, como mínimo, del rol *Colaborador* de la suscripción de Azure y miembro del rol [*Colaborador de Log Analytics*](../platform/manage-access.md#manage-access-using-azure-permissions) del área de trabajo de Log Analytics configurada con Azure Monitor para contenedores.

- Para ver los datos de supervisión, debe ser miembro del permiso de rol [*Lector de Log Analytics*](../platform/manage-access.md#manage-access-using-azure-permissions) en el área de trabajo de Log Analytics configurada con Azure Monitor para contenedores.

## <a name="enable-for-an-existing-cluster"></a>Habilitar para un clúster ya existente

Realice los siguientes pasos para habilitar la supervisión de un clúster de la versión 4 y posteriores de Red Hat OpenShift en Azure implementado en Azure mediante el script de Bash proporcionado.

1. Inicio de sesión en Azure

    ```azurecli
    az login
    ```

2. Descargue y guarde el script que configura el clúster en una carpeta local con el complemento de supervisión mediante el uso de los siguientes comandos:

    `curl -LO https://raw.githubusercontent.com/microsoft/OMS-docker/ci_feature/docs/aroV4/onboarding_azuremonitor_for_containers.sh.`

3. Para identificar el **contexto de Kube** del clúster, después de ejecutar el comando `oc login` correctamente en el clúster, ejecute el comando `kubectl config current-context` y copie el valor.

### <a name="integrate-with-an-existing-workspace"></a>Integración en un área de trabajo existente

El siguiente paso habilita la supervisión del clúster mediante el script de Bash que descargó anteriormente. Para la integración en un área de trabajo de Log Analytics existente, realice los pasos siguientes para detectar primero el identificador del recurso completo del área de trabajo de Log Analytics necesario para el parámetro `workspaceResourceId` y ejecute el comando para habilitar el complemento de supervisión en el área de trabajo especificada. Si no dispone de un área de trabajo para especificar, puede ir al paso 5 y dejar que el script cree una nueva área de trabajo.

1. Enumere todas las suscripciones a las que tiene acceso con el siguiente comando:

    ```azurecli
    az account list --all -o table
    ```

    La salida será similar a la siguiente:

    ```azurecli
    Name                                  CloudName    SubscriptionId                        State    IsDefault
    ------------------------------------  -----------  ------------------------------------  -------  -----------
    Microsoft Azure                       AzureCloud   68627f8c-91fO-4905-z48q-b032a81f8vy0  Enabled  True
    ```

    Copie el valor de **SubscriptionId**.

2. Cambie a la suscripción que hospeda el área de trabajo de Log Analytics con el siguiente comando:

    ```azurecli
    az account set -s <subscriptionId of the workspace>
    ```

3. En el ejemplo siguiente se muestra la lista de áreas de trabajo de sus suscripciones en el formato JSON predeterminado.

    ```
    az resource list --resource-type Microsoft.OperationalInsights/workspaces -o json
    ```

    En la salida, busque el nombre del área de trabajo y copie el identificador de recurso completo de esa área de trabajo de Log Analytics en el campo **identificador**.

4. Reemplace el valor del parámetro `workspaceResourceId` para ejecutar el comando siguiente para habilitar la supervisión: 

    `bash onboarding_azuremonitor_for_containers.sh <kube-context> <azureAroV4ResourceId> <LogAnayticsWorkspaceResourceId>`

    Ejemplo:

    `bash onboarding_azuremonitor_for_containers.sh MyK8sTestCluster /subscriptions/57ac26cf-a9f0-4908-b300-9a4e9a0fb205/resourceGroups/test-aro-v4-rg/providers/Microsoft.RedHatOpenShift/OpenShiftClusters/test-aro-v4  /subscriptions/57ac26cf-a9f0-4908-b300-9a4e9a0fb205/resourcegroups/test-la-workspace-rg/providers/microsoft.operationalinsights/workspaces/test-la-workspace`

Después de habilitar la supervisión, pueden pasar unos 15 minutos hasta que pueda ver la métrica de estado del clúster.

### <a name="integrate-with-default-workspace"></a>Integración con el área de trabajo predeterminada

El siguiente paso habilita la supervisión del clúster de la versión 4.x de Red Hat OpenShift en Azure mediante el script de Bash que descargó. En este ejemplo, no es necesario que cree o especifique un área de trabajo. Este comando le simplifica el proceso al crear un área de trabajo predeterminada en el grupo de recursos predeterminado de la suscripción del clúster, si aún no existe en la región. El formato del área de trabajo predeterminada creada es similar al de *DefaultWorkspace-\<GUID>-\<Region>* .  

    `bash onboarding_azuremonitor_for_containers.sh <kube-context> <azureAroV4ResourceId>`

    For example:

    `bash onboarding_azuremonitor_for_containers.sh MyK8sTestCluster /subscriptions/57ac26cf-a9f0-4908-b300-9a4e9a0fb205/resourceGroups/test-aro-v4-rg/providers/Microsoft.RedHatOpenShift/OpenShiftClusters/test-aro-v4`

Después de habilitar la supervisión, pueden pasar unos 15 minutos hasta que pueda ver la métrica de estado del clúster.

### <a name="from-the-azure-portal"></a>Desde Azure Portal

La vista de varios clústeres de Azure Monitor para contenedores resalta los clústeres de Red Hat OpenShift en Azure que no tienen la supervisión habilitada en la pestaña **Clústeres no supervisados**. La opción **Habilitar** situada junto al clúster no inicia la incorporación de la supervisión desde el portal. Se le redirigirá a este artículo para habilitar manualmente la supervisión según los pasos descritos anteriormente en este artículo.

1. Inicie sesión en [Azure Portal](https://portal.azure.com).

2. En el menú de Azure Portal o en la página principal, seleccione **Azure Monitor**. En la sección **Insights**  (Conclusiones), seleccione **Contenedores**.

3. En la página **Supervisión - Contenedores**, seleccione **Non-monitored clusters** (Clústeres sin supervisión).

4. En la lista de clústeres sin supervisión, busque el clúster y haga clic en **Habilitar**. Puede identificar los resultados en la lista buscando el valor **ARO** en la columna **TIPO DE CLÚSTER**. Después de hacer clic en **Habilitar**, se le redirigirá a este artículo.

## <a name="next-steps"></a>Pasos siguientes

- Con la supervisión habilitada para recopilar el estado y la utilización de recursos del clúster de la versión 4.x de Red Hat OpenShift y las cargas de trabajo que se ejecutan en ellos, obtenga información sobre [cómo usar](container-insights-analyze.md) Azure Monitor para contenedores.

- De forma predeterminada, el agente en contenedores recopila los registros de contenedor stdout y stderr de todos los contenedores que se ejecutan en todos los espacios de nombres excepto kube-system. Para configurar la recopilación de registros de contenedor específica de uno o varios espacios de nombres determinados, consulte [Configuración del agente de Container Insights](container-insights-agent-config.md) para configurar las opciones de recopilación de datos que desee en el archivo de configuraciones ConfigMap.

- Para extraer y analizar las métricas de Prometheus desde el clúster, consulte [Configuración de la extracción de métricas de Prometheus](container-insights-prometheus-integration.md).

- Para aprender a detener la supervisión del clúster con Azure Monitor para contenedores, consulte [Cómo detener la supervisión del clúster de Red Hat OpenShift en Azure](container-insights-optout-openshift.md).

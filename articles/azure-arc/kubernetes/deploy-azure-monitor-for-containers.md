---
title: Incorporación de Azure Arc con Azure Monitor para contenedores (versión preliminar)
services: azure-arc
ms.service: azure-arc
ms.date: 05/19/2020
ms.topic: article
author: mlearned
ms.author: mlearned
description: Incorporación de Azure Arc con Azure Monitor para contenedores
keywords: Kubernetes, Arc, Azure, K8s, contenedores
ms.openlocfilehash: 3e1ea96a9241211b25a4e5b356723290fa647412
ms.sourcegitcommit: 50673ecc5bf8b443491b763b5f287dde046fdd31
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/20/2020
ms.locfileid: "83680738"
---
# <a name="onboard-azure-monitor-for-containers-with-arc-preview"></a>Incorporación de Azure Monitor para contenedores con Arc (versión preliminar)

Incorpore [contenedores habilitados para Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/insights/container-insights-overview) a clústeres de Kubernetes habilitados para Azure Arc.

## <a name="before-you-begin"></a>Antes de empezar

* [Versiones de Kubernetes](https://docs.microsoft.com/azure/aks/supported-kubernetes-versions)
* Distribuciones de Linux para los nodos de clúster (de y trabajo): Ubuntu (18.04 LTS y 16.04 LTS)
* Permiso de rol RBAC mínimo de Colaborador en la suscripción de Azure del clúster de Kubernetes habilitado para Azure Arc
* Identificador de recurso completo de Azure del clúster de Kubernetes habilitado para Azure Arc
* Contexto kubeconfig del clúster de Kubernetes
* El agente de supervisión requiere cAdvisor en el Kubelet que se ejecuta en el puerto seguro: 10250, o bien en el puerto no seguro: 10255 en todos los nodos para extraer las métricas de rendimiento   
* Se recomienda configurar el puerto cAdvisor de Kubelet para proteger el puerto 10250.
* Agente de supervisión requiere los siguientes puertos de salida y dominios para enviar los datos de supervisión al back-end de Azure Monitor (si está bloqueado por un servidor proxy o firewall)
    -  *.ods.opinsights.azure.com 443
    -  *.oms.opinsights.azure.com 443
    -  *.blob.core.windows.net 443
    -  dc.services.visualstudio.com 443

## <a name="onboarding"></a>Incorporación

### <a name="using-helm-chart"></a>Uso del gráfico de Helm

### <a name="option-1-using-powershell--script"></a>Opción 1: mediante un script de PowerShell

1. Descargue del script de incorporación.

    ```console
    curl -LO https://raw.githubusercontent.com/microsoft/OMS-docker/ci_feature/docs/haiku/onboarding_azuremonitor_for_containers.ps1
     ```

2. Instale [PowerShell Core](https://docs.microsoft.com/PowerShell/scripting/install/installing-PowerShell?view=PowerShell-6) en el equipo de desarrollo para ejecutar el script de incorporación de PowerShell.

3. Inicio de sesión en Azure

    ```console
    az login --use-device-code
    ```

4. Ejecute el script siguiente con el valor ResourceId de clúster de K8s de Azure Arc y el contexto del clúster de Kubernetes

    ```console
    .\onboarding_azuremonitor_for_containers.ps1 -azureArcClusterResourceId <resourcedIdOfAzureArcCluster> -kubeContext <kube-context>

    For Example ..
    .\onboarding_azuremonitor_for_containers.ps1 -azureArcClusterResourceId /subscriptions/57ac26cf-a9f0-4908-b300-9a4e9a0fb205/resourceGroups/AzureArcTest/providers/Microsoft.Kubernetes/connectedClusters/AzureArcTest1 -kubeContext MyK8sTestCluster
     ```

### <a name="option-2-using-bash-script"></a>Opción 2: mediante un script de Bash

> **Sugerencia:** El script usa las características de Bash 4, por lo que debe asegurarse de que el bash esté actualizado. Puede consultar la versión actual con `bash --version`.

1. Descargue del script de incorporación.

    ```console
    curl -LO https://raw.githubusercontent.com/microsoft/OMS-docker/ci_feature/docs/haiku/onboarding_azuremonitor_for_containers.sh
     ```

2. Ejecute el script siguiente con el valor ResourceId de clúster de K8s de Azure Arc y el contexto del clúster de Kubernetes

    ```console
    bash onboarding_azuremonitor_for_containers.sh <resourcedIdOfAzureArcCluster>  <kube-context>

    For Example:
    bash onboarding_azuremonitor_for_containers.sh /subscriptions/57ac26cf-a9f0-4908-b300-9a4e9a0fb205/resourceGroups/AzureArcTest/providers/Microsoft.Kubernetes/connectedClusters/AzureArcTest1 MyK8sTestCluster

     ```

## <a name="configure-agent-data-collection"></a>Configuración del agente de la recopilación de datos

De forma predeterminada, el agente no recopila los registros de los contenedores stdout y stderr en el espacio de nombres kube-system.
Consulte https://docs.microsoft.com/azure/azure-monitor/insights/container-insights-agent-config para configurar el agente con los ajustes de recopilación de datos deseados.

## <a name="configure-scraping-of-prometheus-metrics"></a>Configuración de la extracción de métricas de Prometheus

Azure Monitor para contenedores extrae las métricas de Prometheus y las ingiere en el back-end de Azure Monitor.
Consulte https://docs.microsoft.com/azure/azure-monitor/insights/container-insights-prometheus-integration para obtener instrucciones sobre cómo configurar la extracción en Prometheus.

## <a name="user-interface"></a>Interfaz de usuario

Vaya a https://aka.ms/azmon-containers-azurearc para ver el clúster incorporado.

## <a name="disable-monitoring"></a>Deshabilitación de la supervisión

Si por algún motivo quisiera deshabilitar la supervisión, puede eliminar simplemente el gráfico de HELM de Azure Monitor para contenedores a fin de dejar de recopilar e ingerir datos de supervisión en el back-end de Azure Monitor para contenedores.

    ```console
    helm del azmon-containers-release-1
    ```

## <a name="next-steps"></a>Pasos siguientes

* [Uso de Azure Policy para controlar la configuración del clúster](./use-azure-policy.md)


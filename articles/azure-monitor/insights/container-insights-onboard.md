---
title: Cómo habilitar Azure Monitor para contenedores | Microsoft Docs
description: En este artículo se describe cómo habilitar y configurar Azure Monitor para contenedores, de forma que pueda conocer el rendimiento de su contenedor y qué problemas relacionados con su rendimiento se han identificado.
ms.topic: conceptual
ms.date: 05/28/2020
ms.openlocfilehash: 0348d580a42d4a522ac05f929c96547a47e831a9
ms.sourcegitcommit: 2721b8d1ffe203226829958bee5c52699e1d2116
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/28/2020
ms.locfileid: "84147909"
---
# <a name="how-to-enable-azure-monitor-for-containers"></a>Cómo habilitar Azure Monitor para contenedores

En este artículo se proporciona información general acerca de las opciones disponibles para configurar la solución para contenedores de Azure Monitor para supervisar el rendimiento de las cargas de trabajo que se implementan en entornos de Kubernetes y se hospedan en:

- [Azure Kubernetes Service (AKS)](https://docs.microsoft.com/azure/aks/)

- Clústeres de Kubernetes autoadministrados que se hospedan en Azure con el [motor de AKS](https://github.com/Azure/aks-engine).

- Clústeres de Kubernetes autoadministrados que se hospedan en [Azure Stack](https://docs.microsoft.com/azure-stack/user/azure-stack-kubernetes-aks-engine-overview?view=azs-1910) o en el entorno local con el motor de AKS.

- [Red Hat OpenShift en Azure](../../openshift/intro-openshift.md) versión 3.x y 4.x

- [Red Hat OpenShift](https://docs.openshift.com/container-platform/4.3/welcome/index.html) versión 4.x

Azure Monitor para contenedores puede habilitarse para implementaciones de Kubernetes nuevas, o en una o más existentes, mediante los siguientes métodos compatibles:

- Desde Azure Portal, Azure PowerShell o con la CLI de Azure

- Usando [Terraform y AKS](../../terraform/terraform-create-k8s-cluster-with-tf-and-aks.md)

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>Requisitos previos

Antes de empezar, asegúrese de que dispone de lo siguiente:

- **Un área de trabajo de Log Analytics.**

    Azure Monitor para contenedores admite un área de trabajo de Log Analytics en las regiones enumeradas en los [productos por región](https://azure.microsoft.com/global-infrastructure/services/?regions=all&products=monitor) de Azure.

    Puede crear un área de trabajo al habilitar la supervisión de su nuevo clúster de AKS o dejar que la experiencia de incorporación cree un área de trabajo predeterminada en el grupo de recursos predeterminado de la suscripción del clúster de AKS. Si opta por crear el área de trabajo usted mismo, puede usar [Azure Resource Manager](../platform/template-workspace-configuration.md), mediante [PowerShell](../scripts/powershell-sample-create-workspace.md?toc=%2fpowershell%2fmodule%2ftoc.json) o [Azure Portal](../learn/quick-create-workspace.md). Para obtener una lista de los pares de asignación compatibles utilizados para el área de trabajo predeterminado, consulte [Region mapping for Azure Monitor for containers](container-insights-region-mapping.md) (Asignación de regiones para Azure Monitor para contenedores).

- Es miembro del **rol de colaborador de Log Analytics** para habilitar la supervisión de contenedores. Para más información acerca de cómo controlar el acceso a un área de trabajo de Log Analytics, consulte [Administración de áreas de trabajo](../platform/manage-access.md).

- Es miembro del rol de **[propietario](../../role-based-access-control/built-in-roles.md#owner)** en el recurso de clúster de AKS.

[!INCLUDE [log-analytics-agent-note](../../../includes/log-analytics-agent-note.md)]

* Las métricas de Prometheus no se recopilan de forma predeterminada. Antes de [configurar el agente](container-insights-prometheus-integration.md) para recopilarlas, es importante revisar la [documentación](https://prometheus.io/) de Prometheus para comprender lo que se puede recopilar y los métodos admitidos.

## <a name="supported-configurations"></a>Configuraciones admitidas

Lo siguiente se admite oficialmente con Azure Monitor para contenedores.

- Entornos: Red Hat OpenShift en Azure, Kubernetes local y motor de AKS en Azure y Azure Stack. Para más información, vea [Motor de AKS en Azure Stack](https://docs.microsoft.com/azure-stack/user/azure-stack-kubernetes-aks-engine-overview?view=azs-1908).
- Las versiones de Kubernetes y de la directiva de soporte son las mismas que las versiones de [AKS compatibles](../../aks/supported-kubernetes-versions.md). 

## <a name="network-firewall-requirements"></a>Requisitos de firewall de red

La información de la tabla siguiente muestra la configuración de proxy y firewall requerida para que el agente en contenedor se comunique con Azure Monitor para contenedores. Todo el tráfico de red del agente se enlaza a Azure Monitor.

|Recurso del agente|Puertos |
|--------------|------|
| *.ods.opinsights.azure.com | 443 |  
| *.oms.opinsights.azure.com | 443 |
| dc.services.visualstudio.com | 443 |
| *.monitoring.azure.com | 443 |
| login.microsoftonline.com | 443 |

En la información de la tabla siguiente se muestra la información de configuración de proxy y firewall para Azure China.

|Recurso del agente|Puertos |Descripción | 
|--------------|------|-------------|
| *.ods.opinsights.azure.cn | 443 | Ingesta de datos |
| *.oms.opinsights.azure.cn | 443 | Incorporación de OMS |
| dc.services.visualstudio.com | 443 | Para la telemetría del agente que usa Application Insights en la nube pública de Azure. |

En la información de la tabla siguiente se muestra la información de configuración de proxy y firewall para Azure US Government.

|Recurso del agente|Puertos |Descripción | 
|--------------|------|-------------|
| *.ods.opinsights.azure.us | 443 | Ingesta de datos |
| *.oms.opinsights.azure.us | 443 | Incorporación de OMS |
| dc.services.visualstudio.com | 443 | Para la telemetría del agente que usa Application Insights en la nube pública de Azure. |

## <a name="components"></a>Componentes

La capacidad de supervisar el rendimiento se basa en un agente de Log Analytics en contenedores para Linux, desarrollado específicamente para Azure Monitor para contenedores. Este agente especializado recopila datos de rendimiento y de eventos de todos los nodos del clúster, y el agente se implementa y registra automáticamente en el área de trabajo de Log Analytics especificada durante la implementación. La versión del agente es microsoft/oms:ciprod04202018 o posterior y se representa mediante una fecha en el siguiente formato: *mmddaaaa*.

>[!NOTE]
>Con la versión preliminar de Windows Server para AKS, un clúster de AKS con nodos de Windows Server no tiene instalado un agente para recopilar datos y reenviarlos a Azure Monitor. En su lugar, un nodo de Linux implementado automáticamente en el clúster como parte de la implementación estándar recopila y reenvía los datos a Azure Monitor en nombre de todos los nodos de Windows del clúster.  
>

Cuando se publica una nueva versión del agente, se actualiza automáticamente en los clústeres de Kubernetes administrado que se hospedan en Azure Kubernetes Service (AKS). Para seguir las versiones publicadas, consulte los [anuncios de versiones del agente](https://github.com/microsoft/docker-provider/tree/ci_feature_prod).

>[!NOTE]
>Si ya ha implementado un clúster de AKS, puede habilitar la supervisión mediante la CLI de Azure o una plantilla de Azure Resource Manager que se proporciona, como se muestra más adelante en este artículo. No puede usar `kubectl` para actualizar, eliminar, volver a implementar o implementar el agente.
>La plantilla debe implementarse en el mismo grupo de recursos que el clúster.

Para habilitar Azure Monitor para contenedores, puede usar uno de los métodos descritos en la tabla siguiente.

| Estado de la implementación | Método | Descripción |
|------------------|--------|-------------|
| Nuevo clúster de AKS Kubernetes | [Creación de un clúster de AKS con la CLI de Azure](../../aks/kubernetes-walkthrough.md#create-aks-cluster)| Puede habilitar la supervisión de un nuevo clúster de AKS que cree con la CLI de Azure. |
| | [Creación de un clúster de AKS con Terraform](container-insights-enable-new-cluster.md#enable-using-terraform)| Puede habilitar la supervisión de un nuevo clúster de AKS que cree mediante la herramienta de código abierto Terraform. |
| | [Creación de un clúster de OpenShift mediante una plantilla de Azure Resource Manager](container-insights-azure-redhat-setup.md#enable-for-a-new-cluster-using-an-azure-resource-manager-template) | Puede habilitar la supervisión de un clúster de OpenShift que haya creado con una plantilla preconfigurada de Azure Resource Manager. |
| | [Creación de un clúster de OpenShift con la CLI de Azure](https://docs.microsoft.com/cli/azure/openshift?view=azure-cli-latest#az-openshift-create) | Puede habilitar la supervisión durante la implementación de un nuevo clúster de OpenShift con la CLI de Azure. |
| Clúster de AKS Kubernetes existente | [Habilitar un clúster de AKS con la CLI de Azure](container-insights-enable-existing-clusters.md#enable-using-azure-cli) | Puede habilitar la supervisión de un clúster de AKS ya implementado mediante la CLI de Azure. |
| |[Habilitar un clúster de AKS con Terraform](container-insights-enable-existing-clusters.md#enable-using-terraform) | Puede habilitar la supervisión de un clúster de AKS ya implementado mediante la herramienta de código abierto Terraform. |
| | [Habilitar un clúster de AKS desde Azure Monitor](container-insights-enable-existing-clusters.md#enable-from-azure-monitor-in-the-portal)| Puede habilitar la supervisión de uno o varios clústeres de AKS ya implementados desde la página de varios clústeres en Azure Monitor. |
| | [Habilitación desde el clúster de AKS](container-insights-enable-existing-clusters.md#enable-directly-from-aks-cluster-in-the-portal)| Puede habilitar la supervisión directamente desde un clúster de AKS en Azure Portal. |
| | [Habilitar para un clúster de AKS mediante una plantilla de Azure Resource Manager](container-insights-enable-existing-clusters.md#enable-using-an-azure-resource-manager-template)| Puede habilitar la supervisión de un clúster de AKS con una plantilla preconfigurada de Azure Resource Manager. |
| | [Habilitación para un clúster de Kubernetes híbrido](container-insights-hybrid-setup.md) | Puede habilitar la supervisión de una instancia de AKS Engine hospedada en Azure Stack o para Kubernetes hospedado de forma local. |
| | [Habilitar para un clúster de OpenShift mediante una plantilla de Azure Resource Manager](container-insights-azure-redhat-setup.md#enable-using-an-azure-resource-manager-template) | Puede habilitar la supervisión de un clúster de OpenShift existente con una plantilla preconfigurada de Azure Resource Manager. |
| | [Habilitar para un clúster de OpenShift desde Azure Monitor](container-insights-azure-redhat-setup.md#from-the-azure-portal) | Puede habilitar la supervisión de uno o varios clústeres de OpenShift ya implementados desde la página de varios clústeres en Azure Monitor. |

## <a name="next-steps"></a>Pasos siguientes

- Con la supervisión habilitada, puede empezar a analizar el rendimiento de los clústeres de Kubernetes hospedados en Azure Kubernetes Service (AKS), Azure Stack u otro entorno. Para aprender a usar Azure Monitor para contenedores, consulte [Visualización del rendimiento del clúster de Kubernetes](container-insights-analyze.md).

---
title: Integración de Azure Monitor para Red Hat OpenShift 4.3 en Azure
description: Aprenda a habilitar Azure Monitor en el clúster de Red Hat OpenShift en Microsoft Azure.
author: sakthi-vetrivel
ms.author: suvetriv
ms.service: container-service
ms.topic: conceptual
ms.date: 03/06/2020
ms.openlocfilehash: a784fc070400995c56d16a3bc264d589bcb1f64e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "79082853"
---
# <a name="azure-monitor-integration-for-azure-red-hat-openshift-43"></a>Integración de Azure Monitor para Red Hat OpenShift 4.3 en Azure

> [!IMPORTANT] 
> Tenga en cuenta que Red Hat OpenShift 4.3 en Azure solo está disponible actualmente en versión preliminar privada en la región Este de EE. UU. La aceptación de la versión preliminar privada es solo por invitación. Asegúrese de registrar la suscripción antes de intentar habilitar esta característica: [Registro de la versión preliminar privada de Red Hat OpenShift en Azure](https://aka.ms/aro-preview-register)

> [!NOTE]
> Las características en versión preliminar son de autoservicio y se proporcionan tal cual y como están disponibles, y están excluidas del Acuerdo de Nivel de Servicio y la garantía limitada. Por tanto, estas características no están diseñadas para su uso en producción.

En este artículo se describe cómo habilitar la versión preliminar privada de Azure Monitor para contenedores de clústeres de OpenShift 4.3 hospedados de forma local o en cualquier entorno en la nube. También se aplican las mismas instrucciones para habilitar la supervisión de clústeres de Red Hat OpenShift (ARO) 4.3 en Azure.  

## <a name="prerequisites"></a>Prerrequisitos

- [CLI de Azure](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest)
- [Helm 3](https://helm.sh/docs/intro/install/)
- Acceso a kubeconfig del clúster de Kubernetes
- Acceso a una suscripción de Azure
- Acceso al clúster de OpenShift 4.3 para instalar el gráfico de Helm de Azure Monitor para contenedores
- Permiso de rol de RBAC mínimo de Colaborador en la suscripción de Azure  
- Monitoring Agent requiere los siguientes puertos de salida y dominios para enviar los datos de supervisión al back-end de Azure Monitor (si está bloqueado por un servidor proxy o firewall):
  - *.ods.opinsights.azure.com 443
  - *.oms.opinsights.azure.com 443
  - *.blob.core.windows.net 443
  - dc.services.visualstudio.com 443

## <a name="onboarding"></a>Incorporación

> [!TIP]
> El script usa las características de Bash 4, por lo que debe asegurarse de que el bash esté actualizado. Puede consultar la versión actual con `bash --version`.

### <a name="download-the-onboarding-script"></a>Descarga del script de incorporación

```bash
curl -LO  https://raw.githubusercontent.com/microsoft/OMS-docker/ci_feature/docs/openshiftV4/onboarding_azuremonitor_for_containers.sh
```

Ejecute el siguiente script con la información de ID de la suscripción de Azure, región del área de trabajo, nombre del clúster y contexto del clúster de Kubernetes.

```bash
bash onboarding_azuremonitor_for_containers.sh <azureSubscriptionId> <azureRegionforLogAnalyticsWorkspace> <clusterName> <kubeconfigContextNameOftheCluster>
```

Por ejemplo:

```bash
 bash onboarding_azuremonitor_for_containers.sh 27ac26cf-a9f0-4908-b300-9a4e9a0fb205 eastus myocp42 admin 
```

## <a name="configure-agent-data-collection"></a>Configuración del agente de la recopilación de datos

De forma predeterminada, Monitoring Agent recopila los registros de contenedor {stdout; stderr} de todos los contenedores que se ejecutan en todos los espacios de nombres excepto kube-system.  Si desea configurar la colección de registros de contenedor específica para espacios de nombres determinados, puede consultar la [configuración del agente de Container Insights](../azure-monitor/insights/container-insights-agent-config.md). Aquí puede configurar Monitoring Agent con la configuración de recopilación de datos que desee mediante la asignación de configuración.

## <a name="configure-scraping-of-prometheus-metrics"></a>Configuración de la extracción de métricas de Prometheus

Azure Monitor para contenedores extrae las métricas de Prometheus y las ingiere en el back-end de Azure Monitor. Consulte [Configuración de Prometheus en Container Insights](../azure-monitor/insights/container-insights-prometheus-integration.md) para obtener instrucciones sobre cómo configurar la extracción en Prometheus.

Después de una incorporación correcta, vaya a [Supervisión híbrida](https://aka.ms/azmon-containers-hybrid) y, en Entorno, seleccione **"Todos"** para ver el clúster de OpenShift v4 recién incorporado.

## <a name="disable-monitoring"></a>Deshabilitar la supervisión

Si desea deshabilitar la supervisión, puede eliminar el gráfico de Helm de Azure Monitor para contenedores con el siguiente comando para dejar de recopilar e ingerir datos de supervisión en el back-end de Azure Monitor para contenedores.

``` bash
helm del azmon-containers-release-1
```

## <a name="update-monitoring"></a>Actualización de la supervisión

Vuelva a ejecutar el script de incorporación como se describe en la sección [Incorporación](#onboarding) con el mismo parámetro para actualizar al gráfico de Helm más reciente.

## <a name="after-successful-onboarding"></a>Después de una incorporación correcta

Vaya a [Supervisión híbrida](https://aka.ms/azmon-containers-hybrid) y podrá ver el clúster de OpenShift/ARO v4 recién habilitado junto con el estado de mantenimiento en la pestaña **Clústeres supervisados**. Allí, para poder obtener información más detallada como métricas, inventario y registros, haga clic en la columna **Clúster**.

## <a name="supported-features"></a>Características admitidas

Para más información sobre las características y funcionalidades admitidas, consulte [Introducción a Container Insights](../azure-monitor/insights/container-insights-overview.md).

Póngase en contacto con nosotros mediante askcoin@microsoft.com para recibir comentarios y preguntas.

## <a name="next-steps"></a>Pasos siguientes

Para más información acerca de la supervisión, consulte:
- [Introducción a Container Insights](../azure-monitor/insights/container-insights-overview.md)

- [Introducción a las consultas de registros](../azure-monitor/log-query/log-query-overview.md)

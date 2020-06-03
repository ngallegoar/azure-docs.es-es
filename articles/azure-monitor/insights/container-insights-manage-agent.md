---
title: Administración de Azure Monitor para el agente de contenedores | Microsoft Docs
description: En este artículo se describe cómo administrar las tareas de mantenimiento más comunes con el agente de Log Analytics en contenedores que usa Azure Monitor para contenedores.
ms.topic: conceptual
ms.date: 05/12/2020
ms.openlocfilehash: ce014d27c6acc473c4a435dfed4757fb0884f4fe
ms.sourcegitcommit: fdec8e8bdbddcce5b7a0c4ffc6842154220c8b90
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/19/2020
ms.locfileid: "83652197"
---
# <a name="how-to-manage-the-azure-monitor-for-containers-agent"></a>Administración de Azure Monitor para el agente de contenedores

Azure Monitor para contenedores usa una versión con contenedores del agente de Log Analytics para Linux. Después de la implementación inicial, hay tareas rutinarias u opcionales que debe hacer durante su ciclo de vida. En este artículo se detallan los procedimientos para actualizar el agente manualmente y deshabilitar la recopilación de variables de entorno desde un contenedor determinado. 

## <a name="how-to-upgrade-the-azure-monitor-for-containers-agent"></a>Actualización de Azure Monitor para el agente de contenedores

Azure Monitor para contenedores usa una versión con contenedores del agente de Log Analytics para Linux. Cuando se publica una nueva versión del agente, este se actualiza automáticamente en los clústeres de Kubernetes administrado que se hospedan en Azure Kubernetes Service (AKS) y Red Hat OpenShift en Azure versión 3.x. En el caso de un [clúster de Kubernetes híbrido](container-insights-hybrid-setup.md) y Red Hat OpenShift en Azure versión 4.x, el agente no está administrado y debe actualizarlo manualmente.

Si se produce un error en la actualización del agente de un clúster hospedado en AKS o Red Hat OpenShift en Azure versión 3.x, en este artículo también se describe el proceso para actualizar el agente de forma manual. Para seguir las versiones publicadas, consulte los [anuncios de versiones del agente](https://github.com/microsoft/docker-provider/tree/ci_feature_prod).

### <a name="upgrade-agent-on-aks-cluster"></a>Actualización del agente en el clúster de AKS

El proceso para actualizar al agente en clústeres de AKS consta de dos pasos sencillos. El primer paso es deshabilitar la supervisión con Azure Monitor para contenedores mediante la CLI de Azure. Siga los pasos descritos en el artículo [Deshabilitar la supervisión](container-insights-optout.md?#azure-cli). El uso de la CLI de Azure nos permite quitar el agente de los nodos del clúster sin afectar a la solución ni a los datos correspondientes que se almacenan en el área de trabajo. 

>[!NOTE]
>Mientras está realizando esta actividad de mantenimiento, los nodos del clúster no reenviarán los datos recopilados y las vistas de rendimiento no mostrarán datos en el período comprendido entre la eliminación del agente y la instalación de la nueva versión. 
>

Para instalar la nueva versión del agente, siga los pasos descritos en [Habilitar de la supervisión con la CLI de Azure](container-insights-enable-new-cluster.md#enable-using-azure-cli) para completar este proceso.  

Después de volver a habilitar la supervisión, es posible que pasen unos 15 minutos hasta que pueda ver actualizada la métrica de estado del clúster. Para verificar que el agente se ha actualizado correctamente, ejecute el comando: `kubectl logs omsagent-484hw --namespace=kube-system`

El estado debe parecerse al ejemplo siguiente, donde los valores de *omi* y *omsagent* deben coincidir con la versión más reciente especificada en el [historial de versiones del agente](https://github.com/microsoft/docker-provider/tree/ci_feature_prod).  

    User@aksuser:~$ kubectl logs omsagent-484hw --namespace=kube-system
    :
    :
    instance of Container_HostInventory
    {
        [Key] InstanceID=3a4407a5-d840-4c59-b2f0-8d42e07298c2
        Computer=aks-nodepool1-39773055-0
        DockerVersion=1.13.1
        OperatingSystem=Ubuntu 16.04.3 LTS
        Volume=local
        Network=bridge host macvlan null overlay
        NodeRole=Not Orchestrated
        OrchestratorType=Kubernetes
    }
    Primary Workspace: b438b4f6-912a-46d5-9cb1-b44069212abc    Status: Onboarded(OMSAgent Running)
    omi 1.4.2.5
    omsagent 1.6.0-163
    docker-cimprov 1.0.0.31

### <a name="upgrade-agent-on-hybrid-kubernetes-cluster"></a>Actualización de agente en un clúster de Kubernetes híbrido

Siga los pasos que se describen a continuación para actualizar el agente en un clúster de Kubernetes que se ejecute en:

* clústeres de Kubernetes autoadministrados que se hospedan en Azure con el motor de AKS;
* clústeres de Kubernetes autoadministrados que se hospedan en Azure Stack o en el entorno local con el motor de AKS;
* Red Hat OpenShift versión 4.x.

Si el área de trabajo de Log Analytics está en una instancia comercial de Azure, ejecute el siguiente comando:

```
$ helm upgrade --name myrelease-1 \
--set omsagent.secret.wsid=<your_workspace_id>,omsagent.secret.key=<your_workspace_key>,omsagent.env.clusterName=<my_prod_cluster> incubator/azuremonitor-containers
```

Si el área de trabajo de Log Analytics está en Azure China, ejecute el siguiente comando:

```
$ helm upgrade --name myrelease-1 \
--set omsagent.domain=opinsights.azure.cn,omsagent.secret.wsid=<your_workspace_id>,omsagent.secret.key=<your_workspace_key>,omsagent.env.clusterName=<your_cluster_name> incubator/azuremonitor-containers
```

Si el área de trabajo de Log Analytics está en Azure Gobierno de EE. UU, ejecute el siguiente comando:

```
$ helm upgrade --name myrelease-1 \
--set omsagent.domain=opinsights.azure.us,omsagent.secret.wsid=<your_workspace_id>,omsagent.secret.key=<your_workspace_key>,omsagent.env.clusterName=<your_cluster_name> incubator/azuremonitor-containers
```

### <a name="upgrade-agent-on-azure-red-hat-openshift-v4"></a>Actualización del agente en Red Hat OpenShift en Azure versión 4

Siga los pasos que se describen a continuación para actualizar el agente en un clúster de Kubernetes que se ejecuta en Red Hat OpenShift en Azure versión 4. 

>[!NOTE]
>Red Hat OpenShift en Azure versión 4 solo admite la ejecución en la nube comercial de Azure.
>

```
$ helm upgrade --name myrelease-1 \
--set omsagent.secret.wsid=<your_workspace_id>,omsagent.secret.key=<your_workspace_key>,omsagent.env.clusterId=<azureAroV4ResourceId> incubator/azuremonitor-containers
```

## <a name="how-to-disable-environment-variable-collection-on-a-container"></a>Cómo deshabilitar la recopilación de variables de entorno en un contenedor

Azure Monitor para contenedores recopila las variables de entorno de los contenedores que se ejecutan en un pod y las presenta en el panel de propiedades del contenedor seleccionado en la vista **Contenedores**. Puede controlar este comportamiento si deshabilita la recopilación de un contenedor específico, ya sea durante la implementación del clúster de Kubernetes, o después, al establecer la variable de entorno *AZMON_COLLECT_ENV*. Esta característica está disponible en la versión del agente: ciprod11292018 y posterior.  

Para deshabilitar la recopilación de variables de entorno en un contenedor nuevo o existente, establezca la variable **AZMON_COLLECT_ENV** con un valor de **False** en el archivo de configuración yaml de implementación de Kubernetes. 

```  
- name: AZMON_COLLECT_ENV  
  value: "False"  
```  

Ejecute el comando siguiente para aplicar el cambio a los clústeres de Kubernetes que no sean de Red Hat OpenShift en Azure): `kubectl apply -f  <path to yaml file>`. Para editar ConfigMap y aplicar este cambio a los clústeres de Red Hat OpenShift en Azure, ejecute el comando:

``` bash
oc edit configmaps container-azm-ms-agentconfig -n openshift-azure-logging
```

Se abrirá el editor de texto predeterminado. Después de establecer la variable, guarde el archivo en el editor.

Para verificar que el cambio de configuración surta efecto, seleccione un contenedor en la vista **Contenedores** en Azure Monitor para contenedores y, en el panel de propiedades, expanda **Variables de entorno**.  La sección debe mostrar solo la variable que creó anteriormente: **AZMON_COLLECT_EN=FALSE**. Para todos los otros contenedores, la sección Variables de entorno debe enumerar todas las variables de entorno detectadas.

Para volver a habilitar la detección de las variables de entorno, aplique el mismo proceso anterior y cambie el valor de **False** a **True** y, luego, vuelva a ejecutar el comando `kubectl` para actualizar el contenedor.  

```  
- name: AZMON_COLLECT_ENV  
  value: "True"  
```  

## <a name="next-steps"></a>Pasos siguientes

Si experimenta problemas al actualizar el agente, consulte la [guía de solución de problemas](container-insights-troubleshoot.md) para obtener ayuda.

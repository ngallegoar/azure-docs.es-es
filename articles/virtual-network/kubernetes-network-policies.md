---
title: Directivas de red de Azure Kubernetes | Microsoft Docs
description: Más información acerca de las directivas de red de Kubernetes para proteger el clúster de Kubernetes.
services: virtual-network
documentationcenter: na
author: aanandr
manager: NarayanAnnamalai
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 9/25/2018
ms.author: aanandr
ms.custom: ''
ms.openlocfilehash: 5a6da7e65a9a3e962a2df37b062792fbb990d04d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "73159682"
---
# <a name="azure-kubernetes-network-policies-overview"></a>Introducción a las directivas de red de Azure Kubernetes

Las directivas de red proporcionan microsegmentación para pods del mismo modo que los grupos de seguridad de red (NSG) proporcionan microsegmentación para máquinas virtuales. La implementación de la directiva de red de Azure admite la especificación de directiva de red de Kubernetes estándar. Puede usar etiquetas para seleccionar un grupo de pods y definir una lista de reglas de entrada y salida que especifican el tipo de tráfico que se permite hacia y desde los pods. Obtenga más información sobre las directivas de red de Kubernetes en la [documentación de Kubernetes](https://kubernetes.io/docs/concepts/services-networking/network-policies/).

![Introducción a las directivas de red de Kubernetes](./media/kubernetes-network-policies/kubernetes-network-policies-overview.png)

Las directivas de red de Azure funcionan conjuntamente con CNI de Azure, que proporciona integración con red virtual para los contenedores. Actualmente solo se admiten nodos de Linux. Las implementaciones configuran las reglas de la tabla de IP de Linux según las directivas definidas para aplicar el filtrado de tráfico.

## <a name="planning-security-for-your-kubernetes-cluster"></a>Planeación de seguridad para el clúster de Kubernetes
Al implementar la seguridad para el clúster, utilice grupos de seguridad red (NSG) para filtrar el tráfico de norte a sur, es decir, el que entra y sale de la subred del clúster, y use las directivas de red de Kubernetes para el tráfico de este a oeste, es decir, el tráfico entre pods del clúster.

## <a name="using-azure-kubernetes-network-policies"></a>Uso de las directivas de red de Azure Kubernetes
Las directivas de red de Azure pueden usarse de las siguientes formas para proporcionar microsegmentación para los pods.

### <a name="acs-engine"></a>Motor de ACS
Motor de ACS es una herramienta que genera una plantilla de Azure Resource Manager para la implementación de un clúster de Kubernetes en Azure. La configuración del clúster se especifica en un archivo JSON que se pasa a la herramienta al generar la plantilla. Para ver la lista completa de las configuraciones de clúster compatibles y sus descripciones, consulte Microsoft Azure Container Service Engine - Cluster Definition (Motor de Microsoft Azure Container Service: definición de clúster).

Para habilitar las directivas en clústeres implementados con motor de ACS, especifique el valor de la configuración de networkPolicy en el archivo de definición del clúster como "azure".

#### <a name="example-configuration"></a>Ejemplo de configuración

En la siguiente configuración de ejemplo JSON se crea una nueva red virtual y una subred e se implementa un clúster de Kubernetes en ella con CNI de Azure. Se recomienda que utilice "Notepad" para editar el archivo JSON. 
```json
{
  "apiVersion": "vlabs",
  "properties": {
    "orchestratorProfile": {
      "orchestratorType": "Kubernetes",
      "kubernetesConfig": {
         "networkPolicy": "azure"
       }
    },
    "masterProfile": {
      "count": 1,
      "dnsPrefix": "<specify a cluster name>",
      "vmSize": "Standard_D2s_v3"
    },
    "agentPoolProfiles": [
      {
        "name": "agentpool",
        "count": 2,
        "vmSize": "Standard_D2s_v3",
        "availabilityProfile": "AvailabilitySet"
      }
    ],
   "linuxProfile": {
      "adminUsername": "<specify admin username>",
      "ssh": {
        "publicKeys": [
          {
            "keyData": "<cut and paste your ssh key here>"
          }
        ]
      }
    },
    "servicePrincipalProfile": {
      "clientId": "<enter the client ID of your service principal here >",
      "secret": "<enter the password of your service principal here>"
    }
  }
}

```
### <a name="creating-your-own-kubernetes-cluster-in-azure"></a>Creación de su propio clúster de Kubernetes en Azure
La implementación puede utilizarse para proporcionar directivas de red para los pods en clústeres de Kubernetes que implemente usted mismo, sin necesidad de usar herramientas como el motor de ACS. En este caso, en primer lugar debe instalar el complemento de CNI y habilitarlo en todas las máquinas virtuales de un clúster. Para instrucciones detalladas, consulte [Implementación del complemento para un clúster de Kubernetes que usted mismo haya implementado](deploy-container-networking.md#deploy-plug-in-for-a-kubernetes-cluster).

Una vez implementado el clúster, ejecute el siguiente comando `kubectl` para descargar y aplicar la directiva de red de Azure *daemonset* en el clúster.

  ```
  kubectl apply -f https://raw.githubusercontent.com/Azure/acs-engine/master/parts/k8s/addons/kubernetesmasteraddons-azure-npm-daemonset.yaml

  ```
La solución también es de código abierto y el código está disponible en el [repositorio de redes de Azure Container](https://github.com/Azure/azure-container-networking/tree/master/npm).



## <a name="next-steps"></a>Pasos siguientes
- Obtenga más información acerca de [Azure Kubernetes Service](../aks/intro-kubernetes.md).
-  Obtenga más información acerca de [redes de contenedores](container-networking-overview.md).
- [Implemente el complemento](deploy-container-networking.md) para clústeres de Kubernetes o contenedores de Docker.

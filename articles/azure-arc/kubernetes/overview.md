---
title: Introducción a Kubernetes habilitado para Azure Arc
services: azure-arc
ms.service: azure-arc
ms.date: 05/19/2020
ms.topic: overview
author: mlearned
ms.author: mlearned
description: ''
keywords: Kubernetes, Arc, Azure, containers
ms.openlocfilehash: 3b3b184ff7b18e05dfeb1052c0d6b4aa406a8787
ms.sourcegitcommit: fdec8e8bdbddcce5b7a0c4ffc6842154220c8b90
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/19/2020
ms.locfileid: "83665280"
---
# <a name="what-is-azure-arc-enabled-kubernetes-preview"></a>¿Qué es Kubernetes habilitado para Azure Arc (versión preliminar)?

Puede asociar y configurar clústeres de Kubernetes dentro o fuera de Azure con Kubernetes habilitado para Azure Arc (versión preliminar). Cuando un clúster de Kubernetes está asociado a Azure Arc, aparecerá en Azure Portal y tendrá un identificador de Azure Resource Manager y una identidad administrada. Los clústeres se asocian a suscripciones estándar de Azure, residen en un grupo de recursos y pueden recibir etiquetas como cualquier otro recurso de Azure. 


Para la conexión de un clúster de Kubernetes a Azure, es necesario que un administrador de clústeres implemente agentes. Estos agentes se ejecutan en un espacio de nombres de Kubernetes denominado `azure-arc` y son implementaciones estándar de Kubernetes. Los agentes son responsables de la conectividad con Azure, de la recopilación de registros y métricas de Azure Arc y de la supervisión de solicitudes de configuración.  
 
 > [!NOTE]
> Kubernetes habilitado para Azure Arc se encuentra en versión preliminar y no se recomienda para cargas de trabajo de producción. 


## <a name="supported-scenarios"></a>Escenarios admitidos 

Kubernetes habilitado para Azure Arc admite los siguientes escenarios: 

* Conexión de Kubernetes cuando se ejecuta fuera de Azure para la realización de tareas de inventario, agrupación y etiquetado 

* Implementación de aplicaciones y aplicación de la configuración mediante la administración de configuraciones basada en GitOps 

* Uso de Azure Monitor en los contenedores para ver y supervisar los clústeres 

* Aplicación de directivas mediante Azure Policy para Kubernetes 

 
## <a name="supported-regions"></a>Regiones admitidas 

Kubernetes habilitado para Azure Arc se admite actualmente en las siguientes regiones: 

* Este de EE. UU. 
* Oeste de Europa 


## <a name="next-steps"></a>Pasos siguientes

* [Conexión de un clúster](./connect-cluster.md)

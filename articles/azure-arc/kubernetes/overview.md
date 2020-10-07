---
title: Información general de Kubernetes habilitado para Azure Arc
services: azure-arc
ms.service: azure-arc
ms.date: 05/19/2020
ms.topic: overview
author: mlearned
ms.author: mlearned
description: En este artículo se proporciona información general de Kubernetes habilitado para Azure Arc.
keywords: Kubernetes, Arc, Azure, containers
ms.custom: references_regions
ms.openlocfilehash: 61317f7f5f2bf17c88fc019294574993c1854e59
ms.sourcegitcommit: f5580dd1d1799de15646e195f0120b9f9255617b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/29/2020
ms.locfileid: "91540649"
---
# <a name="what-is-azure-arc-enabled-kubernetes-preview"></a>¿Qué es la versión preliminar de Kubernetes habilitado para Azure Arc?

Puede asociar y configurar clústeres de Kubernetes dentro o fuera de Azure mediante la versión preliminar de Kubernetes habilitado para Azure Arc. Cuando un clúster de Kubernetes esté asociado a Azure Arc, aparecerá en Azure Portal. Tendrá un identificador de Azure Resource Manager y una identidad administrada. Los clústeres se asocian a suscripciones estándar de Azure, viven en un grupo de recursos y pueden recibir etiquetas como cualquier otro recurso de Azure. 

Para conectar un clúster de Kubernetes a Azure, es preciso que el administrador de clústeres implemente agentes. Estos agentes se ejecutan en un espacio de nombres de Kubernetes denominado `azure-arc` y son implementaciones estándar de Kubernetes. Los agentes son responsables de la conectividad con Azure, de la recopilación de registros y métricas de Azure Arc y de la supervisión de solicitudes de configuración. 

Kubernetes habilitado para Azure Arc admite SSL estándar del sector para proteger los datos en tránsito. Además, los datos se almacenan cifrados en reposo en una base de datos de Azure Cosmos DB para garantizar la confidencialidad de los datos.
 
> [!NOTE]
> Kubernetes habilitado para Azure Arc está en versión preliminar. No es aconsejable para cargas de trabajo de producción.

## <a name="supported-kubernetes-distributions"></a>Distribuciones de Kubernetes admitidas

Kubernetes habilitado para Azure Arc funciona con cualquier clúster de Kubernetes certificado de Cloud Native Computing Foundation (CNCF), como AKS-Engine en Azure, AKS-Engine en Azure Stack Hub, GKE, EKS y clúster de VMware vSphere, con Azure Arc.

El equipo de Arc ha probado las características de Kubernetes habilitado para Azure Arc en las siguientes distribuciones:
* RedHat OpenShift 4.3
* Rancher RKE 1.0.8
* Canonical Charmed Kubernetes 1.18
* Motor de AKS
* Motor de AKS en Azure Stack Hub
* Proveedor de Cluster API de Azure

## <a name="supported-scenarios"></a>Escenarios admitidos 

Kubernetes habilitado para Azure Arc admite estos escenarios: 

* Conexión de Kubernetes cuando se ejecuta fuera de Azure para la realización de tareas de inventario, agrupación y etiquetado.

* Implementación de aplicaciones y aplicación de la configuración mediante la administración de configuraciones basada en GitOps. 

* Uso de Azure Monitor en los contenedores para ver y supervisar los clústeres. 

* Aplicación de directivas mediante Azure Policy para Kubernetes. 

[!INCLUDE [azure-lighthouse-supported-service](../../../includes/azure-lighthouse-supported-service.md)]

## <a name="supported-regions"></a>Regiones admitidas 

Kubernetes habilitado para Azure Arc se admite actualmente en estas regiones: 

* Este de EE. UU. 
* Oeste de Europa

## <a name="frequently-asked-questions"></a>Preguntas más frecuentes

* ¿Cuál es la diferencia entre Kubernetes habilitado para Azure Arc y Azure Kubernetes Service (AKS)?

    Azure Kubernetes Service (AKS) es la oferta de Kubernetes administrado de Azure. AKS simplifica la implementación en Azure de clústeres de Kubernetes administrado. AKS reduce la complejidad y la sobrecarga operativa de la administración de Kubernetes al descargar gran parte de esa responsabilidad en Azure. Azure administra los maestros de Kubernetes. Usted solo administra y mantiene los nodos del agente.

    Kubernetes habilitado para Azure Arc permite conectar clústeres de Kubernetes a Azure para ampliar las funcionalidades de administración de Azure, como Azure Monitor y Azure Policy. El mantenimiento del propio clúster de Kubernetes subyacente lo realiza el usuario.

* ¿Es necesario conectar a Azure Arc los clústeres de Azure Kubernetes Service que se ejecutan en Azure?

    No. Todas las características de Kubernetes habilitado para Azure Arc, como Azure Monitor o Azure Policy (Gatekeeper), están disponibles de forma nativa con AKS, que ya tiene una representación de recursos en Azure. La configuración de clúster (GitOps) también está disponible de forma nativa en AKS y actualmente en versión preliminar privada. Use este [formulario de registro](https://forms.microsoft.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbR5acO18Lmx5Bk_qao2CrOcFUQ0UyRllDR1BEV1BPNENYRERYN1pFWTQ4WC4u) para obtener acceso a esta característica.
    
* ¿Debo conectar mi clúster de AKS en Azure Stack HCl a Azure Arc? ¿Qué ocurre con los clústeres de Kubernetes que se ejecutan en Azure Stack Hub o en el motor de Azure Stack?

    Sí, la conexión de estos clústeres a Azure Arc presenta ventajas. Por ejemplo, proporciona una representación de recursos para estos clústeres de Kubernetes en Azure Resource Manager. Con esta representación de recursos, algunas funcionalidades, como la configuración de clúster, Azure Monitor y Azure Policy (Gatekeeper), se pueden extender a estos clústeres de Kubernetes.

## <a name="next-steps"></a>Pasos siguientes

* [Conexión de un clúster](./connect-cluster.md)

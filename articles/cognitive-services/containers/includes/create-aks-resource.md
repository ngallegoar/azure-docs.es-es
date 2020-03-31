---
title: Creación de un recurso de clúster de Azure Kubernetes Service
titleSuffix: Azure Cognitive Services
description: Obtenga información sobre cómo crear recursos de Azure Kubernetes (AKS).
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 11/21/2019
ms.author: dapine
ms.openlocfilehash: 4e3102912e88ef904fed3e680f8cdd23242b1f17
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "74383482"
---
## <a name="create-an-azure-kubernetes-service-cluster-resource"></a>Creación de un recurso de clúster de Azure Kubernetes Service

1. Vaya a [Azure Kubernetes Service](https://ms.portal.azure.com/#create/microsoft.aks) y seleccione **Crear**.

1. Introduzca la siguiente información de la pestaña **Aspectos básicos**:

    |Configuración|Value|
    |--|--|
    |Subscription|Seleccione una suscripción adecuada.|
    |Resource group|Seleccione un grupo de recursos disponible.|
    |Nombre del clúster de Kubernetes|Escriba un nombre (en minúsculas).|
    |Region|Seleccione una ubicación cercana.|
    |Versión de Kubernetes|Cualquier valor se marca como **(valor predeterminado)** .|
    |Prefijo del nombre DNS|Se crea automáticamente, pero puede invalidarlo.|
    |Tamaño del nodo|Estándar DS2 v2:<br>`2 vCPUs`, `7 GB`|
    |Número de nodos|Deje el control deslizante en el valor predeterminado.|

1. En la pestaña **Scale** (Escala), deje los campos **Virtual nodes** (Nodos virtuales) y **VM scale sets** (Conjuntos de escalado de máquinas virtuales) con sus valores predeterminados.
1. En la pestaña **Authentication** (Autenticación), deje las opciones **Service principal** (Entidad de servicio) y **Enable RBAC** (Habilitar RBAC) establecidas en sus valores predeterminados.
1. En la pestaña **Networking** (Redes), escriba las siguientes selecciones:

    |Configuración|Value|
    |--|--|
    |Enrutamiento de aplicación HTTP|No|
    |Configuración de red|Básica|

1. En la pestaña **Supervisión**, asegúrese de que la opción **Habilitar la supervisión de contenedores** esté establecida en **Sí** y deje **Área de trabajo de Log Analytics** con el valor predeterminado.
1. En la pestaña **Tags** (Etiquetas), deje los pares nombre/valor en blanco por ahora.
1. Seleccione **Revisar y crear**.
1. Una vez pasada la validación, seleccione **Crear**.

> [!NOTE]
> Si se produce un error en la validación, podría deberse a un error de la entidad de servicio. Vuelva a la pestaña **Autenticación** y, luego, vaya a **Revisar y crear**, donde se debe ejecutar y superar la validación.

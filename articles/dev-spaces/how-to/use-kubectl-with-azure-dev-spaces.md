---
title: Uso de kubectl con Azure Dev Spaces
services: azure-dev-spaces
ms.date: 05/11/2018
ms.topic: conceptual
description: Obtenga información sobre cómo usar los comandos de kubectl en un espacio de desarrollo en un clúster de Azure Kubernetes Service con Azure Dev Spaces habilitado.
keywords: 'Docker, Kubernetes, Azure, AKS, Azure Kubernetes Service, contenedores, Helm, service mesh, enrutamiento de service mesh, kubectl, k8s '
ms.openlocfilehash: e6f79d98cf209d1bc19753f19c9b17b06017c2b7
ms.sourcegitcommit: d103a93e7ef2dde1298f04e307920378a87e982a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/13/2020
ms.locfileid: "91960174"
---
# <a name="use-kubectl-with-an-azure-dev-space"></a>Uso de kubectl con una instancia de Azure Dev Spaces

[!INCLUDE [Azure Dev Spaces deprecation](../../../includes/dev-spaces-deprecation.md)]

Puede tener acceso al clúster de Kubernetes dentro de un espacio de Azure Dev Space y utilizar herramientas de Kubernetes existentes, como `kubectl`.

La ejecución de `az aks use-dev-spaces` agregará automáticamente un contexto de configuración de `kubectl`, por lo que kubectl ya debe estar conectado a su clúster de Kubernetes en Azure Dev Spaces. Ejemplos:
- Confirme el contexto actual: `kubectl config current-context`
- Enumere todos los contextos disponibles: `kubectl config get-contexts`. 
- Cambie el contexto: `kubectl config use-context <context-name>`
- Vea el panel de Kubernetes: ejecute `kubectl proxy` y luego abra el explorador en la dirección que emite este comando (adjunte `/ui` a la dirección URL para navegar al panel de Kubernetes).
- Enumere los servicios en ejecución en el espacio predeterminado de Azure Dev Spaces denominado *default*: `kubectl get services --namespace=default`


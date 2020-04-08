---
title: Reparación automática de nodos de Azure Kubernetes Service (AKS)
description: Obtenga información sobre la funcionalidad de reparación automática de nodos y cómo AKS corrige los nodos de trabajo interrumpidos.
services: container-service
ms.topic: conceptual
ms.date: 03/10/2020
ms.openlocfilehash: 9bf9df69a0a6bfa4d9f4029278d2a146811980c8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "80284847"
---
# <a name="azure-kubernetes-service-aks-node-auto-repair"></a>Reparación automática de nodos de Azure Kubernetes Service (AKS)

AKS comprueba de forma continua el estado de mantenimiento de los nodos de trabajo y, si son incorrectos, los repara automáticamente. En esta documentación se describe cómo Azure Kubernetes Service (AKS) supervisa los nodos de trabajo y repara aquellos que tienen un estado incorrecto.  El objetivo de la documentación es informar a los operadores de AKS sobre el comportamiento de la funcionalidad de reparación de nodos. También es importante tener en cuenta que la plataforma Azure [realiza el mantenimiento en máquinas virtuales][vm-updates] que experimentan problemas. AKS y Azure trabajan de forma conjunta para minimizar las interrupciones del servicio de los clústeres.

> [!Important]
> La funcionalidad de reparación automática de nodos no se admite actualmente para grupos de nodos de Windows Server.

## <a name="how-aks-checks-for-unhealthy-nodes"></a>Comprobaciones de nodos en mal estado en AKS

> [!Note]
> AKS realiza una acción de reparación en los nodos con la cuenta de usuario **aks-remediator**.

AKS usa reglas para determinar si un nodo tiene un estado incorrecto y necesita reparación. AKS usa las reglas siguientes para determinar si es necesaria la reparación automática.

* El nodo notifica el estado de **NotReady** de las comprobaciones consecutivas en un período de 10 minutos.
* El nodo no notifica un estado en 10 minutos.

Puede comprobar manualmente el estado de mantenimiento de los nodos con kubectl. 

```
kubectl get nodes
```

## <a name="how-automatic-repair-works"></a>Funcionamiento de la reparación automática

> [!Note]
> AKS realiza una acción de reparación en los nodos con la cuenta de usuario **aks-remediator**.

Este comportamiento es para **Virtual Machine Scale Sets**.  La reparación automática realiza varios pasos para reparar un nodo interrumpido.  Si se determina que un nodo tiene un estado incorrecto, AKS intenta varios pasos de corrección.  Los pasos se realizan en este orden:

1. Si el entorno de ejecución del contenedor deja de responder durante 10 minutos, los servicios del entorno de ejecución con errores se reinician en el nodo.
2. Si el nodo no está listo después de 10 minutos, se reinicia.
3. Si el nodo no está listo después de 30 minutos, se vuelve a crear la imagen del nodo.

> [!Note]
> Si hay varios nodos con un estado incorrecto, se reparan uno a uno.

## <a name="next-steps"></a>Pasos siguientes

Use [Availability Zones][availability-zones] para aumentar la alta disponibilidad con las cargas de trabajo del clúster de AKS.

<!-- LINKS - External -->

<!-- LINKS - Internal -->
[availability-zones]: ./availability-zones.md
[vm-updates]: ../virtual-machines/maintenance-and-updates.md

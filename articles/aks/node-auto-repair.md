---
title: Reparación automática de nodos de Azure Kubernetes Service (AKS)
description: Obtenga información sobre la funcionalidad de reparación automática de nodos y cómo AKS corrige los nodos de trabajo interrumpidos.
services: container-service
ms.topic: conceptual
ms.date: 06/02/2020
ms.openlocfilehash: 91384461567634faabaaa1dd588d6e7ec6ece60e
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "84735632"
---
# <a name="azure-kubernetes-service-aks-node-auto-repair"></a>Reparación automática de nodos de Azure Kubernetes Service (AKS)

AKS comprueba de forma continua el estado de mantenimiento de los nodos de trabajo y, si son incorrectos, los repara automáticamente. Este documento informa a los operadores sobre cómo se comporta la funcionalidad de reparación automática del nodo. Además de las reparaciones de AKS, la plataforma de VM de Azure [realiza el mantenimiento en máquinas virtuales][vm-updates] que también experimentan problemas. AKS y las VM de Azure trabajan de forma conjunta para minimizar las interrupciones del servicio de los clústeres.

> [!Important]
> La funcionalidad de reparación automática de nodos no se admite actualmente para grupos de nodos de Windows Server.

## <a name="how-aks-checks-for-unhealthy-nodes"></a>Comprobaciones de nodos en mal estado en AKS

> [!Note]
> AKS realiza una acción de reparación en los nodos con la cuenta de usuario **aks-remediator**.

AKS usa reglas para determinar si un nodo es incorrecto y necesita reparación. AKS usa las reglas siguientes para determinar si es necesaria la reparación automática.

* El nodo notifica el estado de **NotReady** de las comprobaciones consecutivas en un período de 10 minutos.
* El nodo no notifica un estado en 10 minutos.

Puede comprobar manualmente el estado de mantenimiento de los nodos con kubectl. 

```
kubectl get nodes
```

## <a name="how-automatic-repair-works"></a>Funcionamiento de la reparación automática

> [!Note]
> AKS inicia las operaciones de reparación con la cuenta de usuario **aks-remediator**.

La reparación automática se admite de forma predeterminada para los clústeres con un tipo de conjunto de VM de **Virtual Machine Scale Sets**. Si se determina que un nodo es incorrecto en función de las reglas anteriores, AKS reinicia el nodo después de 10 minutos consecutivos incorrectos. Si los nodos siguen siendo incorrectos después de la operación de reparación inicial, los ingenieros de AKS investigan las correcciones adicionales.
  
Si hay varios nodos incorrectos durante una comprobación de estado, cada nodo se repara individualmente antes de que comience otra reparación.

## <a name="next-steps"></a>Pasos siguientes

Use [Availability Zones][availability-zones] para aumentar la alta disponibilidad con las cargas de trabajo del clúster de AKS.

<!-- LINKS - External -->

<!-- LINKS - Internal -->
[availability-zones]: ./availability-zones.md
[vm-updates]: ../virtual-machines/maintenance-and-updates.md

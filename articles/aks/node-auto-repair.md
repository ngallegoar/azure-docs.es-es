---
title: Reparación automática de nodos de Azure Kubernetes Service (AKS)
description: Obtenga información sobre la funcionalidad de reparación automática de nodos y cómo AKS corrige los nodos de trabajo interrumpidos.
services: container-service
ms.topic: conceptual
ms.date: 08/24/2020
ms.openlocfilehash: 781a1ffebb40b0cce9f18699d308db90633e8626
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "89490112"
---
# <a name="azure-kubernetes-service-aks-node-auto-repair"></a>Reparación automática de nodos de Azure Kubernetes Service (AKS)

AKS comprueba de forma continua el estado de mantenimiento de los nodos de trabajo y, si son incorrectos, los repara automáticamente. Este documento informa a los operadores sobre cómo se comporta la funcionalidad de reparación automática del nodo para nodos Windows y Linux. Además de las reparaciones de AKS, la plataforma de VM de Azure [realiza el mantenimiento en máquinas virtuales][vm-updates] que también experimentan problemas. AKS y las VM de Azure trabajan de forma conjunta para minimizar las interrupciones del servicio de los clústeres.

## <a name="how-aks-checks-for-unhealthy-nodes"></a>Comprobaciones de nodos en mal estado en AKS

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

Si un nodo es incorrecto de acuerdo con las reglas anteriores y se mantiene en este estado durante 10 minutos consecutivos, se realizan las siguientes acciones.

1. Reinicio del nodo
1. Si el reinicio no se realiza correctamente, restablezca la imagen inicial del nodo.
1. Si la nueva imagen no se ha realizado correctamente, cree y vuelva a restablecer la imagen de un nuevo nodo.

Si ninguna de las acciones se realiza correctamente, los ingenieros de AKS investigarán las correcciones adicionales. Si hay varios nodos incorrectos durante una comprobación de estado, cada nodo se repara individualmente antes de que comience otra reparación.

## <a name="next-steps"></a>Pasos siguientes

Use [Availability Zones][availability-zones] para aumentar la alta disponibilidad con las cargas de trabajo del clúster de AKS.

<!-- LINKS - External -->

<!-- LINKS - Internal -->
[availability-zones]: ./availability-zones.md
[vm-updates]: ../virtual-machines/maintenance-and-updates.md

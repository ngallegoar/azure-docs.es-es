---
title: Segmentos de red de VMware HCX
description: Hay cuatro redes necesarias para VMware HCX.
ms.topic: include
ms.date: 09/28/2020
ms.openlocfilehash: f01aba13d688dfed573acfc8c4344511fbfd9fa8
ms.sourcegitcommit: a422b86148cba668c7332e15480c5995ad72fa76
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/30/2020
ms.locfileid: "91578869"
---
<!-- Used in avs-production-ready-deployment.md and tutorial-deploy-vmware-hcx.md -->

Hay cuatro redes necesarias para VMware HCX:

- **Red de administración:** normalmente, es la misma red de administración que se usa en el clúster de vSphere.  Como mínimo, identifique dos direcciones IP en este segmento de red para VMware HCX (es posible que se necesite un número mayor en función de la implementación).

- **Red de vMotion:** normalmente, es la misma red de que se usa para vMotion en el clúster de vSphere.  Como mínimo, identifique dos direcciones IP en este segmento de red para VMware HCX (es posible que se necesite un número mayor en función de la implementación).  

   La red de vMotion debe exponerse en un conmutador virtual distribuido o vSwitch0. Si no es así, modifique el entorno.

   > [!NOTE]
   > Es correcto que si esta red no está enrutada (privada).

- **Red de vínculo superior:** desea crear una nueva red para el vínculo superior de VMware HCX y extenderla al clúster de vSphere a través de un grupo de puertos.  Como mínimo, identifique dos direcciones IP en este segmento de red para VMware HCX (es posible que se necesite un número mayor en función de la implementación).  

   > [!NOTE]
   > El método recomendado es crear una red /29, pero servirá cualquier tamaño de red.

- **Red de replicación:** desea crear una nueva red para la replicación de VMware HCX y extender esa red al clúster de vSphere a través de un grupo de puertos.  Como mínimo, identifique dos direcciones IP en este segmento de red para VMware HCX (es posible que se necesite un número mayor en función de la implementación).

   > [!NOTE]
   > El método recomendado es crear una red /29, pero servirá cualquier tamaño de red.
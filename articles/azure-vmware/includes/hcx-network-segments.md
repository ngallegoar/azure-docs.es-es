---
title: Segmentos de red de VMware HCX
description: Hay cuatro redes necesarias para VMware HCX.
ms.topic: include
ms.date: 11/23/2020
ms.openlocfilehash: 48894c532c97b70cde1473fb8b81f406ded70343
ms.sourcegitcommit: a43a59e44c14d349d597c3d2fd2bc779989c71d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/25/2020
ms.locfileid: "95999642"
---
<!-- Used in avs-production-ready-deployment.md and tutorial-deploy-vmware-hcx.md -->

Se necesitan cuatro redes para VMware HCX:

- **Red de administración:** normalmente, es la misma red de administración que se usa en el clúster de vSphere. Como mínimo, identifique dos direcciones IP en este segmento de red para VMware HCX. Es posible que necesite un número mayor en función de la implementación.

   > [!NOTE]
   > El método recomendado consiste en crear una red /26. En una red /26, puede usar hasta 10 mallas de servicio y 60 extensores de red (-1 por cada malla de servicio). Se pueden expandir ocho redes por cada extensor de red mediante nubes privadas de soluciones de Azure VMware Solution.
   >
   
- **Red de vMotion:** normalmente, es la misma red de que se usa para vMotion en el clúster de vSphere.  Como mínimo, identifique dos direcciones IP en este segmento de red para VMware HCX. Es posible que necesite un número mayor en función de la implementación.  

   La red de vMotion debe exponerse en un conmutador virtual distribuido o vSwitch0. Si no es así, modifique el entorno.

   > [!NOTE]
   > Esta red puede ser privada (no enrutada).

- **Red de vínculo superior:** desea crear una nueva red para el vínculo superior de VMware HCX y extenderla al clúster de vSphere a través de un grupo de puertos. Como mínimo, identifique dos direcciones IP en este segmento de red para VMware HCX. Es posible que necesite un número mayor en función de la implementación.  

   > [!NOTE]
   > El método recomendado consiste en crear una red /26. En una red /26, puede usar hasta 10 mallas de servicio y 60 extensores de red (-1 por cada malla de servicio). Se pueden expandir ocho redes por cada extensor de red mediante nubes privadas de soluciones de Azure VMware Solution.
   >
   
- **Red de replicación:** Esto es opcional. desea crear una nueva red para la replicación de VMware HCX y extender esa red al clúster de vSphere a través de un grupo de puertos. Como mínimo, identifique dos direcciones IP en este segmento de red para VMware HCX. Es posible que necesite un número mayor en función de la implementación.

   > [!NOTE]
   > Esta configuración solo es posible cuando los hosts del clúster local usan una red de VMkernel de replicación dedicada.  Si el clúster local no tiene definida una red de VMkernel de replicación dedicada, no es necesario crear la red.

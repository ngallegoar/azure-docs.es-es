---
title: Segmentos de red de VMware HCX
description: Hay cuatro redes necesarias para VMware HCX.
ms.topic: include
ms.date: 09/28/2020
ms.openlocfilehash: 8137b4383d2a243d53db317db6f5a78b3bc68e67
ms.sourcegitcommit: 2989396c328c70832dcadc8f435270522c113229
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/19/2020
ms.locfileid: "92173616"
---
<!-- Used in avs-production-ready-deployment.md and tutorial-deploy-vmware-hcx.md -->

Se necesitan cuatro redes para VMware HCX:

- **Red de administración:** normalmente, es la misma red de administración que se usa en el clúster de vSphere. Como mínimo, identifique dos direcciones IP en este segmento de red para VMware HCX. Es posible que necesite más, en función de la implementación.

- **Red de vMotion:** normalmente, es la misma red de que se usa para vMotion en el clúster de vSphere.  Como mínimo, identifique dos direcciones IP en este segmento de red para VMware HCX. Es posible que necesite más, en función de la implementación.  

   La red de vMotion debe exponerse en un conmutador virtual distribuido o vSwitch0. Si no es así, modifique el entorno.

   > [!NOTE]
   > Es correcto si esta red no está enrutada (privada).

- **Red de vínculo superior:** desea crear una nueva red para el vínculo superior de VMware HCX y extenderla al clúster de vSphere a través de un grupo de puertos. Como mínimo, identifique dos direcciones IP en este segmento de red para VMware HCX. Es posible que necesite más, en función de la implementación.  

   > [!NOTE]
   > El método recomendado es crear una red /29, pero servirá cualquier tamaño de red.

- **Red de replicación:** desea crear una nueva red para la replicación de VMware HCX y extender esa red al clúster de vSphere a través de un grupo de puertos. Como mínimo, identifique dos direcciones IP en este segmento de red para VMware HCX. Es posible que necesite más, en función de la implementación.

   > [!NOTE]
   > El método recomendado es crear una red /29, pero servirá cualquier tamaño de red.
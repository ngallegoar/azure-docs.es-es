---
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 10/20/2020
ms.author: dacoulte
ms.custom: generated
ms.openlocfilehash: dc886c780a5400128f0e7396743c71e0584676cb
ms.sourcegitcommit: ce8eecb3e966c08ae368fafb69eaeb00e76da57e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/21/2020
ms.locfileid: "92311550"
---
|Nombre |Descripción |Directivas |Versión |
|---|---|---|---|
|[Habilitar Azure Monitor para conjunto de escalado de máquinas virtuales](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policySetDefinitions/Monitoring/AzureMonitor_VMSS.json) |Se habilita Azure Monitor para los conjuntos de escalado de máquinas virtuales en el ámbito especificado (grupo de administración, suscripción o grupo de recursos). Usa el área de trabajo de Log Analytics como parámetro. Nota: Si establece upgradePolicy del conjunto de escalado en Manual, debe aplicar la extensión a todas las máquinas virtuales del conjunto mediante una llamada a la actualización. En la CLI, se usaría az vmss update-instances. |6 |1.0.1 |
|[Habilitar Azure Monitor para VM](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policySetDefinitions/Monitoring/AzureMonitor_VM.json) |Se habilita Azure Monitor para las máquinas virtuales en el ámbito especificado (grupo de administración, suscripción o grupo de recursos). Usa el área de trabajo de Log Analytics como parámetro. |10 |2.0.0 |

---
title: archivo de inclusión
description: archivo de inclusión
services: lighthouse
author: JnHs
ms.service: lighthouse
ms.topic: include
ms.date: 10/12/2020
ms.author: jenhayes
ms.custom: include file
ms.openlocfilehash: 20d80fe1a09d388552b6289f8a9b23c878672f94
ms.sourcegitcommit: d103a93e7ef2dde1298f04e307920378a87e982a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/13/2020
ms.locfileid: "91974299"
---
En estos ejemplos se muestra cómo usar Azure Policy con las suscripciones que se han incorporado a Azure Lighthouse.

| **Plantilla** | **Descripción** |
|---------|---------|
| [policy-add-or-replace-tag](https://github.com/Azure/Azure-Lighthouse-samples/tree/master/templates/policy-add-or-replace-tag) | Asigna una directiva que agrega o elimina una etiqueta (mediante el efecto Modify) en una suscripción delegada. Para más información, consulte [Deploy a policy that can be remediated within a delegated subscription](../articles/lighthouse/how-to/deploy-policy-remediation.md) (Implementación de una directiva que se pueda corregir en una suscripción delegada). |
| [policy-allow-certain-managing-tenants](https://github.com/Azure/Azure-Lighthouse-samples/tree/master/templates/policy-allow-certain-managing-tenants) | Asigna una directiva que restringe las delegaciones de Azure Lighthouse a inquilinos de administración específicos. |
| [policy-audit-delegation](https://github.com/Azure/Azure-Lighthouse-samples/tree/master/templates/policy-audit-delegation) | Asigna una directiva que auditará las asignaciones de delegaciones. |
| [policy-delegate-management-groups](https://github.com/Azure/Azure-Lighthouse-samples/tree/master/templates/policy-delegate-management-groups) | Asigna una directiva para confirmar que las suscripciones de un grupo de administración se han delegado a un inquilino de administración y, si no se han delegado, crea la asignación.
| [policy-enforce-keyvault-monitoring](https://github.com/Azure/Azure-Lighthouse-samples/tree/master/templates/policy-enforce-keyvault-monitoring) | Asigna una directiva que habilita el diagnóstico en los recursos de Azure Key Vault de una suscripción delegada (mediante el efecto deployIfNotExists). Para más información, consulte [Deploy a policy that can be remediated within a delegated subscription](../articles/lighthouse/how-to/deploy-policy-remediation.md) (Implementación de una directiva que se pueda corregir en una suscripción delegada). |
| [policy-enforce-sub-monitoring](https://github.com/Azure/Azure-Lighthouse-samples/tree/master/templates/policy-enforce-sub-monitoring) | Asigna varias directivas para habilitar los diagnósticos en una suscripción delegada y conecta todas las máquinas virtuales Windows y Linux al área de trabajo de Log Analytics que la directiva crea. Para más información, consulte [Deploy a policy that can be remediated within a delegated subscription](../articles/lighthouse/how-to/deploy-policy-remediation.md) (Implementación de una directiva que se pueda corregir en una suscripción delegada). |
| [policy-initiative](https://github.com/Azure/Azure-Lighthouse-samples/tree/master/templates/policy-initiative) | Aplica una iniciativa (varias definiciones de directivas relacionadas) a una suscripción delegada. |


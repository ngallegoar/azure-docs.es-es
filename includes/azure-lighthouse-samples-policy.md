---
title: archivo de inclusión
description: archivo de inclusión
services: lighthouse
author: JnHs
ms.service: lighthouse
ms.topic: include
ms.date: 12/19/2019
ms.author: jenhayes
ms.custom: include file
ms.openlocfilehash: 1c0be8ed8c176fe32b8fe7fd420d65727c5288d2
ms.sourcegitcommit: 7d8158fcdcc25107dfda98a355bf4ee6343c0f5c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/09/2020
ms.locfileid: "80986639"
---
En estos ejemplos se muestra cómo usar Azure Policy con las suscripciones que se han incorporado a la administración de recursos delegados de Azure.

| **Plantilla** | **Descripción** |
|---------|---------|
| [policy-add-or-replace-tag](https://github.com/Azure/Azure-Lighthouse-samples/tree/master/templates/policy-add-or-replace-tag) | Asigna una directiva que agrega o elimina una etiqueta (mediante el efecto Modify) en una suscripción delegada. Para más información, consulte [Deploy a policy that can be remediated within a delegated subscription](../articles/lighthouse/how-to/deploy-policy-remediation.md) (Implementación de una directiva que se pueda corregir en una suscripción delegada). |
| [policy-audit-delegation](https://github.com/Azure/Azure-Lighthouse-samples/tree/master/templates/policy-audit-delegation) | Asigna una directiva que auditará las asignaciones de delegaciones. |
| [policy-enforce-keyvault-monitoring](https://github.com/Azure/Azure-Lighthouse-samples/tree/master/templates/policy-enforce-keyvault-monitoring) | Asigna una directiva que habilita el diagnóstico en los recursos de Azure Key Vault de una suscripción delegada (mediante el efecto deployIfNotExists). Para más información, consulte [Deploy a policy that can be remediated within a delegated subscription](../articles/lighthouse/how-to/deploy-policy-remediation.md) (Implementación de una directiva que se pueda corregir en una suscripción delegada). |
| [policy-enforce-sub-monitoring](https://github.com/Azure/Azure-Lighthouse-samples/tree/master/templates/policy-enforce-sub-monitoring) | Asigna varias directivas para habilitar los diagnósticos en una suscripción delegada y conecta todas las máquinas virtuales Windows y Linux al área de trabajo de Log Analytics que la directiva crea. Para más información, consulte [Deploy a policy that can be remediated within a delegated subscription](../articles/lighthouse/how-to/deploy-policy-remediation.md) (Implementación de una directiva que se pueda corregir en una suscripción delegada). |
| [policy-initiative](https://github.com/Azure/Azure-Lighthouse-samples/tree/master/templates/policy-initiative) | Aplica una iniciativa (varias definiciones de directivas relacionadas) a una suscripción delegada. |


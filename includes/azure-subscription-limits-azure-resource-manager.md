---
title: archivo de inclusión
description: archivo de inclusión
services: azure-resource-manager
author: tfitzmac
ms.service: cost-management-billing
ms.topic: include
ms.date: 03/26/2020
ms.author: tomfitz
ms.custom: include file
ms.openlocfilehash: 94fd7e692be31ba247e3342246d3940ed08ef9b7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "80334987"
---
| Resource | Límite |
| --- | --- |
| Suscripciones por inquilino de Azure Active Directory | Sin límite. |
| [Coadministradores](../articles/cost-management-billing/manage/add-change-subscription-administrator.md) por suscripción |Sin límite. |
| [Grupos de recursos](../articles/azure-resource-manager/management/overview.md) por suscripción |980 |
| Tamaño de solicitud de API de Azure Resource Manager |4 194 304 bytes. |
| Etiquetas por suscripción<sup>1</sup> |50 |
| Cálculos de etiquetas únicas por suscripción<sup>1</sup> | 10 000 |
| [Implementaciones de nivel de suscripción](../articles/azure-resource-manager/templates/deploy-to-subscription.md) por ubicación | 800<sup>2</sup> |

<sup>1</sup>Puede aplicar hasta 50 etiquetas directamente a una suscripción. Sin embargo, la suscripción puede contener un número ilimitado de etiquetas que se aplican a los grupos de recursos y recursos de la misma. El número de etiquetas por recurso o grupo de recursos se limita a 50. Resource Manager devuelve una [lista de valores y nombres de etiqueta únicos](/rest/api/resources/tags) para la suscripción solo cuando el número de etiquetas es 10 000 o menos. Sin embargo, todavía puede encontrar un recurso por etiqueta cuando el número sea mayor que 10 000.  

<sup>2</sup>Si se alcanza el límite de 800 implementaciones, elimine las implementaciones que ya no necesite del historial. Para eliminar implementaciones de nivel de suscripción, use [Remove-AzDeployment](/powershell/module/az.resources/Remove-AzDeployment) o [az deployment sub delete](/cli/azure/deployment/sub?view=azure-cli-latest#az-deployment-sub-delete).

---
title: archivo de inclusión
description: archivo de inclusión
author: tfitzmac
ms.service: governance
ms.topic: include
ms.date: 03/26/2020
ms.author: tomfitz
ms.custom: include file
ms.openlocfilehash: 9b2bf8763d175bca028be1fcceda921e87eb5b23
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "80334611"
---
| Resource | Límite |
| --- | --- |
| Grupos de administración por directorio | 10 000 |
| Suscripciones por grupo de administración | Sin límite. |
| Niveles de jerarquía de grupos de administración | Nivel raíz más 6 niveles<sup>1</sup> |
| Grupo de administración primario directo por grupo de administración | Uno |
| [Implementaciones de nivel de grupo de administración](../articles/azure-resource-manager/templates/deploy-to-management-group.md) por ubicación | 800<sup>2</sup> |

<sup>1</sup>Los 6 niveles no incluyen el nivel de suscripción.

<sup>2</sup>Si se alcanza el límite de 800 implementaciones, elimine las implementaciones que ya no necesite del historial. Para eliminar las implementaciones de nivel de grupo de administración, use [Remove-AzManagementGroupDeployment](/powershell/module/az.resources/Remove-AzManagementGroupDeployment) o [az deployment mg delete](/cli/azure/deployment/mg?view=azure-cli-latest#az-deployment-mg-delete).

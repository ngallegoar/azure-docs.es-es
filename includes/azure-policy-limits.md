---
title: archivo de inclusión
description: archivo de inclusión
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 04/23/2020
ms.author: dacoulte
ms.openlocfilehash: e9faea1d5913a19dfdeff662e26992529dc1b22d
ms.sourcegitcommit: ba8df8424d73c8c4ac43602678dae4273af8b336
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/05/2020
ms.locfileid: "84466950"
---
Hay un número máximo de cada tipo de objeto de Azure Policy. Una entrada de _Scope_ significa la suscripción o el [grupo de administración](../articles/governance/management-groups/overview.md).

| Where | Qué | Número máximo |
|---|---|---|
| Ámbito | Definiciones de directiva | 500 |
| Ámbito | Definiciones de iniciativa | 100 |
| Inquilino | Definiciones de iniciativa | 2500 |
| Ámbito | Asignaciones de iniciativas o directivas | 100 |
| Definición de directiva | Parámetros | 20 |
| Definición de iniciativa | Directivas | 100 |
| Definición de iniciativa | Parámetros | 100 |
| Asignaciones de iniciativas o directivas | Exclusiones (notScopes) | 400 |
| Regla de directiva | Condicionales anidados | 512 |
| Tarea de corrección | Recursos | 500 |

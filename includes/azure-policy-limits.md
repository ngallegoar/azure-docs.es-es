---
title: archivo de inclusión
description: archivo de inclusión
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 09/30/2020
ms.author: dacoulte
ms.openlocfilehash: f3f706789e14cb20214bf17fd91f6ec1e503848f
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "91605768"
---
Hay un número máximo de cada tipo de objeto de Azure Policy. Para las definiciones, una entrada de _Scope_ significa el [grupo de administración](../articles/governance/management-groups/overview.md) o la suscripción.
En el caso de las asignaciones y exenciones, una entrada de _Scope_ significa el [grupo de administración](../articles/governance/management-groups/overview.md), la suscripción, el grupo de recursos o el recurso individual.

| Where | Qué | Número máximo |
|---|---|---|
| Ámbito | Definiciones de directiva | 500 |
| Ámbito | Definiciones de iniciativa | 200 |
| Inquilino | Definiciones de iniciativa | 2,500 |
| Ámbito | Asignaciones de iniciativas o directivas | 200 |
| Ámbito | Exenciones | 1000 |
| Definición de directiva | Parámetros | 20 |
| Definición de iniciativa | Directivas | 1000 |
| Definición de iniciativa | Parámetros | 100 |
| Asignaciones de iniciativas o directivas | Exclusiones (notScopes) | 400 |
| Regla de directiva | Condicionales anidados | 512 |
| Tarea de corrección | Recursos | 500 |

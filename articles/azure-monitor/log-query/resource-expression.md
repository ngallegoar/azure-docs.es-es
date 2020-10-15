---
title: Expresión resource() en consultas de registro de Azure Monitor | Microsoft Docs
description: La expresión resource se usa en una consulta de registro de Azure Monitor centrada en recursos para recuperar datos de varios recursos.
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 09/10/2018
ms.openlocfilehash: 2a729caefe698b13833098ba48df9d4bfbd97356
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "77665706"
---
# <a name="resource-expression-in-azure-monitor-log-query"></a>Expresión resource() en consultas de registro de Azure Monitor

La expresión `resource` se usa en consulta de Azure Monitor [con ámbito en un recurso](scope.md#query-scope) para recuperar datos de otros recursos. 


## <a name="syntax"></a>Sintaxis

`resource(`*Identificador*`)`

## <a name="arguments"></a>Argumentos

- *Identificador*: Identificador de recurso de un recurso.

| Identificador | Descripción | Ejemplo
|:---|:---|:---|
| Resource | Incluye datos del recurso. | resource("/subscriptions/xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourcesgroups/myresourcegroup/providers/microsoft.compute/virtualmachines/myvm") |
| Grupo de recursos o suscripción | Incluye datos del recurso y todos los recursos que contiene.  | resource("/subscriptions/xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourcesgroups/myresourcegroup) |


## <a name="notes"></a>Notas

* Debe tener acceso de lectura al recurso.


## <a name="examples"></a>Ejemplos

```Kusto
union (Heartbeat),(resource("/subscriptions/xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourcesgroups/myresourcegroup/providers/microsoft.compute/virtualmachines/myvm").Heartbeat) | summarize count() by _ResourceId, TenantId
```
```Kusto
union (Heartbeat),(resource("/subscriptions/xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourcesgroups/myresourcegroup).Heartbeat) | summarize count() by _ResourceId, TenantId
```


## <a name="next-steps"></a>Pasos siguientes

- Consulte [Ámbito e intervalo de tiempo de una consulta de registro en Log Analytics de Azure Monitor](scope.md) para obtener información sobre un ámbito de consulta.
- Acceda a toda la documentación del [lenguaje de consulta de Kusto](/azure/kusto/query/).

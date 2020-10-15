---
title: Expresión workspace() en consultas de registro de Azure Monitor | Microsoft Docs
description: La expresión workspace se usa en una consulta de registro de Azure Monitor para recuperar datos de un área de trabajo específica en el mismo grupo de recursos, en otro grupo de recursos o en otra suscripción.
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 09/10/2018
ms.openlocfilehash: 255888acf5da6149b6a964b23ed038b99715481c
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "75364958"
---
# <a name="workspace-expression-in-azure-monitor-log-query"></a>Expresión workspace() en consultas de registro de Azure Monitor

La expresión `workspace` se usa en una consulta de Azure Monitor para recuperar datos de un área de trabajo específica en el mismo grupo de recursos, en otro grupo de recursos o en otra suscripción. Es útil para incluir datos de registro en una consulta de Application Insights y para consultar datos a través de varias áreas de trabajo en una consulta de registro.


## <a name="syntax"></a>Sintaxis

`workspace(`*Identificador*`)`

## <a name="arguments"></a>Argumentos

- *Identificador*: identifica el área de trabajo mediante uno de los formatos de la tabla siguiente.

| Identificador | Descripción | Ejemplo
|:---|:---|:---|
| Nombre de recurso | Nombre legible del área de trabajo (también denominado "nombre del componente") | workspace("contosoretail") |
| Nombre completo | Nombre completo del área de trabajo con el siguiente formato: "subscriptionName/resourceGroup/componentName" | workspace("Contoso/ContosoResource/ContosoWorkspace") |
| id | GUID del área de trabajo | workspace("b438b3f6-912a-46d5-9db1-b42069242ab4") |
| Id. de recurso de Azure | Identificador del recurso de Azure | workspace("/subscriptions/e4227-645-44e-9c67-3b84b5982/resourcegroups/ContosoAzureHQ/providers/Microsoft.OperationalInsights/workspaces/contosoretail") |


## <a name="notes"></a>Notas

* Debe tener acceso de lectura al área de trabajo.
* Una expresión relacionada es `app`, que le permite realizar consultas en todas las aplicaciones de Application Insights.

## <a name="examples"></a>Ejemplos

```Kusto
workspace("contosoretail").Update | count
```
```Kusto
workspace("b438b4f6-912a-46d5-9cb1-b44069212ab4").Update | count
```
```Kusto
workspace("/subscriptions/e427267-5645-4c4e-9c67-3b84b59a6982/resourcegroups/ContosoAzureHQ/providers/Microsoft.OperationalInsights/workspaces/contosoretail").Event | count
```
```Kusto
union 
(workspace("myworkspace").Heartbeat | where Computer contains "Con"),
(app("myapplication").requests | where cloud_RoleInstance contains "Con")
| count  
```
```Kusto
union 
(workspace("myworkspace").Heartbeat), (app("myapplication").requests)
| where TimeGenerated between(todatetime("2018-02-08 15:00:00") .. todatetime("2018-12-08 15:05:00"))
```

## <a name="next-steps"></a>Pasos siguientes

- Consulte la [expresión de aplicación](app-expression.md) para hacer referencia a una aplicación de Application Insights.
- Obtenga información sobre cómo se almacenan los [datos de Azure Monitor](log-query-overview.md).
- Acceda a toda la documentación del [lenguaje de consulta de Kusto](/azure/kusto/query/).

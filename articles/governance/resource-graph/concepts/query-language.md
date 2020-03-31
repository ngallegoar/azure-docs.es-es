---
title: Descripción del lenguaje de consultas
description: Describe las tablas de Resource Graph y los tipos de datos, los operadores y las funciones de Kusto disponibles que se pueden usar con Azure Resource Graph.
ms.date: 03/07/2020
ms.topic: conceptual
ms.openlocfilehash: 2f4be4d86a340867e1ad3015ff288f98fc54cecf
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "78927498"
---
# <a name="understanding-the-azure-resource-graph-query-language"></a>Información del lenguaje de consulta de Azure Resource Graph

El lenguaje de consulta de Azure Resource Graph admite numerosos operadores y funciones. Cada uno funciona en función del [lenguaje de consulta de Kusto (KQL)](/azure/kusto/query/index). Para información sobre el lenguaje de consulta usado por Resource Graph, comience con el [tutorial para KQL](/azure/kusto/query/tutorial).

En este artículo se tratan los componentes de idioma admitidos por Resource Graph:

- [Tablas de Resource Graph](#resource-graph-tables)
- [Elementos del lenguaje KQL admitidos](#supported-kql-language-elements)
- [Caracteres de escape](#escape-characters)

## <a name="resource-graph-tables"></a>Tablas de Resource Graph

Resource Graph proporciona varias tablas para los datos que almacena sobre los tipos de recursos de Resource Manager y sus propiedades. Estas tablas se pueden utilizar con los operadores `join` o `union` para obtener propiedades de tipos de recursos relacionados. Esta es la lista de tablas disponibles en Resource Graph:

|Tablas de Resource Graph |Descripción |
|---|---|
|Recursos |La tabla predeterminada, si no se ha definido ninguna en la consulta. La mayoría de los tipos de recursos y propiedades de Resource Manager están aquí. |
|ResourceContainers |Incluye los datos y los tipos de recursos de suscripción (en versión preliminar: `Microsoft.Resources/subscriptions`) y grupo de recursos (`Microsoft.Resources/subscriptions/resourcegroups`). |
|AdvisorResources |Incluye recursos _relacionados_ con `Microsoft.Advisor`. |
|AlertsManagementResources |Incluye recursos _relacionados_ con `Microsoft.AlertsManagement`. |
|MaintenanceResources |Incluye recursos _relacionados_ con `Microsoft.Maintenance`. |
|SecurityResources |Incluye recursos _relacionados_ con `Microsoft.Security`. |

Para obtener una lista completa, incluidos los tipos de recursos, consulte [Referencia: tablas y tipos de recursos admitidos](../reference/supported-tables-resources.md).

> [!NOTE]
> _Recursos_ es la tabla predeterminada. Al consultar la tabla _Recursos_, no es necesario proporcionar el nombre de la tabla a menos que se utilicen `join` o `union`. Sin embargo, el procedimiento recomendado es incluir siempre la tabla inicial en la consulta.

Use Azure Resource Graph en el portal para detectar qué tipos de recursos están disponibles en cada tabla. Como alternativa, utilice una consulta como `<tableName> | distinct type` para obtener una lista de los tipos de recursos que admite la tabla de Resource Graph que existe en su entorno.

En la consulta siguiente se muestra un uso sencillo de `join`. El resultado de la consulta combina las columnas y los nombres de columna duplicados de la tabla combinada, _ResourceContainers_ en este ejemplo, se anexan con **1**. Como la tabla _ResourceContainers_ tiene tipos para suscripciones y grupos de recursos, se puede usar cualquier tipo para unirse al recurso desde la tabla _recursos_.

```kusto
Resources
| join ResourceContainers on subscriptionId
| limit 1
```

En la consulta siguiente se muestra un uso más complejo de `join`. La consulta limita la tabla combinada a los recursos de suscripciones y con `project` para incluir solo el campo original _subscriptionId_ y el campo _name_ con el nombre cambiado a _SubName_. El cambio de nombre del campo evita `join` se agregue como _name1_, ya que el campo ya existe en _Resources_ (Recursos). La tabla original se filtra por `where` y el `project` siguiente incluye columnas de ambas tablas. El resultado de la consulta es un solo almacén de claves que muestra su tipo, su nombre y el nombre de la suscripción en que se encuentra.

```kusto
Resources
| where type == 'microsoft.keyvault/vaults'
| join (ResourceContainers | where type=='microsoft.resources/subscriptions' | project SubName=name, subscriptionId) on subscriptionId
| project type, name, SubName
| limit 1
```

> [!NOTE]
> Al limitar los resultados de `join` con `project`, la propiedad utilizada por `join` para relacionar las dos tablas, _subscriptionId_ en el ejemplo anterior, debe incluirse en `project`.

## <a name="supported-kql-language-elements"></a>Elementos del lenguaje KQL admitidos

Resource Graph admite todos los [tipos de datos](/azure/kusto/query/scalar-data-types/), [funciones escalares](/azure/kusto/query/scalarfunctions), [operadores escalares](/azure/kusto/query/binoperators) y [funciones de agregado](/azure/kusto/query/any-aggfunction) de KQL. Resource Graph admite [operadores tabulares](/azure/kusto/query/queries) específicos, algunos de los cuales tienen comportamientos distintos.

### <a name="supported-tabulartop-level-operators"></a>Operadores de nivel superior o tabulares compatibles

Esta es la lista de operadores tabulares de KQL admitidos por Resource Graph con ejemplos específicos:

|KQL |Consulta de ejemplo de Resource Graph |Notas |
|---|---|---|
|[count](/azure/kusto/query/countoperator) |[Contador de almacenes de claves](../samples/starter.md#count-keyvaults) | |
|[distinct](/azure/kusto/query/distinctoperator) |[Show distinct values for a specific alias](../samples/starter.md#distinct-alias-values) | |
|[extend](/azure/kusto/query/extendoperator) |[Count virtual machines by OS type](../samples/starter.md#count-os) | |
|[join](/azure/kusto/query/joinoperator) |[Almacén de claves con el nombre de la suscripción](../samples/advanced.md#join) |Tipos de combinación admitidos: [innerunique](/azure/kusto/query/joinoperator#default-join-flavor), [inner](/azure/kusto/query/joinoperator#inner-join), [leftouter](/azure/kusto/query/joinoperator#left-outer-join). Límite de 3 `join` en una sola consulta. No se permiten las estrategias de combinación personalizadas, como la combinación de difusión. Se puede usar en una sola tabla o entre las tablas _Resources_ y _ResourceContainers_. |
|[limit](/azure/kusto/query/limitoperator) |[List all public IP addresses](../samples/starter.md#list-publicip) |Sinónimo de `take` |
|[mvexpand](/azure/kusto/query/mvexpandoperator) | | Operador heredado, use `mv-expand` en su lugar. Máximo de _RowLimit_ de 400. El valor predeterminado es 128. |
|[mv-expand](/azure/kusto/query/mvexpandoperator) |[Enumeración de Cosmos DB con ubicaciones de escritura concretas](../samples/advanced.md#mvexpand-cosmosdb) |Máximo de _RowLimit_ de 400. El valor predeterminado es 128. |
|[order](/azure/kusto/query/orderoperator) |[List resources sorted by name](../samples/starter.md#list-resources) |Sinónimo de `sort` |
|[project](/azure/kusto/query/projectoperator) |[List resources sorted by name](../samples/starter.md#list-resources) | |
|[project-away](/azure/kusto/query/projectawayoperator) |[Eliminación de columnas de los resultados](../samples/advanced.md#remove-column) | |
|[sort](/azure/kusto/query/sortoperator) |[List resources sorted by name](../samples/starter.md#list-resources) |Sinónimo de `order` |
|[summarize](/azure/kusto/query/summarizeoperator) |[Count Azure resources](../samples/starter.md#count-resources) |Solo la primera página simplificada |
|[take](/azure/kusto/query/takeoperator) |[List all public IP addresses](../samples/starter.md#list-publicip) |Sinónimo de `limit` |
|[top](/azure/kusto/query/topoperator) |[Show first five virtual machines by name and their OS type](../samples/starter.md#show-sorted) | |
|[union](/azure/kusto/query/unionoperator) |[Combinación de los resultados de dos consultas para formar un solo resultado](../samples/advanced.md#unionresults) |Tabla única permitida: _T_ `| union` \[`kind=` `inner`\|`outer`\] \[`withsource=`_nombreDeColumna_\] _tabla_. Límite de 3 partes de `union` en una sola consulta. No se permite la resolución aproximada de tablas de segmento `union`. Se puede usar en una sola tabla o entre las tablas _Resources_ y _ResourceContainers_. |
|[where](/azure/kusto/query/whereoperator) |[Show resources that contain storage](../samples/starter.md#show-storage) | |

## <a name="escape-characters"></a>Carácter de escape

Algunos nombres de propiedad, como los que incluyen `.` o `$`, debe encapsularse o escapar en la consulta para que se interprete correctamente; de lo contrario, no proporcionan los resultados previstos.

- `.` -Encapsule el nombre de propiedad como en: `['propertyname.withaperiod']`
  
  Consulta de ejemplo que encapsula la propiedad _odata.type_:

  ```kusto
  where type=~'Microsoft.Insights/alertRules' | project name, properties.condition.['odata.type']
  ```

- `$` -Incluya un carácter de escape en el nombre de propiedad. El carácter de escape utilizado depende el shell desde donde se ejecute Resource Graph.

  - **bash** - `\`

    Consulta de ejemplo con escape de la propiedad _\$type_ en bash:

    ```kusto
    where type=~'Microsoft.Insights/alertRules' | project name, properties.condition.\$type
    ```

  - **cmd** -no escapa el carácter `$`.

  - **PowerShell** - ``` ` ```

    Consulta de ejemplo con escape de la propiedad _\$type_ en PowerShell:

    ```kusto
    where type=~'Microsoft.Insights/alertRules' | project name, properties.condition.`$type
    ```

## <a name="next-steps"></a>Pasos siguientes

- Consulte el lenguaje en uso en[Consultas básicas](../samples/starter.md).
- Consulte los usos avanzados en [Consultas avanzadas](../samples/advanced.md).
- Obtenga más información sobre cómo [explorar recursos](explore-resources.md).
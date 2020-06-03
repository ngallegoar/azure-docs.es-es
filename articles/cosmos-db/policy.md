---
title: Uso de Azure Policy para implementar la gobernanza y los controles de recursos de Azure Cosmos DB
description: Aprenda a usar Azure Policy para implementar la gobernanza y los controles de recursos de Azure Cosmos DB.
author: plzm
ms.author: paelaz
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/20/2020
ms.openlocfilehash: 2249dbdebecc52a8f5d6decccb83d3b1fc0777f7
ms.sourcegitcommit: 493b27fbfd7917c3823a1e4c313d07331d1b732f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/21/2020
ms.locfileid: "83747375"
---
# <a name="use-azure-policy-to-implement-governance-and-controls-for-azure-cosmos-db-resources"></a>Uso de Azure Policy para implementar la gobernanza y los controles de recursos de Azure Cosmos DB

[Azure Policy](../governance/policy/overview.md) ayuda a aplicar estándares de gobernanza de la organización, a valorar el cumplimiento de los recursos y a implementar correcciones automáticas. Algunos casos de uso comunes son la seguridad, la administración de costos y la coherencia de la configuración.

Azure Policy proporciona definiciones de directivas integradas Puede crear definiciones de directivas personalizadas para escenarios en los que no sirvan las definiciones de directivas integradas. Para más información, consulte la [documentación de Azure Policy](../governance/policy/overview.md).

## <a name="assign-a-built-in-policy-definition"></a>Asignación de una definición de directiva integrada

Las definiciones de directivas describen las condiciones de cumplimiento de los recursos y qué sucederá si se cumple una condición. Las _asignaciones_ de directiva se crean a partir de _definiciones_ de directivas. Puede usar definiciones de directivas integradas o personalizadas con los recursos de Azure Cosmos DB. Las asignaciones de directiva se limitan a un grupo de administración de Azure, una suscripción de Azure o un grupo de recursos, y se aplican a los recursos dentro del ámbito seleccionado. Opcionalmente, puede excluir recursos específicos del ámbito.

Puede crear asignaciones de directiva con [Azure Portal](../governance/policy/assign-policy-portal.md), [Azure PowerShell](../governance/policy/assign-policy-powershell.md), la [CLI de Azure](../governance/policy/assign-policy-azurecli.md) o [plantillas de ARM](../governance/policy/assign-policy-template.md).

Para crear una asignación de directiva a partir de una definición de directiva integrada para Azure Cosmos DB, siga los pasos descritos en el artículo [Creación de una asignación de directiva con Azure Portal](../governance/policy/assign-policy-portal.md).

En el paso para seleccionar una definición de directiva, escriba `Cosmos DB` en el campo de búsqueda para filtrar la lista de definiciones de directivas integradas disponibles. Seleccione una de las definiciones de directivas integradas disponibles y, luego, elija **Seleccionar** para continuar con la creación de la asignación de directiva.

> [!TIP]
> También puede usar los nombres de definición de directiva integrada que se muestran en el panel **Definiciones disponibles** con Azure PowerShell, la CLI de Azure o plantillas de ARM para crear asignaciones de directivas.

:::image type="content" source="./media/policy/available-definitions.png" alt-text="Búsqueda de definiciones de directivas integradas de Azure Cosmos DB":::

## <a name="create-a-custom-policy-definition"></a>Creación de una definición de directiva personalizada

En el caso de escenarios específicos en los que no sirvan las directivas integradas, puede crear [una definición de directiva personalizada](../governance/policy/tutorials/create-custom-policy-definition.md). Más adelante creará una _asignación_ de directiva a partir de la _definición_ de directiva personalizada.

### <a name="property-types-and-property-aliases-in-policy-rules"></a>Tipos y alias de propiedades en reglas de directiva

Use los [pasos de definición de directiva personalizada](../governance/policy/tutorials/create-custom-policy-definition.md) para identificar las propiedades de recursos y los alias de propiedad, que son necesarios para crear reglas de directiva.

Para identificar alias de propiedad específicos de Azure Cosmos DB, use el espacio de nombres `Microsoft.DocumentDB` con uno de los métodos que se muestran en el artículo sobre los pasos de definición de directivas personalizadas.

#### <a name="use-the-azure-cli"></a>Uso de la CLI de Azure:
```azurecli-interactive
# Login first with az login if not using Cloud Shell

# Get Azure Policy aliases for namespace Microsoft.DocumentDB
az provider show --namespace Microsoft.DocumentDB --expand "resourceTypes/aliases" --query "resourceTypes[].aliases[].name"
```

#### <a name="use-azure-powershell"></a>Uso de Azure PowerShell:
```azurepowershell-interactive
# Login first with Connect-AzAccount if not using Cloud Shell

# Use Get-AzPolicyAlias to list aliases for Microsoft.DocumentDB namespace
(Get-AzPolicyAlias -NamespaceMatch 'Microsoft.DocumentDB').Aliases
```

Estos comandos generan la lista de nombres de alias de propiedad para propiedades de Azure Cosmos DB. A continuación, se muestra un extracto de la salida:

```json
[
  "Microsoft.DocumentDB/databaseAccounts/sku.name",
  "Microsoft.DocumentDB/databaseAccounts/virtualNetworkRules[*]",
  "Microsoft.DocumentDB/databaseAccounts/virtualNetworkRules[*].id",
  "Microsoft.DocumentDB/databaseAccounts/isVirtualNetworkFilterEnabled",
  "Microsoft.DocumentDB/databaseAccounts/consistencyPolicy.defaultConsistencyLevel",
  "Microsoft.DocumentDB/databaseAccounts/enableAutomaticFailover",
  "Microsoft.DocumentDB/databaseAccounts/Locations",
  "Microsoft.DocumentDB/databaseAccounts/Locations[*]",
  "Microsoft.DocumentDB/databaseAccounts/Locations[*].locationName",
  "..."
]
```

Puede usar cualquiera de estos nombres de alias de propiedad en las [reglas de definición de directiva personalizada](../governance/policy/tutorials/create-custom-policy-definition.md#policy-rule).

El siguiente es un ejemplo de definición de directiva que comprueba si el rendimiento aprovisionado de una base de datos de Azure Cosmos DB es mayor que el límite máximo permitido de 400 RU/s. Una definición de directiva personalizada incluye dos reglas: una para comprobar el tipo específico de alias de propiedad y la otra para la propiedad específica del tipo. Ambas reglas usan los nombres de alias.

```json
"policyRule": {
  "if": {
    "allOf": [
      {
      "field": "type",
      "equals": "Microsoft.DocumentDB/databaseAccounts/sqlDatabases/throughputSettings"
      },
      {
      "field": "Microsoft.DocumentDB/databaseAccounts/sqlDatabases/throughputSettings/default.resource.throughput",
      "greater": 400
      }
    ]
  }
}
```

Se pueden usar definiciones de directivas personalizadas para crear asignaciones de directiva de la misma manera que se usan las definiciones de directivas integradas.

## <a name="policy-compliance"></a>Cumplimiento de directivas

Una vez creadas las asignaciones de directiva, Azure Policy evalúa los recursos en el ámbito de la asignación. Se valora el _cumplimiento_ de cada recurso con la directiva. El _efecto_ especificado en la directiva se aplica luego a los recursos no compatibles.

Puede revisar los resultados de cumplimiento y los detalles de corrección en [Azure Portal](../governance/policy/how-to/get-compliance-data.md#portal), la [CLI de Azure](../governance/policy/how-to/get-compliance-data.md#command-line) o los [registros de Azure Monitor](../governance/policy/how-to/get-compliance-data.md#azure-monitor-logs).

En la captura de pantalla siguiente se muestran dos ejemplos de asignaciones de directiva. Una asignación se basa en una definición de directiva integrada, que comprueba que los recursos de Azure Cosmos DB se implementan solo en las regiones de Azure permitidas. La otra asignación se basa en una definición de directiva personalizada. Esta asignación comprueba que el rendimiento aprovisionado en los recursos de Azure Cosmos DB no supera un límite máximo especificado.

Una vez implementadas las asignaciones de directiva, el panel de cumplimiento muestra los resultados de la evaluación. Tenga en cuenta que esta operación puede tardar hasta 30 minutos después de implementar una asignación de directiva.

En la captura de pantalla se muestran los siguientes resultados de la evaluación de cumplimiento:

- Cero de una cuenta de Azure Cosmos DB del ámbito especificado son compatibles con la asignación de directiva para comprobar que los recursos se implementaron en regiones permitidas.
- Uno de dos recursos de base de datos o colección de Azure Cosmos DB del ámbito especificado es compatible con la asignación de directiva para comprobar que el rendimiento aprovisionado supera el límite máximo especificado.

:::image type="content" source="./media/policy/compliance.png" alt-text="Búsqueda de definiciones de directivas integradas de Azure Cosmos DB":::

Para corregir los recursos no compatibles, consulte el artículo [Corregir los recursos no conformes con Azure Policy](../governance/policy/how-to/remediate-resources.md).

## <a name="next-steps"></a>Pasos siguientes

- [Revisión de definiciones de directivas personalizadas de ejemplo para Azure Cosmos DB](https://github.com/Azure/azure-policy/tree/master/samples/CosmosDB)
- [Creación de una asignación de directiva en Azure Portal](../governance/policy/assign-policy-portal.md)
- [Revisión de las definiciones de directivas integradas de Azure Policy para Azure Cosmos DB](./policy-samples.md)

---
title: Detalles de la estructura de asignaciones de directivas
description: Describe la definición de asignación de directiva utilizada por Azure Policy para relacionar las definiciones de directiva y los parámetros con los recursos para su evaluación.
ms.date: 09/23/2019
ms.topic: conceptual
ms.openlocfilehash: f03c654dfc4c8dfdf2bdc5103a5961b4d8ce1e64
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "79231416"
---
# <a name="azure-policy-assignment-structure"></a>Estructura de asignaciones de Azure Policy

Azure Policy usa las asignaciones de directivas para definir qué recursos se asignan a qué directivas o iniciativas. La asignación de directivas puede determinar los valores de los parámetros para ese grupo de recursos en el momento de la asignación, lo que permite volver a usar las definiciones de directiva que se ocupan de las mismas propiedades de recursos con diferentes necesidades de cumplimiento.

Utilice JSON para crear una definición de directiva. La asignación de directivas contiene elementos para:

- nombre para mostrar
- description
- metadata
- modo de cumplimiento
- definición de directiva
- parámetros

Por ejemplo, el siguiente archivo JSON muestra una asignación de directiva en el modo _DoNotEnforce_ con parámetros dinámicos:

```json
{
    "properties": {
        "displayName": "Enforce resource naming rules",
        "description": "Force resource names to begin with DeptA and end with -LC",
        "metadata": {
            "assignedBy": "Cloud Center of Excellence"
        },
        "enforcementMode": "DoNotEnforce",
        "policyDefinitionId": "/subscriptions/{mySubscriptionID}/providers/Microsoft.Authorization/policyDefinitions/ResourceNaming",
        "parameters": {
            "prefix": {
                "value": "DeptA"
            },
            "suffix": {
                "value": "-LC"
            }
        }
    }
}
```

Todos los ejemplos de Azure Policy se encuentran en [Ejemplos de Azure Policy](../samples/index.md).

## <a name="display-name-and-description"></a>Nombre para mostrar y descripción

Use **displayName** y **description** para identificar la asignación de directiva y proporcionar el contexto para su uso con el conjunto específico de recursos. **displayName** tiene una longitud máxima de _128_ caracteres y **description** tiene una longitud máxima de _512_ caracteres.

## <a name="enforcement-mode"></a>Modo de cumplimiento

La propiedad **enforcementMode** proporciona a los clientes la capacidad de probar el resultado de una directiva en los recursos existentes sin necesidad de iniciar el efecto de la directiva ni de desencadenar entradas en el [registro de actividad de Azure](../../../azure-monitor/platform/platform-logs-overview.md). Este escenario se conoce comúnmente como "What If" y se alinea con las prácticas de implementación segura. **enforcementMode** es diferente del efecto [Deshabilitado](./effects.md#disabled), ya que ese efecto evita que se produzca la evaluación de recursos.

Esta propiedad tiene los siguientes valores:

|Mode |Valor JSON |Tipo |Corregir manualmente |Entrada de registro de actividad |Descripción |
|-|-|-|-|-|-|
|habilitado |Valor predeterminado |string |Sí |Sí |El efecto de la directiva se aplica durante la creación o actualización de recursos. |
|Disabled |DoNotEnforce |string |Sí |No | El efecto de la directiva no se aplica durante la creación o actualización de recursos. |

Si **enforcementMode** no se especifica en una definición de directiva o iniciativa, se usa el valor _Default_. [Las tareas de corrección](../how-to/remediate-resources.md) se pueden iniciar para las directivas [deployIfNotExists](./effects.md#deployifnotexists), incluso cuando **enforcementMode** está establecido en _DoNotEnforce_.

## <a name="policy-definition-id"></a>Identificador de la definición de directiva

Este campo debe ser el nombre de la ruta de acceso completa de una definición de directiva o una definición de iniciativa.
`policyDefinitionId` es una cadena y no una matriz. Se recomienda que, si a menudo se asignan varias directivas, se use una [iniciativa](./definition-structure.md#initiatives) en su lugar.

## <a name="parameters"></a>Parámetros

Este segmento de la asignación de directiva proporciona los valores para los parámetros definidos en la [definición de directiva o en la definición de iniciativa](./definition-structure.md#parameters).
Este diseño permite reutilizar una definición de directiva o iniciativa con distintos recursos, así como comprobar si hay valores o resultados empresariales diferentes.

```json
"parameters": {
    "prefix": {
        "value": "DeptA"
    },
    "suffix": {
        "value": "-LC"
    }
}
```

En este ejemplo, los parámetros definidos anteriormente en la definición de directiva son `prefix` y `suffix`. Esta asignación de directiva determinada establece `prefix` en **DeptA** y `suffix` en **-LC**. La misma definición de directiva se puede volver a usar con un conjunto diferente de parámetros para otro departamento, lo que reduce la duplicación y la complejidad de las definiciones de directiva, a la vez que proporciona flexibilidad.

## <a name="next-steps"></a>Pasos siguientes

- Más información sobre la [estructura de la definición de directiva](./definition-structure.md).
- Obtenga información acerca de cómo se pueden [crear directivas mediante programación](../how-to/programmatically-create.md).
- Obtenga información sobre cómo [obtener datos de cumplimiento](../how-to/get-compliance-data.md).
- Obtenga información sobre cómo [corregir recursos no compatibles](../how-to/remediate-resources.md).
- En [Organización de los recursos con grupos de administración de Azure](../../management-groups/overview.md), obtendrá información sobre lo que es un grupo de administración.
---
title: 'Patrón: operador count en una definición de directiva'
description: Este patrón de Azure Policy proporciona un ejemplo de cómo usar el operador count en una definición de directiva.
ms.date: 10/14/2020
ms.topic: sample
ms.openlocfilehash: 1339dff7f8bc92a8e38ec5635690cc2069dd8df4
ms.sourcegitcommit: a92fbc09b859941ed64128db6ff72b7a7bcec6ab
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/15/2020
ms.locfileid: "92076409"
---
# <a name="azure-policy-pattern-the-count-operator"></a>Patrón de Azure Policy: operador count

El operador [count](../concepts/definition-structure.md#count) evalúa los miembros de un \[\*\] alias.

## <a name="sample-policy-definition"></a>Definición de directiva de ejemplo

Esta definición de directiva [audita](../concepts/effects.md#audit) los grupos de seguridad de red configurados para permitir el tráfico del Protocolo de escritorio remoto (RDP) entrante.

:::code language="json" source="~/policy-templates/patterns/pattern-count-operator.json":::

### <a name="explanation"></a>Explicación

Los componentes principales del operador **count** son _field_, _where_ y la condición. En el fragmento de código siguiente se resalta cada uno.

- _field_ indica a count de qué [alias](../concepts/definition-structure.md#aliases) debe evaluar los miembros. Aquí, observamos la _matriz_ de alias **securityRules\[\*\]** del grupo de seguridad de red.
- _where_ utiliza el lenguaje de directivas para definir qué miembros de la _matriz_ cumplen los criterios. En este ejemplo, el operador lógico **allOf** agrupa tres evaluaciones de condiciones diferentes de las propiedades de la _matriz_ de alias: _direction_, _access_ y _destinationPortRange_.
- La condición de count en este ejemplo es **greater** (mayor que). Count se evalúa como verdadero cuando uno o más miembros de la _matriz_ de alias coincide con la cláusula _where_.

:::code language="json" source="~/policy-templates/patterns/pattern-count-operator.json" range="12-32" highlight="3,4,20":::

## <a name="next-steps"></a>Pasos siguientes

- Consulte otros [patrones y definiciones integradas](./index.md).
- Revise la [estructura de definición de Azure Policy](../concepts/definition-structure.md).
- Vea la [Descripción de los efectos de directivas](../concepts/effects.md).
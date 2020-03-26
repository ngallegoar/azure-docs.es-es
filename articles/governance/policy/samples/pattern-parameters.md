---
title: 'Patrón: parámetros de una definición de directiva'
description: Este patrón de Azure Policy proporciona un ejemplo de cómo usar parámetros en una definición de directiva.
ms.date: 01/31/2020
ms.topic: sample
ms.openlocfilehash: 4921bb216ef67b561bc8986cf48239e6448beafc
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/24/2020
ms.locfileid: "77170183"
---
# <a name="azure-policy-pattern-parameters"></a>Patrón de Azure Policy: parámetros

Una definición de directiva se puede convertir en dinámica para reducir el número de definiciones de directiva necesarias mediante el uso de [parámetros](../concepts/definition-structure.md#parameters). El parámetro se define durante la asignación de la directiva. Los parámetros tienen un conjunto de propiedades predefinidas que describen el parámetro y cómo se usa.

## <a name="sample-1-string-parameters"></a>Muestra 1: Parámetros de cadena

Esta definición de directiva usa dos parámetros, **tagName** y **tagValue**, para establecer lo que busca la asignación de directiva en los recursos. Este formato permite usar la directiva para cualquier número de combinaciones de nombre de etiqueta y valor de etiqueta, pero solo se mantiene una única definición de directiva.

:::code language="json" source="~/policy-templates/patterns/pattern-parameters-1.json":::

### <a name="sample-1-explanation"></a>Muestra 1: Explicación

:::code language="json" source="~/policy-templates/patterns/pattern-parameters-1.json" range="8-13":::

En esta parte de la definición de directiva, el parámetro **tagName** se define como una _cadena_ y se proporciona una descripción para su uso.

A continuación, el parámetro se usa en el bloque **policyRule.if** para que la directiva sea dinámica. Aquí, se usa para definir el campo que se evalúa, que es una etiqueta con el valor de **tagName**.

:::code language="json" source="~/policy-templates/patterns/pattern-parameters-1.json" range="22-27" highlight="3":::

## <a name="sample-2-array-parameters"></a>Ejemplo 2: Parámetros de matriz

Esta definición de directiva usa un solo parámetro, **listOfBandwidthinMbps**, para comprobar si el recurso del circuito de ExpressRoute ha establecido la configuración de ancho de banda en uno de los valores aprobados. Si no coincide, la creación o actualización del recurso se [deniega](../concepts/effects.md#deny).

:::code language="json" source="~/policy-templates/patterns/pattern-parameters-2.json":::

### <a name="sample-2-explanation"></a>Ejemplo 2: Explicación

:::code language="json" source="~/policy-templates/patterns/pattern-parameters-2.json" range="6-12":::

En esta parte de la definición de directiva, el parámetro **listOfBandwidthinMbps** se define como una _matriz_ y se proporciona una descripción para su uso. Como _matriz_, tiene varios valores que hacer coincidir.

A continuación, el parámetro se usa en el bloque **policyRule.if**. Como parámetro de _matriz_, se debe usar una _matriz_
[ de condiciones](../concepts/definition-structure.md#conditions) con **in** o **notIn**.
En este caso, se usa con el alias **serviceProvider.bandwidthInMbps** como uno de los valores definidos.

:::code language="json" source="~/policy-templates/patterns/pattern-parameters-2.json" range="21-24" highlight="3":::

## <a name="next-steps"></a>Pasos siguientes

- Consulte otros [patrones y definiciones integradas](./index.md).
- Revise la [estructura de definición de Azure Policy](../concepts/definition-structure.md).
- Vea la [Descripción de los efectos de directivas](../concepts/effects.md).
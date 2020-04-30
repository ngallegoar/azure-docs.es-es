---
title: Directivas para etiquetar recursos
description: Describe las instancias de Azure Policy que puede asignar para garantizar el cumplimiento de la etiqueta.
ms.topic: conceptual
ms.date: 03/20/2020
ms.openlocfilehash: e3eeb28ea23b18c3492f68d2fac294fc014420c5
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/28/2020
ms.locfileid: "82147861"
---
# <a name="assign-policies-for-tag-compliance"></a>Asignación de directivas para el cumplimiento de la etiqueta

Puede usar [Azure Policy](../../governance/policy/overview.md) para aplicar reglas de etiquetado y convenciones. Mediante la creación de una directiva, evitará el escenario de recursos que se vayan a implementar en su suscripción que no cuentan con las etiquetas esperadas para su organización. En lugar de aplicar manualmente las etiquetas o buscar recursos que no son compatibles, cree una directiva que aplica automáticamente las etiquetas necesarias durante la implementación. También se pueden aplicar etiquetas a los recursos existentes con el nuevo efecto de [modificación](../../governance/policy/concepts/effects.md#modify) y una [tarea de corrección](../../governance/policy/how-to/remediate-resources.md). La sección siguiente muestra las directivas de ejemplo para las etiquetas.

## <a name="policies"></a>Directivas

[!INCLUDE [Tag policies](../../../includes/policy/samples/bycat/policies-tags.md)]

## <a name="next-steps"></a>Pasos siguientes

* Para información sobre el etiquetado de recursos, consulte [Uso de etiquetas para organizar los recursos](tag-resources.md).
* No todos los tipos de recursos admiten etiquetas. Para determinar si se puede aplicar una etiqueta a un tipo de recurso determinado, consulte [Tag support for Azure resources](tag-support.md) (Compatibilidad con etiquetas para los recursos de Azure).

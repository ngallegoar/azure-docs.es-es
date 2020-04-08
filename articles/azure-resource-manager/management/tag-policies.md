---
title: Directivas para etiquetar recursos
description: Describe las instancias de Azure Policy que puede asignar para garantizar el cumplimiento de la etiqueta.
ms.topic: conceptual
ms.date: 03/20/2020
ms.openlocfilehash: e7febe4c833cefd0ee9a5c49b4b70f4901fea8d7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "80137692"
---
# <a name="assign-policies-for-tag-compliance"></a>Asignación de directivas para el cumplimiento de la etiqueta

Puede usar [Azure Policy](../../governance/policy/overview.md) para aplicar reglas de etiquetado y convenciones. Mediante la creación de una directiva, evitará el escenario de recursos que se vayan a implementar en su suscripción que no cuentan con las etiquetas esperadas para su organización. En lugar de aplicar manualmente las etiquetas o buscar recursos que no son compatibles, cree una directiva que aplica automáticamente las etiquetas necesarias durante la implementación. También se pueden aplicar etiquetas a los recursos existentes con el nuevo efecto de [modificación](../../governance/policy/concepts/effects.md#modify) y una [tarea de corrección](../../governance/policy/how-to/remediate-resources.md). La sección siguiente muestra las directivas de ejemplo para las etiquetas.

## <a name="policies"></a>Directivas

[!INCLUDE [Tag policies](../../../includes/azure-policy-samples-policies-tags.md)]

## <a name="next-steps"></a>Pasos siguientes

* Para información sobre el etiquetado de recursos, consulte [Uso de etiquetas para organizar los recursos](tag-resources.md).
* No todos los tipos de recursos admiten etiquetas. Para determinar si se puede aplicar una etiqueta a un tipo de recurso determinado, consulte [Tag support for Azure resources](tag-support.md) (Compatibilidad con etiquetas para los recursos de Azure).

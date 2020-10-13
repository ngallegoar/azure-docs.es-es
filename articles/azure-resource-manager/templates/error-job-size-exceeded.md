---
title: Error de tamaño de trabajo superado
description: Describe cómo solucionar errores cuando el tamaño o la plantilla de trabajo son demasiado grandes.
ms.topic: troubleshooting
ms.date: 10/07/2020
ms.openlocfilehash: 638bdef246fc908ab997bfb99e7526febdb3792e
ms.sourcegitcommit: d2222681e14700bdd65baef97de223fa91c22c55
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/07/2020
ms.locfileid: "91822151"
---
# <a name="resolve-errors-for-job-size-exceeded"></a>Resolución de errores de tamaño de trabajo superado

En este artículo se describe cómo resolver los errores **JobSizeExceededException** y **DeploymentSizeExceededException**.

## <a name="symptom"></a>Síntoma

Al implementar una plantilla, recibe un error que indica que la implementación ha superado los límites.

## <a name="cause"></a>Causa

Puede obtener este error si el tamaño de la plantilla es superior a 4 MB. El límite de 4 MB se aplica al estado final de la plantilla después de su ampliación para las definiciones de recursos que utilizan la [copia](copy-resources.md) para crear muchas instancias. El estado final también incluye los valores resueltos para variables y parámetros.

El trabajo de implementación también incluye metadatos sobre la solicitud. En el caso de las plantillas grandes, los metadatos combinados con la plantilla pueden superar el tamaño permitido para un trabajo.

Otros límites de la plantilla son:

* 256 parámetros
* 256 variables
* 800 recursos (incluido el recuento de copia)
* 64 valores de salida
* 24 576 caracteres en una expresión de plantilla

## <a name="solution-1---simplify-template"></a>Solución 1: simplificar la plantilla

La primera opción es simplificar la plantilla. Esta opción funciona cuando la plantilla implementa muchos tipos de recursos diferentes. Considere la posibilidad de dividir la plantilla en [plantillas vinculadas](linked-templates.md). Divida los tipos de recursos en grupos lógicos y agregue una plantilla vinculada para cada grupo. Por ejemplo, si necesita implementar una gran cantidad de recursos de red, puede trasladar esos recursos a una plantilla vinculada.

Puede establecer otros recursos como dependientes de la plantilla vinculada y [obtener valores de la salida de la plantilla vinculada](linked-templates.md#get-values-from-linked-template).

## <a name="solution-2---reduce-name-size"></a>Solución 2: reducir el tamaño de los nombres

Intente acortar la longitud de los nombres que utiliza para [parámetros](template-parameters.md), [variables](template-variables.md) y [salidas](template-outputs.md). Cuando estos valores se repiten a través de bucles de copia, un nombre largo se multiplica varias veces.

## <a name="solution-3---use-serial-copy"></a>Solución 3: usar la copia en serie

La segunda opción consiste en cambiar el bucle de copia de [procesamiento paralelo a en serie](copy-resources.md#serial-or-parallel). Use esta opción solo cuando sospeche que el error proviene de la implementación de un gran número de recursos a través de la copia. Este cambio puede aumentar significativamente el tiempo de implementación, ya que los recursos no se implementan en paralelo.

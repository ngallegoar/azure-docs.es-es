---
title: Varias ramas en un flujo de datos de asignación
description: Replicación de flujos de datos en el flujo de datos de asignación con varias ramas
author: kromerm
ms.author: makromer
ms.reviewer: douglasl
ms.service: data-factory
ms.topic: conceptual
ms.custom: seo-lt-2019; seo-dt-2019
ms.date: 01/08/2020
ms.openlocfilehash: 71fb9f1ba9952be0e6b3910dd1079aa6d3c0482d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "75834499"
---
# <a name="creating-a-new-branch-in-mapping-data-flow"></a>Creación de una nueva rama en el flujo de datos de asignación

Agregue una nueva rama para realizar varios conjuntos de operaciones y transformaciones en la misma secuencia de datos. La incorporación de una nueva rama resulta útil si quiere utilizar el mismo origen para varios receptores o para autocombinar datos.

Se puede agregar una nueva rana desde la lista de transformación del mismo modo en que se agregan otras transformaciones. La opción **Nueva rama** solo estará disponible como acción cuando exista una transformación después de la transformación que está intentando ramificar.

![Adición de una nueva rama](media/data-flow/new-branch2.png "Adición de una nueva rama")

En el ejemplo siguiente, el flujo de datos está leyendo los datos de carreras de taxi. Es obligatorio agregar los resultados por día y por proveedor. En lugar de crear dos flujos de datos independientes que leen del mismo origen, se puede agregar una nueva rama. De este modo, ambas agregaciones se pueden ejecutar como parte del mismo flujo de datos. 

![Adición de una nueva rama](media/data-flow/new-branch.png "Adición de una nueva rama")

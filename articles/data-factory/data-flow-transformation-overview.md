---
title: Introducción a las transformaciones en el flujo de datos de asignación
description: Información general sobre las distintas transformaciones disponibles en el flujo de datos de asignación
author: djpmsft
ms.author: daperlov
manager: anandsub
ms.service: data-factory
ms.topic: conceptual
ms.date: 03/10/2020
ms.openlocfilehash: 68423bee6096357add9b5d4b107c984ac67c9cab
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/16/2020
ms.locfileid: "81412964"
---
# <a name="mapping-data-flow-transformation-overview"></a>Introducción a las transformaciones en el flujo de datos de asignación

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

A continuación se muestra una lista de las transformaciones admitidas actualmente en el flujo de datos de asignación. Haga clic en cada una de las transformaciones para obtener información detallada sobre su configuración.

| Nombre | Category | Descripción |
| ---- | -------- | ----------- |
| [Agregada](data-flow-aggregate.md) | Modificador de esquema | Define diferentes tipos de agregaciones, tales como SUM, MIN, MAX y COUNT, agrupadas por columnas calculadas o existentes. | 
| [Alteración de fila](data-flow-alter-row.md) | Modificador de fila | Establece directivas de inserción, eliminación, actualización y upsert en las filas. |
| [División condicional](data-flow-conditional-split.md) | Varias entradas y salidas | Enruta filas de datos a diferentes flujos según las condiciones de coincidencia. |
| [Columna derivada](data-flow-derived-column.md) | Modificador de esquema | Genera nuevas columnas o modifica campos existentes mediante el lenguaje de expresiones de flujo de datos. | 
| [Exists](data-flow-exists.md) | Varias entradas y salidas | Comprueba si los datos existen en otro origen o flujo. | 
| [Filter](data-flow-filter.md) | Modificador de fila | Filtra una fila en función de una condición. |
| [Flatten](data-flow-flatten.md) | Modificador de esquema |  Toma valores de matriz incluidos en estructuras jerárquicas, como JSON, y los expande en filas individuales. |
| [Join](data-flow-join.md) | Varias entradas y salidas |  Combina datos de dos orígenes o flujos. |
| [Lookup](data-flow-lookup.md) | Varias entradas y salidas | Hace referencia a datos de otro origen. |
| [Nueva rama](data-flow-new-branch.md) | Varias entradas y salidas | Aplica varios conjuntos de operaciones y transformaciones en el mismo flujo de datos. |
| [Dinamización](data-flow-pivot.md) | Modificador de esquema | Agregación en la que los valores de fila distintivos de una o más columnas de agrupación se transforman en columnas individuales. |
| [Select](data-flow-select.md) | Modificador de esquema | Establece alias de nombres de columnas y flujos, y quita o reordena columnas. |
| [Sink](data-flow-sink.md) | - | Destino final para los datos. |
| [Sort](data-flow-sort.md) | Modificador de fila | Ordena las filas entrantes del flujo de datos actual. |
| [Origen](data-flow-source.md) | - | Un origen de datos para el flujo de datos. |
| [Clave suplente](data-flow-surrogate-key.md) | Modificador de esquema | Agrega un valor de clave arbitraria no empresarial en incremento. |
| [Unión](data-flow-union.md) | Varias entradas y salidas | Combina varios flujos de datos verticalmente. |
| [Anulación de dinamización](data-flow-unpivot.md) | Modificador de esquema | Dinamiza columnas en valores de fila. |
| [Ventana](data-flow-window.md) | Modificador de esquema |  Define agregaciones basadas en ventanas de las columnas en los flujos de datos. |

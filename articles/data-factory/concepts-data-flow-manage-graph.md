---
title: Grafos de flujo de datos
description: Cómo trabajar con gráficos de flujo de datos de factoría de datos
author: kromerm
ms.author: makromer
ms.service: data-factory
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 11/04/2019
ms.openlocfilehash: 0d357c4c671070a5c5e9d4587e2f90b6628996f4
ms.sourcegitcommit: 5e49f45571aeb1232a3e0bd44725cc17c06d1452
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2020
ms.locfileid: "81605353"
---
# <a name="mapping-data-flow-graphs"></a>Asignar gráficos de flujo de datos

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

La superficie de diseño de flujo de datos de asignación es una superficie de "construcción" en la que se crean flujos de datos de arriba abajo, de izquierda a derecha. Hay un cuadro de herramientas asociado a cada transformación con el símbolo de más (+). Concéntrese en su lógica de negocios, en lugar de en conectar los nodos a través de los extremos de un entorno de DAG de forma libre.

A continuación se muestran mecanismos integrados para administrar el gráfico de flujo de datos.

## <a name="move-nodes"></a>Traslado de nodos

![Opciones de la transformación Agregar](media/data-flow/agghead.png "encabezado de agregador")

Sin un paradigma de arrastrar y colocar, la manera de "desplazar" un nodo de transformación es cambiar el flujo entrante. En su lugar, para mover las transformaciones cambiará el "flujo de entrada".

## <a name="streams-of-data-inside-of-data-flow"></a>Flujos de datos dentro de Data Flow

En Azure Data Factory Data Flow, las secuencias representan el flujo de datos. En el panel de configuración de la transformación, verá el campo "Incoming Stream" (Flujo de entrada). En se indica cuál es el flujo de datos de entrada que alimenta la transformación. Puede cambiar la ubicación física del nodo de la transformación en el gráfico. Para ello, solo es preciso hacer clic en el nombre del flujo de entrada y seleccionar otro flujo de datos. A continuación, tanto la transformación actual como todas las transformaciones posteriores de esa secuencia, se moverán a la nueva ubicación.

Si va a mover una transformación con una o varias transformaciones posteriores, la nueva ubicación del flujo de datos se conectará mediante una nueva rama.

Si no hay transformaciones posteriores al nodo que ha seleccionado, la primera será la única que se moverá a la nueva ubicación.

## <a name="hide-graph-and-show-graph"></a>Ocultar gráfico y mostrar gráfico

Hay un botón en el extremo derecho del panel de configuración inferior, donde puede expandir el panel inferior a pantalla completa al trabajar en configuraciones de transformación. Esto le permitirá usar los botones "anterior" y "siguiente" para navegar por las configuraciones del gráfico. Para volver a la vista de gráfico, haga clic en el botón bajar y vuelva a la pantalla de división.

## <a name="search-graph"></a>Buscar gráfico

Puede buscar el gráfico con el botón buscar en la superficie de diseño.

![Búsqueda](media/data-flow/search001.png "Buscar gráfico")

## <a name="next-steps"></a>Pasos siguientes

Después de completar el diseño de Data Flow, active el botón de depuración y pruébelo en modo de depuración, ya sea directamente en el [diseñador del flujo de datos](concepts-data-flow-debug-mode.md) o en la [depuración de canalización](control-flow-execute-data-flow-activity.md).

---
title: Transformación Anular dinamización en el flujo de datos de asignación
description: Transformación Anular dinamización de flujo de datos de asignación de Azure Data Factory
author: kromerm
ms.author: makromer
ms.service: data-factory
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 07/14/2020
ms.openlocfilehash: ea8881adf39a315df7746dbce14dedcbee18ccf6
ms.sourcegitcommit: 59f506857abb1ed3328fda34d37800b55159c91d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/24/2020
ms.locfileid: "92521057"
---
# <a name="unpivot-transformation-in-mapping-data-flow"></a>Transformación Anular dinamización en el flujo de datos de asignación

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Use Anular dinamización en un flujo de datos de asignación de ADF como una forma de convertir un conjunto de datos sin normalizar en una versión más normalizada, ampliando los valores de varias columnas en un único registro a varios registros con los mismos valores en una sola columna.

![Transformación Anular dinamización](media/data-flow/unpivot1.png "Opciones de anulación de dinamización 1")

> [!VIDEO https://www.microsoft.com/en-us/videoplayer/embed/RE4B1RR]

## <a name="ungroup-by"></a>Ungroup By

![Transformación Anular dinamización](media/data-flow/unpivot5.png "Opciones de anulación de dinamización 2")

En primer lugar, establezca las columnas por las que desea desagrupar la agregación de anulación de dinamización. Establezca una o varias columnas para desagrupar con el + situado junto a la lista de columnas.

## <a name="unpivot-key"></a>Clave de anulación de dinamización

![Transformación Anular dinamización](media/data-flow/unpivot6.png "Opciones de anulación de dinamización 3")

La clave de anulación de dinamización es la columna que ADF utilizará para dinamizar de columna a fila. De forma predeterminada, cada valor único del conjunto de datos de este campo se dinamizará en una fila. Sin embargo, también puede escribir los valores del conjunto de datos que desea dinamizar en valores de fila.

## <a name="unpivoted-columns"></a>Columnas con dinamización anulada

![Transformación Anular dinamización](media/data-flow//unpivot7.png "Opciones de anulación de dinamización 4")

Por último, elija el nombre de columna para almacenar los valores de las columnas no dinamizadas que se transforman en filas.

(Opcional) Puede eliminar las filas con valores NULL.

Por ejemplo, SumCost es el nombre de columna que se eligió en el ejemplo compartido anterior.

![Imagen que muestra las columnas PO, Vendor y Fruit antes y después de una transformación de tipo unipivot que usa la columna Fruit como clave unipivot.](media/data-flow/unpivot3.png)

Al establecer la organización de la columna en "Normal", se agruparán todas las nuevas columnas no dinamizadas a partir de un único valor. Al establecer la organización de las columnas en "Lateral", se agruparán las nuevas columnas no dinamizadas generadas a partir de una columna existente.

![Transformación Anular dinamización](media/data-flow//unpivot7.png "Opciones de anulación de dinamización 5")

El conjunto de resultados final de los datos con dinamización anulada muestra los totales de columna ahora sin dinamizar en valores de fila independientes.

## <a name="next-steps"></a>Pasos siguientes

Use la [transformación Dinamizar](data-flow-pivot.md) para dinamizar las filas en columnas.

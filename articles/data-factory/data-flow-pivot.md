---
title: Transformación Dinamización del flujo de datos de asignación
description: Dinamice los datos de filas a columnas mediante la transformación Dinamizar del flujo de datos de asignación de Azure Data Factory
author: kromerm
ms.author: makromer
ms.service: data-factory
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 01/30/2019
ms.openlocfilehash: 8f23b5e61e1aee83172a12466fac8d5b5003fea8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "74930283"
---
# <a name="azure-data-factory-pivot-transformation"></a>Transformación Dinamizar de Azure Data Factory


Utiliza Dinamizar en ADF Data Flow como una agregación en la que los valores distintivos de una o más columnas de agrupación se transforman en columnas individuales. Básicamente, puede dinamizar valores de fila en nuevas columnas (convertir datos en metadatos).

![Opciones de dinamización](media/data-flow/pivot1.png "dinamizar 1")

## <a name="group-by"></a>Agrupar por

![Opciones de dinamización](media/data-flow/pivot2.png "dinamizar 2")

En primer lugar, establezca las columnas que desea agrupar para la agregación dinámica. Puede establecer más de una columna con el signo + que aparece junto a la lista de columnas.

## <a name="pivot-key"></a>Clave dinámica

![Opciones de dinamización](media/data-flow/pivot3.png "dinamizar 3")

La clave dinámica es la columna que ADF dinamizará de fila a columna. De forma predeterminada, cada valor único del conjunto de datos de este campo se dinamizará en una columna. Sin embargo, también puede escribir los valores del conjunto de datos que desea dinamizar en valores de columna. Se trata de la columna que determinará las nuevas columnas que se crearán.

## <a name="pivoted-columns"></a>Columnas dinamizadas

![Opciones de dinamización](media/data-flow/pivot4.png "dinamizar 4")

Por último, elegirá la agregación que se va a usar para los valores dinamizados y cómo desea que las columnas se muestren en la nueva proyección de salida de la transformación.

(Opcional) Puede establecer un patrón de nomenclatura con un prefijo, un término intermedio y el sufijo que se agregará a los nuevos nombres de columna creados a partir de los valores de fila.

Por ejemplo, dinamizar "Ventas" por "Región" daría lugar a nuevos valores de columna a partir de cada valor de ventas; es decir: "25", "50", "1000", etc. Sin embargo, si establece un valor de prefijo de "Sales-", cada valor de columna agregará "Sales-" al principio del valor.

![Opciones de dinamización](media/data-flow/pivot5.png "dinamizar 5")

Al establecer la organización de la columna en "Normal", se agruparán todas las columnas dinámicas con sus valores agregados. Al establecer la organización de las columnas en "Lateral", se alternará entre columna y valor.

### <a name="aggregation"></a>Agregación

Para establecer la agregación que se va a usar para los valores dinámicos, haga clic en el campo situado en la parte inferior del panel Columnas dinamizadas. Se abrirá el Generador de expresiones de ADF Data Flow, donde puede crear una expresión de agregación y proporcionar un nombre de alias descriptivo para los nuevos valores agregados.

Use el lenguaje de expresiones de ADF Data Flow para describir las transformaciones de columna dinamizada en el Generador de expresiones: https://aka.ms/dataflowexpressions.

## <a name="pivot-metadata"></a>Metadatos dinámicos

La transformación Dinamizar generará nuevos nombres de columna que serán dinámicos en función de los datos entrantes. La clave dinámica genera los valores de cada nombre de columna nuevo. Si no especifica valores individuales y quiere crear nombres de columna dinámicos para cada valor único en la clave dinámica, la interfaz de usuario no mostrará los metadatos en Inspeccionar y no se producirá ninguna propagación de columna para la transformación Receptor. Si establece los valores de clave dinámica, ADF puede determinar los nombres de columna nuevos, que estarán disponibles en las asignaciones de Inspeccionar y Receptor.

### <a name="generate-a-new-model-from-dynamic-columns"></a>Generación de un nuevo modelo a partir de columnas dinámicas

Dinamizar genera nuevos nombres de columna de forma dinámica en función de los valores de fila. Estas nuevas columnas se pueden convertir en metadatos a los que se puede hacer referencia posteriormente en el flujo de datos. Para ello, haga clic en la pestaña Vista previa de datos. Las nuevas columnas generadas por la transformación Dinamizar aparecen con un icono de desviación en el encabezado de la tabla. Haga clic en el botón "Map drifted" (Asignar desviadas) para convertir esas nuevas columnas en metadatos, haciendo así que pasen a formar parte del modelo de flujo de datos.

![Columnas dinámicas](media/data-flow/newpivot1.png "Asignar columnas dinámicas desplazadas")

### <a name="landing-new-columns-in-sink"></a>Aterrizaje de nuevas columnas e Receptor

Incluso con los nombres de columna dinámicos de Dinamizar, puede recibir nuevos valores y nombres de columna en el almacén de destino. Solo tiene que establecer "Allow Schema Drift" (Permitir desviación del esquema) en la configuración de Receptor. No verá los nombres dinámicos nuevos en los metadatos de la columna, pero la opción de desviación del esquema le permitirá el aterrizaje de los datos.

### <a name="view-metadata-in-design-mode"></a>Ver metadatos en el modo de diseño

Si desea ver los nuevos nombres de columna como metadatos en Inspeccionar y quiere ver la propagación explícita de las columnas a la transformación Receptor, establezca valores explícitos en la pestaña Clave dinámica.

### <a name="how-to-rejoin-original-fields"></a>Cómo volver a combinar los campos originales
La transformación dinámica solo proyectará las columnas utilizadas en la acción de agregación, agrupación y dinamización. Si desea incluir las demás columnas del paso anterior en el flujo, utilice una rama nueva del paso anterior y el patrón de autocombinación para conectar el flujo con los metadatos originales.

## <a name="next-steps"></a>Pasos siguientes

Pruebe la [transformación Anular dinamización](data-flow-unpivot.md) para convertir los valores de columna en los valores de fila. 

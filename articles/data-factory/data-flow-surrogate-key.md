---
title: Transformación Clave suplente en el flujo de datos de asignación
description: Uso de la transformación Clave suplente de flujo de datos de asignación de Azure Data Factory para generar valores de claves secuenciales
author: kromerm
ms.author: makromer
ms.reviewer: daperlov
ms.service: data-factory
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 10/30/2020
ms.openlocfilehash: d1f8993b1adc297b1bfadba114df76a66e59afa2
ms.sourcegitcommit: 4b76c284eb3d2b81b103430371a10abb912a83f4
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/01/2020
ms.locfileid: "93147191"
---
# <a name="surrogate-key-transformation-in-mapping-data-flow"></a>Transformación Clave suplente en el flujo de datos de asignación 

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Use la transformación Clave suplente para agregar un valor de clave incremental a cada fila de datos. Esto resulta útil al diseñar tablas de dimensiones en un modelo de datos analíticos de esquema de estrella. En un esquema de estrella, cada miembro de las tablas de dimensiones requiere una clave única que no sea una clave empresarial.

## <a name="configuration"></a>Configuración

![Transformación Clave suplente](media/data-flow/surrogate.png "Transformación Clave suplente")

**Columna de clave:** nombre de la columna de clave suplente generada.

**Valor inicial:** valor de clave más bajo que se generará.

## <a name="increment-keys-from-existing-sources"></a>Incrementar claves a partir de orígenes existentes

Para iniciar la secuencia desde un valor existente en un origen, se recomienda usar un receptor de caché para guardar ese valor y utilizar una transformación de columna derivada para agregar los dos valores juntos. Use una búsqueda almacenada en caché para obtener la salida y anexarla a la clave generada. Para obtener más información, consulte los temas sobre los [receptores de caché](data-flow-sink.md#cache-sink) y las [búsquedas almacenadas en caché](concepts-data-flow-expression-builder.md#cached-lookup).

![Búsqueda de clave suplente](media/data-flow/cached-lookup-example.png "Búsqueda de clave suplente")

### <a name="increment-from-existing-maximum-value"></a>Incremento del valor máximo existente

Para inicializar el valor de clave con el máximo anterior, puede usar dos técnicas en función de la ubicación de los datos de origen.

#### <a name="database-sources"></a>Orígenes de base de datos

Use una opción de consulta SQL para seleccionar MAX() desde el origen. Por ejemplo, `Select MAX(<surrogateKeyName>) as maxval from <sourceTable>`.

![Consulta de clave suplente](media/data-flow/surrogate-key-max-database.png "Consulta de transformación Clave suplente")

#### <a name="file-sources"></a>Orígenes de archivo

Si el valor máximo anterior se encuentra en un archivo, use la función `max()` en la transformación Agregado para obtener el valor máximo anterior:

![Archivo de clave suplente](media/data-flow/surrogate-key-max-file.png "Archivo de clave suplente")

En ambos casos, tendrá que escribir en un receptor de caché y buscar el valor. 


## <a name="data-flow-script"></a>Script de flujo de datos

### <a name="syntax"></a>Sintaxis

```
<incomingStream> 
    keyGenerate(
        output(<surrogateColumnName> as long),
        startAt: <number>L
    ) ~> <surrogateKeyTransformationName>
```

### <a name="example"></a>Ejemplo

![Transformación Clave suplente](media/data-flow/surrogate.png "Transformación Clave suplente")

El script de flujo de datos para la configuración de clave suplente anterior se encuentra en el siguiente fragmento de código.

```
AggregateDayStats
    keyGenerate(
        output(key as long),
        startAt: 1L
    ) ~> SurrogateKey1
```

## <a name="next-steps"></a>Pasos siguientes

En estos ejemplos se usan las transformaciones [Combinación](data-flow-join.md) y [Columna derivada](data-flow-derived-column.md).

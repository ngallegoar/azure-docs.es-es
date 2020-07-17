---
title: 'Tipo de datos admitidos: Azure Time Series Insights | Microsoft Docs'
description: Información acerca de los tipo de datos admitidos en la versión preliminar de Azure Time Series Insights.
author: lyrana
ms.author: lyhughes
manager: deepakpalled
ms.workload: big-data
ms.service: time-series-insights
services: time-series-insights
ms.topic: conceptual
ms.date: 06/03/2020
ms.custom: seodec18
ms.openlocfilehash: 1a1ab9fa19956341e930c85d13d7067be4449cea
ms.sourcegitcommit: e132633b9c3a53b3ead101ea2711570e60d67b83
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/07/2020
ms.locfileid: "86049451"
---
# <a name="supported-data-types"></a>Tipos de datos admitidos

En la tabla siguiente se enumeran los tipos de datos admitidos por Time Series Insights

| Tipo de datos | Descripción | Ejemplo | Nombre de la columna de propiedad en Parquet
|---|---|---|---|
| **bool** | Tipo de datos que tiene uno de dos estados: `true` o `false`. | "isQuestionable" : true | isQuestionable_bool
| **datetime** | Representa un instante de tiempo, normalmente expresado en forma de fecha y hora del día. Se expresa en formato [ISO 8601](https://www.iso.org/iso-8601-date-and-time-format.html). Las propiedades DateTime siempre se almacenan en formato UTC. Los desplazamientos de zona horaria, si tiene el formato correcto, se aplicarán y, luego, el valor se almacenará en UTC. Consulte [esta](concepts-streaming-ingestion-event-sources.md#event-source-timestamp) sección para obtener más información sobre la propiedad de marca de tiempo del entorno y los desplazamientos datetime | "eventProcessedLocalTime": "2020-03-20T09:03:32.8301668Z" | eventProcessedLocalTime_datetime 
| **double** | Un número de 64 bits de precisión doble  | "value": 31.0482941 | value_double
| **long** | Un entero de 64 bits con signo  | "value" : 31 | value_long
| **string** | Los valores de texto, deben constar de UTF8 válidos. |  "site": "DIM_MLGGG" | site_string
| **dynamic** | Tipo complejo (no primitivo) que consta de una matriz o un contenedor de propiedades (diccionario). Actualmente, solo las matrices JSON de cadenas de elementos primitivos o matrices de objetos que no contienen el identificador de TS o las propiedades de marca de tiempo se almacenarán como dinámicas. Lea este [artículo](./concepts-json-flattening-escaping-rules.md) para comprender cómo se acoplarán los objetos y se pueden anular las matrices |  "values": "[197, 194, 189, 188]" | values_dynamic

> [!IMPORTANT]
>
> * Su entorno TSI tiene un establecimiento de tipos inflexible. Si los dispositivos o etiquetas envían datos enteros y no enteros, los valores de propiedad del dispositivo se almacenarán en dos columnas double y long separadas, y la [función coalesce()](https://docs.microsoft.com/rest/api/time-series-insights/preview#time-series-expression-and-syntax) debe usarse al realizar llamadas API y definir las expresiones de las variables del modelo de serie temporal.

#### <a name="objects-and-arrays"></a>Objetos y matrices

Puede enviar tipos complejos, como objetos y matrices, como parte de la carga del evento. Los objetos anidados se acoplarán y las matrices se almacenarán como `dynamic` o sin acoplar para generar varios eventos en función de la configuración del entorno y la forma de JSON. Para obtener más información, lea sobre las [Reglas de acoplamiento y de escape de JSON](./concepts-json-flattening-escaping-rules.md)

## <a name="next-steps"></a>Pasos siguientes

* Lea las [Reglas de acoplamiento y de escape de JSON](./concepts-json-flattening-escaping-rules.md) para comprender cómo se almacenarán los eventos. 

* Comprender las [limitaciones de rendimiento](concepts-streaming-throughput-limitations.md) del entorno

* Obtenga información sobre [Orígenes de eventos](concepts-streaming-ingestion-event-sources.md) para ingerir datos en streaming.

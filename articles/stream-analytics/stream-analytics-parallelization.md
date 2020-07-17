---
title: Uso de la paralelización de consultas y la reducción horizontal en Azure Stream Analytics
description: En este artículo se explica cómo escalar trabajos de Stream Analytics mediante la configuración de particiones de entrada, la optimización de la definición de consulta y el ajuste de las unidades de streaming del trabajo.
author: JSeb225
ms.author: jeanb
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 05/04/2020
ms.openlocfilehash: 8a86c1df5925097fa85d09590b59f8f30fde41d4
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85296328"
---
# <a name="leverage-query-parallelization-in-azure-stream-analytics"></a>Aprovechamiento de la paralelización de consultas en Azure Stream Analytics
En este artículo se muestra cómo aprovechar la paralelización en Azure Stream Analytics. Aprenda a escalar los trabajos de Stream Analytics mediante la configuración de particiones de entrada y el ajuste de la definición de consultas de análisis.
Como requisito previo, puede que le interese conocer la noción de Unidad de streaming que se describe en [Descripción y ajuste de las unidades de streaming](stream-analytics-streaming-unit-consumption.md).

## <a name="what-are-the-parts-of-a-stream-analytics-job"></a>¿Cuáles son las partes de un trabajo de Stream Analytics?
La definición de un trabajo de Stream Analytics incluye por lo menos una entrada de streaming, una consulta y la salida. Las entradas proceden del lugar en el cual el trabajo lee el flujo de datos. La consulta se usa para transformar el flujo de entrada de datos y la salida es el lugar al que el trabajo envía los resultados.

## <a name="partitions-in-inputs-and-outputs"></a>Particiones en entradas y salidas
La creación de particiones permite dividir los datos en subconjuntos en función de una [clave de partición](https://docs.microsoft.com/azure/event-hubs/event-hubs-scalability#partitions). Si se crearon particiones en la entrada (por ejemplo, Event Hubs) con una clave, se recomienda especificar dicha clave al agregar entradas al trabajo de Stream Analytics. El escalado de un trabajo de Stream Analytics aprovecha las particiones en la entrada y la salida. Un trabajo de Stream Analytics puede consumir y escribir diferentes particiones en paralelo, lo que aumenta el rendimiento. 

### <a name="inputs"></a>Entradas
Todas las entradas de Azure Stream Analytics pueden aprovechar la creación de particiones:
-   EventHub (la clave de partición se debe establecer explícitamente con la palabra clave PARTITION BY si se usa el nivel de compatibilidad 1.1 o uno anterior)
-   IoT Hub(la clave de partición se debe establecer explícitamente con la palabra clave PARTITION BY si se usa el nivel de compatibilidad 1.1 o uno anterior)
-   Blob Storage

### <a name="outputs"></a>Salidas

Cuando trabaja con Stream Analytics, puede aprovechar la creación de particiones en las salidas:
-   Azure Data Lake Storage
-   Azure Functions
-   tabla de Azure
-   Blob Storage (la clave de partición se puede establecer explícitamente)
-   Cosmos DB (la clave de partición se debe establecer explícitamente)
-   Event Hubs (la clave de partición se debe establecer explícitamente)
-   IoT Hub (la clave de partición se debe establecer explícitamente)
-   Azure Service Bus
- SQL y SQL Data Warehouse con partición opcional: obtenga más información en la página [Salida de Azure Stream Analytics a Azure SQL Database](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-sql-output-perf).

Power BI no admite la creación de particiones. Sin embargo, de todos modos puede crear particiones de la entrada, tal como se describe en [está sección](#multi-step-query-with-different-partition-by-values) 

Para más información sobre las particiones, vea los siguientes artículos:

* [Información general de las características de Event Hubs](../event-hubs/event-hubs-features.md#partitions)
* [Creación de particiones de datos](https://docs.microsoft.com/azure/architecture/best-practices/data-partitioning)


## <a name="embarrassingly-parallel-jobs"></a>Trabajos embarazosamente paralelos
Un trabajo *embarazosamente paralelo* es el escenario más escalable en Azure Stream Analytics. Conecta una partición de la entrada en una instancia de la consulta a una partición de la salida. Este paralelismo tiene los siguientes requisitos:

1. Si la lógica de la consulta depende de la misma clave que procesa la misma instancia de consulta, ha de asegurarse de que los eventos vayan a la misma partición de la entrada. En Event Hubs o IoT Hub, esto significa que los datos del evento deben tener establecido el valor **PartitionKey**. También puede usar remitentes con particiones. En Blob Storage, esto significa que los eventos se envían a la misma carpeta de partición. Un ejemplo sería una instancia de consulta que agrega datos a cada userID, de modo que el centro de eventos de entrada se divide en particiones usando userID como clave de partición. No obstante, si la lógica de consulta no requiere que la misma instancia de consulta procese la misma clave, puede ignorar este requisito. Un ejemplo de esta lógica sería una sencilla consulta select-project-filter.  

2. El siguiente paso consiste en hacer que la consulta tenga particiones. En el caso de los trabajos con un nivel de compatibilidad 1.2 o superior (recomendado), se puede especificar una columna personalizada como clave de partición en la configuración de entrada, y el trabajo se realizará en paralelo automáticamente. Los trabajos con un nivel de compatibilidad 1.0 o 1.1 requieren el uso de **PARTITION BY PartitionId** en todos los pasos de la consulta. Se pueden usar varios pasos, pero todos deben particionarse con la misma clave. 

3. La mayoría de las salidas compatibles con Stream Analytics pueden aprovechar la creación de particiones. Si usa un tipo de salida que no admite la creación de particiones, el trabajo no será *embarazosamente paralelo*. En el caso de las salidas del centro de eventos, asegúrese de que la **columna de clave de partición** está configurada con la misma clave de partición que se usó en la consulta. Consulte la [sección de salida](#outputs) para más detalles.

4. El número de particiones de entrada debe ser igual al número de particiones de salida. La salida de Blob Storage puede admitir particiones y hereda el esquema de partición de la consulta de nivel superior. Cuando se especifica una clave de partición para Blob Storage, los datos se particionan por partición de entrada, por tanto, el resultado sigue siendo totalmente paralelo. Vea ejemplos de valores de partición que permiten un trabajo totalmente paralelo:

   * Ocho particiones de entrada de Event Hubs y ocho particiones de salida de Event Hubs
   * Ocho particiones de entrada de Event Hubs y salida de Blob Storage
   * Ocho particiones de entrada de Event Hubs y salida de Blob Storage particionadas por un campo personalizado con cardinalidad arbitraria
   * Ocho particiones de entrada de Blob Storage y salida de Blob Storage
   * Ocho particiones de entrada de Blob Storage y ocho particiones de salida de Event Hubs

En las siguientes secciones se describen algunos escenarios de ejemplo que son embarazosamente paralelos.

### <a name="simple-query"></a>Consulta sencilla

* Entrada: Event Hubs con 8 particiones
* Salida: Centro de eventos con 8 particiones (la columna de clave de partición debe establecerse para usar "PartitionId")

Consulta:

```SQL
    --Using compatibility level 1.2 or above
    SELECT TollBoothId
    FROM Input1
    WHERE TollBoothId > 100
    
    --Using compatibility level 1.0 or 1.1
    SELECT TollBoothId
    FROM Input1 PARTITION BY PartitionId
    WHERE TollBoothId > 100
```

Esta consulta es un filtro sencillo. Por consiguiente, no hay que preocuparse por crear particiones en la entrada que se envía al centro de eventos. Tenga en cuenta que los trabajos con un nivel de compatibilidad anterior a 1.2 deben incluir la cláusula **PARTITION BY PartitionId** para cumplir el requisito n.º 2 anterior. Para la salida, debemos configurar la salida de Event Hub en el trabajo para que la clave de partición esté establecida en **PartitionId**. Una última comprobación consiste en asegurarse de que el número de particiones de entrada es igual al número de particiones de salida.

### <a name="query-with-a-grouping-key"></a>Consulta con una clave de agrupación

* Entrada: Event Hubs con 8 particiones
* Salida: Blob Storage

Consulta:

```SQL
    --Using compatibility level 1.2 or above
    SELECT COUNT(*) AS Count, TollBoothId
    FROM Input1
    GROUP BY TumblingWindow(minute, 3), TollBoothId
    
    --Using compatibility level 1.0 or 1.1
    SELECT COUNT(*) AS Count, TollBoothId
    FROM Input1 Partition By PartitionId
    GROUP BY TumblingWindow(minute, 3), TollBoothId, PartitionId
```

Esta consulta tiene una clave de agrupación. Por lo tanto, los eventos agrupados en conjunto se deben enviar a la misma partición de Event Hub. Como en este ejemplo la agrupación se hizo por TollBoothID, debemos asegurarnos de que TollBoothID se usa como la clave de partición cuando los eventos se envían a Event Hub. A continuación, en ASA se puede usar **PARTITION BY PartitionId** para heredar desde este esquema de partición y habilitar la paralelización completa. Puesto que la salida es Blob Storage, no hay que preocuparse en configurar un valor de clave de partición, como estipula el requisito 4.

## <a name="example-of-scenarios-that-are-not-embarrassingly-parallel"></a>Escenarios de ejemplo que *no* son embarazosamente paralelos

En la sección anterior, se han mostrado algunos escenarios embarazosamente paralelos. En esta sección se describen escenarios en los que no se cumplen todos los requisitos para que sean embarazosamente paralelos. 

### <a name="mismatched-partition-count"></a>Recuento de particiones no coincidente
* Entrada: Event Hubs con 8 particiones
* Salida: Event Hubs con 32 particiones

Si el recuento de particiones de entrada no coincide con el recuento de particiones de salida, la topología no es embarazosamente paralela, independientemente de la consulta. Sin embargo, todavía se podrá obtener cierto nivel de paralelización.

### <a name="query-using-non-partitioned-output"></a>Consulta con salida sin particiones
* Entrada: Event Hubs con 8 particiones
* Salida: Power BI

La salida de Power BI no admite en este momento la creación de particiones. Por consiguiente, este escenario no es embarazosamente paralelo.

### <a name="multi-step-query-with-different-partition-by-values"></a>Consulta de varios pasos con diferentes valores de PARTITION BY
* Entrada: Event Hubs con 8 particiones
* Salida: Event Hubs con 8 particiones
* Nivel de compatibilidad: 1.0 o 1.1

Consulta:

```SQL
    WITH Step1 AS (
    SELECT COUNT(*) AS Count, TollBoothId, PartitionId
    FROM Input1 Partition By PartitionId
    GROUP BY TumblingWindow(minute, 3), TollBoothId, PartitionId
    )

    SELECT SUM(Count) AS Count, TollBoothId
    FROM Step1 Partition By TollBoothId
    GROUP BY TumblingWindow(minute, 3), TollBoothId
```

Como puede ver, el segundo paso utiliza **TollBoothId** como clave de partición. Este paso no es igual al primer paso y, por tanto, hay que seguir un orden aleatorio. 

### <a name="multi-step-query-with-different-partition-by-values"></a>Consulta de varios pasos con diferentes valores de PARTITION BY
* Entrada: Event Hubs con 8 particiones
* Salida: Centro de eventos con 8 particiones (la columna de clave de partición debe establecerse para usar "TollBoothId")
* Nivel de compatibilidad: 1.2 o superior

Consulta:

```SQL
    WITH Step1 AS (
    SELECT COUNT(*) AS Count, TollBoothId
    FROM Input1
    GROUP BY TumblingWindow(minute, 3), TollBoothId
    )

    SELECT SUM(Count) AS Count, TollBoothId
    FROM Step1
    GROUP BY TumblingWindow(minute, 3), TollBoothId
```

El nivel de compatibilidad 1.2 o superior permite la ejecución de consultas en paralelo de forma predeterminada. Por ejemplo, la consulta de la sección anterior se particionará siempre y cuando la columna "TollBoothId" se haya establecido como la clave de partición de entrada. La cláusula PARTITION BY ParttionId no es obligatoria.

## <a name="calculate-the-maximum-streaming-units-of-a-job"></a>Cálculo de las unidades máximas de streaming para un trabajo
El número total de unidades de streaming que se puede utilizar en un trabajo de Stream Analytics depende del número de pasos de la consulta definida para el trabajo y del número de particiones para cada paso.

### <a name="steps-in-a-query"></a>Pasos de una consulta
Una consulta puede tener uno o varios pasos. Cada paso es una subconsulta definida mediante la palabra clave **WITH**. La consulta que queda al margen de la palabra clave **WITH** (una sola consulta) también se cuenta como un paso; por ejemplo, la instrucción **SELECT** de la consulta siguiente:

Consulta:

```SQL
    WITH Step1 AS (
        SELECT COUNT(*) AS Count, TollBoothId
        FROM Input1 Partition By PartitionId
        GROUP BY TumblingWindow(minute, 3), TollBoothId, PartitionId
    )
    SELECT SUM(Count) AS Count, TollBoothId
    FROM Step1
    GROUP BY TumblingWindow(minute,3), TollBoothId
```

Esta consulta tiene dos pasos.

> [!NOTE]
> Esta consulta se analiza con más detalle más adelante en el artículo.
>  

### <a name="partition-a-step"></a>Posicionamiento de un paso
El particionamiento de un paso requiere las siguientes condiciones:

* El origen de entrada debe tener particiones. 
* La instrucción **SELECT** de la consulta debe leer desde un origen de entrada particionada.
* La consulta comprendida en el paso debe incluir la palabra clave **PARTITION BY**.

Cuando una consulta está particionada, los eventos de entrada se procesan y agregan en grupos de particiones independientes, y se generan eventos de salida para cada uno de los grupos. Si quiere usar un agregado combinado, debe crear un segundo paso sin particionar que agregar.

### <a name="calculate-the-max-streaming-units-for-a-job"></a>Cálculo de las unidades máximas de streaming para un trabajo
Todos los pasos sin particiones juntos pueden escalarse hasta seis unidades de streaming (SU) para un trabajo de Stream Analytics. Además, puede agregar 6 SU para cada partición en un paso con particiones.
Puede ver algunos **ejemplos** en la tabla a continuación.

| Consultar                                               | Número máximo de unidades de streaming del trabajo |
| --------------------------------------------------- | ------------------- |
| <ul><li>La consulta contiene un solo paso.</li><li>El paso no está particionado.</li></ul> | 6 |
| <ul><li>El flujo de datos de entrada está particionado en 16.</li><li>La consulta contiene un solo paso.</li><li>El paso está particionado.</li></ul> | 96 (6 * 16 particiones) |
| <ul><li>La consulta contiene dos pasos.</li><li>Ninguno de los pasos está particionado.</li></ul> | 6 |
| <ul><li>La secuencia de datos de entrada está particionada en tres.</li><li>La consulta contiene dos pasos. El paso de entrada tiene particiones y el segundo paso no.</li><li>La instrucción <strong>SELECT</strong> se lee de la entrada particionada.</li></ul> | 24 (18 para los pasos particionados y 6 para los pasos no particionados) |

### <a name="examples-of-scaling"></a>Ejemplos de escalado

La siguiente consulta calcula el número de vehículos que pasan por una estación de peaje con tres cabinas de peaje en una ventana temporal de tres minutos. Esta consulta se puede escalar hasta seis unidades de streaming.

```SQL
    SELECT COUNT(*) AS Count, TollBoothId
    FROM Input1
    GROUP BY TumblingWindow(minute, 3), TollBoothId, PartitionId
```

Para usar más unidades de streaming en la consulta, el flujo de datos de entrada y la consulta deben estar particionados. Dado que la partición del flujo de datos está establecida en 3, la siguiente consulta modificada puede escalarse hasta 18 unidades de streaming:

```SQL
    SELECT COUNT(*) AS Count, TollBoothId
    FROM Input1 Partition By PartitionId
    GROUP BY TumblingWindow(minute, 3), TollBoothId, PartitionId
```

Cuando una consulta está particionada, se procesan los eventos de entrada y se agregan en grupos de particiones independientes. También se generan eventos de salida para cada uno de los grupos. La creación de particiones puede ocasionar algunos resultados inesperados cuando el campo **GROUP BY** no es la clave de partición del flujo de datos de entrada. Por ejemplo, el campo **TollBoothId** de la consulta anterior no es la clave de partición de **Input1**. El resultado es que los datos de la cabina de peaje 1 se pueden distribuir en varias particiones.

Cada una de las particiones de **Input1** se procesará por separado en Stream Analytics. Por consiguiente, se crearán varios registros de recuento de vehículos para la misma cabina en la misma ventana de saltos de tamaño constante. Si la clave de partición de entrada no se puede cambiar, este problema se puede solucionar agregando un paso sin particiones para agregar valores entre las particiones, como en el ejemplo siguiente:

```SQL
    WITH Step1 AS (
        SELECT COUNT(*) AS Count, TollBoothId
        FROM Input1 Partition By PartitionId
        GROUP BY TumblingWindow(minute, 3), TollBoothId, PartitionId
    )

    SELECT SUM(Count) AS Count, TollBoothId
    FROM Step1
    GROUP BY TumblingWindow(minute, 3), TollBoothId
```

Esta consulta se puede escalar hasta 24 unidades de streaming.

> [!NOTE]
> Si va a combinar dos flujos de datos, asegúrese de que estén particionados mediante la clave de partición de la columna que usa para realizar las combinaciones. Asegúrese también de que tiene el mismo número de particiones en ambos flujos de datos.
> 
> 

## <a name="achieving-higher-throughputs-at-scale"></a>Lograr un mayor rendimiento a escala

Un trabajo [verdaderamente paralelo](#embarrassingly-parallel-jobs) es necesario, pero no suficiente para admitir un mayor rendimiento a escala. Cada sistema de almacenamiento y su correspondiente salida de Stream Analytics presentan variaciones a la hora de lograr el mejor rendimiento de escritura posible. Como ocurre con cualquier escenario a escala, hay algunos desafíos que pueden resolverse usando las configuraciones apropiadas. En esta sección se describen las configuraciones de unas cuantas salidas comunes y se proporcionan ejemplos para mantener las tasas de ingesta de 1000, 5000 y 10 000 eventos por segundo.

En las observaciones siguientes se usa un trabajo de Stream Analytics con una consulta (passthrough) sin estado, una UDF JavaScript básica que escribe en Event Hubs, Azure SQL DB o Cosmos DB.

#### <a name="event-hub"></a>Centro de eventos

|Tasa de ingesta (eventos por segundo) | Unidades de streaming | Recursos de salida  |
|--------|---------|---------|
| 1000     |    1    |  2 TU   |
| 5000     |    6    |  6 TU   |
| 10 000    |    12   |  10 TU  |

La solución [Event Hubs](https://github.com/Azure-Samples/streaming-at-scale/tree/master/eventhubs-streamanalytics-eventhubs) escala linealmente en términos de unidades de streaming (SU) y rendimiento, de modo que constituye la forma más eficaz y de mayor rendimiento para analizar y transmitir datos fuera de Stream Analytics. Los trabajos se pueden escalar en hasta 192 unidades de streaming, lo que se traduce aproximadamente en un procesamiento de hasta 200 MB/s o 19 trillones de eventos al día.

#### <a name="azure-sql"></a>Azure SQL
|Tasa de ingesta (eventos por segundo) | Unidades de streaming | Recursos de salida  |
|---------|------|-------|
|    1000   |   3  |  S3   |
|    5000   |   18 |  P4   |
|    10 000  |   36 |  P6   |

[SQL Azure](https://github.com/Azure-Samples/streaming-at-scale/tree/master/eventhubs-streamanalytics-azuresql) permite la escritura en paralelo (operación conocida como herencia de particiones), pero no está habilitada de forma predeterminada. Sin embargo, habilitar la herencia de particiones, además de una consulta totalmente paralela, puede no ser suficiente para lograr un mayor rendimiento. El rendimiento de la escritura SQL depende significativamente del esquema de tabla y la configuración de base de datos. En el artículo sobre [rendimiento de salida SQL](./stream-analytics-sql-output-perf.md) encontrará más detalles sobre los parámetros que ayudan a disparar el rendimiento de escritura. Tal y como se indica en el artículo [Salida de Azure Stream Analytics a Azure SQL Database](./stream-analytics-sql-output-perf.md#azure-stream-analytics), esta solución no escala linealmente como una canalización totalmente paralela más allá de 8 particiones, y puede que deba volver a particionar antes de la salida SQL (vea [INTO](https://docs.microsoft.com/stream-analytics-query/into-azure-stream-analytics#into-shard-count)). Se necesitan SKU Premium para dar cabida a tasas de E/S elevadas, así como a la sobrecarga de copias de seguridad de registros que sucede cada pocos minutos.

#### <a name="cosmos-db"></a>Cosmos DB
|Tasa de ingesta (eventos por segundo) | Unidades de streaming | Recursos de salida  |
|-------|-------|---------|
|  1000   |  3    | 20 000 RU  |
|  5000   |  24   | 60 000 RU  |
|  10 000  |  48   | 120 000 RU |

La salida de [Cosmos DB](https://github.com/Azure-Samples/streaming-at-scale/tree/master/eventhubs-streamanalytics-cosmosdb) de Stream Analytics se ha actualizado para usar la integración nativa en el [nivel de compatibilidad 1.2](./stream-analytics-documentdb-output.md#improved-throughput-with-compatibility-level-12). El nivel de compatibilidad 1.2 permite un rendimiento significativamente mayor y reduce el consumo de RU en comparación con el nivel 1.1, que es el nivel de compatibilidad predeterminado de los trabajos nuevos. La solución utiliza contenedores de Cosmos DB particionados en /deviceId y el resto de la solución está configurada de forma idéntica.

Todos los [ejemplos de Azure de streaming a escala](https://github.com/Azure-Samples/streaming-at-scale) usan como entrada un centro de eventos que se alimenta de cargas que simulan clientes de prueba. Cada evento de entrada es un documento JSON de 1 KB, que convierte fácilmente las tasas de ingesta configuradas en tasas de rendimiento (1 MB/s, 5 MB/s y 10 MB/s). Los eventos simulan un dispositivo IoT que envía los siguientes datos JSON (en formato abreviado) de hasta 1000 dispositivos:

```
{
    "eventId": "b81d241f-5187-40b0-ab2a-940faf9757c0",
    "complexData": {
        "moreData0": 51.3068118685458,
        "moreData22": 45.34076957651598
    },
    "value": 49.02278128887753,
    "deviceId": "contoso://device-id-1554",
    "type": "CO2",
    "createdAt": "2019-05-16T17:16:40.000003Z"
}
```

> [!NOTE]
> Las configuraciones están sujetas a cambios debido a los distintos componentes que se usan en la solución. Para lograr una estimación más precisa, personalice los ejemplos de forma que se ajusten a su escenario.

### <a name="identifying-bottlenecks"></a>Identificación de cuellos de botella

Utilice el panel Métricas del trabajo de Azure Stream Analytics para identificar los cuellos de botella en la canalización. Revise **Eventos de entrada/salida** para ver el rendimiento y [Retraso de la marca de agua](https://azure.microsoft.com/blog/new-metric-in-azure-stream-analytics-tracks-latency-of-your-streaming-pipeline/) o **Eventos de trabajos pendientes** para ver si el trabajo está al día con respecto a la tasa de entrada. Para métricas de Event Hubs, busque **Solicitudes limitadas** y ajuste las unidades de umbral como corresponda. En lo relativo a las métricas de Cosmos DB, vea **Máximo de RU/s consumidas por cada intervalo de claves de partición**  en Rendimiento para garantizar que los intervalos de claves de partición se consumen uniformemente. Para Azure SQL DB, supervise las **E/S de registro** y la **CPU**.

## <a name="get-help"></a>Obtener ayuda

Para más ayuda, pruebe nuestra [página de preguntas y respuestas de Microsoft sobre Azure Stream Analytics](https://docs.microsoft.com/answers/topics/azure-stream-analytics.html).

## <a name="next-steps"></a>Pasos siguientes
* [Introducción a Azure Stream Analytics](stream-analytics-introduction.md)
* [Introducción al uso de Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Referencia del lenguaje de consulta de Azure Stream Analytics](https://docs.microsoft.com/stream-analytics-query/stream-analytics-query-language-reference)
* [Referencia de API de REST de administración de Azure Stream Analytics](https://msdn.microsoft.com/library/azure/dn835031.aspx)

<!--Image references-->

[img.stream.analytics.monitor.job]: ./media/stream-analytics-scale-jobs/StreamAnalytics.job.monitor-NewPortal.png
[img.stream.analytics.configure.scale]: ./media/stream-analytics-scale-jobs/StreamAnalytics.configure.scale.png
[img.stream.analytics.perfgraph]: ./media/stream-analytics-scale-jobs/perf.png
[img.stream.analytics.streaming.units.scale]: ./media/stream-analytics-scale-jobs/StreamAnalyticsStreamingUnitsExample.jpg
[img.stream.analytics.preview.portal.settings.scale]: ./media/stream-analytics-scale-jobs/StreamAnalyticsPreviewPortalJobSettings-NewPortal.png   

<!--Link references-->

[microsoft.support]: https://support.microsoft.com
[azure.event.hubs.developer.guide]: https://msdn.microsoft.com/library/azure/dn789972.aspx

[stream.analytics.introduction]: stream-analytics-introduction.md
[stream.analytics.get.started]: stream-analytics-real-time-fraud-detection.md
[stream.analytics.query.language.reference]: https://go.microsoft.com/fwlink/?LinkID=513299
[stream.analytics.rest.api.reference]: https://go.microsoft.com/fwlink/?LinkId=517301


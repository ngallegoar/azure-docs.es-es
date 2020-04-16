---
title: 'Almacenamiento y entrada de datos en la versión preliminar: Azure Time Series Insights | Microsoft Docs'
description: Información acerca del almacenamiento y entrada de datos en la versión preliminar de Azure Time Series Insights.
author: lyrana
ms.author: lyhughes
manager: cshankar
ms.workload: big-data
ms.service: time-series-insights
services: time-series-insights
ms.topic: conceptual
ms.date: 02/10/2020
ms.custom: seodec18
ms.openlocfilehash: 95a579cacc339360295f5f25fa6415ab29cd68ff
ms.sourcegitcommit: b129186667a696134d3b93363f8f92d175d51475
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2020
ms.locfileid: "80673897"
---
# <a name="data-storage-and-ingress-in-azure-time-series-insights-preview"></a>Almacenamiento y entrada de datos en la versión preliminar de Azure Time Series Insights

En este artículo se describen las actualizaciones en el almacenamiento y la entrada de datos de la versión preliminar de Azure Time Series Insights. Describe la estructura de almacenamiento subyacente, el formato de archivo y la propiedad Id. de serie temporal. También se describen el proceso de entrada subyacente, los procedimientos recomendados y las limitaciones actuales de la versión preliminar.

## <a name="data-ingress"></a>Entrada de datos

El entorno de Azure Time Series Insights contiene un *motor de ingesta* para recopilar, procesar y almacenar datos de series temporales. 

Hay algunas consideraciones que se deben tener en cuenta para garantizar el proceso de todos los datos entrantes, lograr una gran escala de entrada y minimizar la *latencia de ingesta* (el tiempo que tarda Time Series Insights en leer y procesar los datos del origen del evento) al [planear el entorno](time-series-insights-update-plan.md).

Las directivas de entrada de datos de la versión preliminar de Time Series Insights determinan dónde se pueden obtener los datos de origen y qué formato deben tener.

### <a name="ingress-policies"></a>Directivas de entrada

La *entrada de datos* implica cómo se envían los datos a un entorno de versión preliminar de Azure Time Series Insights. 

A continuación se resumen las configuraciones principales, el formato y los procedimientos recomendados.

#### <a name="event-sources"></a>Orígenes de eventos

La versión preliminar de Azure Time Series Insights admite los siguientes orígenes de eventos:

- [Azure IoT Hub](../iot-hub/about-iot-hub.md)
- [Azure Event Hubs](../event-hubs/event-hubs-about.md)

La versión preliminar de Azure Time Series Insights admite un máximo de dos orígenes de eventos por instancia. Cuando conecte un origen de eventos, el entorno TSI leerá todos los eventos almacenados actualmente en su lote o en Event Hub, empezando por el evento más antiguo. 

> [!IMPORTANT] 
> * Al conectar un origen del evento a un entorno de versión preliminar, puede experimentar una latencia inicial elevada. 
> La latencia del origen del evento depende del número de eventos que se encuentren actualmente en IoT Hub o en el Centro de eventos.
> * Tras la primera ingesta de datos del origen del evento la latencia elevada disminuirá. Envíe una incidencia de soporte técnico a través de Azure Portal si sufre una latencia elevada de forma continuada.

#### <a name="supported-data-format-and-types"></a>Tipos y formatos de datos admitidos

Azure Time Series Insights admite JSON codificado en UTF8 enviado a través de Azure IoT Hub o Azure Event Hubs. 

Los tipos de datos admitidos son:

| Tipo de datos | Descripción |
|---|---|
| **bool** | Tipo de datos que tiene uno de dos estados: `true` o `false`. |
| **dateTime** | Representa un instante de tiempo, normalmente expresado en forma de fecha y hora del día. Se expresa en formato [ISO 8601](https://www.iso.org/iso-8601-date-and-time-format.html). |
| **double** | Punto flotante [IEEE 754](https://ieeexplore.ieee.org/document/8766229) de doble precisión de 64 bits. |
| **string** | Valores de texto, compuestos de caracteres Unicode.          |

#### <a name="objects-and-arrays"></a>Objetos y matrices

Puede enviar tipos complejos, como objetos y matrices, como parte de la carga del evento, pero los datos se someterán a un proceso de acoplamiento al almacenarse. 

La información detallada que describe cómo definir los eventos JSON, enviar un tipo complejo y acoplar objetos anidados está disponible en el artículo sobre [dar forma a los eventos JSON para la entrada y la consulta](./time-series-insights-update-how-to-shape-events.md) destinado a facilitar el planeamiento y la optimización.

### <a name="ingress-best-practices"></a>Procedimientos recomendados de entrada

Se recomienda aplicar los siguientes procedimientos recomendados:

* Configure Azure Time Series Insights y una instancia de IoT Hub o Event Hubs en la misma región para reducir la latencia potencial.

* Para [planear sus necesidades de escala](time-series-insights-update-plan.md), calcule la tasa de ingesta prevista y compruebe que se encuentra dentro de la tasa admitida que se indica a continuación.

* Entienda cómo optimizar y dar forma a los datos JSON, así como las limitaciones actuales de la versión preliminar, al leer [Cómo dar forma a JSON para la entrada y la consulta](./time-series-insights-update-how-to-shape-events.md).

### <a name="ingress-scale-and-preview-limitations"></a>Escala de entrada y limitaciones de la versión preliminar 

A continuación se describen las limitaciones de entrada de la versión preliminar de Azure Time Series Insights.

> [!TIP]
> Lea [Planeamiento del entorno de versión preliminar](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-update-plan#review-preview-limits) para obtener una lista completa de todos los límites de la versión preliminar.

#### <a name="per-environment-limitations"></a>Limitaciones de cada entorno

En general, las tasas de entrada se ven como el factor del número de dispositivos que hay en la organización, la frecuencia de emisión de eventos y el tamaño de cada evento:

*  **Número de dispositivos** × **Frecuencia de emisión de eventos** × **Tamaño de cada evento**.

De forma predeterminada, la versión preliminar de Time Series Insights puede ingerir datos entrantes a una velocidad de **hasta 1 megabyte por segundo (MBps) por entorno de Time Series Insights**. Existen limitaciones adicionales [por partición de centro de conectividad](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-update-storage-ingress#hub-partitions-and-per-partition-limits).

> [!TIP] 
> * La compatibilidad del entorno con velocidades de ingesta de hasta 16 MBps se puede proporcionar a petición.
> * Póngase en contacto con nosotros si necesita mayor rendimiento mediante el envío de una incidencia de soporte técnico a través de Azure Portal.
 
* **Ejemplo 1:**

    Contoso Shipping tiene 100.000 dispositivos que emiten un evento tres veces por minuto. El tamaño de un evento es de 200 bytes. Usan una instancia de IoT Hub con cuatro particiones como origen de eventos de Time Series Insights.

    * La tasa de ingesta para su entorno de Time Series Insights sería: **100 000 dispositivos * 200 bytes/evento * (3/60 evento/segundo) = 1 MBps**.
    * La tasa de ingesta por partición sería de 0,25 MBps.
    * La tasa de ingesta de Contoso Shipping se encuentra dentro del límite de escala de la versión preliminar.

* **Ejemplo 2:**

    Contoso Fleet Analytics tiene 60.000 dispositivos que emiten un evento cada segundo. Usan una instancia de Event Hub con un número de particiones de 4 como origen de eventos de Time Series Insights. El tamaño de un evento es de 200 bytes.

    * La tasa de ingesta de entorno sería: **60 000 dispositivos * 200 bytes/evento * 1 evento/segundo = 12 MBps**.
    * La velocidad por partición sería de 3 MBps.
    * La tasa de ingesta de análisis de Contoso Fleet Analytics supera los límites del entorno y de la partición. Pueden enviar una solicitud a Time Series Insights a través de Azure Portal para aumentar la tasa de ingesta de su entorno y crear una instancia de Event Hub con más particiones que estén dentro de los límites de la versión preliminar.

#### <a name="hub-partitions-and-per-partition-limits"></a>Particiones del centro y límites por partición

Al planear el entorno de Time Series Insights, es importante tener en cuenta la configuración de los orígenes de eventos que se van a conectar a Time Series Insights. Tanto Azure IoT Hub como Event Hubs usan particiones para habilitar el escalado horizontal para el procesamiento de eventos. 

Una *partición* es una secuencia ordenada de eventos que se mantiene en un centro. El número de particiones se establece durante la fase de creación del centro y no se puede cambiar. 

Para conocer los procedimientos recomendados de partición de Event Hubs, revise [¿Cuántas particiones necesito?](https://docs.microsoft.com/azure/event-hubs/event-hubs-faq#how-many-partitions-do-i-need)

> [!NOTE]
> La mayoría de las instancias de IoT Hub que se usan con Azure Time Series Insights solo necesitan cuatro particiones.

Tanto si está creando un nuevo centro para el entorno de Time Series Insights como si usa uno existente, deberá calcular la tasa de ingesta por partición para determinar si se encuentra dentro de los límites de la versión preliminar. 

La versión preliminar de Azure Time Series Insights tiene actualmente un **límite general por partición de 0,5 MBps**.

#### <a name="iot-hub-specific-considerations"></a>Consideraciones específicas de IoT Hub

Cuando se crea un dispositivo en IoT Hub, se asigna de forma permanente a una partición. Al hacerlo, IoT Hub puede garantizar el orden de los eventos (puesto que la asignación nunca cambia).

Una asignación de partición fija también afecta a las instancias de Time Series Insights que ingieren datos enviados desde la instancia de IoT Hub de bajada. Cuando los mensajes de varios dispositivos se reenvían al centro con el mismo identificador de dispositivo de puerta de enlace, pueden llegar a la misma partición al mismo tiempo, lo que posiblemente haga superar los límites de escala por partición. 

**Impacto**:

* Si una sola partición experimenta una tasa sostenida de ingesta sobre la limitación de la versión preliminar, existe la posibilidad de que Time Series Insights no sincronice la telemetría de todos los dispositivos antes de que se haya superado el período de retención de datos de IoT Hub. Como resultado, los datos enviados se pueden perder si los límites de ingesta se superan constantemente.

Para mitigar esta circunstancia, se recomiendan los siguientes procedimientos recomendados:

* Calcule sus tasas de ingesta por entorno y por partición antes de implementar la solución.
* Asegúrese de que los dispositivos IoT Hub tengan una carga equilibrada en la medida de lo posible.

> [!IMPORTANT]
> En el caso de los entornos que usan IoT Hub como origen de eventos, calcule la tasa de ingesta usando el número de dispositivos concentradores en uso para asegurarse de que la velocidad esté por debajo de 0,5 MBps por límite de partición en la versión preliminar.
> * Aunque varios eventos lleguen simultáneamente, no se superará el límite de la versión preliminar.

  ![Diagrama de particiones de IoT Hub](media/concepts-ingress-overview/iot-hub-partiton-diagram.png)

Consulte los siguientes recursos para obtener más información sobre cómo optimizar el rendimiento y las particiones del centro:

* [Escala de IoT Hub](https://docs.microsoft.com/azure/iot-hub/iot-hub-scaling)
* [Escala del centro de eventos](https://docs.microsoft.com/azure/event-hubs/event-hubs-scalability#throughput-units)
* [Particiones del centro de eventos](https://docs.microsoft.com/azure/event-hubs/event-hubs-features#partitions)

### <a name="data-storage"></a>Almacenamiento de datos

Al crear un entorno de SKU de *pago por uso* de la versión preliminar de Time Series Insights, se crean dos recursos de Azure:

* Un entorno de versión preliminar de Azure Time Series Insights que se puede configurar para el almacenamiento de datos intermedio.
* Una cuenta de blob de uso general v1 de Azure Storage para el almacenamiento de datos en frío.

Los datos del almacenamiento intermedio solo están disponibles a través de [Consulta de Serie temporal](./time-series-insights-update-tsq.md) y el [explorador de la versión preliminar de Azure Time Series Insights](./time-series-insights-update-explorer.md). El almacén intermedio contendrá datos recientes dentro del [período de retención](./time-series-insights-update-plan.md#the-preview-environment) seleccionado al crear el entorno de Time Series Insights.

La versión preliminar de Time Series Insights guarda los datos del almacenamiento intermedio en Azure Blob Storage, en [formato de archivo de Parquet](#parquet-file-format-and-folder-structure). La versión preliminar de Time Series Insights administra estos datos del almacenamiento intermedio de forma exclusiva, aunque están disponibles para leerse directamente como archivos estándar de Parquet.

> [!WARNING]
> Como propietario de la cuenta de Azure Blob Storage donde residen los datos del almacenamiento intermedio, tiene acceso total a todos los datos de la cuenta. Este acceso incluye permisos de escritura y eliminación. No modifique ni elimine los datos que escribe la versión preliminar de Time Series Insights, ya que esto puede provocar una pérdida de datos.

### <a name="data-availability"></a>Disponibilidad de los datos

La versión preliminar de Azure Time Series Insights crea particiones de los datos y los indexa para lograr un rendimiento óptimo de las consultas. Los datos están disponibles para realizar consultas tanto en almacenamiento parcialmente activo (si está habilitado) como en almacenamiento en reposo. La cantidad de datos que se va a ingerir puede afectar a esta disponibilidad.

> [!IMPORTANT]
> Durante la versión preliminar, es posible que experimente un período de hasta 60 segundos hasta que los datos estén disponibles. Si experimenta una latencia considerable de más de 60 segundos, envíe una incidencia de soporte técnico a través de Azure Portal.

## <a name="azure-storage"></a>Azure Storage

En esta sección se describen detalles de Azure Storage relevantes para la versión preliminar de Azure Time Series Insights.

Para obtener una descripción detallada de Azure Blob Storage, lea [Introducción a Azure Blob Storage](../storage/blobs/storage-blobs-introduction.md).

### <a name="your-storage-account"></a>Cuenta de almacenamiento

Cuando se crea un entorno de pago por uso de la versión preliminar de Azure Time Series Insights, se crea una cuenta de blob de uso general v1 de Azure Storage como almacenamiento en frío a largo plazo.  

La versión preliminar de Azure Time Series Insights conserva un máximo de dos copias de cada evento en su cuenta de Azure Storage. Una copia almacena los eventos ordenados por hora de ingesta, permitiendo siempre el acceso a los eventos en una secuencia ordenada por tiempo. Con el tiempo, Time Series Insights vista previa también crea una copia con particiones de los datos que se van a optimizar para una consulta de Time Series Insights eficaz. 

Durante la versión preliminar pública, los datos se almacenan de forma indefinida en su cuenta de Azure Storage.

#### <a name="writing-and-editing-time-series-insights-blobs"></a>Escritura y edición de blobs de Time Series Insights

Para garantizar el rendimiento de las consultas y la disponibilidad de los datos, no edite ni elimine los blobs que crea la versión preliminar de Time Series Insights.

#### <a name="accessing-time-series-insights-preview-cold-store-data"></a>Acceso a los datos de almacenamiento en frío de Time Series Insights Preview 

Además de tener acceso a los datos desde el [explorador de la versión preliminar de Time Series Insights ](./time-series-insights-update-explorer.md) y la [consulta de Time Series](./time-series-insights-update-tsq.md), puede que también desee tener acceso a los datos directamente desde los archivos de Parquet almacenados en el almacén frío. Por ejemplo, puede leer, transformar y limpiar los datos en un cuaderno de Jupyter y luego usarlo para entrenar el modelo de Azure Machine Learning en el mismo flujo de trabajo de Spark.

Para acceder a los datos directamente desde su cuenta de Azure Storage, necesita acceso de lectura a la cuenta usada para almacenar los datos de la versión preliminar de Time Series Insights. A continuación, puede leer los datos seleccionados en función de la hora de creación del archivo de Parquet que se encuentra en la carpeta `PT=Time` que se describe a continuación en la sección [Formato de archivo de Parquet](#parquet-file-format-and-folder-structure).  Para más información sobre cómo habilitar el acceso de lectura a su cuenta de almacenamiento, consulte [Administración del acceso a los recursos de la cuenta de almacenamiento](../storage/blobs/storage-manage-access-to-resources.md).

#### <a name="data-deletion"></a>Eliminación de datos

No elimine los archivos de la versión preliminar de Time Series Insights. Los datos relacionados solo se administran desde la versión preliminar de Time Series Insights.

### <a name="parquet-file-format-and-folder-structure"></a>Formato de archivo de Parquet y estructura de carpetas

Parquet es un formato de archivo en columnas de código abierto diseñado para lograr un almacenamiento y un rendimiento eficaces. La versión preliminar de Time Series Insights usa Parquet para habilitar el rendimiento de las consultas basadas en el identificador de Time Series a escala.  

Para más información sobre el tipo de archivo de Parquet, lea la [documentación de Parquet](https://parquet.apache.org/documentation/latest/).

La versión preliminar de Time Series Insights almacena copias de los datos de la siguiente manera:

* La primera copia inicial se particiona por hora de ingesta y almacena los datos más o menos en orden de llegada. Los datos residen en la carpeta `PT=Time`:

  `V=1/PT=Time/Y=<YYYY>/M=<MM>/<YYYYMMDDHHMMSSfff>_<TSI_INTERNAL_SUFFIX>.parquet`

* La segunda copia con particiones se agrupa por los identificadores de Time Series y reside en la carpeta `PT=TsId`:

  `V=1/PT=TsId/Y=<YYYY>/M=<MM>/<YYYYMMDDHHMMSSfff>_<TSI_INTERNAL_SUFFIX>.parquet`

En ambos casos, la propiedad de tiempo del archivo de Parquet se corresponde con la hora de creación del blob. Los datos de la carpeta `PT=Time` se conservan sin cambios una vez que se escriben en el archivo. Los datos de la carpeta `PT=TsId` se optimizan para la consulta a lo largo del tiempo y no están estáticos.

> [!NOTE]
> * `<YYYY>` se asigna a una representación de año de cuatro dígitos.
> * `<MM>` se asigna a una representación de mes de dos dígitos.
> * `<YYYYMMDDHHMMSSfff>` se asigna a una representación de marca de tiempo con un año de cuatro dígitos (`YYYY`), un mes de dos dígitos (`MM`), un día de dos dígitos (`DD`), una hora de dos dígitos (`HH`), un minuto de dos dígitos (`MM`), un segundo de dos dígitos (`SS`) y un milisegundo de tres dígitos (`fff`).

Los eventos de la versión preliminar de Time Series Insights se asignan al contenido de los archivos de Parquet de esta manera:

* Cada evento se asigna a una sola fila.
* Cada fila incluye la columna **timestamp** con una marca de tiempo del evento. La propiedad time-stamp nunca es null. Su valor predeterminado es **event enqueued time** si no se especifica en el origen del evento. La marca de tiempo almacenada siempre está en UTC.
* Cada fila incluye las columnas de identificadores de Time Series como se define durante la creación del entorno de Time Series Insights. El nombre de la propiedad del identificador de Time Series incluye el sufijo `_string`.
* Las demás propiedades enviadas como datos de telemetría se asignan a nombres de columna que terminan en `_string` (cadena), `_bool` (booleano), `_datetime` (fecha y hora) o `_double` (doble), en función del tipo de propiedad.
* Este esquema de asignación se aplica a la primera versión del formato de archivo, a la que se hace referencia como **V=1** y se almacena en la carpeta base del mismo nombre. A medida que esta característica evolucione, este esquema de asignación podría cambiar y se incrementaría el nombre de referencia.

## <a name="next-steps"></a>Pasos siguientes

- Lea [Dar forma JSON para entradas y consultas](./time-series-insights-update-how-to-shape-events.md).

- Lea sobre el nuevo [modelo de datos](./time-series-insights-update-tsm.md).
